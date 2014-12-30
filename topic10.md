# Topic 10: EMail Spam
## Basics
- UCE: Unsolicited Commercial EMail
- UBE: Unsolicited Bulk EMail, non-commercial mails send in masses
- Spam: junk-mail, unsolicited EMail, most cases commercial and send in bulk
- 'unsolicited': recipient didn't give (implicit) permission to receive it
- non-spam mails sometimes referred to as «ham»
- Over 60% of email traffic is spam
- Problems caused by spam
	- Technical resources are misused
	- Wastes time (and therefore money) of IT statt and end users
	- Wasted other resources (e.g. energy) and therefore, again, money
	- DoS
	- May carry malware
	- Results in environmental impact (e.g. CO2)
- EMail Address collection
	- Directory Harvest Attack (DHA) against mail server: guess address and send mail to confirm
	- Crawlers: crawle websites and extract addresses
	- Malware: collect locally stored addresses
	- Databases/Applications: use public directories, hacked online shops, etc.
- Reporting
	- Inform US Federal Trace Commission for fraud attempt and because of a NYC address, results in webpage take down
	- Report misuse of account on a webpage, account usually removed by webpage operator
- Variaties
	- Mails with malware attachment
	- Propaganda
	- Chain letters
	- Pyramid schemes
	- Hoaxes
	- Appeal for funds
	- Advertising for (illegal) activities (e.g. online gambling)
	- etc.
- Spam is illegal in Switzerland (if message sent by someone in Switzerland), similar legislation in other countries

## Business Models
- Direct sales
	- Some people (~7% in UK) buy stuff sold by spam
	- Is a very cheap advertising channel
- Underground Business
	- Operators rent out botnets to send spam
	- Sell addresses

## Anti-Spam Techniques
- It's hard to define what is spam and what not
- Multiple ways what to do with mails categorised as spam
	- Delete/reject it
	- Mark it (e.g. add some kind of additional string to email subject)
	- Quarantine it into a special folder
	- Tag it by adding some header to mail (e.g. X-SPAM-FLAG)
- Whitelist: only emails matching specific criteria allowed
- Greylist: defer initial email, but accept followup mails
- Blacklist: reject emails matching specific criteria
- DNS Blacklist
	- Do a DNS lookup against a DNS blacklist provider for `<email source IP address>.<DNS blacklist domain>`. If a record is returned the source of the email is blacklisted, otherwise not
	- Same can be done for the source domain instead of the source IP address
- Realtime Blacklists (RBLs)
	- Nomination
		- Manually by list administrator
		- User submitted
		- Tested by trusted testers
		- etc.
	- Listing lifetime
		- relatively short (5 seconds up to 20 minutes)
		- until de-listing requested
		- until spam stops
	- Can be defeated by botnet machines, stolen email accounts, short-term domains, etc.
	- Can be used to DoS someone (e.g. sometimes whole /24 IP address space is blacklisted)
- Firewalls can deploy rules to not allow outgoing emails that didn't go trough the internal email server (e.g. should stops malware to send directly spam email)
- Heuristic content filtering
	- Run algorithm that identifies usual features of spam email content (e.g. unusual use of fonts, tiled images, "strange" URLs, etc.)
	- Score features to calculate a single score for whole email
	- If threshold reached by score, identify email as spam
- Statistical content filtering
	- Filter trained on known spam and legitimate emails
	- Calculate probability that given the new email address is spam
- Distributed checksum clearinghouse (DCC)
	- Counts number of same email messages seen
	- High counts indicates spam
	- Large email providers can detect new spam by checksumming incoming emails
- Image spam filtering
	- Sometimes attachments (images, PDFs, etc.) inside spam mail used
	- Do optical character recognition (OCR) to extract text and apply previously mentioned techniques
	- Signature-based detection afterwards

## Email Sender Authentication

### DomainKey Identified Mail (DKIM)
- Sender's email server signs hash of email with its private key
- Receiver's email server fetches public key of sender and verifies signature
- Guarantees message integrity and sender authenticity
- Based on domain (domain-level digital signature)
- Organisation takes responsibility for message
- Goal is to prevent phishing with a PKI

### Sender Policy Framework (SPF)
- Most emails with forged sender address are spam
- Allow domain owner to declare which addresses are legitimate for their domain
- Uses DNS record ("v=spf1")
- Domain owner publishes records via DNS to inform world which machine are authorised to use their domain for the `SMTP HELO` and `FROM` field
