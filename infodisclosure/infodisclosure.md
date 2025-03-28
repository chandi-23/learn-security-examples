# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

    const user = await User.findOne({ username: username as string }).exec();
    
    directly uses user input (req.query.username) in a MongoDB query without validation or sanitization.
    Information Disclosure:

    If an attacker successfully injects a malicious query, the server may reveal sensitive user data (e.g., full user document).

    Logging Raw Input:
    Logging the username directly (console.log(username)) leaks sensitive data to logs.

2. Briefly explain how a malicious attacker can exploit them.
    
    If the attacker sends message:

    GET /userinfo?username[$ne]=null

    Result: MongoDB returns the first user document where username is not null — no auth required!

    This allows the attacker to:

    Bypass authentication

    Retrieve potentially sensitive user information

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
    
    Using parameterized queries

    Check for type validators

    Use input validators