---
layout: "post"
title: "Epic Road Trip Concluded"
date: "2021-09-27 08:44"
category: drives
featured_image: /images/2021/09/2021-09-22 by the numbers.png
---

It is done! The (potentially over-ambitious) 2021 Epic Road Trip has successfully concluded with >10k miles recorded.  Read on to see some numbers, learn about my next steps for the data, and check out the final donut stop on the trip.

## By The Numbers
A final summary of 'by the numbers' to wrap up the road trip:
- Miles: 10,639
- Latest leg: 412 miles across 2021-09-21 & 2021-09-22
- Donuts enjoyed: 71
- Data records: 153,178
- Data size: < 15 MB [^1]

[^1]: Partitioning hasn't been added to the schema yet. As a result, my latest query scanned 14.93 MB representing the entire dataset.  This entire dataset contains data beyond the the road trip which ended on 2021-09-22.

Final map graphic:
![By The Numbers: Thru 2021-09-21](/images/2021/09/2021-09-22 by the numbers.png)

## Data
As with all data efforts, the data is good but needs some work.  Attributes like `elevation` are null while `battery_level` is an integer value.  The former is painful because I will have to backfill the elevation while the latter is annoying because I will not be able to do precise power consumption analysis.  All good, though, because I am already scheming up a couple data projects to augment this capture dataset with a couple other sources for elevation and weather.

## Donuts
Before I left Salem, OR, there was a donut shop I saved for the last: [Bigwig Donuts](https://g.page/BigwigDonuts?share). They do not serve traditional donuts; their thing is all about craft donut holes!  Really neat atmosphere and lots of 'big wig' jokes when you stop inside their shop.  They had six different types of donut hole offerings when I visited... so I figured I would try them all.  What I did not realize until I ordered was this is a 'vegan' donut spot.  That is not necessarily a strike but these donuts were a bit too 'wet' for my liking.  Vegan donuts do not rise like traditional yeast donuts, rely on pumpkin filling or applesauce as a base, and are often baked rather than fried.  This ends up with a consistency closer to that of a brownie than a traditional donut.  I thought the flavors were great but without the dry, flakey consistency they are not my favorite.  Their coffee, though, was fantastic.  I had a maple brown sugar oak milk latte.  It was so good that I had to text a sweet-tooth'ed friend of mine about it.

### Photos
<div class="gallery" data-columns="3">
	<img src="/images/2021/09/IMG_8110.png">
  <img src="/images/2021/09/IMG_8111.png">
  <img src="/images/2021/09/IMG_8112.png">
</div>

### Location
<div class="map-responsive">

<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2824.1613609774367!2d-123.04121618448927!3d44.940387576262395!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x54bfff906f09b6c1%3A0x9411226d38cdbe71!2sBigwig%20Donuts!5e0!3m2!1sen!2sus!4v1632764596055!5m2!1sen!2sus" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy"></iframe>

</div>
