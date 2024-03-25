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

ToLearn

- Promise
- Middleware
- Feedback Loop
