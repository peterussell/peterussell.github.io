---
title: "PMP Update 7: Web project setup with Next.js and Docker"
date: 2024-01-17 22:53:00
categories: [projects, pmp]
tags: [nextjs, react, docker, pmp]
---

## Preamble

Now we have the web API [running in Docker]({% post_url 2024-01-07-pmp6-running-webapi-in-docker %}), the next
job is to set up a project for the frontend and get that running in a container as well.

I decided on React given it's the frontend language I'm most familiar with, paired with [Next.js](https://nextjs.org/)
which seems to be framework-of-the-month (unless it's out of date by the time I publish this). I have some experience
with Next.js from a little while back, but have been mostly in backend land in the last year or so. Although I chose
Next.js for familiarity, it came as no surprise that I found myself reading the 'getting started' docs again.

The good news is a lot of the changes seem to be for the better, and setup was actually quite straight-forward.

## Setup

The recommended approach is to create the app using `create-next-app`, which prompts for options like using TypeScript, ESLint,
Tailwind, whether to use a `src/` directory, [import aliases](https://nextjs.org/docs/pages/building-your-application/configuring/absolute-imports-and-module-aliases)
and whether to use [App Router](https://nextjs.org/docs/app), which seems like the biggest change from the last
time I used Next.js.

I went with the following (mostly default):
 - TypeScript: yes
 - ESLint: yes
 - Tailwind CSS: yes
 - `/src` directory: yes
 - App Router: yes
 - Customize default import alias? yes
 - Import alias:  `@components/*`

### Import aliases

Import aliases allow you to mostly avoid `../../` navigation in `import` statements. For example, using
import aliases allows you to import a component with something like  `import { MyCustomButton } from '@/components/shared`
instead of `import { MyCustomButton } from '../../../../components/shared`. It also has the advantage of
not running into potential issues if you move files around (although realistically your modern IDE of choice
should be able to handle this). I've used import aliases in past projects and they're usually a help, so it
was nice to see this as a `create-next-app` option.

However with the value I gave above (`@components`) I ended up with `@components/*` mapping to my `/src` directory,
which wasn't really what I was trying to do. What I actually wanted was `@components/*` mapped to `/src/components/*`.

After reading [this post](https://dev.to/justindwyer6/what-import-alias-would-you-like-configured-51n4) I came to the
conclusion that I probably wanted the default option (`@/*`) but with `baseUrl` in tsconfig.json set to `/src`.

```
{
  "compilerOptions": {
    ...
    "baseUrl": "src/",
    "paths": {
      "@/*": ["./*"]
    }
  },
  ...
}
```

This allows import statements like this `import { MyCustomButton } from '@/components/shared`, which *does* add an extra `/`
after the `@` symbol, and which could be fixed by mapping `"@components/*": ["./components/*"]` 
etc., but this has the downside that if you add other directories that you want an import alias for you'd
have to remember to add each one as a new path in tsconfig.json. The approach I took allows new directories 
to be added to `/src/`  without having to update tsconfig.json.

## Configuring Docker

Next.js has great docs for deploying apps with Docker. It's covered in the [official documentation](https://nextjs.org/docs/pages/building-your-application/deploying#docker-image)
and there's an [example project](https://github.com/vercel/next.js/blob/canary/examples/with-docker).

The sample repo gives examples of deploying to either multiple environments (dev/staging/prod) or into a single environment.
I chose the single environment approach for simplicity and because the multi environment project includes Makefiles and
docker-compose which a) adds complexity and b) is likely to clash with using Kubernetes for deployment down the road.

The steps to build a single-environment app were simple, and worked out of the box:

 1. Copy the [example Dockerfile](https://github.com/vercel/next.js/blob/canary/examples/with-docker/Dockerfile) to the project root
    (`/web` in my case, one directory up from `/src`)
 2. Add `output: "standalone"` to the `nextConfig` section in next.config.js
    ```
    /** @type {import('next').NextConfig} */
    const nextConfig = {
      output: "standalone"
    }

    module.exports = nextConfig
    ```
    *NB. I missed this step initially, see 'Troubleshooting' below. Lesson learned, don't skim read the docs ;)

### Troubleshooting

The Dockerfile assumes build output in the `standalone` folder mentioned above. If you somehow forgot to set
up the configuration in next.config.js (\*ahem\*like me\*ahem\*), you'll get something like this error:

 > `"ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref (...): "/app/.next/standalone": not found"`

The solution is just adding `output: "standalone"` to next.config.js as mentioned above.

### Yarn, NPM, and PNPM

The Dockerfile in the example above works with your choice of npm, yarn, or pnmp, by checking for which type of
lockfile you have and calls the appropriate command. So feel free to use your package manager of choice.

## Building the image

From a terminal in the root directory (`/web` in my case, not `/src`):

  ```
  docker build -t web .
  ```

 - `-t web` - tags the image as "web"

If the build succeeds, run `docker images` in the terminal to see the newly created image.

## Running the container

In a terminal, run this to start the container (assuming the container's named `web`) and have it listen
on port 3000.

```
docker run -p 3000:3000 web
```

*NB. See my previous post on [building and running the API project]({% post_url 2024-01-07-pmp6-running-webapi-in-docker %})
for more information on port forwarding with the `docker run` command.*

You should now be able to browse to `http://localhost:3000` to view your new Next.js project running in a
Docker container.

## Next steps

To recap, we've now got the C#.NET API and the Next.js web app both building and running in containers.
So our next goal is to spin them up in minikube and figure out how to send API requests from the app
to the API.
