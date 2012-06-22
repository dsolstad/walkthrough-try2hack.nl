#### Level 11

This PHP script seemed to work nice:
```php
<?php

$url = "http://www.try2hack.nl/levels/level11-vmituh.xhtml";
$html = file_get_contents($url);
$x = 1;
if (preg_match("/positions\s(.*?)\sand\s(\d+)\s/", $html, $m )) {
   $pos = explode(',', trim($m[1]));
   $pos[] = trim($m[2]);
}
if (preg_match("/:<br \/>(.*?)<hr \/>/", $html, $m )) {
    $string = trim($m[1]);
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
curl_close($ch);
print preg_replace('/.*?&lt;!-- content --&gt;\s+(.*?.)?&lt;b.*href="(\S+)".*/s', '$1 Next level: $2', $response) . "\n";

?>
```

```
user@ubuntu:~$php try2hack_level11_solution.php
Congratulations, you have passed Level 11. Next level: level12-kvdsju.xhtml
```