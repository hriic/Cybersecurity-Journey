# Modern Web Stacks — Security Notes

> Authorized-lab learning notes. The goal is to understand how modern applications are built, how to recognize their components, and how architecture changes the attack surface.

## 1. What a modern web stack is

A web application is usually not one program. It is a collection of layers that cooperate:

```text
Browser / Client
      |
      | HTTP/HTTPS
      v
Reverse Proxy / Web Server
      |
      v
Application / API
      |
      v
Database / Services
```

A tester should identify each layer because a weakness can exist in the proxy, framework, application logic, API, authentication layer, database integration, deployment configuration, or the trust boundary between components.

## 2. Frontend vs backend

The **frontend** executes mainly in the browser and handles the interface, routing, rendering, forms, and client-side state. Technologies such as React can be visible through JavaScript bundles, HTML structure, source maps, framework artifacts, and browser behavior.

The **backend** receives requests, applies business logic, authenticates users, talks to databases/services, and returns HTML or API responses. Node.js with Express is one example.

Client-side restrictions are not security boundaries. If a button is hidden in the browser but the backend API does not enforce authorization, directly requesting the endpoint may expose the flaw.

## 3. MERN mental model

MERN commonly means:

- **MongoDB** — document-oriented database.
- **Express.js** — web framework running on Node.js.
- **React** — frontend UI library.
- **Node.js** — JavaScript runtime used on the server.

A useful mental model is:

```text
React UI -> HTTP/API -> Express routes -> Node.js logic -> MongoDB
```

During testing I try to determine which component is responsible for a behavior rather than calling the entire application "JavaScript."

## 4. Fingerprinting Express / Node.js

Indicators practiced in labs include:

```bash
curl -I http://TARGET:3000/
```

Possible clues:

- `X-Powered-By: Express`
- cookies such as `connect.sid`
- Express-style errors such as `Cannot GET /nonexistent`
- JSON API routes
- stack traces or development error pages
- filesystem paths mentioning Node application files

No single header proves vulnerability. Fingerprinting narrows the technology hypothesis.

## 5. Routes and APIs

Express applications commonly define routes such as:

```text
GET  /api/user
POST /api/user/update
GET  /api/admin/...
```

For each endpoint I ask:

1. What HTTP method is expected?
2. What input does it accept?
3. Is authentication required?
4. Is authorization checked server-side?
5. Does the response leak implementation details?
6. Does changing JSON properties alter security-sensitive state?

The important lesson is to understand **data flow**: user input enters a route, the application transforms it, then it may reach an object, database query, filesystem operation, template, or operating-system command.

## 6. Sessions and cookies

In labs I practiced saving and reusing session cookies with curl:

```bash
curl -c cookies.txt http://TARGET/...
curl -b cookies.txt http://TARGET/...
```

`-c` stores cookies received from the server. `-b` sends stored cookies back. This helps separate authentication state from the browser and makes request behavior easier to reproduce.

## 7. Prototype Pollution concept

JavaScript objects inherit properties through the prototype chain. Prototype Pollution occurs when attacker-controlled input can modify properties on a shared prototype or otherwise inject unexpected inherited properties.

Security impact depends on how the application later uses those properties. A polluted property is not automatically privilege escalation; exploitation requires a useful **source → pollution primitive → gadget/sink** relationship.

The lab lesson was to inspect object-update functionality carefully, especially endpoints accepting nested JSON. If authorization logic later trusts a property that can be inherited or polluted, application state may behave unexpectedly.

## 8. Next.js / React Server Components observations

Modern Next.js applications can expose framework-specific artifacts in page source and network traffic. In App Router / React Server Components environments I encountered serialized framework data such as `window.__next_f`.

That is a **fingerprint**, not a vulnerability by itself.

Testing workflow:

```text
Identify Next.js
   ↓
Understand routing and middleware
   ↓
Find protected route
   ↓
Observe normal response
   ↓
Research version-specific behavior
   ↓
Validate only in authorized lab
```

## 9. Middleware trust boundaries

Middleware can run before a route and perform authentication, authorization, redirects, logging, or request rewriting.

A key lesson from the Next.js lab was that a protected page can depend heavily on middleware. If framework-internal request metadata is trusted incorrectly, a request may reach a route without the intended middleware decision.

This demonstrates a broader principle:

> Security controls should not blindly trust client-controlled headers or metadata that are intended to be internal.

## 10. Environment variables and debug exposure

Node/Express applications often use environment variables for configuration:

```text
NODE_ENV
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
API_KEY
```

Environment variables are normal. The vulnerability is **exposure**, for example a debug endpoint returning configuration to an unauthenticated user.

In a lab, verbose behavior revealed details such as PostgreSQL port `5432`, application paths like `/opt/nodeapp/app.js`, SQL/database information, and development configuration. These details reduce uncertainty for an attacker.

## 11. Static files and configuration

Express can expose static directories with middleware such as `express.static()`. Static content deserves enumeration because JavaScript/configuration files can reveal:

- API paths
- internal hostnames
- feature flags
- development settings
- comments
- client-side keys or identifiers

A file being under a static path does not mean all filesystem files are accessible. Dotfiles such as `.env` may be denied even while another configuration file is exposed.

## 12. My modern-stack testing methodology

```text
Fingerprint
   ↓
Identify technology
   ↓
Map routes / APIs
   ↓
Understand authentication and session state
   ↓
Find input and trust boundaries
   ↓
Form a vulnerability hypothesis
   ↓
Test manually
   ↓
Compare responses
   ↓
Document evidence and impact
```

The main improvement in my approach is moving from "run tools" to "understand architecture first."