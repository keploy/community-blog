---
title: "Key Extraction by uprobe Attachment on OpenSSL for SSL Inspection"
seoTitle: "Key Extraction via uprobe on OpenSSL for SSL Inspection"
seoDescription: "Learn how to extract TLS keys using uprobes on OpenSSL for SSL inspection without breaking encryption or needing trusted certificates."
datePublished: Fri Apr 11 2025 05:20:09 GMT+0000 (Coordinated Universal Time)
cuid: cm9cc8rjl001c09l4gbfiggsl
slug: key-extraction-by-uprobe-attachment-on-openssl-for-ssl-inspection
canonical: https://keploy.io/blog/community/key-extraction-by-uprobe-attachment-on-openssl-for-ssl-inspection
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744348623734/a7904bb5-5634-48ca-b94f-887371b59591.png
tags: ssl, tls, ssl-certificate, protocols

---

Man-in-the-middle attacks are elegantly mitigated in TLS, with TLS1.3 introducing more robust encryption protocols and a streamlined handshake process that significantly reduces the vulnerability to these types of attacks. By employing stricter encryption standards and eliminating outdated cipher suites, TLS 1.3 enhances security measures to effectively counter potential interception and unauthorised data decryption. This upgrade to the TLS protocol ensures a higher level of confidentiality and integrity of data in transit, making it more challenging for attackers to exploit communication channels.

SSL (Secure Sockets Layer) inspection, also known as TLS (Transport Layer Security) inspection, plays a critical role in network security. It involves the interception and decryption of encrypted traffic to allow for deep packet inspection (DPI). By examining the contents of encrypted SSL/TLS traffic, organisations can identify and mitigate potential security threats like malware, data ex-filtration, and unauthorised access that might otherwise remain hidden.

[Keploy.io](http://keploy.io) integrates seamlessly with applications that use OpenSSL for secure communication, enabling reliable and secure [API test generation](https://keploy.io/blog/community/test-case-generation-for-faster-api-testing).  
By capturing traffic from OpenSSL-enabled services, Keploy can generate accurate test cases without compromising encryption or data integrity.  
This makes Keploy an ideal choice for teams building secure, high-performance APIs with TLS/SSL layers powered by OpenSSL.

## **SSL Inspection Techniques**

SSL Inspection can be done in multiple ways:

**1\. Man-in-the-Middle (MitM) Proxy  
**In this method, the proxy intercepts the TLS handshake and establishes separate TLS connections with both the client and the server. For this to work, the client must trust the proxy’s certificate, which may not always be feasible in production environments or BYOD networks.

**2\. SSL Decryption with Uprobes  
**An advanced method involves the use of uprobes (user-space probes) attached to OpenSSL functions like SSL\_write or SSL\_read, capturing plaintext data or extracting session keys needed to decrypt the traffic. This method does not interfere with the TLS handshake and allows the proxy to decrypt the data without initiating a new TLS connection.

This approach is particularly useful for debugging encrypted traffic or building passive inspection tools. It requires OpenSSL to be present on the system, and it’s supported on multiple platforms, including Linux and Windows. For developers looking to experiment with this, OpenSSL for Windows can be easily set up by downloading the latest precompiled binaries. Many developers use openssl s\_client on Windows or Linux to verify server certificates or test SSL/TLS connections from the command line after installing OpenSSL.

### **Key Extraction Workflow**

To extract the keys using a uprobe on SSL\_write, the first argument to the function (the SSL context) is accessed using the bpf\_probe\_read\_user helper. The SSL context is a private struct inside OpenSSL.

```python
// ssl_st->version

#define SSL_ST_VERSION 0x0   

// Reading ssl_st_ptr

void ssl_st_ptr = (void )PT_REGS_PARM1_CORE(ctx);

__u64 ssl_version_ptr = (__u64 )(ssl_st_ptr + SSL_ST_VERSION);

int ret = bpf_probe_read_user(&version, sizeof(version), (void *)ssl_version_ptr);
```

  
This code extracts the TLS version number from the context. Similarly, other fields such as cipher\_id, client\_random, handshake\_secret, and exporter\_master\_secret can also be extracted by calculating their respective offsets.

Once the secrets are collected, they are pushed into a perf buffer so they can be read in userspace. The key extraction process is designed to work with the proxy, which uses the secrets to decrypt traffic without generating new certificates. This is crucial in setups where the proxy's certificate is not trusted by the client.

### **Complete Flow of SSL Inspection with Uprobes**

* **Client Application:  
    **Sends data over TLS using SSL\_write, with the OpenSSL library managing encryption.
    
* **Uprobe Hook on OpenSSL:  
    **A uprobe is attached to SSL\_write. When triggered, it reads the SSL context and extracts encryption keys.
    
* **Perf Buffer:  
    **These secrets are passed from kernel to userspace through a high-speed perf buffer.
    
* **Proxy:  
    **Listens for encrypted traffic from the client and uses the extracted secrets to decrypt it. Once decrypted, the payload is re-encrypted (if needed) and sent to the server. The proxy doesn’t perform a TLS handshake or use its own certificate.
    
* **Server:  
    **Receives the decrypted traffic, processed as if it came from a legitimate TLS session.
    
* **Data Store (Optional):  
    **The decrypted payload may be stored for further analysis, inspection, or compliance auditing.  
    

**Getting Started with OpenSSL: Download and Installation**

If you're establishing a secure development or inspection environment, the first thing to do is acquire the appropriate tools, beginning with OpenSSL. It's easy enough to do, and an OpenSSL download is easily found for every major platform—Windows, macOS, and Linux. Whether you're conducting SSL certificate verification, decrypting encrypted traffic for debugging purposes, or adding OpenSSL to your application, having the updated version installed provides access to the newest security standards and functionalities. Windows users can download precompiled binaries from reputable sites such as \[SLProweb\]([https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html)), whereas macOS users can install it seamlessly through Homebrew.

### **Getting Started with OpenSSL for Windows**

If you're working in a Windows environment and want to try this out or inspect TLS sessions manually, installing OpenSSL is straightforward. You can download OpenSSL for Windows and use the included openssl s\_client utility to interact with secure endpoints. This tool is invaluable for debugging and validating your inspection pipeline—whether you're working on certificate pinning, handshake validation, or traffic inspection.

By leveraging OpenSSL’s low-level internals with uprobes and eBPF, this approach to SSL inspection offers a powerful, transparent method of monitoring TLS traffic—without needing to manipulate the handshake or insert untrusted certificates. Whether you're on Linux or Windows, getting started with OpenSSL tools and utilities opens the door to robust observability of encrypted communication. 

## Conclusion

OpenSSL is still a foundational element of contemporary digital security, driving encryption and safe communications throughout millions of applications and systems. Whether you're auditing SSL/TLS traffic, handling certificates, or [writing secure code](https://keploy.io/blog/community/code-quality-with-automated-tools), knowing and leveraging OpenSSL is critical. With simple installation packages available for Windows, Linux, and [macOS,](https://keploy.io/blog/community/tips-macbook-with-touch-bar-users) it's easy to get started for developers and security experts alike. By remaining current and familiarizing yourself with utilities such as `openssl s_client` and uprobe-based key extraction, you can better see and control encrypted traffic in today's advanced threat environment.

## **FAQ’S**

### What is OpenSSL?

OpenSSL is an open-source library that offers cryptographic functions for encrypting data transmission.

It supports SSL and TLS protocols for secure communication over networks.

Developers utilize it for implementing HTTPS, certificate generation, and key management.

It's used extensively in servers, clients, and embedded systems for secure communication.

### How to install OpenSSL on Windows?

Download the latest OpenSSL installer from a reliable source such as slproweb.com.

Run the installer and accept the default settings, making the recommended selections.

Add the installation directory to your system's environment variables for command-line execution.

Open Command Prompt after installing and type `openssl` to verify it runs.

### How to upgrade OpenSSL 3.1 in Ubuntu 22.04?

Remove the current version first using `sudo apt remove openssl`.

Download and extract the OpenSSL 3.1 source code from the official webpage.

Run `./config`, followed by `make` and `sudo make install` to compile and install it.

Finally, check for the upgrade using `openssl version` in the terminal.

### How to install OpenSSL?

On Linux, use `sudo apt install openssl` for rapid installation using the package manager.

On macOS, install Homebrew using `brew install openssl`.

For Windows, install using the installer downloaded and executed with default options.

On completion of installation, check if it is properly configured using `openssl version`.