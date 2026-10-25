---
author: Jason Zerbe
layout: post
location: 40.0012561,-105.2627494
outcome: WORKED
published: true
tags:
- Vraid Systems
- LLMs
- operations
- procurement
- board governance
title: Running Vraid Systems on LLMs
---
__Result:__ It worked, better than I expected.
The leads pipeline and bid evaluation are fully automated.
I understand the fine print of the policies I sign up for, so I can manage risk instead of just accepting it.
Fulfillment is the next piece and is in progress.

_First written March 2026, updated October 2026._

## The Problem
Vraid Systems sells goods and software into public tenders, mostly state governments and UN agencies.
That market is too complex for most mom-and-pop shops and too small for the big primes, which is the opening.

The catch is paperwork.
Every tender is a stack of PDFs, procurement rules, and legal terms in a different jurisdiction.
Reading all of it is a full-time clerical job, and I wanted the business to run without one.

## The Hypothesis
> With the advent of AI and LLMs many things are changing about how money is made.
> Administration and clerical work goes away, which means the C-suite should be reshaped,
> as much of what they do is glorified clerical work from their background in law, MBAs, and finance.

If that is true at a big company, it should be even more true at a small one.
So: can LLMs do the clerical and administrative work, so I only do the parts that need a person?

## What I Set Up

- __Leads__: Tender notices land in a Slack channel. An LLM pulls the bid documents from the procurement portal,
checks them against my criteria, and posts the ones worth chasing to the sales channel.
I log in to the portals myself. Everything after that is automated.
- __Evaluation__: For each bid it checks whether we can win, the risk versus reward,
and whether we can source it, then fills out the submission PDFs.
Goods bids get a sourcing sheet with at least three suppliers, with customs duties and tariffs included.
- __Back office__: LLMs review audit questions against the Federal Acquisition Regulation (FAR),
balance sheet risk, and corporate law in each place we operate.
- __Humans__: I submit the bid, approve the sourcing, and send payments.
Anything that moves money or signs my name, I do.
- __Network__: I trade notes with other small business owners who lean hard into automation.

## Did It Work?
Yes.

- __Leads__: Fully automated, from notice to short list. Bids that do not fit get dropped before I spend time on them.
- __Risk__: The biggest surprise. I now get the nuance in FAR clauses, liability terms, and local corporate law.
I can ask "what happens to me if this goes wrong?" and get a straight answer that points at the clause.
- __Fulfillment__: In progress. Sourcing and ordering are next.

One lesson came out of tuning the evaluation rules: stop bidding on single-lot goods tenders.
Only the manufacturers win those, and it is a race to the bottom.
Tenders split into lots are where a small reseller can actually win.

## What This Says About Bigger Companies
In my business, the work that got automated first was the work that looks most like an executive's job:
reading policy, weighing risk, checking compliance, filling out forms.
What is left for a person is trust, relationships, and signing for the money.

My partner made a better point than my original hypothesis:
> I don't believe a company's board would vote out their social class peers even though CEOs are obsolete.

I still think [Fortune's take on the AI productivity paradox](https://fortune.com/2026/02/17/ai-productivity-paradox-ceo-study-robert-solow-information-technology-age/)
asks the wrong question. To scale almost any system, you push as much work as possible out to the edge.
If leadership admits AI changes the work, leadership is the next thing to get evaluated.
[This Forbes piece on replacing the C-suite](https://www.forbes.com/councils/forbesbusinesscouncil/2025/11/07/the-c-suite-is-becoming-obsolete-what-should-replace-it/)
is closer to right.

Running Vraid this way did change my mind on one thing.
The part the LLM cannot do is the network: who trusts you,
and who picks up the phone when tariffs change or an embargo hits.
That is what a well-connected board member actually brings.
<span class="sidenote-number"></span><span class="sidenote">
Resource dependence: board networks provide access to information, trust, and capital.
<a href="https://www.redalyc.org/journal/3223/322359389007/html/">redalyc.org</a>
</span>
The research backs both sides.
Tech-literate CEOs drive more product innovation,
<span class="sidenote-number"></span><span class="sidenote">
<a href="https://pubsonline.informs.org/doi/10.1287/isre.2021.0553">Information Systems Research</a>
</span>
and zombie directors and plurality voting are early signs of a company falling behind.
<span class="sidenote-number"></span><span class="sidenote">
<a href="https://corpgov.law.harvard.edu/2025/09/09/governance-matters-dont-overlook-board-oversight/">Harvard Law Forum on Corporate Governance</a>,
<a href="https://www.glasslewis.com/article/how-plurality-voting-allows-directors-to-stay-on-the-board-without-majority-support">Glass Lewis</a>
</span>
But legal and finance backgrounds still matter when the risk is AI-generated fraud or a trade shock.
<span class="sidenote-number"></span><span class="sidenote">
<a href="https://www.klorconsulting.com/blog/ai-accountability-delaware-ruling">Klor Consulting</a>,
<a href="https://www.bcg.com/publications/2025/five-dynamics-that-will-test-ceos-in-2025">BCG</a>
</span>

My updated view: vote against busy directors and zombie boards.
Keep a few well-connected directors for the network you need to get through tariffs and embargoes.
Point the rest of the board at making the product great.

## What Is Next
Automate fulfillment end to end. Payments and signatures stay with me.
