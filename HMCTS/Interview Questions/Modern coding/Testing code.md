- I treat it as a core part of development, not an afterthought.
- I use TDD, writing a failing test, then I write the minimum amount of code needed to pass the test, and then continue to cycle.

- Testing pyramid approach - largest number of tests (at the base of the pyramid) are unit tests which cover the logic of individual components.
- Unit tests are quick to write, precise, fast to run, and help catch regressions early. 
- I use JUnit for Java unit tests, with mockito for mocking. Vitest for JavaScript.

- Next I use integration tests to ensure for example that a controller interacts correctly with a service and a database.
- I use mocks and test containers to isolate side effects whilst still validating the system's structure.

- At the top are end-to-end tests which I use for web applications. If there is a user-facing system, I'd use Cypress or Playwright to simulate a real user's journey through the system, testing it from the UI down to the backend.

- I automate these tests in CI pipeline, so every pull request runs the full suite. 
- When a bug crops up, I write a failing test that replicates the issue before fixing it so that the test can guard against regressions in the future.
- Over time the testing suite builds confidence in the codebase.