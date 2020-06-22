---
title: Improve code reviews without going pro
updated: 2020-05-30 08:00
comments: true
mailchimp: true
image: /images/code_reviews.png
excerpt: At some point in a Product Engineering team you will need to address some issues related to code reviews. You don't have to go pro to do it.
---

You can read this post in Spanish [here](/es/improve-code-reviews).

At some point in a _Product Engineering_ team you will need to address some issues related to code reviews. In some teams, code is pushed without barely looking at because reviews are too light. In others, code stays there for too long because reviews are too harsh. In others, pull requests are completely abandoned because nobody actually reviews code in their day to day.

Some more mature teams like Netlify arrive at [creative solutions](https://www.netlify.com/blog/2020/03/05/feedback-ladders-how-we-encode-code-reviews-at-netlify/) like adopting a metaphor to give feedback on code reviews. Others, like Squarespace, create [more extended](https://engineering.squarespace.com/blog/2019/code-review-culture-part-2) [guidelines](https://engineering.squarespace.com/blog/2019/code-review-culture-part-1) to submit and review pull requests. Google as an even more [extensive guide](https://google.github.io/eng-practices/review/reviewer/standard.html). It doesn't have to be so pro. It can be something simpler. I want to share my approach for the teams I worked with. We actually have this written down in a Confluence page.

![](/images/code_reviews.png)

As a rule of thumb, a pull request must be reviewed by at least one team member from another team. It doesn't avoid knowledge silos, but it helps to have an idea of what is happening in other teams. For that, we need to have some common rules about submitting and reviewing PRs, so the experience is as uniform as possible.

## Submitting

The responsibility of the submitters is to make the code as easy to understand as possible. To make the reviewer's job easier, the submitter should:

#### Give enough context

Use a [code review template](https://help.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository) and answer the following questions:

- What does this PR do?
- Why do we want to do that?
- What high level changes did you make to the code?
- What other information should the reviewer be aware of?

Provide information like UI screenshots or GIFs, link to the JIRA ticket, etc.

#### Make the PR a manageable size

More than 300 changes is a red flag. More than 10 files is a red flag. There are exceptions like renaming a package or moving files around.

![](/images/pull_requests.png)

#### Do not submit any code that is not used on the same PR

Unless the code is intended to be a public API consumed by others. This is controversial, but if the code we push to production is not used, how can we ensure we are adding value?

## Reviewing

The responsibility of the reviewers is to ensure they accept the code as theirs and will take care of it as if it was written by themselves. Different software engineers will look for different aspects of the code, but to ensure a fluid experience for everyone the reviewer should:

#### Buy a ticket for at least the 4 buses of code review

First thing in the morning, before lunch, after lunch and last thing in the afternoon. Having at least four times in the day where code is reviewed, helps quite a lot. It ensures PRs don't stay too long without anyone looking at them. It also avoids personal Slack review reminders. You can use [Trailer](http://ptsochantaris.github.io/trailer/) to accelerate your GitHub workflow but it's not mandatory.

![](/images/bus_stop.png)

#### Ensure the PR fulfills the merge-able checklist

Don't be too light on the code review. Remember that you are accepting the code as your own. Also don't be too harsh. Different software engineers have different ways of solving a problem. For a code to be accepted, it only has to fulfill that:

- It achieves the given task
- It has been tested and the tests are passing
- It follows the agreed architectural guidelines
- It has a [manageable size](https://geshan.com.np/blog/2019/12/how-to-get-your-pull-request-pr-merged-quickly/)
- There is no unused, dead or duplicated code
- The system is not going to be harmed
- It doesn't introduce [unnecesary complexity](https://youtu.be/kfffy12uQ7g)

#### Do a follow up

Return to the PR if there are requested changes and propose alternatives to solve them. Do a [pair programming](https://martinfowler.com/articles/on-pair-programming.html) session if there are too many requested changes.

## Wrapping up

Adapt it to what your team actually needs. Address the issues your team is struggling with and write something down. If you want to go pro, do it. Write more extensive guidelines or [automate even further](https://www.freecodecamp.org/news/how-to-automate-code-reviews-on-github-41be46250712/). But you don't need to go that far to start improving code reviews.

Whether or not you should be even doing pull requests is a topic for another post.

Huge thanks to [undraw.com](https://undraw.co) for the illustrations. You can subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.
