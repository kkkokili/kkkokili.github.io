---
layout:     post
title:      event.target vs event.currentTarget
subtitle:   target和currentTarget的区别
date:       2021-12-29
author:     KO
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - JS
    - Front End
  
---


# event.target vs event.currentTarget

#### event.target是你当前点击的target element, currentTarget是监听事件绑定的element (event.currentTarget也是this)。
#### （下面示列 1指代整个element， 2是1中的一个元素，3是1中的p标签） 
#### 比如你的click listener绑定了1, 现在提出一个需求: 当你随便点击1的时候（不管点到1哪里），你都能获取到3的text. 这个时候不能用e.target,因为这是你点击的元素，如果你点在2处获取不到3。这时要用e.currentTarget,3是e.currentTarget.childNodes[1].firstChild.innerText.

```
-------------------------------
|     1.        3：text        |
|              ----------      | 
|              |  2.     |     |
|              ----------      |
-------------------------------
```

![picture](https://upload.cc/i1/2022/02/16/8RwtYU.png)


正确写法：`e.currentTarget.childNodes[1].firstChild.innerText`

-------------------- 我是分割线---------------------------------
另外,  有关冒泡与捕获事件的传送带:
[冒泡与捕获](https://zh.javascript.info/bubbling-and-capturing)


