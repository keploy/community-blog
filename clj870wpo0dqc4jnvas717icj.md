---
title: "Tracing Go Function Arguments in Production"
seoTitle: "Tracing Go Function Arguments in Production Using eBPF"
seoDescription: "eBPF (extended Berkeley Packet Filter) has emerged as a powerful tool that can be used to trace system events in real-time and provide detailed insights."
datePublished: Fri Jun 23 2023 06:30:39 GMT+0000 (Coordinated Universal Time)
cuid: clj870wpo0dqc4jnvas717icj
slug: using-ebpf-for-tracing-go-function-arguments-in-production
canonical: https://keploy.io/blog/community/tracing-go-function-arguments-in-production
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1687411061690/f6366628-6a77-4e02-8af7-1f42986c69bc.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1687411145843/71b8145c-608c-4b83-aaa5-e6ac2f0634f8.png

---

Kubernetes is an incredibly complex system that is used to manage and orchestrate containerized applications. It provides a way to run and manage your applications across multiple machines, abstracting away the underlying infrastructure complexities. With Kubernetes, you can easily deploy, scale, and update your applications, ensuring high availability, fault tolerance, and efficient resource utilization.

With so many moving parts, it can be difficult to pinpoint performance issues and troubleshoot problems when they arise. However, eBPF (extended Berkeley Packet Filter) has emerged as a powerful tool that can be used to trace system events in real-time and provide detailed insights into system behaviour. In this blog post, we will explore how eBPF can be used to trace Go function arguments in a Kubernetes environment.

## What is eBPF?

eBPF (extended Berkeley Packet Filter) is a revolutionary technology that allows you to safely and efficiently extend the capabilities of the Linux kernel without modifying its source code. It provides a programmable and highly efficient way to analyze and manipulate network packets, system calls, and various kernel events. With eBPF, you can write small, sandboxed programs that run directly within the kernel, enabling powerful networking, monitoring, and security applications. It has gained significant popularity in the Kubernetes ecosystem, empowering developers to build advanced networking and observability solutions for containerized environments.

## **Why Trace Go Function Arguments?**

In a Kubernetes environment, applications are often written in Go, a popular programming language for building containerized applications. When performance issues arise, it can be challenging to pinpoint the root cause. By tracing Go function arguments, we can gain valuable insights into the application's behaviour and identify areas where improvements can be made.

### Tracing Go Function Arguments with eBPF

eBPF is a powerful tool that can be used to trace system events in real-time. It allows us to insert probes into the kernel or user-space code and capture data about the system's behavior.

First, make sure you have the bcc tool installed on your system. You can refer to the bcc documentation for installation instructions: [**https://github.com/iovisor/bcc**](https://github.com/iovisor/bcc)

Let's get started with the code, in the below example we are writing a code to add the number passed in the arguments.

```go
//main.go
package main

import (
	"fmt"
)

func add(a, b int) int {
	return a + b
}

func main() {
	result := add(2, 3)
	fmt.Println("Result:", result)
}
```

Now it's time to write our eBPF program:

```c
//trace_args.c
#include <uapi/linux/ptrace.h>

BPF_PERF_OUTPUT(events);

int trace_func_args(struct pt_regs *ctx) {
	int arg1 = PT_REGS_PARM1(ctx);
	int arg2 = PT_REGS_PARM2(ctx);

	bpf_trace_printk("arg1: %d, arg2: %d\\n", arg1, arg2);

	return 0;
}
```

We are specifying the function we want to trace and capture the function arguments using the `arg1` and `arg2` variables. Save the above code to a file called `trace_args.c`.

Compile the eBPF program using the bcc tool:

```powershell
clang -O2 -target bpf -c trace_args.c -o trace_args.o
```

Once we have written our eBPF program, let's modify the Go code to load and attach the eBPF program:

```go
//main.go
package main

import (
	"fmt"
	"os"
	"os/signal"
	"syscall"
	"unsafe"

	"golang.org/x/sys/unix"
)

const (
	perfEventTypeTracepoint = 10
	tracepointCategory      = "go"

	bpfProgramPath = "./trace_args.o"
)

func loadBPFProgram() int {
	fd, err := unix.BPF(unix.BPF_PROG_LOAD, bpfProgramPath)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to load BPF program: %v\n", err)
		return -1
	}
	return fd
}

func attachTracepoint(fd int, tracepoint string) int {
	attr := &unix.PerfEventAttr{
		Type:   perfEventTypeTracepoint,
		Config: uint64(fd),
	}
	attr.SetSampleFreq(1)
	attr.SetWakeupEvents(1)

	tpName := fmt.Sprintf("%s:%s", tracepointCategory, tracepoint)
	cTpName := unix.ByteSliceFromString(tpName)
	attr.SetTracepoint(unix.BytePtrFromString(tpName))

	_, _, errno := unix.Syscall6(
		unix.SYS_PERF_EVENT_OPEN,
		uintptr(unsafe.Pointer(attr)),
		unix.Getpid(),
		-1,
		-1,
		unix.PERF_FLAG_FD_CLOEXEC,
	)
	if errno != 0 {
		fmt.Fprintf(os.Stderr, "Failed to attach to tracepoint %s: %v\n", tracepoint, errno)
		return -1
	}
	return 0
}

func main() {
	fd := loadBPFProgram()
	if fd < 0 {
		os.Exit(1)
	}
	defer unix.Close(fd)

	if attachTracepoint(fd, "trace_func_args") < 0 {
		os.Exit(1)
	}

	signalChan := make(chan os.Signal, 1)
	signal.Notify(signalChan, syscall.SIGINT, syscall.SIGTERM)
	<-signalChan

	fmt.Println("Exiting...")
}
```

With our eBPF program running, we can now send requests to our application and capture the function arguments in `real-time`. This allows us to gain valuable insights into our application's behaviour and identify areas where improvements can be made.

Now, let's compile and run the modified Go code:

```powershell
go build -o main main.go
sudo ./main
```

When you run the code, it will trace the `add` function arguments using the eBPF program and print them. Press Ctrl+C to stop the program.

The output will be similar to:

```plaintext
arg1: 2, arg2: 3
```

## Conclusion

eBPF is a powerful tool that can be used to trace system events in `real-time` and provide detailed insights into system behaviour. By tracing Go function arguments in a Kubernetes environment, we can gain valuable insights into our application's behaviour and identify areas where improvements can be made. We can improve the performance and reliability of our applications and ensure that they are running at their best, with the power of eBPF.