# Topic 1: (In)security, Risk and Lifecycle of Vulnerabilities
## (In)security Landscape
- Complexity huge problem
- System's security is equal to the weakest link
- Complexity increases quickly by adding features, extensibility, dependencies, etc.
- Features are seldom sacrificed to increase security

## Vulnerability Lifecycle
- Security vulnerability: a weakness in a system allowing an attacker to violate the confidentiality, integrity, availability of the system, data or applications it hosts
- There are cases where a vulnerability is considered a feature
- Common Vulnerability Exposures (CVE)
	- tries to standardise the name for publicly known vulnerabilities and security exposures
	- Format: CVE-[YEAR]-[DIGITS]
- Vulnerability Lifecycle
	1. creation: vulnerability is created
	2. discovery: vulnerability is discovered
	3. exploit: exploit for vulnerability is created
	4. disclosure: vulnerability is made public, this is also called the «zero-day». From now on the vulnerability is considered to be known to everyone
	5. patch available: vendor creates a patch to remove vulnerability
	6. patch installed: patches are installed
- Exposure Risks
	- Pre-disclosure risk: time from discovery to disclosure, only small group of people is aware of vulnerability
	- Post-disclosure risk: time from disclosure to patch available. Users wait for the patch being available while vulnerability is publicly known
	- Post-patch risk: time from patch available to patch installation
- Zero-day exploits & vulnerabilities:
	- Zero-day: the day vulnerability goes public
	- Zero-day exploit: Attack exploiting previously unknown vulnerability

## Risk Management
- Security is a trade-off
- Basic methodology
	1. Collect assets we try to protect
	2. What are the risks to those assets?
	3. Evaluate how well security solutions in place mitigate those risks
	4. Search for risks introduced by security mechanisms
	5. What costs and trade-offs does the security solution impose?
	6. Is the trade-off worth it?
- Options in regard to risks
	- avoid risk by giving something up
	- decrease risk trough technology, procedures, etc.
	- transfer risk, e.g. buy insurance
	- accept risk and live with it
- Security measurements must make sense in context of business goals
