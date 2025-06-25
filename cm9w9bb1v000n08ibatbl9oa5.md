---
title: "Executing EBPF in Github Actions"
seoTitle: "Running eBPF in GitHub Actions for Network Tracing"
seoDescription: "Learn how to execute eBPF in GitHub Actions for network monitoring, testing, and seamless CI/CD integration without full root access."
datePublished: Fri Apr 25 2025 03:53:32 GMT+0000 (Coordinated Universal Time)
cuid: cm9w9bb1v000n08ibatbl9oa5
slug: executing-ebpf-in-github-actions
canonical: https://keploy.io/blog/community/executing-ebpf-in-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744876722605/68d86cd7-61ef-4061-bec7-5dfe0e61c662.jpeg
tags: github, gitlab, github-actions-1, ebpf

---

# Introduction

At [Keploy](http://www.keploy.io), we wanted our tool to be part of the GitHub pipeline so that developers could be guaranteed that modifications made in pull requests could be safely merged and deployed. The only problem we had was ~ is it possible.

The reason for this question is because Keploy uses eBPF to track network calls, which requires sudo capabilities. So the question becomes, will GitHub grant users sudo rights for a program they construct and execute on their systems?

> If you're interested in how eBPF can be used in other projects and environments like OpenWRT, check out our community post on [Connecting a Hosted UI Website to an AWS EC2 Instance](https://keploy.io/blog/community/connecting-a-hosted-ui-website-to-an-aws-ec2-instance).

---

## How does a regular merging cycle work?

A typical merge cycle is simple: you create a PR, certain workflows are run, and if passed, the reviewer merges.

## How are the workflows run?

A workflow is a customizable automated process that does one or more jobs. Workflows are defined by a [YAML file](https://keploy.io/blog/community/yaml-vs-yml-developers-guide-to-syntax-and-ease-of-use) that is checked into your repository and will run when prompted by an event in your repository, manually, or on a set schedule.

```plaintext
|.github
    |->workflows
          |-> go.yml #our sample workflow file
```

So, in a normal merging cycle, these runners are used to check that the test of tests pre-computed rules are followed and that the program acts as expected.

---

## What is eBPF, and why do we use it?

The extended Berkeley Packet Filter (eBPF) enables the programmable, safe, and efficient tracing and monitoring of kernel and user-space processes. So, by monitoring system calls, we'll be able to see incoming and outgoing calls to the program without having to interact with the application code.

> Want to understand **what information can eBPF tell us about an incoming packet**? It includes metadata like packet size, destination IP, ports, and protocol type—perfect for tracing network activity without interfering with app logic.

We intend to run this process whenever a pull request is sent to the main branch.

## Running eBPF in runner

A sample workflow to run Keploy in GitHub Action would look like something below.

```yaml
name: Golang On Linux
on: [pull_request]
jobs:
  golang_linux:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Build binary
      run: |
        go build -o keployv2 //Build the binary

    - name: Run samples-go application
      run: |
        cd samples-go/gin-mongo
        source ./../../.github/workflows/golang-linux.sh //the script
```

The script for the aforementioned runner might look something like this. In my example, I used the Keploy repository to run in "record" and "test" mode to track network calls.

```bash
#!/bin/bash

# Start mongo before starting binary.
docker run --rm -d -p27017:27017 --name mongoDb mongo

# Build the binary of the app to be traced with eBPF program.
go build -o ginApp

# Start the gin-mongo app in record mode and record testcases and mocks.
sudo -E env PATH="$PATH" ./../../keployv2 record -c "./ginApp" --generateGithubActions=false &

sleep 15
# Get the pid of the application.
pid=$(pgrep keploy)

curl -X GET http://localhost:8080/CJBKJd92

# Wait for 5 seconds for keploy to record the tcs and mocks.
sleep 5

# Stop the gin-mongo app.
sudo kill $pid

# Wait for 5 seconds for keploy to stop.
sleep 5
done

# Start the gin-mongo app in test mode.
sudo -E env PATH="$PATH" ./../../keployv2 test -c "./ginApp" --delay 7 --generateGithubActions=false
```

---

## How can we gain the ability to read network calls?

Cgroups (control groups) are a Linux kernel feature that hierarchically organizes processes and distributes system resources in a controlled and flexible manner.

GitHub-hosted runners do not provide access to edit or configure cgroups due to their secure multi-tenant environment.

So how are we capturing network activity?

We use **eBPF hooks** (specifically kprobes) that don’t require cgroup manipulation. Our Keploy implementation can still observe system calls and packet information passively.

> Want to go deeper into how this tech links with native code? Check out [Java Native Interface](https://keploy.io/blog/community/java-native-interface) on our blog.

Additionally, if you're deploying in OpenWRT environments, you might be curious **how to enable eBPF in the kernel in OpenWRT**. It typically involves enabling kernel config options like `CONFIG_BPF` and `CONFIG_BPF_SYSCALL`, then recompiling the firmware.

## The results of running eBPF:

GitHub and GitLab both give us sudo privileges for runners (letting us attach hooks on kprobes). Bitbucket, on the other hand, does not.

These configurations helped us learn we need custom runners for ARM64 architecture. Below is a snippet that demonstrates HTTP requests being captured multiple times by Keploy.

## Conclusion

There are a few learnings from this venture:

* GitHub runner does give us root-like privilege by allowing us to attach hooks to kprobes.
    
* GitHub runners are 2-core, 7 GB RAM, 14 GB SSD disk space beasts, perfect for running [eBPF-based tools.](https://keploy.io/blog/community/ebpf-for-tls-traffic-tracing-secure-efficient-observability)
    
* **GitHub integration** is possible even with root-level operations like network monitoring using eBPF.
    

> For more insights into developer tools, tech communities, and tips, check out [10 Developer Communities to Be a Part of in 2025](https://keploy.io/blog/community/10-developer-communities-to-be-a-part-of-in-2025) on our blog.

### Explore more eBPF and integration use cases on [www.keploy.io](https://www.keploy.io/).

## FAQs

**1\. Can eBPF run inside GitHub Actions without full root access?**  
Yes, eBPF can run in GitHub Actions because it attaches to kernel hooks like kprobes.  
These hooks don’t require full root or cgroup configuration access.  
GitHub runners provide limited sudo privileges, which are enough for Keploy.  
This allows passive network tracing during pull requests safely.

**2\. What kind of data can eBPF extract from incoming packets?**  
eBPF can capture metadata like source and destination IPs, ports, and protocols.  
It can also track packet sizes, timings, and system call context.  
This makes it useful for tracing and debugging network activity.  
All this happens without modifying the application code.

**3\. How do you enable eBPF in OpenWRT?**  
To enable eBPF in OpenWRT, you need to recompile the kernel with BPF options.  
Enable flags like `CONFIG_BPF` and `CONFIG_BPF_SYSCALL` in kernel config.  
Also include bpftool and required headers for runtime utilities.  
Then flash the firmware and verify with `bpftool`commands.

**4\. Why does Keploy use eBPF in its GitHub workflow?**  
Keploy uses eBPF to monitor network calls without touching the app code.  
This lets it auto-generate test cases and mocks by passively observing traffic.  
Running it in GitHub ensures every PR gets verified safely.  
It’s a powerful way to integrate deep network tracing into CI/CD.

**5\. How is GitHub integration handled for eBPF-based tools like Keploy?**  
GitHub workflows use YAML files to define steps and runners.  
Keploy is built, run, and tested in these workflows using sudo where needed.  
The eBPF logic runs inside custom bash scripts that are triggered per PR.  
This seamless integration allows consistent testing across environments.