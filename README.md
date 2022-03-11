![](code/imgs/spider-5.png)

---

## HTTP
### HTTP简介：

###### HTTP（HyperText Transfer Protocol）即超文本传输协议，是一种详细规定了浏览器和万维网服务器之间互相通信的规则，它是万维网交换信息的基础，它允许将HTML（超文本标记语言）文档从Web服务器传送到Web浏览器。

###### HTTP协议目前最新版的版本是1.1，HTTP是一种无状态的协议，无状态是指Web浏览器与Web服务器之间不需要建立持久的连接，这意味着当一个客户端向服务器端发出请求，然后Web服务器返回响应（Response），连接就被关闭了，在服务器端不保留连接的有关信息。也就是说，HTTP请求只能由客户端发起，而服务器不能主动向客户端发送数据。

###### HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

### HTTP工作原理：
- HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。
- Web服务器有：Apache服务器，IIS服务器等
- Web服务器根据接收到的请求后，向客户端发送响应信息
- HTTP默认端口号80

### HTTP请求头和响应头
>HTTP请求报文由3部门组成(请求行+请求头+请求体)

![](code/imgs/http01.png)

HTTP请求报文

请求方法（Request method）
- GET
- POST
- HEAD
- PUT

请求URL地址（统一资源定位符）

协议名称版本号

报文头（服务端获取客户端信息key：value）

报文体（a=1&b=2的键值对编码成一个格式字符串传递数值）

HTTP请求报文属性
- Accpet：高数服务器客户端接受什么类型的响应
- Referer：表示这个请求是从那个url进来的
- Cache-Control：对缓存进行控制
- Accept-Encoding：接收编码格式
- Host：指定要请求资源所在主机和端口
- User-Agent：浏览器版本信息

### HTTP响应报文
>响应报文由三个部分组成（相应行，响应头，响应体）

![](code/imgs/http04.png)

- 报文协议及版本；

- 状态码及状态描述；

- 响应报文头，也是由多个属性组成；

- 响应报文体，即我们要的数据。

HTTP状态码

![](code/imgs/status_code.png)

- 200 OK 表示成功
- 303 重定向,把你重定向到其他页面
- 304 资源并未修改,可以直接使用本地的缓存
- 404 找不到页面(页面被删除或其他)
- 500 服务端错误

HTTP响应报文属性
- Cache-Control：响应输出到客户端后，服务器通过该属性告诉客户端该怎么控制响应内容的缓存
- ETag：表示你请求资源的版本，如果该资源发生变化，那么属性也会跟着变化
- Location：在重定向中或者创建新资源时使用
- Set-Cookie：服务端可以设置客户端cookie

---

![](code/imgs/http03.png)

### 一次完整的http请求过程：

>域名解析 --> 建立连接 --> 接受请求 --> 处理请求 --> 访问资源 --> 构建相应报文 --> 发送响应报文 --> 记录日志

- 域名解析：当用户在浏览器地址栏输入http://www.baidu.com 发起一个请求，首先会把该域名解析为ip地址。

    DNS 的详细解析过程：http://vinsent.blog.51cto.com/13116656/1967876

- 建立连接：浏览器会开启一个随机端口向服务器的80端口发起tcp连接请求，经过3次握手后建立tcp连接，然后向服务器发起httpd请求。   
  - TCP三次握手
  - TCP四次挥手

[![alt text](code/imgs/tcp.png "“TCP三次握手，四次挥手”你真的懂吗？")](https://zhuanlan.zhihu.com/p/53374516)

- 接收请求：接受请求所要完成的工作就是接收来自网络的请求报文中对某一资源的请求过程
  - 单进程I/O模型
  - 多进程I/O模型
  - 复用I/O结构
  - 复用多线程I/O模型
- 处理请求：以Apache的prefork工作模式为例，管理进程在接受到请求报文后会选择一个工作进程来对该请求进行处理，得到其请求方法和资源URL等相关信息
- 访问资源：对请求处理时一般需要访问后端资源，执行代码得到请求结果，把结果返回给服务器
- 构建响应报文：在得到返回的请求结果后，开始构建响应报文
  - 永久重定向
  - 零时重定向
- 发送响应报文：响应报文构建完成后，发送响应报文
- 记录日志：最后，当事务结束时，web服务器会在日志文件中添加一个条目，来描述已执行的事务

---

### Cookie
什么是Cookie？

![](code/imgs/cookie.jpeg)

  1. Cookie是一种在客户端保持HTTP状态信息的技术
  2. Cookie实在浏览器访问WEB服务器的某个资源时，由WEB服务器在HTTP响应消息头中附带发送给浏览器的数据
  3. 一旦WEB浏览器保存了某个Cookie，那么它在以后每次访问该WEB服务器时，都应在HTTP请求头中将这个Cookie发送给WEB服务器

> Cookie技术最先被Netscape公司引入到Navigator浏览器中，
> Cookie只是一段文本，所以它只能保存字符串。

Cookie功能特点：
- 存储于浏览器头部/传输于HTTP头部
- 写时带属性，读时无属性
- HTTP头中Cookie：user=admin;pwd=123;
- 属性name/value/expire/domain/path/.....
- 由三元组[name,doman,path]确定唯一cookie

Cookie的安全属性：
- secure属性：当设置为true时，表示创建的Cookie会被以安全的形式向服务器传输，也就是只能在HTTPS连接中被浏览器传递到服务器段进行会话验证，如果时HTTP连接则不会传递该信息，所以不能窃取到Cookie的具体内容。
- HttpOnly属性：如果在Cookie中设置了”HttpOnly“属性，那么通过程序（JS脚本等）将无法读取到Cookie信息，这样能有效防止XSS攻击。

> secure属性是防止信息在传递的过程中被监听捕获后信息泄露，HttpOnly属性的目的是防止程序获取cookie后进行攻击

### Session
什么是Session？

![](code/imgs/session2.png)

  1. 使用Cookie和附加URL参数都可以将上次请求的状态信息传递到下次请求中，但是如果传递的状态信息较多，将极大降低网络传输效率和增大服务器端程序处理的难度。
  2. Session是一种将会话状态保存在服务器端的技术。
  3. 客户端需要接收、记忆和发送Session的会话标识号，Session可以且通常是借助于Cookie来传递会话标识号。

Session的跟踪机制

Session的超时管理

利用Cookie实现Session跟踪
- 如果web服务器处理某个访问请求时创建了新的HttpSession对象，它将会把会话标识号作为一个Cookie项加入到响应消息中，通常情况下，浏览器在随后发出的访问请求中又将会话标识号以Cookie的形式回传给web服务器。
- web服务器端程序依据回传的会话标识号就知道以前已经为该客户端创建了HttpSession对象，不必再为客户端创建新的HttpSession对象。而是直接使用与该会话标识号匹配的HttpSession对象，通过这种方式就实现了对同一个客户端的会话状态的跟踪。

### Cookie和Session区别

![](code/imgs/session&cookie.png)

session和cookie同样都是针对单独用户对象，不同的用户在访问网站时，都会拥有各种的session或者cookie，不同用户之间互不干扰。
- 存储位置
- 生命周期

cookie和session区别
- cookie数据存放在客户的浏览器，session数据存放在服务器。
- cookie不是很安全，可以分析存放在本地COOKIE并进行COOKIE欺骗考虑安全应当使用session。
- session会在一定时间内保存在服务器。当访问增多，会占用服务器性能。考虑减轻服务器性能方面，应当使用COOKIE。
- 单个cookie在客户端的限制时3k。


---

![](code/imgs/spider12.jpg)

## 爬虫

### 什么是网络爬虫？

网络爬虫（又称为网页蜘蛛，网络机器人，在FOAF社区中间，更经常的称为网页追逐者），是一种按照一定的规则，自动地抓取万维网信息的程序或者脚本。另外一些不常使用的名字还有蚂蚁、自动索引、模拟程序或者蠕虫。

### 爬虫分类
1.通用网络爬虫(Scalable Web Crawler)
 
>通用网络爬虫的结构大致可以分为页面爬行模块、页面分析模块、链接过滤模块、页面数据库、URL 队列、初始 URL 集合
- 深度优先策略:按照深度又低到高的顺序，依次访问下一级网页链接，这种策略比较适合垂直搜索或站内搜索。
- 广度优先策略:按照网页内容目录层次深浅来爬行页面，处于较浅目录层次的页面首先被爬行。这种策略能够有效控制页面的爬行深度。

2.聚焦网络爬虫(Focused Crawler)

>选择性爬行那些与预先定义好的主题相关页面的网络爬虫
- 基于内容评价的爬行策略：将用户输入查询词作为主题，包含查询词的页面被视为与主题相关，起局限性在于无法评价页面与主题相关度的高低。

- 基于链接结构评价的爬行策略：通过计算每个已访问页面的Authority权重和Hub权重，并依次决定链接的访问顺序。

- 基于增强学习的爬行策略：Rennie 和 McCallum 将增强学习引入聚焦爬虫，利用贝叶斯分类器，根据整个网页文本和链接文本对超链接进行分类，为每个链接计算出重要性，从而决定链接的访问顺序。

- 基于语境图的爬行策略：通过建立语境图学习网页之间的相似度，训练一个机器学习系统，通过该系统可计算当前页面到相关Web页面的距离，距离越近的页面中的链接优先访问。

3.增量式网络爬虫(Incremental Web Carawler)

>指对已下载网页采取增量式更新和只爬行新产生的或者已经发生变化网页的爬虫，它能够在一定程度上保证所爬行的页面尽可能新的页面。

常用的方法有：
- 统一更新发
- 个体更新发
- 基于分类的更新法

4.Deep Web爬虫(Surface Web)
>Deep Web 是那些大部分内容不能通过静态链接获取的、隐藏在搜索表单后的，只有用户提交一些关键词才能获得的 Web 页面。
Deep Web 爬虫体系结构包含六个基本功能模块 （爬行控制器、解析器、表单分析器、表单处理器、响应分析器、LVS 控制器）和两个爬虫内部数据结构（URL 列表、LVS 表）。 其中 LVS（Label Value Set）表示标签/数值集合，用来表示填充表单的数据源。
 
 Deep Web爬虫爬行过程中表单填写类型：
- 基于领域知识的表单填写
- 基于网页结构分析的表单填写

### 爬虫核心
 1. 爬取网页数据
 2. 解析数据
 3. 爬虫和反爬之间的博弈

### 爬虫用途
 - 数据分析/数据源
 - 社交软件冷启动
 - 竞争对手监控
 - 舆情监控

![](code/imgs/spider-10.jpeg)

### 常见反爬手段
1. 反爬的原因
   - 爬虫占总PV（PV是指页面的访问次数，没打开或刷新一次页面，就算一个PV）比较高，浪费资源。
   - 公司免费查询资源被批量爬取，丧失竞争力。
   - 法律的灰色地带，状告爬虫成功机率小。
   - 成型的商业对手
    
2. 爬虫领域的概念
   - 爬虫：使用任何技术手段，批量获取网站信息的一种方式。
   - 反爬虫：使用任何技术手段，阻止别人批量获取自己网站信息的一种方式。
   - 误伤：在反爬虫的过程中，错误的将普通用户识别为爬虫。误伤率高的反爬虫策略，效果再好也不能用。
   - 拦截：成功地阻止爬虫访问。这里会有拦截率的概念。通常来说，拦截率越高的反爬虫策略，误伤的可能性就越高，因此需要做个权衡。
   - 资源：机器成本与人力成本的总和
    
3. 反爬虫的三个方向
   - 基于身份识别进行反爬
   - 基于爬虫行为进行反爬
   - 基于数据加密进行反爬
    
4. 常见基于身份识别进行反爬
   >请求参数的获取方法有很多，向服务器发送请求，很多时候需要携带请求参数，通常服务器端可以通过检查请求参数是否正确来判断是否为爬虫
   - 通过headers中User-Agent字段反爬 
     ```
      反爬原理：爬虫默认情况下没有User-Agent，而是使用模块默认设置
      解决办法：请求之前添加User-Agent
      ```
   - 通过referer字段或其他字段发爬
     ```
      反爬原理：爬虫默认情况不会带上referer字段，服务器端通过判断请求发起源头，判断请求是否合法
      解决办法：添加referer字段
     ```

   - 通过cookie反爬 
     ```
      反爬原理：通过检查cookies来查看发起请求的用户是否具备响应权限，以此进行反爬
      解决办法：进行模拟登录，成功获取cookies之后再进行数据爬取
      ```
   - 通过html静态文件中获取请求数据
     ```  
      反爬原理：通过增加获取请求参数的难度进行反爬
      解决办法：仔细分析抓包得到的每一个包，搞清楚请求之间的联系
       ```
   - 通过发送请求获取请求数据
     ``` 
      反爬原理：通过增加获取请求参数的难度进行反爬
      解决办法：仔细分析抓包得到的每一个包，搞清楚请求之间的联系，搞清楚请求参数的来源
     ``` 
   - 通过js生成请求参数
     ``` 
      反爬原理：js生成了请求参数
      解决办法：分析js、观察加密的实现过程，通过js2py获取js的执行结果，或者是selenium来实现
     ``` 
   - 通过验证码来反爬
     ``` 
      反爬原理：对方服务器通过弹出验证码强制验证用户浏览行为
      解决办法：打码平台或者机器学习的方法识别验证码
      ``` 
5. 常见基于爬虫行为进行反爬
   
   基于请求频率或者总请求数量
   > 爬虫的行为与普通用户有着明显的区别，爬虫的请求频率与请求次数要远高于普通用户
   - 通过请求IP/账号单位时间内总请求数量进行反爬
      ```
     反爬原理：正常浏览器请求网站，速度不会太快，同一个IP/账号大量请求了对方服务器，有更大的可能性会被识别为爬虫
     解决办法：对应的通过购买高质量的IP的方式能够解决问题/购买多个账号
      ```

   - 通过同一IP/账号请求之间的时间间隔进行反爬
      ```
      反爬原理：正常人操作浏览器浏览网站，请求之间的时间间隔是随机，而爬虫前后两个请求之间时间间隔通常比较固定，同时时间间隔较短，因此可以用来做反爬
      解决办法：请求之间进行随机等待，模拟真实用户操作，在添加时间间隔后，为了能够高速获取数据，尽量使用代理池，如果是账号，则将账号请求之间设置随机休眠
      ```

   - 通过对请求IP/账号每天请求次数设置阈值进行反爬
      ```
     反爬原理：正常的浏览行为，其一天的请求次数是有限的，通常超过某一个值，服务器就会拒绝响应
     解决办法：对应的通过购买高质量的IP的方法/多账号，同时设置请求间随机休眠
      ```
   
   - 通过js实现跳转来反爬
      ```
     反爬原理：js实现页面跳转，无法在源码中获取下一页url
     解决办法：多次抓包获取条状url，分析规律
     ```

   - 通过蜜罐（陷阱）获取爬虫IP（获取代理IP），进行反爬
      ```
     反爬原理：在爬虫获取链接进行请求的过程中，爬虫会根据正则、xpath、css等方式进行后续链接的提取，此时服务器端可以设置一个陷阱url，会被提前规则获取，但是正常用户无法获取，这样就能有效的区分爬虫和正常用户
     解决办法：完成爬虫的编写之后，使用代理批量爬取测试/仔细分析响应内容结构，找出页面中存在的陷阱
     ```

   - 通过假数据反爬
      ```
     反爬原理：向返回的响应中添加假数据污染数据库，通常假数据不会被正常用户看到
     解决办法：长期运行，核对数据库中数据同实际页面中数据对应情况，如果存在问题/仔细分析响应内容
     ```

   - 阻塞任务队列
      ```
     反爬原理：通过生成大量垃圾url，从而阻塞任务队列，降低爬虫的实际工作效率
     解决办法：观察运行过程中请求响应状态/仔细分析源码获取垃圾url生成规则，对URL进行过滤
     ```

   - 阻塞网络IO
      
        反爬原理：发送请求获取响应的过程实际上就是下载的过程，在任务队列中混入一个大文件的url，当爬虫在进行该请求时将会占用网络IO，如果是有多线程则会占用线程
        
        解决办法：观察爬虫运行状态/多线程对请求线程计时/发送请求线
 

   - 运维平台综合审计
      ```
     反爬原理：通过运维平台进行综合管理，通常采用复合型反爬虫策略，多种手段同时使用
     解决办法：仔细观察分析，长期运行测试目标网站，检查数据采集速度，多方面处理
     ```

6. 常见基于数据加密进行反爬
   
   对响应中含有的数据进行特殊化处理
    
    通常的特殊化处理主要指的就是css数据偏移/自定义字体/数据加密/数据图片/特殊编码格式等
   - 通过自定义字体来反爬，例如猫眼电影的评分
     ```
     反爬思路：使用自有字体文件
     解决办法：切换到手机版/解析字体文件进行翻译
     ```
   - 通过css来反爬
     ```
     反爬思路：源码数据不为真正的数据，需要通过css位移才能产生真正的数据
     解决办法：计算css的偏移
     ```     
   - 通过js动态生成数据进行反爬
     ```
     反爬原理：通过js动态生成
     解决办法：解析关键js，获得数据生成流程，模拟生成数据
     ```     
   - 通过数据图片化反爬
     ```
     反爬原理：58同城短租（https://baise.58.com/duanzu/3801871883498x.shtml）
     解决办法：通过使用图片解析引擎从图片中解析数据
     ```     
   - 通过编码格式进行反爬
     ```
     反爬原理：不适用默认编码格式，在获取响应之后通常爬虫使用utf-8格式进行解码，此时解码结果将会是乱码或者报错
     解决办法：根据源码进行多格式解码，或者真正的解码格式
     ```
     
- ###### 原文地址：https://blog.csdn.net/qq_43401941/article/details/109367328

>###### robots.txt协议：robots.txt(小写)是一种存放与根目录下的ASCCI编码的文本文件，它通常搞忘网络蜘蛛，此网站中的哪些内容是不应被网络蜘蛛爬取的，哪些是可以被网络蜘蛛爬取的。robots.txt协议并不是一个规范，而只是约定俗成的，所以并不能保证网站的隐私。

### Urllib

##### urllib库使用
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/03-crawler_urllib_baidu_page.py

##### 请求对象定制
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/03-crawler_urllib_request_Request.py
##### 编解码
- urllib.parse.quote()
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/04-crawler_urllib_parse_quote.py
- urllib.parse.urlencode()
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/04-crawler_urllib_parse_urlencode.py


##### ajax的get请求
>###### 案列:百度翻译(https://github.com/epover/python_crawler/blob/master/code/04-crawler_urllib_post_baidu.py)

##### ajax的post请求
>###### 案列:kfc官网(https://github.com/epover/python_crawler/blob/master/code/05-crawler_urllib_ajax_post_kfc.py)

##### URLError HTTPError
- HTTPError类是URLError类的子类
- 导入urllib.error.HTTPError urllib.error.URLError
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/05-crawler_urllib_except.py

##### cookie登录
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/05-crawler_urllib_cookie_login.py

##### Handler处理器
- 为什么要学习handler？
   - urllib.request.urlopen(url) 不能定制请求头
   - urllib.request.Request(url,headers,data)
   - Handler 可以定制更高级的请求头

##### 代理服务器（代理池）

- 代理的日常功能
  - 突破自身ip限制访问
  - 访问一些单位或团体内部资源
  - 提高访问速度
  - 隐藏真实ip
- 配置代理
  - 创建Request对象
  - 创建ProxyHandler对象
  - handler对象创建opener对象
  - opener.open发起请求
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/06-crawler_urllib_ProxyHandler.py
>###### 案列:https://github.com/epover/python_crawler/blob/master/code/06-crawler_urllib_ProxyHandler_proxypool.py


### 解析
xpath
- xpath插件安装（ctrl+shift+x）
- 安装lxml库（pip install lxml）
- 懒加载
- xpath使用
 ```python
from lxml import etree

# etree.parse()读取本地HTMl
tree = etree.parse('07-xpath_test.html')

# 获取body/ul/li标签的内容
list_li = tree.xpath('body/ul/li/text()')

# 获取body/ul/li标签id为l1的内容
list_li = tree.xpath('//ul/li[@id="l1"]/text()')

# 获取body/ul/li标签有id属性的内容
list_li = tree.xpath('//ul/li[@id]/text()')

# 获取body/ul/li标签有class属性的内容
list_li = tree.xpath('//ul/li[@class]/text()')

# 获取body/ul/li标签有id和有class属性的内容
list_li = tree.xpath('//ul/li[@id and @class]/text()')

# 获取body/ul/li标签的id以l开头的内容
list_li = tree.xpath('//ul/li[starts-with(@id,"l")]/text()')

# 获取body/ul/li标签的id包含l的内容
list_li = tree.xpath('//ul/li[contains(@class,"c")]/text()')

# 获取body/ul/li标签id为l1的class属性值
attribute = tree.xpath('//ul/li[@id="l1"]/@class')
 ```
>###### 案列：抓取站长素材前10页图片(https://github.com/epover/python_crawler/blob/master/code/07-crawler_xpath_imgs.py)

JsonPath
- pip install jsonpath
- JsonPath使用
 ```python
import json,jsonpath

object = json.load(open('08-crawler_jsonpath.json','r',encoding='utf-8'))

# 获取所有书的作者
author_list = jsonpath.jsonpath(object,'$.store.book[*].author')
print(author_list)

# 所有的作者
author_list = jsonpath.jsonpath(object,'$..author')
print(author_list)

# store下面的所有元素
tag_list = jsonpath.jsonpath(object,'$.store.*')
print(tag_list)

# store里面所有东西的price
price = jsonpath.jsonpath(object,'$.store..price')
print(price)

# 第三本书
book = jsonpath.jsonpath(object,'$..book[2]')
print(book)

# 最后一本书
book = jsonpath.jsonpath(object,'$..book[(@.length-1)]')
print(book)

# 前面两本书
book_list = jsonpath.jsonpath(object,'$..book[:2]')
book_list = jsonpath.jsonpath(object,'$..book[0,1]')
print(book_list)

# 过滤出所有包含isbn的书
book_list = jsonpath.jsonpath(object,'$..book[?(@.isbn)]')
print(book_list)

# 哪本书价格超过了10快
book_list = jsonpath.jsonpath(object,'$..book[?(@.price>10)]')
print(book_list)
 ```

>###### 案列：淘票票城市数据(https://github.com/epover/python_crawler/blob/master/code/08-crawler_jsonpath_taopiaopiao_city.py)

BeautifulSoup（bs4）
- pip install bs4
- bs4使用
 ```python
from bs4 import BeautifulSoup

# 通过bs4解析本地文件,默认读取文件方式为gbk,所以需要指定utf-8编码
soup = BeautifulSoup(open('09-crawler_bs4.html','r',encoding='utf-8'),'lxml')

# 根据标签名查找到第一个符合条件的数据
print(soup.a)

# 获取标签的属性和属性值
print(soup.a.attrs)

# bs4的一些函数
# (1) find
# 返回的是第一个符合条件的数据
print(soup.find('li'))

# 根据title的值来找到对应的标签
print(soup.find('a',title='a2'))

# 根据class属性值来找到对应的标签,class语法已经纯在所以需要添加_
print(soup.find('p',class_='p1'))

# (2)find_all 返回所有匹配的标签list
print(soup.findAll('li'))

# 查找多个标签
print(soup.findAll(['a','span']))

# limit的作用是查找的前几个数据
print(soup.findAll('li',limit=2))

# (3) select
# select查询返回一个list
print(soup.select('li'))

# 通过类选择器
# 查找class属性为a1的标签
print(soup.select('.a1'))
print(soup.select('#l2'))

# 属性选择器
# 查找li标签中id=l2的标签
print(soup.select('li[id="l2"]'))

# 查找li标签中有id的标签
print(soup.select('li[id]'))

# 层级选择器
# 后代选择器
# 找到div下面的li
print(soup.select('div li'))

# 子代选择器:某标签的第一级标签
print(soup.select('div > ul > span'))

# 找到a标签和li标签的所有对象
print(soup.select('a,li'))

# 节点信息
tag_l1 = soup.select('#l1')[0]
print(tag_l1)

# 获取标签内容
print(tag_l1.get_text())

# 获取标签对象中，存在标签那么string就获取不到数据，使用get_text()就能获取数据
print(tag_l1.string)

# 节点属性
tag_obj = soup.select('#p1')[0]
print(tag_obj.name)
print(tag_obj.attrs)soup = BeautifulSoup(response.read().decode(),'lxml')
 ```
>###### 案列：星巴克菜单图片数据(https://github.com/epover/python_crawler/blob/master/code/09-crawler_bs4_starbucks_menu_picture.py)

### selenium

1.什么是selenium？
- Selenium是一个用于Web应用程序测试的工具
- Selenium测试直接运行在浏览器中，模拟用户真实操作浏览器
- 支持各种driver（FirfoxDriver,IternetExplorerDriver,ChromeDriver等）驱动
- selenium支持无界面浏览器操作

2.为什么使用selenium？
 
模拟浏览器功能，自动执行网页中的js代码，实现动态加载

3.安装selenium
- ChromeDriver（http://chromedriver.storage.googleapis.com/index.html）
- pip install selenium

4.selenium使用步骤
```python
from selenium import webdriver

path = 'chromedriver.exe'
browser = webdriver.Chrome(path)
browser.get("https://www.baidu.com/")

# 通过id定位元素
button = browser.find_element_by_id("su")
print(button)

# 通过name定位元素
name = browser.find_elements_by_name("wd")
print(name)

# 通过xpath语发定位元素
img_xpath =  browser.find_elements_by_xpath("//div[@id='wrapper']//input")
print(img_xpath)

# 通过标签名定位元素
tag_name = browser.find_elements_by_tag_name('input')
print(tag_name)

# 通过css属性定位元素
css_selector = browser.find_elements_by_css_selector('#kw')
print(css_selector)

# 通过超链接文本定位
link_text = browser.find_element_by_link_text('贴吧')
print(link_text)

# 获取元素文本
print(link_text.text)

# 获取元素属性
print(link_text.get_attribute('class'))

# 获取标签名
print(link_text.tag_name)
# 关闭浏览器
browser.quit()
```

### Phantomjs
- 无界面浏览器
- 支持页面元素查找，js代码运行
- 由于不进行css和gui渲染，运行效率高
```python
from selenium import webdriver

browser = webdriver.PhantomJS('phantomjs.exe')
browser.get('https://www.baidu.com/')
# 保存屏幕快照
browser.save_screenshot('baidu.png')
browser.find_element_by_id('kw').send_keys('刘德华')
browser.find_element_by_id('su').click()
browser.save_screenshot('刘德华.png')
browser.quit()
```

### Chrome handless
- chrome-headless模式
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

def share_browser():
    chrome_options = Options()
    chrome_options.add_argument('‐‐headless')
    chrome_options.add_argument('‐‐disable‐gpu')
    path = r"C:\Program Files\Google\Chrome\Application\chrome.exe"
    chrome_options.binary_location = path
    return webdriver.Chrome(chrome_options=chrome_options)

browser = share_browser()
browser.get('https://www.baidu.com/')
```




