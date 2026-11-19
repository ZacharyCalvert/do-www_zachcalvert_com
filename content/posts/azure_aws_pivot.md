+++
date = '2026-11-19T08:06:00-05:00'
draft = false
title = 'Azure to AWS Pivot'
tags = ["architecture", "engineering", "DevSecOps", "AI"]
weight = 178
+++

# Azure Failed Me

I started working on a pet project to flex some AI muscle, test Gemini, and build an app I wanted to use called [Planned Wallet](https://www.plannedwallet.com).  The project itself is an isolated, personal budgeting app meant for tracking expenses, balancing credit cards, and trending wealth.  This post isn't about the app, but rather the cloud experience I've had.

I chose Azure as my cloud host given it was what I used at McKesson, and seems to be trending given MS Teams and o365 being the massive enterprise hooks.  I have mixed feelings about Copilot and have had some personal frustrations with the budget limit controls Azure offers, but I felt the choice was worth the full stack and architecture experience.  

My first week was building the local framework with Gemini including a developer focused docker-compose, in-RAM database mirroring a PostgreSQL repository contract for fast testing, golang back-end, NextJS front-end, and nginx gateway routing between the static content hosted on the root URL path, and the APIs offered on the /api path.  Next came the cloud provisioning.

All of this is on my personal budget and savings, intended to test the waters of a startup while getting some more experience on cloud administration, AI directing, and just enjoying building something again.  Initially, things looked alright with Azure.  Cost wise, Azure was reasonable on spend at around $200 a month for a stage and production environment with a very easy means to scale up if things took off faster than I anticipated.  I had encryption at rest, TLS network encryption, network segmentation and IAM assigned with least privileges for the various components.  

Then, provisioning failed.  Not just failed in a "hey, retry me" kind of fashion, but in a "Azure says this is available, the CLI command prompt says it is available, but Terraform is failing provisioning".  Come to find out, the CLI and Azure's site were lying about their PostgreSQL burstable tier for westus3.  No big deal, I'll just pivot to a region that has capacity.  I land on the "centralus" region and provision successfully.  

I want to deploy and promote to production when ready, so I sit on a stage only cloud environment. I wrap up what I believe to be the minimum viable product and kick off the prod provisioning.  Wouldn't you know it, "B_Standard_B1ms" SKU is not available.  Now I'm irritated.  It certainly was three days ago.  So, I fire up a ticket thinking maybe this is a quota issue given I'm a new subscription with low spend.  I'm not on the free tier, but hey, Azure hasn't made a dime off of me.

I've got receipts of this next mess.  I fired my support ticket and receive an ack at Tuesday, November 17th, 2026 at 10:17 AM.  First response is at Tuesday, November 17th, 2026 at 11:46 PM, after I get to bed.  The reply is simple:
> Kindly confirm the SKUs and New limit to proceed further

Great, right?  This is just a quota problem.  I checked my email first thing when I woke up (who doesn't get excited about going to production) and sent my reply just over 5 hours later at Wednesday, November 18th, 2026 at 4:53 AM.  No response that day.  

My next response is almost a full 24 hours later at Thursday, November 19th, 2026 at 3:33 AM:
> Currently, we are experiencing capacity constraints for Burstable compute in this region, which may prevent or delay the deployment of Burstable servers. We sincerely apologize for the inconvenience this may cause to your deployment plans.

And they direct me to a general purpose product SKU at 5 times the current cost.  I could have been informed of this the day before.  It was clear I'm too small to get same day response, and now my trust with them is ruined and my cost estimates are ruined.

I tend to think, decide, and act.  I get my best thinking done with a cup of coffee and a shower.  By 7 AM, after a weightlifting session, coffee, and shower, I had decided I would not trust Azure with my new toy and instead would pivot to AWS.  By 8 AM I had Gemini reworking pipelines and Terraform.  I tore down all Azure hosted assets.  No, I do not trust Gemini with my cloud admin credentials, and I likely never will.  I'm still of the mindset that humans have to be in the loop and still need to do manual reviews.  

By 9 AM Azure was scorched earth.

By 10 AM I was reworking checkov findings and capturing documentation on next steps to administer DNS and begin the provisioning in phases.  

11 AM was lunch and a quick trip to the store to pick up a Christmas gift I had ordered.

Noon was asset deploy and DNS provisioning.

By 1 PM cloud assets were up and ready for static content and golang microservice container delivery.

By 3 PM I was fully up and running, both [prod](https://www.plannedwallet.com) and [stage](https://stage.plannedwallet.com) environments.  Manual testing and review done in time to take a short walk outside on this lovely Thursday.

If anything, startups and greenfield applications have been handed a brand new means to pivot based on the slightest of inconveniences or cost inefficiencies.  [Prod](https://www.plannedwallet.com) and [stage](https://stage.plannedwallet.com) are back under a cost threshold of $100 a month.

What could have been a 2 month cloud asset pivot, requiring both AWS and Azure expertise, turned into a 6-hour ordeal.  Azure, you failed me.  Gemini, you impressed me.