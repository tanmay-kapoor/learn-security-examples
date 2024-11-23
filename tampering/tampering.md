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

The insecure.ts application has some vulnerabilities with improper input handling and session management. It directly
injects user input into the response-for example, in a name field-without sanitizing it, thus allowing the possibility
of Cross-Site Scripting attacks. On top of this, poor input validation also creates an added risk of sessions being
hijacked or manipulated through malicious scripts.

2. Briefly explain how a malicious attacker can exploit them.

This is because an attacker may inject malicious scripts in the name field, thereby running the script on page render
to steal sensitive information-such as session cookies-or perform unauthorized actions on behalf of the user.
An attacker could inject scripts to access or modify session data, thereby impersonating other users, escalating
privileges, or disrupting the application's flow of sessions.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

In secure.ts, user input from the name field is sanitized by the escapeHTML function before being included in the
response. This function escapes special HTML characters so that any malicious scripts, such as
`<script>alert('Hacked!');</script>`, would show up as plain text instead of executing in the browser. 
Additionally, secure.ts follows best security practices by using the httpOnly and sameSite cookie attributes,
which effectively mitigates Cross-Site Request Forgery (CSRF) and other session-related attacks.