---
title: "BBB #1 - Back Hunting"
slug: bbb-1-back-hunting
date: 2024-05-30 19:41:45.9870544 +0930 ACST
draft: false
categories:
- All
- Application Security
tags:
- Bug Bounty
comments: true
ShowToc: true

cover:
  image: "bbb-1-back-hacking.jpg"
  alt: 
  relative: true
---

G'day! I'm Jakob, an Application Security consultant from Australia, welcome to my Bug Bounty Blog (BBB).

After a long hiatus from bug bounty, I have decided to fire up nikto again and start scanning the web for fun and profit. This blog is all about committing what I'm learning and thinking to paper, and to share it with the world.

## Why did I stop bug bounty?

Good question, thanks for asking. I started bug bounty hunting shortly after the pandemic hit in 2020. While everybody else was farming turnips in Animal Crossing, I turned to bug bounty as a way to put my isolated time to good use, and to give me something to dive into as a distraction in a challenging time.

I had a moderate amount of success. I found a bunch of cool bugs, from broken access control to cross-site scripting. I even managed to find an unrestricted file upload resulting in RCE, which earned me a [Bugcrowd MVP award](https://www.bugcrowd.com/blog/congralations-to-our-mvps-for-q2). I had a good time.

Then, 2021 ended up being the busiest year of my life. I was approached by the University of South Australia to develop and deliver a new course on Secure Software Development. Rather than scaling back my day job, I decided it was a good idea to moonlight writing the course. Long days of thinking at my laptop, followed by long evenings of writing was far more taxing that I anticipated.

My partner and I also built a house which required a whole new world of learning and project management. We also had a baby, which the parents out there will understand is the most incredible thing that also happens to turn your life upside down.

Fair to say, I had no time for bug bounty hunting, and I haven't really since then. I still work as an AppSec consultant, I still teach one semester a year at UniSA, and I still love spending time with my family. I've also taken up marathon running, though that's a topic for another day.

But, I've settled into my new normal, and I can't help but be drawn back to bug bounties...

## Why am I starting again?

This is an important question. Why spend my free time trying to find bugs in software, when it would be so much easier to play some videogames (which, don't get me wrong, I love to do). Understanding my own motivations will also help guide my approach to bug bounty. I may approach hunting very differently if my goal is to learn compared to if my goal is to make a few dollars on the side. Here's what I have come up with:

I am returning to bug bounty hunting to:

- Focus my learning and development as an AppSec professional.
- Practice and improve my offensive security skills.
- Try out, or better, build new tooling and methodologies.
- Gain experience. The more bugs I've seen, the better I am at my job.
- Meet others hackers.
- Have a bit of fun.

## Why blog about it?

I'm a firm believer that writing is a part of learning. This blog is mostly for me and my learning process. But, if I'm going to go to all this effort to write down my bug bounty journey, why not share it with the world in the hope that others interested in bug bounty hunting or application security in general will learn something with me.

I plan to keep this blog pretty low key. This isn't a report for a client, and it isn't a highly refined and edited piece of journalism. It's quick and it's relaxed, letting me spend more time learning and hunting.

## Plan for the coming week

Here's how I plan to tackle the coming week:

1. **Set a schedule**  
   Long-term growth comes from consistency, and the best way to achieve consistency is to build habits and routines that allow us to dedicate regular uninterrupted blocks of time to our pursuits. I need to schedule in time for bug bounty, and everything beyond that is a bonus.
2. **Reactivate accounts**  
   I have the benefit how having been here before. I have accounts on Bugcrowd, Hackerone, GitHub, Shodan etc etc. However, it'll no doubt take some time to log in, change passwords, and battle with MFA.
3. **Set up my hacking environment**  
   Similarly, I know I'm going to need Burp Suite and all the extensions, Firefox and all the add-ons, VS Code and Docker for building automation. I need to make sure my laptop is set up with the tooling and licenses needed for hunting.
4. **Go over old notes**  
   Last time I was bug bounty bunting, I had begun to document a methodology and build out automation to help with my hunting. There's a good chance I will want to rebuild everything bigger and better, but I'll have a good leg-up if I start where I left off rather than from scratch.
5. **Begin researching latest tooling and techniques**  
   While the foundations remain the same, the tools and techniques used by hunters in 2024 may have evolved from those used in 2020. How are people approaching subdomain enumeration? Is everything ported over to Rust now? Who are the people to follow online? I need to get back in the loop.
6. **Begin planning automation**  
   I'm wary of over-investing in automation too early as a distraction from getting my hands dirty. I'm here to hack, and no amount of over-engineering a notification system that pings Slack everytime a developer makes a coffee is going to find bugs. That can come later. However, I'd prefer not to restrict myself to testing the most obvious targets in a program, so I'll want to start with some basic recon automation to get things going.

Sound like a plan? Good. See you again next week.

\- Jakob