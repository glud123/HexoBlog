---
title: express中cookie/session
date: 2017-05-14 17:43:09
tags: [node,node基础]
categories: node
---
## body-parser
解析请求体的会将请求的数据挂载在req.body上
```
npm install body-parser --save
let bodyParser = require('body-parser');
app.use(bodyParser.json());//解析json
app.use(bodyParser.urlencoded({extended:false}));//解析formData的格式
```

## querystring的核心模块
```
let querystring = require('querystring');
querystring.parse('name=1&age=2','&','=');
querystring.stringify({name:1,age:2},'&&','=');
```

## cookie
```
npm install cookie-parser --save
let cookieParser = require('cookie-parser');
app.use(cookieParser());
req.cookies //可以直接获取cookie转换后的对象
req.cookie(key,value,{domain,path,expires,maxAge,httpOnly});
```

## session
```
npm install express-session --save
let session = require('express-session');

```
