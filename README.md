<h1>Email Header and Sender Analysis (Hands On)</h1>

I opened this Email (sample1.eml) in Thunderbird, as we can see the email just looks like a normal email from Chase Bank, appears to be asking to restore or reactivate our account and we can see some of our comman headers. We have the From field (Red), which is "alerts@chase.com", we have the To field (Orange) which is "Bob Sanders", the Subject line (Green) which is " Your Bank Account has been blocked due to unusual activities", and as well as the Time stap (Purple) which is "5/1/24, 16:04".</b>

<img src="https://i.imgur.com/Ez904XO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<h2>Sublime Text</h2>

 Just looking at Email itself isn't enough, so let's dig deep using Sublime! I opened my terminal on Ubuntu and navigated to the downloaded file of the email, after I used the command <b>(subl sample1.eml)</b> to open the email in Sublime Text 

<img src="https://i.imgur.com/E8pg4QN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

Here we have the email message opened up in Sublime Text, but when we open up an email in Sublime Text by default, we don't get the benefit of syntax highlighting that makes our lives easier! 

To enable syntax highlighting for email messages in Sublime Text:

Install Package Control by going to "Tools" > "Install Package Control..." If already installed, skip this step.

Open the Command Palette with <b>CTRL+SHIFT+P</b> (or <b>CMD+SHIFT+P</b> on Mac), type "Package Control: Install Package," and select it. Search for "Email Header" and install the one from "<b>github.com/13Cubed/EmailHeader</b>"

Open your email file, click "Plain Text" at the bottom right, type "Email Header," and select it. Syntax highlighting will be applied.

<img src="https://i.imgur.com/qRrKSA7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<h2>Most Comman/Useful Headers</h2>

- <b>Date</b>

The date header in an email specifies the date and time when the message was composed or sent. We'll often want to document the date and time of an email for our records, whether it be this time stap directly from the header or from the email client or from the ticket that was created. Knowing when an email was delivered can be very useful in performing email searches across the organization for similar emails or for correlating other suspicious activity that might have resulted in a phish, like in our sim, for instance.

- <b>From</b>

The from header, as we're likely familiar with, specifies the supposed sender of an email. This is the header that will be displayed in an email clients to show who sent a specific email. However, because the from header can be set to any arbitrary value, well this is one of the most common headers that attackers can spoof to make it look like a phishing email came from someone else or trusted organization. But we should still document this though, because we can use it to find discrepancies as well as use it as an artifact to search off of.

- <b>Subject</b>

The Subject field specifies the topic or main point of the email. It provides a brief summary or description of the email's content and is displayed in the recipient's inbox to indicate what the email is about. The Subject is crucial for catching the recipient's attention and often determines whether the email will be opened or ignored. 

- <b>Message ID</b>

The Message-ID field in an email header is a unique identifier assigned to every email message. According to the standard documents, it should be a unique identifier, although there are no specific algorithm requirements defined for generating it. The Message-ID is used to differentiate the email from all others, aiding in email threading, identifying duplicate messages, and tracking the email as it is forwarded or replied to..

- <b>To</b>

The To header in an email specifies the primary recipients of the email. It contains one or more email addresses to which the message is directly addressed. The addresses listed in the To field can represent individual recipients, mailing lists, or group aliases.

- <b>Reply-TO</b>

The Reply-To header in an email specifies the email address or addresses to which replies should be sent. If the Reply-To header is present, it overrides the From address when the recipient uses the "Reply" option in their email client. This is particularly useful when the sender wants responses to go to a different email address or multiple addresses instead of the one from which the email was originally sent.

- <b>Return-Path</b> 

The Return-Path header in an email indicates the address to which bounced messages (non-delivery notifications) should be sent. It is often referred to as the "envelope sender" address, as it represents the actual sender of the email at the protocol level (SMTP). The Return-Path is set by the email server when the message is initially sent and is not typically modified by end users.

- <b>X-Sender-IP / X-Originated-IP</b>

The X-Sender-IP header in an email is an optional and non-standard header that may be used to indicate the IP address of the sender's mail server. It is typically used by email systems to provide additional information about the source of the email, which can be useful for spam filtering and diagnosing delivery issues.

- <b>Recieved</b>

The Received header in an email records the path taken by the email from the sender to the recipient. It is added by each mail server that handles the email, providing a timestamp and the server's details. This header is useful for tracking the email's journey, diagnosing delivery issues, and identifying potential spam or malicious activity.

Each Received header entry includes information such as:

- Read form bottom to top (Bottom= closest  to the sender, Top= closest to the reciver).

- The timestamp of when the mail server received the message.

- The IP address and hostname of the sending server (Ex. 185.70.40.140) .

- The protocol used (e.g., SMTP).

<h2>whois</h2>

Now that we have something going on lets investigate the IP address I found! I used the <b>whois</b> command on my terminal for more digging! 
 
<img src="https://i.imgur.com/Dv6PpWR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<h2>whois.domaintools.com</h2>

The command line isnt he only way to search valueble information! Let's go into our favirate browser and search "<b>https://whois.domaintools.com/</b>". 

<img src="https://i.imgur.com/MvgKVLQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<img src="https://i.imgur.com/zBUXVWQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

This IP address belongs to is in "Switzerland" and the reverse DNS lookup, or the resolved host, points to "<b>mail-40140.protonmail.ch</b>", and we can also see the ASN, or the autonomouse system number, belongs to "<b>Proton AG</b>", which confirms that we are dealing with a Proton Mail email system that originally sent this email. This obviously does not match up with Chase Bank, which is what the sender wanted us to belive by spoofing the sender address as "<b>alerts@chase.com</b>.
