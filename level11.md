#### Level 11

This PHP script seemed to work nice:
```php
<?php

$url = "http://www.try2hack.nl/levels/level11-vmituh.xhtml";
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$html = curl_exec($ch);

$x = 1;
if (preg_match("/positions\s(.*?)\sand\s(\d+)\s/", $html, $m)) {
   #print_r($m);
   $pos = explode(',', trim($m[1]));
   $pos[] = trim($m[2]);
} else {
    print 'No positions found';
}

if (preg_match("/:<br \/>(.*?)<hr \/>/", $html, $m )) {
    $string = trim($m[1]);
} else {
    print "Found no random string";
}

foreach ($pos as $key => $value) {
   $x = bcmul($x, ord($string[$value - 1]));
}

$postvalue =  substr($x, 0, 5);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, 'answer=' . $postvalue . '&submit=click here to continue');
$response = curl_exec($ch);
print $response;
curl_close($ch);


?>
```

```
user@ubuntu:~$php try2hack_level11_solution.php
Congratulations, you have passed Level 11. Next level: level12-kvdsju.xhtml
```
