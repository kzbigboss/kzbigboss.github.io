---
layout: "post"
title: "New blog, who dis?"
date: "2021-08-16 10:28"
featured_image: /images/elements/default_blog.jpeg
category: data
---

Ok so "new blog" is a stretch.  This is the same blog that's hosted via [Github Pages](https://pages.github.com/) but with a new backend and purpose.  Check out this post to learn about recent updates to my blog and the new stuff I will be posting here.

## Backend Updates
Previously I had [installed Jekyll](https://jekyllrb.com/docs/installation/) to a personal laptop. A few months ago I updated Ruby for an unrelated personal project... and of course it caused a problem with Jekyll builds.  I am fairly opinionated about Ruby (I hate it) and did not want to deal with uninstalling and reinstalling it on my local machine... so the blog went without posts since March 2021.

Last week, I started a sabbatical from my job and figured it was to time to rebuild Jekyll.  I set a goal to work with containers more during this break, so I figured out how to get [Jekyll running in a Docker image](https://hub.docker.com/r/jekyll/jekyll) and studied up a bit about `docker-compose`.  Now, with a little bit of YAML, Jekyll is running and building my blog without any changes to my local machine's environment.  Nice.  I used this opportunity to jump off a fork of [Barry Clark's Jekyll Now](https://github.com/barryclark/jekyll-now) and swapped to a fresh new Jekyll template: [Journal by Jekyll Themes](https://jekyllthemes.io/theme/journal-personal-jekyll-theme).

And now the kicker - if I could do this all over again, I'd drop Jekyll.  I did experiment with [Ghost](https://ghost.org/) as an alternative web platform but decided against it.  I grabbed coffee with a buddy this past weekend and learned he is using [Hugo](https://gohugo.io/) for his passion project's blog.  Hugo is Go-based and I am super interested in GoLang... but I already had this  new Jekyll-based site up and running and it would be silly to tear it down right now.  Eventually I will make the swap, though.

## Blog's purpose
Previously the blog hosted my "2020 Vacation Project" posts where I setup a self-healing data stream.  Now I will be expanding the blog to three topics: data, drives, and donuts.  [Data](/data/) is what I was already using this blog for: posting about various data projects I tinker with (like this blog).  [Drives](/drives/) will be all about my road trips: posts about upcoming road trips as well as eventually porting over previous road trip posts from my [KZTeslaRoadtrip](https://kzroadtrip.wordpress.com/) blog.  Finally, [Donuts](/donuts/), is all about, well, donuts!  I have been posting my donut tours and trips to social media... but I am starting to get over social media.  So instead, I'm bringing my donut posts here.
