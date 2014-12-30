# Topic 4: Domain Name System (DNS)

## Basics
- Client-Server application
- Hierarchical system
	- Domain name consists of labels
	- Hierarchy of domain descends from right to left
	- Delegation: servers can tell which server knows more
- Answers can be cached to improve performance
- Terminology
	- Zone: collection of hostnames/IP address pairs, managed together
	- Name Server: server answering DNS queries
	- Authoritative Name Server: name server knowing the answer directly, no need to ask other name servers. Responsible for the zone
	- Resolver: client part of DNS system, resolves domains
	- Recursive Name Server: name server + resolver; name server that asks other name servers to resolve if it is not responsible for the requested zone
	- Sub Resolver: forwards DNS request to a recursive name server, typically used by end-hosts
- Caching
	- Reduces lookup overhead
	- Balances consistency and efficiency
	- Time To Live (TTL) property controls caching
- DNS Hierarchy
	- Root Name Servers: are at the top, controlled by IANA
		- hard-coded in a file
		- globally distributed for fast lookups
	- Top Level Domain Name Server: managed by selected organisations (IANA decides)
	- Authoritative Name Servers: managed by private entities
- Top Level Domain Classes:
	- gTLD: generic top level domain (e.g. `.com`, `.net`, etc.)
	- ccTLD: country code top level domain (e.g. `.ch`, `.de`, etc.)
- Distributed System
	- DNS server answers with one of three responses
		- with the correct answer
		- informs that server won't answer
		- doesn't know the answer, but returns an alternative DNS server to ask (delegation)

## Attacks
- DoS
	- DDoS against root name servers is hard
		- 13 root name servers are in fact 13 clusters
		- uses anycast towards more than 300 servers
		- overprovisioned
- Account takeover (registrars/resellers)
	- because of weak passwords, social engineering or vulnerabilities inside the web frontend
- Manipulation of local DNS settings (e.g. local config, DHCP)
	- try to redirect all requests to malicious DNS server
	- possible attack vectors
		- manipulate local host config (e.g. `/etc/hosts`)
		- manipulate router, NAT, modem, etc.
		- spoof DHCP replies
- Manipulate DNS lookup process (e.g. spoofing DNS, cache poisoning)
	- inject incorrect resolution information by malicious host
	- basic attack vector: answer faster than correct server
- Bailiwick Attack
	- Client requests DNS record for `example.org` and receives an answer with `example.org` related information *and* for example an entry for `somethingelse.net`. Client's resolver would ignore the additional entry about the domain that was not requested. Assumes an attack.
- Authentication of queries is weak
	- only 16-bit query transaction ID (TXID)
	- query string part of response
	- port
	- because first good answer wins attacker can fake above properties and response faster than the "good" server
	- easier if we sniff packets (e.g. read out TXID)
- Cache poisoning
	- insert invalid entry into cache of some intermediate DNS server
	- this intermediate DNS server returns malicious value

### Kaminsky Attack
1. Client browses webserver under attacker's control
2. Returned webpage contains content for some domain (e.g. `123.somebank.com`)
3. Browser tries to load the content, sends first a DNS query for URL (e.g. `http://123.somebank.com/someFakeImage.jpg`)
4. Attacker floods client with spoofed DNS responses containing the real domain (e.g. `www.somebank.com`) with an IP address that is under control of the attacker
5. Next time client (if cache is active and entry still valid) browses `www.somebank.com` will end up on attacker's server

## Domain Name System Security Extensions (DNSSEC)
- Provides...
	- Authenticity: validates received data came from correct source
	- Integrity: validates received data is not manipulated
	- Backward compatibility: integrates with existing server infrastructure and user clients
- Doesn't provide confidentiality and no protection against DoS
- All records are signed (asymmetric crypto), key pair for each zone
- Uses chain of trust, root zone's keys used as anchor/root of trust
- Supports multiple algorithms in case attacks for existing ones are found, new ones can be deployed
- New record types, some examples:
	- RRSIG: digital signature of the answered resource record set
	- DNSKEY: stores public key
	- DS: delegation signer, glues the trust chain, used by higher level entries

## Guest Lecture
- Browsers / operating systems trust lots root CA certificates
- Each of them can issue certificate for any domain
- Each of them can also issue certificates that allow the owner to issue certificates for any domain
- So we get hundreds of equally trusted parties
- Several levels of verification done by CAs
	- Domain Validation (DV)
	- Extended Validation (EV)
	- And levels in between ("Premium", "High assurance", etc.)
- Usually large window between incident discovery by CA and response
- Incidents often discovered not right away because no incident detection mechanisms in place
- No automated incident reporting mechanisms installed (e.g. if pinned certificated does not match the one provided by server no reporting done by browser)
- Basic idea is to minimise window of opportunity for attacker
	- Makes it more expensive for the attacker to run attacks
	- Completely preventing attacks not possible
- Owner of domain knows which certificate is the right one
- Extend user's browser to report incidents, not everyone has to participate, interested in a large number

### Certificate Transparency
- All publicly accessible SSL/TLS endpoints' certificates become public knowledge
- Hols CAs publicly accountable
- Doesn't add additional entity we have to trust, more of an observer
- Certificate Transparency Log
	- Append-only log
	- Lists certificates
	- Server verifies certificate chain
	- Periodically new certificates are added and signed
	- Log is publicly available
	- Can't say if a certificate is "good" or "bad", just informs about the presence of it
	- Merkle Tree used as data structure

