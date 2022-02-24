This document examines the specific properties of the QUIC protocol in regards to censorship circumvention and evaluates the applicability of existing evasion building blocks to QUIC.

## Potential pitfalls for QUIC censors and how to exploit them

### Identifying QUIC connections
QUIC makes it rather difficult for censors to keep state of connections.
A QUIC connection is uniquely identified by the source and destination connection IDs which are chosen during the QUIC handshake. The connection IDs can be changed mid-connection by using the encrypted NEW_CONNECTION_ID frame. Note that a QUIC connection is neither bound to a specific IP address nor UDP port but can migrate paths as long as the connection IDs stay the same.
- A stateful censor that remembers connection IDs will not be able to track when QUIC endpoints change connection IDs because the announcement of such a change is encrypted. 
- A censor that remembers IP addresses will no be able to track when a connection migration to a different path happens because endpoints can spontaneously switch addresses without announcing it to their peer beforehand.

These considerations imply the potential of the following censorship circumvention approaches unique to QUIC:
- Change connection IDs during of shortly after the handshake.
- Change paths after the handshake. Paths have to be consistent for the duration of the handshake, as stated in the [RFC](https://www.rfc-editor.org/rfc/rfc9000.html#name-connection-migration).

### Parsing QUIC Initials
QUIC Initial packets are protected with a version specific key to [ensure that the sender of the packet is on the network path](https://www.rfc-editor.org/rfc/rfc9000.html#name-protected-packets). Though this protection doesn't provide confidentiality it does require the censors to invest additional efforts to recover the keys and decrypt the Initial packet. Due to efficiency reasons it is safe to assume that some censors might take shortcuts to parse QUIC Initial packets. Thus, when it comes to QUIC censorship evasion, it might be worth experimenting with Initial headers to make them unexpected format while keeping their expected format intact.<br/>

As the initial protection is, among others, derived from the QUIC version it might be sensible to try changing the used QUIC version of a client. For example, if a censorship system expects QUIC connections to use the current RFC version 1, it might not be able to parse draft-29 QUIC Initial packets.<br/>

Tampering with the token field in the QUIC Initial packet could be another way to detect and exploit any shortcuts the censor might use for parsing the handshake packets.

### Inspecting the Server Name Indication
As with TLS over TCP traffic, SNI blocking of QUIC should be avoidable by changing the content of the Server Name Indication (SNI) extension of the TLS1.3 ClientHello contained in the QUIC Initial packet. The offending SNI could be replaced by a (randomly chosen) allowed domain. There are however some practical implications of the circumvention method as some servers do not allow a different domain in the SNI or behave unexpectedly.

## Packet-level circumvention building blocks
Automated censorship evasion discovery based on packet- or property-level building blocks ([e.g. Geneva](https://geneva.cs.umd.edu/papers/geneva_ccs19.pdf)) is a promising new approach for censorship evasion.
This section demonstrates which previously used packet-level building blocks can be applied to QUIC, keeping in mind the specific properties of the protocol. As QUIC packets travel in UPD datagrams packet-level operations are also considered.<br/>

For context, Geneva proposed the following packet-level building blocks for TCP:
1. *duplicate*: copy a TCP packet
2. *fragment*: fragments or segments an IP/TCP packet
3. *tamper*: replaces or corrups certain fields of the TCP packet and recomputes checksums 
4. *drop*: disregards an (incoming) packet

### UDP packet level
QUIC runs on top of UDP which means that QUIC packets travel in UDP datagrams. 
While TCP-based censorship evasion strategies have relied on fragmenting TCP packets to confuse the censor, fragmentation of UDP datagrams cannot be applied in the effort to avoid QUIC censorship. This is, among others, due to the restrictions of UDP datagram sizes in many networks. Additionally, given that UDP does not enforce in-order delivery (there is no such thing as packet numbers when it comes to UDP datagrams), it is easy to comprehend why such fragmentation of UDP datagrams is explicitly forbidden in the QUIC protocol: [*UDP datagrams MUST NOT be fragmented at the IP layer.*](https://www.rfc-editor.org/rfc/rfc9000.html#name-datagram-size). <br/>

Other packet-level actions that have been used in TCP censorship evasion strategies before, are *duplicate* and *drop*. <br/>

Duplicating UDP datagrams can be a sensible censorship evasion building block when applied against stateful censors that forget a connection after having blackholed the first UDP datagram. (REF TO GENEVA visualization) <br/>

Dropping incoming UDP datagrams is an inbound building block that can also be applied to QUIC traffic, however, there is no known attack vector similar to TCP RST. This means that as of now, there is no known way how a censor could terminate a QUIC connection by injecting a packet. <br/>

### QUIC packet level
*duplicate* can also be applied to QUIC traffic on the QUIC packet level. This action would copy a given QUIC packet and send it in the same or a new UDP datagram. (QUIC packets are handled by the receiving peer as if they were received in separate datagrams.) <br/>

Running a *drop* action for incoming QUIC packets does not seem beneficial as it cannot be expected (due to efficiency reasons) that a censor would inject a single QUIC packet into an in-flight UDP datagram. This action should still be considered when drafting up QUIC censorship evasion building blocks.<br/>

Fragmentation cannot be applied to QUIC packets which is explicitly stated in RFC 9000: *Frames always fit within a single QUIC packet and cannot span multiple packets.* ([RFC 9000](https://www.rfc-editor.org/rfc/rfc9000.html#name-frames-and-frame-types))
