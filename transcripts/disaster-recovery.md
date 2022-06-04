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

And there were various considerations there.  You've got the obvious physical disruptions - your bushfires, your floods, your natural disasters - but you would also have to deal, there, with smaller physical failures.  So if you had a power outage that affected your data centre, or even if you had a server rack blow, that could be quite a big problem.  And you would be dealing with the same sorts of things that we would deal with in terms of not on-premises disaster recovery around those attacks by malicious actors, and also around misconfigurations, code errors, things that might delete or corrupt your data accidentally.

But, of course, disaster recovery in the cloud is different.  We don't manage the physical data centres.  We don't need to worry about whether we're locating ourselves in Belgrave or Werribee.  So to understand the practical considerations, we're going to go back to our old friends at ProductCorp, and their recently acquired subsidiary Fly Money.

ProductCorp, for those of you who've never heard my talks before, is a Sydney-based startup.  And yes, it is spelled 'ProductCorp', that's because every startup has to find a naff way to differentiate themselves.  ProductCorp principally builds e-commerce software. They have two flagship products: BuyIt and SellIt.  BuyIt is essentially their version of Shopify; SellIt is software as a service for inventory management.  And ProductCorp runs entirely on AWS.

Now when we first met them - when I first gave a talk at the Melbourne AWS User Group a bit over two years ago - they were running a traditional server-based infrastructure.  So they were running EC2, they had everything in VPCs.  But during lockdown, they have migrated their infrastructure so that it is entirely serverless.  They are using Lambda for compute, they are using DynamoDB for data storage, and for the long-running order tracking which is part of the SellIt product, they're using Step Functions for that.  So this is an entirely serverless workflow.

Fly Money is a bit of a different kettle of fish.  They are a small payment processing company based here in Melbourne, and ProductCorp bought them in an effort to cut out the middleman.  Fly Money operates worldwide, primarily in emerging markets - so places like Eastern Europe, areas of Africa, and South America, where a lot of payments are generally done via mobile phone.  And, being a payments processor, they must comply with the PCI-DSS standard.

Unlike ProductCorp, Fly Money runs a traditional server-based architecture.  So we're talking about things like RDS for data storage.  We're talking about things like EC2 instances, which are- they're using an Application Load Balancer, and they're doing all of their scaling using Auto Scaling Groups.  Fundamentally, they're using some other services, but the vast majority of what they're doing is a traditional server-based architecture - what you would get if you lifted it out of on-premises and stuck it in the cloud.

And we've met Ben Nguyen, who was recently promoted to be ProductCorp's Head of Engineering - ah, their replacement is Otgonbat Enkhtuya, and I apologise to any Mongolian speakers here, because I have almost certainly horribly butchered her name.  She's the new Cloud Platform Lead at ProductCorp, so she's taken over Ben's job, and notably, she originally trained as a civil engineer.  One important thing that she learned during her training as a civil engineer is around business continuity, disaster recovery, and ensuring that you've actually got plans in place for what happens before your bridge falls over, rather than having your bridge just fall over on you.  And so one of the things that she has taken responsibility for here is the matter of business continuity and disaster recovery planning.

And so Enkhtuya, when she originally took over Ben's job - when Ben was promoted - set up ProductCorp's disaster recovery infrastructure almost from scratch.  She utilised a feature of DynamoDB called Global Tables, which allows you to configure DynamoDB in multiple regions at once.  It doesn't matter where you read or write data, it will all be percolated across all of the regions that you've got things configured in.  So they're running that in `ap-southeast-2`, in Sydney, and `ap-southeast-1`, Singapore, as a backup.  They have a couple of other safeguards: table deletion requires you to use multi-factor authentication, but if you need to actually restore something from a backup, DynamoDB supports point-in-time restore.  So you don't have to worry about the matter of going and restoring something manually from a backup, you can just roll back DynamoDB to any time - I think it's within the past 35 days.

They're running their Lambda functions on hot standby in Singapore as well, so that means that if anything goes wrong, they can immediately route their traffic to Singapore.  And their plan when the Melbourne region launches is to eventually move to an active-active consi- ah, configuration, where data can go to either the Sydney setup or the Melbourne setup, because the vast majority of their customers are in Australia.  And that provides them with a greater level of redundancy - in serverless terms, for not that much more cost.

Fly Money, again, is a different beast.  They brought in some very very expensive consultants to do their business continuity and disaster recovery planning.  They never actually asked their very expensive consultants whether they had any cloud experience, so the plan that they got back was primarily focused around things like workforce issues - what many of us have probably been dealing with in the past few years - and things like natural disasters.  In terms of their actual infrastructure, they've partially defined everything as infrastructure as code, so that means that if something goes wrong, they would have to bring some things up manually.  They have some self-healing of EC2 failures, but they're not using health checks at the application level.  They're using health checks one level above - essentially at the instance level.  So if something goes wrong with an instance, that's fine, it will be taken out of service.  But if something goes wrong with the application, they don't have the ability to check that.

The more interesting thing, at least from Enkhtuya's point of view, is the database backups.  These were being run- their databases running on RDS Postgres, these were being run via `pg-dump` on a cron schedule twice a day.  And the theory here was that those database backups were being sent to Digital Ocean Spaces, which is S3-compatible data storage.  That means that you can use the standard S3 tools if you ever need to to pull that data back in- into S3, and then restore onto RDS.  The problem with that is nobody actually checked to ensure that those backups are running.  So when they go in to look at Digital Ocean, they discover that the backups haven't actually run in any of the regions that Fly Money operates in - seven, as you'd recall from the previous
presentations - for the last 12 months.

Which is not a great place to start.  And so that brings us to the subject of a business continuity plan.  As I said before, this is not just about disaster recovery.  This is about, if something goes wrong, how will the business ensure smooth and continuous operation?  There are many many things that can be of interest here, but what we're primarily interested in are the recovery time objective and the recovery point objective - RTO and RPO.  These are core elements of any business continuity plan.  And I can boil them down for you to three questions.

RTO is 'how long can we afford to be down?  If everything goes down and it takes us 24 hours for u- us to bring our systems back up, can we survive?  Can we survive if they're down for 48 hours?'  RPO boils down to 'how much data can we afford to lose?  Is it five minutes of data, 15 minutes of data, an hour of data?  How much data can we as a business afford to lose - afford to have not backed up - once the system is back up?'  And the third question is 'can we do better than we can afford to without too much extra effort?'  It does not make a lot of sense, in most situations, to go from five minutes to one minute.  But if you can cut your RPO, say, from 24 hours worth of data lost to an hour worth of data lost without doing much more, that's quite good.  That's the kind of thing that you should be aiming to do.  So basically, 'what kind of losses can the business eat if everything goes completely wrong, and can we do it better than that?'

Then you've got to work out what your failure scenarios actually are.  `us-east-1` going down, in the cloud, is a very obvious one, but what you're looking at here is going to be specific to your company infrastructure.  And it should cover many different domains.  So you've got your pl- full platform outages, but note: that's not just cloud providers.  If you're using hosted Git, that's going to be relevant.  What happens if you lose your hosted Git provider?  If you have things like a password manager; if you're using an issue tracker, like a Jira; what are you going to do if one of those platforms goes down?

You've got the region outages, but another one to consider, particularly if you're in AWS, is things like availability zone outages.  Then you've got, you know, your component outages; you might have an underlying failure on the hardware of an RDS instance, that has been known to happen.  They're just EC2 instances at the bottom of it.  Or you might end up misconfiguring something, which means that it fails to come up in the first place, or it fails to work properly.  Another scenario that you want to understand is data loss or data corruption, and that can happen in a lot of different ways.  It can happen in the catastrophic scenario of an attacker going in and just deleting all of your backups, but there are situations, as well, where you can actually push code changes which will cause your data to become corrupt, and then you need to roll back.  So once you have worked out what your business can afford to lose, then you've got to work out what your failure scenarios are.

And then it comes to 'how do we actually mitigate them?'  And the most important thing to understand here is some fixes can be automated.  There are a lot of things that you can do here - and this varies from the very, very basic to the extremely complex.  So looking at some of the basic things.  [audience member coughs]  As we saw with Fly Money's infrastructure, if your instances are failing health checks, you can actually get your auto-scalers to tear them down and bring up new ones.  And that can be done as well, in AWS, at the application level.  You just need to configure it that way.  Another one which is maybe not as simple, but is really important, is if you've got DNS misconfigurations or networking misconfigurations.  That's what took down `us east-1` in one of those two outages last year.  So if you know what your DNS and your networking should look like, it's a good idea to build in some sanity checks, and potentially roll back any changes that look problematic.

In theory, you can do this with quite a lot of things.  AWS has EVentBridge; anything that fires events, you can look for those events and build on it.  But my advice to you here would be, if you're planning to go all in on self-healing architecture, start small.  It's very, very difficult to build an entire self-healing architecture from scratch, but if you can start with self-healing things like your virtual machines, your auto-scaling groups, your DNS, and then build on top of that, eventually that is what will get you to a fully self-healing architecture.

And as we all know, if you haven't tested a plan, you can't be sure that it works.  If you haven't tested a plan, that's how you end up with backups that haven't actually run for the past 12 months.  So before you say 'we have a disaster recovery plan *that works*', please make sure that you test it.  Another reason why this is really important, though, is that services and processes don't stay static.  ProductCorp, in two and a bit years, have evolved from an architecture that was entirely server based - based on your EC2, your RDS - to Lambda, Step Functions and DynamoDB.  Any disaster recovery plan that ProductCorp wrote two and a half years ago, at this point, was- is not worth the keyboard that it was typed on.  But another way that services and processes don't stay static is cloud providers will change things.  AWS is notoriously resistant to deprecating stuff, but even they've managed to deprecate the odd thing once in a while.  And another important thing that will change - which is the thing that AWS is not resistant to changing - is the UI.  So if there are things that you need to do manually, you want to test that process to make sure that people actually understand what the state of it is at any given point.

Another reason why this is really important is if your teams have tested it to make sure that it works, they're then going to be much more confident when someone calls them up, or whatever your pager system is wakes them up, at three in the morning and says 'Hey!  All of our production systems are down.'  When you're - and I can say this from experience - when you're running on very little sleep, if you've had a reasonable amount of experience poking around the system and you understand what's going on, you're going to do much better recovering the system in that scenario than you are if you've never touched the disaster recovery plan before, you've never tested it, or you don't even know what's in it.  So regular disaster recovery testing is really important for ensuring that teams are actually confident when they get that page.  And, speaking of pages, your alerting systems, your monitoring systems, and your pager systems are part of disaster recovery.  They should be part of your business continuity plan and you should be testing them.  For every one of those failure scenarios, it's well worthwhile to know where you're likely to get the warning from.  Is it going to be your call centre at three in the morning, or is it just going to come through an automated alert?

And again, in terms of the DevOps side of things, the best disaster recovery process is an automated one.  One of the best things that you can do, in terms of checking to ensure that things work, is integrating the testing of your disaster recovery into your day-to-day workflow.  So a few ways that you might do this: if you're building your infrastructure from- you know, you're building your dev infrastructure through infrastructure as code, you might actually want to tear that down over the weekends to save cost.  And so, if you can automate the rebuilding of your dev environments using infrastructure as code and CI/CD, w- and automate the tearing down, automate the rebuilding at the beginning of each day, or the beginning of each week, that's a double win!  Because it's going to save you money, and it's going to mean that you know that if your system goes completely kaputt, you do have the processes there to build it all up from scratch.  Another thing that you can do is - to ensure that your database backups work, if you need to work- if you need to have essentially, a pre-production environment which has data which is very close to the data in your production environment - in some scenarios you can take your production database, strip out any identifying information, and you can use that anonymised backup to construct test databases.  This is not going to work if you're in really highly regulated environments, but if you're not, it's a worthwhile method of considering because the database backups are generally the things that are most likely to fail.

So that is the end of the presentation.  I am more than happy to take questions.  Contacts otherwise are up there, and if you're interested in following the previous history of ProductCorp, there is a link there to all of the talks that I've done previously, including the ones that I've done at the AWS User Group.

AUDIENCE: [claps]

DAWN: Thank you very much!

ROB: [from a distance] Does anybody have any questions?  [pause]  Either online or in the room?

DAWN: [chuckles]

ROB: No questions!  Thank you very much, Dawn!

DAWN: Thank you for having me again!  [pause, Rob walks up to stage] Now I assume we just disconnect that.

ROB: Yep.

DAWN: And you'll be wanting that back.

ROB: Thank you.

DAWN: [sound of laptop hitting lectern]  And I really need to grow a third hand.

ROB: Is it possible- you can leave that there, I won't-

DAWN: Okay.

ROB: I won't need the space, I just need to grab the-

DAWN: Yeah.

ROB: -[inaudible] back on this one.

DAWN: You need that?

ROB: Yep.

DAWN: I'll give you that one back later, if that's alright.

ROB: Yeah, that's fine.  You might want to switch it off, though, unless- there we go, that should be off.
