# TLS (Transport Layer Security)

The first step for every server (or client) that needs to identify itself is to get a signed TLS certificate. Generally, the server administrator creates a Certificate Signing Request (CSR) and submits it to a Certificate Authority (CA); the CA verifies the CSR and issues a digital certificate. Once the (signed) certificate is received, it can be used to identify the server (or the client) to others, who can confirm the validity of the signature. For a host to confirm the validity of a signed certificate, the certificates of the signing authorities need to be installed on the host.

# HTTP (HyperText Transfer Protocol)

HTTP relies on TCP and uses port 80 by default.
After resolving the domain name to an IP address, the client will carry out the following two steps:
- Establish a TCP three-way handshake with the target server
- Communicate using the HTTP protocol; for example, issue HTTP requests, such as `GET / HTTP/1.1`

## HTTP Over TLS

HTTPS stands for Hypertext Transfer Protocol Secure. It is basically HTTP over TLS.
Requesting a page over HTTPS will require the following three steps (after resolving the domain name):
- Establish a TCP three-way handshake with the target server
- Establish a TLS session
- Communicate using the HTTP protocol; for example, issue HTTP requests, such as `GET / HTTP/1.1`

# SMTPS, POP3S and IMAPS

Adding TLS to SMTP, POP3, and IMAP is no different than adding TLS to HTTP.
Using these protocols over TLS is no different than using HTTP over TLS.

The secure versions i.e over TLS use the following TCP port numbers by default:
1) HTTPS -> 443
2) SMTPS -> 465 and 587
3) POP3S -> 995
4) IMAPS -> 993

# SSH

OpenSSH offers several benefits. We will list a few key points:

- **Secure authentication**: Besides password-based authentication, SSH supports public key and two-factor authentication.
- **Confidentiality**: OpenSSH provides end-to-end encryption, protecting against eavesdropping. Furthermore, it notifies you of new server keys to protect against man-in-the-middle attacks.
- **Integrity**: In addition to protecting the confidentiality of the exchanged data, cryptography also protects the integrity of the traffic.
- **Tunneling**: SSH can create a secure “tunnel” to route other protocols through SSH. This setup leads to a VPN-like connection.
- **X11 Forwarding**: If you connect to a Unix-like system with a graphical user interface, SSH allows you to use the graphical application over the network.

**While the TELNET server listens on port 23, the SSH server listens on port 22.**

# SFTP and FTPS

SFTP stands for SSH File Transfer Protocol and allows secure file transfer. It is part of the SSH protocol suite and shares the same port number, 22. If enabled in the OpenSSH server configuration, you can connect using a command such as `sftp username@hostname`.

FTPS is secured using TLS, just like HTTPS. While FTP uses port 21, FTPS usually uses port 990. It requires certificate setup, and it can be tricky to allow over strict firewalls as it uses separate connections for control and data transfer.

Setting up an SFTP server is as easy as enabling an option within the OpenSSH server. Like HTTPS, SMTPS, POP3S, IMAPS, and other protocols that rely on TLS for security, FTPS requires a proper TLS certificate to run securely.

# VPN (Virtual Private Network)

A VPN client in the remote branches is expected to connect to the VPN server in the main branch. In this case, the VPN client will encrypt the traffic and pass it to the main branch via the established VPN tunnel.
Once a VPN tunnel is established, all our Internet traffic will usually be routed over the VPN connection, i.e. via the VPN tunnel.
Consequently, when we try to access an Internet service or web application, they will not see our public IP address but the VPN server’s.