
> [!cite]- **Drafts**
> #### First Draft
> 
> ##### I think anybody who's ever read documentation for a tool or framework has had to ask at some point, 'who is this *for*'. Bad documentation might be written in a way that obscures the difficulties and pitfalls you encounter as a beginner. Or skips complex intermediary steps when giving examples of how to actually be productive with the tool, or overwhelms you with detail when you just need a simple explanation. What different flavors of bad documentation have  in common in my experience is making a lot of assumptions about what information is important to the reader, and subsequently leaving huge gaps that the user needs to fill in themselves. 
> 
> ***Why does this happen?***
> ##### It's easy to blame the people writing the documentation, and in my experience it's true that most people hate writing documentation. It's almost always more fun to build something than document it. But I'm a systems engineer and I think in systems, and from my experience writing documentation on the job, i think a big part of is is that the incentive structure for productivity heavily discounts good documentation. In other words the documentation landscape is this way because most people and companies don't understand the value of institutional knowledge
> 
> ##### The following are what I've personally experienced with bad documentation
> 
> - ##### Documentation is an afterthought
> - ##### There's no direct incentive to improve documentation. It's usefulness is hypothetical and disconnected. 
> - ##### Good documentation takes a lot of time to write. If you're responsible for shipping too, time spent documenting is time spent not working on the product
> - ##### Documentation is low-visibility to stakeholders, including bosses and managers - and they struggle to see the positive impact it has down the line because *they* have no incentive to incentivize spending time on it.
> 
> 
> ##### This is what i've experienced with *good* documentation
> - ##### It starts at the beginning. It doesn't drop the reader into the middle of a bunch of concepts or bespoke technical terms. It gently places them at a natural starting point.
> - ##### It lets the reader start learning by doing immediately. If you're a programmer, think back to your first 'Hello World' moment.
> - ##### It answers the questions it surfaces, and makes those answers accessible. 
> - ##### It doesn't overwhelm the reader with technical concepts or walls of text. This includes including complex tool chains or concepts that the reader might not be familiar with yet, but that are necessary to understand before the reader can be productive with the tool. 
> - ##### It is formatted in a way that is legible, aesthetically pleasing, and intuitive to read. 
> - ##### It gently encourages absorption of additional context and knowledge that is beneficial for skillful use, long-term retention and embedded understanding of the tool **without forcing that information on the reader** . 
>   
> #### Second Draft 
> #### **(LLM)**
> 
> If you've ever read documentation for a tool or framework, you've probably had that moment: _who is this even for?_
> 
> Maybe it dropped you into a wall of technical concepts with no on-ramp. Maybe it skipped the hard parts entirely, the awkward middle steps where most people actually get stuck. Maybe it answered a different question than the one you were asking.
> 
> Bad documentation isn't usually the writer's fault. Most people genuinely hate writing it — building something is almost always more interesting than explaining it. But I'm a systems engineer, and I think in systems, and when I look at why documentation is consistently bad across the industry, I don't think it's a people problem. I think it's an incentive problem.
> 
> Documentation is low-visibility work. Stakeholders can't easily see its value. There's no direct feedback loop between writing good docs and shipping product. Time spent documenting is time not spent building. So it becomes an afterthought — something that happens after the fact, if it happens at all. And the institutional knowledge that could have been captured just... doesn't get captured.
> 
> Here's what I've noticed about documentation that actually works. It starts where the reader is, not where the writer assumes they are. It gets the reader doing something real as fast as possible — if you've ever written your first Hello World, you know exactly what I mean. It answers the questions it surfaces, and makes those answers easy to find. It doesn't overwhelm you with concepts you don't need yet, but it creates a natural path toward the deeper understanding you'll eventually want. And it's formatted in a way that's legible, aesthetically pleasing, and intuitive to navigate — because good formatting isn't decoration, it's how a document signals that someone actually finished the job.
> 
> The through line in all of that: good documentation meets the reader where they are and earns their attention incrementally. It doesn't demand that you already understand it in order to use it.
> 
> Most documentation does the opposite. And I got tired of that — so I built something to fix it.
> 
> More on that next time.
> 
> ---
> #### **My Rewrite
> 
> If you do any kind of engineering or technical work, you probably read a lot of internal documentation. Probably while trying to fix something for an irate client, or figure out some weird bug while trying to write some kind of business logic on a deadline. And if you're lucky enough to *find* the documentation you need, half the time you'll be thinking _who was this actually written for?_ as you scroll through it. Because it rarely seems to be for you, the person who's reading it to learn how to actually understand the thing being documented. 
> 
> **Four types of bad documentation**
> 
> **The Soup**
> Tons of technical concepts, assumptions about the readers familiarity with the subject, and poor discovery, that seem designed to confuse you as soon as you've left the 'Getting Started' section. You just have to wade in and hope you encounter what you're looking for and learn to swim in the process. 
> 
> **Undercooked and Raw-In-The-Middle**
> This kind of documentation is especially frustrating when you lack a mental model of the thing being documented and are trying to learn. Maybe it starts strong, but it's clear that whoever writing it basically gave up when some other ticket pulled their attention away. not  If you're lucky it might give you an API method section with explanations, and some ultra-specific 400 line example written in powershell 5.1 and python 2, with a cryptic *Ask Jerry*. 
> 
> **The Swiss Cheese**
> This is my biggest pet peeve, not least because it's extremely common, and is often mistake for *good documentation*. This is generally serviceable documentation with a lot of small gaps, assumptions and addendums that require you to continually interrupt yourself and look at other docs or prompt on the side to try to get the missing context. It's experienced as a string of small frustrations that sap your ability to digest the information constantly. Note: This is not confined to internal documentation. Looking at you Microsoft Learn. 
> 
> **The Sloppy Joe**
> If its between no documentation and LLM documentation, the latter is *usually* an improvement. The sloppy joe wasn't invented by people using LLM's, but LLM's makes it much easier to make, and harder to spot than it ever has been before. The sloppy Joe is well-formatted, well-written, and generally palatable, but with lots of filler, seasoned generously with hallucinated insights, out-of-date context, and all-around dumb assumptions. The worst thing about this isn't actually the parts that are wrong- its that 70% - 90% of it is accurate, but it all looks *great*, which puts the burden on the reader to figure out what parts are sound and factual, and which parts are slop. 
> 
> \**Note:* *Just* using AI to help you write your documentation does *not* make it sloppy. It's not *understanding* the input or checking the output and correcting it where needed that makes it truly deserving of the moniker. I'll write more in the future about how to use AI to *actually* improve your documentation, not just write a lot of it.
> 
> **Why does so much bad documentation exist?**
> 
> It's tempting to blame the people writing the documentation for poor documentation practices. And yes, it's almost a meme how much engineers of all stripes hate writing it. But, as a systems engineer, when I see a recurring pattern that looks like laziness or lack of attention to detail on the part of individuals it's usually a symptom of a poorly optimized system. My theory? A lot of people *want* to write good documentation, but they don't because -
> 
> 1. **Writing good documentation isn't rewarded**
> 	Those of you who write documentation as part of your job, when was the last time you were told, "Hey, great job with the quality and accuracy of that documentation, here's a bonus!". From the average business major's perspective, writing documentation is a cost center. It's impact is only rarely felt, doesn't show up in the quarterly report, and competes with shipping product and features for resources.
> 2. **Institutional Knowledge isn't seen as intrinsically valuable.** I once worked at a small firm where the CEO casually raised the suggestion of laying off the senior engineer with the most knowledge of our internal systems, because they had moved 2 hours away and wouldn't be able to come into the office as often. Decisions like this do damage over time, often in unpredictable and highly inconvenient ways. And, since it was never viewed as valuable, it isn't documented either, which further compounds the knowledge-cliff. So institutional knowledge loss reduces a company's resilience and ability to navigate challenges over time. But institutional knowledge holders have good reasons *not* to write documentation such as -
> 3. **Documenting the systems you work with can hurt your job security.** If you've hung around on the internet and have looked at job forums and places that , you've probably seen this piece of advice crop up. *If you're the only one who knows how to do the thing that prevents the business from bursting into flames, don't document it.* This is because if a business is downsizing, documenting your work takes you from an employee with a huge amount of leverage and bargaining power to someone replaceable. You don't have to look hard to find plenty of stories about people who were hired back as a consultant at $500 an hour to fix some critical infrastructure, costing the company millions in downtime and lost clients. 
> 4. **Writing good documentation is hard, and it takes time to write.** Even if you work at an org where good documentation is valued, good technical communication and writing is *hard*. It's a different skillset than engineering (though I think there are actually significant benefits to an engineering mindset in writing documentation, more on that later). Good documentation also takes time to write and to vet for accuracy and readability. Because of the above incentive structure, most people write to the minimum spec, not to the highest possible one. 
> 
> #### *What good documentation looks like.*
> 
> Despite all the points to the contrary above, good documentation absolutely does exist, and and I've seen enough of it to have identified some common patterns. 
> 
> 
> **Good documentation is like a good broth**
> Good documentation is a reliable base that supports the reader whatever their goals or experience. It doesn't demand that you start at the beginning, but if you do it supports you with resources that both let you start being productive quickly, and give you the context to make good design choices right from the beginning. It doesn't take the center stage , but whenever you encounter a particularly grisly problem it's there to soften it. It uses simple, accurate language, doesn't overwhelm, and is easy to search through. It supports total beginners, advanced learners, and skilled practitioners equally. 
> 	
> *The through line in all of this: good documentation meets the reader where they are and earns their attention incrementally. It doesn't demand that you already understand it in order to use it.*
> 
> So yes! Good documentation is out there. But sadly it's uncommon enough that when you find it, it's about as surprising as finding a diamond ring in your soup :)
> 
> The question I've been asking is, what would it take to raise the bar for what good documentation looks like, and lower the barriers to writing it? That's what I've been working on, and I'm excited to share more about what I think is the answer is. 
> 
> More on that next time.
> 
> 
> #### Third Draft (LLM)
> 
> Here's the revised piece with food metaphors woven into the "why" section and the other edits applied — your voice kept intact throughout:
> 
> ---
> 
> If you do any kind of engineering or technical work, you probably read a lot of internal documentation. Probably while trying to fix something for an irate client, or figure out some weird bug while trying to write some kind of business logic on a deadline. And if you're lucky enough to _find_ the documentation you need, half the time you'll be thinking _who was this actually written for?_ as you scroll through it. It rarely seems written for the person reading it to actually learn how to understand the thing being documented.
> 
> **Four types of bad documentation**
> 
> **The Soup** Tons of technical concepts, assumptions about the reader's familiarity with the subject, and poor discovery — designed to confuse you the moment you've left the 'Getting Started' section. You just have to wade in and hope you encounter what you're looking for, learning to swim in the process.
> 
> **Undercooked and Raw-In-The-Middle** This kind is especially frustrating when you lack a mental model of the thing being documented. Maybe it starts strong, but it's clear that whoever was writing it basically gave up when some other ticket pulled their attention away. If you're lucky it might give you an API method section with explanations, and some ultra-specific 400-line example written in PowerShell 5.1 and Python 2, with a cryptic _Ask Jerry_. Nothing says "we ran out of steam" quite like _Ask Jerry_.
> 
> **The Swiss Cheese** This is my biggest pet peeve — not least because it's extremely common, and is often mistaken for _good_ documentation. Generally serviceable, but riddled with small gaps, assumptions, and addendums that require you to constantly interrupt yourself to look at other docs or prompt on the side for missing context. It's experienced as a string of small frustrations that sap your ability to digest the information. Note: this is not confined to internal documentation. Looking at you, Microsoft Learn.
> 
> **The Sloppy Joe** If it's between no documentation and LLM documentation, the latter is _usually_ an improvement. The Sloppy Joe wasn't invented by people using LLMs, but LLMs make it much easier to produce and harder to spot than ever before. It's well-formatted, well-written, and generally palatable — but packed with filler, seasoned generously with hallucinated insights, out-of-date context, and all-around dumb assumptions. The worst part isn't the bits that are wrong. It's that 70–90% of it is accurate, but it all _looks_ great — which puts the burden on the reader to figure out what's sound and what's slop.
> 
> *_Note: Just_ using AI to help write your documentation doesn't make it a Sloppy Joe. It's not comprehending the input, verifying the output, and correcting where needed that earns the name. I'll write more in the future about how to use AI to actually improve your documentation, not just produce a lot of it.*
> 
> **Why does so much bad documentation exist?**
> 
> It's tempting to blame the people writing it. And yes, it's almost a meme how much engineers hate writing documentation. But when I see a recurring pattern that looks like individual laziness, it's usually a symptom of a poorly optimized system. My theory? A lot of people _want_ to write good documentation — they just don't, because the incentives are cooked wrong.
> 
> **Nobody's rewarding the chef.** When was the last time someone told you, "Hey, great job on the accuracy and clarity of that documentation — here's a bonus"? From the average business major's perspective, documentation is a cost center. Its impact is only rarely felt, doesn't show up in the quarterly report, and competes with shipping product and features for resources. So naturally, most people write to the minimum spec.
> 
> **The recipe isn't being passed down.** Institutional knowledge isn't treated as intrinsically valuable — which means it rarely gets documented, which means when someone who holds it leaves, it walks out the door with them. I once worked at a small firm where the CEO casually floated the idea of laying off the senior engineer with the most knowledge of our internal systems, because they'd moved two hours away and couldn't come in as often. Decisions like that do damage in slow, unpredictable, and highly inconvenient ways. And since the knowledge was never valued, it was never written down — which compounds the cliff when it comes.
> 
> **The secret sauce is job security.** If you've spent any time on job forums, you've probably seen this advice: _if you're the only one who knows how to prevent the business from bursting into flames, don't document it._ Because if a business is downsizing, documented knowledge turns you from someone with enormous leverage into someone replaceable. There's no shortage of stories about people hired back as consultants at $500 an hour to fix critical infrastructure — costing the company millions in downtime and lost clients. Can you blame them for keeping the recipe to themselves?
> 
> **What good documentation looks like**
> 
> Despite all of the above, good documentation absolutely exists, and I've seen enough of it to recognize the pattern.
> 
> **Good documentation is like a good broth.** It's a reliable base that supports the reader whatever their goals or experience level. It doesn't demand you start at the beginning — but if you do, it gives you the context to be productive quickly _and_ to make good design choices from the start. It doesn't take center stage, but whenever you hit a particularly grisly problem, it's there to soften it. Simple, accurate language. Easy to search. Not overwhelming.
> 
> _Good documentation meets the reader where they are and earns their attention incrementally. It doesn't demand that you already understand it in order to use it._
> 
> So yes — good documentation exists. But it's uncommon enough that when you find it, it's about as surprising as finding a diamond ring in your soup.
> 
> The question I've been asking is: what would it take to raise the bar for what good documentation looks like, and lower the barriers to writing it? That's what I've been working on, and I'm excited to share what I think the answer is.
> 
> More on that next time.
> 
> ---
> 
> **Changes made:** The "why" section now has three named headers in the same food-metaphor register as the taxonomy. Point 4 was folded into Point 1 ("nobody's rewarding the chef") rather than cut entirely — it fits there naturally and doesn't need its own section. The floating "not" is fixed, the Sloppy Joe footnote is tightened, and the through-line sentence in the broth section is set apart so it lands with more weight. Everything else is your words.
> 
> 
> Here's the revised piece with food metaphors woven into the "why" section and the other edits applied — your voice kept intact throughout:

---

**My Rewrite**

If you do any kind of engineering or technical work, you probably read a lot of internal documentation. Probably while trying to fix something for an irate client, or figure out some weird bug while trying to write some kind of business logic on a deadline. And if you're lucky enough to _find_ the documentation you need, half the time you'll be thinking _who was this actually written for?_ as you scroll through it. It rarely seems written for the person reading it to actually learn how to understand the thing being documented.

**Four types of bad documentation**

**The Soup** Tons of technical concepts, assumptions about the reader's familiarity with the subject, and poor discovery — designed to confuse you the moment you've left the 'Getting Started' section. You just have to wade in and hope you encounter what you're looking for, learning to swim in the process.

**Undercooked and Raw-In-The-Middle** This kind is especially frustrating when you lack a mental model of the thing being documented. Maybe it starts strong, but it's clear that whoever was writing it basically gave up when some other ticket pulled their attention away. If you're lucky it might give you an API method section with explanations, and some ultra-specific 400-line copy and pasted implementation example with a cryptic _Ask Jerry_. 

**The Swiss Cheese** This is my biggest pet peeve, not least because it's extremely common, and is often mistaken for _good_ documentation. Swiss cheese is generally serviceable, but riddled with small gaps, assumptions, and addendums that require you to constantly interrupt yourself to look at other docs or prompt on the side for missing context. It's experienced as a string of small frustrations that sap your ability to digest the information. Note: this is not confined to internal documentation. Looking at you, Microsoft Learn.

**The Sloppy Joe** If it's between no documentation and LLM documentation, the latter is _usually_ an improvement. The Sloppy Joe wasn't invented by people using LLMs, but LLMs make it much easier to produce and harder to spot than ever before. It's well-formatted, well-written, and generally palatable — but packed with filler, seasoned generously with hallucinated insights, out-of-date context, and all-around dumb assumptions. The worst part isn't the bits that are wrong. It's that 70–90% of it is accurate, but it all _looks_ great — which puts the burden on the reader to figure out what's sound and what's slop.

>>> move this to the comments: *_Note: Just_ using AI to help write your documentation doesn't make it a Sloppy Joe. It's not comprehending the input, verifying the output, and correcting where needed that earns the name. I'll write more in the future about how to use AI to actually improve your documentation, not just produce a lot of it.*

**Why does so much bad documentation exist?**

It's tempting to blame the people writing it. And yes, it's pracia meme how much engineers hate writing documentation. But when I see a recurring pattern that looks like individual laziness, it's usually a symptom of a poorly optimized system. My theory? A lot of people _want_ to write good documentation. They just don't, because the incentives are wrong.

**Nobody's rewarding the chef for writing down their recipes.** When was the last time someone told you, "Hey, great job writing that clear and accurate documentation. Here's a bonus!"? From the average businesses perspective, documentation is a cost center. Its impact is only rarely felt, doesn't show up in the quarterly report, and competes with shipping product features for resources. So naturally, most people write to the minimum spec, rather than pushing for Michelin-star quality. 

**The recipe isn't being passed down.** Institutional knowledge isn't treated as intrinsically valuable, and that means it rarely gets documented. When someone who holds it leaves, it walks out the door with them. I once worked at a small firm where the CEO casually floated the idea of laying off the senior engineer with sole knowledge of most several vital internal systems, because they'd moved two hours away and couldn't come in as often. Decisions like that do damage in slow, unpredictable, and highly inconvenient ways, often at 4 am on a Monday. Institutional knowledge loss reduces a company's resilience and ability to navigate challenges over time.

**Writing down your secret sauce recipe is bad for your job security.** If you've spent any time on job forums, you've probably seen this advice: _if you're the only one who knows how to prevent the business from bursting into flames, don't document what you know._ Why? Because documenting your knowledge turns you from someone with enormous leverage into someone replaceable. There's no shortage of stories about people hired back on short notice as consultants at $500 an hour to fix critical infrastructure costing the company millions in downtime and lost clients. Can you blame them for keeping that secret sauce to themselves?

**What good documentation looks like**

**Good documentation is like a good broth.** It's a reliable base that supports the reader whatever their goals or experience level. It doesn't demand you consume it in any particular way, but it gives you the context to be productive quickly _and_ to make good design choices from the start. It doesn't take center stage, but whenever you hit a particularly grisly problem, it's there to soften it. _Good documentation meets the reader where they are and earns their attention. And crucially, it doesn't demand that you already understand it in order to use it._

So yes - good documentation exists. But it's uncommon enough that when you find it, it's about as surprising as finding a diamond ring in your soup.

The question I've been asking is: what would it take to raise the bar for what good documentation looks like, and lower the barriers to writing it? That's what I've been working on.

More on that next time.

---
> [!cite]- **LinkedIn Vers. Draft**
> #### For Linked In (LLM)
> If you do engineering or technical work, you read a lot of internal documentation.
> 
> Usually while trying to fix something urgent. And half the time you find it, you're thinking: _who was this actually written for?_
> 
> After years of wading through it, I've started categorizing the types of bad documentation I keep running into.
> 
> **The Soup** - So many assumptions baked in that you're lost the moment you leave the Getting Started section. You just have to wade in and hope.
> 
> **Undercooked and Raw-In-The-Middle** - Starts strong, then clearly abandoned when some other ticket pulled the author away. Ends with a 400-line copy-paste and a cryptic _Ask Jerry_.
> 
> **The Swiss Cheese** - serviceable on the surface, but riddled with small gaps that force you to constantly stop and look elsewhere for missing context. Ubiquitous in public-facing docs too. (Looking at you, Microsoft Learn.)
> 
> **The Sloppy Joe** - well-formatted, well-written, and 70-90% accurate. The rest is hallucinated filler that blends in with the good stuff. Impossible to tell what's slop and what's not, so you have to verify all of it.
> 
> 
> Good documentation, when you actually find it, is like a good broth. It's a reliable base that enables you to make you want, doesn't demand you already understand it to use it, and is there to soften the grisly parts of tech work.
> 
> It's also about as rare as finding a diamond ring in your soup :)
> 
> I've been working on a recipe to change that. More on that next time.

**For LinkedIn My Rewrite**

If you do engineering or technical work, you read a lot of internal documentation. Usually while trying to fix something urgent. And half the time, if you find it, you're thinking: who was this actually written for?

After years of paddling through it, I've started categorizing the types of bad documentation I keep running into.

The Soup - Lots of assumptions about the reader's understanding of the thing being documented, and filled with abstract technical language. You just have to wade in and hope.

Undercooked and Raw-In-The-Middle - Might start promising, then clearly abandoned when some other ticket pulled the author away. Ends with a 400-line copy-pasted log and a cryptic Ask Jerry.

The Swiss Cheese - Serviceable on the surface, but riddled with small gaps that force you to constantly stop and look elsewhere for missing context. Ubiquitous in public-facing docs too (Looking at you, Microsoft Learn).

The Sloppy Joe - well-formatted, well-written, and 70-90% accurate. Includes hallucinated filler that blends in with the good stuff. Impossible to tell what's slop and what's not, so you have to verify all of it.

Good documentation, when you actually find it, is like a good broth. It's a reliable base that enables you to make what you need, doesn't demand you already understand it to use it, and is there to soften the grisly parts of tech work.

It's also about as rare as finding a diamond ring in your soup :)

I've been working on a recipe for changing that. More on that next time.

-G