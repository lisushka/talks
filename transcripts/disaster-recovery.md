# Disaster recovery in the cloud - Transcript

ROB: Okay, so-

DAWN: [laughs] Technical difficulties, please stand by!

ROB: Why is it never as easy as it should be?  Use as - mirror for built-in - there we go, much better.

DAWN: Except that we still don't have the slides up.

ROB: No, but they're in- there!

DAWN: Excellent! I do believe we have got the audiovisual working...[checks mic] Can you all hear me?

AUDIENCE: [inaudible]

DAWN: Excellent!  So, I am here today - this evening - back at the AWS User Group in person!  And it is wonderful, after two years of presenting virtually, to be able to see all of your faces.  I am here today to talk to you about disaster recovery in the cloud.

First of all, I would like to acknowledge that I am presenting on the land of the Wurundjeri people of the Kulin Nation.  This is, and always will be, Aboriginal land; their sovereignty was never ceded.

I don't actually know how many of you know me, because so many of these recently have been virtual.  But for those of you who don't; my name is Dawn; I do cloud security and DevOps at Innablr, and as per Mike earlier, we are hiring, so go and have a chat with him if you're interested in that sort of thing; and my credentials to talk to you about disaster recovery are that I am a habitual over planner.  I never have a plan A and a plan B, I always have to go through plans C, D, E, and F before anything gets executed.  Outside of work, I am an occasional author and kitchen alchemist - sometimes that ends really well, sometimes that ends badly - and if you're wondering why I'm standing here talking to you in a hockey sweater, that's because I am also a raging sportsball fan.

So - business continuity and disaster recovery.  The disaster recovery bit is inherent from the fact that this is a talk on disaster recovery in the cloud, but part of what we're really talking about here when we talk about disaster recovery is the concept of business continuity.  And that's not just 'what are we going to do if everything falls over?', that's 'what do we need to do to ensure that our business keeps functioning, and what kind of plans do we need to put in place?'

Why would we want to do this?  Well, if you cast your mind back a few months to December 2021, anyone who was working with AWS at that time will remember that the `us-east-1` region went down - twice! - in less than a month.  And, in AWS terms, `us-east-1` is actually a single point of failure.  Many of the glob- so-called global services run almost entirely or entirely out of `us-east-1`.  So if you're running any global services, you need to have some sort of backup plan in place [laughs] for when `us-east-1` goes down next.  It's also often required for compliance purposes.  If you want to operate in a highly regulated industry, more often than not you are actually going to have to have some sort of business continuity plan that you can present to your regulators and say 'yes, we can do this'.  Proper business continuity planning will also help you to fix things when they go wrong.  We'll get more into that later.  And, probably most importantly - although perhaps rarest - it can help you mitigate the consequences of attacks by destructive malicious actors.  These are the people who put on black hats and go 'har har har' as they exfiltrate all of the data out of your system.  That's something you probably gen- generally want to avoid.

And, a hat tip to my colleague Luke Carter-Key for coming up- or, for surfacing for me this beautiful example of 'what you want to avoid'.  Until the 17th of June 2014, there existed a company called Code Spaces.  And what they did was they provided, essentially, a collaborative Github, where you could host your code, you could go in, you could sign in on the cloud, you could- and you could do things.  The reason why Code Spaces no longer exists is thanks to the work of one of these destructive malicious actors.  What happened to them was they were DDoSed by someone, and when they went in to try to recover control of their system, the attacker went- the attacker, who had created a backdoor into their system, just went in and started deleting all the backups.  And that meant that it was no longer financially viable for them to run.  This is the absolute worst case scenario, this is why we do business continuity planning.
