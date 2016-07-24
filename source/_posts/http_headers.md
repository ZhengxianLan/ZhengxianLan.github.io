---
title: http_headers
layout: post
date: 2016-06-11 10:24:58
categories: ['http']
tags: ['headers']
---

```
POST /search HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0 Galeon/1.2.5 (X11; Linux i686; U;) Gecko/20020606
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,
        text/plain;q=0.8,video/x-mng,image/png,image/jpeg,image/gif;q=0.2,
        text/css,*/*;q=0.1
Accept-Language: en
Accept-Encoding: gzip, deflate, compress;q=0.9
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66
Keep-Alive: 300
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 31

hl=en&q=HTTP&btnG=Google+Search
```

- request line:
` POST /search HTTP/1.1 `
- request headers:
```
Host: www.google.com
User-Agent: Mozilla/5.0 Galeon/1.2.5 (X11; Linux i686; U;) Gecko/20020606
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,
        text/plain;q=0.8,video/x-mng,image/png,image/jpeg,image/gif;q=0.2,
        text/css,*/*;q=0.1
Accept-Language: en
Accept-Encoding: gzip, deflate, compress;q=0.9
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66
```
- general headers:
```
Keep-Alive: 300
Connection: keep-alive
```
- entity headers:
```
Content-Type: application/x-www-form-urlencoded
Content-Length: 31
```
- content
```
hl=en&q=HTTP&btnG=Google+Search
```

