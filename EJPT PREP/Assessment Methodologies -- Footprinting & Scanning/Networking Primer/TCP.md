TCP, or Transmission Control Protocol, is one of the main protocols operating at the Transport Layer (Layer 4) of the OSI model. 

It is a connection-oriented, reliable protocol that provides a dependable and ordered delivery of data between two devices over a network. 

TCP ensures that data sent from one application on a device is received accurately and in the correct order by another application on a different device.

Connection-Oriented: 
+ TCP establishes a connection between the sender and receiver before any data is exchanged. This connection is a virtual circuit that ensures reliable and ordered data transfer.

Reliability: 
+ TCP guarantees reliable delivery of data. It achieves this through mechanisms such as acknowledgments (ACK) and retransmission of lost or corrupted packets. If a segment of data is not acknowledged, TCP automatically resends the segment

Ordered Data Transfer: 
+ TCP ensures that data is delivered in the correct order. If segments of data arrive out of order, TCP reorders them before passing them to the higher layer application

## TCP 3-Way Handshake
The TCP three-way handshake is a process used to establish a reliable connection between two devices before they begin data transmission

It involves a series of three messages exchanged between the sender (client) and the receiver (server).

SYN (Synchronize): The process begins with the client sending a TCP segment with the SYN (Synchronize) flag set. This initial message indicates the client's intention to establish a connection and includes an initial sequence number (ISN), which is a randomly chosen value.

SYN-ACK (Synchronize-Acknowledge): Upon receiving the SYN segment, the server responds with a TCP segment that has both the SYN and ACK (Acknowledge) flags set. The acknowledgment (ACK) number is set to one more than the initial sequence number received in the client's SYN segment. The server also generates its own initial sequence number

ACK (Acknowledge): Finally, the client acknowledges the server's response by sending a TCP segment with the ACK flag set. The acknowledgment number is set to one more than the server's initial sequence number.

At this point, the connection is established, and both devices can begin transmitting data. ● After the three-way handshake is complete, the devices can exchange data in both directions. The acknowledgment numbers in subsequent segments are used to confirm the receipt of data and to manage the flow of information

## TCP Control Flags

 TCP (Transmission Control Protocol) uses a set of control flags to manage various aspects of the communication process. 

 These flags are included in the TCP header and control different features during the establishment, maintenance, and termination of a TCP connection.


Establishing a Connection: + SYN (Set): Initiates a connection request. + ACK (Clear): No acknowledgment yet. + FIN (Clear): No termination request. 

Establishing a Connection (Response): + SYN (Set): Acknowledges the connection request. + ACK (Set): Acknowledges the received data. + FIN (Clear): No termination request

Terminating a Connection: + SYN (Clear): No connection request. + ACK (Set): Acknowledges the received data. + FIN (Set): Initiates connection termination

## TCP Port Range
TCP (Transmission Control Protocol) uses port numbers to distinguish between different services or applications on a device. 

Port numbers are 16-bit unsigned integers, and they are divided into three ranges. 

The maximum port number in the TCP/IP protocol suite is 65,535

Well-Known Ports (0-1023): Port numbers from 0 to 1023 are reserved for well-known services and protocols. These are standardized by the Internet Assigned Numbers Authority (IANA). Examples include: 
+ 80: HTTP (Hypertext Transfer Protocol) 
+ 443: HTTPS (HTTP Secure) 
+ 21: FTP (File Transfer Protocol) 
+ 22: SSH (Secure Shell) 
+ 25: SMTP (Simple Mail Transfer Protocol) 
+ 110: POP3 (Post Office Protocol version 3

Registered Ports (1024-49151): Port numbers from 1024 to 49151 are registered for specific services or applications. These are typically assigned by the IANA to software vendors or developers for their applications. While they are not standardized, they are often used for well-known services. Examples include: 
+ 3389: Remote Desktop Protocol (RDP)
+ 3306: MySQL Database 
+ 8080: HTTP alternative port 
+ 27017: MongoDB Database
