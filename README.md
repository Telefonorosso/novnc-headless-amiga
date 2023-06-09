# novnc-headless-amiga
An emulated Amiga in your browser!!  
(no audio, sorry)  
  
Based on FS-UAE copyright © 2011-2022 Frode Solheim  
x11vnc Copyright (C) 2002-2010 Karl J. Runge  
noVNC by Joel Martin

## PREREQUISITES
- Debian 11 on real hardware or virtual environment  
- A few CPU cores and some RAM  
- Amiga OS files and suitable ROM  

## install Docker
```
su
curl -fsSL https://get.docker.com -o get-docker.sh
sh ./get-docker.sh
```
## create container!
```
cd
mkdir novnc-headless-amiga
cd novnc-headless-amiga
mkdir files
cd files
mkdir HDD
cd HDD
```
[COPY YOUR AMIGA FILES IN THERE]
```
cd ..
```
[COPY YOUR AMIGA ROM IN THERE AND RENAME IT AMI.ROM]
```
cd ..
nano Dockerfile
```
```
FROM debian
RUN apt-get -y update && export DEBIAN_FRONTEND=noninteractive && apt-get -y install procps net-tools xorg xvfb x11vnc fs-uae novnc
COPY files/ /
RUN chmod +x /boot.sh
CMD ["sh", "/boot.sh"]
```
```
nano files/boot.sh
```
```
#!/bin/bash
export DISPLAY=:0
startx /usr/bin/fs-uae /OK.fs-uae &
x11vnc -quiet -nopw -no6 -forever -create -cursor none &
chmod +x /usr/share/novnc/utils/launch.sh
/usr/share/novnc/utils/./launch.sh
```
```
nano files/OK.fs-uae
```
```
[fs-uae]
amiga_model = A4000
uae_cpu_speed = real
chip_memory = 2048
zorro_iii_memory = 131072
video_format = rgb565
hard_drive_0 = /HDD
kickstart_file = /AMI.ROM
bsdsocket_library = 1
mouse_integration = 1
automatic_input_grab = 0
```

## your folder should look like this:
```
novnc-headless-amiga/
├── Dockerfile
└── files
    ├── AMI.ROM
    ├── boot.sh
    ├── HDD
    │   ├── Utilities
    │   ├── C
    │   ├── Devs
    │   ├── Prefs
    │   └── blah...
    └── OK.fs-uae
```
## READY???
```
docker build -t novnc-headless-amiga:1.0 .
docker run -it --privileged -p 8080:6080 novnc-headless-amiga:1.0
```
You can stop it by hitting CTRL+C

## GO!!!

[ASSUMING SERVER IS AT 192.168.1.10]

http://192.168.1.10:8080/vnc_lite.html
  
   
![Cattura](https://user-images.githubusercontent.com/36540604/230849791-b8cc18f4-53d7-40fe-999a-203dc1360a6d.PNG)

