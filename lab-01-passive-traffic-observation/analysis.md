# Analysis – Lab 01: Passive Network Traffic Observation

## Introduction

This analysis documents observations made during **Lab 01: Passive Network Traffic Observation** using Wireshark.  
The goal of this lab was to understand how common network protocols behave on the wire during normal client activity, without performing any active attacks, manipulation, or interception.

All observations are based on actual captured traffic generated through normal browsing and diagnostic commands in a controlled virtual environment.  
The focus of this analysis is on how protocols communicate, what information is visible, and what is protected, rather than exploiting or modifying traffic.

---

## 1. Transmission Control Protocol (TCP)

Transmission Control Protocol (TCP) is the protocol used to establish a reliable connection between a client and a server before data transfer.

The TCP three-way handshake observed in this lab occurs as follows:

- First, the client sends a request to connect to the server using a **[SYN]** packet  
  *(ephemeral/private port → server port 443)*

- Then, the server responds with a **[SYN, ACK]**, acknowledging the connection request  
  *(server port 443 → client ephemeral port)*

- Finally, the client responds with an **[ACK]**, confirming the connection  
  *(ephemeral/private port → server port 443)*

After this exchange, a reliable communication channel is established between the client and the server.

The above interactions can be observed in the image `TCP_3way_handshake.png` provided in the `images` directory.

---

## 2. Transport Layer Security (TLS)

Transport Layer Security (TLS) is the protocol used to secure HTTP traffic, resulting in HTTPS communication.

The TLS handshake observed in this lab includes the following behavior:

- The **Client Hello** message is sent from the client to the server.
- During this phase, certain metadata such as the **Server Name Indication (SNI)** remains visible  
  *(for example, `wikipedia.org` in this capture)*.
- At this stage, encryption has not yet started.
- After the handshake completes with **Server Hello** and encryption keys are established, application data becomes encrypted.
- All subsequent communication after the handshake appears as encrypted TLS application data and cannot be read in plaintext.

The above interactions can be observed in the images `TLS_client_hello.png`, `TLS_server_hello.png`, and `TLS_encrypted_app_data.png` provided in the `images` directory.

---

## 3. Domain Name System (DNS)

Domain Name System (DNS) is used to resolve human-readable domain names into IP addresses.

Observed behavior in this lab includes:

- When a domain name (e.g., `youtube.com`) is entered in a browser, a DNS query is sent to resolve the domain.
- The DNS response provides the IP address of the server to which the client should connect.
- **A records** represent IPv4 addresses.
- **AAAA records** represent IPv6 addresses.
- Multiple DNS queries for different domains are visible in plaintext in the capture.
- If a domain cannot be resolved, the connection attempt fails.

The above interactions can be observed in the image `DNS_standard_query_response.png` provided in the `images` directory.

---

## 4. Internet Control Message Protocol (ICMP)

Internet Control Message Protocol (ICMP) is primarily used for error reporting and network diagnostics.

In this lab, ICMP was observed using the `ping` command:

- `ping` sends **Echo Request** messages to a target host.
- The target host responds with **Echo Reply** messages.
- This exchange is used to verify connectivity and measure round-trip time (RTT).
- Sequence numbers allow matching requests to replies.

The `ping google.com` command was used to demonstrate ICMP behavior.

This behavior can be observed in the image `ICMP_ping_request_reply.png` provided in the `images` directory.

---

## 5. Hypertext Transfer Protocol (HTTP)

Hypertext Transfer Protocol (HTTP) is an application-layer protocol used for web communication.

Observations from this lab include:

- HTTP traffic is transmitted without encryption.
- Packet contents, including headers and payload data, are fully visible.
- Resources such as images and web content can be inspected directly in Wireshark.
- This lack of encryption makes HTTP unsuitable for transmitting sensitive information.
- This is a key reason why HTTP is being replaced by HTTPS for secure communication.

This behavior can be observed in the image `HTTP_content_observation.png` provided in the `images` directory.

---

## Conclusion

This lab demonstrates how common network protocols operate during normal client activity and highlights the differences between plaintext and encrypted communication.

Key takeaways include:

- TCP establishes reliable connections using a clearly observable handshake.
- DNS queries reveal domain lookups in plaintext.
- TLS protects application data while still exposing limited metadata.
- ICMP provides valuable diagnostic information.
- HTTP traffic exposes all transmitted data, reinforcing the importance of HTTPS.

By passively observing real network traffic, this lab builds foundational packet-level understanding that is essential for future work in network security, traffic analysis, and security research.

---

## Security Implications

Observations from this lab highlight several important security implications of normal network communication:

- DNS traffic is visible in plaintext, allowing passive observers to learn which domains a client is attempting to access, even when HTTPS is used later for encryption.
- TCP connection establishment exposes metadata such as source and destination ports, sequence numbers, and timing information, which can be used for traffic analysis even without access to application data.
- TLS protects application-layer content but does not completely hide communication patterns. Information such as the destination server (via SNI) and the timing of encrypted packets remains visible to observers.
- HTTP traffic exposes all transmitted data, including headers and response content, making it unsuitable for handling sensitive information. This demonstrates why HTTPS is critical for secure web communication.
- ICMP traffic can reveal network reachability and latency, which may be useful for diagnostics but can also be leveraged for network reconnaissance if unrestricted.

Overall, this lab demonstrates that encryption significantly improves confidentiality but does not make network communication invisible. Passive observers can still infer meaningful information from metadata, emphasizing the importance of layered security and secure protocol choices.
