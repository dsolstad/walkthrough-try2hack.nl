#### Level 6

Download and install Wireshark (you will find it easily on google) or any other packet sniffer. Start capturing.
Now open LEVEL6.EXE and type a random username and password. Now you will se that you got some packages in Wireshark.
One of them contain this data:
```
(ENCRYPTION TYPE)
B*C*N**N

(USERNAME)
aaabb aaaaa aaaab abbab ababb aaaab

(PASSWORD)
aabaa abbaa aaaba baaaa babba abbba baaba abaaa abbab abbaa baaaa aaaaa babaa abaab baaab

(PAGE)
babab aabab abaab abbab aabbb aaaba
```

I googled B*C*N**N and found http://www.bbc.co.uk/dna/h2g2/A9837183

Then I made this php script:
```php
<?php

$x = array('aaaaa', 'aaaab', 'aaaba', 'aaabb', 'aabaa', 'aabab', 'aabba', 'aabbb', 'abaaa', 
           'abaaa', 'abaab', 'ababa', 'ababb', 'abbaa', 'abbab', 'abbba', 'abbbb', 'baaaa', 
           'baaab', 'baaba', 'baabb', 'baabb', 'babaa', 'babab', 'babba', 'babbb');

$y = range('a','z');

$s  = "Username:aaabb aaaaa aaaab abbab ababb aaaab" . "\n";
$s .= "Password:aabaa abbaa aaaba baaaa babba abbba baaba abaaa abbab abbaa baaaa aaaaa babaa abaab baaab" . "\n";
$s .= "url:babab aabab abaab abbab aabbb aaaba";

$s = str_replace($x, $y, $s);
$s = str_replace(' ', '', $s);
print $s;

?>
```
