---
title: Dos and Don'ts for a Product Engineering team
updated: 2020-06-02 08:00
comments: true
mailchimp: true
image: /images/research.png
excerpt: The little brother of the Engineering Principles are the Dos and Don'ts. It's just a list of rules to evolve your team's mindset.
---

You can read this post in Spanish [here](/es/dos-and-donts).

No _Product Engineering_ team is perfect but every one feels the need to improve. That improvement is usually done under some kind of guidelines. There are a lot of posts about **Engineering Principles**. I personally like and get inspired by the ones at [Monzo](https://monzo.com/blog/2018/06/29/engineering-principles) or [Medium](https://medium.engineering/engineering-values-7143c0db0bd6). **Engineering Principles** are necessary but usually they are generic and static.

The little brother of the **Engineering Principles** are the **Dos and Don'ts**. It's just a list of specific rules. They address specific problems that a smaller team has at a certain time. **Dos and Don'ts** need to be reminded constantly until they become part of the team's mindset. They can and will change over time. Write them down somewhere, create a Slack bot to remind them automatically or create a poster and place it where everyone can see it. You can even portray them on blog post, like I'm doing here. The following **Dos and Don'ts** are specific for my current team. Yours can and should be different. This tool may or may not work for your team depending on your context. There is no silver bullet and it is sad that I have to make this disclaimer.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.

{% include mailchimp.html %}

### Ask "why" more than once and be ready to be asked "why" more than once

When doing a presentation to expose a new product initiative or technical strategy, be ready to be asked ["why" as much as five times](https://www.youtube.com/watch?v=FJ0eWm5PxkU). Do your research beforehand and go as deep as necessary to have the answers but keep being pragmatic.

### Ask for data and be ready to be asked for data

In the world of _Product Engineering_, supporting your arguments with data is **mandatory**. Don't just say "we need to build that feature". Along with the **whys**, gather and expose the data to create compelling arguments. If there is no data, you will lose your credibility and respect as a Software Engineer, Product Manager or Product Designer. Every member of a _Product Engineering_ team has the obligation to [create a data-driven culture](https://aws.amazon.com/blogs/enterprise-strategy/how-to-create-a-data-driven-culture/).

![](/images/research.png)

### Do your research. Investigate more than one possible solution

Working in a _Product Engineering_ team requires [creativity as well as process](https://uxdesign.cc/what-can-pablo-picasso-teach-us-about-product-strategy-586664e128f1). The obvious solution is usually [not the best one](https://www.youtube.com/watch?v=M68ndaZSKa8). Research multiple solutions without judging, do timeboxed spikes, gather data, create diagrams, write pros and cons, make a list of your biases and have a **decision day**.

### Do pair programming

Try to [pair program](https://www.youtube.com/watch?v=k3cJjZiZ-cw) the hell out of everything. It has [so many benefits](https://martinfowler.com/articles/on-pair-programming.html) that is hard to find compelling arguments agains it. Use the technique that works best for your situation. Nice if you do Ping-Pong-TDD. Driver-Navigator is also fine.

Disclaimer. Pair programming has challenges, specifically mental fatigue. Use it in a way that benefits you the most. It is [not mandatory to use it all the time](https://twitter.com/dhh/status/1016398757674577920).

### Keep your PR’s small and easy to review

Keep your PRs as small as possible and your code as easy to understand as possible. As with anything else, evaluate and try to [improve your code reviews](/improve-code-reviews) from time to time.

### Keep staging always clean

Multiple teams ship their code to staging, and as fast as possible to production, multiple times a day. At any time anything that's on master can go to production. Keep this in mind when merging a pull request. This is not a problem so don't try to create extra environments to test your code. Some _Software Engineering_ teams out there [drop staging environments completely](https://launchdarkly.com/blog/staging-servers-are-dead-long-live-a-staging-server/). Try to think of problems beforehand and merge code that just works. If you're not sure, put it under a feature flag.

![](/images/version_control.png)

### Make sure the code you ship works in production

Ideally, you would have [automated end-to-end testing in production](https://medium.com/@copyconstruct/testing-in-production-the-safe-way-18ca102d0ef1). Until it becomes a reality, manual test that you didn't break anything when your code hits production. Keep in mind that as complexity increases, manual testing in production becomes harder and harder. So you better start automating soon.

### Measure the code you ship

"Done" means a feature can be measured from a business, product and engineering point of view. This is the only way to [assess that what you build has value](/focus-on-value). For engineering you start with [the four golden signals](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals) and iterate. For business you can start with some of [these key metrics](https://www.ycombinator.com/resources/key-metrics) and iterate. For product you need to [find the metrics that matter](https://www.intercom.com/blog/finding-the-metrics-that-matter-for-your-product/) but you can [start](https://amplitude.com/blog/2016/06/14/10-steps-behavioral-analytics) with some basic [behavioural analytics](https://mixpanel.com/blog/2018/08/01/behavioral-analytics-guide/) and iterate.

### Don’t overcomplicate. Ship simple code

Simplicity is the most [underrated](https://blog.pragmaticengineer.com/software-architecture-is-overrated/) characteristic of software design. Keep it as simple as possible and don't let [entropy](https://www.youtube.com/watch?v=kfffy12uQ7g) become a problem. Your future self will be grateful.

![](/images/online_chat.png)

### Don’t expect somebody to answer you right away on Slack. Nothing is urgent. Ever

[Yeah](https://basecamp.com/guides/group-chat-problems). Also, don't apologize for not answering right away.

### Don’t schedule meetings for everything, sometimes a Slack message is enough

Some even think [meetings are toxic](https://twitter.com/dhh/status/1242935396356354048?s=20) and [online meetings](https://www.youtube.com/watch?v=JMOOG7rWTPg) are even worse. I'm not that radical. But if a meeting is really needed, make sure it is well planned: it has a purpose, documentation to get prepared, an agenda and an output.

### Wrapping up

These rules are supposed to shape the team towards a healthier product mindset and culture. They are part of the utopia and they will never be respected 100% of the time. Our environment is complex and unpredictable and we are better that blindly following a list of rules. Be wise.

Huge thanks to [undraw.com](https://undraw.co) for the illustrations.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.
