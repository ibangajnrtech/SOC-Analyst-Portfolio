# Cancel Your Order

**Phishing Techniques Used**

- **Spoofed email address**: Mimicking a trusted service to gain immediate credibility
- **URL shortening**: Using redirection services to hide the true destination of a link
- **Branding HTML**: Impersonating legitimate corporate imagery to create a sense of authenticity

## First Observations

1) **Attention-grabbing subject line**: The subject line uses a fake transaction to create a sense of urgency, prompting you to react in haste
2) **From Address**: This is an immediate red flag as the sender details *service@paypal.com* do not match the actual address *gibberish@sultanbogor.com*
3) **To Address**: This is an unusual email recipient address and not a normal Yahoo domain

## Email Body Analysis

The email is designed to appears as a legitimate email from paypal. There aren't any attachments associated with this email and the only interactive element in this email is the *Cancel the Order* button.
![Email-Body](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774315608150.svg)

### Button Investigation

By inspecting the raw source of the email, we can further investigate the hyperlinks and underlying HTML that form the message. The *Cancel the Order* button leads to the shortened URL. Because the attacker is using a URL shortening service, the final destination is obfuscated, making it impossible to verify the landing page at a glance.
> Never interact with buttons or links without first confirming exactly where they lead.
![Raw-source](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155041555.png)

> We can use a tool call [WhereGoes](https://wheregoes.com/) to investigate shortened URLs without having to actually visit the destination.

