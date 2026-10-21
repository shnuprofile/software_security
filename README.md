# Practices for Secure Software Report – Artemis Financial
## Client Summary
## Client: Artemis Financial. The company handles a large amount of personally identifiable information, PII, so protecting that data is critical.

### Software requirements and the issue to address:
Adopt defense in depth as core security approach.
User identity verified on the frontend with multifactor authentication; backend enforces authorization based on least privilege so each user can only access what is needed for their tasks.
Data must be encrypted both in transit and at rest at the database level.
Recommended cipher: Advanced Encryption Standard, AES, with AES-256 paired with SHA-256 for data verification. Keys must be generated securely and managed tightly; no hard-coded secrets.

## What I did well finding vulnerabilities and why security matters
I followed a structured secure-software process and documented findings in the Practices for Secure Software Report. Focusing on defense in depth, least privilege, and proper crypto use helped identify where data protection and transport security were weak. Coding securely is important because insecure code exposes PII, enables unauthorized access, and can lead to data loss, regulatory penalties, and reputational damage. Software security adds value by preserving confidentiality, integrity, and availability of customer data, reducing risk, building trust, and supporting business continuity.

## Vulnerability assessment – challenging / helpful
Helpful parts were applying industry-standard controls: using standard cryptographic APIs instead of custom implementations, externalizing SSL settings to application.properties, and verifying checksums with SHA-256. The assessment was challenging in ensuring key management practices were clearly defined and that no secrets were hard-coded while keeping configuration external to source.

## Increasing layers of security and future assessment approach
### Layers added:
ServerController updated to compute a SHA-256 digest of a unique data string using the MessageDigest API. The /hash endpoint now returns the "Hello World Check Sum!" data string and its cryptographic checksum.
SSL enabled for the Tomcat server via application.properties pointing to a self-signed keystore with alias "artemis" so the service runs over HTTPS on port 8443.
Transport security enforced with HTTPS and a keystore; cryptographic controls applied via standard SHA-256 implementation; all configurations kept external of the source directory; no hard-coded secrets.
No new third-party libraries were added, so dependency risk remained unchanged.
In the future I would use OWASP ASVS and OWASP Dependency-Check for systematic vulnerability assessment, plus SAST/DAST tools and a risk-based threat model to decide mitigation techniques, with regular dependency scanning and secure coding reviews.

## Functional and security validation after refactoring
Functional testing confirmed the refactored code executed without errors and the /hash endpoint returned the expected checksum. Secondary testing included a dependency-check report. No new dependencies were added, so findings remained the same as baseline and the refactoring did not introduce additional security vulnerabilities. Industry standard best practices were followed: SSL settings externalized, standard crypto APIs used, configuration separated from code.

## Resources, tools, coding practices
Resources referenced for AES and SHA-256 guidance. Tools/practices used: FastAPI-style rate limiting sketch concepts, Python logging with deferred formatting, MessageDigest API for SHA-256, Tomcat keystore/HTTPS configuration, application.properties externalization, dependency-check reporting, defense in depth, least privilege, MFA on frontend, encryption in transit and at rest.
