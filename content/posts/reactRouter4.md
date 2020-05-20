---
title: 使用 react-router ^4.0  创建 404 页面
date: 2019-09-25
author: "me"
tags: ["react", "react-router", "原创"]
categories: ["前端"]
---

## Why

经常访问网站的你们对 404 应该不陌生。404，通俗点讲就是网页找不到了。比如一个博客网站，删除了某篇文章对应的页面，而用户仍使用原URL进入，则会进入404页面。如果我们不做任何处理，可能会显示如下：

> 404 Not Found
> nginx/x.x.x

这样的用户体验会很差，如果我们的前端是由 React 构建的，如何创建并定义404页面呢？

## What

我们可以使用 React Router 来定义路来，且可用其创建404页面。

[react-router](https://github.com/ReactTraining/react-router): 定义React路由的库

> 本文使用 react-router 4+ 版本

## How

创建 404 页面和其他页面几乎没什么不同，看下 Demo 就明白了：

```jsx
// App.js

...

const NoMatchPage = () => {
  return (
    <h3>404 - Not found</h3>
  );
};

const App = () => {
  return (
    <section className="App">
      <Router>
        ...
        <Link to="/users">Users</Link>
        <Link to="/search?q=react">Search</Link>
        ...
        <Route path="/about" component={AboutPage} />
        <Route path="/search" component={SearchPage} />
        <Route component={NoMatchPage} />
      </Router>
    </section>
  );
};

...
```

当路由指定的所有路径没有匹配时，就会加载这个NoMatchPage组件，也就是404页面（没错，就那么简单）。至于组件要怎么搞，完全自由发挥了。

看到这里，严重质疑这篇文章很水，好吧... 我承认。其实，404 页面更多需要产品的定制，有兴趣的可阅读：[404页面三问：你真的懂「404页面」？](http://www.woshipm.com/pd/740464.html)


