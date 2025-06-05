- For both transit and rest, you need a layered, end-to-end security approach
- Take into account cryptographic standard, access controls, and key management

Transit:
- For data in transit, always enforce TLS (Transport Layer Security)
- This ensures encryption between clients and servers
- Prevents man-in-the-middle attacks
- Redirect all HTTP traffic to use HTTPS and use HSTS headers to enforce it
- Validate certificates properly and use truster certificate authorities

- In API communications, especially between microservices, use mutual TLS (mTLS) to authenticate both parties
- Signed JWTs or OAuth2 tokens can help ensure secure ,authenticated requests

Rest:
- Encrypt all sensitive information using an algorithm like AES-256
- Cloud providers offer built-in encryption for storage solutions like S3 (AWS Cloud Object Storage)

- Never hard-code keys in the source code
- Use key management services like AWS KMS, Azure Key Vault, HashiCorp Vault
- I've often just used an .env file

- Consider database-level encryption, field-level encryption for especially sensitive values (like card numbers)
- Use auditing and logging to detect unauthorised access attemps

In short:
- Use TLS for all communications, encrypt sensitive data with strong ciphers, manage keys securely, and apply principle of least privilege at every layer.