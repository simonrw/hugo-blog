---
Title: Cross site REST development
Date: 2013-03-10
Category: tech
keywords:
- python
- webdev
layout: post
comments: true
---



Often when developing complex client side apps, a simple `python -m SimpleHTTPServer ` can host the html. For a node backend though, a second server has to be run to host the REST api, which must be on a different domain. For example the python server is on port `8000`, whereas the REST server is run on port `3000`.

<!--more-->

This will usually throw an error containing the script 

    Origin https://localhost:8000 is not allowed by Access-Control-Allow-Origin

This is a security measure input by web browsers to prevent cross domain scripting attacks, and is a good idea, but during development this can be a problem.

One option is to use a fully featured web server e.g. express but for rich client side applications this is generally overkill, a simpler library such as [restify](https://mcavage.github.com/node-restify/) is ideal.

A nice option as supplied by [this](https://stackoverflow.com/a/14187628/56711) SO answer is to add middleware to allow certain domains access to the REST server.



For development this is a handy workaround, but for production obviously *DO NOT USE!*

For restify this can be inserted before your normal CRUD routes as middleware using `server.use(...)`.
