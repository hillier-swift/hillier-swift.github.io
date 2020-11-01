---
header:   
  overlay_image: /images/Header/2019-03-10-security-headers-getting-from-c-to-a.jpg
  caption: "Photo credit: [**unsplash: @danielsessler**](https://unsplash.com/@danielsessler)"
title: Security Headers upgrading from C to A+
excerpt: "My journey of updating my blogs security rating"
categories:
  - Security
tags:
  - Blog Updates
---
[Security Headers](https://securityheaders.com) is a site run by [Scott Helme](https://scotthelme.co.uk), and I was getting some stick for getting **C** when I first setup the blog. This is my journey as an Operations guy not a WebDev so most of this is on the advise on people with more experience than me. Some of this was easy; some took a lot of work and help from a friend. Where this has been the case it's better to read their resources rather than mine, and there are links when it's needed. The reasons I didn't get an A are:

* **Content-Security-Policy** - Content Security Policy is an effective measure to protect your site from XSS attacks. By whitelisting sources of approved content, you can prevent the browser from loading malicious assets.
* **X-XSS-Protection** - X-XSS-Protection sets the configuration for the cross-site scripting filter built into most browsers. Recommended value "X-XSS-Protection: 1; mode=block".
* **Referrer-Policy** - Referrer Policy is a header that allows a site to control how much information the browser includes with navigations away from a document.
* **Feature-Policy** - Feature Policy is a header that allows a site to control which features and APIs can be used in the browser. For example I can say my site will never need to use your webcam so don't allow it to.

## Why bother

Not only is it the responsible thing to do; but running a site and not using all the tools in your arsenal to protect your site can bite you later. For me I am using this as part of my personal brand if it was used for nefarious reasons wouldn't look good for my brand. If you run a business you would put an alarm on your office and lock the doors for me this is the same for my web presence.

## Updating the policies

### Content-Security-Policy

This one felt like it was going to be a royal pain and it was. I am still finding a few odd sites that the platform site uses or at the end of my post I embed a tweet that needed about five extra sites adding. To get to where am I used a mixture of Chrome's dev console and [Report URI](https://hillier-swift.co.uk/security-headers-getting-from-c-to-a/report-uri.com) to help me out. This was achieved by editing the configuration files under in `/etc/nginx/sites-available/[yoursite.com]-ssl.conf`

> Pro tip - Once you think you have your CSP down, check your site again do this for a few cycles there will almost certainly be one more thing.

This took a large amount of time and I am still fiddling with it but thanks to James over at [https://melodiouscode.net/](https://melodiouscode.net/) I got the blog up and running. To be honest, this would have taken a few days to get right without his help. It's achievable with the wizard mode in Report URI just a little slower than getting someone to help out check out [https://scotthelme.co.uk/content-security-policy-an-introduction/](https://scotthelme.co.uk/content-security-policy-an-introduction/) for a good write up.

### X-XSS-Protection/Referrer-Policy/Feature-Policy

These were just an edit of the configuration file found at  `/etc/nginx/sites-available/yoursite.com-ssl.conf` adding the line in the server { section just before location tag. Add the following three lines and jobs done.

**X-XSS-Protection** -  `add_header X-XSS-Protection "1; mode=block";`.
**Referrer-Policy** - `add_header Referrer-Policy "strict-origin-when-cross-origin";`
**Feature-Policy** - `add_header "accelerometer 'none'; camera 'none'; geolocation 'none'; gyroscope 'none'; magnetometer 'none'; microphone 'none'; payment 'none'; usb 'none'";`

Once you have added the above run a `nginx -s reload` to update the config and rescan your site.

## Bonus

* [HSTS](https://hstspreload.org/?domain=hillier-swift.co.uk) - This hard codes HTTPS in the browser and as I was in the HTTPS world I enabled this via Cloudflare and registered with [HSTS Preload](https://hstspreload.org/?domain=hillier-swift.co.uk) - I followed this  [Video](https://youtu.be/mVzdEl5G0iM?t=30) on how do it.
* TLS 1.2 - Again done via CloudFlare it was just a slider. Like the video above I would recommend watching the videos on the [httpsiseasy](https://httpsiseasy.com/) site.
* I have an A+ rating  from [SSL Labs](https://www.ssllabs.com/ssltest/analyze.html?d=hillier-swift.co.uk) with this work and the bonus changes.

## Summary

It was a long road and if you are going down the same path its getting the CSP right that takes up the most time. Now my site cannot do more harm than good and it's at a high standard for web security. The task also got me to play around in the Linux command line which has been awhile. I would recommend anyone running a site to run through these steps.

## Update

This might not be the end of the road for this story after tweeting this article out it was pointed out that a whitelist approach might not offer the protection I thought. I need to some research and come back to this.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Unfortunately your CSP offers no protection against XSS as it&#39;s trivially bypassable: <a href="https://t.co/i5f6Asm0Sk">https://t.co/i5f6Asm0Sk</a></p>&mdash; Lukas Weichselbaum (@we1x) <a href="https://twitter.com/we1x/status/1104874751246381059?ref_src=twsrc%5Etfw">March 10, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## Resources

**Scott Helme** - [https://scotthelme.co.uk/hardening-your-http-response-header](https://scotthelme.co.uk/hardening-your-http-response-headers/)
**Troy Hunt** - creator of [https://httpsiseasy.com/](https://httpsiseasy.com/)
**Melodious Code** - less the site more the man - [https://melodiouscode.net/](https://melodiouscode.net/) or [@jamesakadamingo](https://twitter.com/jamesakadamingo) on twitter and on his friendly tweet below I will end the post. *Note my twitter handle is [@thillier_uk](https://twitter.com/thillier_uk)*

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Teaching <a href="https://twitter.com/tony_hillier1?ref_src=twsrc%5Etfw">@tony_hillier1</a> how to <a href="https://twitter.com/hashtag/CSP?src=hash&amp;ref_src=twsrc%5Etfw">#CSP</a> in a <a href="https://twitter.com/StarbucksUK?ref_src=twsrc%5Etfw">@StarbucksUK</a>, making heavy use of <a href="https://twitter.com/reporturi?ref_src=twsrc%5Etfw">@reporturi</a>. Now if he could only learn to write coherent articles! <a href="https://twitter.com/hashtag/PotKettleBlack?src=hash&amp;ref_src=twsrc%5Etfw">#PotKettleBlack</a></p>&mdash; melodiouscode (@melodiouscode) <a href="https://twitter.com/melodiouscode/status/1101820986788405248?ref_src=twsrc%5Etfw">March 2, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
