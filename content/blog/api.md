---
title: 'RESTful API'
date: 2024-03-20
draft: true
description: 'Backend Web Dev'
slug: 'api'
tags: ['Backend', 'RESTful API']
---

Write about restful api here.

API:
Application Programming Interface. Piece of software that can be used by another piece of software, in order to allow applications to talk to each other.

REST:

1. Separate API into logical resources
   - users
   - projects
   - bugs
   - teams
2. Expose structured, resource-based urls
   - \*//www.myproject.com/addNewProject (not good)
   - \*//www.myproject.com/projects/8(Project ID)
     - (use HTTP Method such as 'get')
3. Use HTTP methods
   - Create: Post
   - Read: Get
   - Update: Put / Patch (for patch, only need part of object)
   - Delete: Delete
4. Send data as JSON
5. Be stateless
   - All state is handled on the client
   - Server should not have to remember previous requests

Only use app.js for express related configs & more
For env variables, deal with it in server.js

Environment Variables

- Dev Environment
  - NODE_ENV = development
- Prod Environment
  - NODE_ENV = production

MongoDB

- NoSQL database
- Collections (tables)
  - Documents (rows)
- Scalable & Flexible

Mongoose

- Object Data Modeling(ODM) library for MongoDB and Node.js, higher level abstraction
- rqpid and simple development of mongoDB database interactions
  - schemas, easy data validations, simple query api, middleware etc.
    - schema: where we model data, describing structure, default vals, and validation
- Mongoose model: wrapper for the schema, pproviding an interface to the database for CRUD operations

## MVC

- Separate Business Logic & Application Logic

  - Application Logic (Controller)
    - Only concerned with application's implementation
    - managing req and res
    - more of technical aspects
    - bridge between model and view
  - Business Logic (Model)
    - Code tath actualy solves the business problem
    - Directly related to business rules
    - EX
      1. Creating new tours in database
      2. Check if user pw is correct
      3. Validating user input data
      4. Ensuring only users who bought a tour can review it
  - Fat Models & Thin Controllers:
    offload as much logic as possible onto the model, and keep controllers as simple as possible

- Model
  - Applications data, Business logic
- Controller
  - Handle application requests, interact with models, send back responses to client
- View
  - GUI, server-side rendered wbesites

Alias

- Creating middleware for popular routers/requests

Aggregation Pipeline

- For grouping, sorting, filtering data to get statistics that are useful for ex business needs

Mongoose Middleware:

- Document Middleware: before or after saving documents
- Query Middleware:
- Aggregation Middleware:
- Model Middleware

## Data Sanitization & Validation

- Setting models for each data types

## Errors in Express

1. Operational error

- Problems that we can predict will happen at some point
- Invalid user input, failed to connect to server, request timeout, invalid path accessed, etc.

-> Create a global express handling middleware so that all operational errors end up there.

2. Programming error

- Bugs in code

## User Authentication & Authorization

- bcrypt for password hashing => Argon2 is the newest algorithm among these and is currently considered to be the strongest password hashing algorithm available. It is designed to be memory-hard, meaning that it requires a lot of memory to compute. This makes it difficult for attackers to use specialized hardware like GPUs and ASICs to crack passwords. Argon2 has several parameters that need to be kept in mind when using it, such as the amount of memory to use and the number of iterations to perform.

Bcrypt is an older algorithm but is still widely used and considered to be secure. It is designed to be computationally expensive, meaning that it requires a lot of processing power to compute. Bcrypt has a cost parameter that determines the number of iterations to perform, and it also has a work factor that determines the amount of memory to use.

Scrypt is similar to bcrypt in that it is also designed to be computationally expensive. However, it is also designed to be memory-hard, like Argon2. Scrypt has several parameters that need to be kept in mind, such as the amount of memory to use and the number of iterations to perform.

PBKDF2 is a key derivation function that is designed to be computationally expensive. It is often used in conjunction with other algorithms, such as SHA-256 or SHA-512, to create a stronger password hashing function. PBKDF2 has a number of parameters, such as the number of iterations to perform and the length of the derived key.

When choosing which algorithm to use, it's important to consider the specific requirements of your use case. Factors such as the number of users, the computing resources available, and the security requirements should all be taken into account. In general, newer algorithms like Argon2 are considered to be stronger than older ones like bcrypt and PBKDF2, but they may require more memory or processing power to compute. It's also important to keep in mind that password hashing is just one aspect of password security, and other measures like password policies and multi-factor authentication should also be used to improve security.

- JSON WEB TOKEN WRITE HOW IT WORKS

- User AUTH IS SERIOUS AND IMPORTANT: DO IT RIGHT

- Sent email for resetting password

## Security Practices

1. Copmromised Database

- Encrypt passwords with salt and hash (argon2)
- Encrypt password reset tokens (sha256)

2. Brute force attacks

- Make login requests be slow (argon2)
- Implement rate limiting (express-rate-limit)
- Implement maximum login attempts (not implemented)

3. Cross-site scripting(XSS) attacks

- Store JWT in HTTP only cookies
- Sanitize user input data
- Set special HTTP headers (helmet package)

4. Denial-of-Service (DOS) attack

- Implement rate limiting (express-rate-limit)
- Limit body payload (in body-parser)
- Avoid evil regular expressions

5. NoSQL Query Injection

- Use mongoose for MongoDB (schemaTypes)
- Sanitize user input data

6. Other practices

- Always use HTTPS
- Create random password reset token with expiry dates
- Deny access to JWT after password change
- Don't commit sensitive config data to Git
- Don't send error details to clients
- Prevent Cross-Site Request Forgery (csurf package)
- Require re-authentication before a high-value action
- Implement a blacklist of untrusted JWT
- Confirm user email address after first creating account
- Keep user logged in with refresh tokens
- Implement two-factor authentication
- Prevent parameter pollution causing Uncaught Exceptions

## Data Modeling

1. Different types of relationships between data

   - 1:1
   - 1:Many
     - 1:Few
     - 1:Many
     - 1:Ton
   - Many:Many

2. Referencing/normalization vs Embedding/denormalization

   - Referenced
     - Pro: Easier to query each document on its own
     - Con: need 2 queries to get data from referenced document
   - Embedded
     - Pro: can get all the information in one query
     - Con: impossible to query embedded document on its own

3. Embedding or referencing decisions

   - Relationship type
   - Data access patterns
   - Data closeness

4. Different types of referencing

- Child referencing
- Parent referencing (Best One)
- Two-way referencing

FACTORY FUNCTION

- Works bc of JAVASCRIPT CLOSURES: Inner function gets access to vars of outer functions even after it is returned

USE INDEXES TO IMPROVE DB READ PERFORMANCE: Consider read-write pattern, compare query of using exact field with cost of using index

- It takes memory to use indexes. Only use on low Write-Read ratio documents (More reads than writes)

## TODO::Implement caching to prevent multiple requests of same data.

- https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage
- Cookies vs Web Storage API vs IndexedDB
  - For better performance and/or larger datasets, IndexedDB is better
  - However, only used for simple use cases

## Handlebars SUCK

- Cannot directly access res.locals, but pug can
- ## Does not have extends, but only partials

## TO provide current project ID into JS Frontend Code, create a hidden field in HTML/Pug with req.body.project.\_id included

## TODO: Instead of deleting tickets & projects, mark them as inactive and simply don't display it to user.

## Implemented Client-Side Rendering with frontend JS for comments on Tickets to reduce initial loading time and inefficient data retrievals

To Learn

- Promise
- Feedback Loop
- function() {} vs () => {}
- Static Method vs Review Method

# Choices Made

1. MongoDB choice: mySQL, noSQL, postgreSQL (child vs parent vs 2 way refenrencing vs embedding)
2. How I applied MVC
3. Security measures done (pw on DB, argon2, and more)
4. Performance improvements
5. Which templating engines to use (Pug, handlebars, ejs)
   Which frontend framework should be used?
