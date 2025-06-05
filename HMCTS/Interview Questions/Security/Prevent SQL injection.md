In short:
- Parameterise everything
- Never trust user input
- Minimise blast radius

- You have to control how the user input interacts with your database layer.
- The most robust and widely accepted method is parameterised queries or prepared statements.
- Don't concatenate strings to build queries.
- Defind SQL structure in advance
- Then pass user inputs as parameters.

- Most modern libraries and ORMs (e.g. JPA/Hibernate)
- This means that the SQL is in place but with placeholders for user input

Without parameterised queries, if the user inputs:

```sql
'  OR 1=1 --
```

For a vulnerable query such as:

```sql
SELECT * FROM users WHERE username = 'USER_INPUT' AND password = 'PASSWORD_INPUT';
```

With the user input it becomes:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = 'password123'
```

The database sees this as:
- Find rows where `username` is an empty string (`''`)
- OR
- `1=1` (which is always true)
- The rest of the line is commented out including `AND password = 'password123'`

Thus, this effectively becomes:

```sql
SELECT * FROM users
```

You can avoid this using parameterised queries (prepared statements), because if the user inputs `' OR 1=1 --`, it's treated as a literal string value for the `username` parameter and the database will look for a username that exactly matches ` OR 1=1 --`, instead of prematurely closing the string in the original SQL (`... username = ''`).

- Another key practice is input validation and sanitisation
- For example check that an email is a valid email, that numbers are actually numbers etc.
- You can do this on the front and backend for multiple layers of defence.

- Use least privilege access at the database level because then the SQL injector won't even have the rights to access certain information.

- Static analysis tools and automated tests can catch unsafe query patterns early in development, and web application firewalls can add an extra layer of protection.