# Express.js & Node.js — Enumeration and Security Notes

## Node.js vs Express

**Node.js** is a JavaScript runtime. **Express.js** is a web framework commonly used on top of Node.js to define routes, middleware, APIs, static content, sessions, and application behavior.

Express is therefore not a web server product in the same sense as Nginx or IIS. An architecture may look like:

```text
Internet -> Nginx -> Express/Node.js -> PostgreSQL
```

## Fingerprinting

I practiced starting with headers:

```bash
curl -sI http://TARGET:3000/
```

Indicators may include `X-Powered-By: Express`, a `connect.sid` cookie, characteristic error responses, or JSON API behavior.

A deliberate invalid path can also help understand error handling:

```text
/nonexistent
```

Development responses may reveal framework names, code locations, stack traces, database errors, or internal paths.

## Middleware

Express middleware sits in the request-processing chain. Middleware may:

- parse JSON/body data;
- manage sessions;
- authenticate users;
- authorize routes;
- log requests;
- serve static files;
- modify requests/responses;
- handle errors.

Order matters. A security check that runs after a sensitive handler, or does not apply to every route, can create inconsistent protection.

## Routes

Routes connect an HTTP method and path to application logic.

Conceptually:

```javascript
app.get('/api/user', handler)
app.post('/api/user/update', handler)
```

During enumeration I care about both the path and the method. A route can exist for POST while GET returns `Cannot GET ...`.

## Verbose errors

Development-mode errors are useful to developers but dangerous when exposed publicly. They can disclose:

- source file paths;
- framework internals;
- SQL statements;
- database technology;
- database host/port;
- function names;
- stack traces.

The security lesson is **information disclosure compounds**. One leak may appear low impact, but several leaks can map the backend.

## Debug endpoints

A route such as `/api/debug/env` is high-interest because environment configuration can contain credentials or architecture details.

Defensive rule: debug endpoints should not expose secrets in production and should be removed or strongly access-controlled.

## Static content

With `express.static()`, an application can publish files from a directory. I learned to inspect JavaScript/config files under static paths because they may reveal API endpoints or configuration.

Important distinction:

```text
static directory exposed ≠ arbitrary filesystem exposed
```

## Sessions

Express applications may use server-side sessions represented by a cookie such as `connect.sid`. The cookie identifies state; it should be protected against theft, fixation, insecure transport, and weak server-side authorization.

For lab reproduction:

```bash
curl -c cookies.txt ...
curl -b cookies.txt ...
```

## Database clues

An Express error that mentions PostgreSQL, SQL, or port `5432` tells me something about the backend architecture. It does not mean the database is directly reachable from my machine.

I distinguish:

- application can reach database;
- database port is listening locally;
- database is exposed on the network;
- credentials are known;
- authentication succeeds.

These are separate facts and should never be assumed from one error message.

## Security checklist

When I identify Express/Node.js, I investigate:

- headers and error handling;
- route/API enumeration;
- authentication and authorization;
- session cookies;
- JSON/object input;
- debug endpoints;
- static files;
- environment/config exposure;
- dependency/version context;
- server-side input handling;
- reverse-proxy behavior.

## Defensive takeaways

Production Express deployments should disable unnecessary framework disclosure, use controlled error handling, protect secrets, validate input, enforce authorization on the server, secure cookies, remove debug endpoints, and keep dependencies patched.