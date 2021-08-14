---
layout:     post
title:      Fix The Problem “LocalHost ERR_CONNECTION_REFUSED” in XAMPP as Set Up Local Dev Environment When Learning Ajax Basics
subtitle:   (Original) 用XXAMPP配置本地环境
date:       2021-06-21
author:     KOKILI
header-img: img/post-bg-shadow.jpg
catalog: true
tags:
    - AJAX
    - Back End
    - Error
---
## First Error in XAMPP
Today I came across an error when I try to use the XAMPP to deploy the local server envirnment.
[Ajax Crash Course](https://www.youtube.com/watch?v=82hnvUYY6QA)

The error shows as below:

![1.png](https://i.loli.net/2021/08/11/ScPHkrs25DpmCZ1.png)

## Second Error 'Port Occupied'
So I googled and someone says that “Make sure you started php and mysql from xampp control panel now on your browser”, and Indeed, I didn’t turn the mysql on. When I try to turn mysql in XAMPP on, another problem popped up which says **‘Port 3306 in use by "Unable to open process"!‘**:

![Snipaste_2021-07-13_19-19-31.png](https://i.loli.net/2021/08/11/YBj9ziIRrFvCO1p.png)

## Solution
Again, I rummaging around on StackOverflow to find the solution, and I found sth helpful:

![Snipaste_2021-07-13_19-38-03.png](https://i.loli.net/2021/08/11/FjW4rp7Y8BXDmTh.png)
![Snipaste_2021-07-13_19-38-12.png](https://i.loli.net/2021/08/11/pZ9yL8g7TXnz2uI.png)

I tried this out in my CMD:

![Snipaste_2021-07-13_19-39-40.png](https://i.loli.net/2021/08/11/aKo81ZeEpfUxY24.png)

And find one programme with PID (process Id) 6916,
useful link explaining about PID: https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/finding-the-process-id
And finally I found the culprit mysqld.exe

![Snipaste_2021-07-13_19-45-36.png](https://i.loli.net/2021/08/11/63mQW71PaDhSpLV.png)

I finished it, and restart mysql in XAMPP, Then successed. The localhost problem alose solved!
