- Security principles should be integrated throughout the software development life cycle.
- It shouldn't be an afterthought - security by design

- Start with the requirements and design stage
- Conduct threat modelling using a framework like STRIDE to identify potential attacj vectors before any code is written.
- For example, if you're building an authentication system, you map out how it might be abused or bypassed.

|Threat|Desired property|Threat Definition|
|---|---|---|
|Spoofing|[Authenticity](https://en.wikipedia.org/wiki/Message_authentication "Message authentication")|Pretending to be something or someone other than yourself|
|Tampering|[Integrity](https://en.wikipedia.org/wiki/Data_integrity "Data integrity")|Modifying something on disk, network, memory, or elsewhere|
|Repudiation|[Non-repudiability](https://en.wikipedia.org/wiki/Non-repudiation "Non-repudiation")|Claiming that you didn't do something or were not responsible; can be honest or false|
|Information disclosure|[Confidentiality](https://en.wikipedia.org/wiki/Confidentiality "Confidentiality")|Someone obtaining information they are not authorized to access|
|Denial of service|[Availability](https://en.wikipedia.org/wiki/Availability "Availability")|Exhausting resources needed to provide service|
|Elevation of privilege|[Authorization](https://en.wikipedia.org/wiki/Authorization "Authorization")|Allowing someone to do something they are not authorized to do|
- In development adopt the principle of least privilege for code and infrastructure.
- Avoid dynamics SQL, raw `eval`, unescaped HTML rendering.
- Use secure and up-to-date libraries that handle encryption, authentication, and output encoding.
- Incorporate testing into your CI/CD pipeline
- Static Application Security Testing and Dynamic Analysis
- Dependency scanning has help check for vulnerable packages (OWASP Dependency-Check or GitHub Dependabot which automatically updates vulnerable packages)

- Testing should include security unit tests and fuzzing (invalid, unexpected, or random input)
- Post-deployment you need logging, monitoring, and alerting

- Invest in a DevSecOps mindset, where devs, ops, and security work collaboratively.
- Security is a set of shared responsibilities
- Automate as much of it as possible

- In short:
	- Bake security into every step, automate where possible, and train your team to see security not as a blocker, but as a baseline of quality.
