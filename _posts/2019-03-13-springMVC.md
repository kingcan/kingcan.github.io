---
title: 关于web的request与response
layout: post
categories: Java
tags: JavaWeb
---
## 请求与转发的问题
<p align="center">
	<img src="https://i.loli.net/2019/06/14/5d0341d1a190018312.png" alt="Sample"  width="750" height="340">
	<p align="center">
		<em>index.jsp</em>
	</p>
</p>
<p align="center">
   <p align="center">
		<em>是对于这个pageContext.request.contextPath的不了解，上图中是index页面，
启动项目就是从这里开始的，这个界面很明显有三种请求参数，<br >
假设我们点击注册就会发出/register的请求，PageController会来响应这个请求。它会返回结果给/register界面。在这个界面里面</em>
	</p>
</p>

<p align="center">
	<img src="https://i.loli.net/2019/06/14/5d034300e081c89824.png" alt="Sample"  width="750" height="340">
	<p align="center">
		<em>pageController（全局控制）</em>
	</p>
</p>
<p align="center">
   <p align="center">
		<em>在这个界面里面，填写表格，再一次发出请求/user/register,但如何知道是交给UserController来处理呢？注解又是如何知道是交给哪个Controller类来处理呢<br >
		这个问题以后再说这个</em>
	</p>
</p>

<p align="center">
	<img src="https://i.loli.net/2019/06/14/5d0347624412c21955.png" alt="Sample"  width="750" height="340">
	<p align="center">
		<em>register.jsp（注册界面）</em>
	</p>
</p>

   <p align="center">
		<em>
		controller处理完后，将结果返回给success.jsp</em>
	</p>
</p>

<p align="center">
	<img src="https://i.loli.net/2019/06/14/5d03497a06e8021159.png" alt="Sample"  width="750" height="340">
	<p align="center">
		<em>success.jsp（返回成功界面）</em>
	</p>
</p>