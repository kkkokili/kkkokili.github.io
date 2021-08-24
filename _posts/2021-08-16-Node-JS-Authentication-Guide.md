---
layout:     post
title:      Node JS Authentication Guide
subtitle:   (Original) Authentication in Node.js
date:       2021-08-16
author:     KOKILI
header-img: img/jellyfish.jpg
catalog: true
tags:
    - Node.js
    - Authentication
---
> CONSIDER THIS
> You’ve added a popular hashing method to your application, but you’d like to simplify the code or, better, put it behind the scenes. It’s great to   know how hashing works, and tools are available to perform the hashing you want without the need to manually set up your own criteria for hashing.  Packages such as passport.js hash and authenticate user interactions without your needing to specify a password field in the schema. In this article, you look at the quickest and most efficient implementations of the passport package.


> note:  Note Session data is not saved in the cookie itself, just the session ID. Session data is stored server-side.

## Prerequisites
package: passport, passport-local, passport-local-mongoose, express-session

`npm i passport passport-local passport-local-mongoose express-session`

### Documentation:

* [express-session](https://openbase.com/js/express-session)
*  [passport](http://www.passportjs.org/docs/)
*   [Different passport strategy](http://www.passportjs.org/packages/)
*    [passport-local-mongoose](https://openbase.com/js/passport-local-mongoose)

## Package Guide
### express-session

This package is used to create a session middleware and set the needed cookie for the specified session. This module expands the Express request object with the session property (among other things), which itself is an object that can be used by other middleware.

It creates a session for every user by generating a special ID that serves as a unique key for the session data. This ID is stored and sent in a cookie, while the session data is saved in a memory store or cache.

![2021-08-21.png](https://i.loli.net/2021/08/21/ZU7bRvqTjCH5NI9.png)

Cookie-session is basically used for lightweight session applications where the session data is stored in a cookie but within the client(browser). Browsers are supposed to support at most 4096 bytes per cookie, to ensure you don’t exceed the limit, don’t exceed a size of 4093 bytes per domain.

whereas, Express-Session stores just a mere session identifier within a cookie in the client end, whilst storing the session data entirely on the server. Compared with cookie-session package which only resides in browser, it supports more different session stores (like files, DB, cache and whatnot).

Cookie Session is helpful in applications where no database is used in the back-end. However, the session data cannot exceed the cookie size. On conditions where a database is used, it acts like a cache to stop frequent database lookups which is expensive.



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

![Snipaste_2021-08-21_16-23-15.png](https://i.loli.net/2021/08/21/ner46RU5yqSgC3E.png)

* **secret**: The secret param is used to encrpt the session id, because you don't want the normal id to be sent in the browser, so that anyone can see it. So we will enript it first through the express-session package here.

* **resave**:  This basicly means for every request to the server, we want to have a new session. when set to true, this will force the session to save even if nothing changed.  If you don't set this, the app will still run but you will get a warning in the terminal

* **saveUninitialized**:  Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing false is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie. Choosing false will also help with race conditions where a client makes multiple parallel requests without a session.


#### Test express-sessions

I try to read as many articles as I can to figure out how exactly all these packages work, and which package would in chage of which part. However, I haven't found a crystal enough article to explain each mechanism clearly. So I try to test each package by myself. The first one is express-session. In order to test it, I deleted passport, passport-local and passport-local-mongoose and followed the documentation of express-session. And the first several code I test here is 

```
const express = require("express");

const app = express();

const ejs = require("ejs");

const mongoose = require("mongoose");

const session =require('express-session');

const MongoDBStore = require('connect-mongodb-session')(session);

// ---------------------Server Setup Template START-----------------------------

app.set('view engine', 'ejs');

app.use(express.static("public"));

app.use(express.urlencoded({
  extended: true
}));

app.use(express.static("public"));


// -----------------------------MONGOOSE START---------------------------------

var store = new MongoDBStore({
  uri: 'mongodb://localhost:27017/connect_mongodb_session_test',
  collection: 'mySessions'
});

// Catch errors
store.on('error', function(error) {
  console.log(error);
});

// ------------------------------MONGOOSE END-----------------------------------

// ----------------------express session---------------------

app.set('trust proxy', 1); // trust first proxy
app.use(session({
  secret: 'xiao',
  store: store,
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true,
             maxAge: 1000 * 60 * 60 * 24 * 7 /*1 week*/
          }
}));
```
when I get to the homepage, I get the session result of 

![Snipaste_2021-08-24_19-00-54.png](https://i.loli.net/2021/08/24/GkiCKSRvcTlNHDe.png)

notice here I set the option "saveUninitialized" true. Next, let's see what will happen after set it to false.

![Snipaste_2021-08-24_19-18-51.png](https://i.loli.net/2021/08/24/XNgxKjUC2E5RYIf.png)

Different from the result we get above, when we reach the homepage, No session saved in the collection. But I'm still confused about what they wanna express through the word "Uninitialized"...

When I read through the documentation of passport in "sessions" narration of "Configure" H1, I think that's might be the hint. The sentence shows as below.

![Snipaste_2021-08-24_21-17-27.png](https://i.loli.net/2021/08/24/YG4y72z6FPE8vKw.png)

I guess if we set the uninitialized  false, the session will only be established when the authentication is successed. It's my only guess, but I will leave it here and let it go. Next let's use all the 4 packages(express-session, passport, passport-local, passport-local-mongoose) together to set up the web app.





### Passport
Passport is Express-compatible authentication middleware for Node.js.

Passport's sole purpose is to authenticate requests, which it does through an extensible set of plugins known as strategies. Passport does not mount routes or assume any particular database schema, which maximizes flexibility and allows application-level decisions to be made by the developer. The API is simple: you provide Passport a request to authenticate, and Passport provides hooks for controlling what occurs when authentication succeeds or fails.

### Passport-Local
Passport strategy for authenticating with a username and password.

This module lets you authenticate using a username and password in your Node.js applications. By plugging into Passport, local authentication can be easily and unobtrusively integrated into any application or framework that supports Connect-style middleware, including Express.

Passport JS has over 500 authentication “Strategies” that can be used within a Node/Express app. Many of these strategies are highly specific (i.e. passport-amazon allows you to authenticate into your app via Amazon credentials), but they all work similar within your Express app.
In my opinion, the Passport module could use some work in the department of documentation. Not only does Passport consist of two modules (Passport base + Specific Strategy), but it is also a middleware, which as we saw is a bit confusing in its own right. To add to the confusion, the strategy that we are going to walk through (passport-local) is a middleware that modifies an object created by another middleware (express-session).

### passport-local-mongoose
Passport-Local-Mongoose is a Mongoose plugin which uses passport and mongoose together to perform hashing for you behind the scenes. This module auto-generates salt and hash fields, you don’t require to hash the password with this crypto module, the passport-local-mongoose does this for you.



## Reference


* [Everything you need to know about the `passport-local` Passport JS Strategy](https://levelup.gitconnected.com/everything-you-need-to-know-about-the-passport-local-passport-js-strategy-633bbab6195)
* [passport-local](http://www.passportjs.org/packages/passport-local/)
* [Authentication: JWT usage vs session](https://stackoverflow.com/questions/43452896/authentication-jwt-usage-vs-session)
* [Cookies and Session IDs](https://cscie12.dce.harvard.edu/lecture_notes/2007-08/20080423/slide51.html)
