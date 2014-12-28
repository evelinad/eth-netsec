# Topic 3: Firewalls, NAT, IDS/IPS

## Firewalls
- Hardware or software configured to permit, deny or proxy data through a computer network
- Configuration called a *policy*
- Types
	- Simple packet filter
		- Examines a packet at network layer
		- Decision based on network layer header
		- Application independent
		- Provides good performance and scalability
		- Downside: no state or application context for more advanced filtering
	- Stateful filter
		- Kepps track of the state of network connections
		- Decision based on session state
		- Allows more powerful rules
		- Downsides: UDP has no state, may result in inconsistent states between firewall and host, state explosion
	- Application layer/proxy based
		- Takes application state into security decision
		- Aware of application
		- Downsides: Needs to support many application protocols, performance and scalability
- Usually in between two networks with different levels of trust
- Ingress: filter incoming traffic
- Egress: filter outgoing traffic
- Default policy must be specified, either accept or deny
- Kinds to deny access
	- Drop: silently drop packet
	- Reject: drop packet and inform sender (ICMP)
- Stateless Firewall: Packet Filter
- Special case of application based firewalls: Web Application Firewall (WAF)
	- Protects web-based application by filtering malicious requests
	- Filters requests based on patterns, static/dynamic blacklisting/whitelisting
	- Problem with false positives
- Attack/Bypass Techniques
	- IP source address spoofing
	- Artificial fragmentation: port number only in first fragment, without reassembly attack gets through
	- Vulnerabilities in firewall
	- DoS (e.g. state explosion)
	- Tunneling/Covert channels (e.g. use ICMP packets or DNS requests)
- Firewall detection
	- Pot scanning
		- use traceroute to find firewall's IP address
		- Analyse response of port scan
		- Modify Time to Live (TTL)
			- Set TTL to expire one hop after firewall
			- Packet passed by firewall will result in a TTL expired notification (ICMP packat)
		- Trying to hide existence of firewall is OK, but not a security technique
- Linux `netfilter`/`iptables`
	- `netfilter`: Linux packet filter
	- `iptables`: user-mode tool for configuring `netfilter`
	- Able to do deep packet inspection, be used stateful, NAT
	- Contains *chains* linked to *tables* containing *rules*
	- What firewall does if packet matches a rule
		- ACCEPT: accept packet
		- DROP: drop packet
		- QUEUE: hand packet off to user-space process
		- RETURN: stop processing in current chain and resume in previous chain
		- MASQUERADE: only in NAT-table, rewrite source or destination address
- Organisational challenges
	- Large rulesets: complex and grow over time
	- Rulesets hard to manage and understand
	- Tools needed to manage hundreds of firewalls securely in a larger organisation
	- Needs a process to allow rule changes
	- Conflicting goals: networking statt vs. security statt, former try to provide connectivity without disruption while latter try to secure network and disrupt connectivity

## NAT
- "Network Address Translation"
- Multiple hosts on private network access Internet using a single public IP address
- Rewrites source and/or destination address of IP packets while they pass through router/firewall doing NAT
- Benefits
	- Malicious activity by outside hosts is prevented to some extent
	- Saves address space
- Drawbacks
	- No true end-to-end connectivity
	- Protocols might be disrupted by NAT (e.g. IPsec, SIP, FTP, etc.)
- Session
	- Session endpoint: a pair of IP address and port number
	- Uniquely identified by its two session endpoints
	- Direction of a session normally the direction of the packet that initiated the session (e.g. initial `TCP SYN` packet)
- NAT Modes
	1. Basic NAT
		- Translates IP addresses only, port numbers are not touched
		- One public IP for each internal host needed
	2. Network Address and Port Translation (NAPT)
		- Translates IP addresses and port numbers
		- Many internal hosts can share one public IP address
- NAT UDP Hole Punching
	1. Both clients can send packets to a rendezvous server, have active UDP session with rendezvous server
	2. Rendezvous server records clients' private and public session endpoints
	3. First client asks rendezvous server to help to establish connection to second client
	4. Rendezvous server replies to first client with second client's public and private endpoints
	5. Rendezvous server sends second client a connection request with first client's endpoints
	6. Both clients have now each others endpoints
	7. Both clients send each other UDB packets to the other side's endpoint
	8. First UDP packets sent by clients punch a hole in their own NAT, but will be blocked by the NAT of the receiving client (depending on timing, one of the packets may even already go through the other side's NAT)
	9. Next UDP packets will go through and reach the other side

## Intrusion Detection and Prevention System (IDS/IPS)
- Firewalls are not enough
- Firewalls allow traffic in to legitimate destinations
- Users might download malicious files
- Some kind of protection inside the perimeter required
- Intrusion Detection:
	- Try to detect intrusions on the network
	- Traffic compared against database / known attack signatures
- Intrusion Prevention:
	- Block traffic after detection
- Both systems output alerts, action (e.g. block traffic), reporting, analysis, etc.
- Classification of IDS/IPS
	- Observed object:
		- Packet: analyse packet header/content
		- Flow: analyse flow parameters (IP address, ports, # of packets, # of bytes, timing parameters, ...) of multiple packets
	- Observed point:
		- Host: software running on host
		- Network: data collectors attached to network
	- Observation method:
		- Signature: compare observed data against database containing known malicious signatures
		- Behaviour: detect deviation from normal state
- Flow based analysis
	- (pro) Scales well for high link speeds
	- (pro) In case of encrypted traffic, payload matters less
	- (con) Information loss due to aggregation
	- (con) No content-based analysis possible
- Host based IDS
	- (con) hard to deploy, software on each host needed
	- (con) one sensor per monitored machine needed
	- (pro) because of additional information (e.g. OS, Applications) less false positives
- Network based IDS
	- (pro) easy to deploy
	- (pro) one sensor monitors multiple machines
	- (con) less information available resulting in more false positives
	- (con) performance issues because of more data measured
- Signature based IDS
	- (pro) precision due to reference database results in less false positives
	- (con) unknown attacks can't be detected resulting in more false negatives
	- open question if signatures can be generated automatically
- Behaviour based IDS
	- (pos) allows to detect unknown attacks by detecting anomalies
	- (con) hard to establish what is "normal" («ground truth»)
	- more false positives, maybe less false negatives
	- open area of research
- Operational challenges
	- handle false negatives and false positives
	- cope with network speed
	- managing sensors and signature databases
	- policy management
	- define process for intervention
	- 24/7 monitoring needs people
	- complex business, often outsourced in some way
- IDS Evasion Techniques
	- DoS: flooding, resource exhaustion (e.g. algorithmic complexity)
	- Use DoS to hide other malicious activity

