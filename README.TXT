THIS HAS BEEN ONLY TESTED ON THE LATEST LEAPPAD PLATINUM FIRMWARE VERSION

Confirmed working on LeapPad Platinum.
Likely working on LeapPad Ultimate, LeapPad 3.

Make sure your tablet is connected to the internet or at least some kind of hotspot with your computer.

1: Using LFManager execute this command (or one with an equivelent function) to enable FTP: /etc/init.d/vsftpd start &

2: Download and install nmap and run the applicible script (or if you know what you are doing, you can find it another way)
(Windows, Powershell): nmap -sn 192.168.1.0/24 | ForEach-Object { if ($_ -match "Nmap scan report for (.*)") { $ip = $matches[1] } if ($_ -match "MAC Address:.*LeapFrog Enterprises") { $ip } }
(Linux, Bash): sudo nmap -sn 192.168.1.0/24 | awk '/Nmap scan report for/{ip=$NF} /MAC Address/ && /LeapFrog Enterprises/ {gsub(/[()]/,"",ip); print ip}'
If it does not work, look up how to find your subnet for your OS and replace 192.168.1.0/24 with your subnet.
It should output your LeapPad's local IP address in the parenthesis and look something like this on Windows:
NXP4330.lan (192.168.1.123)
and this on Linux:
192.168.1.123
DO NOT USE A VIRTUALIZED NETWORK ADAPTER, INCLUDING WSL AND VIRTUAL MACHINES, AS IT WILL FAIL TO FIND THE DEVICE'S MANUFACTURER

3: Using any FTP software that allows uploading folders (like WinSCP), upload everything in the root folder to your tablet's root directory.

4: Using LFManager execute this exact command ONCE: /bin/sh /bin/glibinst/inst

5: You now have glibc, some terminal libraries, bash, and nano(-tiny)!





== TECHNICAL DETAILS ==

THE LIBRARY AND EXECUTABLE FILES WERE TAKEN FROM THESE DEBIAN 8 PACKAGES

libc6_2.19-18+deb8u10_armhf.deb
libgcc1_4.9.2-10+deb8u1_armhf.deb
libstdc++6_4.9.2-10+deb8u1_armhf.deb
zlib1g_1.2.8.dfsg-2+b1_armhf.deb
libncurses5_5.9+20140913-1+deb8u3_armhf.deb
libncursesw5_5.9+20140913-1+deb8u3_armhf.deb
libreadline6_6.3-8+b3_armhf.deb
libslang2_2.3.0-2_armhf.deb
libtinfo5_5.9+20140913-1+deb8u3_armhf.deb
nano-tiny_2.2.6-3_armhf.deb
bash_4.3-11+deb8u1_armhf.deb

WHERE bash.bashrc IS MODIFIED IN THIS WAY

# set a fancy prompt (non-color, overwrite the one in /etc/profile)
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '

IS CHANGED TO

# set a fancy prompt
export PS1='\w \$ '

AND FILES IN /usr/lib/arm-linux-gnueabihf WERE MOVED TO /lib/arm-linux-gnueabihf