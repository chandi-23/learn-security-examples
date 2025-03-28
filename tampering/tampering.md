# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    
    This server is tampering via Script Injection (XSS)

    The req.body.name value is taken directly from user input and stored in the session:

    req.session.user = req.body.name.trim();
    Later, it is rendered directly into the HTML without escaping:
    This makes the app vulnerable to Cross-Site Scripting (XSS): attackers can inject malicious HTML or JavaScript into the DOM.

2. Briefly explain how a malicious attacker can exploit them.

    When the attacker submits this payload as their name, it gets saved into the session without sanitization.

    On the next visit to /, the injected <script> runs in the victim’s browser.

    Attacker can manipulate the DOM, redirect users, steal session cookies, or even make background requests to perform CSRF.

    If a logged-in admin visits the page, their session could be hijacked.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

    Your secure.ts version fixes this through input sanitization, specifically by escaping dangerous HTML

    const sanitizedName = escapeHTML(req.body.name.trim());
    req.session.user = sanitizedName;
    
    escapeHTML() Function:
    Replaces characters like <, >, ", ' with their HTML entity equivalents:

    <script> &lt;script&gt;

    Any potentially malicious HTML is neutralized before rendering.

    The browser does not interpret it as executable code.

    Users see their input as plain text, not active HTML.

    this can be counterd with Sanitization pattern. 

