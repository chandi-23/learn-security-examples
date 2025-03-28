# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    
    This file has privelege escalation vulnerability
    Simulated Authentication,
    const user = users.find(u => u.id === Number(userId));

    This line assumes the requester is the user they're claiming to be by sending a userId in the request body.
    The check for admin is ueseless as the user can send the newRole value in the request body.

2. Briefly explain how a malicious attacker can exploit them.
    
    The attacker sends this request:
       
    POST /update-role
    Content-Type: application/json

    {
    "userId": 1,
    "newRole": "admin"
    }

    Since there's no authentication/integrity check, allowing anyone to escalate their privileges.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

    1. use JWT/sessions to get the user details
    2. Simulate user data safely enforcing role constraints on the backend.


