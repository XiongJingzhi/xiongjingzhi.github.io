---
title: regular express
date: 2019-03-21 14:25:29
tags: regular expression
---

## 正则表达式

正则表达式是用来处理字符串的匹配字符串中字符组合的模式,
可以用 有限状态机 来表示， finite state machine。

``` mermaidjs
graph LR
s0((s0))-->|a|s1((s1))
s1-->|b|s2((s2))
s2-->|b|s3((s3))
s3-->|b|s2((s2))
s3-->|a|s4((s4))
```
<div class="post-svg-container">
  ![abbba-fsm](/images/abbba-fsm.svg)
</div>