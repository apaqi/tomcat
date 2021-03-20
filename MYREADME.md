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

##处理请求过程
RequestFacade-->StandardWrapper-->HttpServlet-->Servlet-->StandardWrapper-->
StandardWrapperValve#invoke()-->ApplicationFilterChain#doFilter[servlet.service()]-->
Servlet-->,,,
重点：RequestFilterValve 实现此接口可以实现自己管道阀门，可以放在Host配置下，也可以放在context配置下。

##接请求过程
http协议 -->浏览器\tomcat(Connector,IO模型 => AbstractEndpoint 子类-->Acceptor-->Nio2Endpoint(Nio2Acceptor)-->
接口--socket---核心方法
        ||
        \/
tcp_connect-->(数据，linux,操作系统)
        ||
        \/
TCP协议  -->操作系统


##安全机制
>使用安全机制启动tomcat,catalina.bat -security  （conf/catalina.policy ,SecurityManager） ,防止恶意代码。可参考：FileInputStream
> 
>https://blog.csdn.net/qpzkobe/article/details/78792872
>
##热部署




