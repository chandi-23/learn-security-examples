# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

    The vulnerability lies in how the session cookie (connect.sid) is exposed and not protected:

    httpOnly: false in session config:
    allows JavaScript access to session cookies, making it easy for attackers to steal them via XSS or other means.

2. Briefly explain different ways in which vulnerability can be exploited.
    
    1. A malicious site like mal-steal-cookie.html runs:

    `document.cookie`
    and logs the user's connect.sid value.

    The attacker can now impersonate the user by attaching this stolen cookie in their requests.

    2. The user visits mal-csrf.html while still logged into insecure.ts.

        That malicious page sends a POST request (via form or fetch) to:

        `http://localhost:8000/sensitive` without the user's knowledge.

    Since the browser automatically includes the session cookie, the server believes it’s a legitimate request and performs the sensitive operation.

    Even though the user never clicked anything on the original site, the attacker has tricked the server into executing privileged actions.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

    Session Cookie Theft: httpOnly: true ensures JavaScript cannot access the session cookie, blocking theft.
    
    CSRF Protection	sameSite: true (default = 'Lax') ensures cross-origin POSTs are blocked.

    Session Integrity: secret passed via CLI and used securely; not hardcoded.

    No exposure via logging	No console.log(req.body) or other leaks of session data.
