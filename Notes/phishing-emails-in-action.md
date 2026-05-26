# Cancel Your Order

**Phishing Techniques Used**

- **Spoofed email address**: Mimicking a trusted service to gain immediate credibility
- **URL shortening**: Using redirection services to hide the true destination of a link
- **Branding HTML**: Impersonating legitimate corporate imagery to create a sense of authenticity

## First Observations

1) **Attention-grabbing subject line**: The subject line uses a fake transaction to create a sense of urgency, prompting you to react in haste
2) **From Address**: This is an immediate red flag as the sender details *service@paypal.com* do not match the actual address *gibberish@sultanbogor.com*
3) **To Address**: This is an unusual email recipient address and not a normal Yahoo domain

![First-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155041580.png)

## Email Body Analysis

The email is designed to appears as a legitimate email from paypal. There aren't any attachments associated with this email and the only interactive element in this email is the *Cancel the Order* button.
![Email-Body](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774315608150.svg)

### Button Investigation

By inspecting the raw source of the email, we can further investigate the hyperlinks and underlying HTML that form the message. The *Cancel the Order* button leads to the shortened URL. Because the attacker is using a URL shortening service, the final destination is obfuscated, making it impossible to verify the landing page at a glance.
> Never interact with buttons or links without first confirming exactly where they lead.

![Raw-source](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155041555.png)

> We can use a tool call [WhereGoes](https://wheregoes.com/) to investigate shortened URLs without having to actually visit the destination.

![WhereGoes-Output](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155041518.png)

# Track Your Package

**Phishing Techniques Used**

- **Spoofed email address**: Mimicking a trusted distribution center to gain immediate credibility
- **Pixel tracking**: Embedding invisible images to notify the sender when the email is opened
- **Link Manipulation**: Masking a malicious destination with a fraudulent tracking number

## First Observations

1) **Subject line**: The subject line uses a fake tracking number to create a sense of urgency, prompting the recipient to click to see their package status
2) **From address**: This is an immediate red flag, as the display name **Distribution Center** does not match the actual sender address *contact@beginpro.club*
3) **Hyperlink**: The link in the email body matches the subject line, although we do not know where it directs to just yet

![First-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155041580.png)

## Hyperlink Tracking

These trackers send information back to the spammer's server about whether you have opened the email, when you opened it and where you were at the time by using tracking pixels.

![hyperlink-tracking](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155517052.svg)

# Download Document Here

**Phishing Techniques Used**
- **Artificial urgency**: Creating a narrow window for action to create a sense of urgency
- **Brand Impersonation**: Layering trusted brands, like Microsoft and Adobe, to build a false sense of security
- **Link redirection**: Using a chain or URLs to hide the final malicious destination from basic email filters
- **Credential harvesting**: Deploying a fake login portal to capture and exfiltrate usernames and passwords

## First Observations

1) **Send date**: The email was sent on Thursday, July 15th, 2021
2) **Expiration date**: A sense of urgency is introduced in this email. Notice that the link to download the fax document exires on the same day
3) **Download Document Here button**: There is an action to perform. In this case, a button to download the fax

![first-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774155942403.png)

# Your Account is on Hold

**Phishing Techniques Used**
- **Spoofed email address**: The sender's display name is set to **Netflix billing** to appear legitimate
- **Sense of Urgency**: Using a suspended account notification to pressure the victim into acting quickly
- **Brand impersonation**: Utilizing HTML templates and logos to mimic Netflix billing
- **Poor grammar and typos**: Noticeable misspellings of Netflix
- **Attachments**: Using a file attachment rather than a direct link to hide the malicious URL

## First Observations

1) **Email Subject**: The receiver's ID was suspended and must act quickly
2) **From address**: The display name does not match the user and domain
3) **Brand impersonation**: The email utilizes rendered HTML to impersonate Netflix

![first-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774156168798.png)

# Your Recent Purchase

**Phishing Techniques Used**
- **Spoofed email address**: The sender's display name is set to **Apple Support**
- **Recipient is BCCed**: The victim is not directly sent the email
- **Urgency**: Relies on the use of **Action Required** and a fake purchase notification
- **Poor grammar and typos**: Noticeable spelling erros within the email header
- **Attachments**: The email contains a *.dot* file (Microsoft Word Template), which is an unusual format for a receipt

## First Observations

1) **Email subjects**: The recipient is told they must act quickly to resolve an unauthorized purchase, creating a false sense of urgency
2) **From address**: The display name, **Apple Support** does not match the user and domain. There are also typos in the From and To addresses
3) **Blind Carbon Copy**: The recipient was not directly emailed, instead *Blind Carbon Copied* (BCC)

![first-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774156403764.png)

## Analysing the Attachment

The body of the email is completely blank, which is suspicious on its own. The only content present is an attachment in the form of a *.dot* file. When the user interacts with the large image embedded in the document, they are redirected to a phishing site. Although the URL includes familiar terms like apps and ios to appear legitimate, its excessive length and complexity are strong indications of a malicious redirection attempt.

![analyzing-attachment](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774328679571.svg)

# Scheduled Shipment

**Phishing Techniques Used**
- **Spoofed email address**: The sender's display name is set to *DHL EXPRESS*
- **Brand impersonation**: Utilizing HTML templates and logos to mimic DHL
- **Attachments**: An Excel document that triggers executable code upon opening

## First Observations

1) **Email subject**: Gives the impression that DHL will be shipping a package
2) **From address**: The display name, *DHL Express* does not match the user and domain
3) **Brand impersonation**: The HTML in the email body is designed to look like it is sent from DHL

![first-observation](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774156685057.png)


