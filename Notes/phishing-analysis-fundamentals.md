# Anatomy of an Email Address

1) **Username**: User mailbox that identifies the specific recipient's mailbox on  the email server.
2) **@** symbol: Separates the username from the domain and tells the system where to route the email.
3) **Domain name**: Specifies the mail server responsible for receiving the message.

# Email Delivery

When you send an email, several protocols work together behind the scenes to deliver your message from sender to recipient, and each protocol has a specific role:
- **Simple Mail Transfer Protocol (SMTP)**: Sends emails
- **Post Office Protocol (POP3)**: Downloads emails to a device
- **Internet Message Access Protocol (IMAP)**: Syncs emails across devices

## POP3

- Emails are downloaded and stored on a single device
- Sent messages are stored on the single device from which the email was sent
- Emails can only accessed from the single device to which the emails were sent
- Emails are typically removed from the server after download

## IMAP

- Emails are stored on the server and can be downloaded to multiple devices
- Sent messages are stored on the server
- Syncs messages across multiple devices
- Emails remain on the server unless explicitly deleted

## An Email's Journey

1) User sends an email: The sender's email client sends the message to their mail server using SMTP
2) Mail server queries DNS: The sending server asks DNS for the recipient domain's mail server
3) DNS responds: DNS returns the address of the recipient's mail server
4) Email is delivered: The message is sent across the internet to the recipient's server
5) The recipient checks their mailbox: The recipient's email client connects to their mail server
6) Email is retrieved: The message is downloaded (POP3) or synced (IMAP) to the recipient's device

# Email Header

An email consists of two main parts:
- **Email Header**: Contains metadata about the message, such as sender and the servers involved in delivery.
- **Email body**: Contains the actual message content, which may be plain text or HTML

## Components of the Email Header
1) **From**: The sender's email address
2) **To**: The receiver's email address
3) **Reply to**: Address where replies are sent (not required)
4) **Subject**: The email's subject line
5) **Date**: The time and date that the email was sent

> Another method to obtain the same email header information, and more, is by viewing the raw message data. This displays the full raw message, including all header fields and email body, which may contain plain text or HTML. It also reveals technical details not visible in the standard inbox view.

# Types of Phishing

Different types of malicious emails can be classified as one of the following:
1) **Spam**: Unsolicited bulk emails sent to a large number of recipients. A more malicious form of spam is often called malspam.
2) **Phishing**: Emails that impersonate a trusted entity to trick recipients into revealing sensitive information.
3) **Spear Phishing**: A targeted form of phishing aimed at a specific individual or organisation, often using personalised information.
4) **Whaling**: A type of spear phishing that specifically targets high-level executives (CEO, CFO) to obtain sensitive data or financial access.
5) **Smishing**: Phishing attacks conducted via SMS or text messages, targeting users on mobile devices.
6) **Vishing**: Phishing attacks carried out through voice calls, where attackers use social engineering over the phone.

## Anatomy of a Phishing Email

- **Spoofed From Address**: The sender's email is spoofed to appear as a trusted entity (*noreply@microsof.com*)
- **Urgent Subject or Message**: The email creates a sense of urgency (*Your account will be locked in 24 hours*)
- **Brand Impersonation**: The email is designed to mimic a legitimate organisation (logos or colors matchin a real company)
- **Grammar & Spelling Issues**: The message may contain errors, though with AI these are now less common (awkward phrasing or unnatural wording)
- **Generic Content**: The message lacks personalization (*Dear Customer* instead of your name)
- **Hidden or Shortened Links**: Hyperlinks may disguise their true destination (*bit.ly/secure-login*)
- **Malicious Attachments**: Attachments are included and disguised as legitimate files (*invoice.pdf.exe*)

## Safe Analysis

When dealing with hyperlinks and attachments, be careful not to click them accidentally. [Hyperlinks](https://static-labs.tryhackme.cloud/sites/eviction/) and [IP-Addresses](https://static-labs.tryhackme.cloud/sites/eviction/) should be defanged.
Defanging makes URLS, domains, or email addresses unclickable to prevent accidental clicks that could lead to a security breach. It works by replacing special characters such as *@* in an email or *.* in a URL, with alternate characters:
