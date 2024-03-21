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
