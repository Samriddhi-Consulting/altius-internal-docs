# Doer to Leader

For six years, you were the person people came to when a thing had to actually get done.

At ClearSight Systems, which sells compliance software to banks and NBFCs, that reputation was a career. You were the engineer who knew why the audit module broke in 2021 and how it was patched, and who could get the infrastructure team to prioritise a ticket just by walking over to their pod. You once unblocked a release at 11 PM because the product team trusted your read of the risk more than their own ticket queue. When something stalled anywhere near your team, the shortest path through was you.

Four months ago they made you Engineering Manager. Eight reports. Your manager, Sandhya, said one thing in your transition chat that you wrote down and have looked at twice since: "Your job is no longer to be the shortest path. It is to build eight people who don't need one."

You nodded at the time, the way you nod at most good advice, filing it under things that would obviously make sense once you'd had more practice. Four months in, you have had less practice at this than at almost anything else you have ever done for a living, including the parts of the job nobody warns you about, like how strange it feels to sit in meetings where your job is to say less, not more.

You have thought about that sentence most mornings since, usually right before your calendar fills up with exactly the kind of thing it warned you about.

The Meridian Bank programme did not make the transition easier. It was already amber when you took the manager title, and it has stayed there because every integration decision that touches a regulator-facing field mapping has to pass through product sign-off. Engineering can build the pipe. Product owns what goes through it. Meera's stream is where those two truths collide every week.

---

**The Deployment**

Meridian Bank is not a logo on a slide deck. It is a Rs 14 crore annual contract, the firm's largest regulated-banking deployment in three years, and the reference case sales will use for every NBFC conversation next quarter if the go-live lands clean. The integration layer Meera owns sits between Meridian's core banking feed and ClearSight's audit and reporting modules. Field mappings, API behaviour, client-facing routing rules: each one needs a product owner to say yes before engineering can commit code.

Sandhya ran the programme review last Friday. Meera's lane was amber. Not red, which would have triggered executive escalation, but amber long enough that people had started asking questions in side channels. You said you were coaching Meera through the product dependency pattern. You did not say that three of your other reports had asked you to make similar calls in the last six weeks, or that your own calendar still had blocks labelled "quick sync with Nikhil" that you had not yet deleted.

---

**Meera**

Meera Iyer joined the firm two years ago from a services company, and within six months nobody remembered the team without her. She is the one who reads the entire spec before the kickoff, and whose code reviews catch what everyone else waves through. Her estimates, you can bank. School topper, college medallist, the profile is easy to read: she has spent her whole life being excellent at things that have a syllabus, and this is the first team she has joined where nobody handed her one.

Her current assignment is the first one without one. She owns the integration layer for the Meridian Bank deployment, the firm's biggest go-live of the year, and the work depends on decisions that sit outside engineering: field mappings the product team must confirm, an API behaviour only the product owner can sign off, a client-facing question that has to route through product because that is the rule.

The go-live is in five weeks. Her workstream has been amber for two. Nobody else on the team is blocked the same way; the dependency happens to sit entirely on her part of the build, which means there is no one nearby whose habits she could simply copy without being told to.

---

**What You Know That She Said**

Yesterday's stand-up, Meera, evenly but with an edge you hadn't heard before: "Still blocked on product. I've sent four emails in two weeks. Nothing."

Afterwards she asked for thirty minutes on your calendar today. The invite title: "Need help unblocking product dependencies."

You did what you now do before these conversations, a habit you picked up in your first month and have not told anyone about: looked, quietly, at what has actually happened before walking in with an opinion. Her four emails are in the thread she forwarded you. They are thorough, which is Meera. Each one is long, addressed to the product group alias, lays out the full context from the beginning, lists between six and nine open items, and closes with "please confirm at the earliest." Three went out after 7 PM. None names a single owner. None says which item blocks the go-live and which can wait. The fourth one, from Monday, copies you.

---

**What You Know That She Didn't Say**

The product team is neither idle nor malicious, just five people carrying three go-lives, and their head, Nikhil, has his team triaging ruthlessly: anything without a named owner, a stated deadline, and a reason attached goes to the bottom of the pile. A nine-item email addressed to an alias is, in their triage, not a request but a document.

You know this because of Arjun, on your own team, who somehow always has his product answers. You have watched how, more than once, because you used to wonder if he was simply luckier with timing than the rest of the team. He walks over or calls, asks for one decision at a time, tells them what it blocks and by when, and books fifteen minutes with the specific owner when it's genuinely complex. Nikhil once told you, half joking, that Arjun is the only engineer whose name in an inbox does not make his people tired.

Last Tuesday you saw it happen in real time. Arjun needed a sign-off on a webhook retry policy for a smaller client. He walked to the product pod, caught the owner between stand-ups, and left with a decision in eleven minutes. You noticed because you were on your way to refill your water bottle and overheard the exchange. One question. One deadline. One name. Meera was at her desk when he walked back, still drafting her fourth email.

---

**What You Know About Nikhil's Team**

Nikhil runs product with five people and three concurrent go-lives. His triage rule is not personal; it is survival. Emails without a named owner, a stated deadline, and a reason attached sink to the bottom because his team cannot guess urgency from volume. Meera's four emails read, in that system, like thorough documentation of a problem, not a request for a decision. That is not something she will hear as neutral feedback unless someone helps her see the product inbox the way Nikhil's team sees it.

And there is the other thing you know, which is about you. Six years of unblocking releases means Nikhil takes your calls on the second ring; you have earned that the slow way, one honest read at a time. One call, ten minutes, and Meera's nine items would be closed by Friday. You have already reached for the phone once this morning and put it down.

Because you have started noticing a pattern in your first four months. Your calendar is full of other people's blockages. Three of your eight reports now open conversations with "can you talk to," and you can trace it back: each time, the fastest path was you, and you took it, and the next time they came back sooner. You have not yet said this out loud to anyone, including Sandhya, partly because saying it out loud would mean admitting how much of your first four months you have spent doing the job you already knew, instead of the one you were actually given.

---

**Priya**

There was a version of this on the old team. Priya, two years ago, blocked on a security review, and you, then the star IC with the network, fixed it in an afternoon with two calls. She thanked you sincerely. The next quarter she brought you the same problem in a different jacket, and the quarter after that she brought it before trying anything at all. You didn't notice the pattern forming in real time; you noticed it eighteen months later, reading her exit interview notes, where the phrase "always knew I could just ask" appeared twice. When she moved to another company last year, her farewell message to you said, warmly and without irony, that she didn't know how she'd manage without you. You have thought about that message more since the promotion than you ever did before it.

The go-live pressure is real; five weeks is not forever, and letting Meera flounder indefinitely would be its own failure, a different way of failing her than the one you are more tempted toward. But what she is asking for is the answer, today, from the person who always has one, not a week left to struggle alone.

---

**The Clock**

It is 3:15 now. The meeting room you booked holds six but feels smaller when someone brings a blocker and a deadline in the same breath. Meera's amber workstream feeds the programme dashboard Sandhya reviews every Monday. You have a staff meeting at four-thirty. Nikhil's number is still on your phone from this morning, the call you started and cancelled.

Five weeks to go-live. Two weeks amber. Four emails that did not move the queue. One report who has never missed a deadline on work that was fully in her control, standing outside your door for the first time asking you to be the shortest path again.

---

**What's Actually Yours to Give Today**

You cannot move the go-live, and you should not want to; five weeks is tight but real, and Meera's workstream genuinely needs to turn green in the next few days, not the next few weeks.

What you can give her: your time, your questions, and, if she ends up needing it after working through her own options, an introduction to Nikhil that she asks for and makes herself, not a call you place on her behalf. The difference between those two is most of what this meeting is actually testing.

There is a version of the next thirty minutes where you leave feeling useful and she leaves feeling handled. There is another version where you leave feeling unhelpful, having barely said anything of substance, and she leaves with something she built herself and will still be using on a different blocker six months from now. Only one of those is the job Sandhya described.

---

**3:30 PM**

She comes in exactly on time, laptop open, the email thread already on screen, prepared as always. She sits, turns the laptop slightly toward you, and takes a breath like someone who has rehearsed the opening line on the walk over. Through the glass wall behind her, you can see the rest of the team at their desks, not looking over, all of them aware in that unspoken office way that this particular half hour is not a normal one.

She is about to speak. You still have Nikhil's number one thumb-swipe away, and Sandhya's sentence written in the margin of your notebook from four months ago. The go-live clock, the amber dashboard, and the choice between feeling useful today and building someone who will not need you tomorrow are all in the room with her.

**[END OF PRE-READ]**
