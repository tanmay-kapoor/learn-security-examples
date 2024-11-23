# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the
services provided by a server.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

The insecure.ts vulnerability involves the lack of authentication and logging mechanisms. Any malicious user is
enabled to send and retrieve messages without identifying themselves. Since there are no logs tracing messages to users,
users can even deny responsibility for their actions. This leads to a situation wherein users cannot be held
accountable,
and thus, the service is open to many different types of misuse and potential abuse, given that anyone can access the
endpoints without restriction.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.

The secure.ts file addresses the vulnerability by introducing user authentication and logging mechanisms. This ensures
that users must log in before sending messages or obtaining messages, making these actions only possible for
authorized people. Also, it logs every user request and action, creating a comprehensive audit trail. The logging
enables tracking all the interactions made with the server by a particular user; thus, it will prevent them from
later denying their actions, thereby enhancing accountability.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

Security-related issues are dealt with in the secure.ts file by implementing the Middleware design pattern.
The middleware functions run before the requests make it to the route handlers. In secure.ts, middlewares are used for
logging of requests, error handling, and authentication checks. In this way, it provides a separation of concerns and,
hence, various kinds of functionalities, like logging, authentication, and error handling can be independently handled
and yet applied consistently over all routes. The application can remain clean by using middleware, but at the same
time ensure security measures are applied properly throughout.