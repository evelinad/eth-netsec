# Topic 2: Identity and Authentication
## Identity & Identity Theft
- Identity: specifies a principle, a unique entity
	- Individuals (e.g. person's name)
	- Physical objects (e.g. computer's network address)
	- Logical objects (e.g. application's name and version)
	- Groups of principals
- Identity theft: crime in which imposter obtains key pieces of personally identifying information to use it for their own gain or do harm
	- e.g. steal Social Security Number
	- Preventions:
		- don't make this personal information available to untrusted people
		- react fast in case one suspects identity theft
		- insure oneself against identity theft
	- Variants:
		- Financial Identity Theft: use of someone else's credit card or other information to obtain goods/services
		- Criminal Identity Theft: posing as another person in case of apprehension for a crime
		- Identity Cloning: use another identity to assume his/her identity in daily life
		- Business/Commercial Identity Theft: using another's business identity (e.g. obtain a credit)

## Authentication
- Authentication: process of verifying the claim of an identity. Binds principle to an identity
- Authentication criteria:
	- Something the entity knows (e.g. password)
	- Something the entity has (e.g. card)
	- Something the entity is (e.g. biometrics)
	- Entity's location (e.g. being in a special room)
	- Entity's ability (e.g. signing)
- Weak authentication: checking only one authentication criteria
- Strong authentication: checking two or more authentication criteria

### Protocols
- OpenID
	- Open standard for decentralized user authentication
	- Allows to use an existing account to sign-in into multiple websites
	- No need to create a new password for each site
	- Password only given to *identity provider*, that provider confirms identity to the other website
- OAuth
	- User (*resource owner*) grants some service (*client*) access to protected data stored on a *resource server* without sharing authentication information with the *client*.
	- Resource owner authenticates directly to resource server's *authorization server*. This authorization server generates an access token to *client*.
- 3D Secure
	- Secures online card transactions
	- Also known as «Verified by Visa» and «MasterCard SecureCode»
	- Original specification
		1. User enters payment card details on merchant's site
		2. 3D Secure pops up (webpage) for user to enter password connected to card
		3. If entered password was correct, user returns to merchant's page
		4. Merchant receives authorisation token to submit to bank
	- Security problems
		- Pop-up blockers might be in the way, `iframe` used nowadays
		- If `iframe` used, SSL indicator in address bar not visible
		- Protocol does not specify how customer is verified. Some banks ask for card's PIN
		- Liability shift: customer becomes liable for losses if card and password are misused
		- Bank uses weak authentication, prone to MitM attacks
		- Not specified how password can be reset, multiple bad implementation (were) used
		- Privacy issues: description of the transaction must be shown, unlike in counter situation

### 802.1x: Port-Based Network Access Control
- Client-Server access control protocol
- Restricts unauthorised devices from connecting to (W)LAN trough publicly accessible ports
- Data Link Layer used for higher-level authentication protocols
- For each 802.1x port switch creates two virtual access points
	- Controlled Port: only open after device has been authorised
	- Uncontrolled Port: provides path **only** for Extensible Authentication Protocol Over LAN (EAPOL) traffic
- Steps:
	1. User activates link (i.e. turns wireless on) and asks to access Access Point
	2. Authentication server sends authentication challenge
	3. User's device responds to challenge
	4. Authentication server checks response and sends result if it's check
	5. Switch opens controlled port if authentication server successfully authenticated device
- System Components
	- 802.1x Supplicant: User or client that wants to be authenticated (e.g. computers, switches, etc.), provides client capability
	- 802.1x Authenticator: Device between supplicant and authentication server (e.g. wireless access point), controls layer 2 resources, mechanisms to grant access
	- 802.1x Authentication Server: authorisation policy (e.g. AAA/Radius/ACS server)
- 802.1x EAP
	- Extensible Authentication Protocol (EAP)
	- Authentication framework with support for multiple authentication methods
	- Runs over data link layer (e.g. Point-to-Point Protocol (PPP), IEEE 802 (LAN), etc.), does not require IP
	- Provides own duplicate elimination and retransmission
	- Authentication Methods
		- EAP-MD5: MD5 hashed username and password
		- EAP-OTP: uses one-time password
		- EAP-TLS: strong PKI authenticated TLS, server and client need valid x.509 certificate
		- EAP-TTLS: sets up encrypted TLS tunnel, this one is used by the authentication method
		- many more
- 802.1x Identity and Security
	- Authentication: who is allowed to access network/services?
	- Authorisation: what is user allowed to do?
	- Access Control: based on authentication and authorisation
	- Policy Enforcement: combines authentication, authorisation and access control to enforce enterprise policies
- Benefits:
	- Standards-based technology
	- Extends authentication to other security areas
	- Uses link layer
	- Operates in wired, wireless and switching scenarios
	- Enables/performs centralised user administration
- Limitations (primary with 802.11)
	- MitM attack
	- Session hijacking

## Anonymization
- Levels of Anonymity
	- Identity: actual principle is known
	- Pseudonymity: principle indirectly known as the holder of a pseudonym, actual principle not disclosed
	- Anonymity: principle is known to be part of a defined set («anonymity set»). However, members can't be distinguished individually. Actual principle not disclosed
- Kinds of pseudonyms
	- Public pseudonyms: link between pseudonym and the human being publicly known/easy to discover
	- Non-public pseudonyms: link known to system operators, but not publicly disclosed
	- Unlinkable pseudonyms: link unknown to system operators, can't be determined
- Onion routing (see InfSec lecture)
- Mixnets
	-  Proxy handles messages in batches
	-  Each batch is permuted
	-  Goal is to unlink incoming and outgoing messages at each proxy
	-  Additional attack defences
		-  Pad message to constant size
		-  Remove replays
		-  Detect and remove tainted packets
		-  add/drop dummy packets
		-  etc.
-  Attacks against anonymity
	-  Traceback: break systems on path. Defend by adding more proxies
	-  Collusion: node owners collaborate to break anonymity. Defend by adding reputation system
	-  Traffic analysis
		-  Tagging by creating bit errors
		-  Replay messages
		-  Look for corresponding features in the outgoing stream
		-  Defenses: hearbeats, traffic shaping, padding, etc.
	-  Logging
		-  Paths *must* be reset periodically
		-  In case of onion routing first and last machine on path needed
		-  In case of mixnet, all machines on path required
