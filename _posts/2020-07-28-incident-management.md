---
title: Incident Management. Not everything is critical
updated: 2020-07-28 08:00
comments: true
mailchimp: true
image: /images/alert.png
excerpt: At some point in a Product Engineering team you will need to address some issues related to managing incidents. Not everything is critial and urgent.
---

You can read this post in Spanish [here](/es/incident-management).

At some point in a _Product Engineering_ team you will be tired of every [bug](https://xkcd.com/1700/) or issue being urgent and critical. This usually happens after dealing with several problems informed by late night Slack messages. At this point nobody really knows which issue is an urgent incident, which is worth waiting to the next morning and which isn't even something worth solving. All this is harmful for the team's culture and for the business itself. When everything becomes urgent, nothing is urgent and nobody cares anymore.

Unlike other processes, Incident Management doesn't emerge naturally. It's a defensive mechanism agains [chaos](https://www.youtube.com/watch?v=GdTMuivYF30); it needs to be created and agreed on.

You can't fix it all at once and you can't do perfect from the beginning. But you need to start somewhere. This post is about how to go from having nothing related to Incidenct Management to having something.

## Where to start

You may feel the urge to adopt [Pager Duty's incident response process](https://response.pagerduty.com/) or [Atlassian's Incident Management process](https://www.atlassian.com/incident-management/handbook/incident-response). Or even to use what's in this post. Be careful. I'm writting what has worked for my team. Don't just copy. You have a brain. Use it.

My team literally has all this written down in a Counfluence page. Writting it down is not enought. Everyone should agree to use it. You need to educate through workshops, talks and training sessions giving real life examples and making sure everybody understands the basic process you are about to set in place. Everybody must be on board.

First thing you probably need to address is everything being urgent and critial. For that you need to define what is really an [incident](https://xkcd.com/838/) and a few severity levels. Give some real life examples and instructions on how to report each incident. Don't worry if it isn't perfect. With every incident you will adapt and iterate over it.

![](/images/alert.png)

## What is an incident

An incident is an event that causes disruption to a service or a reduction in the quality of a service which requires an emergency response.

That las part is important. Emergency response. An incident has such an impact that gives someone the right to disturb somebody else's life.

## Severity levels

When you detect an incident try to analyze the impact and apply a severity level. What is severity? The severity of an incident is derived based on the effect of that incident on the system. It indicates the level of threat and how can it affect the system.

### Sev1

Critical incident that involves irreversible damage to the company or brand reputation, it must be solved instantly even if it means to take the site down. This should never happen.

Example: Security vulnerability exposing customer data, images showing violent content appear on the home page, customer accounts have been hacked.

Typical Response: Call the responsible Engineering Manager.

### Sev2

Critical system incident actively impacting many customers' ability to use the product. It is an incident which is a major blocker and has affected the functionality of the whole service. It affects the business as a whole.

Example: Showing big differences in prices, discounts that shouldn't exist but apply to all the products, site down.

Typical Response: Send a message on Slack on the #radar channel. If nobody responds in 10 minutes, call the responsible Engineering Manager. The fix is to be deployed as soon as possible.

### Sev3

Stability or minor customer-impacting incidents that require immediate attention from service owners. Priority number one for the team involved. It can be fixed the next morning.

Example: One category is not working. Emails are not being sent.

Typical Response: Send a message on Slack on the #radar channel. If it happens out of office time it could be fixed the next day.

### Sev4

Minor issues, cosmetic issues or bugs, requiring action, but not affecting customer ability to use the product, must be prioritized.

Example: One customer didn’t receive a confirmation email. One customer bought an item without stock. The image of a product is not correct.

Typical response: Only in working hours. Talk to the Product Owner and Team Lead of the team responsible for the service/feature.

![](/images/bug_fixing.png)

An incident's severity level can be upgraded if it is not solved in a resonable amount of time. If that happens, you'll need to take more severe and drastic actions.

## Incident response

Keep calm. Don't pressure anybody. People work and communicat poorly under pressure.

You don't need to set up all the roles of a mature _Incident Management_ process. The most important one that you do need is the **Communications Manager** or **Communications Lead**. Somebody will take this role during an incident. This person keeps everybody informed about the status of the situation. What has been detected, what is being investigated, what the team has found, timeframe for resolution, actions that are being taken, etc.

If updates are not communicated regularly people get nervious. Even a message like _"We are still investigating."_ helps to keep the nerves under control. Give updates every few minutes. It may seem like a lot but believe me, it's not. When in crisis mode, overcommunicate. I can't stress this enough.

Later on you can add other roles as **Commander** or **Deputy**, set up **On-Call** processes, etc. All depending on your needs and customized to your organization and culture.

## Wrapping up

Start small. Something is better than nothing. And done is better than perfect. Educate and give people time to understand and use the process. Ask for feedback, use each postmortem to analyze the Incident Management process and try to improve it just a tiny bit.

Huge thanks to [undraw.com](https://undraw.co) for the illustrations. You can subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.
