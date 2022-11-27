---
title: "Burpsuite setup with Firefox"
date: 2022-11-27 19:27:00 +0100
categories: [Tools, installation]
tags: [busrpsuite, installation, foxyproxy, kali linux, tools]     # TAG names should always be lowercase
---

A guide on how to setup Burpsuite on Firefox with the FoxyProxy Extension.

## 1. Install FoxyProxy add-on in Firefox 
<hr>

- First we'll need to add the Add-on and configure it:<br>
- Firefox Settings -> "Extensions & Themes" left bottom in the sidemenu <br>
- Search for FoxyProxy and install

### 2. Configure FoxyProxy
<hr>

After Installation click on the FoxyProxy Icon and go to "Options"
- Enter the values as in the picture (change name to preference)
    ![FoxyProxy options](/assets/img/post-images/foxyproxy.png)
   


## 3. Configure Burpsuite
<hr>

- Go to "options" in the Proxy tab
![FoxyProxy options](/assets/img/post-images/burpsuiteproxyoptions.png)
    - Go to "Proxy Listeners" section and click "Add"
        ![FoxyProxy options](/assets/img/post-images/burpsuiteoptions.png)
        - Add port: 8080
        - Add Specific adress: 127.0.0.1
        - Click "OK"

### 4. Import Certificate
-  Click on "Import / export CA certificate" underneath the Proxy Listeners
    ![FoxyProxy options](/assets/img/post-images/burpsuiteoptionsCA.png)
    - A prompt will show asking for the format. Choose "Certificate in DER format" under Export
    ![FoxyProxy options](/assets/img/post-images/burpsuiteoptionsDER.png)

## 5. Configure Firefox
<hr>

- Go to settings -> "Privacy & Security" -> Scroll to "Certificates" section (or search in the searchbar)
    ![FoxyProxy options](/assets/img/post-images/firefoxCA.png)
    - Click on "View Certificates"
    - Click on "Import" and select the previosuly saved .der certificate file
    - After installation make sure PortsSwigger is in the list of Authorities

## 6. All done!
<hr>

- Now start up Burpsuite and Turn on Interceptions
- Click on "Burp" (or whatever name you chose) in FoxyProxy
- Enjoy Burpsuite