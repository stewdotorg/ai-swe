![Thumbnail (1920x1080)](https://i.ytimg.com/vi/YG4t7aMY81c/maxresdefault.jpg)
# [I Ranked Cloudflare’s Software Factory and Wow… S TIER TOKENOMICS](https://www.youtube.com/watch?v=YG4t7aMY81c)

**Visibility**: Public
**Uploaded by**: [IndyDevDan](https://www.youtube.com/@indydevdan)
**Uploaded at**: 2026-06-08
**Published at**: 
**Length**: 41:54
**Views**: 9146
**Likes**: 472
**Category**: Science & Technology

## Description

```
Cloudflare ran 130,000 AI code reviews across 5,000 codebases for ONE DOLLAR per review. If you still think agentic AI isn't increasing productivity, open your eyes. 🔥

This is our first ever agentic engineering tier list, and Cloudflare just earned S TIER TOKENOMICS. We rank every element of their software factory F through S (plus a legendary ZTE level) and break down exactly how they pulled off this insane token arbitrage.

🎥 VIDEO REFERENCES
• Cloudflare AI Code Review Blog: https://blog.cloudflare.com/ai-code-review/
• OpenCode: https://opencode.ai/
• I Studied Stripe's AI Agents: https://youtu.be/V5A1IU8VVp4


✅ PUSH YOUR AGENTIC ENGINEERING BEYOND
Tactical Agentic Coding: https://agenticengineer.com/tactical-agentic-coding?y=YG4t7aMY81c

🚀 In this agentic engineering tier list, we break down Cloudflare's CI-native code review software factory piece by piece. They didn't grab an out-of-the-box AI code reviewer and call it a day. They built a custom agent harness on OpenCode, launched up to seven specialized reviewer agents (security, performance, code quality, documentation, release management, compliance), and wrapped it all in a coordinator agent (the one agent to rule them all) that dedupes, judges, and posts a single structured review. This is multi-agent orchestration with real tokenomics behind it.

🛠️ We rank harness engineering, agent specialization, multi-agent orchestration, context engineering, prompt engineering, model flexibility, system resilience, agent observability, and developer experience. Cloudflare's plug-in architecture (agents plus code) gives them an extensible factory that runs across thousands of repos. Their JSONL streaming gives them real-time agent observability and retry logic. Their risk-tiered compute means they don't send the dream team to review a typo fix. This is harness engineering done right.

🔥 The headliner is tokenomics: use tokens, generate value, arbitrage that value for more than it costs. Cloudflare reviews a merge request in 3 minutes for 1 dollar while engineers burn hours. They get there with a three-tier model stack (state-of-the-art, workhorse, lightweight), aggressive prompt caching, shared context files, and diff patch scoping so each subreviewer only reads what its domain needs. Stop token maxing on Opus for everything. Start practicing real tokenomics.

💡 Key takeaways:

S Tier Tokenomics: 1 dollar per merge request review. Massive token arbitrage at the scale of 5,000 codebases.

Harness Engineering: Own and customize your agent harness on OpenCode instead of renting a closed one. Programmatic SDK access is non-negotiable.

Agent Specialization: Seven specialized reviewer agents always beat one generalist with a giant generic prompt.

Context Engineering: Shared context files and per-domain diff patches kill the 7x token blowup of duplicating the full merge request.

Prompt Engineering: Tightly scoped prompts that tell each agent what to look for AND what to ignore. Don't nitpick syntax.

Model Flexibility: A tiered model stack with provider fallbacks. Deploy the right model for the job, not the most expensive one.

System Resilience: Timeouts, retry budgets, step-finish triggers, error classification, and model fallbacks for true out-loop reliability.

ZTE and the North Star: Why zero touch engineering, prompt to production, is phase three of agentic engineering, and what's still standing in Cloudflare's way.

If you want to build a software factory so powerful your codebase runs itself, this is the breakdown for you. We're studying the best engineers in the game so we can push what's possible further beyond vibe coding and into real agentic engineering.

Stay focused and keep building. 🚀
- IndyDevDan

#agenticengineering #cloudflare #aicodereview
```

## Transcript

Over a single month, Cloudflare ran
130,000 AI code reviews across 5,000
different code bases. Guess how much it
cost per review. Just $1 single dollar.
That's $1 per merge request reviewed by
a team of agents. There's a group of
engineers and investors who think
Agentic AI isn't increasing
productivity. All I have to say to that
is, are your eyes open? Cloudflare needs
no introduction. Nine out of 10 devs use
and love them for their networking,
CDNs, compute services, R2 storage, and
much, much more. Cloudflare is trading
at all-time highs, and for good reasons.
They've got compute, cracked engineers,
and as you'll see, a new software
factory dedicated to AI code review.
I've been engineering for 15 plus years,
building with agents since the GBT 3.5
Turbo days, and I am going to master
agentic engineering. I'll accomplish
this by studying what the very best
engineers are doing with their precious
time and resources. A few weeks back, we
ranked Stripe's software factory. Now,
it's Cloudflare's turn. We'll be
breaking down how Cloudflare is
orchestrating AI code review at scale.
This is also our very first agentic
engineering tier list. We're ranking
every element of Cloudflare's software
factory F through S and an additional
legendary ZTE level. We'll explain
later. If you want to understand what
makes a great software factory for code
review, stick around and let's rank one
of the most important skills of agentic
engineering. Tokconomics.
So, tokconomics is the skill of using
tokens, generating value, and then
arbitrageing that value for more than it
costs. It's a simple three- tier system.
Use tokens, generate value, capture
revenue, and right away, we have to give
Cloudflare an S ranking here for
tokconomics. They spend $1 per merge
request. This is unheard of. Engineers
can spend anywhere from minutes to hours
reviewing code. This means that
Cloudflare has attained a massive token
arbitrage. This is a great place to
start. It's a great headliner for
Cloudflare. Now, let's understand how
they've been able to achieve these
incredible tokconomics of $1 per code
review by breaking down their blog.
Shout out Ryan Skidmore for a great
writeup. Let's start with the most
important piece, the problem. In
Cloudflare's own words, code review is a
fantastic mechanism for catching bugs
and sharing knowledge, but it's also one
of the most reliable ways to bottleneck
an engineering team. You know this in
the age of agents, there are two
constraints, planning and reviewing.
Work stacks up at one of these two
levels. But there's an additional
problem here. A merge request sits in a
queue. A reviewer eventually context
switch to read the diff. They leave a
handful of nitpicks. The author responds
and the cycle repeats. The median wait
time for a first review is often
measured in hours. So this is the
problem and it's very easy to visualize
this. Cloudflare is attacking the review
constraint here, not the planning
constraint. And the big problem here is
that it takes hours to days to review.
And it looks like this. You know this
very well. The engineering owner puts up
the merge request, puts up the pull
request, and then every second they have
to wait, they're losing context on how
this works. But then finally, the
reviewer context switch from what
they're doing to review the code.
There's some back and forth here between
these two. And then eventually there's a
merge. Now with AI review, this equation
changes. The engineering owner gets an
AI review. within minutes that engineer
or a separate engineering reviewer can
come in review those comments understand
what's going on and then they can get a
clean merge this is much much faster to
have AI review hard block or let your
work go through and that's exactly what
Cloudflare has accomplished here but
there's a journey to this they didn't
start there no one starts with a great
code review system you have to get there
first how did they get there when they
started experimenting with AI code
review they took the path most do they
tried a few different AI coding tools
found a lot of these tools work pretty
well. Unfortunately though, one
recurring theme that kept coming up was
that they just didn't offer enough
flexibility and customization for an
organization of the size of Cloudflare.
Once again, we have this theme of
specialization. If you're solving real
problems, you have built a custom
solution. Custom solutions are by nature
specialized. Out of the box solutions,
out of the box agents, out of the box
prompt, skills, so on and so forth. it
will not solve your problem like you can
solve it. So what did they do next? Next
they started vibe coding. They grabbed
the diff. They shove it into a halfbaked
prompt and ask the language model to
find bugs. The results were exactly as
you might expect. Noisy vague
instructions, hallucinated syntax and
unhelpful advice to consider adding
error handling. So they realized pretty
quickly naive summarization approach
wasn't going to get them the results
they needed, especially on a complex
codebase. Swap the name complex with
specialized. This is where we push into
our rankings. You can see we have a nice
keyword here right away. Open code fans
are going to like this. So instead of
building a monolithic code review agent
from scratch, we decided to build a CI
native orchestration system. This is
their software factory. The CI kicks it
off. That's the trigger. And then their
orchestration system takes from there.
They built it around open code. Big
shout out to open code. So this is big.
This means that we can rank our harness
engineering. So harness engineering is
about owning and customizing your agent
harness instead of renting a closed one.
When you own your agent harness, you can
control the results. You control the
code underneath it. You can control
everything. So, we don't know how good
this is. We're going to start their
agent harness in C and then we're going
to see where it goes from here. All
right. When an engineer opens a merge
request, they get an initial pass from a
coordinated smorgsborg of AI agents, a
team of agents, right? Rather than
relying on one model with a massive
generic prompt, we launch up to seven
specialized reviewer agents. Very good.
Tell me more. They cover security,
performance, code quality,
documentation, release management, and
compliance with their internal
engineering codecs. These specialists
are managed by a get this coordinator
agent. This is also a name for the one
agent to rule them all, the orchestrator
agent that dupes their findings, judges,
and then posts a single structured
comment review. Okay, so we have some
more stuff to grade here on Cloudflare's
code review software factory. We have
multi- aent orchestration. So right
away, we can just jump that into C. And
we also have specialized agents. Many
specialized agents always beat one
generalist. This is just a fact. You can
look at any industry and you'll find
this to be true. Right away, we're
actually going to jump up their agent
specialization into B. You see they
define specific roles. Seven specialized
agents security, performance,
documentation, release management. You
can imagine all their system prompts
inside of their agent harness that
they're running is specialized. Really,
really strong start. We've been running
the system internally across tens of
thousands of requests. It approves clean
code, flags real bugs, and actively
blocks merges when it finds genuine
serious problems or security
vulnerabilities. This is incredible.
Really, really strong start by the
Cloudflare engineers. Now, let's talk
about their plugin systems. Plugins all
the way to the moon. When I started
reading this, I knew there was something
special here. Let me show you exactly
what's going on here with their plug-in
system. They built their system on a
composable plug-in architecture where
the entry point delegates all
configuration to plugins that compose
together to define how a review runs.
Okay, so this is a CI based trigger.
They have a GitLab merge request. CI
starts all their plugins load and then
each plugin has a three-step hook that
they can plug into. Bootstrap,
configure, and then post configure. And
they detail that all here. Each plug-in
implements review plug-in interface with
three life cycle phases. They have
bootstrap configure post configure that
runs after the configuration is
assembled. So what does this look like?
They have this plug-in system. This
gives them extensibility and
flexibility. We have plugins here 1 2 3
up to N. And then they have this
three-step workflow that they can
dynamically change per plugin. Super
super important. And then they have
ajson file that loads all the
configuration right into their agent
harness. This is their agents plus code
pipeline system. I'm going to throw this
into B. We'll move it if we need to. But
this is really powerful. Actually, we're
going to give them an A right away here
because agents plus code is how you
outperform agents alone and how you
outperform code. You need agents and
code so that you can properly manage
both the upside and the downside of the
non-deterministic nature of agents. And
you also want to do this in the most
extensible way. And that ties right into
their extensible factory. They have a
plug-in system that allows them to
quickly update, modify, change, and
configure their system with a variety of
plugins across their code bases. Super,
super powerful stuff here. This means
that across their 5,000 code bases,
across their massive engineering teams,
they still have customization,
adaptability, and extensibility. These
are key pillars of agentic engineering.
We can also add extensible factory.
We're going to throw that right into a
tier. They're covering a lot of ground
here. very very quickly. So great, they
have a plug-in system. Also important to
mention here, Brain Trust, they have
distributed tracing and observability.
Let's go ahead and just throw
observability, agent observability.
Let's give them B tier right away for
that using great out of the box tooling.
And now we get to some of the juicy
stuff. How they use open code under the
hood. So what's going on here? By the
way, if this is all making sense, you
know, drop a like, let the algorithm
know that you're interested in agentic
engineering content like this. We're
going to be covering a lot of software
factories, a lot of great engineering
teams, and understanding what they're
doing so that we can push what we can do
further beyond. These are the type of
valuable deep dives that can really
change what you can do. And we're
continuing on that theme we set at the
end of last year. We're building systems
that we trust to do more on our behalf.
So, join the journey if you're not
already. Subscribe, like, all that good
stuff. Let's continue. Let's talk about
Open Code and why they chose this. So,
we picked Open Code as our coding agent
of choice for a couple of reasons. We
use it extensively internally, meaning
we're already familiar with how it
worked. This is the sign of a great
engineering team. They use tools that
they know, not the new flashy thing.
They use tools that they know and
understand. This is a consistent theme
across great teams. Couple key reasons
here that they chose this. It's open
source so they can contribute features
upstream. Really importantly here, they
have a great open source SDK so that
they can create programmatic sessions.
This is just super critical. Any agent
coding tool that does not have
programmatic access isn't a legitimate
agent coding tool. Let's go ahead and
bump up their harness engineering. They
chose this tool for the right reason.
Let's move harness engineering into B
tier and see what they've done with
their multi- aent orchestration. So,
this is where things get really
interesting. On the channel, we've spent
a decent amount of time here breaking
down different multi- aent orchestration
patterns. They have a nice simple one.
Let's break down how this works. They
run this in two processes, and I've
actually diagrammed this out. Let's just
look at that. They have two layers of
orchestration. They have a review
plugin, which contains all of their sub
agents. So, this is where all their
specialized sub aents are. And they have
their coordinator again also known as
the orchestrator agent, the one agent to
rule them all. They use standard in and
they write out via JSON L. Writing out
via JSON L is super important. We'll
talk about that in a moment here. But
they have a single custom tool that
they've given their orchestrator agent
called spawn reviewers. This kicks off
an entire agentic workflow really. It's
not just kicking off sub agents. It's
kicking off a bunch of code plus agents
together. Again, we'll dial into that in
just a moment. It's a plugin. So really
great extensible system here. Very
customizable. They have a review plugin
that their spawn reviewers tool call
from their coordinator aka orchestrator
calls. Okay, so this is their
orchestration system. It's beautiful.
It's simple. It's tried and true. You
know, you can imagine the results of the
XML come back and are joined here via
what's called a fusion prompt or a
concatenation process where the
orchestrator takes all the results and
fuses them back together. And that's how
it works. You can see their breakdown
here. And then you can see that this is
calling that tool that kicks off their
sub agents. So very simple, very
powerful. Let's go ahead and boot up
their multi- aent orchestration. They
clearly know what they're doing here.
We're going to go ahead and move this up
to a B. And we also know they have a
custom tool. Now, you'll notice that
throughout this, they only have a single
custom tool. This is a critique I have.
We'll talk about that later. We're going
to leave custom tools in C. We'll keep
multi- aent orchestration in the B tier
because there's not really anything
special going on here. This is a very
simple orchestration pattern, right?
This is effectively a delegation
pattern. It's a little more complicated
than that, but we'll let them explain
that. So, let's go ahead and continue on
here. They spawn their sub reviewers via
the spawn reviewers tool and then they
do something really interesting here.
Sub agents respond in a structured XML
but also they write their outputs in
JSON L. Why are they writing in JSON L?
Instead of a YAML format, instead of a
JSON format, they write JSON L. What are
the advantages that JSON L gives your
agent? It's very very simple. With JSON
L, your file is always in a valid file
format. As your agents are streaming out
that JSONL, appending it to that file,
that file is immediately readable. That
JSON L is immediately streamable. You
can operate on that JSON L. And that's
exactly what they do. They act on a few
stream triggers like step finish to give
real-time agent observability and
action. They can actually take action on
these different events streaming. If
you're using JSON, you have to wait for
everything to complete and you risk the
fact that JSON might not complete. The
agent might throw an error. If you're
using JSON L, you always have valid
observability. Super important, super
key. This is of course agent
observability. I'm going to go ahead and
boost this up. They really understand
what they're doing here with their Asian
observability. And again, just to
mention it, they do have a observability
UI via Brain Trust. This is a
distributed tracing and observability
system. Very powerful, very capable. So,
we're going to boot that up. They know
what they're doing. They understand
that. If you don't measure what your
agents are doing, you cannot improve
that. You need to stream. You need to
have real time updates of what your
agents are doing. And JSON L lets them
do that. Okay. So, very powerful stuff
there. This is something another great
agent harness decision. Open code
already supports this. No need to
reinvent the wheel. And of course, this
means you don't have to worry about
buffering massive payloads hoping for
the closing JSON array. As mentioned,
streaming pipeline stuff here. The
important thing to mention is they wait
on a step finish event to track costs to
look for errors to kick off important
retry logic for when the coordinator or
the sub reviewers fail. When they see
the step finish with reason length, they
know that the model has hit its max
tokens and got cut off mid-sentence and
that they should trigger a retry. So
another really important part of your
agentic system of your software
factories, it's not always going to
work. So you need to build in resilience
into your system. Of course, this is
part of agentic engineering. If we
search for system resilience, we have it
here. I'm going to right away throw this
into B tier for Cloudflare. They're
clearly thinking about retrying. They're
thinking about failure modes and they're
addressing them directly in their
system. This is very important. If
you're building oneshot systems that you
think are going to work 100% of the
time, you're just vibe coding. Okay?
That's not engineering. This is where
things get interesting. Here we start
talking about prompt engineering,
context engineering, and agent
specialization. All topics we've talked
about a lot on this channel week after
week after week for years now. Instead
of asking one model to review
everything, we split them into
domainspecific agents. Yes. Yes. Yes.
When I read this section, I knew I had
to bring it to the channel and really
talk about why this is so important.
Each agent has a tightly scoped prompt
or a purpose, a function, and what to
look for, and more importantly, what to
ignore. Very important, very interesting
prompt engineering technique. you don't
see a lot. It's not just about what's
good. This is what we want you to do.
It's also about what isn't good. Here's
what I want you to avoid. Don't flag
this stuff. Don't code review these
items. And something that I put in all
my code review agents and pipelines is
like don't nitpick syntax. Don't nitpick
nonfunctional pieces of the code. I
don't care about that. I care about how
it operates. I care about runtime. I
care about security. And you can see
here that they're calling out these
things. Exactly. What do we actually
care about? And what do we not care
about at all? This is great prompt
engineering. Okay, so let's go and just
throw their prompt engineering. They
know what they're doing here. I'm going
to throw their prompt engineering into
an A here because of something else that
they do as well that we're going to
cover in a moment here. They flag what
to look for, what not to look for. I
myself don't even use that technique
enough. It's a great one. Another thing
on their prompt engineering that
deserves this A tier is every reviewer
produces findings in a structured XML
format with a severity classification.
Here's another great section from the
cloud for team. Here they talk about
model selection. So of course everything
comes back to the core four context
model prompt tools. A lot of engineers
just throw the highest most powerful
model on and they call it a day and then
they destroy the economics of their
business. They destroy the tokconomics
of their business. Okay? You cannot use
opus for everything. That is just a
fact. If you're doing real outloop
agentic engineering work where you are
not always there, where your agents are
running in the background, where they're
doing work for you while you're AFK, you
cannot use top tier models. It's just
not going to scale. Especially as you
grow, what you want to do, right? You
want to grow, you want your company,
your work to be successful. These models
are not sustainable at the large scales.
A lot of companies right now are token
maxing. As mentioned, that is the lowest
hanging fruit of how to use tokens. You
want to be employing your tokconomics,
generate tokens, generate value from the
tokens, and then arbitrage the value you
generate. But you can't do that if
you're just torching cash on Opus. So
very important stuff here. Because we
split review into specialized domains,
we don't need to use a super expensive,
highly capable model for every task.
Instead, we assign models based on the
complexity of the agent's job. They have
a three- tier system. Top tier,
state-of-the-art, standard workh horses,
and then lightweight tasks. This is
fantastic. I love this. They have a
model stack. something we've talked
about on the channel in the past,
they're stacking up different sets of
models. So, not only do they have multi-
aent orchestration, they have a stack of
models. And the trade-off here is, as
we've discussed in previous videos, as
you move up this stack here toward the
state-of-the-art models, you're trading
cost and speed for performance. This
cannot go on forever. As we're seeing
here, Enthropic is locking down their
subscriptions. We're going to get like
20 or 200 bucks a month for SDK usage.
That's nothing. We have to be using
these APIs for our Outlook agentic
engineering work. But the trade-off here
is this. When you get that
state-of-the-art performance, you trade
off speed and cost. Most importantly,
the cost. So, what's the solution? Don't
just use state-of-the-art. Create a tier
stack of models for standard usage and
then lightweight usage and understand
when to deploy the model. Otherwise,
you'll never attain great tokconomics.
This is part of tokconomics. And so, we
have an additional item here for this.
This is called model flexibility. We're
going to throw this right into B tier
and we'll probably move it up in a
moment here if there's nothing negative
about their model flexibility. Okay, so
they have a tiered system. There's
levels to this as you'll see in a
moment. You can also really focus on
token caching, which is something
CloudFlare has done. So, we have a
section here on prompt injection
prevention. Not super interesting or
important, but worth a read. Of course,
I'm going to leave this blog linked in
the description for you. Here's the more
interesting part. Saving tokens with
shared context. Each subreviewer only
reads a patch to the files relevant to
its domain. Once again, specialization.
Once again, doing some great context
engineering here. The systems don't just
read all the diffs into the prompt.
They're not just saying read all this
stuff. They're first saving the diffs of
the PR to a file path. They're doing
some context engineering work here and
then each subreviewer only reads the
patch that's relevant to its domain.
They also have, of course, a shared
context file. This is very, very common.
What this does here is really
incredible. If they duplicated all this,
all the information inside of a
moderatelysized merge request, all that
context, it would multiply their token
costs 7x. At the scale that Cloudflare
operates, they have to consider costs
and value arbitrage of their tokens from
the get-go. This is key to their success
at the scale they're operating. Let's go
and give them an A for context
engineering. Very aware of all the
context flowing in and out of their
system. And as mentioned, you know, with
the plug-in system, they can dynamically
change their prompts and change the
context of the code review. Just saving
tokens there. That's fantastic. And then
we get to more multi- aent
orchestration. So I mentioned the fusion
prompt earlier. What does that mean?
What's that all about? With the classic
sub aent multi- aent system. You have
your orchestrator, you have your sub
agents here, and then you have all the
responses going back to your
orchestrator. Bam, bam, bam. Simple fan
out, and then a close in. You can do a
lot of magic at this point of the
process when your results are coming
back into your orchestrator agent. And
so what are they doing here? They're
dduping stuff. The orchestrator or they
call the coordinator is recategorizing
work and it's also making sure that the
work that's mentioned is reasonable. I'm
a huge fan of this. I hate overdetailed
nitpick code reviews like let's just
focus on the value. So they have a
filter inside of their agentic system to
just do this all the time. They have
templated their engineering into their
agentic system which is like a core
thing we talk about on the channel all
the time. This is the core thesis inside
of tactical agentic coding. So this
filter looks for speculative issues,
nitpicks, false positives and
conventions that contradict findings.
Here's an important part and an
expensive part of this. If the
coordinator isn't sure, it uses tools to
read the source directly and verify.
Still important. There's tokens you just
need to spend and they're doing it here
with the coordinator to validate the
work. So fantastic stuff. You can see
some of the decision rubric they're
setting up here for their coordinator.
The part of the system that combines the
responses from the sub aents into the
primary agent that looks like this. So
you have that final step here. Classic
fusion of all the specialists of all
your sub aents. This is what happens
basically anytime you spin up a bunch of
sub aents under an orchestrator agent.
You have to combine and understand the
results. Keep this at B, but then bump
up their agent specialization cuz I
think that's really what the value is
here. It's not the fact that their
multi- aent orchestration is that unique
or interesting. It's that their agent
specialization is valuable. This is
where that final decision comes in.
There's approved a merge request getting
agentically approved, saving you and
your team tons of time. Approve of
comments, approve of comments, and then
minor issues. And then significant
concerns. This is what you pay for.
You're really paying for these two. I
really like how Cloudflare has set this
up. This bias is explicitly toward
approval, meaning a warning and an
otherwise clean MR still gets approved
with comments rather than a hard block.
This is super super key. They're pushing
and going in the direction of being more
agent forward, more agentic. And to be
super clear here, why do they have no
ZTE in this rank? This is zeroouch
engineering. This is the northstar for
all agentic engineers. You want to build
systems that operate without you. And
all these elements are interlocked.
These all help you get there. All of
them are important in their own way. But
the northstar of agentic engineering,
which you know, we're going to give them
a final grade at the end here with this
kind of catch all agentic engineering
item. The northstar is zero touch
engineering. I like the decision here to
auto approve these PRs in many
situations, right? Some engineers would
block on some warning. They're not
taking that risk. But the key here is if
there's clearly no production risk,
Cloudflare is going to approve this. I
love this. They're pushing toward being
agent forward. agent first while leaving
in some great escape hatches. So if a
human reviewer comments break glass, the
system forces approval regardless of
what the AI found. So their AI is never
limiting them. It's never hard blocking
them. They can always get around the
system. It's only a matter of time until
you have to break rule in your system. I
love this. I think this lets us put a
couple things on the map. We have human
out the loop. So this is the ability of
the system to run without humans. We're
going to throw this in C tier. And you
know, maybe we can push this up to B
because they are letting their agent
cover a decent number of cases here.
They're really letting their agent
approve. So we'll go ahead and give them
a beat here for human out the loop. If
you want to push towards ZTE towards
that really powerful agentic system and
software factory, you can't be
bottlenecking your agent. It's only a
matter of time. What is the planning and
review constraint? It's us. It's you and
I. We are the constraint now. So you
want to be pushing yourself out the
loop, not in the loop. And I know that's
hard and I know especially at scale it's
really hard to do this in production
systems like Cloudflare like no one
wants Cloudflare to go down because of
some agentic issue. This is a learning
debt of the new skill of agentic
engineering we have to pay down if you
want to push toward the north star. So
anyway we almost have all of our
sections ranked here. Let's keep looking
at what they've done. Another really
really great piece here. They have risk
tiers more great context engineering.
More great tokconomics. Don't send in
the dream team to review a typo fix. So
brilliant. So simple. I had to diagram
this out cuz I love the idea so much.
Spin compute by risk level. You don't
deploy the dream team to fix a oneline
readme fix. You can deploy that to
production with ZTE, frankly, which more
teams should try and experiment with.
They have this great scaling system for
trivial changes with lines less than 10,
files less than 20. They only deploy two
of their agents, right? The coordinator
and one generalized code reviewer agent.
What's the next tier? They have a light
tier. So lines changed files change and
then number of agents they deploy for
this set. So if there's less than 100
lines less than 20 files they deploy a
coordinator code quality documentation
and then one more other agent. Okay. And
then there's the full pipeline their
full agentic system their full software
factory where they deploy all
specialists when there's more than 100
lines or more than 50 files changed.
Then you get the full suite just really
really great. This is like a combination
of things but it's mostly multi- aent
orchestration. I'm going to go ahead and
bump up their multi- aent orchestration.
Really, really great stuff here with
this system. And it's not because
they're using a novel multi- aent
orchestration system. It's because
they're using just the right amount of
compute when they need it. Again, this
all comes back to their tokconomics.
Their tokconomics are absolutely S tier.
All right. So, we have more context
engineering work here. They're getting
rid of large files. They don't want the
bunlock. They don't want the menjs
files. Save your agents from mistakenly
reading one of these and blowing up your
cost curve. Looks great. Here's their
multi-agent orchestration system. Again,
my problem with this system and why I'm
going to leave custom tools in Ctier is
because they only have one tool doing
all of this work. So, this single tool
does all of this work and it's quite a
lot of work. Like, they basically have
this tool to kick off a program. I would
question whether they need this to be a
tool. I would go one of two directions.
Is this actually a tool that your agent
needs to run or should this be more
tools? Is this really agentic? if it's a
one-off tool that kicks off a bunch of
stuff. This is one of the weaker areas I
see in their agentic layer. So, we're
going to leave custom tools and see. No
system is perfect. Here we do need to
talk about what they have done really,
really well. So, they have timeouts. So,
they have per task overall retry budget
again managing tokens. And then they
have resiliency. So, they have a state
diagram here that just makes sure that
the system is running. They also have
this great fallback system for their
models. So if the system runs into an
issue, that's the model's fault. They
fall back on previous models. Okay? So
you can imagine they updated this to
Opus 4.8, but they also have their lower
tiers of models, their workhorse models
and their light model suites. So this is
really good system resilience. Love that
they're building this in. And they also
have error classification. So if a
subreer session fails, the system needs
to decide if it should trigger a model
fallback or if it's a problem that
changing the model won't fix. So they
have a nice set of errors. Again, their
system resilience is fantastic. I'm
actually going to boot this up into this
next tier here. A lot of great A tier
stuff. Maybe we need to move a couple
things into S tier, right? They also
have the coordinator level fallbacks.
Looks great. They built resiliency into
the system. Love to see that. And more
resiliency. If a model provider goes
down, they have a system a CI job to
just fetch the model configuration and
update it. This means that they can
quickly switch on and off their
different model providers. Anthropic,
Google, open router is really really
good for this. This is super powerful,
right? Again, their system is very
resilient. They're not going to go down
because they're hitting rate limits.
They're not going to go down because a
model provider is down or having a bad
day, whatever it might be. Again, this
is another thing that's all part of a
Gentic engineering. It's that full
thought process of will my system go
down? How long can it stay up? What are
all the failure cases I need to build
against? More pipeline details here.
We're going to just kind of gloss over
that. I want to talk about something
really important, which I think is going
to be the nail in the coffin that pushes
their resiliency up to an S tier. Let's
go and do that now. I'm going to push
system resilience into an S tier as
well. Why? It's because they also aren't
wasting time with re-reviews. Often
times, you know, developer will push new
commits, system reruns, incremental
review. The coordinator receives the
full context of the last review. So,
it's not wasting time. It's not wasting
tokens. Their system is not going to
start from scratch. If they have a
re-review, the agent picks up previous
context. So, they're managing state
really well. Again, they're context
engineering their state. We already have
context engineering in a tier, so that's
good. Sounds about right. This is really
interesting here. User can reply won't
fix or acknowledge. The AI treats the
finding as resolved. If they reply with
disagree, the coordinator will read
their judgment and either resolve or
argue back. This is really cool, right?
They're treating this new compute, these
tokens, this intelligence that we have
access to as actually intelligent. You
and I get stuff wrong all the time. We
want to understand why. We want to learn
from it. We want to understand the pros
and cons. The agent more and more will
be doing more engineering work. And this
is part of it. disagreeing, giving a
different perspective. That's part of
engineering. This is also why model
diversity, having different models in
your model stack is super important as
well. Different models will have
different opinions about the exact same
thing. Here's a really interesting
section here, and this is where we get
into self-improving systems. Their
agents rely heavily on agents.md. So,
this is a file I'm not a huge fan of
because it applies to every single agent
and often times these files get bloated,
overused, and they grow infinitely. But,
they address that directly. These files
rot incredibly quickly. So what do they
do? So they build a specific agent to
assess the materiality of a merge
request and yell at developers if they
make a major architectural change
without updating the AI instructions.
Okay, so a little bit of
self-improvement here, but not much at
all. So this system is still relying on
humans. In fact, when you're yelling at
your engineers to do stuff, another
thing that is not good for the system.
I'm going to drop human out the loop
down to a C and self-improving is going
to stay in C. Maybe this stays in F
tier. We'll go ahead and give them an F
tier. Right? This system isn't
self-improving. This is one of those
things that could be self-improved. You
want to yell at your engineers for fewer
things, not more. And agents give us a
great opportunity to do that. Let's talk
about the developer experience. How do
you actually use this? This is where
Cloudflare really shines. Configuring
these systems is what they do. So you
can imagine they have great DX. All you
need to do is add a single CI into their
GitHub YAML file. They use GitLab. All
these systems are largely the same. They
include a component for their CI/CD.
Teams can customize behavior by dropping
in their agent MD. I wish this was a
specific file for this process, not the
agents MD. Agents MD is for every agent
that operates in the codebase. There's a
little bit of conflation here with that
context. So, I don't like that. But
anyway, bunch of other stuff gets
injected and they can also run this
locally via a full review inside the
Open Code terminal user interface. Okay.
A lot of stuff to like, a few things not
to like. Again, their strongest suit
here is their tokconomics. So, show me
the numbers. What's the final results
here? On average, the review costs a
dollar. Just incredible tokconomics and
the median cost is a dollar. And so,
this changes based on the percentile,
right? Based on the p value you're
looking at. Of course, there are always
going to be outliers. And specifically,
I think they mentioned this somewhere.
The longer the diff, the longer the
context of the merge request, the more
tokens it uses, right? Your agent just
have to read more. Great stats here.
Like I mentioned, 130 reviews across 50k
merge requests. This is crazy. They're
working across 5,000 repositories. So,
they're not even specializing on their
code base. They've just built a code
review system that can operate on all of
their code bases. Very impressive work
there. Also, something that I would like
to see specialize. Maybe they have
different types of code bases. Reviews
complete in 3 minutes. Very, very
impressive. The costs are just
fantastic. And here's what they actually
found. What was the value they got?
Doesn't really matter if you can do a
bunch of cool stuff if you're burning
more cash than it costs you to get the
value. Cloudflare is measuring so that
they know where they're at and how to
improve. Okay, so here's all their
findings, right? They found 8,000
critical issues and then they found a
bunch of suggestions. The key thing you
want to do with your code review
software factory is find the critical
issues that are going to block or
destroy production. The rest of this is
just the journey to get there. Warnings
that could turn into critical issues are
also important, but really you just want
to find the critical issues. This is the
stuff that matters. This is the value
that they're really paying for with each
of these tokens. Per reviewer, per
agent, they're looking at who found the
good stuff. Their most important agent
in terms of performance is their code
quality agent. It found 6K critical
issues. And then security also going to
give them a lot of return on investment.
Found 400 critical issues. Some
compliance stuff. When you're observing
your agentic systems, you really want to
know who's paying the bills. What agent
is actually generating the value for
you? Their security reviewer agent flags
the highest proportion of critical
issues. Token usage 120 billion tokens
in total. But here's the impressive
part. Look at the cash reads. This is
something I need to spend more time with
to understand how they were able to get
their cash reads so high. And again,
this is why their tokconomics sits at
the very top of what they're doing.
Great observability work from the
Cloudflare team. They have input output
token and cache read. Right? You can see
that the top tier models, you know, once
again talking about tokconomics, talking
about context engineering, prompt
engineering. The top tier models are
using 51% of all of the token spend. You
know, notice this. They have more input,
a little bit less output, more cash
reads, more cash rights, but this is
only 46.2% of all their token usage.
Sitting on that lightweight tier, they
have the Kimmy K2.5 with 11 million
tokens and 200 million output using up a
very very small amount of their token
usage per agent token usage. You know,
last week we talked about agent
observability. These are the numbers you
want to be able to pull out of your
observability system. Love to see this.
And then they have their cost by risk
tier. So, what does it cost to run their
specific stack of agent team? Really,
really good stuff here. Couple final
notes. Here's what a review looks like
for them. We have developer experience
in here. I'm going to have to, you know,
dock some points here. I'm not sure
where this is. Maybe this is in their
GitLab UI. But, um, the review itself is
good. I'm just complaining about the DX,
right? Like the ideal developer
experience is a unified interface for
all things in the software factory. So,
let's go ahead and give them a ranking
here. Their developer experience, we'll
give them a high B because it's super
easy to spin up and control the review
system. They can run / full review right
in the repository and it hooks up to CI
very easily. And then they have some
limitations that they're super honest
about. The big one here is cost size
scales with div size. There's not really
a way around this concurrency bugs,
cross system impact. And then there is
architectural awareness. So this is
keeping their context engineering down
from that S tier. It is hard to know
exactly how much of the full context you
need for an agent to be performance.
Sometimes it's ultra valuable to have
that full big picture. Other times
you're just burning tokens and wasting
time. Let's go and rank our final items
here. Right. This system is not always
on. This is a northstar in addition to
zero touch engineering. Always on agents
is how you get to zero touch
engineering. If your agents are always
on, they're always operating. They're
always giving you results. You are at
the highest levels of agentic
engineering. This is kicked off via CI
and via a slash command. It's not always
on. It's on when it needs to be, but
there's a lot of human in the loop
involved here. So always on. We're going
to go ahead and drop this into Ctier.
And then here's one we can be a bit
critical on. Spend tole learn. I'm going
to drop this into C tier. Spend tolearn
is often at odds with tokconomics. If
you're optimizing your system for cost,
you are by nature not spending a lot of
money to learn how to best operate your
software factories. You know, these are
at odds a little bit. We have
tokconomics at the top. Their system is
very resilient. It'll just keep
rerunning within some good bounds. They
have a great plug-in system that
combines agents plus code that is
extensible, right? They build an
extensible software factory. Again,
thanks to their plug-in system, they use
Brain Trust and they have, as you can
see here, like great analytics and great
agent observability. They can pull all
the stats that matter out of their
agentic system. So, we're going to give
their agent observability a solid A.
They understand some great prompt
engineering. They're prompting things
they want their agents to do. They
prompting things they don't want their
agents to do. And they're also injecting
prompt context with their plug-in
system. Right? So the prompt engineering
is very dynamic. They're doing some
pretty interesting advanced stuff with
their context engineering. One of them
being diff patch files. So they're
conserving that context window. They're
using agent specialization and multi-
aent orchestration to reduce their
context windows. These are key ideas we
talk about in agentic horizon. Reduce
and delegate. R&D is key for managing
your context window. They of course have
seven specialist agents. Many
specialized agents will always
outperform generalist agents and they
have a decent multi- aent orchestration
system. Their multi- aent orchestration
isn't novel. We've talked about far more
interesting patterns here on the
channel. What is novel is how they're
organizing their teams of agents into
different tiers. Their developer
experience looks okay based on the blog
post. Their harness engineering looks
pretty good. They have a bunch of
plugins. Nothing super special. They are
controlling their Asian hardness to
control their results. You can bet
they're doing things they can't with
claw code or with codeex. So, we'll give
them a solid B tier here. Model
flexibility, we are going to move this
up into a tier. I really like how
they're able to change their models very
quickly. And more importantly, I like
their split into their three tiers.
They're really controlling the core four
by owning their Asian harness and then
by creating these tier system of when to
use which model. because once again
don't always need state-of-the-art and
more and more over time this is going to
become more true which means you can
save more money on your tokens and spend
more tokens if you're using the standard
tier or even lightweight tier this is
critical for product engineering work
and in this case AI code review they're
still doing engineering work but for any
outloop agentic engineering work you
must be able to flex your models custom
tools again not super hyped on their one
tool we're going to keep this in C tier
seems like that should be pulled out
into code or they should add more tools.
To me, it looks like this is a pretty
standard uh human in the loop process.
They're not doing anything special with
keeping their engineers out the loop
outside of just having the code review
get done by an agent, which is what you
would expect from the system, right?
Nothing special. The system is not
always on. It runs when it needs to.
Again, always on is oftent times at odds
with tokconomics. This is okay, but we
can't give them a high rank for this.
And for the same reason, they're not
aggressively spending to pay down their
learning debt of this new skill of
agentic engineering. Once again, often
at odds with chokconomics, their system
is not self-improving at all. They
update this by hand, which is fine. This
is a, you know, more advanced pattern of
agentic engineering. And so, let's give
them a final grade. As you can see here,
we're going to give them a solid A for
their agentic engineering. They have a
lot of stuff in that A tier. It's
looking really good. And they have a
couple S's here. Something I put in the
ZTE honestly is stuff that just pushes
us from phase two of agentic coding of
building trust with agents of scaling
our computer scale impact to more zero
touch type of things where it's one
prompt and that prompt goes to
production. This is one of the advanced
concepts we talk about in tactical agent
coding and some of our final lessons.
I'll be honest, I haven't seen anyone
doing zero touch work yet. This is what
I'm starting to consider the phase three
next generation work where we truly go
prompt to reduction. Not because you're
vioding and you're not looking, but
because you're agentic engineering and
you've built systems that work so well,
you don't have to look. Okay? So, very,
very different than vibe coding. They
ride on a similar line that feel similar
from looking on the outside, but it is a
completely different game, a completely
different monster when you're doing
this, right? Okay. So, nothing zero-
touch engineering like in the system.
It's still early and I'm putting this up
here just to mention it, just to start,
you know, spinning your brain on how you
can start thinking through this single
idea of what would it take to build a
system that I trust so much that I would
write a single prompt and then work
would be shipped into production. Okay,
that's the ZTE mindset. This is the next
phase, right? That's phase three. We're
all in phase two. Some of us are stuck
in phase one where we're prompting back
and forth in the terminal. That's fine.
We're all at different places. That's
part of why I'm creating this content,
to kind of share the spectrum of what's
possible by looking at what the best
engineers in the game are doing with
their engineering, with their agents,
with their orchestrating of
intelligence. Big shout out to Ryan
Skidmore for this great write up. Uh big
shout out to all the Cloudflare
engineers. You guys are doing great
work. Love the service. If you're
interested, definitely check out this
blog. I'll leave it linked in the
description. If you want to work at
Cloudflare, reach out. This is not
sponsored. I have not talked to any of
these engineers. They're just doing
great work very clearly. And so it looks
like a great place to be because they
are agentic engineering. I would
recommend if you're looking for a job,
avoid every place that isn't AI first,
AI forward because they're going to get
cooked by engineers like you and me
orchestrating intelligence to build at
the agentic speed, not the speed of
humans, the speed of agents. Okay. Uh if
you enjoyed this, if you watch to the
end, definitely drop a like, join the
journey. This is our bread and butter.
As I mentioned in the beginning, I am
going to master agentic engineering. I
deliver weekly videos on these very
ideas every single week. If you want
some extra juice, if you want to pay to
play and get an advantage, check out
tactical agentic coding. This is my take
on how to scale far beyond AI coding and
v coding with advanced agentic
engineering. So powerful your codebase
runs itself. Guess how adws aka software
factories. Check out the landing page
here. Link in the description. You know
where to find me every single week. Stay
focused and keep building.