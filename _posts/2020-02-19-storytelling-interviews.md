---
title: Storytelling tips for technical interviews
updated: 2020-02-19 16:28
comments: true
mailchimp: true
image: /images/introduccion-machine-learning-sin-hype.png
---

> Do not go gentle into that good night,
> Old age should burn and rave at close of day;
> Rage, rage against the dying of the light.

Humans love stories. We love telling them and we love reading, listening and watching them. Beyond entertainment, stories have allowed us to collaborate and build the world we live in today. Sapiens by Yuval Noah Harari talks in-depth about this.

Given the importance of stories to the human mind, it seems there is no better way to explore it. I would like to share my approach to interviews and to emphasize the importance of storytelling when interviewing candidates for technical positions. Although I assume these tips can also be applied to interviews for product managers, designers, etc.

You must assume the candidate will tell you the story you wish to hear; although most of the time, they won't. Unless they have done a lot of research, the candidate doesn't know the taste of their audience. In this case, you. They will need guidance and the first step is to let them know who is their audience.

## Present the audience

Presenting yourself proves that you care enough about the candidate to let them know with whom they are going to spend the next hour. Knowing what the audience cares about will set the tone of the story.

_I'm David. I'm a Product Engineer. After surviving many startups in Madrid I moved to Valencia. The sun, the beach, the city, the people and the raising tech and startup community brought me here. A few months ago, Creditas adopted me in order to improve the things that I'm supposedly good at: android, backend, leading teams and question Product Managers._

This is not your CV so you must do it under one minute. Prepare it beforehand. Don't improvise. The most important part is to be human and stick to what matters. Mention what you like, what is important to you, what drives you and what motivated you to be where you are. Giving a lot of details about your technical experience maybe is irrelevant to the candidate's story.

## Worldbuilding 🏗

_In any agile product engineering team, you end up with some tasks that are pending to be done. Sometimes it is a ticket on a JIRA board; or just a post-it on the wall; or even a message in Slack from your Product Manager saying it is something urgent. Hopefully not... Anyway... It is a really long journey until a feature is in a machine in the vast wonders of the internet and somebody is using it. I would like you to take me into a journey that takes that feature from to-do to production._

Start the story at the point you think necessary, giving a recap on what happened until that point in which the story is taking place. That world has rules that need to be respected. Mention them if they are important. It has characters that have some abilities and responsibilities. Mention them if they are important. It has a history that needs to be taken into account and it has momentum: a force in the direction the story is led.

## The rhythm of the story 🏃‍♀️

_You have a task waiting to be done. Maybe you go to a whiteboard with a colleague. How do you start?_

Every step of the story is an opportunity to go as deep into a topic as the candidate wants. This rhythm of the story will show what is important to them. At this point, they may explore things like problem space exploration, architecture definition, spikes, etc.
You have spent some time at the whiteboard exploring different solutions. Then you sit down in front of a keyboard and a monitor ready to pair program the hell out of it. How do you do that?
As a guide you should try to keep a constant base rhythm. Accelerate or decelerate slowly enough so the candidate has time to adapt.

## Branching 🔀

_The task is done. Or is it? Well, we know that at least the feature is programmed. It is coded. But that code can't stay on our machine forever. It's useless there since it doesn't bring any value to the user._

By the time the candidate completes the story, from thousand possible paths, only one is going to remain explored. The story always follows the timeline. Flashbacks or jumping between branches is not a good idea because it will only add confusion. So give them a transition and let the candidate explore the path that they think is best.

_Before deploying that code to production, maybe we should invite somebody to review it._

Sometimes there is something specific you want to hear about like testing strategies as “outside in or inside out”. So you will force the candidate to go into places that they were or were not intending to go. That's a forced branch. That's fine if it builds in the direction the story is leading to. You should keep the momentum the story already has and take advantage of it. Observe how the candidates handle the situation, how they get out of it and how they improvise.

At this point, they have a good opportunity to talk about code reviews, staging environments, continuous integration pipelines, trunk based development, etc.

## Transitions and plot twists 😱

_You are almost there. The code is reviewed and the feature is tested in a staging environment. Now is time to deploy to production._

Transitions help the story advance in a certain direction. Most candidates don't know how the story is supposed to advance and what places they need to explore. The candidate can explore topics like feature flags, canary builds, functional testing, deploy failure notifications, engineering metrics, business metrics, analytics, etc. Transitions shouldn't be drastic. They advance the story one tiny bit at a time. This gives the candidate time to adapt and be prepared to build the story further.

_When we deploy to production, we hope everything will go well. And this time it seems everything has gone well. However the next day the Product Manager reports that there is a bug in the feature that you deployed. How do we hunt that bug down? What tools do we need?_

Plot twists, on the other hand, take the candidate and put them in a completely new situation. Plot twists don’t change context completely. They build upon the context that the candidate already has but it doesn't take full advantage of the momentum of the story. Plot twists exist because there is some conflict. Something unexpected but familiar has happened. Let the candidate explore topics around bug hunting, rollback mechanisms, error reporting, logs, etc.

_Products are not static, they evolve... Software is supposed to change. If it wasn't, it should be hardware. We need to be as lean as possible working in small feedback loops to be sure that what we do really adds value to the user. From an engineering perspective, the simplest setup is having a frontend client and a backend that serve a REST API. As our product evolves, our API will also evolve. Let's talk about what things must take into account when we evolve our API in such iterative ways._

Stories are continuous. Events don't just happen without any reason. Any event has a cause. We know and understand the reason behind them and we are able to respond in a proper and proportional way. Transitions need to give enough context to understand why something is happening. Only then, the candidate will be able to respond in a appropriate way. You want the candidate to understand what you are asking. If the branch you want to explore opens a whole new world, the transition needs to be richer.

## Levels of abstraction 🕋

_After many iterations, our product is more successful than we ever imagined. The load on our services is increasing day by day. We are starting to see traffic spikes. What do we need to take into account and what do we need to do at this point?_

You started the story at a very low level, talking about testing, code quality, pair programming, architecture, etc. You are now at a different level talking about more abstract concepts. You shouldn't expect concrete answers as the candidate will not try to be specific anymore. As the interview goes on and the story gets built, the mental fatigue increases. It is a good idea to go from specifics to more abstract topics. At the end of the story, you are merely talking about higher level ideas or conclusions.

## Wrapping up 🎁

We are coming to an end. We went from writing code, testing, code reviews to scaling our system beyond what we could imagine. We talked about bugs, metrics, processes, continuous integrations and all that has helped us build a story. Now it's your turn to ask questions that you may have about any topic.  
Before transitioning to questions that the candidate may have, make a quick recap of the story they have just told you. Show that you listened. You prove that you care. And that makes them care too.
