---
layout: post
title: "HTTPS: Securing java web services"
categories: []
tags: []
published: True
date: 2015-06-02 15:25:34
---

As part of my internship project I had to learn about HTTPS, how it works and how to use HTTPS to secure SOAP based web services written in java.

### Why do we need HTTPS?
The document transfer protocol of choice for the internet, [HTTP](http://www.w3.org/Protocols/) uses a plain text format for transfering data. Hence anyone who is able to intercept the communication channel([MITM attack](http://en.wikipedia.org/wiki/Man-in-the-middle_attack)) can view everything that is being transmitted by the sender and receiver. This is specially a problem for wireless networks where any one with a pretty basic wireless network adapter can tap into and listen to the communications channel. 

This can be demonstrated using free and opensource tools like [Wireshark](https://www.wireshark.org/) and [mitmproxy](https://mitmproxy.org/)

<h4>Demo:</h4>
Lets login to a website which does not use https to secure the connection using wireshark.
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_http_raw.png">
	![Wireshark http raw]({{site.url}}/assets/ssl/wireshark_http_raw.png)
</a>
If we inspect the packets using wireshark (as seen in the screenshot below) we can easily see the data sent to the server, which in this case are the email and password of the user. Hence anyone who can tap into the network can easily get hold of the login credentials and there by the users authenticity is compromised. 
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_http_raw_post.png">
	![Wireshark http raw post data]({{site.url}}/assets/ssl/wireshark_http_raw_post.png)
</a>
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_http_raw_post2.png">
	![Wireshark http raw post data]({{site.url}}/assets/ssl/wireshark_http_raw_post2.png)
</a>

Now lets check login to the same website with https enabled and inspect the packets:
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_https_raw_post.png">
	![Wireshark post data using https]({{site.url}}/assets/ssl/wireshark_https_raw_post.png)
</a>
As you can see from the image below, this time the data sent to the server is encrypted; i.e. the user's credentials are safe.
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_ssl_encrypted_data.png">
	![Wireshark ssl encrypted data]({{site.url}}/assets/ssl/wireshark_ssl_encrypted_data.png)
</a>

So, HTTP by default is not suitable for sending private data. For this we need some protocol which provides data security using encryption like HTTPS

### What is TLS/SSL?
The required data security/encryption can be provided by many mechanisms, but TLS/SSL has become the ubiquitous defacto standard for the Internet. SSL stands for Secure Socket Layer and TLS is Transport Layer Security. Both are more or less the same specifications with minor differences which we can generally ignore. 
<a class="img_tag" href="{{site.url}}/assets/ssl/security_layers.png">
	![Security protocols in different layers]({{site.url}}/assets/ssl/security_layers.png)
</a>

<a class="img_tag" href="{{site.url}}/assets/ssl/ssl_stack.png">
	![SSL in the IP stack]({{site.url}}/assets/ssl/ssl_stack.png)
</a>
As we can see from the above figure, the ssl/tls acts as a layer on top of the transport layer in the IP stack. The connection between the communicating peers is provided by TCP. SSL/TLS acts as a layer in between the transport and application layers and encrypts the data passing through the connection.


### Basics of encryption
Broadly speaking there are two kinds of cryptographic techniques used to encrypt data in  SSL: symmetric key and asymmetric key cryptography.

<h4>Symmetric key or secret key cryprography:</h4>
<a class="img_tag" href="{{site.url}}/assets/ssl/symmetric_crypto_system.png">
	![Secret key cryptography]({{site.url}}/assets/ssl/symmetric_crypto_system.png)
</a>
As shown in the figure above, in case of symmetric key cryptography, both the sender and receiver have the same secret key which is used to both encode and decode the data. This technique is fine but we need a way to securely share the secret key. 

<h4>Assymetric key or public key cryptography:</h4>
<a class="img_tag" href="{{site.url}}/assets/ssl/asymmetric_crypto_system.png">
	![Public key cryptography]({{site.url}}/assets/ssl/asymmetric_crypto_system.png)
</a>

Now in case of asymmetric key or public key cryptography, there is a pair of keys. One is called the private key and the other is called its public counterpart or simply public key. 
Data encrypted with the private key can only be decrypted using the corresponding public key and that encrypted using a public key and only be decrypted using the corresponding private key.
Hence, in this case the problem of sharing a secret is taken care off.



### A basic overview of TLS/SSL:
<a class="img_tag" href="{{site.url}}/assets/ssl/ssl_stack.png">
	![SSL in the IP stack]({{site.url}}/assets/ssl/ssl_stack.png)
</a>
<em>Note: Public key encryption is more computationally intensive than Secret key encryption. Hence in case of SSL, public key encryption is used to share the secret. This secret is then used to encrypt the data.</em>
<a class="img_tag" href="{{site.url}}/assets/ssl/ssl_stack.png">
	![SSL handshake]({{site.url}}/assets/ssl/ssl_stack.png)
</a>

<h4> The first set of messages is sent from the client to the server.</h4>
+ It only comprises a `CLIENTHELLO` message.
<h4> The second set of messages comprises 2-5 messages that are sent from the
server to the client: </h4>
+ A `SERVERHELLO` message is sent in response to the `CLIENTHELLO` message.
+ If the server is to authenticate itself (which is generally the case), it may
send a `CERTIFICATE` message to the client.
+ Under some circumstances, the server may send a `SERVERKEYEXCHANGE` message to the client.
+ If the server requires the client to authenticate itself with a certificate, it
may send a `CERTIFICATEREQUEST` message to the client.
+ Finally, the server sends a `SERVERHELLODONE` message to
the client.
+ After having exchanged `CLIENTHELLO` and `SERVERHELLO` messages,
the client and server have negotiated a protocol version, a session identifier
(ID), a cipher suite, and a compression method. Furthermore, two random
values (i.e., `ClientHello.random` and `ServerHello.random`), have
been generated and are now available for use.
<h4> The third set of messages comprises messages that are again sent from the client to the server: </h4>
+ If the server has sent a `CERTIFICATEREQUEST` message, then the client sends a `CERTIFICATE` message to the server.
+ In the main step of the protocol, the client sends a `CLIENTKEYEXCHANGE` message to the server. The content of this message
depends on the key exchange algorithm in use.
+ If the client has sent a certificate to the server, then it must also send a
`CERTIFICATEVERIFY` message to the server. This message is
digitally signed with the private key that corresponds to the certificate's
public key.
+ The client sends a `CHANGECIPHERSPEC` message to the server (using
the SSL Change Cipher Spec Protocol) and copies its pending write state
into the current write state.
+ The missing type indicates that the `CHANGECIPHERSPEC` message is not an SSL Handshake
Protocol message. Instead, it is an SSL Change Cipher Spec Protocol message (identified with a
content type value of 20).
+ The client sends a `FINISHED` message to the server. As mentioned above, this is the first message that is cryptographically protected
under the new cipher spec.
<h4> Finally, the fourth set of messages comprises two messages that are sent from the server to the client:</h4>
+ The server sends another `CHANGECIPHERSPEC` message to the client
and copies its pending write state into the current write state.
+ Finally, the server sends a `FINISHED` message to the client.

At this point in time, the SSL handshake is complete and the client and
server may begin exchanging application layer protocol data units (using the SSL
Application Data Protocol).


Now lets look into the handshake process using wireshark:
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_ssl_handshake.png">
	![Wireshark SSL handshake]({{site.url}}/assets/ssl/wireshark_ssl_handshake.png)
</a>
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_ssl_certificate.png">
	![Wireshark SSL certificate]({{site.url}}/assets/ssl/wireshark_ssl_certificate.png)
</a>
<a class="img_tag" href="{{site.url}}/assets/ssl/wireshark_https_raw_post3.png">
	![Wireshark SSL data]({{site.url}}/assets/ssl/wireshark_https_raw_post3.png)
</a>


### A few words about certificates:
As described earlier the ssl protocol requires that the communicating peers verify their identity using digitally signed certificates. These certificates are generally issued by some trusted certifying authority(CA) which guarantees the identity of the senders. 

The most commonly used format for exchanging these certificates is the [X.509](http://en.wikipedia.org/wiki/X.509) format.
<a class="img_tag" href="{{site.url}}/assets/ssl/certificate_struct.png">
	![x509 certificate]({{site.url}}/assets/ssl/certificate_struct.png)
</a>

There is another type of cerificate scheme which is gaining popularity called the [PGP](http://en.wikipedia.org/wiki/Pretty_Good_Privacy).