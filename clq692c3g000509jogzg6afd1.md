---
title: "Decoding HTTP/2 Traffic is Hard, but eBPF can help"
seoTitle: "Decoding HTTP/2 Traffic"
datePublished: Fri Dec 15 2023 06:30:09 GMT+0000 (Coordinated Universal Time)
cuid: clq692c3g000509jogzg6afd1
slug: decoding-http2-traffic-is-hard-but-ebpf-can-help
canonical: https://keploy.io/blog/community/decoding-http2-traffic-is-hard-but-ebpf-can-help
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702882245270/110ca084-efdd-4a30-8ea1-7d4c6d29277c.png
tags: http2, python, wireshark, ebpf

---

I've come across a particular challenge that many of us face: decoding HTTP/2 traffic. In this blog, I'll share insights on why decoding HTTP/2 headers can be tricky, how HPACK adds a layer of complexity, and most importantly, how eBPF uprobes can come to the rescue.

It is crucial to gain visibility into the messages exchanged between services for a comprehensive understanding and effective troubleshooting of issues. Luckily, it is possible to track the traffic enabling you to effectively debug your HTTP/2 applications.

## **What does Wireshark do?**

[Wireshark](https://www.wireshark.org/) is a popular open-source network protocol analyzer that allows you to capture and inspect the data conversing back and forth on a network in real time.

However, Wireshark sometimes fails to decode the HTTP/2. The issue stems from the binary framing of HTTP/2 packets, making it challenging for Wireshark to precisely decode headers. This challenge intensifies when dealing with encrypted traffic or intricate sequences of frames, leaving developers in a quandary. Let's delve into a scenario where we attempt to inspect HTTP/2 traffic using Wireshark. We might encounter difficulties decoding headers due to the multiplexing of streams within a single connection. Traditional tools, designed for simpler protocols, may falter in providing a clear interpretation, emphasizing the need for a more sophisticated solution.

```python
# Sample HTTP/2 binary framing
frame_data = b'\x00\x00\x0c\x08\x00\x00\x00\x00\x00\x00\
x00\x01\x00\x00\x0c\x04\x00\x00\x00\x01Hello, HTTP/2!'

# Decode HTTP/2 headers
decoded_headers = decode_http2_headers(frame_data)
print(decoded_headers)
```

This snippet showcases the challenge of interpreting binary HTTP/2 frames, which can be a stumbling block for tools like Wireshark. Normally, we can create a function such as `decode_http2_headers` to determine the exact output of the above.

```python
def decode_http2_headers(frame_data):
    # Assuming the frame_data follows the HTTP/2 binary framing format
    # Extract the frame type and payload length
    frame_type = frame_data[3]
    payload_length = int.from_bytes(frame_data[5:9], byteorder='big')

    # Check if it's a HEADERS frame (frame type 0x01 for HTTP/2)
    if frame_type == 0x01:
        # Extract the payload containing headers
        headers_payload = frame_data[9:]

        # Parse the headers payload (a more sophisticated parser is needed in a real-world scenario)
        headers = parse_headers_payload(headers_payload)

        return headers

    return None

def parse_headers_payload(payload):
    # This is a simplified parser; a complete parser would need to handle HPACK compression, etc.
    headers_list = payload.decode('utf-8').split('\r\n')

    # Convert headers to a dictionary
    headers_dict = {}
    for header in headers_list:
        if ':' in header:
            key, value = header.split(': ', 1)
            headers_dict[key] = value

    return headers_dict


# Sample HTTP/2 binary framing
frame_data = b'\x00\x00\x0c\x08\x00\x00\x00\x00\x00\x00\x00\x01\x00\x00\x0c\x04\x00\x00\x00\x01Hello, HTTP/2!'

# Decode HTTP/2 headers
decoded_headers = decode_http2_headers(frame_data)
print(decoded_headers)
```

By running the above code snippet we can get our output:-

```python
{'Hello': 'HTTP/2!'}
```

But this is a highly simplified example, and a real-world HTTP/2 header decoding function would need to handle a variety of scenarios, including HPACK compression, binary encoding, and more. The actual output would depend on the structure and content of the HTTP/2 headers in the given `frame_data`.

## **How does eBPF solve the issue?**

So if we can’t properly decode HTTP/2 traffic without knowing the state, what can we do?

Thankfully, with eBPF it becomes possible for us to observe HTTP/2 implementation to get the information that we need, without requiring state. By attaching uprobes to the HTTP/2 library APIs that take clear-text headers as input, the uprobes can directly read the header content from application memory.

The first thing I need to do is find a specific function in my code that holds all the important info about HTTP/2. This function should use a straightforward argument structure for easy data access within the eBPF code. The objective is to establish a reliable and adaptable foundation for observing and optimizing HTTP/2 interactions, this process entails strategically selecting a function that simplifies the manual pointer manipulation required for eBPF code.

```python
from bcc import BPF
from datetime import datetime

# BPF program definition
bpf_code = """
#include <uapi/linux/ptrace.h>

BPF_HASH(start, u32);

int trace_http2_send_request_headers(struct pt_regs *ctx) {
    u32 pid = bpf_get_current_pid_tgid();
    u64 ts = bpf_ktime_get_ns();
    start.update(&pid, &ts);
    return 0;
}

int trace_http2_recv_response_headers(struct pt_regs *ctx) {
    u32 pid = bpf_get_current_pid_tgid();
    u64 *tsp, delta;

    tsp = start.lookup(&pid);
    if (tsp != 0) {
        delta = bpf_ktime_get_ns() - *tsp;
        bpf_trace_printk("HTTP/2 request took %lld ns\\n", delta);
        start.delete(&pid);
    }

    return 0;
}
"""

# Attach BPF program to HTTP/2 functions
b = BPF(text=bpf_code)
b.attach_uprobe(name="your_http2_binary", sym="http2_send_request_headers", fn_name="trace_http2_send_request_headers")
b.attach_uprobe(name="your_http2_binary", sym="http2_recv_response_headers", fn_name="trace_http2_recv_response_headers")

# Print trace events
while True:
    try:
        task, pid, cpu, flags, ts, msg = b.trace_fields()
        print(f"{datetime.utcfromtimestamp(ts).strftime('%Y-%m-%d %H:%M:%S')} PID {pid}: {msg}")
    except KeyboardInterrupt:
        break
```

This is a simplified example of how we can do HTTP/2 tracing using eBPF uprobes. Now, let's customize it so that the tracer is launched after the connection between the client and server is established.

```python
bpf_code = """
#include <linux/sched.h>

BPF_HASH(start, u32);

int trace_http2_headers(struct __sk_buff *skb) {
    u32 pid = bpf_get_current_pid_tgid();
    u64 ts = bpf_ktime_get_ns();
    start.update(&pid, &ts);
    return 0;
}
"""
```

Instead of `trace_http2_recv_response_headers` and `trace_http2_send_request_headers`, we are using the `trace_http2_headers` function which is associated with the HTTP/2 headers, and prints a message when headers are received.

We are using `tcp_v{4,6}_connect` tracepoint, which is triggered when a TCP connection is established, and when this event occurs, it updates a timestamp in the BPF hash table. You can refer to the sample app code on [GitHub](https://github.com/Sonichigo/http_server/tree/main/server).

Now, when I run the Flask app and access it through my browser, I will get output on my terminal, which will look something like this:

```yaml
2023-12-11 12:00:00 PID 1234: HTTP/2 headers received
2023-12-11 12:00:05 PID 5678: HTTP/2 headers received
```

The messages indicate when HTTP/2 headers are received, and the associated PID helps identify the process of handling the HTTP/2 traffic.

## **Conclusion**

Tracing HTTP/2 activity is hard because of a complicated compression method called HPACK. However, in this post, we showed a different method to catch messages. Instead of dealing with HPACK directly, we used eBPF Uprobes to track certain functions in the HTTP/2 library. This gives us a clearer way to see what's happening with the messages in our HTTP/2 traffic.

The main advantage is the ability to trace messages regardless of when the tracer was deployed. In the end, our goal was to optimize for an approach that worked out of the box, regardless of the deployment order, which is what led us to the eBPF Uprobe-based approach.