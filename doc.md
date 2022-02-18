This document contains observed cases of QUIC and HTTP/3 censorship in various countries.

Censoring QUIC proofs that censors are continously adapting and improving their censorship methodologies with the aim to block unwanted website traffic in the most comprehensive way.

## UDP endpoint blocking

**What is it?** <br/>
Blocking that targets certain UDP endpoints. A UDP endpoint is the unique combination of the IP address and the UDP port used. 
In such a scenario, blocking triggers are at least the IP address and the protocol type (`udp`) and possible also the 
port number (`443`, for HTTP/3).

UDP endpoint blocking differs from IP blocking in the sense that IP blocking affects all protocols above layer 3. Thus, HTTP/S and HTTP/3 are equally impaired by IP blocking. UDP endpoint blocking is more precise and allows for different treatment of transport protocols.

UDP endpoint blocking is probably cheap for censors as it does not require Deep Packet Inspection but rather only the inspection of very few IP and UDP header fields.  

**How to detect it**<br/>

<img src="images/detect_udp_endpoint_blocking_quer.png" alt="decision tree udp endpoint blocking"/>

**Oberserved in**<br/>
Iran, 2021

**Circumvention approaches**<br/>
Connection migration feature.


## SNI blocking


**Observed in**<br/>



