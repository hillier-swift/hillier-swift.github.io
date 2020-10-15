---
layout: post
title: LanSweeper torrent client report
---

This might not be the cleanest way for doing it but got the job done. The aim was to create a quick report to enable us to find Torrent applications on Client machines.

```sql
Top 1000000 tblAssets.AssetID,  
tblAssets.AssetName,  
tblAssets.Username,  
tblSoftwareUni.softwareName,  
tblSoftware.softwareVersion  
From tblAssets  
Inner Join tblAssetCustom On tblAssetCustom.AssetID = tblAssets.AssetID  
Inner Join tblSoftware On tblSoftware.AssetID = tblAssets.AssetID  
Inner Join tblSoftwareUni On tblSoftwareUni.SoftID = tblSoftware.softID  
Where (tblSoftwareUni.softwareName Like '%torrent%' Or  
tblSoftwareUni.softwareName Like '%ABC%' Or tblSoftwareUni.softwareName Like  
'%Acquisition%' Or tblSoftwareUni.softwareName Like '%Azureus%' Or  
tblSoftwareUni.softwareName Like '%BitComet%' Or  
tblSoftwareUni.softwareName Like '%BitLet%' Or  
tblSoftwareUni.softwareName Like '%BitLord%' Or  
tblSoftwareUni.softwareName Like '%BitTornado%' Or  
tblSoftwareUni.softwareName Like '%BitTyrant%' Or  
tblSoftwareUni.softwareName Like '%Deluge%' Or  
tblSoftwareUni.softwareName Like '%FlashGet%' Or  
tblSoftwareUni.softwareName Like '%Folx%' Or tblSoftwareUni.softwareName  
Like '%Free Download Manager%' Or tblSoftwareUni.softwareName Like  
'%FrostWire%' Or tblSoftwareUni.softwareName Like '%KGet%' Or  
tblSoftwareUni.softwareName Like '%Lftp%' Or tblSoftwareUni.softwareName  
Like '%LimeWire%' Or tblSoftwareUni.softwareName Like '%Miro%' Or  
tblSoftwareUni.softwareName Like '%MLDonkey%' Or  
tblSoftwareUni.softwareName Like '%OneSwarm%' Or  
tblSoftwareUni.softwareName Like '%Robert%' Or  
tblSoftwareUni.softwareName Like '%Shareaza%' Or  
tblSoftwareUni.softwareName Like '%Shareaza%' Or  
tblSoftwareUni.softwareName Like '%Tixati%' Or  
tblSoftwareUni.softwareName Like '%Tonido%' Or  
tblSoftwareUni.softwareName Like '%Transmission%' Or  
tblSoftwareUni.softwareName Like '%Tribler%' Or  
tblSoftwareUni.softwareName Like '%Vuze%' Or tblSoftwareUni.softwareName  
Like '%Xunlei%') And tblAssetCustom.State = 1  
Order By tblAssets.Username  
```

*This post is a "just for documentation (JFD)" post which I use for my referance, but I have made public as it may be useful. I put less time into these posts as they are my reference. If you would like more information or some help post a comment, and I can see what I can do.*
