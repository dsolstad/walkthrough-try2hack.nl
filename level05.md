#### Level 5

Download the file zip-file and extract it.

Download vbrun300.dll from http://www.dll-files.com/dllindex/dll-files.shtml?vbrun300 and place it in the same folder as LEVEL5.EXE

Get the Dodi VB decompiler from http://vbdis4.angelfire.com

Now try to open LEVEL5.EXE, you will probably get some errors, but that's OK. Now you will find some new files in you level5 folder.
The important files are LEVEL5.bas and main.txt

main.txt:
```basic
Global Const gc0006 = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ.,:;-*+=~|&!_$#@()[]{}<\/>"
Global Const gc000A = "http://www.try2hack.nl/levels/level6-ksghvb.xhtml"
```
LEVEL5.bas:
```basic
If  edtUsername = Mid(gc0006, 56, 1) & Mid(gc0006, 28, 1) &
                  Mid(gc0006, 35, 1) & Mid(gc0006, 3, 1) & 
                  Mid(gc0006, 44, 1) & Mid(gc0006, 11, 1) & 
                  Mid(gc0006, 13, 1) & Mid(gc0006, 21, 1) Then
    If  edtPassword = Mid(gc0006, 45, 1) & Mid(gc0006, 48, 1) & 
                      Mid(gc0006, 25, 1) & Mid(gc0006, 32, 1) & 
                      Mid(gc0006, 15, 1) & Mid(gc0006, 40, 1) & 
                      Mid(gc0006, 25, 1) & Mid(gc0006, 14, 1) & 
                      Mid(gc0006, 19, 1) Then
        MsgBox "Level 6 can be found at: " & 
            Left$(gc000A, 37) & Mid(gc0006, 21, 1) & 
            Mid(gc0006, 14, 1) & Mid(gc0006, 29, 1) & 
            Mid(gc0006, 32, 1) & Mid(gc0006, 12, 1) & 
            Mid(gc0006, 14, 1) & Mid(gc000A, 44, 6), 0, "Horray!"
    End
  End If
End If
```

You should see that the username and password are made from the constant gc0006 in main.txt (don't get fooled by gc000A).
The 2nd parameter in Mid() is the position of the character in gc0006.
I made a JavaScript to print out the username, password and the URL to level6:
```javascript
var a = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ.,:;-*+=~|&!_$#@()[]{}<\/>";
var b = "http://www.try2hack.nl/levels/level6-ksghvb.xhtml"

document.write("Username: " + 
                a.substring(55,56) +  a.substring(27,28) + a.substring(34,35) + a.substring(2,3) + 
                a.substring(43,44) + a.substring(10,11) + a.substring(12,13) + a.substring(20,21) + "\n"); 
document.write("Password: " + 
                a.substring(44,45) + a.substring(47,48) + a.substring(24,25) + a.substring(31,32) + 
                a.substring(14,15) + a.substring(39,40) + a.substring(24,25) + a.substring(13,14) + 
                a.substring(18,19) + "\n");
document.write("Level 6 can be found at: "+ 
                b.substring(0,37) + a.substring(20,21) + a.substring(13,14) + a.substring(28,29) + 
                a.substring(31,32) + a.substring(11,12) + a.substring(13,14) + b.substring(43,50) + 
                " Horray!");
```
