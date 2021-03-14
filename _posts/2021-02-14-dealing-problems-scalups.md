---
title: Dealing with the problems of scaleups
updated: 2021-02-14 00:00
comments: true
mailchimp: true
image: /images/scalups_cover.png
excerpt: How to deal with some of the problems that come with the sacrifices needed to scale a company that fast?
---

You can read this post in Spanish [here](/es/dealing-problems-scalups).

[Ried Hoffman](https://twitter.com/reidhoffman) talks in his book [Blitzscaling](https://www.blitzscaling.com/) about the sacrifices a company needs to make so it can scale aggressively fast. And when your job is to lead a team and you join a company that's blitzscaling, nobody tells you what was sacrificed. But don't worry. One way or another, you'll find out.

![](/images/scalups_cover.png)

There may be structural problems, missing information, lack of trust between teams, poor defined business goals, lack of a unified engineering or product strategy, communication probles between departments, huge dependencies between teams, a lot of bureaucracy and the list goes on and on. I've faced some of these problems. I tried to solve some, I had to accept some and I'm still frustrated with others. I want to share with you how to identify and deal with some of them.

## The newcomer
As a leader your success will be measured by the success of your team plus the success of the people you influence. Your primary focus should be to make your team achieve their goals. Your seconday focus should be to gain the credibility and trust of your peers and managers. Even if you have to take care of both at the same time, always prioritize your team. Ask for forgiveness instead of permission. It's more valuable to get your team achieve the business goals than to please your peers. So if you have to choose between making a decision to get your team on track and waiting for approval from other departments, choose what's best for your team. This is only sustainable in the short term. And it has a lot of risk because if you make a decision and your team underachieves or fails, other people can use your decisions as an argument against you or against your team.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.

{% include mailchimp.html %}

Some of the most common anti patters you'll see in a team are: lack of clear business objectives, lack of information and poorly managed dependencies. Let's dig deeper in each of these.

## The north star
A team without a clear business goal is doomed and destined to fail. Your job as a leader is to make sure a business goal exists and both the team and the stakeholders are comfortable with it.

Why would a team without a clear business goal even exist? Maybe because it's difficult to coordinate multiple objectives but it's easier to coordinate initiatives. Objectives are non-binary. It is hard to make sure they fit together and create a unified and cohesive business strategy. That's why techniques like [OKRs are not easy](https://productcoalition.com/5-ways-your-company-may-be-misusing-okrs-3d5cdb22aa4e) to implement.

A few points that can help you identify this problem:

* The team is not involved in the whole lifecycle of the product. One person is deciding what the team should build.

* The team is evaluated on what they build and not on the value they add. There are no dashboards to measure success or failure.

* Product iterations take weeks or months. That [building trap](https://www.youtube.com/watch?v=jWfI8yXBLsM) creates the illusion of lower risk.

* A Product Manager or Product Owner acts as a bridge between Engineering and Business who don't or can't communicate.

You're not going to change this at a company level so start small. Ask the Business Owner to give a talk to the team about how the business works. Make sure the Engineering team understands and cares about the business. Start involving the Business Owner in the day to day conversations with the rest of the team. Start a pilot with one initiative where the business goal is established together between the Business Owner and the rest of the team. That's your [Trojan Horse](https://en.wikipedia.org/wiki/Trojan_Horse). Build from there.

## Information is ~~power~~ time
Documentation is often the first thing to sacrifice. Organizing information and recording knowledge is usually poorly done or completely ignored. This leads to spending a lot of time aquiring information. This is dangerous because it creates multiple threads of communication between a lot of people.

A few points that can help you identify this problem:

* Meetings for acquiring information are pretty frequent.

* Information is not presented in different levels. We use the [C4 model](https://c4model.com/) to document software architecture for a reason. Not everybody is interest in the same amout of detail.

* Documentation has no standard. Specially the one that should serve as a interface with another teams.

* Information is spread across multiple services and formats. Docs, Sheets, Confluence or Notion pages, JIRA, Trello, etc. 

* Information is constantly shared through pinned messages in Slack channels. Slack is often used as a chat messaging app and not as a forum. [This post](https://highgrowthengineering.substack.com/p/taming-slack-) talks about how to solve this specific problem.

* When you ask for information, you are redirected to other people instead of getting pointed right to the documentation.

* There are different versions of the same document. Nobody knows what is the latest and correct version. Nor where to find it.

Most of the times, the only way to get the right information is to ask somebody else. Somebody that has been in the company longer than you. So minimize the amount of people needed for those interactions. Do it yourself if needed. Or ask somebody from your team to join you. Organize the relevant information for your team in a single place. Read as much documentation as possible then summarize it for your team.

![](/images/scalups_documentation.png)

If your team also has the bad behaviour of not documenting their decisions and not recording information, do a workshop with them. Establish [some ground rules](/dos-and-donts) and try to evangelize the culture of [documentation driven development](https://medium.com/swlh/documentation-driven-development-f9a6d3258e5). If your team has good documentation you'll avoid being in pointless meetings. When somebody asks you about something, you can redirect them to the documentation.

## Dependencies
A scalup is a complex [socio-technical system](https://en.wikipedia.org/wiki/Sociotechnical_system). And more often than not, different teams depend upon each-other. One of the reasons may be that as a startup grows, teams tend to specialiase. Company wide initiatives will involve many teams or at least, many systems. Another reason may be that a scalup has went through many reorganisations. And the current structure does not reflect the architecture of the systems. Check [Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law) for the consequences of this. Often you will find confusion about what team owns what service. A team or some people can have a deeper knowledge of some systems but they don't own that system anymore. And you'll have to deal with these domain knowledge dependencies.

A few points that can help you identify this problem:

* Not mapped dependencies. There is no document with services or teams that depend on each other.

* Development of features and their dependencies start at the same time.

* Poor or no communication between dependent teams. And a generalised lack of trust.

* Code reviews on systems your team depends on take forever.

* Repositories in which many teams contribute have no clear documentation.

Don't try to remove dependencies. That's impossible. Try to manage them. I found two primary ways in which you can survive dependencies:

### Provider as a Services
Try to solve dependencies ahead of time. This can be achieved if your team has enough structure and long term vision on their business goals. The feature your team depends on will be delayed. Mostly because every team prioritises their own goals. Plan with that in mind.

But instead of asking for a feature, getting the yes, discovering it has been delayed at the worst possible time, getting frustrated and being the bearer of bad news for your team… try another strategy. 

### Go in. Be intense. Get out
Ask for two hours of another Engineer so they can pair program with your team for a couple of days. Nobody will reject that proposal. Usually people are happy to collaborate as long as the required time is kept small. Two hours a day. For a week. So you'll start the feature together and hopefully gain enough domain knowledge so your team can solve their own dependencies. This approach works way better. Guaranteed.

![](/images/scalups_dependencies.png)

When managing dependencies you'll need to solve the lack of trust you'll often observe between members of different teams. In such big organisations it's normal for most people to not know eachother. Preserving a unified culture gets more and more difficult. People from another teams seem like people from another companies. You will see antipatters like [Us vs Them](https://www.theemotionmachine.com/the-us-vs-them-mentality-how-group-thinking-can-irrationally-divide-us/) and interactions between teams characterised by lack of trust and constant tension. This gets more problematic in a remote world where people interact with avatars ignoring the human on the other side of the screen. It's easy to de-humanize and antagonise people from different contexts. Try to solve this for teams that depend upon eachother. Have some team building sessions before starting any kind of collaboration or interaction. Make members from different teams have an informal coffee chat. This has proven to work better than establishing rigid processes and rules. Don't get me wrong. Processes and rules about interaction patters are important. Read [Team Topologies](https://teamtopologies.com/book) if you want to dig deeper into this topic.

## Credibility
As organizations grow, getting anything approved gets slower. Getting access to some internal service or obtaining budget for using a third party product may take weeks. Starting an initiative to solve a structural problem may never see the light of day. When you ask for permission, many people from diferent departments will want to jump in and discuss the details with you. Legal, compliance, security, business, PPM, etc. Decisions can take months. Don't fall into that trap. It doesn't go anywhere. Try to find shortcuts and establish them as a standard when you have enough credibility. But how do you achieve that?

When you first arrive at a company, nobody knows you. You don't have any credibility and you'll have to earn it. Nobody trusts you. Even if they say they do, they don't. You're the newcomer. There are several ways to build trust and empathy and you'll have to do them all at once:

### Build a Network
Start with your peers. Other leaders from your organization will be happy to talk to you. What's their story? What's their day to day like? Ask them about their pains. What would they solve if they could? Genuinely care.

### Help others
In a company that is blitzscaling, people will constantly ask for help. Don't jump in and try to solve the problem yourself. When people ask for help, they want to solve the problems themselves with somebody's guidance. Identify and pick the opportunities where you can have more impact. Help other people and teams succeed in their own initiatives and goals. That's the key. Help. Listen to somebody's problem or frustration and try to point them in the right direction. Offer some time of your team to help other teams. Maybe some pair programming. Most of the time you won't have the time or mental capacity to do an intensive follow up. Set up a reminder for some weeks later to do a check on the people you're trying to help.

### Collaborate in cross initiatives
It's very unusual for a company to be on the extreme that nobody is trying to fix anything. So in any compnay you'll find people who are leading or trying to lead initiatives to improve and solve the structural problems that come with blitzscaling. In the beginning, join them. Use those opportunities to collaborate with other people. Partner with anyone who is trying to change the status quo. If nobody else is doing it, bring those people together. Sometimes that's enough. Other times you'll need to add some structure to these initiatives.

![](/images/scalups_collaboration.png)

Credibility will help you have early support when you try to solve any problem you think is worth your attention. You'll have faster access to resources. People won't try to discuss the details of your decisions and initiatives. Even if they don't agree with you, you'll be able to pull anything off. Your credibility will also help your team succeed. Other teams will approve your team's PRs earlier. You'll be able to open doors for your team that were closed before.

## From teams to business units 
What if you're not leading a small team of 5 to 10 people but a whole business unit of 30 to 50 people? It's common to see teams and leaders optimising for local maximas. If you see something working within a team, give them the challenge of scaling the solution up. Ask them to give a talk about how they solved that particular problem. It's important you are present. That will give them credibility and authority to make a change. Every team has their own particularities, so standarisation while the company is still growing at a fast pace, takes time. Don't get frustrated if it doesn't work out the way you expected. Not all initiatives are going to see the light of day. Make sure the different leaders of your business unit have enough trust between them so they can collaborate effectively.

## Influence
The last point I want to touch on is influence. Inside and outside your company. A scaleup is the perfect environment for experimenting and learning while you still have mentors and references to look up to. Nobody has time to control every and each one of your moves. You have enough freedom and autonomy to try as many new things as you want. And you still can ask for help. Share what you learn. In a format you feel comfortable with. Help your team share what they learn and achieve too. Inside and outside the company. If they're never wrote an article, help them. If they've never given a talk, help them prepare. Share the content they create. Give them visibility. Be their promotor. Make the pie bigger for everyone.

Start small, prove a point and scale up. Partner with other leaders. Involve other people. A scaleup is an environment with a lot of momentum. And if you want to solve any problem, you'll need more.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.
