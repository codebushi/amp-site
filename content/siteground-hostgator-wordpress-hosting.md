---
title: Siteground vs. Hostgator for Wordpress Hosting
date: "2018-04-22"
path: "/siteground-hostgator-wordpress-hosting/"
image: "./img/siteground-hostgator-hosting.jpg"
description: "A real world comparison between Siteground and Hostgator for Wordpress hosting. A new Wordpress site was installed with each hosting provider and we ran various page speed tests to find the winner."
featured: false
disclosure: true
imagewidth: 920
imageheight: 503
---

For as long as I can remember, I've been hosting my Wordpress sites with [Hostgator.com](https://partners.hostgator.com/c/1225302/177309/3094) without any major issues. They were the most popular option for Wordpress hosting at the time and their prices were very reasonable. Recently a friend of mine wanted to start a new Wordpress site so I looked into some new hosting options with him. I came across [Siteground.com](https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f) a lot during my research and everyone kept raving about their speed and reliability. Since I'm no longer getting the cheapest prices anymore with Hostgator, I decided to try out Siteground and take advantage of their introductory offer.

It seems like both companies have been in the hosting space for a long time, I was curious to see if Siteground was indeed faster than Hostgator. I decided to run a side by side comparison by creating two new Wordpress sites, putting one on my old Hostgator and the other on my new Siteground server. With Hostgator, I'm using their ["Baby Plan"](https://partners.hostgator.com/c/1225302/177309/3094?u=https%3A%2F%2Fwww.hostgator.com%2Fweb-hosting) which is their mid-tier shared hosting plan with unlimited domains. I chose a similar option with Siteground, their ["GrowBig"](https://www.siteground.com/web-hosting.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f
) plan which is also mid-tier shared hosting and unlimited domains.

I also installed an identical theme on each site and configure the servers use the same php.ini configuration. The theme I chose was a free one called [NewsMag](https://wordpress.org/themes/newsmag/). Here are the two sites:

<ul>
  <li><a href="http://hostgator.codebushi.com/" target="_blank">http://hostgator.codebushi.com/</a></li>
  <li><a href="http://siteground.codebushi.com/" target="_blank">http://siteground.codebushi.com/</a></li>
</ul>

After installing the theme on each site and importing the dummy content, I ran a series of speed tests for each hosting provider. The website testing tools I used were:

- [Pingdom Tools](https://tools.pingdom.com/)
- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
- [GTmetrix](https://gtmetrix.com/)
- [WebPagetest](https://www.webpagetest.org/)

The main thing I'm looking for is page load speeds because Google has stated many times that [page speed](https://webmasters.googleblog.com/2018/01/using-page-speed-in-mobile-search.html) will be a factor in search rankings. Let's check out the results.

<div class="row text-center mt-5">
<div class="col-md-6">
<p><strong>Hostgator Speed Test w/ Pingdom Tools</strong></p>
<amp-img src="../img/hostgator-pingdom.png" alt="Hostgator speed test - Pingdom" layout="responsive" width="1000" height="1355"></amp-img>
</div>
<div class="col-md-6">
<p><strong>Siteground Speed Test w/ Pingdom Tools</strong></p>
<amp-img src="../img/siteground-pingdom.png" alt="Siteground speed test - Pingdom" layout="responsive" width="1000" height="1354"></amp-img>
</div>
</div>

As you can see, the page size and number of requests are identical since we have the exact same theme installed on each Wordpress site. The load times are pretty close, with Siteground being slightly faster (by 0.29 seconds). It seems like with Pingdom Tools, Siteground is just a tiny bit faster than Hostgator.

**Winner:** [Siteground.com](https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f) by a small margin

<div class="row text-center mt-5">
<div class="col-md-6">
<p><strong>Hostgator Optimization Test w/ Google PageSpeed</strong></p>
<amp-img src="../img/hostgator-google.png" alt="Hostgator optimization test - Google" layout="responsive" width="1000" height="821"></amp-img>
</div>
<div class="col-md-6">
<p><strong>Siteground Optimization Test w/ Google PageSpeed</strong></p>
<amp-img src="../img/siteground-google.png" alt="Siteground optimization test - Google" layout="responsive" width="1000" height="776"></amp-img>
</div>
</div>

When testing Hostgator and Siteground using Google PageSpeed Insights, you can see a much bigger discrepancy with the mobile optimizations. The Wordpress site on Hostgator scored a 75 with mobile performance and has an optimization suggestion to "Reduce server response time." The site on Siteground scored much better with mobile, a pretty impressive score of 90. This is a pretty important difference since Google is now rolling out their [mobile-first indexing](https://webmasters.googleblog.com/2018/03/rolling-out-mobile-first-indexing.html) and you'll want higher mobile performance in the future.

**Winner:** [Siteground.com](https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f)

<div class="row text-center mt-5">
<div class="col-md-6">
<p><strong>Hostgator Speed Test w/ GTmetrix</strong></p>
<amp-img src="../img/hostgator-gt.png" alt="Hostgator speed test - GTmetrix" layout="responsive" width="1406" height="855"></amp-img>
</div>
<div class="col-md-6">
<p><strong>Siteground Speed Test w/ GTmetrix</strong></p>
<amp-img src="../img/siteground-gt.png" alt="Siteground speed test - GTmetrix" layout="responsive" width="1000" height="608"></amp-img>
</div>
</div>

Using GTmetrix, both Wordpress sites scored pretty much the same in all categories. The PageSpeed and YSlow scores were identical, so it seems like a tie for this test.

**Winner:** Tie

<div class="row text-center mt-5">
<div class="col-md-6">
<p><strong>Hostgator Site Test w/ WebPagetest</strong></p>
<amp-img src="../img/hostgator-webpagetest.png" alt="Hostgator site test - WebPagetest" layout="responsive" width="1000" height="1240"></amp-img>
</div>
<div class="col-md-6">
<p><strong>Siteground Site Test w/ WebPagetest</strong></p>
<amp-img src="../img/siteground-webpagetest.png" alt="Siteground site test - WebPagetest" layout="responsive" width="1000" height="1240"></amp-img>
</div>
</div>

The load times with WebPagetest.org are longer than what we've seen previously, but there's still a slight difference between Hostgator and Siteground. Not only is Siteground faster in load time, it's also faster when rendering the first bytes of the website. This means that visitors to the Siteground site will see something on the screen just a little bit faster when compared to Hostgator.

**Winner:** [Siteground.com](https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f) by a small margin

For the most part, the Siteground hosted Wordpress site was faster than Hostgator, but only by a small margin. The only test that presented a large discrepancy was the Google PageSpeed Insights with mobile optimization. If you just signed up with [Hostgator.com](https://partners.hostgator.com/c/1225302/177309/3094), it's probably not worth switching since you'll have access to their low intro prices. However, if you're no longer getting the low prices with Hostgator, then consider switching to [Siteground.com](https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f) for better performance and get their low introductory prices.

<h4 class="mt-4"><a href="https://www.siteground.com/index.htm?afcode=a3b8a147dc2f39afc11273a54b853a4f">Sign up with Siteground</a></h4>