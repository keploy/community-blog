---
title: "eBPF for TLS Traffic Tracing: Secure & Efficient Observability"
seoTitle: "eBPF Enhances TLS Traffic Observability"
seoDescription: "Discover how eBPF revolutionizes TLS tracing, minimizing performance impact and enhancing security for modern observability systems"
datePublished: Fri Jan 10 2025 06:51:41 GMT+0000 (Coordinated Universal Time)
cuid: cm5qefyhs000u08mka38pht77
slug: ebpf-for-tls-traffic-tracing-secure-efficient-observability
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736491832410/7f51aefa-cd7e-4305-aa96-81f7ca9b8eda.jpeg

---

Tracing TLS (Transport Layer Security) traffic is crucial for modern observability systems. It helps monitor encrypted communication, diagnose issues, and optimize application performance.

However, traditional methods like **TLS proxying** and **packet capturing** often come with significant performance overheads and security risks. They are not always the ideal solution, particularly for high-performance or security-sensitive environments.

This is where **eBPF (Extended Berkeley Packet Filter)** steps in. eBPF is a revolutionary technology that enables **seamless TLS traffic tracing** directly within the Linux kernel, minimizing performance impact and enhancing security. This article explores how eBPF works, its implementation, and its benefits for developers and DevOps teams.

## **Challenges with Traditional TLS Traffic Tracing**

[Tracing TLS traffic](https://keploy.io/blog/community/decoding-http2-traffic-is-hard-but-ebpf-can-help) has always been an integral part of observability systems. Traditionally, these systems have relied on techniques such as:

* **TLS Proxying**
    
* **Man-in-the-Middle (MITM) Proxies**
    
* **Packet Capturing and Decryption**
    
* **Logging TLS Handshakes**
    

Unfortunately, these methods come with **major drawbacks** that prevent any one of them from becoming the dominant approach:

1. **Performance Overhead**: Techniques like TLS proxying and packet capturing can significantly tax the CPU due to the effort required for decryption and packet analysis.
    
2. **Security Risks**: TLS proxies often require creating and maintaining fake certificates or adding custom certificate authorities to the client’s system. This can lead to serious security vulnerabilities.
    

## **The eBPF Advantage**

eBPF addresses these challenges by offering a **state-of-the-art method** for TLS traffic tracing. Here’s why it stands out:

* **Minimal Performance Impact**: eBPF operates directly within the Linux kernel, avoiding the CPU-intensive operations of traditional methods.
    
* **Enhanced Security**: It eliminates the need for fake certificates or risky man-in-the-middle techniques.
    

eBPF enables developers to attach hooks to system calls. For instance, a system call like `sendto` used for sending data over a UDP socket can be monitored. By attaching an [eBPF program](https://keploy.io/blog/community/a-guide-for-observing-go-process-with-ebpf) to `sendto`, you can capture critical information like the buffer, function descriptor, and buffer length as soon as the system call is invoked.

This makes TLS tracing seamless and secure, addressing the limitations of older methods. The next obvious question is ***how eBPF is able to achieve all of this?*** How does it work under the hood.

## How does eBPF work?

eBPF programs are typically written in C and are then compiled into a intermediate bytecode representation. As mentioned above, these programs can be attached to various system calls, allowing them to be called when those calls are executed.

For storing data in eBPF, we first need to create special data structures called BPF maps to store the data. They can be declared as follows:

![Code snippet defining BPF](https://cdn.hashnode.com/res/hashnode/image/upload/v1695930310603/c35a5c94-482c-4508-b228-e760934e8c2b.png align="center")

Therefore, to trace TLS data specifically, these hooks can be attached to the following system calls: `sendto`, `recvfrom`, `socket`, `connect`, `accept`, `SSL_read`, and `SSL_write`. These calls contain the actual buffer, its size, and the function descriptor before the request gets encrypted and after the response gets decrypted. Thus, they don’t need the session key to decrypt the responses coming from the server.

## **Sample application attaching hooks to** `SSL_read` **and** `SSL_write` **functions**

After capturing the data in the kernel space, it needs to be sent to the user space. To do this, we write user space programs, in a variety of languages(Golang is more popular nowadays because of Cilium) where the eBPF hooks are attached to the system calls again, and the data is collected from the kernel space and shown to the user.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695930020370/563c20f5-edd4-4cd1-adc0-f44bdf608883.png align="center")

### **Sending data from the kernel space to the user space**

#### **Attaching hooks on the user side.**

Bringing it all together, we can compile this into a complete program which will look something like this:

```c
// +build ignore

#include <linux/types.h>
#include <bpf/bpf_helpers.h>
#include <bpf/bpf_core_read.h>

#include "vmlinux.h"
#include "bpf_tracing.h"
#include "tls_message.h"
#include "openssl_tracer_types.h"

// Declaring the perf submit event
struct
{
  __uint(type, BPF_MAP_TYPE_PERF_EVENT_ARRAY);
} TLS_DATA_PERF_OUTPUT SEC(".maps");

// Declaring this using btf type format
struct
{
  __uint(type, BPF_MAP_TYPE_HASH);
  __type(key, __u64);
  __type(value,  char *);
  __uint(max_entries, 10000);
  __uint(map_flags, 0);
} active_ssl_read_args_map SEC(".maps");
struct
{
  __uint(type, BPF_MAP_TYPE_HASH);
  __type(key, __u64);
  __type(value, const char *);
  __uint(max_entries, 10000);
  __uint(map_flags, 0);
} active_ssl_write_args_map SEC(".maps");

struct
{
  __uint(type, BPF_MAP_TYPE_PERCPU_ARRAY);
  __type(key, u32);
  __type(value, struct ssl_data_event_t);
  __uint(max_entries, 1);
} data_buffer_heap SEC(".maps");

static __inline struct ssl_data_event_t *create_ssl_data_event(struct pt_regs *ctx ,u64 current_pid_tgid)
{
  u32 kZero = 0;
  struct ssl_data_event_t *event = bpf_map_lookup_elem(&data_buffer_heap, &kZero);
  if (event == NULL)
  {
    return NULL;
  }
  const u32 kMask32b = 0xffffffff;
  event->timestamp_ns = bpf_ktime_get_ns();
  event->pid = current_pid_tgid >> 32;
  event->tid = current_pid_tgid & kMask32b;
  return event;
}

static int process_SSL_data(struct pt_regs *ctx, u64 id, enum ssl_data_event_type type,
                            char *buf)
{
  int len = (int)PT_REGS_RC(ctx);
  if (len < 0)
  {
    return 0;
  }

  struct ssl_data_event_t *event = create_ssl_data_event(ctx, id);
  struct ssl_data_event_t event2 = {};
  if (event == NULL)
  {
    return 0;
  }

  event->type = type;
  // This is a max function, but it is written in such a way to keep older BPF verifiers happy.
  event->data_len = (len < MAX_DATA_SIZE ? (len & (MAX_DATA_SIZE - 1)) : MAX_DATA_SIZE);
  event2.type = type;
  asm volatile("%[len] &= 0x1fff;\n" ::[len] "+r"(len):);
  bpf_probe_read(&event->data, len & 0x1fff, buf);
  bpf_perf_event_output(ctx, &TLS_DATA_PERF_OUTPUT , BPF_F_CURRENT_CPU, event, sizeof(*event));
  return 0;
}

SEC("uprobe/SSL_write")
int uprobe_entry_SSL_write(struct pt_regs *ctx)
{
  u64 processThreadID = bpf_get_current_pid_tgid();
  char *user_space_buf = (char *)PT_REGS_PARM2(ctx);
  bpf_printk("This is the buffer in the write functio%s:\n", user_space_buf);
  bpf_map_update_elem(&active_ssl_write_args_map, &processThreadID, &user_space_buf, 0);

  return 0;
}
SEC("uprobe/SSL_read")
int uprobe_entry_SSL_read(struct pt_regs *ctx)
{
  u64 processThreadID = bpf_get_current_pid_tgid();
  void *user_space_buf = (void *)PT_REGS_PARM2(ctx);
    bpf_printk("The value of buf is: %s", user_space_buf);

  bpf_map_update_elem(&active_ssl_read_args_map, &processThreadID, &user_space_buf, 0);
  return 0;
}

SEC("uretprobe/SSL_write")
int uprobe_return_SSL_write(struct pt_regs *ctx)
{
  u64 processThreadID = bpf_get_current_pid_tgid();

  // Looking up the buffer in the map
  char *buffer = bpf_map_lookup_elem(&active_ssl_write_args_map, &processThreadID);
  if (buffer != NULL)
  {
    process_SSL_data(ctx, processThreadID, kSSLWrite, buffer);
  }
  bpf_map_delete_elem(&active_ssl_write_args_map, &processThreadID);
  return 0;
}

// Attach to the return of the read function.
SEC("uretprobe/SSL_read")
int uprobe_return_SSL_read(struct pt_regs *ctx)
{
  u64 processThreadID = bpf_get_current_pid_tgid();
  // Looking up the buffer in the map
  char *buffer = bpf_map_lookup_elem(&active_ssl_read_args_map, &processThreadID);
  if (buffer != NULL)
  {
    process_SSL_data(ctx, processThreadID, kSSLRead, buffer);
  }
  bpf_map_delete_elem(&active_ssl_read_args_map, &processThreadID);
  return 0;
}

char _license[] SEC("license") = "GPL";
```

And to add these probes on the user side, we can use the Cilium library to write Go code as given below:

```go
//This program demonstrates how to use eBPF to intercept OpenSSL calls and log the contents of the data buffers.

package main

import (
	"bytes"
	"encoding/binary"
	"errors"
	"log"
	"os"
	"os/signal"
	"syscall"

	"github.com/cilium/ebpf/link"
	"github.com/cilium/ebpf/perf"
	"github.com/cilium/ebpf/rlimit"
)

const (
	// The path to the ELF binary containing the function to trace.
	// On some distributions, the 'SSL_read' and 'SSl_write' functions are provided by a
	// dynamically-linked library, so the path of the library will need
	// to be specified instead, e.g. /usr/lib/libreadline.so.8.
	// Use `ldd /bin/bash` to find these paths.
	libsslPath = "/usr/lib/x86_64-linux-gnu/libssl.so.3"
	read  = "SSL_read"
	write = "SSL_write"
)
func main() {
	stopper := make(chan os.Signal, 1)
	signal.Notify(stopper, os.Interrupt, syscall.SIGTERM)

	//Allow the current process to lock memory for eBPF.
	if err := rlimit.RemoveMemlock(); err != nil {
		log.Fatalf("failed to raise rlimit: %v", err)
	}
	//Load precompiled eBPF program and maps into the kernel
	objs := bpfObjects{}
	if err := loadBpfObjects(&objs, nil); err != nil {
		log.Fatalf("failed to load BPF: %v", err)
	}
	defer objs.Close()

	//Open an ELF binary and read its es.
	ex, err := link.OpenExecutable(libsslPath)
	if err != nil {
		log.Fatalf("failed to open executable: %v", err)
	}
	upw, err := ex.Uprobe(write, objs.UprobeEntrySSL_write, nil)
	if err != nil {
		log.Fatalf("failed to open uprobe: %v", err)
	}
	defer upw.Close()

	//Attach uretprobe to the SSL_write function.
	urw, err := ex.Uretprobe(write, objs.UprobeReturnSSL_write, nil)
	if err != nil {
		log.Fatalf("failed to open uretprobe: %v", err)
	}
	defer urw.Close()

	//Attach the uprobe to the SSL_read function.
	upr, err := ex.Uprobe(read, objs.UprobeEntrySSL_read, nil)
	if err != nil {
		log.Fatalf("failed to open uprobe: %v", err)
	}
	defer upr.Close()
	//Attach uretprobe to the SSL_read function.
	urr, err := ex.Uretprobe(read, objs.UprobeReturnSSL_read, nil)
	if err != nil {
		log.Fatalf("failed to open uretprobe: %v", err)
	}
	defer urr.Close()

	rd, err := perf.NewReader(objs.TLS_DATA_PERF_OUTPUT, os.Getpagesize())
	if err != nil {
		log.Fatalf("failed to create perf event reader: %v", err)
	}

	defer rd.Close()

	go func() {
		//Wait for the signal to stop the program.
		<-stopper
		log.Println("Detaching probes...")
		if err := rd.Close(); err != nil {
			log.Fatalf("failed to close perf event reader: %v", err)
		}
	}()
		log.Printf("Attaching probes...")
		var event bpfSslDataEventT
		for {
			record, err := rd.Read()
			if err != nil {
				if errors.Is(err, perf.ErrClosed) {
					return
				}
				log.Printf("reading from perf event reader: %s", err)
				continue
			}
			log.Printf("perf event recorded")
			if record.LostSamples != 0 {
				log.Printf("perf event ring buffer full, dropped %d samples", record.LostSamples)
				continue
			}
			// Parse the perf event entry into a bpfEvent structure.
			if err := binary.Read(bytes.NewBuffer(record.RawSample), binary.LittleEndian, &event); err != nil {
				log.Printf("parsing perf event: %s", err)
				continue
			}
			log.Printf("this is the event: %v", event)
			// Print the data buffer contents.

		}
}
```

Now, to run this code, you need to compile the bpf files and generate the go structs and then run the code, for this, we can create a simple shell script as given below:

```bash
export BPF_CLANG=clang-14
export BPF_CFLAGS="-I/usr/include/x86_64-linux-gnu -D__x86_64__ -O2 -g -Wall -Werror"
export TARGET=<your-cpu-architecture>

# To compile and run the ebpf program...
go generate ./... && go run -exec sudo .
```

Now when you run this script, it will start tracing all the TLS calls that you make from your machine. The data for them will be shown on the same terminal window.

### **Outputting the data to the user.**

It is because of these advantages that eBPF is gaining popularity by every passing day. But eBPF too, has some challenges that cannot be ignored.

1. For starters, writing and debugging eBPF programs can be a challenging task, especially for programmers who are not familiar with low-level programming.
    
2. Since eBPF code gets loaded onto the kernel, it needs to be bug-free, as any vulnerabilities in the eBPF code can lead to kernel crashes or security exploits.
    
3. Another constraint with eBPF programs is their kernel version dependency. They support different features across different kernel versions so porting eBPF code can be tough and can lead to compatibility issues.
    
4. Since eBPF is a relatively new technology the data structures used to write these programs(BPF maps) have certain limitations. For instance, their size cannot be changed dynamically and some map types do not support concurrent read and write operations.
    

## Limitations Of This Approach

Not everything works all the time and no matter how robust, this approach has its limitations as well. Some of the limitations are as follows:

* Kernel Dependency: eBPF relies on specific kernel features that may not be present in older kernels. This limits its use to environments with up-to-date kernels, making it unsuitable for legacy systems.
    
* Kernel Stability: Bugs or changes in the kernel could impact the stability and performance of eBPF programs, requiring close tracking of kernel updates and patches.
    
* User-Space Encryption: If encryption and decryption occur entirely in user-space libraries (like OpenSSL or BoringSSL), eBPF might not capture all necessary data, as it primarily hooks into system calls and kernel functions.
    
* Sandbox Restrictions: eBPF programs run in a sandboxed environment with limited capabilities. While this enhances security, it also restricts what eBPF programs can do, requiring careful design to achieve the desired tracing functionality.
    

## How Keploy uses eBPF for TLS Handling?

**Keploy**, a modern [API testing platform](https://keploy.io/), which leverages **eBPF (Extended Berkeley Packet Filter)** to efficiently trace **TLS (Transport Layer Security)** traffic. This approach addresses the challenges of monitoring encrypted data streams without compromising performance or security.

#### **Key Benefits:**

1. **Seamless Traffic Tracing:** Keploy uses eBPF to attach hooks to system calls such as `SSL_read` and `SSL_write`. These hooks capture encrypted and decrypted data before it reaches the application layer, enabling detailed traffic inspection.
    
2. **Minimal Overhead:** By operating directly in the Linux kernel, eBPF allows Keploy to monitor TLS traffic with negligible impact on system performance, making it suitable for high-load environments.
    
3. **Improved Observability:** The collected data helps Keploy deliver actionable insights for API testing, debugging, and observability. It ensures high test coverage and robust monitoring for applications with encrypted communications.
    

## Conclusion

eBPF offers a mixed bag of power and limitations, but no one can really deny its potential, and as the technology gets more mature, it is certainly bound to leave a big impact on the tech world. As eBPF continues to evolve, it holds immense promise for shaping the future of observability, security, and performance monitoring.

While it does come with its own set of challenges, such as kernel dependencies and a learning curve, the benefits of eBPF in modern observability are undeniable. Its seamless integration with the Linux kernel, minimal overhead, and robust tracing capabilities make it an invaluable tool for debugging, optimizing, and ensuring the reliability of secure applications. Now is the perfect time to explore this groundbreaking technology and harness its potential for your systems.

## **FAQs**

### **What is eBPF?**

eBPF (Extended Berkeley Packet Filter) is a technology that allows developers to run sandboxed programs in the Linux kernel without modifying its source code. It’s widely used for networking, observability, and security purposes.

### **Why is tracing TLS traffic important?**

Tracing TLS traffic is crucial for debugging, performance optimization, and understanding the behavior of secure applications. It helps identify bottlenecks, diagnose issues, and ensure compliance with security policies.

### **How does eBPF enable TLS traffic tracing?**

eBPF attaches hooks to system calls like `sendto`, `recvfrom`, `SSL_read`, and `SSL_write`. These hooks capture data before encryption or after decryption, allowing seamless tracing without needing session keys or modifying certificates.

### **What are the advantages of using eBPF over traditional TLS tracing methods?**

* Minimal performance overhead on the CPU.
    
* No need for fake certificates or proxy configurations.
    
* Enhanced security with a kernel-native approach.
    
* Compatibility with modern observability tools.
    

### **Are there any limitations to using eBPF?**

Yes, eBPF has some limitations, including:

* Kernel dependency: It requires a modern Linux kernel.
    
* Steep learning curve: Writing and debugging eBPF programs can be challenging.
    
* Restricted capabilities: eBPF programs run in a sandboxed environment.
    

### **What programming languages can I use with eBPF?**

eBPF programs are typically written in C but can be integrated with user-space programs written in languages like Golang, Python, and Rust.

### **How can I get started with eBPF?**

Start by learning the basics of Linux system calls and kernel development. Use tools like BPFTrace or frameworks like Cilium for easier eBPF programming. Tutorials and eBPF books are also great resources.

### **What are some practical use cases of eBPF in observability?**

* Tracing application network traffic.
    
* Debugging performance bottlenecks.
    
* Enhancing security through real-time monitoring.
    
* Capturing and analyzing TLS data without decryption keys.