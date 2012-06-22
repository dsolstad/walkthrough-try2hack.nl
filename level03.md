#### Level 3

You will get an annoying prompt that disables you to view the source with right click and ctrl+u.
Just click on it and stop the browser before you get transfered to disneyland!
View the source directly with: view-source:http://try2hack.nl/levels/level3-.xhtml 

The Javascript you will find is:
```javascript
      pwd = prompt("Please enter the password for level 3:","");
      if (pwd==PASSWORD){
        alert("Allright!\nEntering Level 4 ...");
        location.href = CORRECTSITE;
      }
      else {
        alert("WRONG!\nBack to disneyland !!!");
        location.href = WRONGSITE;
      }
      PASSWORD="AbCdE";
      CORRECTSITE="level4-sfvfxc.xhtml";
      WRONGSITE="http://www.disney.com";
```

Nevermind that Javascript you see there, it's fake, but check out that external Javascript that is camouflaged right over it: 

```html
<script src="JavaScript"></script>
```

Lets check it out: view-source:http://try2hack.nl/levels/JavaScript
```
PASSWORD = "try2hackrawks";
CORRECTSITE = "level4-kdnvxs.xhtml";
WRONGSITE = "http://www.disney.com";
```

There you go!