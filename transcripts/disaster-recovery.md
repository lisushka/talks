# Disaster recovery in the cloud - Transcript

ROB: Okay, so-

DAWN: [laughs] Technical difficulties, please stand by!

ROB: Why is it never as easy as it should be?  Use as - mirror for built-in - there we go, much better.

DAWN: Except that we still don't have the slides up.

ROB: No, but they're in- there!

DAWN: Excellent! I do believe we have got the audiovisual working... [checks mic] Can you all hear me?

AUDIENCE: [inaudible]

DAWN: Excellent!  So, I am here today - this evening - back at the AWS User Group in person!  And it is wonderful, after two years of presenting virtually, to be able to see all of your faces.  I am here today to talk to you about disaster recovery in the cloud.

First of all, I would like to acknowledge that I am presenting on the land of the Wurundjeri people of the Kulin Nation.  This is, and always will be, Aboriginal land; their sovereignty was never ceded.

I don't actually know how many of you know me, because so many of these recently have been virtual.  But for those of you who don't, my name is Dawn; I do cloud security and DevOps at Innablr, and as per Mike earlier, we are hiring, so go and have a chat with him if you're interested in that sort of thing; and my credentials to talk to you about disaster recovery are that I am a habitual over-planner.  I never have a plan A and a plan B, I always have to go through plans C, D, E, and F before anything gets executed.  Outside of work, I am an occasional author and kitchen alchemist - sometimes that ends really well, sometimes that ends badly - and if you're wondering why I'm standing here talking to you in a hockey sweater, that's because I am also a raging sportsball fan.

So - business continuity and disaster recovery.  The disaster recovery bit is inherent from the fact that this is a talk on disaster recovery in the cloud, but part of what we're really talking about here when we talk about disaster recovery is the concept of business continuity.  And that's not just 'what are we going to do if everything falls over?', that's 'what do we need to do to ensure that our business keeps functioning, and what kind of plans do we need to put in place?'

Why would we want to do this?  Well, if you cast your mind back a few months to December 2021, anyone who was working with AWS at that time will remember that the `us-east-1` region went down - twice! - in less than a month.  And, in AWS terms, `us-east-1` is actually a single point of failure.  Many of the glob- so-called global services run almost entirely or entirely out of `us-east-1`.  So if you're running any global services, you need to have some sort of backup plan in place [laughs] for when `us-east-1` goes down next.  It's also often required for compliance purposes.  If you want to operate in a highly regulated industry, more often than not you are actually going to have to have some sort of business continuity plan that you can present to your regulators and say 'yes, we can do this'.  Proper business continuity planning will also help you to fix things when they go wrong.  We'll get more into that later.  And, probably most importantly - although perhaps rarest - it can help you mitigate the consequences of attacks by destructive malicious actors.  These are the people who put on black hats and go 'har har har' as they exfiltrate all of the data out of your system.  That's something you probably gen- generally want to avoid.

And, a hat tip to my colleague Luke Carter-Key for coming up- or, for surfacing for me this beautiful example of 'what you want to avoid'.  Until the 17th of June 2014, there existed a company called Code Spaces.  And what they did was they provided, essentially, a collaborative Github, where you could host your code, you could go in, you could sign in on the cloud, you could- and you could do things.  The reason why Code Spaces no longer exists is thanks to the work of one of these destructive malicious actors.  What happened to them was they were DDoSed by someone, and when they went in to try to recover control of their system, the attacker went- the attacker, who had created a backdoor into their system, just went in and started deleting all the backups.  And that meant that it was no longer financially viable for them to run.  This is the absolute worst case scenario, this is why we do business continuity planning.

And - for those of you dinosaurs who are many years older than me, ah, you would probably remember this back i- [laughs]

AUDIENCE: [laughs]

DAWN: Apologies - I am very, very young, but you would remember this back in the days of on-premises.  I'm a baby.  I don't remember
it back in the days of on-premises.  But it was, at that stage, based around physical redundancy and physical separation.  So what you would have is you would have a great big tape deck, into which you put rolls of actual physical tape.  Your backups would go onto them, and then you would take these backups, and you would put them in the back of a truck.  And you would ship them off to some warehouse, somewhere, hopefully temperature controlled, hopefully not near any large magnets, and hopefully not prone to flooding.  And if you had something completely catastrophic happen, that was your basic disaster recovery plan.  You would get your big truck, you would ship the backups back, and you would reinstall them.

The other alternative - and this was *hideously* expensive to do back in the on-premises days - was to operate multiple isolated data centres.  So, the Melbourne example here would be you would have a data centre in Werribee, and you would have a data centre in Belgrave - essentially two opposite sides of the city.  And what that allowed you to do was, if you had a bushpa- a bushfire in Belgrave, your data centre
in Werribee would still be running.  If you had a flood in Werribee, your data centre in Belgrave would still be running.  Of course, this meant that you actually had to handle transferring data between those two data centres, which, again, hideously expensive endeavour.

And there were various considerations there.  You've got the obvious physical disruptions - your bushfires, your floods, your natural disasters - but you would also have to deal, there, with smaller physical failures.  So if you had a power outage that affected your data centre, or even if you had a server rack blow, that could be quite a big problem.  And you would be dealing with the same sorts of things that we would deal with in terms of not on-premises disaster recovery, around those attacks by malicious actors, and also around misconfigurations, code errors, things that might delete or corrupt your data accidentally.

But, of course, disaster recovery in the cloud is different.  We don't manage the physical data centres.  We don't need to worry about whether we're locating ourselves in Belgrave or Werribee.  So to understand the practical considerations, we're going to go back to our old friends at ProductCorp, and their recently acquired subsidiary Fly Money.
