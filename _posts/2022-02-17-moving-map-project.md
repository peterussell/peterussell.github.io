---
title: A new project
date: 2022-02-17 20:31:00 +1300
categories: [projects, pmp]
tags: [projects, pmp, aviation]
---

I thought it would be an interesting challenge to try and build a web app
on top of freely available [VFR](https://www.faa.gov/air_traffic/flight_info/aeronav/digital_products/vfr/)
and [IFR](https://www.faa.gov/air_traffic/flight_info/aeronav/digital_products/ifr/)
aeronautical chart data from the FAA. I'm not sure exactly what the final
'product' looks like, but have a few ideas (below).

Either way, figuring out how to download, parse, and turn the chart data into
something usable in a browser sounds like a fun, so thought I'd start there
and see how it goes.

Breaking this into three rough parts:

 1. Build a service to fetch charts from the FAA website (ideally with some kind of
    caching layer), collate, process, and present them as a browser-based moving map.
 2. Add some form of basic flight planning component, sort of like a simplified version
    of [skyvector.com](https://skyvector.com/).
 3. ???

As mentioned, I have a few ideas percolating for 'Part 3' thanks to feedback from
friends. Examples could be displaying 
[Localized Aviation MOS Program (LAMP)](https://vlab.noaa.gov/web/mdl/lamp) data for a
flight route, or replaying historic flight plans over time (eg. from a digital
log book).

### Motiviation

This project's an excuse to learn about GIS (something I've been interested in for
a while but know nothing about), get deeper into a few technologies like [Docker](https://www.docker.com/), 
[GraphQL](https://graphql.org/), [CQRS](https://martinfowler.com/bliki/CQRS.html), and 
explore a few new [AWS Services](https://aws.amazon.com/products/?hp=tile&so-exp=below&aws-products-all.sort-by=item.additionalFields.productNameLowercase&aws-products-all.sort-order=asc&awsf.re%3AInvent=*all&awsf.Free%20Tier=*all&awsf.tech-category=*all).

The plan is to write the backend services layer in Java (which I haven't written since
university), the UI in React + Redux Toolkit, and have them communicate via a REST API (possibly
Spring + GraphQL).

### Tasks

 - Set up a Trello board.
 - Set up a GitHub repo.
 - Set up an AWS account.
 - Create a 'Hello world' Java-based Docker container, and get it to automatically build and push to
   [Amazon ECR](https://aws.amazon.com/ecr/) on commit.
 - Learn more about running Docker containers on AWS (eg. tradeoffs between K8s, ECS, etc)
   and decide on which approach to go with.
