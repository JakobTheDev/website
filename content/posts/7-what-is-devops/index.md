---
title: "What is DevOps?"
description: The simplest introduction to DevOps and the benefits it can provide to your organisation.
slug: what-is-devops
date: 2019-07-18T15:45:00+10:30
draft: true
categories:
- All
- Technology
- Software Development
tags:
- DevOps
- DevSecOps 
comments: true
ShowToc: true

cover:
  image: "devops.png"
  alt: "A figure of 8 loop showing the 8 phases of the DevOps lifecycle: Plan, Code, Build, Test, Release, Deploy, Operate, Monitor."
  relative: true
---

DevOps as a philosophy for software development has been around for some time now. It has evolved from being a buzzword, the new hip thing in software development, to a tried and tested practice by organisations of all sizes. But, for people and organisations who are considering DevOps or have recently adopted a DevOps approach, it can be difficult to see the benefits of DevOps and how it works in practice.

To help break through this barrier, I am writing a series of articles that serve as an introduction to DevOps and related concepts, and you have stumbled across the first article in the series. I hope you enjoy them and find them useful. Onwards!

## DevOps — Put Simply

At its core, DevOps is a set of tools and practices that help organisations build, test and deploy software more reliably and at a faster rate. DevOps is enabling organisations to evolve and deliver their products more quickly than those with a traditional development and release cycle, which can provide a competitive edge. Rather than pushing out a release once a fortnight or longer, new features can be delivered to the user daily, and bugfixes can be deployed in hours, all following the same repeatable automated pipeline.

Complementing this tooling is a culture of collaboration between software developers and IT operations, two teams that are traditionally separate and often have conflicting goals. Developers want the ability to release new features and bugfixes quickly with little friction to provide value to users as soon as possible. Operations more are concerned about stability, ensuring that everything is running smoothly with high availability. Agility and stability can be contradictory and can lead to a development methodology which leaves neither developers nor the operations team happy.

DevOps helps satisfy both of these needs. It provides agility for developers, allowing for multiple releases per month, week or day, depending on the appetite of the organisation. At the same time, automated testing and manual reviews at each stage of the build process assures the operations team that defective builds are failed well before they land in a production environment. Automated provisioning and testing of infrastructure through infrastructure-as-code improve the reliability of deployments, leaving one less thing operations needs to worry about. Finally, logs and metrics are collected at each stage of the process, providing more visibility to developers and the operations teams alike.

## Benefits of DevOps

The main benefit of DevOps stems from the fact that automated processes can do repetitive actions faster and more reliably than people. It’s not feasible nor productive for an organisation to have developers or other staff building and deploying code all day long. Automating these repetitive tasks frees up developers to do what they do best — hack code.

What this does is allows code to be built and deployed within minutes, limited only by how an organisation chooses to manage its DevOps pipeline. This means that the time between developing a feature or bugfix and delivering an improved experience to the end-user can be significantly shorter, resulting in more satisfied users.

It also creates a better feedback loop. The sooner new features are delivered to users, the sooner an organisation can gather feedback and metrics and gain insight into what users like about their product. This allows organisations to remain agile and provides a better environment for innovation.

## What is DevOps, really?

It’s all well and good to talk about DevOps from a conceptual point of view, but what is DevOps really? Let’s step through a simple DevOps pipeline to deliver a feature from the developer’s screen to the user’s to get an idea for what DevOps can do.

> **Note:** At Taptu, we follow a gitflow workflow, but you can adjust your DevOps pipeline to work with the source control workflow that works for you.

A developer finishes writing the code for a new feature for their widget. They commit their code to their feature branch which kicks off some lightweight tests on their development machine, checking for any code styling issues while also scanning for packages with newly disclosed security vulnerabilities. The developer submits a pull-request to merge their code into the code repository, which sends a notification to the team chat.

Another developer in the team reviews the code changes, and after finding no issues in the code, approves the pull-request. The code is automatically merged into the develop branch which kicks off the build process. The build server clones the develop branch, installs all package dependencies and builds the widget. The build server runs unit and integration tests to make sure the new feature didn’t cause any regressions in other parts of the widget.

Each of the tests is passed and the build succeeds. A new container is automatically provisioned in the cloud according to the best-practice configurations defined within the codebase, and the widget is deployed.

At this point, an organisation has two options. They can choose to automatically release the updated widget into the production environment and make the feature available to all users, or to a select group of users who have opted-in to receive the latest feature. Automated deployment into production is called Continuous Deployment (CD).

Alternatively, the organisation may choose to only release the feature into a User Acceptance Testing (UAT) environment and manually approve a release into production based on a pre-defined schedule. Adding a manual approval process into the pipeline is commonly referred to as Continuous Delivery (another flavour of CD).

Regardless of whether a manual step in involved, additional automated testing is performed once the widget is successfully deployed into production. Further tools collect metrics on performance and user behaviour which are presented to IT operations and development teams to provide live feedback, highlighting potential bugs and helping to shape new features.

This is a fairly typical process for a basic DevOps pipeline, but the specifics are up to the organisation. Some organisations favour fast deployments into the production environment, hiding new features behind feature flags allowing for a staged release to the user base. Others will prefer using a more traditional dev, test and prod environment structure, where features are batched and slowly released through multiple manual gates before deployment into production.

DevOps can be tailored to the specific needs of an organisation or project. The process tends to evolve, adding additional tests to produce a more secure application, or finding ways to optimise the pipeline for faster builds and less human intervention.

---

**Thanks for reading!**  
If you enjoyed this post, follow on [Twitter](https://www.twitter.com/@JakobTheDev) or [Mastodon](https://infosec.exchange/@JakobTheDev) for more content. If you have any feedback or suggestions, leave it in the comments below and I'll do my best to get back to you.
