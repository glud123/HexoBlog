---
title: express框架使用
date: 2017-05-13 09:47:23
tags: [node,node基础]
categories: node
---
## express 是后台框架
帮我们解决手动搭建服务，处理逻辑的复杂
```
npm init -y
npm install express --save
```

## 使用postman模拟数据发送
<a href="https://www.getpostman.com/">https://www.getpostman.com/</a>
> 测试接口是否可用

## 路由
根据请求的方法和请求的路径返回不同的内容
```
app.方法('路径',callback)
app.all('*',callback)
```
> 匹配路由从上到下匹配，匹配成功后不继续向下执行

## express提供的属性
```
req.path
req.query
req.method
req.headers
```

## 路径参数params
- :id表示站位必须要有
```
app.get('/user/:id',callback)
```

## 中间件
中间件一般在路由上面写，错误中间件一般写在底部
- 扩展属性和方法，请求时中间件的res和req与路由中的是同一个
- 做权限处理，next可以决定是否向下执行
- 中间件可以写多个，和路由在同一个数组中
```
app.use('/',function(req,res,next){})
next();//表示是否向下执行
```
> 默认不写路径任何请求都能执行
