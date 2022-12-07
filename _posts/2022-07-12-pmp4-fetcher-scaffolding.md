---
title: "PMP Update 4: Fetcher scaffolding"
date: 2022-07-12 10:00:00
categories: [projects]
tags: [pmp]
---

I caught up with my friend Matt and talked through some of the questions
from my [previous post]({% post_url 2022-06-27-from-scratch %}), which
was a huge help and unblocked me enough to write the scaffolding for the
fetcher service (thanks Matt).

The fetcher's responsible for checking for new chart data on the
[FAA website](https://www.faa.gov/air_traffic/flight_info/aeronav/digital_products/vfr/)
and syncing any updates.

Before getting too far into that, here are the answers to a couple of
the things I was thinking about last time.

### Question 1: reloading Java code in dev containers

> What’s the best way to get Java updates into containers running in dev? Can we do hot-reloading? What triggers recompiles?

Hot-reloading Java code inside a container is possible, but while thinking
it through I realized the easier approach was probably just to run the
Java app locally while developing, and build the docker image as part of
the build pipeline.

I might run into issues here where I want to develop directly in the container
(which should be possible with docker
[bind mounts](https://docs.docker.com/storage/bind-mounts/)), but this is
probably a case where [attaching a remote debugger in from IntelliJ to code
in the container](https://medium.com/swlh/remote-debugging-a-java-application-running-in-docker-container-with-intellij-idea-efe54cd77f02) would work nicely. I'm likely to use this approach,
and will write up some notes if I do.

### Question 2: Replicating AWS infrastructure in dev

> How do we replicate AWS infra like S3 (for storing map data) and a message bus in dev?

There are two options here. I haven't explored either in depth so don't have good
grasp on any limitations yet, but they are...

 1. Set up dev infrastructure in AWS replicating the production environment. This means
    the envrionment will be a lot closer to a production setup, and lets you test things
    like keys, paths, etc. knowing that it's what you'll get in the real world.
 2. [LocalStack](https://localstack.cloud/) - "A fully functional cloud stack. Develop
    and test your cloud and serverless apps offline." This seems like a good approach
    if offline is the only option. It could be really good for serverless apps, and
    might be the best approach for using testing queues.

I'm going to go with option 1 for testing S3 uploads (which is the main part of the
fetcher) given it should be relatively cheap and a good excuse to get the AWS
account set up.

### Question 3: Java project layout

> What does a Java project layout look like?

It turns out that Maven has a [standard directory layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html). I'm sure there are more complex
use cases that deviate from this, but it's a great resource for getting
started.

### Question 4: Fetcher system design

> Map data udpates are published by the FAA, so we don’t have to download the full set each time. What does the system design look like around doing the initial fetch, and then diffs?

Now the good stuff. I'll post a more in-depth description of this as development
continues, but here's what I have so far:

 1. First, we need to get the metadata about charts from the FAA site. This includes
    the areas available, and for each area, when the current edition of the chart
    was published, and when the next edition will be published. Each 'edition' also
    has multiple downloads (a geo-referenced TIFF in a ZIP file, and a PDF). We want
    to grab those URLS for each edition, for each area, and save them. It looks like
    the FAA [might have an API for this](https://app.swaggerhub.com/apis/FAA/APRA/1.2.0),
    otherwise it'll be good old scraping.

    *(NB. this **could** be hardcoded in theory -- the chart areas and URLs are unlikely to change,
    but having them loaded dynamically gives us a bit of robustness on the off chance
    this data does change).*

 2. Then, we run a 'consistency check' stage where we check our S3 bucket to make
    sure we have all the charts we found on the website (ignoring updates at
    this point), and fetch any missing charts.

    There are two options for storing the metadata: in a table with an S3 URL, or encode it in the filename.
    I think I'll opt for the filename approach (something like `sectional/albuquerque_current_2022-05-19.zip`)
    which saves a database trip given we'll already be enumerating the filenames.

 3. Once the consistency check is done, we should be able to download the "56-day sets"
    containing only charts that have been updated, and update the affected charts
    in S3.

### Current status

So far I've written out classes to hold the chart metadata, and method stubs for
the consistency checks. Next steps will be checking the API (or HTML doc) and
loading chart metadata.

More soon...

