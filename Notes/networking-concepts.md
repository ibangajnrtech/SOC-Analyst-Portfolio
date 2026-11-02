# OSI Model

The OSI model is composed of seven layers:

1) **Physical Layer**: The physical layer, also referred to as layer 1, deals with the physical connection between devices; this includes the medium, such as a wire, and the definition of the binary digits 0 and 1. Data transmission can be via an electrical, optical, or wireless signal.
2) **Data Link Layer**: The data link layer, i.e., layer 2, represents the protocol that enables data transfer between nodes on the same network segment. Examples of layer 2 include Ethernet, i.e., 802.3, and WiFi, i.e., 802.11. Ethernet and WiFi addresses are six bytes. Their address is called a MAC address, where MAC stands for Media Access Control. Examples of layer 2 standards are Ethernet (802.3), WiFi (802.11), etc.
3) **Network Layer**: The network layer, i.e., layer 3, is concerned with sending data between different networks. In more technical terms, the network layer handles logical addressing and routing, i.e., finding a path to transfer the network packets between the diverse networks. Examples of layer 3 protocols are IP, ICMP, IPSec, etc.
4) **Transport Layer**: Layer 4, the transport layer, enables end-to-end communication between running applications on different hosts. Examples of layer 4 are Transmission Control Protocol (TCP) and User Datagram Protocol (UDP).
5) **Session Layer**: The session layer is responsible for establishing, maintaining, and synchronising communication between applications running on different hosts. Establishing a session means initiating communication between applications and negotiating the necessary parameters for the session.Examples of the session layer are Network File System (NFS) and Remote Procedure Call (RPC). Examples of layer 5 protocols are NFS, RPC, etc.
6) **Presentation Layer**: The presentation layer ensures the data is delivered in a form the application layer can understand. Layer 6 handles data encoding, compression, and encryption. An example of encoding is character encoding, such as ASCII or Unicode. Examples of layer 6 standards are Unicode, MIME, JPEG, PNG, MPEG, etc.
7) **Application Layer**: The application layer provides network services directly to end-user applications. Your web browser would use the HTTP protocol to request a file, submit a form, or upload a file. Examples of Layer 7 protocols are HTTP, FTP, DNS, POP3, SMTP, and IMAP.

# TCP/IP Model

From top to bottom, we have:

1) **Application Layer**: The OSI model application, presentation and session layers, i.e., layers 5, 6, and 7, are grouped into the application layer in the TCP/IP model.
2) **Transport Layer**: This is layer 4.
3) **Internet Layer**: This is layer 3. The OSI model’s network layer is called the Internet layer in the TCP/IP model.
4) **Link Layer**: This is layer 2.

# IP Addresses aand Subnets

Every host on the network needs a unique identifier for other hosts to communicate with him. Without a unique identifier, the host cannot be found without ambiguity. When using the TCP/IP protocol suite, we need to assign an IP address for each device connected to the network.
The 0 and 255 are reserved for the network and broadcast addresses respectively. This means that `192.168.1.0` is the network address, while `192.168.1.255` is the broadcast address.
Sending to the broadcast address targets all the hosts on the network. 

## Looking up your Network Configuration

You can look up your IP address on the MS Windows command line using the command `ipconfig`. On Linux and UNIX-based systems, you can issue the command `ifconfig` or `ip address show`, which can be typed as `ip a s`.

## Private Addresses

There are two types of IP addresses:

- Public IP addresses
- Private IP addresses

RFC 1918 defines the following three ranges of private IP addresses:

- `10.0.0.0` - `10.255.255.255` (`10/8`)
- `172.16.0.0` - `172.31.255.255` (`172.16/12`)
- `192.168.0.0` - `192.168.255.255` (`192.168/16`)

A private IP address is different; it cannot reach or be reached from the outside world.
For a private IP address to access the Internet, the router must have a public IP address and must support Network Address Translation (NAT).

## Router

A router is like your local post office; you hand them the mail parcel, and they would know how to deliver it.