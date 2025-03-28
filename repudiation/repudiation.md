# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
    
    Malicious user can:

    Impersonate another user by sending arbitrary user values.
    
    Read all messages without authentication.
    
    Deny (repudiate) having sent any message — since there’s no logging or proof.


2. Briefly explain why the vulnerability is addressed in __secure.ts__.
    Impersonation: Logging identifies the sender and their IP.
    Repudiation (denying actions): All actions (sending, retrieving, errors) are logged with timestamps and IPs.
    Unauthenticated access: Simulated isAuthenticated check added in GET /get-messages.
    Lack of audit trail	server.log: logs all key actions: requests, message submissions, and errors.
    No error handling:An Express error-handling middleware catches and logs all server-side errors.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
    
    Uses middeleware Pattern i.e chain of responsibility pattern

    Middleware intercepts requests at different stages (before hitting the route).

    This approach provides separation of concerns:

        Logging logic is separate from business logic.

        Input handling is decoupled from routing.

        Security checks (like authentication) can be plugged in as middleware.