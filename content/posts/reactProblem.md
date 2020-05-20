---
title: react style !important 失效问题的解决方案
date: 2019-10-17
author: "me"
tags: ["react", "原创"]
categories: ["前端"]
---


## What

在 react 项目中，发现 style 加上 `!important` 后未生效，参考 codesandBox 示例如下：
![react_ques_style.png](http://image.ftopia.cn/blog/react_ques_style.png)

css 里面已有定义 `!important` ，故需要在 style 里面加上强制，官方建议不加`!important`故不做支持，且此类场景很少，但万一必须要呢？这边提供一个替代方式。

## How

使用节点 .setProperty 方式，代码更改参考如下：

```jsx
    <div className="App">
      {/* <h1 style={{fontSize: "12px !important"}}>Hello CodeSandbox</h1> */}
      <h1
        ref={(node) => {
          if (node) {
            node.style.setProperty('font-size', '12px', 'important');
          }
        }}
      >Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
```

相关 codesandBox 示例参考: https://codesandbox.io/s/react-style-import-j9k1n


