- Never store passwords in plain text
- Always store a hashed version using a cryptographic hashing algorithm for passwords
- Use bcrypt or Argon2
- The algorithms are slow and computationally expensive, deliberately, which protects against brute force attacks, even if the hash database is leaked.

- Also remember to salt:
	- You generate a unique salt for each password
	- A salt is a random string added to the password before hashing
	- This ensure that two identical passwords will have different hashes
	- This part thwarts precomputed dictionary or rainbow table attacks

- Rainbow tables are pre-computed databases of hashes for millions of common passwords.

- You also need secure key management:
	- Salts should be stored with the hash
	- Because when a user inputs their password, the salt is concatenated with it and then checked to see if it matches the original password hash

- You should also enforce strong password policies (length, special characters, numbers)
- Implement rate limiting and account lockout mechanisms
- Consider multi-factor authentication, especially for sensitive accounts such as brokerages, government.