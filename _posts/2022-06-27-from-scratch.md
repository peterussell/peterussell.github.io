---
title: "PMP Update 3: From scratch"
date: 2022-06-27 10:59:00
categories: [projects]
tags: [pmp]
---

Life's had a rapid reshuffle over the last few weeks, mainly centered
around interviewing for and then getting up to speed with a new job.
But the smoke has cleared and I'm excited to pick up where I left off
with my [map project]({% post_url 2022-02-17-moving-map-project%}).

So here's the first hurdle. Many of the things I'm planning to build
are things I've either worked on in situations where they're
already set up (eg. build pipelines deploying to ECR), or I just
have very little - or out outdated - knowledge of (eg. how to structure
a Java project).

I ended up with a long list of questions spinning in my head, and I
figured the best way to attack them is to get them down, answer them
to the best of my knowledge, and get started making some mistakes :D

![The general problem](/assets/img/posts/2022-06-27/the-general-problem.png)

In the interest of "building in the open", here's the list:

 - What's the best way to get Java updates into containers running
   in dev? Can we do hot-reloading? What triggers recompiles?
 - How do we replicate AWS infra like S3 (for storing map data)
   and a message bus in dev?
 - Event notifications (eg. maps are stored and ready for
   processing) -- SQS, SNS, EventBridge?
 - Downloads of map data are large ZIPs over a *very* slow connection.
   What's the best way to work with this in dev (eg. stage the files
   locally, use smaller files, etc.?)
 - Should dev save downloads to local mock S3 instance, or is it
   better to set up a dev environment in AWS for a local machine to
   connect to? How does this work with no internet connection in dev?
   What are the costs associated with this?
 - For data fetching, is it better to have a schedule in AWS spin
   up the container, run the task, and have the container shut down,
   or is it better to keep a container alive all the time and run
   fetches on a cron internally? What are the costs associated with
   these options?
 - What does a Java project layout look like? How are assets stored,
   are things generally organized around services or features? etc.
   (I've been working mostly in C# and React lately, both of which
   have quite different project layouts).
 - When is Spring required/not required? Is it better to try doing
   things without Spring and add it into the mix later?
 - Should I be thinking about use auth (API and web/SSO) early? 
   I don't really have any use cases that require this at the moment,
   but how difficult is it to integrate later if I want to?
 - Caching map data seems important to reduce bandwith - are there
   established ways of handling this other than standard CDNs?
 - Map data udpates are published by the FAA, so we don't have to
   download the full set each time. What does the system design look
   like around doing the initial fetch, and then diffs? Should it be
   a manual initial fetch, or better to be fault tolerant (eg. if
   I find no map data in S3, get the full set of maps, not just
   incremental).

Projects like this, and having to figure out how all these things fit
together is where I find myself learning the most. So although it's
a little embarassing to post some questions that I'm sure are pretty
straight-forward for many seasoned veterans, I look forward to figuring
out how to get this working and learn a bunch doing it.
