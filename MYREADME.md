## Welcome to Apache Tomcat!
## https://seaboat.blog.csdn.net/article/list/11 << tomcat 内核设计剖析>>
## https://www.cnblogs.com/kismetv/p/7228274.html


##B站课程
##https://www.yuque.com/books/share/9f4576fb-9aa9-4965-abf3-b3a36433faa6/ostorw   密码： ggmc
>https://www.bilibili.com/video/BV1jJ41197va?p=7&spm_id_from=pageDriver
##一：部署方式 HostConfig#deployApps()
>1.xml 配置方式（在解析完server.xml配置，通过事件的方式拉起服务。）。
>
>2.war 发布方式，参考HostConfig（在解析完server.xml配置，通过事件的方式拉起服务。）
>
>3.资源描述方式，比如jar部署方式（在解析完server.xml配置，通过事件的方式拉起服务。）
>
>4.在server.xml 的 host节点中配置context 节点信息。（在解析server.xml配置的时候就拉起服务。）

Pipeline：管道，每个容器都有一个对应的管道。每个管道都有一些阀门（Valve），及server.xml中的Valve节点配置。
每个容器都有一个对应的标准实现，即StandardXXX,e.g： StandardEngine、StandardWrapper。。。
重点：RequestFilterValve 实现此接口可以实现自己管道阀门，可以放在Host配置下，也可以放在context配置下。

Engine容器
    --List<Host>
    --Pipeline pipeline
        --List<Value>
Host容器
    --List<Context>
    --Pipeline pipeline
            --List<Value>
Context容器
    --List<Wrapper>
    --Pipeline pipeline
            --List<Value>  
    说明：context--应用--容器
Wrapper (SingleThreadModel )--servlet类
    --List<Servlet> servlets;
    --Servlet
    --Pipeline pipeline
            --List<Value>

##处理请求过程
Request->StandardWrapper-->HttpServlet-->Servlet-->StandardWrapper-->
StandardWrapperValve#invoke(),wrapper.allocate（生成servlet实例）-->ApplicationFilterChain#doFilter(RequestFacade request)[servlet.service，调的是父类httpServlet中的service方法]-->
Servlet-->,,,
注意：ApplicationFilterChain#doFilter 中将tomcat的Request转换为了RequestFacade对象。

##接请求过程（请求-->Request的过程）
http协议（应用层协议） ，解析过程：
    socket---http协议--解析---Request

socket---TCP:解析过程：
    TCP协议--传输--操作系统--tcp_connect（）
    
http协议（应用层协议） -->浏览器\tomcat(Connector（设置协议）,IO模型 => Http11AprProtocol(Http11AprProtocol构造函数创建JioEndpoint)-->AbstractEndpoint（提供不同的NIO模型） 子类-->Acceptor-->Nio2Endpoint(Nio2Acceptor)-->Http11Processor
TCP协议接口--Socket（核心方法）-->JDK-->操作系统
        ||
        \/
tcp_connect-->(数据，linux,操作系统)
        ||
        \/
TCP协议（传输数据）  -->操作系统（linux: tcp_output.c  window:）


##请求解析
Http11AprProtocol(父类AbstractHttp11Protocol# createProcessor())-->Http11Processor 
##安全机制
>使用安全机制启动tomcat,catalina.bat -security  （conf/catalina.policy ,SecurityManager） ,防止恶意代码。可参考：FileInputStream
> 
>https://blog.csdn.net/qpzkobe/article/details/78792872
>
##热部署




