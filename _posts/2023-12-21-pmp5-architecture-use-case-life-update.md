---
title: "PMP Update 5: Architecture, use case, life update"
date: 2023-12-21 08:17:00
categories: [projects, pmp]
tags: [azure, c#, pulumi, kubernetes, pmp]
---

## Life update

After a short 1 year, 3 months, and 29 day process, my green card has been issued.

It's been a whirlwind since. We packed up life in [Christchurch](https://maps.app.goo.gl/6d7cd5HLNb1HjxPt8), moved
to the Chicago North Shore and bought a house near the [Chicago Botanic Garden](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwibi-rA6qCDAxWgIUQIHX77A_wQFnoECCUQAQ&url=https%3A%2F%2Fwww.chicagobotanic.org%2F&usg=AOvVaw1nRCJLh7AqLeZZBjsz7kAJ&opi=89978449), haggled for a car, started preschool, re-learned how to speak American,
and generally pretended to be grown-ups.

## Project update

Things are settling now though, and I've started working again on the map project I wrote about earlier in the year.
I've been thinking about what I'd like to learn, but also avoid cognitive overload by building everything by trying
to align the tech stack largely what what I'm using at my normal job. That led to this list of constraints, desribed
in more detail below.

### Platform & language

 - API: C#
 - Web: Next.js
 - Fetcher (service for downloading FAA aeronatical charts): C#
 - Infrastructure: Pulumi
 - Kubernetes (and ability to run a dev cluster locally)
 - Azure (AKS, Blob storage, Azure SQL, ...?)

### CI/CD

 - Separate dev and prod environments
 - Deployed via workflows on GitHub actions
   - Merge to `dev` -> deploy to dev cluster (only appropriate components, eg, web, API, IAC)
   - Merge to `main` -> deploy to prod cluster, manual approval step
 - HA (ie. 2 instances of each critical component)

## Detail

### Azure, C#, Pulumi

We mainly use C# and Azure at my day job. Azure is fairly new to me (more learning!). So far I've been able to
deploy some infrastructure to Azure using [Pulumi](https://www.pulumi.com/). Pulumi is similar to Terraform but written in
the programming flavor of your choice, rather than YAML.

The API server has been migrated from Java to C# for the reason mentioned above - trying to line up what I'm doing at work
with this. I hesitate to invoke the "[t-shaped developer](https://killalldefects.com/2020/02/22/specializing-vs-generalizing-careers/)"
concept, but I *would* like to focus more on going deep on a language to improve on my broad-but-not-especially deep
background.

### Kubernetes

Kubernetes is definitely overkill for this, but also fun. I helped migrate on on-prem site to K8s
at [Trade Me](https://www.trademe.co.nz/a/) and enjoyed that project. I'm interested in more hands-on learning
with distributed systems, and see K8s mentioned on a lot of interesting projects I read about. 

## A use case!

The plan was always to build a service to fetch FAA aeronatical charts and display them to the users. The rest, however,
was vague: "some form of basic flight planning component". I use [Microsoft Flight Simulator 2020](https://www.flightsimulator.com/)
to stay current when I'm not able to fly. The old version had a post-flight debrief component which was perfect for
things like checking how accurately you'd flown instrument approaches or tracked airways. This doesn't exist in the
newer version, so I thought it'd be fun to try and build a replacement. Some do exist ([Tacview](https://www.tacview.net/product/about/en/),
[LittleNavMap](https://albar965.github.io/littlenavmap.html), [Volanta](https://volanta.app/)), but I thought it'd
be fun anyway.

## Goals for the week

 - Get minikube running locally
 - Create dev containers for web and API, and get them running locally
