---
layout:     post
title:      Node JS Authentication Guide
subtitle:   (Original) Authentication in Node.js
date:       2021-08-16
author:     KOKILI
header-img: img/about-bg-walle.jpg
catalog: true
tags:
    - Node.js
    - Authentication
---
> CONSIDER THIS
> You’ve added a popular hashing method to your application, but you’d like to simplify the code or, better, put it behind the scenes. It’s great to   know how hashing works, and tools are available to perform the hashing you want without the need to manually set up your own criteria for hashing.  Packages such as passport.js hash and authenticate user interactions without your needing to specify a password field in the schema. In this article, you look at the quickest and most efficient implementations of the passport package.


> note:  Note Session data is not saved in the cookie itself, just the session ID. Session data is stored server-side.

## Prerequisites
package: passport, passport-local passport-local-mongoose express-session

## Package Install Code

`npm i passport passport-local passport-local-mongoose express-session`

## Package Clarify
### express-session

This package is used to create a session middleware and set the needed cookie for the specified cookie



#### Set-Up-Code

```
const session =require('express-session');

app.use(session({
  secret: 'key that will sign cookies',
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}))

```

#### Option Params Explain

* **resave**:  This basicly means for every request to the server, we want to have a new session.

* **saveUninitialized**:  Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing false is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie. Choosing false will also help with race conditions where a client makes multiple parallel requests without a session.






### Passport
Passport.js is middleware used by Node.js to hash new user passwords and authenticate their activity on an application. Passport.js uses different methods to create and log in user accounts, ranging from basic login with username and password to login with third-party services such as Facebook. These login methods are called strategies, and the strategy you’ll use for your recipe application is a local strategy because you aren’t using external services.

### passport-local-mongoose
Passport-Local-Mongoose is a Mongoose plugin which uses passport and mongoose together to perform hashing for you behind the scenes. This module auto-generates salt and hash fields, you don’t require to hash the password with this crypto module, the passport-local-mongoose does this for you.



## Reference

* [Local Authentication Using Passport in Node.js](https://www.sitepoint.com/local-authentication-using-passport-node-js/)
* [Everything you need to know about the `passport-local` Passport JS Strategy](https://levelup.gitconnected.com/everything-you-need-to-know-about-the-passport-local-passport-js-strategy-633bbab6195)
* [passport-local](http://www.passportjs.org/packages/passport-local/)
* [Authentication: JWT usage vs session](https://stackoverflow.com/questions/43452896/authentication-jwt-usage-vs-session)
