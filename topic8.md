# Topic 8: Malware

## Basics
- Trojan
	- Also known as spyware, backdoor software
	- Seems to offer useful functionality
	- Contains hidden functionality, executes in the name and with the rights of the user to undermine system security
-  Worm
	-  Self-contained software that replicates itself from computer to computer
	-  On arrival, worm may perform unwanted operations and continues propagation
-  Bot
	- Part of a Botnet
	- Runs on compromised machine
	- Executes operations received from Bot Master, the operator of the Botnet
	- Listens to a command and control (C&C) center
	- Botnet consists of communicating bots
- Rootkit: activates during boot, hides from operating system
- Keylogger: records key presses to steal information
- Dialer: misuses internet dial-up modems
- Ransomware: encrypts data, extorts money for decryption key
- Peripherals might be infected too
- Primary impact on network caused by scanning activity of worm
	- High ARP activity
	- High ICMP activity (destination unreachable messages)

## Detection
- Antivirus software can be deployed on an endpoint: scans files, processes and settings
- Can also be deployed on the network, scans web traffic (e.g. web proxy), scans mails
- Signature-based detection
	- Works for known malware
	- Database keeps growing
	- Signatures can cope with simple polymorphisms
- Behavior-based detection
	- Need some kind of model what is "normal" for the system

## Advanced Persistent Threat (APT)
- Sophisticated stealth customised attack on selected high value targets
- Attacker uses sophisticated attack techniques (e.g. zero day vulnerabilities, social engineering, highly targeted deployment, etc.)
- Attacker can launch attack anytime and hide it well
- Attackers have very specific objective and are well trained, motivated, organised and funded

## Antivirus Detection Evasion Techniques
- Polymorphism: Every replication is a mutation of the original version
- Code obfuscation: Disguise purpose, make analysis hard
- Encrypt code and messages
- Code changes to prevent disassembly
- Virtual Machine / Sandbox detection to make analysis harder
- Bootstrapping/multi stage worms

## Worm Propagation Mechanism
- Typical steps
	1. Select a target host
	2. Contact target host
	3. Check if target has known vulnerabilities
	4. Exploit vulnerability
	5. Copy worm on target
	6. Start copy on target (maybe user interaction needed)
	7. Repeat

## Countermeasures
- Endsystem: keep up-to-date, use firewalls, IPS, Antivirus, etc.
- Educate users
- Monocultures are targeted first by attackers
- Local Network: use OPS systems, restrictive firewalls, etc.
- Internet: Backbone filters and monitoring of anomalies
- Application Whitelisting: only whitelisted software allowed to run
