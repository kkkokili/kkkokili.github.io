---
layout:     post
title:      Understanding Response.json() in Fetch API deeply
subtitle:   (Original) Web-Backend-Fetch()
date:       2021-07-21
author:     KOKILI
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - Back End
    - Async
---

Because the using of Fetch API is based on the understanding of promise. I will first comb through two concepts learnt in asynchronous operation so far.
# Step 1:
Comparing XMLHTTPRequest() & Promise in asynchronous operation >>
>>
## What is Promise?
The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.
A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future.

![Snipaste_2021-07-20_17-07-51.png](https://i.loli.net/2021/08/13/KVMYHG9aSJFytID.png)

## What is XMLHTTPRequest() with call back ?

![Snipaste_2021-07-20_17-04-30.png](https://i.loli.net/2021/08/13/kAergOTutD5QNE3.png)

![Snipaste_2021-07-20_17-04-37.png](https://i.loli.net/2021/08/13/jker8s7aKMZm3vd.png)

我觉得XMLHTTPRequest和Promise 都会用到XMLHTTPRequest,但是区别在于用了callback的XMLHTTPRequest还要想着写什么样的callback function来传递收到的从server传过来的data,到下一个function中直到web app的功能都得以实现。而promise是自带传递data功能的object,  从server 接受data asynchronously >> xhr在promise object中得到true/false的判断依据+ (resolve,reject)方法传递到 >> promise (.then, .catch) 就okay了。

# Step 2:
Response.json() in Fetch API
After having an overall preview of  XHR in Ajax and Promise, now I can understand fetch better. First, I should know what is fetch:

![网页捕获_20-7-2021_172939_developer.mozilla.org.jpeg](https://i.loli.net/2021/08/13/IsZY3RXhSNMrGxc.jpg)

![Snipaste_2021-07-20_17-39-07.png](https://i.loli.net/2021/08/13/NmOIj9GUockHa5R.png)

![Snipaste_2021-07-20_17-41-22.png](https://i.loli.net/2021/08/13/WiCvfElXKLNSGmI.png)

![Snipaste_2021-07-20_17-41-28.png](https://i.loli.net/2021/08/13/Wg3JdUrvacmQGRL.png)

Fetch相当于把XHR和promise结合简化了，而且是特别精简。 但是还是要理解一下response.json()
部分。

![un.png](https://i.loli.net/2021/08/14/NYvlCwLDFgTb43x.png)
