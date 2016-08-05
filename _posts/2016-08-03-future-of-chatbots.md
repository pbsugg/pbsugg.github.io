---
layout: post
title: Bots and Unstructured Interfaces`
---

I recently attended an event in Chicago about building a simple chatbot of one's own.  Everyone wants a pet chatbot these days.  Bots are an eye-catching subject of discussion, from events and endless simple tutorials to a startlingly large number of [frameworks](https://dev.botframework.com/) and [platforms](http://www.pandorabots.com/) that have come a long way in the past year. The most enthusiastic bot boosters suggest that the need for more bots is so urgent that we should enable even those without any coding knowledge to [build and deploy them](https://chatfuel.com/)!  

Although I haven't (yet!) built one myself outside of simple tutorial examples, I have some experience with [NLP](https://en.wikipedia.org/wiki/Natural_language_processing) technologies and have done enough digging into the state of the field to offer a few thoughts about how this technology will challenge both consumers and developers. Bots are an attractive solution to developers' [increasing difficulty](http://qz.com/253618/most-smartphone-users-download-zero-apps-per-month/) convincing users to download their apps, but bots they are to offer the same value as an app with the equivalent functionality, users are going to have to learn a new language of engagement and a new set of expectations about interacting with them.

A lot of the current thinking around bot implementation is concerned with the problem of interface design.   According to this thinking, if web developers and UX/UI designers have traditionally been concerned with creating intuitive menus and workflows, the addition of bots to our sites will force us to go back and rethink good design principles within this new linguistic paradigm. How is your user supposed to react if he/she gets a few successful questions deep into a conversation with a bot and suddenly makes a malformed statement that isn't recognized?  Start from the beginning? Ask the bot to back up to the last thing it understood? Although [some commenters](https://medium.com/swlh/a-natural-language-user-interface-is-just-a-user-interface-4a6d898e9721#.j0c802gqo) think the problem is overblown--that "user interface is still user interface"--I want to suggest that that the interface problem is real, that *communicating a bot's depth, sophistication and capabilities* will continue to be a challenging problem for the foreseeable future.  It's not enough to spend time on the back-end making your bot a little (or a lot) "smarter," you've also got to figure out how to communicate this intelligence to users in an intuitive, transparent way.

Although they've actually been around in different forms for a while, bots take advantage of a newfound optimism about the progress of artificial intelligence systems to process language.  The advantage of the so-called "conversational" interface is that it takes advantage of something distinctively human--language--to bring the user closer to the goal of the interaction in the first place.  Ideally, they a conversation communicates something about an individual's or a brand's personality that visual design cannot.  When a Swiss developer designed a simple chatbot (with pre-selected responses) [for his personal website](https://azumbrunnen.me/) he drew enthusiastic reviews and a huge spike in traffic, if for no other reason than the novelty value. It allowed him to speak to his audience in his own voice, and to get to the point of who he is and what he does in a simple and direct manner. But since this bot interface provides just one or two possible answers to every question in the chat, he's provided heavy guardrails to sidestep the problem of open-endedness that inevitably occurs when you start allowing users to ask their own questions. The answers provided structure to the unstructured, open-ended conversation that is human langage

The bot concept covers a lot of territory. There are the anonymous and ubiquitous "help" bots that spring up in chat apps like Slack--really a slicker version of the help menu and so boring and limited in its capabilities that you hardly want to give it a name.  On the other end of the spectrum is an open-ended, high-concept, slickly-branded example like [Viv](http://viv.ai/), the so-called "intelligent interface to anything" built by some of the same people who created Apple's Siri. In between are the bots that interact with users to convey some routine set of information or services, like a bot that would be made available to new employees to orient them to [company policies]  (https://www.technologyreview.com/s/602068/the-hr-person-at-your-next-job-may-actually-be-a-bot/), or a bot that buys you pizza on Facebook.

Let's just call a bot any system with an unstructured, natural language interface. But the problem with an unstructured interface is that user still needs to have an intuitive sense of its capacities and limits. If not, frustration sets in. A few questions the user has to have an intuitive grasp of: What can I say (type) to this bot, how precise do I have to be in saying it, and what is my range of confidence when I ask it something I've never asked before? When I first got my iPhone with Siri, I asked it (her?) a lot of questions, but I mostly lost interest in it after a few days because I found its success unpredictable, and it didn't seem worth the hassle of discovering new features where I'm sure Siri would have exceeded my expectations. So there is an unstated structure that has to be there in a unstructured linguistic interface. It's the structure of user's awareness of the domain limits, and the user's confidence within that domain that queries will be answered.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Rblb3sptgpQ" frameborder="0" allowfullscreen></iframe>

In the [demo](https://techcrunch.com/2016/05/09/siri-creator-shows-off-first-public-demo-of-viv-the-intelligent-interface-for-everything/) that is part of Viv's unveiling, we basically get a system that is good at taking complicated, multi-part voice commands and turning them into actionable queries for multiple other apps.  First you ask it the weather (4:45), then the weather a few weeks from now (5:20), then we move into  complex queries with multiple qualifications (5:55): "Will it be hotter than X temperature at Y location after Z time..."  Once the presenter orders a taxi and flowers, it becomes clear what it means for Viv to be an "interface to anything."  Viv aspires to be the universal translator for other apps, the most intelligent form-filler, box-checker, and command builder for graphical interfaces, ever.  Viv, its creator Dag Kittlaus says, will one day be as ubiquitous as the bluetooth symbol on your smartphone screen.  His demo is all about setting expectations.  Although it was very impressive to see what sorts of complexities Viv could handle, it was also clear what the triggers were: prepositions (after X, before Y,) and recognizable nouns with clear referents (times, places, names).

As I watched the demo, I was already forming a mental map of how I would need to structure my speech in order to make it click with a Viv-like interface.  Let's break up one statement into key lines:

  Will it be
  warmer than
  70 degrees
  near the
  Golden Gate Bridge
  after
  5 pm
  the day
  after
  tomorrow

Kittlaus also gives us a look behind the scenes, where we saw a graph of how the Viv interface parsed the speech into a "dynamically generated" set of methods and function calls in the correct order, an on-the-spot, fully-coded algorithm for passing instructions to the app.  But the user probably didn't need to see this slide--just the demo--to understand the level of precision and ambiguity that the Viv interface would tolerate.  The response of the bot itself has to convey to us the limits of the interface.  Viv fills out the appropriate fields in the weather app, and we see the command come to life.

Now, in a world where a user might come across dozens or more bots a day, quickly and efficiently conveying the capabilities and limits of an interface is no easy task.

It would have been interesting if the Viv demo had included a failed command, because how a bot fails will probably be as important as how it succeeds in communicating the unstructured interface.

When a simple bot, like Slack's *help* bot, fails it simply tells me "I'm afraid I don't understand. I'm sorry!"  Many commands that seem to have a different meaning offer the same response, like "what is a channel" and "where are channels."  As a user, I learn the bot's limitations through a combination of (1) failed commands that yield me nothing and (2) commands that are too specific and default to a more generic answer.  

If a designer wants convey more information to the user about the bot's limitations, here area few things he/she could try:

* A **welcome message**. Slack's bot says that it will try to answer questions, and if it fails, then it will consult the "help center." But isn't the slackbot already giving me information from the help center?  Does this mean it will send me help center articles based on keyword searches if it doesn't understand my queries?
* A **command history**.  Wouldn't it be useful to see a list of questions that the user has asked so far--which ones have failed and succeeded, with summary information about the sort of information they yielded? Make it abbreviated--just break out the keywords of the interaction and summarize their response, so that the user begins to grasph the interface structure
* A **"counter-bot" or an "interpreter" bot**.  If I'm interacting with a both that performs a service or fetches something for me, maybe it gives the user more confidence to provide another helper bot that could deal with common pitfalls if it looks like the interaction is failing.  This bot could us machine learning tools to build up knowledge of all the past mistakes made by other users.  It could even respond automatically, jumping into the conversation as a "third party" once the user starts to get frustrated. Granted, these capabilities could be built into the main bot, but it changes the user's perception of the interface if the help comes from another "helper."
* Finally, think more about the **power of demonstrations**.  Find unobtrusive ways to show your user what the bot can do.  If the bot has a cool capability related to the current query, alert the user to this effect.

All this leads me to three big considerations about creating structure in a bot interface:

1.  Communicate the value of of a bot upfront.  Think about how you can *show* a user what the bot can reasonably be expected to know.  For a bot that works with other apps, there is nothing like seeing it interact with the graphical interface immediately to give the user a sense of its value. For a chat bot, give me a sample question.  This also means being sure of your domain.  Should a bot that tells you about public transit also answer questions about the time and weather?  It's a design decision that has to be communicated effectively to the user.
2. Generate trust from your user.  Fail gracefully and spend time thinking about how your user's interactions might go wrong.  A bot that does a few things really well might still be quite useless if it performs other, related tasks very poorly.  This leads me to my third point...
3.  Sophistication in an unstructured bot interface is inherently invisible and unstable.  A cool feature that is buried in the tree of possible user interactions might go unnoticed. And one failure can be more visible--and more discouraging to future investment in bots-- than a hundred wins. If you want to build a bot, think through how you would communicate its sophistication to your organization to justify further investment.  If you start with a small set of capabilities and gradually build them up, how will you keep showing off the bot's new powers to your users?

With the growing powers of NLP tools, bots offer a lot of promise, but the linguistic paradigm presents a new set of challenges for interface design.






Lessons:
  Be sure of your domain; aim low to begin with; don't have to be sophisticated if can do *one* thing well!
  Communicate the value--should it know the time? What can I reasonably be expected to know?
  Generate *trust* in the app.
  If you can do something cool, be upfront about it; spend time with examples
  Sophistication is invisible and unstable--how does an organization justify investment?





See Matt Schlicht's [recent post](https://chatbotsmagazine.com/the-complete-beginner-s-guide-to-chatbots-8280b7b906ca#.6mpygm2w1) for a nice rundown of the options, and watch his [twitter feed](https://twitter.com/MattPRD?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) which has been particularly active on this front.
