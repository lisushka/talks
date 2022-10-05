# What's New in AWS - September 2022 - Transcript

ARJEN: Then I will have Dawn join me now for our 'What's New In AWS' section which is full of positives.

DAWN: [laughs, walking stick clunks]

ARJEN: Cool!  So, we'll start with, erm, 'Finally in Sydney'.  For those of you new here, 'Finally in Sydney' is basically the section of the news where we talk about services that have been available in other regions for a long time - or sometimes not that long - and are now finally available to us here in Sydney.  The first one of these is QuickSight Q, which from my understanding is basically a way of- type in your query and QuickSight will generate a pretty dashboard for you.

DAWN: It's natural language processing but for QuickSight dashboards.  I have never played with it, I assume it's a relatively fun toy?  It's probably worth trying at least once.

ARJEN: Yep.  And the other one is EMR Serverless, which is EMR, but serverless.  Erm, this is also one of those 'serverless' services that were announced at re:Invent last year that aren't actually serverless, but are branded serverless.  So you can't scale it down to zero.

DAWN: [laughs] You can't scale it down to zero, but it might still save you some money depending on how you're architecting things.

ARJEN: Yes.

DAWN: Yes.  It- it has less server.

ARJEN: Cool.  Speaking of less servers-

DAWN: [laughs]

ARJEN: Erm, the main one here that I want to call out - and it's not really serverless, but it was the best place to put it - is the new console for Simple Workflow Service.  Erm, who among you remembered that this service existed?

DAWN: I did, because it was on all of the AWS exams! [laughs] It's- this is a service that has been obsolete for probably the entire time that I have been [using AWS, it's been superseded by Step] Functions, but apparently we have a new console for it now.  I'm sure that someone- someone had a lot of fun designing this, probably.

ARJEN: Yeah.

DAWN: Speaking of Se- Step Functions, we have 14 new intrinsic functions in Step Functions.  And these are cool, because they let you do built-in things like, ah, processing of JSON data.  A lot of different workflows which you might not necessarily ex- think of when you're thinking of tying things together with Step Functions, but they allow you to actually set things up without having to go in and build the overhead yourself.  That's nice.  I like that we're getting more of those things- we're getting more of those scenarios where AWS looks at something and goes 'yeah, this is a use case that we can solve for'.  That's good.

ARJEN: Yep.  Another one that's very useful, erm, is RDS Proxy support for RDS for SQL Server.  If, erm - for those of you- so, RDS Proxy is basically a way to proxy your database connections for your Lambda functions, basically making the round trip to the underlying database a lot shorter.  And if you are stuck with [RDS Proxy], if you're running something like .NET - erm, which we had a wonderful presentation about last wee- er, month - then this should save you a bunch of time.

DAWN: Yeah, I'm- 

AUDIENCE: [inaudible]

ARJEN: Sorry?

AUDIENCE: [inaudible]

ARJEN: I believe it maintains proxy connections.  I think that's just- mm, the thing, how RDS proxy works.

AUDIENCE: [inaudible]

ARJEN: I think it tries to re-establish, erm, because Lambda functions are generally very short-lived, so it can't keep consistence.

DAWN: I really want to know who has a workflow that involves SQL server and Lambda that's not .NET.  If you have one of those, I would love to hear about it.  Come and give a talk!

ARJEN: [chuckles] Containers!  Erm, good news for everybody who uses containers the right way - using Fargate - you can now, erm, you can now increase the size to up to 16 virtual CPUs and 120 gigs of RAM.  This is be- one requirement for that is that you opt-in to the new [vCPU pricing, that's an] underlying requirement for this, so that's why you have to do that.  Not entirely sure why, but that's-

DAWN: Oh, I- I bet you that was a customer request somewhere, but big beefy workloads on Fargate?  Great.  More excuse to throw out the EC2 instances?  Yes please.

ARJEN: The other Fargate thing, erm, it supports Windows Server 2022.

AUDIENCE: [inaudible]

ARJEN: I think any.  Of course, erm, if you go for the- if you go for the biggest option, you'll probably run out, or you- that's probably why they waited with this for the 16 CPUs and 120 gigs of RAM because you'll need that to run it. 

DAWN: Yeah.  Yeah.

ARJEN: That's AWS' problem.

DAWN: Ah, if you are using ECS, whether you are using it with Fargate or not, you now have faster scale in and scale out with ECS.  I actually can't remember if that's an EC2 only thing or if it's Fargate as well, but that's good.  Erm, that's- it's nice to see ECS getting a little bit of love in that regard.  Ah, trying to sort of get it to the point where it works better than some of the knobs and levers that you get for service- services that have been around for longer might be another question, but faster scale-in and scale-out is always good.

ARJEN: And of course, if you're a big fan of Kubernetes, you can now use Kubernetes to deploy your Lambda functions.

DAWN: If you- if you really want to set everything up so that you have Kubernetes orchestrating your entire platform - including the AWS Managed Prometheus service, so that when your Kubernetes cluster goes down, you will also lose your Prometheus service - that is now a thing you can do!  I don't know why you would want to.  If you are using Kubernetes, please run your monitoring stack somewhere other than the Kubernetes cluster that you're running it on, because you don't want your monitoring stack to go down when your Kubernetes cluster goes down.  But yeah, you can now do this!  You can orchestrate your Step Functions from Kubernetes. I don't know why you would want to, but you can!

ARJEN: Cool. Then, Dev and Ops.  Erm, shout out to Chris, who's probably in the livestream - Coretto 19 is out, this is a mandatory thing we always have to say, I didn't bother looking up what's new about it.

DAWN: [laughs] We do have the CloudFormation new language extensions transform.  This is clever.  I really, really want to play around with this at some point.  It allows you to use functions, ah, intrinsic- intrinsic- more intrinsic functions in CloudFormation via language extensions transformation.  I want to see what people can do with this.  I really want to see someone push the bounds of that, I'm curious to see whether that's something where you're going to be able to do game-breaking stuff with CloudFormation as a result of that.

ARJEN: Yep.  Basically, under the hood, how this is implemented is that it's a CloudFormation Macro, but it's one that's hosted by AWS - similar to SAM - so you don't actually have to deploy anything to use these transforms.  The other thing with CloudFormation is - some of you may have noticed this already, erm, if you deploy CDK applications, it will now show a preview in, erm, the resources section.  This preview is by default completely collapsed and you have to click a lot to see everything.  Personally I think they can make some tweaks to it to make it a bit more user-friendly, but it's an attempt to improve things.  We'll see if it's, ah-

DAWN: Hopefully- hopefully it will get better.

ARJEN: Yeah.  And you can now run Nitro enclaves, erm, with Apache.  So, people who remember when Nitro Enclaves actually came out, this is a way to split up your EC2 instance; basically into a regular EC2 instance and a very secure one, and that way you can- erm, the main use case was that you could load ACM Search into your NGINX.  Now you can do the same with Apache.

DAWN: Ah.  Security.  And I noticed the words Control Tower on there, which means, because I'm up here, you know there's a rant coming!  Guardrail management through APIs.  I'm pretty sure that I'm on record at least four times having said that the th- the one thing that I really wanted from Control Tower was APIs?  Well, we got them, but they're not quite what I was looking for. [inaudible] manage - allegedly - you can manage the guardrails which Control Tower provides around your AWS organisation setup using APIs.  This has a few fairly major limitations; you can only query it if you know the OU or account that you are querying for some things, which means that you cannot go in and just download a full set of data.  It's also only the guardrail management, you don't have account management.  It's a start.  I hope that it's the start of many many more, because I still don't know that I can get up here and with a straight face advice people in large enterprises to use Control Tower after this announcement.

ARJEN: A ringing endorsement.

DAWN: [laughs]

ARJEN: Erm, speaking of APIs that have been long-awaited, erm, IAM Identity Centre - more commonly known as SSO, which was the old name - now has APIs for managing users and groups.  I haven't tried it out yet to see how well it works but I'm, erm, I actually know some of my colleagues have used it, and were ma- able to do the management, so that's a good thing.

DAWN: Yeah, I mean this one is very much a case of- any sort of API for that was going to be vastly better than what already existed.  So, it's not like- it's not like the Control Tower rant.  This is something that if it's implemented well, it's- it's really good.  Being able to manage that automatically is a really good thing.

ARJEN: Any other that you want to-?

DAWN: Yeah, I- the Route 53 support for DNS resource record set permissions is the other one that I want to pull off here, because it used to be that when you were setting up permissions in Identity Center - formerly SSO - or however you were running IAM users or what have you, for the- for DNS resources, ah, so Route 53, you would have to set those permissions up by hosted zone.  Now you can do it with subsets of DNS records in a hosted zone, you can do it with individual records in a hosted zone.  This is really cool, because it means that you can essentially have one person manage- one person or group manage the hosted zone, and you can delegate permissions for managing the subdomains out to different groups of people, or different teams in your org.  I really like the fact that there's now some separation of concerns there, which allows you to actually give people some control.  It means that that domain-based stuff doesn't always have to go through a central platform team.  That's great.

ARJEN: Cool.  Other cool stuff.  A very finally moment - erm, the health dashboard now actually shows us times and dates that make sense.

DAWN: [laughs]

ARJEN: Because, I mean, obviously we all were used to thinking of what time was it in UTC when this event happened.  Well, now you can actually just realise, ah, this was during the night.  So that's awesome.

DAWN: Yeah.  Mildly disappointed that, ah, Matt isn't up here for this one, because we've got the, ah, CloudFormation support for Amazon Connect instance creation, and I'm sure that-

MATT: Woohoo!

DAWN: -he would have a great deal to say about that were he up here.  And again, in much the same vein as me and Control Tower, I feel another presentation coming on at some point!  If you want to use Connect, now you can automate it.

ARJEN: Yep.  Connect also has a couple more search APIs which I'm sure, erm, are useful.  And of course, the thing everybody has been waiting for is the console home widgets for blog posts and launch announcements, so that you can see the latest five announcements whenever you open your console.

DAWN: See, I want to say something snarky about that, but there are so many scenarios where I've come up here and done What's New In AWS, and only known about things that have changed because of that, I actually think this is a good thing.  Because it means that people- at least for the launch announcements, people are going to realize when things have changed, and they're going to stop doing things the old way.  Hopefully.  We can hope, right?

ARJEN: Hopefully, except, erm, if there's a limit of five being shown, I haven't seen many days for AWS only released five things.  So the one that you're after might not actually show up.

DAWN: This is true.  You're- you're relying on the timezones lining up.

ARJEN: Yep.  Cool!  And that's it for the news!  This is not what we're going for now.  So, thank you Dawn.

DAWN: I will take my leave.

ARJEN: And then we'll move on to our first speaker.
