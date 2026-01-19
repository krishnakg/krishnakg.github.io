+++
title = "Quickly subscribing to RSS feeds in FreshRSS"
author = ["Krishna Gopalakrishnan"]
date = 2025-04-13T18:13:00-07:00
lastmod = 2025-04-13T18:27:36-07:00
tags = ["freshrss"]
draft = false
+++

I use a selfhosted [FreshRSS](https://freshrss.org/index.html) instance to manage my rss feeds. I wanted a quick way to add a web page's rss feed into FreshRSS without copy pasting the URLs. So i created a bookmarklet with the following code to open the freshRSS add-feed page with the URL filled in.

<!--more-->

```javascript
javascript:(function(){
  var freshRssUrl = 'https://[FreshRSS_Domain_URL]/i/?c=feed&a=add&url_rss=';
  window.open(freshRssUrl + encodeURIComponent(window.location.href));
})()
```

Of course, update the URL to your FreshRSS domain accordingly in that snippet!

Another quick tip: Did you know that all Youtube channels have a rss feed? I use these rss feeds to get notifications for any new youtube videos on my favorite channels.
