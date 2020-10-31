---
title: Migrating from Wordpress to Ghost
categories:
  - Secuirty
tags:
  - Blog Updates
---

I had some fun moving a few articles from my old WordPress blog to my new one, it would have been quicker to copy and paste, but I shall not be defeated! *Editors note - I was nearly defeated.*

### Prereqs

1. Install Node.JS from <https://nodejs.org/en/download/>
2. Download & Install Python <https://www.python.org/downloads>. Make sure python is in Path variable
3. Extract Zip to a helpful location, *I used a _dump folder on a root of a HDD*
4. You will need a version of Ghost 1.x somewhere I use a DigitalOcean droplet later on.

### Getting Wordpress XML and converting to JSON for Ghost 1.0

1. Log into your WordPress admin portal
2. Go to **settings**
3. Scroll down to **export**
4. Download the zip and extract to a handy location I used _dump/blog
5. Open Node.js command prompt
6. Type `npm install -g wp2ghost`
7. Take note of the install location my default was `%appdata%\npm\node_modules\wp2ghost\`
8. In command prompt navigate to install location
9. Type `node wp2ghost N:\Dump\blog\YourXMLfile.xml`
10.You will now have a `YouXMLfile.xml.JSON` in the location your XML files stored.  

### The Update

We now need to convert the JSON from Ghost 1.0 format to something Ghost 2.0 can use; this involves importing and exporting it from a Ghost 1.x instance. I see some options, and a free way is using Docker locally, but I am playing with Digital Ocean and have a bunch of free credits so going down this route. Feel free to use my [referral code](https://m.do.co/c/d1bbd20b7350) to get some free credits if you are doing this from scratch.

1. Log into DigitalOcean
2. Create a Ubuntu Droplet, give it a name etc
3. Wait for Email, connected via SSH change root PW
4. Your now in your droplet we need to install Ghost 1.0 on this droplet
5. I am not going to reinvent the wheel; follow this [guide](https://docs.ghost.org/install/ubuntu/) but when running ghost install change latest to this `ghost install 1.25.1`
6. I had an issue with Nginx rerunning the setup fixed it not sure why.
7. Once you have Ghost running on docker or somewhere we need to clean the JSON.

### Notes

I had to delete any line with Slug under the posts section in the JSON before anything worked.

I had a bug that would only import the first item in posts section, got the error `Post: Entry was not imported and ignored. Detected duplicated entry.` Even in a basic file of just titles, it would only import the first one. I gave up and imported posts one by one. Best guess its something to do with the ID mapping as I had to remove the ID to import them as well.  My cheaty way is to delete a post at a time rather than rebuilding it. Also if you need to edit posts update the status field to `"status":"draft"`. I found I needed to do a fair bit of editing as I had a lot of code blocks so made all the posts draft and published when I edited the formatting.

If you do end up using the above method and remove the ID's you would lose your tags mapping.

You can edit posts in this droplet or on your new site, I wanted to check it's all working so I export the blog and import into my live site.

Side note images are hardcoded back to WordPress, might want to change these if you take down the other site.

### Closing statement

This was a harder effort that I was hoping for but I got my posts from my old and under used blog to my new site. This could have been smoother but never using node.js or ghost the fact I got anything up was a win. There must be a smoother way to migrate a hosted wordpress blog to Ghost 2.x but I could not find it. If my site was larger I would have spent some more time figuring out the bugs but I wasn't seeing a return on my time so fixed the rest up in post. If anyone has a better way of doing this I am all ears.
