# 0 爬虫准备工作
- 参考资料
    - python网络数据采集， 图灵工业出版
    - 精通Python爬虫框架Scrapy， 人民邮电出版社
    - [Python3网络爬虫](https://blog.csdn.net/c406495762/article/details/72858983)
    - [Scrapy官方教程](https://scrapy-chs.readthedocs.io/zh_CN/0.24/intro/tutorial.html)
- 前提知识
    - url
    - http协议
    - web前端，html, css, js
    - ajax
    - re, xpath
    - xml
# 1. 爬虫简介
- 爬虫定义：网络爬虫（又被称为网页蜘蛛，网络机器人，在FOAF社区中间，更经常的称为网页追逐者）， 
是一种按照一定的规则，自动地抓取万维网信息的程序或者脚本。 
另外一些不常使用的名字还有蚂蚁、自动索引、模拟程序或者蠕虫。
- 两大特征
    -能按作者要求下载数据或者内容
    -能自动在网络上流窜
- 三大步骤：
    - 下载网页
    - 提取正确的信息
    - 根据一定规则自动跳到另外的网页上执行上两步内容

- 爬虫分类
    - 通用爬虫
    - 专用爬虫(聚焦爬虫)  
- Python网络包简介
    - Python2.x：urllib, urllib2, urllib3, httplib, httplib2, requests
    - Python3.x: urllib, urllib3, httplib2, requests
    - python2: urllib和urllib2配合使用，或者requests
    - Python3： urllib，requests    
# 2.urllib       
- 包含模块
    - urllib.request：打开和读取urls
    - urllib.error：包含urllib.reuqest产生常见的错误，使用try捕捉
    - urllib.parse：包含解析url的方法
    - urllib.robotparse：解析robots.txt文件
    
- 网页编码问题解决
    - chardet 可以自动检测页面文件的编码格式，但是，可能有误
    - 需要安装， conda install chardet
    - 案例v2    
    
- urlopen 的返回对象
    - 案例v3
    - geturl: 返回请求对象的url
    - info: 请求反馈对象的meta信息
    - getcode：返回的http code    
- request.date的使用
    - 访问网络的两种方法
        - get:
            - 利用参数给服务器传递信息，
            - 参数为dict，然后用parse编码
            - 案例v4
        - post    
            - 一般向服务器传递参数使用
            - post是把信息自动加密处理
            - 我们如果想要使用post信息，需要用到data参数
            - 使用post，意味着http的请求可能需要更改
                - Content-Type:application/x-www.form-urlencode
                - Content-Length:数据长度
                - 简而言之，一旦更改请求方法，请注意其他请求头部信息相适应
             - urllib.parse.urlencode可以将字符串自动转换成上面的
            - 案例v05
            - 为了更多的设置请求信息，单纯的通过urlopen函数已经不太好用
            - 需要利用request.Request类
            - 案例v06
- urllib.error
    - URLError产生的原因
        - 没网
        - 服务器连接失败 
        - 找不到指定服务器
        - 是OSError子类
        - 案例v07
    - HTTPError，是URLError的一个子类
        - 案例v08
    
    - 两者的区别：
        - HTTPError是对应HTTP请求的返回码错误，如果返回错误码是404以上，则引发HTTPError
        - URLError对应的一般是网络出现问题，包括url问题
        - 关系区别：OSError-URLError-HTTPError
- UserAgent
    - UserAgent:用户代理，简称UA，输入heads的一部分，服务器通过UA来判断访问者的身份
    - 常见的UA值，使用的时候可以直接复制粘贴，也可以用浏览器访问的时候抓包
                                   
             1.Android
             
              Mozilla/5.0 (Linux; Android 4.1.1; Nexus 7 Build/JRO03D) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Safari/535.19
              Mozilla/5.0 (Linux; U; Android 4.0.4; en-gb; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30
              Mozilla/5.0 (Linux; U; Android 2.2; en-gb; GT-P1000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1
            
              2.Firefox
            
              Mozilla/5.0 (Windows NT 6.2; WOW64; rv:21.0) Gecko/20100101 Firefox/21.0
              Mozilla/5.0 (Android; Mobile; rv:14.0) Gecko/14.0 Firefox/14.0
            
              3.Google Chrome
            
              Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36
              Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile Safari/535.19
            
              4.iOS
            
              Mozilla/5.0 (iPad; CPU OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3
              Mozilla/5.0 (iPod; U; CPU like Mac OS X; en) AppleWebKit/420.1 (KHTML, like Gecko) Version/3.0 Mobile/3A101a Safari/419.3
              
    - 设置UA可以通过两种方式：
        - heads
        - add_header
        - 案例v09

- ProxyHandler处理(代理服务器)
    - 使用代理IP，是爬虫的常用手段           
    - 获取代理服务器的地址：
        - www.xicidaili.com
        - www.goubanjia.com
    - 代理用来隐藏正式访问中，代理也不允许频繁访问一个固定网站，所以，代理一定要很多很多
    - 基本使用步骤：
        1.设置代理地址
        2.创建ProxyHander
        3. 创建Opener
        4. 安装Opener       
    - 案例v10
    
- cookei & session
    - 由于http协议的无记性，人们为了弥补这个缺憾，所采用的的一个补充协议
    - cookie是发送给客户(即http浏览器)的一段信息，session是保存在服务器上的对应的另一半信息，用来记录用户信息
    
- cookie和session的区别
    - 存放位置不同
    - cookie不安全
    - session会保存在服务器一定时间
    - 单个cookie保存数据不超过4k,很多浏览器限制一个站点最多保存20个
- session的存放位置
    - 存在服务器端
    - 一般情况下，session是放在内存中或者数据库中    
    - 没有cookie登录 案例v11，可以看到，没使用cookie则返回网页为未登陆状态
- 使用cookie登录
    - 直接把cookie复制下来，然后手动放入请求头，案例v12
    - http模块包含一些关于cookie的模块，通过他们我们可以自动使用cooike
        - CookieJar
            - 管理存储cookie，向传出的http请求添加cookie
            - cookie存储在内容中，CookieJar实例回收后cookie将消失
        - FileCookieJar(filename,delayload=None,policy=None):
            - 使用文件管理cookie
            - filename是保存cookie的文件
        - MozillacCookieJar
            - 创建mocilla浏览器cookie.txt兼容的FileCookieJar实例 
        - LwpCookieJar
            - 创建与libwww-perl标准兼容的Set-Cookie3格式的FileCookieJar实例
        - 他们的关系是：CookieJar-->FileCooikeJar-->MozillacCookieJar & LwpCookieJar
    - 利用cookiejar访问人人，案例v13 
        - 自动使用cookie登录，大致流程是
        - 打开登录页面后自动通过用户密码登录
        - 自动提取反馈回来的cookie
        - 利用提取的cookie登录隐私页面
    - handler是Handler的实例，常用的有
        - 用来处理复杂请求
        
              # 生成 cookie的管理器
              cookie_handler = request.HTTPCookieProcessor(cookie)
              # 创建http请求管理器
              http_handler = request.HTTPHandler()
              # 生成https管理器
              https_handler = request.HTTPSHandler()
    - 创建handler后，使用opener打开，打开后相应的业务由相应的handler处理
    - cookie作为一个变量，打印出来，案例v14     
        - cookie的属性
            - name:名称
            - value:值
            - domain:可以访问此cookie的域名  
            - path:可以发布此cookie的页面路径
            - expires:过期日期
            - size:大小
            - Http字段  
    - cookie的保存-FileCookieJar，案例v15
    - cookie的读取，案例v16      

- SSL
    - SSL证书就是指遵守安全套阶层协议的服务器数字证书(SercureSocketLayer)
    - 美国网景公司开发
    - CA(CertifacateAuthority)是数字证书 认证中心，是发放，管理，废除数字证书的收信人第三方机构
    
- js加密
    - 有的反爬虫策略采用js对需要传输的数据进行加密处理(通常是取md5值)
    - 经过加密，传输的就是密文，但是
    - 加密函数或者过程一定是在浏览器完成，也就是一定会把代码(js代码)暴露给使用者
    - 通过阅读加密算法，就可以模拟出加密过程，从而达到破解
    - 过程参看案例v18,v19 笔记比较少，仔细观察
- ajax
    - 异步请求
    - 一定会有url，请求方法，可能有数据
    - 一般使用json格式
    - 案例，爬去豆瓣电影， 案例v20    

# Requests-献给人类
- HTTP for Humans，更简洁更友好
- 继承了urllib的所有特征
- 底层使用的是urllib3
- 开源地址:[https://github.com/requests/requests]
- 中文文档:[ http://docs.python-requests.org/zh_CN/latest/index.html]
- 安装： conda install requests
- get请求
    - requests.get(url)
    - requests.request("get",url)
    - 可以带有headers和params参数
    - 案例v21
- get返回内容
    - 案例v22
                   
- post 
    - rsp = requests.post(url, data=data)
    - 参看案例v23           
    - data,headers要求dict类型            
- proxy     


            pxrxies = {
            "http":"address of proxy"
            "http":"address of proxy"
            }
            rsp = requests.request("get","http:xxxxxx",proxies=proxies)
  - 代理有可能报错，如果使用人数多，考虑安全问题，可能会被强制关闭
  
- 用户验证
    - 代理验证
    
            # 可能需要使用HTTP basic Auth·可以选择
            # 格式为 用户名:密码@代理地址:端口地址
            proxy = {"http":"china:123456@192.168.1.123"}
            rsp = requests.get("http://baidu.com", proxies=proxy)
- web客户端验证
    - 如果遇到web客户端验证·需要添加auth=(用户名·密码)
        auth=("test1","123456")#授权信息
        rsp = request.get("http://www.baidu.com", auth = auth)
- cookie
    - requests可以自动处理cookie信息
        
        
             rsp = requests.get("http://xxxxxxxxxxx")
            # 如果对方服务器给传送过来cookie信息，则可以通过反馈的cookie属性得到
            # 返回一个cookiejar实例
            cookiejar = rsp.cookies   
            
            #可以讲cookiejar转换成字典
            cookiedict = requests.utils.dict_from_cookiejar(cookiejar) 

- session
    - 跟服务器端session不是一个东东
    - 模拟一次会话，从客户端浏览器链接服务器开始，到客户端浏览器断开
    - 能让我们跨请求时保持某些参数，比如在同一个session实例发出的 所有请求之间保持cookie
        
        
            # 创建session对象·对象保持cookie值
            ss = requests.session()
            
            headers = {"User-Agetn":"xxxxxxxxxxxxxxxx"}
            
            data = {"name":"xxxxxxxx"}
            
            # 此时·由创建的session管理请求·负责发出请求
            ss.post("http://www.baidu.com",data = data,headers = headers)
            
            rsp = ss.get("xxxxxxxx")
            
-  https请求验证ssl证书
    - 参数verify负责表示是否需要验证ssl证书·默认是True
    - 如果不需要验证ssl证书·则设置成False表示关闭 
    
    
        rsp = requests.get("https://www.baidu.com", verify=False)
        # 如果用verify=True访问12306，会报错，因为他证书有问题                                                  