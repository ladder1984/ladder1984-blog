---
title: 浅谈行为驱动开发(BDD)
date: 2017-05-16 00:45:44
tags: 
 - 第一站
---
## 简介

BDD（Behaviour-Driven Development行为驱动开发）是一种敏捷开放方式，鼓励开发者、测试、业务方参与开发协作，虽然多数情况下只有开发和测试参与。BDD使用自然语言描述用户故事，从而让各方都可读、可写测试用例，并且用通用的语言精确描述软件的行为。

### 核心思想

一种测试方案，模拟用户的真实行为，来保证各个软件模块执行后的最终结果是正确的。

<!-- more --> 
## 运作方式

### 文件组织
用Gherkin语法描述step，一个个关联且有序的step组成完整的scenario，相关的scenario放入一个feature文件中。

### Gherkin语法

- Given：把系统置为某种状态，也就是准备数据
- When：描述用户行为，也就是要测试的step
- Then：获得结果，通常用于校验
- And：表示使用上一个step的关键词

### 标记
用tag来标记不同模块，也用tag来标记通过和未通过的scenario

### 执行
令人怀念的`behave -kt @xxx`命令

### Jenkins执行

可用Jenkins输出完整报告。在Jenkins执行时可用tag或目录分割多组feature，则并行执行，理论最短时间为最长scenario的耗时。

参考：
- [Gherkin](https://github.com/cucumber/cucumber/wiki/Given-When-Then)
- [Given When Then](https://github.com/cucumber/cucumber/wiki/Given-When-Then)

## 优缺点

### 优点

以用户故事为测试单位，保证软件在真实、连贯场景中的正确性。

### 缺点

当业务越来复杂、微服务越来越多时，这种集成测试方案难以维护，对开发者要求较高。在没有良好并行方案时，速度很慢。

## 技术

- 库：behave，其他Python库实现：Lettuce、radish
- 处理异步：celery使用同步模式、异步消息消费者采用子进程阻塞调用
- 夸服务调用：BDD SERVER，使用http服务器传递step，并使用sub step调用，用http response返回结果
- 第三方服务：各种mock
- IDE支持：Pycharm


