## Network Protocols

● In computer networks, hosts communicate with each other through the use of network protocols
● Network protocols ensure that different computer systems, using different hardware and software can communicate with each other.
● There are a large number of network protocols used by different services for different objectives/functionality. 
● Communication between different hosts via protocols is transferred/facilitated through the use of packets. Packets

## Packets

● The primary goal of networking is the exchange information between networked computers; this information is transferred by packets. 
● Packets are nothing but streams of bits running as electric signals on physical media used for data transmission. (Ethernet, Wi-Fi etc) 
● These electrical signals are then interpreted as bits (zeros and ones) that make up the information
______


**header:

The header has a protocol-specific structure: this ensures that the receiving host can correctly interpret the payload and handle the overall communication.

**payload:

The payload is the actual information being sent . It could be something like part of an email message or the content of a file during a download.

```
+--------------------------------------------------------------------+
|                           PACKET                                    |
| +----------------+ +--------------------------------------------+  |
| |     HEADER     | |                  PAYLOAD                   |  |
| |                | |                                            |  |
| | - Contains control information | - The actual data being sent  |  |
| |   (source/destination addresses|   (e.g., part of an email,    |  |
| |    protocol type, sequence     |    file fragment, web page    |  |
| |    numbers, error checking)    |    content)                   |  |
| |                                |                                |  |
| | - Ensures the receiving host   | - The "message" or purpose of  |  |
| |   can correctly interpret and  |   the communication            |  |
| |   handle the communication     |                                |  |
| |                                |                                |  |
| | - Protocol-specific structure  |                                |  |
| +----------------+ +--------------------------------------------+  |
+--------------------------------------------------------------------+
```

____________

**The OSI Model 

● The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes the functions of a telecommunication or computing system into seven abstraction layers. 
● It was developed by the International Organization for Standardization (ISO) to facilitate communication between different systems and devices, ensuring interoperability and understanding across a wide range of networking technologies. 
● The OSI model is divided into seven layers, each representing a specific functionality in the process of network communication.

