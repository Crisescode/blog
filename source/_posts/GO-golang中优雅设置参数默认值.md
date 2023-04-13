---
title: '[GO] golang中优雅设置参数默认值'
date: 2023-04-13 23:55:33
index_img: /img/2.jpg
banner_img: /img/2.jpg
tags:
  - golang
category: GO
---
## 前言

今天在使用 golang 重构 python 站点时，遇到了一个有趣的事情。就是在 python 函数中参数是可以设置默认值的，这样在函数调用时，就不需要每个参数都传一个值，其实这也是变相的实现重载功能。但是我发现在 golang 中参数是不能设置默认值的，但是基于程序员的直觉，相信此事没那么简单，于是抱着打破沙锅问到底的态度，就有了这篇文章。
## 不设置默认值带来的不便

首先，通过一个例子，说明我在编码过程中遇到的问题。对于平台后端开发工程师来说，会经常遇到以下场景：
通过

- 通过多种条件查询数据库记录，比如：select * from user where user_name in (xxx, xxx, xx) and user_region = “shanghai” order by id desc；
- 需要在不同功能模块做以上方式查询。

基于此，我将这些查询方式抽象成一个方法，之后所有的功能模块，都可以使用该方法进行多条件联合查询数据记录，具体抽象如下：

```
func DBQueryAllWithConditions(ctx context.Context, orderBy map[string]string, eqFilters map[string]interface{}, inFilters map[string]interface{}, likeFilters map[string]string) {
		
}
```

## 研读开源代码的优雅实现

## 改进自己的代码

## 总结