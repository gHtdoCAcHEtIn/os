Ubuntu_RDP.md

## RDP setup on an Ubuntu machine

Follow these steps :

#### Step 1 – Install xRDP
Open Terminal (Ctrl+Alt+T) and execute the following commands:
```
sudo apt-get update
sudo apt-get install xrdp
```

#### Step 2 – Install XFCE4
Unity doesn't seem to support xRDP in Ubuntu 14.04; although, in Ubuntu 12.04 it was supported. That's why we install Xfce4.
```
sudo apt-get install xfce4
```

#### Step 3 – Configure xRDP
In this step, we modify two files to make sure xRDP uses Xfce4. First we need to create, or edit, our .xsession file in our
home directory. We can either use nano or simply redirect an echo statement (easier):
```
echo xfce4-session > ~/.xsession
```

The second file we need to edit is the startup file for xRDP, so it will start Xfce4.
```
sudo nano /etc/xrdp/startwm.sh
```

The content should look like this (pay attention to the last line and ignore . /etc/X11/Xsession):
```sh
#!/bin/sh
if [ -r /etc/default/locale ]; then
. /etc/default/locale
export LANG LANGUAGE
fi
startxfce4
```

#### Step 4 – Restart xRDP
To make all these changes effective, restart xRDP as such:
```
sudo service xrdp restart
```

## Testing your xRDP connection

On the computer that will remotely control your Ubuntu machine, start you RDP client. Windows comes standard with a
Remote Desktop client (mstsc.exe – you can start it from a command prompt, or find the shortcut to Remote Desktop
under Accessories). Or Search &#39;remote&#39; in start (Windows 7) Or &#39;remote&#39; in search box in Windows 8.

Whichever client you use, most will work with either the computer network name or IP address of your Ubuntu
machine.

To find the IP address on your Ubuntu box, type: (note: this is a capital “i”)
``` 
hostname -I
```
Enter IP address of your Ubuntu machine. For example:

![Remote Desktop](/assets/image.png)

Depending on your RDP client capabilities and settings (for example: Microsoft RDP Client allows automatic login), you
might or might not see the login screen. Here we enter our Ubuntu username and password and click “OK”.

![Ubuntu login]()

![Ubuntu desktop]()
