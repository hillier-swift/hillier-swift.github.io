---
title: Plex - Docker for Windows (Lessons Learnt)
header:   
  image: images/Header/2018-4-4-plex.jpeg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - PowerShell
tags:
  - Docker
---

For the long Easter weekend, I set my self a task to rebuild my Microserver to Windows 10. Part of this reason was so I could play with Docker, the other was AV companies charging a fortune for server AV. I didn't want to go full headless, and Linux for a home lab is a step too far.  
  
I went into this with zero understanding of the mindset of containers virtual machines sure, but throw way containers seamed odd. Over the weekend I almost went back to the standard install a few times but started  
  
Fast forward to now, I have deleted over 20 plex containers getting the setup just right about 10 of these I didn't even know existed as was me miss understanding the commands. I have a PowerShell script that will start up a new container and if I wanted I could just have it removing the old one. I don't have the trust there yet, but it will come.  
  
### Lessons learnt  
  
* Standard IT - Google is your friend, but a lot of the commands are for [Linux](https://hub.docker.com/r/plexinc/pms-docker/)  
  * In the linked example \ is a joiner making the code easier to read don't use this in Powershell.  
* For plex media store, for your data out of the container. How I have configured this is below  
  * Media - Slow storage unless you have everything on an SSD  
  * Config - SSD this also hosts the metadata if you have the space to do it makes loading quicker  
  * Transcoding - SSD is a must slow transcoding is not fun for anyone  
  
* If everything above is done right don't be afraid of dumping a container, you make a mistake with the script don't try to fix in post, delete it set the script and try again, makes results repeatable.  
I am not going to do a full setup guide there is a few already [out there](https://hub.docker.com/r/plexinc/pms-docker/) on the image page, but the below is the script I am using to deploy a new container. The idea is I can have updates happen overnight if I want but rather than Plex doing the update I can start to clean this is just the first step.  
  
I made a few modifications to the port rules to allow my Apple TV to connect to Plex as Apple's "Just Works" is a nightmare for firewalls.  
  
```powershell
$containerName ="plex"  
$Hostname = "Hostname"  
#To change location of files make sure Docker can see the drive and only change before the :/  
$configLocation= "C:/Docker/Plex/config:/config"  
$transcodeLocation = "C:/docker/Plex/transcode:/transcode"  
$dataLocation = "D:/Plex/Media:/data"  
#Change to the image branch you have downloaded  
$imageused = "plexinc/pms-docker:plexpass"  
#Got from https://www.plex.tv/claim/  
$PlexClaim = "claim-abc123141324sad"  
$AvertisedIP = "http://192.168.0.1:32400/"  
  
#I am aware I should change the port rules to be variables  
docker run --detach --name $containerName -p 1900:1900/udp -p 32410:32410/udp -p 32412:32412/udp -p 32413:32413/udp -p 32414:32414/udp -p 32400:32400 -p 3005:3005 -p 5353:5353/udp -p 8324:8324 -p 32469:32469 -e TZ=Europe/London -e PLEX_CLAIM=$PlexClaim -e ADVERTISE_IP=$AvertisedIP -h $hostname -v $configLocation -v $transcodeLocation -v $dataLocation --restart unless-stopped $imageused  
```  

EDIT: After much playing around I found hosting the database on an NTFS share it keeps corrupting I did create a volume within Docker, this fixed the issue. This method also causes a double NAT which for external access is a pain so I have moved back to the windows install
