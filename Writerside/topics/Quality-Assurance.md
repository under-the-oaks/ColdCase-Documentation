# Tseting Strategy for Coldcase

Our test strategy for **Coldcase** is designed to ensure a robust, maintainable product. We combine unit tests, integration tests, and manual testing with quality gates that help us maintain high standards throughout development.

## Testing the Game Client

The client is built with LibGDX, and we use JUnit to run tests in a headless environment. Our unit tests are designed to mimic real-world use cases (a grey-box approach) such as user-interactions (e.g. moving a block), ensuring that the components work correctly while isolating dependencies with mocks. Although achieving high coverage is important, our tests are primarily focused on verifying practical scenarios.

## Testing the Game Server

The game server is developed in Deno, and our testing strategy there mirrors the client's approach, focusing on unit tests that reflect use cases rather than solely internal implementation details. For example, our server tests include checks for core functionality such as lobby management and the handling of websocket connections. We utilize Deno's standard asserts to verify their correct behavior. This approach not only validates individual functionalities but also serves as a grey-box test to ensure that the server behaves as expected from a use-case perspective.

## Integration and System Testing

Integration testing is performed by verifying that the interactions between the client and server are seamless. We conduct these tests by running the game and playing through the deployed levels. Independent testers, unfamiliar with the backend details, are engaged to provide unbiased feedback, ensuring that the game experience is smooth and bug-free.

## Quality Gates

We maintain high quality by combining automated testing, code reviews, and documentation:

- Well-documented code: We keep our documentation up to date, using JavaDoc and detailed explanations for complex logic.
- Continuous Integration (CI): Every time new code is pushed to the main branch, our CI pipelines run all available tests and check code coverage.
- Code reviews: Every pull request is reviewed by at least one developer who wasn't involved in the changes. For updates to the main branch, we require three reviews to catch potential issues before they go live.

## Future Directions

We're working on expanding our integration tests to automate more real-world gameplay scenarios, reducing the need for manual testing. As we continue improving Coldcase, we'll keep refining our testing strategy based on feedback and new tools to ensure the game remains reliable and scalable.