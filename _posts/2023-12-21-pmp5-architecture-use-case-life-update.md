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
I've been thinking about what I'd like to learn while also trying to avoid cognitive overload from *everything* being
new and/or unrelated to what I'm doing at my normal job. That led to these decisions (more detail below):

### Platform & language

 - API and services: C#
 - Web: Next.js
 - Infrastructure: Kubernetes, Azure
 - IAC: Pulumi

### CI/CD

 - Independent dev and prod environments
 - Deployment via GitHub Actions
   - Merge to `dev` -> deploy to dev cluster (only appropriate components, eg, web, API, IAC)
   - Merge to `main` -> deploy to prod cluster, manual approval step
 - High-availability: min 2 instances of each critical component

## Detail

### Azure, C#, Pulumi

My day job is mainly C# and Azure, with Azure being fairly new to me which is why it's been thrown into the mix in place
of AWS. So far I've successfully deployed to Azure using [Pulumi](https://www.pulumi.com/), which is similar to Terraform but written in
your programming language of choice, as opposed to YAML for Terraform.

I hesitate to invoke the "[t-shaped developer](https://killalldefects.com/2020/02/22/specializing-vs-generalizing-careers/)"
concept, but I *would* like to focus more on going deep on a language. Migrating the existing work on the API from Java to
C# is an attempt at that, as opposed to working on simultaneous projects across 2-3 languages like I have in the past.

### Kubernetes

Kubernetes is definitely overkill for this, but also fun. I have some K8s experience (I helped migrate on on-prem site to K8s
at [Trade Me](https://www.trademe.co.nz/a/)) and interested in building out a distributed system from scratch. Despite
microservices/K8s falling out of favor a little bit recently, I see see K8s frequently mentioned as being used in interesting
projects or companies I read about.

## A use case!

The plan was always to build a service to fetch FAA aeronatical charts and display them to users. The rest, however,
was vague: "some form of basic flight planning component".

I had the thought while practicing instrument approaches in [Microsoft Flight Simulator 2020](https://www.flightsimulator.com/), which
I do to stay current when I'm not able to fly. [Prepar3d](https://www.prepar3d.com/) (the older-version) had a post-flight debrief component
built-in which was perfect for reviewing accuracy on instrument approaches or tracking airways, but for some reason it wasn't ported to the
newer version. I thought it'd be fun to build a replacement.

Although some do exist ([Tacview](https://www.tacview.net/product/about/en/), [LittleNavMap](https://albar965.github.io/littlenavmap.html),
[Volanta](https://volanta.app/)) they don't quite fit what I'm looking for. The best I've found so far is
[tracking the flight in Foreflight](https://support.foreflight.com/hc/en-us/articles/204115275-How-do-I-connect-Microsoft-Flight-Simulator-2020-MSFS2020-to-ForeFlight-)
(the best real-world-flying app in the world) and using their debriefing tools, but requires a Foreflight subscription and an iPad/iPhone to run on, which is probably a bit
spendy for someone just wanting to debrief flight sim flights.

Either way, the point isn't really to be a world-changing product - it's to build something interesting, learn something new,
[just for fun](https://justforfunnoreally.dev/).

## Goals for the week

 - Get minikube running locally
 - Create dev containers for web and API, and get them running locally
