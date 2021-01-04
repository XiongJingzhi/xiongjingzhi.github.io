---
title: 浏览器与Node的事件循环
date: 2019-04-12 11:27:23
tags: front-end
---

## 线程与进程
  1. 进程：CPU资源分配的最小单位(独立内存)
  2. 线程：CPU调度的最小单位(共享进程的内存, 实际执行)

## Chrome
  - process
    - Browser主进程: 浏览器界面, tab页面的管理, 绘制Renderer进程的bitmap, 网络资源的管理、下载
    - 每个tab(优化合并)的Render进程
    - 第三方插件进程
    - GPU进程
  - thread
    每个Render进程
    - GUI 渲染线程
      - 负责渲染浏览器界面, 解析HTML, CSS, 构建DOM树和RenderObject树, 布局和绘制等
      - 当界面需要重绘（Repaint）或由于某种操作引发回流(reflow)时, 该线程就会执行
      - GUI渲染线程与JS引擎线程是互斥的, 当JS引擎执行时GUI线程会被挂起（相当于被冻结了）, GUI更新会被保存在一个队列中等到JS引擎空闲时立即被执行
    - JS引擎线程
      - 负责解析Javascript代码
      - JS引擎一直等待着任务队列中任务的到来, 然后加以处理, 一个Tab页（renderer进程）中无论什么时候都只有一个JS线程在运行JS程序
      - 如果JS执行的时间过长, 这样就会造成页面的渲染不连贯, 导致页面渲染加载阻塞
    - 事件触发线程
     - 与js引擎线程独立, 用来控制事件循环


