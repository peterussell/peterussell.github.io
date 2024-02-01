---
title: "PMP Update 8: Porting the fetcher"
date: 2024-01-31 22:25:00
categories: [projects, pmp]
tags: [c#, fetcher, pmp]
---

The original plan for this update was to get the [API and web containers talking to each other]({% post_url 2024-01-14-pmp7-web-project-setup-with-nextjs-and-docker %})
but we're going to change tack and try to get 'The Fetcher' up and running.

To recap, the idea for this project is to show lateral and vertical flight path data from
MSFS 2020 rendered on top of FAA aeronautical charts. To be clear, I have no idea what I'm
doing but I'm hoping we'll figure it out together.

The first core piece is downloading the charts from the FAA, which is what the fetcher's
responsible for (learn the gory details in a [previous post]({% post_url 2022-07-12-pmp4-fetcher-scaffolding %})).

Fortunately I'd already worked out stubbing some of the classes and getting a Dockerfile
running in another project, so the update this week was just porting that into the `pmp`
repo and plugging it into the existing solution. It went really smoothly so now we have
a little hello world.

Hello, fetcher!

![Hello, fetcher](/assets/img/posts/2024-01-31/fetcher-hello-world.png)

If you want to follow along, the repository's available at [https://github.com/peterussell/pmp/](https://github.com/peterussell/pmp/).
