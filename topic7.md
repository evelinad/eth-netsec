# Topic 7: Web Application Security

## Basics
- Web Applications Anatomy
	- On Client
		- Browser
		- Plug-In's (e.g. Flash, PDF Reader, etc.)
		- Java (e.g. Applets)
		- ActiveX
		- ...
	- Inside DMZ
		- Web Server
		- Server Side Includes
		- CGI, Scripts, Servlets
		- Programs,
		- Plug-In's
		- ...
	- Internal
		- Database Server
		- Business Services
		- Messaging
		- ...
- Threat Vectors
	- Client
		- XSS
		- Phishing
		- Pharming
		- Malware
		- ...
	- DMZ
		- XSS
		- Input Validation
		- Error Messages
		- Google Dorks
		- Response Splitting
		- Authentication
		- Session Management
	- Internal
		- SQL Injection
		- Second Order Code Injection
		- Business Logic

## HTTP and Session Management
- HTTP is a stateless protocol
- State can be added by web application
	- Create session ID on first connection
	- Client stores session ID locally
	- Client sends session ID each time
	- Server identifies client by session ID
- Transfer of session ID
	- HTTP GET Requests
		- (pro) easy to implement
		- (pro) works with cookies disabled
		- (con) session ID part of Browser's history
		- (con) session ID might be logged on intermediate devices (firewalls, proxy, etc.)
		- (con) HTTP REFERER header field reveals session ID to other web servers
	- HTTP POST Request
		- (pro) less obvious than HTTP GET
		- (pro) works with cookies disabled
		- (pro) client can safely store/transmit without revealing session information
		- (con) field seems hidden, but isn't in reality
		- (con) prone to coding errors
	- Cookies
		- (pro) can be restricted to HTTPS only
		- (pro) restricted to domain and path, has an expiration date
		- (con) persistent cookies exist as files on client's system (e.g. might be stolen in internet café setting)
		- (con) users might disable cookies
- Session ID generation
	- Session ID should be hard to guess, even if whole system is known
	- Don't use predictable values (time, date, process ID, client IP) or a hash of them
- Session ID revocation
	- Should be possible to remove session on client and server side
	- Session should be time limited in general (session expires)
	- Server should terminate session after a period of inactivity, in case of misuse or any kind of attack is detected, after an error and when user leaves secure part of the application
	- Client should be able to invalidate its session, so provide a logout mechanism

## Code Injection
- Command/Shell injection targets operating system
- XSS targets browser
- SQL injection targets database

## Cross-Site Scripting

### Same Origin Policy
- Script can only access content and properties (e.g. cookie, DOM object, etc.) of document loaded from the same origin as the document containing the script
- Same origin means:
	- same protocol
	- same hostname
	- same port
- If a document loads a script from another origin, this script can access the document it was loaded by
- AJAX requests to other URL than the origin of the script not allowed. However target of request can allow requests from other domain by providing specific HTTP HEADER information as answer to a HTTP OPTIONS request by the client

### Cross-Site Scripting (XSS)
- Happens if web application takes input and sends it to browser without validating/encoding it
- Used to inject code into web page
- Injected code executed inside victim's browser
- Can hijack user's session, deface web site, introduce worms, etc.
- Targets the user, not the system itself
