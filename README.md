# TimeCapsule

This guide simply creates a Samba (as refered to as smb) server with configuration to support TimeMachine on MacOS, it will also be accessible on Windows as a network drive.

### Step 1
Find an old computer to use for this project. I bought an old i386 Acer aspire r3610, it was cheap and has a ethernet port. As it is a 32-bit computer I was unable to download the latest version of Ubuntu. Using a 32bit computer may make maintainance more difficult in the future, althought I am happy with my build thus far. I wouldn't reccomend a raspberry pi for this project either as it has limited ram and no SATA support, this may change in the future of course.
You could also you a laptop for this project, as long as it runs Linux. Don't forget to upgrade the storage!

### Step 2 
Install Linux. I reccomend Ubuntu for 64 bit computers and Debian for 32-bit. 

### Step 3
Install Samba and avahi.
`sudo apt-get install samba avahi-daemon`

Avahi is for bonjour support which enables the computer to appear on the LAN as a .local address

SAMBA VERSION MUST BE 4.8 OR HIGHER. You can check your version using `smbd -V`
If samba 4.8 or higher is not available for your distro/archtecture you may find it on launchpad:
https://launchpad.net/~linux-schools/+archive/ubuntu/samba-latest/+packages
or at the van belle apt repo
http://apt.van-belle.nl/

alternatively it can be build from source but this is not for the faint of heart
https://www.linuxsecrets.com/samba-wiki/index.php/Build_Samba_from_Source.html



### Step 4
configure samba via `sudo nano /etc/samba/smb.conf`
change the contents to the contents of this file https://github.com/alhockly/pi-wireless-drive/blob/master/smb.conf

You should only need to change the lines under the `[Time Capsule]` definition

check config is correct with `sudo testparm -s`

restart samba `sudo service smbd restart`

### Step 5
Add a new user to samba `sudo smbpasswd -a <username>`

Enable new user `sudo smbpasswd -e <username>`

Copy timemachine.service from this repo to /etc/avahi/services to enable .local address broadcasting


## MacOS config

### Step 1
On you MacOS computer check the "Network" item in finder and you should see the samba share by its hostname. If not go to Finder > Go > Connect to server then enter smb://ip-address

Finding the IP address of you server varies by linux distro, most allow you to use `ifconfig`

Once you establish a connection with the server login with the details you specificied in step 5

### Step 2
You should now be able to add the new network loctation as a Time Machine backup location in System Preferences


## Optional 

On Debian and Ubuntu you can use `sudo nano /etc/hostname` and `sudo nano /etc/hosts` to change your computers hostname, which is what will appear to other computer when they discover it on the network.



some articles I used

>[install samba on pi] https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/
>[time machine on remote share] https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder
>https://wiki.samba.org/index.php/Configure_Samba_to_Work_Better_with_Mac_OS_X
>https://www.thedigitalpictureframe.com/installing-samba-on-your-raspberry-pi-buster-and-optimizing-it-for-macos-computers/
>https://surrouter.asuscomm.com/surblog/2019/05/05/mac-timemachine-with-samba-4-8-on-ubuntu-18-10/

Samba docs: https://www.samba.org/samba/docs/current/man-html/samba.7.html

~AFP~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~



