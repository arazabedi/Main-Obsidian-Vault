- Intuitive, predictable, and easy for clients to consume.
- Should follow resource-oriented design (endpoints represent nouns like `/cases` or `/users`).
- HTTP methods define the action carried out on the resource:
	- `GET` - Retrieve
	- `POST` - Create
	- `PUT` - Update (completely replaces an entire resource with the data provided in the request body - this is an idempotent request meaning identical requests result in identical resource state)
	- `PATCH` - Update (applies a partial modification to an existing resource - not inherently idempotent)
	- `DELETE`- Remove
- Consistency is key - use uniform naming conventions, correct status codes for each outcome (e.g. `200 OK`, `201 Created, `400 Bad request, `404 Not Found`) signalling what's happened to the client without ambiguity.
- A good REST API should also support filtering, sorting, and pagination in a standardised way - usually through query parameters like `?status=open&sort=created_at`.
- It should return structure, predictable JSON.

- Security and reliability are also vital - authentication and authorisation via OAuth2 or JWT, rate limiting, graceful error handling with meaningful error messages in a standard format.

- Good documentation matters - you can use Swagger/OpenAP to describe endpoints and schemas.
- A great REST API should feel boring in a good way, behaving exactly as expected with no surprises