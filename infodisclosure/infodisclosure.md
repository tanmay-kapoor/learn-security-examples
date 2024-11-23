# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in
   the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test
   data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and
running the command `show dbs;`.

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

The insecure.ts file has a NoSQL injection bug. This comes from the way this code handles user input in the
/userinfo route. It directly uses the 'id' query parameter in MongoDB query without any kind of validation or
sanitization. This would, therefore, allow an attacker to exploit this bug by injecting malicious query objects into
the database query.

2. Briefly explain how a malicious attacker can exploit them.

- Query Injection: The id query parameter is directly used to query the MongoDB database without any validation or
  sanitization, which might allow an attacker to inject malicious query objects and manipulate the database query.
- ObjectId Validation: The id parameter is not validated as a valid MongoDB ObjectId, which allows attackers to send
  crafted requests to manipulate the intended query,

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

- Input Type Validation: This checks that the query parameter username is of type string. If this condition does not hold, then the server will return a status of 400 Bad Request to make sure only valid inputs are processed.
- Input Sanitization: The username input gets sanitized through a regex: `username.replace(/[^\\\\\\\\w\\\\s]/gi, '')`. This removes any non-alphanumeric characters and thus prevents malicious inputs from being executed within database queries in order to mitigate NoSQL injection.
- Security: The SQL query is wrapped in a try/catch block to catch and log any errors that may occur without exposing sensitive information, while returning a generic response 500 Internal Server Error to the client.
- Context-Appropriate Querying: It is not querying against a generic identifier, such as _id, instead against a username field that is more applicable to this authentication and less likely to expose data accidentally.