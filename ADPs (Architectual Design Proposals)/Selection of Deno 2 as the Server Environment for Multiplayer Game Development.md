# Architecural Design Proposal #1

**Title:** Selection of Deno 2 as the Server Environment for Multiplayer Game Development

**Decision:** Adopt Deno 2 as the server environment for our multiplayer game in libGDX, favoring it over Spring Boot and Node.js.

**Status:** Proposed

**Context:** Our multiplayer game will support a limited number of clients (approximately a dozen) and does not require the complexity of an enterprise-level framework. The server environment should be optimized for performance, security, and simplicity, avoiding unnecessary overhead and complexity.

**Decision Drivers:**

- **Performance and Security:** Deno 2 provides better performance than Node.js, benefiting from a more modern runtime optimized for speed and resource efficiency. It also offers enhanced security features, including sandboxing and secure-by-default permissions.

- **Node Module Compatibility:** With Deno 2’s support for Node modules, we can leverage the Node ecosystem, including popular real-time libraries such as Socket.io, without abandoning Deno’s environment.

- **Simplicity and Minimal Overhead:** Compared to Spring Boot, Deno has less setup and runtime overhead, making it ideal for small-scale applications where simplicity and quick deployment are priorities. Spring Boot’s enterprise-oriented capabilities are not necessary for our project scope.

**Consequences:**

- **Positive:** 
  - Simplified server setup
  - High performance
  - Reduced security risks
  - Access to Node.js libraries.

- **Trade-offs:** 
  - Less mature ecosystem compared to Node.js or Spring Boot
  - Potential limitations with certain advanced or enterprise-scale functionalities.

**Decision Owner:** Max, Jean-Luc, Daniel