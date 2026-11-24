# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| **Прізвище, ім'я** |Мелькумов Тимур |
| **Група** |ЗКІ-21 |
| **Номер варіанта** | 20 |
| **Домен варіанта** | w3.org|
| **Середовище виконання** | PowerShell |
| **Дата виконання** | 24.11.2026|

## Частина A. Збір експериментальних даних

# A.1. Формування запиту вручну
     
     
   **Команда:**
    
       $d = "w3.org"
       >> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
       >> $s = $c.GetStream()
       >> $w = New-Object System.IO.StreamWriter($s)
       >> $w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
       >> $w.Flush()
       >> (New-Object System.IO.StreamReader($s)).ReadToEnd()
       >> $c.Close()
       
       
   **Вивід:**
      
     
      HTTP/1.1 301 Moved Permanently
      Date: Tue, 24 Nov 2026 16:34:46 GMT
      Content-Type: text/html; charset=UTF-8
      Transfer-Encoding: chunked
      Connection: close
      Location: https://www.w3.org/
      set-cookie: __cf_bm=7h4SMM7ZAyedtWFdAkXznjnc0f7RGbfzxd8jzcwzaS8-1795538086.5160668-1.0.1.1-S75mkwx6tC6DcAWo4rqjiAu2qQdBCQWNJbPb4yFLLjbKh0xQQhiDlJs_u_eHsDIt5pQds9TPd3.jgZztolFfe4kk1mJKEbPXSkFAn8ZCxESyxgdLSMYkhGxVFpOXzUnQ; HttpOnly; Path=/; Domain=w3.org; Expires=Tue, 24 Nov 2026 17:04:46 GMT
      X-Content-Type-Options: nosniff
      Server: cloudflare
      CF-RAY: a5f9d2b0be982761-WAW
      alt-svc: h3=":443"; ma=86400
```
### A.2. Запит без поля Host у версії 1.1

   **Команда:**
      $c = "w3.org"
     >> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
     >> $s = $c.GetStream()
     >> $w = New-Object System.IO.StreamWriter($s)
     >> $w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
     >> $w.Flush()
     >> (New-Object System.IO.StreamReader($s)).ReadToEnd()
     >> $c.Close()
     
    **Вивід:**
     HTTP/1.1 400 Bad Request
     Server: cloudflare
     Date: Tue, 24 Nov 2026 16:39:43 GMT
     Content-Type: text/html
     Content-Length: 155
     Connection: close
     CF-RAY: -
     
     <html>
     <head><title>400 Bad Request</title></head>
     <body>
     <center><h1>400 Bad Request</h1></center>
     <hr><center>cloudflare</center>
     </body>
     </html>
     
 ### A.3. Вплив поля Host на відповідь сервера
  # A.3.1. Чуже доменне ім'я в полі Host
   
   **Команда:**
    $c = "w3.org"
    >> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
    >> $s = $c.GetStream()
    >> $w = New-Object System.IO.StreamWriter($s)
    >> $w.Write("GET / HTTP/1.1`r`nHost: iana.org`r`nConnection: close`r`n`r`n")
    >> $w.Flush()
    >> (New-Object System.IO.StreamReader($s)).ReadToEnd()
    >> $c.Close()
    >>
   
    **Вивід:**
    
 
    HTTP/1.1 530 <none>
    Date: Tue, 24 Nov 2026 16:42:16 GMT
    Content-Type: text/plain; charset=UTF-8
    Content-Length: 17
    Connection: close
    Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
    Expires: Thu, 01 Jan 1970 00:00:01 GMT
    Referrer-Policy: same-origin
    X-Frame-Options: SAMEORIGIN
    Server: cloudflare
    CF-RAY: a5f9ddaf4d9df9da-WAW
    alt-svc: h3=":443"; ma=86400
    
    error code: 1016

# A.3.2. Неіснуюче ім'я в полі Host

**Команда:**
$c = "w3.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()

**Вивід:**

HTTP/1.1 409 Conflict
Date: Tue, 24 Nov 2026 16:45:44 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a5f9e2c16a573560-WAW

error code: 1001

### A.3.3. Запит без поля Host у версії 1.0
**Команда:**
 $c = "w3.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.0`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()
>> 
**Вивід:**

###HTTP/1.1 403 Forbidden
Date: Tue, 24 Nov 2026 16:48:21 GMT
Content-Length: 57
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Referrer-Policy: same-origin
Expires: Thu, 01 Jan 1970 00:00:01 GMT
CF-RAY: a5f9e697eb34eeb9-WAW

### A.4. Два запити в одному з'єднанні
**Команда:**
$c = "w3.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()

**Вивід:**


HTTP/1.1 301 Moved Permanently
Date: Tue, 24 Nov 2026 16:53:22 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Location: https://www.w3.org/opism-pr02-12345
set-cookie: __cf_bm=nQNXP0J.5H_4ePTznzxsKRccMPtHcIP9pHhDuvHVYso-1795539202.2463005-1.0.1.1-PM38zIdBRZpaeA10L8PUbPaV3uYgPLiXtmAUTaCiebt7.p31xMfWTXPth_amGTbGrzalS7GXueaPVwXth8rvxoQbDA2DTBJnabgmAIru1M65VO.SMTO21ylS7WK6thhn; HttpOnly; Path=/; Domain=w3.org; Expires=Tue, 24 Nov 2026 17:23:22 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a5f9edee0c6beed1-WAW
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0

HTTP/1.1 301 Moved Permanently
Date: Tue, 24 Nov 2026 16:53:22 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Location: https://www.w3.org/
set-cookie: __cf_bm=6ngZq5ELnrnm3kdcrF6URof3FD0Xxi06KGTorZ0ZUD8-1795539202.260106-1.0.1.1-Ij5YhlFEJ31gsi7xrqPHSA0gVJ9A5H1qjql3E3DY_XukrqUE9miMWCqOYQ0yweHnTJ_r9HXznwiS_sDPrEmABGuFh2LZ0O8XWIDXHnKRUxe78rkK1sukZLsu7OvEFxSq; HttpOnly; Path=/; Domain=w3.org; Expires=Tue, 24 Nov 2026 17:23:22 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a5f9edee1c86eed1-WAW
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0

### A.5. Запит за допомогою клієнтської програми
**Команда:**
curl.exe -v --http1.1 http://w3.org/ -o NUL

**Вивід:**
* Host w3.org:80 was resolved.
* IPv6: (none)
* IPv4: 104.18.23.19, 104.18.22.19
*   Trying 104.18.23.19:80...
* Established connection to w3.org (104.18.23.19 port 80) from 192.168.0.102 port 57515
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
  0      0   0      0   0      0      0      0                              0* using HTTP/1.x
> GET / HTTP/1.1
> Host: w3.org
> User-Agent: curl/8.22.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Tue, 24 Nov 2026 16:56:46 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Location: https://www.w3.org/
< set-cookie: __cf_bm=X5fxJuLecYKEYhiCyCkXnIx6ImzBZ5a3tNxSkBl5dvw-1795539406.8456116-1.0.1.1-5MgGRqviLOU3owjeeDrXoRwEYt78kiG9woum48od6rKemxwJsr0RsyJSVaR5SNOgnmqM63V45vqpuMahPPw64Kf_vk3ma7gOrtF8y_.uebLmyNKnm.cWPdVpd1sUZ4hR; HttpOnly; Path=/; Domain=w3.org; Expires=Tue, 24 Nov 2026 17:26:46 GMT
< X-Content-Type-Options: nosniff
< Server: cloudflare
< CF-RAY: a5f9f2ecc9f4ecc3-WAW
< alt-svc: h3=":443"; ma=86400
<
{ [178 bytes data]
100    167   0    167   0      0   1321      0                              0
* Connection #0 to host w3.org:80 left intact


### A.6. Додаткови дії при отриманні коду стану класу 3xx
Виконується на iana.org

**Команда:**
 $d = "iana.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 443)
>> $ssl = New-Object System.Net.Security.SslStream($c.GetStream(), $false)
>> $ssl.AuthenticateAsClient($d)
>> $w = New-Object System.IO.StreamWriter($ssl)
>> $w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($ssl)).ReadToEnd()
>> $c.Close()

**Вивід:**
HTTP/1.1 301 Moved Permanently
Date: Tue, 24 Nov 2026 17:06:25 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 28 Nov 2026 17:06:25 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
