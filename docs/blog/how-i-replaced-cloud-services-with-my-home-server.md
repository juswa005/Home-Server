# How I Replaced Cloud Services With My Home Server

## Subtitle

How I moved my files, photos, music, Git repositories, and server management tools onto an Ubuntu machine at home.

## Draft

For a long time, I kept relying on a mix of cloud services for things that felt small on their own but added up over time.

My files lived in one place. My photos lived somewhere else. My music library needed a separate solution. My personal code projects were hosted on another platform. Even basic server management usually meant depending on another hosted dashboard or service.

None of that was necessarily bad, but I kept coming back to the same thought: I already have hardware. I already have a network. I already like tinkering with Linux. So why not build a home server that gives me the same convenience while keeping the services under my own control?

That idea turned into an Ubuntu home server that now handles a big part of my personal cloud setup.

This is the stack I ended up with, why I chose it, and what I learned from replacing several cloud services with self-hosted alternatives.

## Why I Wanted To Self-Host

My goal was not to become a full-time infrastructure person at home.

I wanted a setup that would do a few practical things well:
- keep my files in a place I control
- back up and browse photos easily
- stream my personal music collection
- host my own Git repositories
- manage everything remotely without making it painful

The bigger motivation was convenience with ownership.

Cloud services are easy to start with, but over time they can make your setup feel fragmented. Self-hosting looked like a way to pull those pieces back together into one system that I could understand and maintain myself.

## The Base Setup

I built the server on Ubuntu and used Docker for the application layer.

That gave me a setup with a few nice properties:
- the host system stays relatively simple
- each app can be managed independently
- services are easier to restart, update, or replace
- data stays organized in per-app directories

For remote access, I use Tailscale. That part made a huge difference.

Instead of thinking about public exposure first, I could treat the server like a machine that lives on my own private network, then securely reach it from my other devices. That removed a lot of the friction that usually scares people away from home hosting.

## The Services I Replaced

Here is the stack currently running on my server:

| Need | Self-Hosted Service |
| --- | --- |
| File sync and personal cloud storage | Nextcloud |
| Photo backup and gallery | Immich |
| Music streaming for my own library | Navidrome |
| Personal Git hosting | Gitea |
| Web-based container management | Portainer |

What I like about this setup is that it feels focused. Each tool does one job clearly, and together they cover most of the services I actually use.

## Nextcloud: My Personal File Cloud

Nextcloud replaced the biggest mental category for me: file storage.

This is the part that feels closest to replacing services like Google Drive or Dropbox. I can store files, browse them through the web UI, and keep the service on hardware I control.

What I like about it:
- it gives me a familiar cloud-like experience
- it fits naturally into a self-hosted stack
- it makes the server feel immediately useful, not just experimental

If someone asked me what made the home server feel real for the first time, it would probably be this.

Suggested screenshot:
- Nextcloud dashboard or Files page

Image placeholder:

```md
![Nextcloud dashboard](../screenshots/nextcloud-dashboard.png)
```

## Immich: Replacing My Photo Backup Workflow

Immich is probably one of the most satisfying parts of the whole setup.

Photo libraries get large quickly, and cloud photo apps are convenient enough that it is easy to stay locked into them. Immich gave me a self-hosted way to keep that same feeling of browsing a timeline and managing a media library without sending everything to a third-party service.

What stood out to me:
- it feels modern and polished
- it solves a very real storage problem
- it turns the server into something personal, not just technical

There is something nice about seeing your own photo timeline come from your own machine.

Suggested screenshot:
- Immich timeline or gallery grid

Image placeholder:

```md
![Immich timeline](../screenshots/immich-timeline.png)
```

## Navidrome: Streaming My Own Music Library

Navidrome handles the music side of the setup.

This one is a little different from replacing a mainstream streaming platform because I am not trying to mirror the entire music industry. What I wanted was a clean way to access and stream my own library from anywhere.

Navidrome fits that perfectly.

What I like about it:
- lightweight and focused
- good fit for a personal collection
- easy to understand as part of the server

It is one of those services that does not need to be flashy to be valuable.

Suggested screenshot:
- Navidrome library page or player view

Image placeholder:

```md
![Navidrome library](../screenshots/navidrome-library.png)
```

## Gitea: Hosting My Own Repositories

For personal Git hosting, I run Gitea.

This covers the part of the stack that normally lives on GitHub or GitLab for a lot of people. I still like public code platforms, especially for collaboration and open source visibility, but for personal hosting and experimentation, Gitea is a really good fit.

It gives me:
- a simple web interface
- Git over SSH
- a home for personal repositories that I want to keep under my own control

It also fits the overall theme of the project really well: replacing dependency on external services where it makes sense, without making the setup harder than it needs to be.

Suggested screenshot:
- Gitea dashboard or repository list

Image placeholder:

```md
![Gitea dashboard](../screenshots/gitea-dashboard.png)
```

## Portainer: A Better Way To Manage Containers

Portainer is not really replacing a mainstream consumer cloud service in the same way as Nextcloud or Immich, but it absolutely improved the experience of running the server.

It gives me a web interface for Docker management, which makes the whole setup easier to inspect at a glance.

That matters more than it sounds like it should.

Once a home server starts hosting multiple apps, having a clear visual overview helps reduce the mental overhead of maintaining it.

Suggested screenshot:
- Portainer dashboard or containers list

Image placeholder:

```md
![Portainer dashboard](../screenshots/portainer-dashboard.png)
```

## What Worked Better Than I Expected

A few things surprised me in a good way.

First, Docker made the whole system much easier to keep organized than I expected. Each app has its own deployment directory and container setup, so the server does not feel like one giant pile of manually installed software.

Second, Tailscale removed a lot of the pain around access. Instead of immediately dealing with complicated public networking decisions, I could securely reach the server through the tailnet and keep things simpler.

Third, the stack feels genuinely useful day to day. This is not just a lab machine for experiments anymore. It replaced real tools I actually use.

## What Was Harder Than Expected

The hard part of self-hosting is not usually getting the first container running.

The harder part is thinking like the person who has to live with the setup later.

That means questions like:
- how will I back this up?
- how will I update services safely?
- what should actually be exposed?
- what breaks if the machine goes down?
- what details should stay private if I write about this publicly?

Those questions are not reasons to avoid self-hosting, but they are the real work behind it.

In my case, even writing documentation for the server made that obvious. It is easy to forget how many little details matter until you try to explain the setup clearly.

## Tradeoffs

I do not think self-hosting is automatically better than cloud services in every case.

What you gain:
- more control
- better visibility into your own stack
- the ability to shape the system around your needs
- a stronger understanding of how your services actually run

What you also take on:
- maintenance responsibility
- uptime responsibility
- backup responsibility
- security decisions that cloud providers would normally hide from you

For me, that tradeoff is worth it because this setup replaces services I care about in a way that feels practical, educational, and honestly just fun.

## What I Would Improve Next

My current setup works, but it is still evolving.

The next improvements I would think about are:
- better backup documentation and recovery steps
- tighter documentation for which ports and routes are intentional
- a cleaner public-facing writeup with screenshots from each app
- maybe a clearer reverse proxy story for selected services

That is one of the nice things about a home server: it can start small, solve one real problem, and then gradually become a more complete personal platform.

## Final Thoughts

The best part of this project is that it stopped feeling like "setting up a server" and started feeling like building my own version of the services I actually want.

Instead of paying for convenience somewhere else, I now have a system at home that handles files, photos, music, repositories, and service management in one place.

It is not identical to the cloud platforms it replaces, and it does not need to be.

It is mine, it works, and I understand it better every time I improve it.

If you are thinking about self-hosting, I would not start by trying to rebuild the whole internet. I would start with one thing you already use every week, replace that first, and let the rest grow from there.

## Optional Ending Line

Self-hosting stopped being just a technical experiment for me once it started replacing real services I depended on. That is when a home server became worth it.
