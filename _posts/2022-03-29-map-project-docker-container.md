---
title: "PMP Update 2: Java+Docker"
date: 2022-03-29 20:58:00 +1300
categories: [projects, moving-map, pmp]
tags: [projects, moving-map, aviation]
---

After a few evenings fiddling around learning how the world of Java packages and
Maven builds works, and how to squeeze that into a Docker container, I'm happy
to announce:

![Hello world](/assets/img/posts/2022-03-29/pmp-hello-world.png)

### Running - *and building* - in containers!
I roughly followed the steps in [https://codefresh.io/docker-tutorial/create-docker-images-for-java/](https://codefresh.io/docker-tutorial/create-docker-images-for-java/)
under "Multi-stage Build".

This is a two-step build, where step one uses an [Apache Maven](https://maven.apache.org/)
Docker image to compile the source into a JAR file, then copies the JAR
to a new Docker image based on [OpenJDK](https://hub.docker.com/_/openjdk)
to run. This has a couple of advantages: 1) it doesn't include the build tools
in the final image, making it more light-weight, and 2) it doesn't require
the JDK or build tools to be installed on the host machine -- the build and
runtime are all done within containers.

### Trello & GitHub

As promised, here are links to the Trello board and GitHub repo:

 - **Trello board:** [https://trello.com/pmp3000](https://trello.com/pmp3000)
 - **GitHub repository:** [https://github.com/peterussell/pmp](https://github.com/peterussell/pmp)
i
### PMP?

Pete's Map Project. A working title until we figure out what it's going to do :D 

### Challenges

Being new to Java, Maven, and building Docker images, a few things caught me out:

 1. The package structure of Java projects. I kept getting a runtime error saying
   that the Java runtime couldn't find a main method at the location I specified
   in `pom.xml` (Maven build file). This turned out to be a combination of having
   an incorrect package/directory structure, and missing the line
   `package nz.httpete.fetcher` at the top of my main class file. I found
   [dstar55's example GitHub repository](https://github.com/dstar55/docker-hello-world-spring-boot) really helpful for understand the correct mapping between these. *(NB. I'm still
   learning this stuff, so either of these may or may not be necessary).*
   ![package structure](/assets/img/posts/2022-03-29/package-structure.png)
 2. A mismatch between the Java version I was compiling for, and the OpenJDK Docker
   image I was using. In the Maven build configuration (`pom.xml`) I was specifying
   a target of Java 17 here: `<maven.compiler.target>17</maven.compiler.target>`,
   but using an OpenJDK Docker image for Java 11. Fortunately this had a helpful runtime
   error which helped debugging.
 3. Understanding how the Maven build system and dependencies worked. I'd say I'm
   fairly familiar with other build and package management systems, but this really
   took a bit of a paradigm shift for me to understand. I found the official docs
   quite 'Linux-ey' and a little hard to follow. This is where I first encountered
   [baeldung.com](https://www.baeldung.com/maven) - which it turns out is sort of a go-to
   place for Java questions and tutorials. It's awesome. I also found it was much faster to troubleshoot
   issues by installing Maven on my laptop, getting the build working, then transferring
   what I'd done locally in the Maven Docker build image.

### Next up

 - I've decided to use Kubernetes to host the project, mainly just as an
   excuse to learn more about it. So the next step is to get a K8s cluster
   running the image locally.
 - I'm also having fun getting back into studying and posting about
   Data Structures & Algorithms, which will be interspersed with these
   project updates.
