#### Level 4

As you see in the source, the file you need to check out is http://try2hack.nl/levels/PasswdLevel4.class
Get a Java decompiler. I recommend Cavaj, which you may find here: http://www.bysoft.se/sureshot/cavaj
Use Cavaj to decompile the class and check out the source:

```java
import java.applet.Applet;
import java.applet.AppletContext;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.EventObject;

public class PasswdLevel4 extends Applet
    implements ActionListener
{

    private URL finalurl;
    String infile;
    String inuser[];
    int totno;
    InputStream countConn;
    BufferedReader countData;
    URL inURL;
    TextField txtlogin;
    Label label1;
    Label label2;
    Label label3;
    TextField txtpass;
    Label lblstatus;
    Button ButOk;
    Button ButReset;
    Label lbltitle;

    public PasswdLevel4()
    {
        inuser = new String[22];
        totno = 0;
        countConn = null;
        countData = null;
        inURL = null;
        txtlogin = new TextField();
        label1 = new Label();
        label2 = new Label();
        label3 = new Label();
        txtpass = new TextField();
        lblstatus = new Label();
        ButOk = new Button();
        ButReset = new Button();
        lbltitle = new Label();
    }

    void ButOk_ActionPerformed(ActionEvent actionevent)
    {
        boolean flag = false;
        for(int i = 1; i <= totno / 2; i++)
        {
            if(txtlogin.getText().trim().toUpperCase().intern() == inuser[2 * (i - 1) + 2].trim().toUpperCase().intern() && txtpass.getText().trim().toUpperCase().intern() == inuser[2 * (i - 1) + 3].trim().toUpperCase().intern())
            {
                lblstatus.setText("Login Success, Loading..");
                flag = true;
                String s = inuser[1].trim().intern();
                String s1 = getParameter("targetframe");
                if(s1 == null)
                {
                    s1 = "_self";
                }
                try
                {
                    finalurl = new URL(getCodeBase(), s);
                }
                catch(MalformedURLException _ex)
                {
                    lblstatus.setText("Bad URL");
                }
                getAppletContext().showDocument(finalurl, s1);
            }
        }

        if(!flag)
        {
            lblstatus.setText("Invaild Login or Password");
        }
    }

    void ButReset_ActionPerformed(ActionEvent actionevent)
    {
        txtlogin.setText("");
        txtpass.setText("");
    }

    public void actionPerformed(ActionEvent actionevent)
    {
        Object obj = actionevent.getSource();
        if(obj == ButOk)
        {
            ButOk_ActionPerformed(actionevent);
            return;
        }
        if(obj == ButReset)
        {
            ButReset_ActionPerformed(actionevent);
        }
    }

    public void destroy()
    {
        ButOk.setEnabled(false);
        ButReset.setEnabled(false);
        txtlogin.setVisible(false);
        txtpass.setVisible(false);
    }

    public void inFile()
    {
        new StringBuffer();
        try
        {
            countConn = inURL.openStream();
            countData = new BufferedReader(new InputStreamReader(countConn));
            String s;
            while((s = countData.readLine()) != null) 
            {
                if(totno < 21)
                {
                    totno = totno + 1;
                    inuser[totno] = s;
                    s = "";
                } else
                {
                    lblstatus.setText("Cannot Exceed 10 users, Applet fail start!");
                    destroy();
                }
            }
        }
        catch(IOException ioexception)
        {
            getAppletContext().showStatus("IO Error:" + ioexception.getMessage());
        }
        try
        {
            countConn.close();
            countData.close();
            return;
        }
        catch(IOException ioexception1)
        {
            getAppletContext().showStatus("IO Error:" + ioexception1.getMessage());
        }
    }

    public void init()
    {
        setLayout(null);
        setSize(361, 191);
        add(txtlogin);
        txtlogin.setBounds(156, 72, 132, 24);
        label1.setText("Please Enter Login Name & Password");
        label1.setAlignment(1);
        add(label1);
        label1.setFont(new Font("Dialog", 1, 12));
        label1.setBounds(41, 36, 280, 24);
        label2.setText("Login");
        add(label2);
        label2.setFont(new Font("Dialog", 1, 12));
        label2.setBounds(75, 72, 36, 24);
        label3.setText("Password");
        add(label3);
        add(txtpass);
        txtpass.setEchoChar('*');
        txtpass.setBounds(156, 108, 132, 24);
        lblstatus.setAlignment(1);
        label3.setFont(new Font("Dialog", 1, 12));
        label3.setBounds(75, 108, 57, 21);
        add(lblstatus);
        lblstatus.setFont(new Font("Dialog", 1, 12));
        lblstatus.setBounds(14, 132, 344, 24);
        ButOk.setLabel("OK");
        add(ButOk);
        ButOk.setFont(new Font("Dialog", 1, 12));
        ButOk.setBounds(105, 156, 59, 23);
        ButReset.setLabel("Reset");
        add(ButReset);
        ButReset.setFont(new Font("Dialog", 1, 12));
        ButReset.setBounds(204, 156, 59, 23);
        lbltitle.setAlignment(1);
        add(lbltitle);
        lbltitle.setFont(new Font("Dialog", 1, 12));
        lbltitle.setBounds(12, 14, 336, 24);
        String s = getParameter("title");
        lbltitle.setText(s);
        ButOk.addActionListener(this);
        ButReset.addActionListener(this);
        infile = new String("level4");
        try
        {
            inURL = new URL(getCodeBase(), infile);
        }
        catch(MalformedURLException _ex)
        {
            getAppletContext().showStatus("Bad Counter URL:" + inURL);
        }
        inFile();
    }
}
```

Notice:
```java
infile = new String("level4");
```

Thats a file being included. Lets take a look at it: view-source:http://try2hack.nl/levels/level4
```
level5-fdvbdf.xhtml
appletking
pieceofcake
```

There you have the username and password and the url to the next level.
