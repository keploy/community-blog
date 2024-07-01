---
title: "SSL Problem "Unable to get Local Issuer Certificate""
datePublished: Thu Jun 27 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cly2iezqv000409md9zqj1umz
slug: ssl-problem-unable-to-get-local-issuer-certificate

---

In this age of modern era, where privacy is one of the biggest concern SSL/TLS certificates plays a vital for secure communication over the internet. They encrypt data, ensuring it is transmitted securely between servers and clients. However, while working with SSL/TLS, you may encounter the "Unable to Get Local Issuer Certificate" error. So let's try to understand why this error comes and what is SSL/TLS.

## What is SSL/TLS Certificate Chain ?

A Certificate Authority (CA) is a trusted entity that issues SSL/TLS certificates. These certificates form a chain of trust, starting from the Root CA to Intermediate CAs, and finally to the Leaf certificate (the one installed on your server). This chain ensures that the certificate presented by a server is trusted by clients.

## What causes "Unable to Get Local Issuer Certificate" ?

Usually, `unable to get local issuer certificate` error is caused by the misconfiguration of the SSL certificate on your machine. **Here are some common causes of the error:**

* **Missing Intermediate Certificates**: Intermediate certificates act as a bridge between the Root CA and the Leaf certificate. If these intermediates are missing, the client cannot verify the certificate chain, leading to the error.
    
* **Misconfigured Server**: Servers must be configured to send the entire certificate chain, excluding the Root CA. Misconfigurations, such as not including the intermediate certificates, result in clients being unable to verify the certificate.
    
* **Outdated Certificate Store**: Operating systems and browsers maintain a certificate store containing trusted Root CAs. If these stores are outdated, the client's system may not recognize newer CAs, causing the error.
    

## How to resolve this Error ?

For a quick workaround, you can disable SSL globally or locally but this isn't recommended since it creates a security issue. To resolve the error from root, you can try these steps: -

* **Updating the Certificate Store**: Ensure your operating system's certificate store is up-to-date. On Linux, you can use package managers like `apt` or `yum` to update CA certificates. On Windows, updating the operating system generally updates the certificate store.
    
* **Configuring the Server Correctly**: Add intermediate certificates to your server configuration. For Apache, include the intermediates in the `SSLCertificateChainFile` directive. For Nginx, concatenate the intermediate certificates with your server certificate in the `ssl_certificate` file.
    
* **Handling Self-Signed Certificates**: If using self-signed certificates, manually add them to the client's trusted certificate store. On Linux, copy the certificate to `/usr/local/share/ca-certificates/` and run `update-ca-certificates`. On Windows, use the Certificate Manager (`certmgr.msc`).
    

## How to prevent ‘unable to get local issuer certificate’ errors

The main purpose of a SSL certificate is to confirm authentication so that the information passed between client and server is secure. To prevent the error, ensure that you have a valid SSL keep your certificate stores updated to avoid trust issues and regularly audit your SSL/TLS configurations, tools like SSL Labs' SSL Test can help identify issues and provide recommendations.

## Conclusion

The "Unable to Get Local Issuer Certificate" error often stems from missing intermediate certificates, server misconfigurations, or outdated certificate stores.

## FAQ's

### What does the "Unable to Get Local Issuer Certificate" error mean?

The "Unable to Get Local Issuer Certificate" error indicates that the client's system cannot verify the certificate chain provided by the server. This usually occurs because the necessary intermediate certificates are missing or not correctly configured on the server. Without these intermediates, the client cannot establish a trusted connection to the server, leading to the error.

### How can I diagnose the "Unable to Get Local Issuer Certificate" error?

To diagnose the error, you can use several methods:

* **Command Line Tools:** Use `openssl s_client -connect` [`yourdomain.com:443`](http://yourdomain.com:443) `-showcerts` to check the certificate chain and identify any missing intermediates.
    
* **Browser Developer Tools:** In Chrome, navigate to the Security tab in Developer Tools to inspect the SSL/TLS certificate. In Firefox, click the Padlock icon in the address bar to view certificate details.
    
* **Server Logs:** Review your server logs for SSL/TLS-related errors that may indicate misconfigurations or missing certificates.
    

### How do I update the certificate store on my system?

Updating the certificate store ensures your system recognizes the latest trusted Certificate Authorities (CAs). Here’s how you can do it on different operating systems:

* **Linux:** Use your package manager to update CA certificates. For example, on Debian-based systems, run `sudo apt-get update` followed by `sudo apt-get install ca-certificates`. On Red Hat-based systems, use `sudo yum update ca-certificates`.
    
* **Windows:** Updating Windows generally updates the certificate store. You can also manually update it via the Certificate Manager (`certmgr.msc`).
    
* **macOS:** Keep your macOS system updated to ensure the certificate store is up-to-date.
    

### What steps should I take to correctly configure intermediate certificates on my server?

To correctly configure intermediate certificates, follow these steps for your web server:

* **Apache:** Ensure your intermediate certificates are included in the `SSLCertificateChainFile` directive. Example configuration:
    
    ```python
    SSLCertificateFile /path/to/your_domain.crt
    SSLCertificateKeyFile /path/to/your_domain.key
    SSLCertificateChainFile /path/to/intermediate_certificate.crt
    ```
    
* **Nginx:** Concatenate your server certificate and intermediate certificates into a single file and specify it in the `ssl_certificate` directive. Example configuration:
    
    ```python
    ssl_certificate /path/to/your_domain_combined.crt;
    ssl_certificate_key /path/to/your_domain.key;
    ```
    
    Ensure the combined file includes both the server certificate and the intermediate certificates in the correct order.