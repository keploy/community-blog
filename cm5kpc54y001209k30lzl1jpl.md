---
title: "Tracing TLS Data with Ethical and Secure Practices"
seoTitle: "Tracing TLS Data with Ethical and Secure Practices"
seoDescription: "Explore the use of MITM proxies for tracing secure data in TLS connections and understand their ethical implications and applications in security"
datePublished: Mon Jan 06 2025 07:10:02 GMT+0000 (Coordinated Universal Time)
cuid: cm5kpc54y001209k30lzl1jpl
slug: tracing-tls-data-with-ethical-and-secure-practices
canonical: https://keploy.io/blog/community/tracing-tls-data-with-ethical-and-secure-practices
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736147329193/ce250ebe-bf74-4d99-bcda-82f1ae5f43cf.png

---

Network security professionals and observability applications have been trying to trace data in secure TLS connections since a very long time now. TLS( Transport Layer Security) is an encryption protocol that is used by servers are clients for encrypting the data that they share over a connection. Specifically, it uses symmetric encryption which creates a single shared key that both the server and the client use for encrypting and decrypting the data.

![TLS Handshake explanation](https://cdn.hashnode.com/res/hashnode/image/upload/v1706117728830/f8c3cfc9-f6e8-4ef6-84de-37dd8fd53c63.png align="center")

Now, you might think, the only way to decrypt this data is to have access to that shared key, so that you can trace the data and then decrypt it. But there is another way, a more insecure way, by creating a man in the middle proxy.

## Understanding MITM Proxies

It's like those detective movies, where the detective would disguise himself as the one of the criminals, get in their camp, and then get access to all of their strategies and secrets. Similarly, a man-in-the-middle(MITM) proxy stands between the client and the server and makes the client believe that it is the server, and then creates a TLS connection with it and gets access to all the data that the client was going to send to the server.

![MITM proxy](https://www.dinofizzotti.com/blog/2022-04-24-running-a-man-in-the-middle-proxy-on-a-raspberry-pi-4/mitmproxy_basic_hu30a123de70a8550bbfcedf637e7da663_30692_1200x0_resize_box_2.png align="left")

Now fooling a client(an application), is not very easy, when creating a TLS connection, the client and the server have a process called TLS handshake, where the server sends a server certificate, that is signed by a valid certificate authority(CA) to the client. The client holds a list of famous and trustable CAs in its storage, and uses that to validate if the server certificate is signed by one of them or not.

So an MITM proxy exploits this very property of a client against it. It creates a fake server certificate by the name of the actual server that the client was trying to call. Then it creates a fake CA and signs that server certificate with it. Now for the final piece of the puzzle, it adds the fake CA to the client's trust store.

## Generating Fake Certificates for MITM Proxies in TLS Communication

To implement such a server, you need a way to route your traffic to the proxy(you can use iptables for that), and you need a proxy that runs the code to generate a certificate authority(CA), its private key, for signing server certificates. Then it must transfer the certificate authority to the correct destination for new authorities( which is different in operating systems as well as distros) and then run the update command for that specific distro or operating system. This will ensure that the CA is added to the trust store. Given below is a code implementation in Golang for the same:

```go
package main

import (
	"crypto"
	"crypto/tls"
	"fmt"
	"log"
	"os/exec"
	"strings"

	"github.com/cloudflare/cfssl/csr"
	"github.com/cloudflare/cfssl/log"
	"github.com/cloudflare/cfssl/signer"
	"github.com/cloudflare/cfssl/signer/local"
)

var caStoreUpdateCmd = []string{"update-ca-certificates", "update-ca-trust"}

func main() {
	err := updateCaStore()
	if err != nil {
		log.Fatalf("Error updating CA store: %v", err)
	}

	serverTlsCert, err := generateServerCert("example.com")
	if err != nil {
		log.Fatalf("Error generating server certificate: %v", err)
	}

	log.Printf("Server certificate: %v", serverTlsCert)
}

func updateCaStore() error {
	commandRun := false
	for _, cmd := range caStoreUpdateCmd {
		if commandExists(cmd) {
			commandRun = true
			_, err := exec.Command(cmd).CombinedOutput()
			if err != nil {
				return err
			}
		}
	}
	if !commandRun {
		return fmt.Errorf("no valid CA store update command found")
	}
	return nil
}

func generateServerCert(serverName string) (*tls.Certificate, error) {
	cfsslLog.Level = cfsslLog.LevelError

	serverReq := &csr.CertificateRequest{
		CN: serverName,
		Hosts: []string{
			serverName,
		},
		KeyRequest: csr.NewKeyRequest(),
	}

	serverCsr, serverKey, err := csr.ParseRequest(serverReq)
	if err != nil {
		return nil, fmt.Errorf("failed to create server CSR: %v", err)
	}

	cryptoSigner, ok := caPrivKey.(crypto.Signer)
	if !ok {
		log.Printf("Error in typecasting the caPrivKey")
	}
	signerd, err := local.NewSigner(cryptoSigner, caCertParsed, signer.DefaultSigAlgo(cryptoSigner), nil)
	if err != nil {
		return nil, fmt.Errorf("failed to create signer: %v", err)
	}

	serverCert, err := signerd.Sign(signer.SignRequest{
		Hosts:   serverReq.Hosts,
		Request: string(serverCsr),
		Profile: "web",
	})
	if err != nil {
		return nil, fmt.Errorf("failed to sign server certificate: %v", err)
	}

	serverTlsCert, err := tls.X509KeyPair(serverCert, serverKey)
	if err != nil {
		return nil, fmt.Errorf("failed to load server certificate and key: %v", err)
	}

	return &serverTlsCert, nil
}

func commandExists(cmd string) bool {
	_, err := exec.LookPath(cmd)
	return err == nil
}
```

After running this code, you can run the command below to verify if the CA has been added to the trust store or not.

```bash
grep -q "ca.crt" /etc/ssl/certs/ca-certificates.crt && echo "CA is trusted" || echo "CA is not trusted"
```

If the certificate was successfully updated, you should see the output "CA is trusted".

So now whenever the client tries to make a call to a server, the proxy redirects the call onto itself and then sends its fake server certificate to the client. The client then tries to validate the certificate by checking if its signed by one of the CAs or not, and since we had already put our CA into the trust store of the client, the certificate is validated and the proxy makes a TLS conneciton with the client.

It is also important to mention the security implications that come with MITM proxies.

* **Data privacy and confidentiality**: With MITM proxies you can get access to confidential data that was meant to be sent to the server instead. This can include personal information, login credentials, financial data or confidential business data. Therefore using them for unethical purposes can lead to data access and privacy breaches.
    
* **Authentication Bypass**: Sometimes those proxies can also be used to fake secure websites where you are supposed to send your authentication credentials for login. If MITM proxies get access to this data, it can lead to unauthorized access in the systems and services of the client.
    
* **Legal and Ethical Concerns**: Using MITM proxies without consent is often illegal and unethical. That is because it is essentially modifying and accessing data without the user's consent.
    
      
    But even after these downsides, MITM proxies are widely used by network security engineers and ethical hackers to make systems more secure by challenging their authentication mechanisms. If you know what you are doing, and have consent for it, MITM proxies can be a great way to trace SSL data for any usecase.
    

### How Keploy Uses MITM Proxies for API Tracing Safely

This is one of the approaches that we use here at Keploy, for tracing the API calls from clients. Although insecure, it does not affect any TLS connections of the client machine as long as Keploy is not started and attached to a user application. You can try out Keploy [here](https://keploy.io), and see for yourself how the fake certificate and the fake CA are being generated and added to the system's trust store. Until next time, stay safe, and happy hacking!

## Conclusion

MITM proxies, despite their potential for misuse, are powerful tools for analyzing and tracing data in secure TLS connections. By carefully implementing them with consent and ethical intent, security professionals can challenge and improve the security posture of systems. The approach detailed here demonstrates the technical intricacies and potential uses of MITM proxies, particularly in the context of tools like Keploy for API tracing. Always prioritize legal compliance, ethical considerations, and data privacy when leveraging such techniques.

## FAQs

### **Why is TLS encryption critical in secure communications?**

TLS ensures the confidentiality and integrity of data transferred between a client and server by encrypting it. This prevents unauthorized access or tampering during transmission.

### **How does a MITM proxy bypass TLS security?**

A MITM proxy intercepts communications by creating a fake certificate signed by a custom Certificate Authority (CA). When the fake CA is added to the client’s trust store, the proxy can decrypt and re-encrypt the data, posing as the server.

### **Is using a MITM proxy always unethical or illegal?**

No, using a MITM proxy is not inherently unethical or illegal. It is widely used in ethical hacking and network security for identifying vulnerabilities. However, using it without consent or for malicious purposes violates ethical guidelines and legal standards.

### **How does Keploy use MITM proxies without compromising security?**

Keploy uses MITM proxies in a controlled and consented environment for tracing API calls. The fake CA and certificate are only used within the scope of tracing, ensuring the client's overall TLS connections remain unaffected when Keploy is not running.