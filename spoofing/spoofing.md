# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request
forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

   `$ npx install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

   `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a
   __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the
   console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not
   logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

This is because the spoofing vulnerability in insecure.ts is due to the insecure handling of session
cookies, precisely setting the httpOnly property of the session cookie to false. It is now exposed to
client-side access and can be subjected to XSS attacks. The attacker will be able to steal session cookies by
injecting malicious scripts. Also, the inability of a web application to implement CSRF protection enables attackers
to conduct unauthorized actions in the name of clients, which is well demonstrated in examples where submission of
malicious forms may trigger sensitive operations without user consent.

2. Briefly explain different ways in which vulnerability can be exploited.

An attacker could write scripts to capture session cookies, which can later be utilized in attacks.
An attacker can inject malicious JavaScript into a webpage viewed by the victim. Because the session cookie is
accessible due to the disabled httpOnly flag, the attacker can steal the cookie and send it to his server, thus
enabling the session hijacking.
The attacker constructs a malicious link that will forward the victim to a rogue server designed to capture the
session cookie; this usually happens through phishing emails or by using fake advertisements.


3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

The HttpOnly property of the session cookie is set to true, which prevents the client-side scripts from accessing the
session cookie, reducing the chance for Cross-Site Scripting XSS attacks that may steal the cookie.
SameSite is set to true, which prevents Cross-Site Request Forgery (CSRF) attacks by forbidding the browser from
sending cookies with cross-origin requests. The same origin sends cookies with requests for the session, ensuring the
session cookie is not included in the requests of third-party sites.
The session secret is given as a command-line argument and not hardcoded in the source code. This approach further
improves security by preventing sensitive information from being exposed in version control systems.

