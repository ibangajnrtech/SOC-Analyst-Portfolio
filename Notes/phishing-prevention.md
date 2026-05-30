# Sender Policy Framework

Sender Policy Framework is used to authenticate the sender of an email. With an SPF record in place, Internet Service Providers can verify that a mail server is authorized to send email for a specific domain. An SPF record is a DNS TXT record containing a list of the IP addresses that are allowed to send email on behalf of your domain.

![Diagram of SPF](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759814016781.svg)

## Verification Results and their Intended Actions

- **Pass, Neutral, None**: Intended to Accept (Allowand process the email)
- **SoftFail, PermError**: Intended to Flag (Mark as suspicious but allow)
- **Fail, TempError**: Intended to Reject (Immediately discard the email)

## SPF Records

Example of an SPF Record: `v=spf1 ip4:127.0.0.1 include:_spf.google.com -all`

- **v=spf1**: Signifies the start of the SPF record
- **ip4:127.0.0.1**: Specifies which IP can send mail (IPv4 in this case)
- **include:_spf.google.com**: Specifies which domain can send mail
- **-all**: Non-authorized emails will be rejected

Google Admin Toolbox [Messageheader](https://toolbox.googleapps.com/apps/messageheader/) allows you to analyze delivery details using an email's full header. It shows various record results, including SPF. 

# Domain Keys Identified Mail (DKIM)

Domain Keys Identified Mail (DKIM) is used for authentication of an email that's being sent.
Like SPF, DKIM is an open standard for email authentication that is used for DMARC alignment. A DKIM record exists in the DNS, but it is more complex than SPF. DKIM’s advantage is that it can survive forwarding, which makes it superior to SPF and a foundation for securing your email.

![Diagram of DKIM](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759815078151.svg)

## DKIM Records

Example of a DKIM record: `v=DKIM1; k=rsa; p=<public_key>`
- **v=DKIM1**: Specifies the version of DKIM being used (optional)
- **k=rsa**: The key type. The RSA encryption algorithm is standard
- **p=**: This is the public key that will be matched to the private key to verify the DKIM signature

# Domain-Based Message Authentication, Reporting and Conformance (DMARC)

DMARC, an open source standard, uses a concept called alignment to tie the result of two other open source standards,  SPF (a published list of servers that are authorized to send email on behalf of a domain) and DKIM (a tamper-evident domain seal associated with a piece of email), to the content of an email.

## DMARC Records

Example of a DMARC record: `v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com`
- **v=DMARC1**: The version of DMARC (required)
- **p=quarantine**: The DMARC policy (quarantine = move to the spam folder)
- **rua=mailto:postmaster@website.com**: An optional tag. In this case, aggregate reports will be sent to the email specified

# Secure/Multipurpose Internet Mail Extensions (S/MIME)

Secure/Multipurpose Internet Mail Extensions (S/MIME) is a standard protocol for sending digitally signed and encrypted messages. It is based on public key cryptography, where the private key is never shared and the public key can be distributed openly. The two main components and security features of S/MIME are:

1) **Digital Signature**: The sender signs the message with their private key, the recepient verifies the sender's identity using the sender's public key. This security feature provides:

- **Authentication**: Confirms the sender's identity through their digital certificate
- **Non-repudiation**: Ensures the sender cannot deny sending the message
- **Data Integrity**: Detects any changes to the message after it's signed

2) **Encryption**: The sender encrypts the message using the recipient's public key, allowing only the recipient to decrypt it with their private key. This security feature provides:

- **Confidentiality**: Keeps the content private and readable only by the intended recipient

# How Organisations Stop Phishing

## Technical Defenses

- **Email Filtering**: Provides filtering based on IP and domain reputation, allowing for blocking or quarantining of suspicious messages.
- **Secure Email Gateways (SEGs)**: Scan messages to detect impersonation attempts, spoofing, and other phishing techniques that other filters might miss.
- **Link Rewriting**: Replaces suspicious or unknown URLs with safe, redirected ones, giving the system time to scan and verify the link.
- **Sandboxing**: Isolates and tests suspicious links or attachments in a secure, virtual environment to check for malicious behavior.

## User-Facing Tools & Training

- **Trust & Warning Indicators**: Modern email platforms display visual cues to help users understand if a message is safe. A banner may read “External Sender,” “Suspicious Link,” or signify that a message is from a trusted organization or sender. 
- **Phishing Reporting**: Easy, in-email reporting options that let users quickly report suspicious messages.
- **User Awareness Training**: Train employees on identifying phishing attempts, social engineering tactics, and safe email practices.
- **Phishing Simulation Exercises**: Run controlled phishing campaigns to test and reinforce employee training.