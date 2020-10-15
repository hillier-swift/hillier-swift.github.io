---
layout: post
title: Ping Subnet
---

Been a few days so dug into my useful scripts. Wrote this one when a member of staff gave a device a static IP address and forgot what IP they gave it. It could be jazzed up to run the pings quicker but this gets the job done and you can fire it in the background and come back to CSV of IP's you can filter. Also handy for a home networks where you may not have the access to the DHCP server.  
  
```powershell
#usage Ping-range.ps1 -subnet 10.14.0  
param  
([parameter(Mandatory=$true)][string]$subnet,  
[string]$outfile = "C:\tmp\PingRange.csv"  
)  
  
$results = 1..254 | foreach { new-object psobject -Property @{Address=”$subnet.$_”;Ping=(test-connection “$subnet.$_” -quiet -count 1)}}  
  
$results | Export-csv $outfile  
```
