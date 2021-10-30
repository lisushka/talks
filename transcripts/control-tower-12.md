# Control Tower 12 Months On - Transcript

DAWN: All right!  Ah, welcome back everyone!  Once again we are looking at AWS Control Tower; and it's not really 12 months on, is it?  Because I think it was actually about- August or September last year that I did the original presentation.  But, this is a look at AWS Control Tower 12 and a bit months on.

Ah, first off I would like to acknowledge the Wurundjeri people of the Kulin Nation on whose land I am presenting tonight.  This is and always will be Aboriginal land.  Their sovereignty was never ceded.

Yeah, I- I probably don't need an introduction by now, but on the off chance that there are people here who- haven't seen the introduction before, my name is Dawn.  I'm now doing cloud security and DevOps at Innablr - goodness me, I do move around a lot! - and Innablr is hiring. Er, and my approach to handling AWS environments is effectively - the more AWS accounts the better!  Spin them down, bring them back up again, configure them, leave them as sandboxes for people to work in.  Outside of work, I am an occasional author and kitchen alchemist - sometimes that ends really well, sometimes it doesn't.  I'm also a raging sportsball fan, which is why I rotate through pictures of me doing or wearing sportsbally things.

So - a little bit over 12 months ago, we met ProductCorp.  And yes, it is pronounced 'ProductCorps' even though it's spelt ProductCorp, because startups are naff and want to use anything they possibly can to differentiate themselves.  So let's meet ProductCorp yet again; let's have a little refresher on what they're actually doing.  So this is, ah, a startup; their head office is in Sydney, and ProductCorp's primary market is in e-commerce software.  So they have two flagship products: they've got BuyIt, which is a- inventory management software as a service platform; which connects with SellIt, their other flagship product, which is effectively a managed online shopping platform similar to Shopify.  Everything that ProductCorp does is targeted towards the Australian market, being that they're a startup with their head office in Sydney, and their infrastructure runs on AWS, which is very important for what we're looking at here.  They've been in business for about seven years, and they're starting to hit the scale where- they've got a bit of money to play around with as a result of investments now, and they've been using other payment processors and have decided that they want to cut out, ah, the middleman.  And so they have gone out and bought a payments processing company called Fly Money.

And the process of merging all of Fly Money's workloads into ProductCorp's existing setup will be headed up by Ben Nguyen, who we have met before, but they've been promoted since the last time we talked; they're now ProductCorp's Head of Engineering.  And they're very passionate about improving infrastructure in general - they've got over 10 years of experience, so they are quite looking forward to seeing what they can learn from Fly Money's systems.  Spoiler: probably not going to be what you think it is.  You know, they've mainly worked with startups before, so they're very familiar with this sort of- running AWS at a small scale and scaling it up.

And in terms of the way that ProductCorp's AWS accounts are set up, unlike when we checked in with them- a bit over 12 months ago, it's actually quite sensible.  So, they've got everything split out into separate organisational units, and because they don't have too many products, it's split ou-  basically according to the production life cycle.  So they've got `prod`, `pre-prod`, `dev`, and `lab`.  And for each of their services that they're running, they have individual- prod, pre-prod, and dev accounts for those, and for microservices, some of them are split out into separate accounts as well.  They've also got one lab account per team: basically a sandbox where people can go in and play around with things.  And their account management and guardrails, that's all done with Control Tower.

A couple of interesting things to note, though; all of ProductCorp's workloads run in `ap-southeast-2`.  And that's going to be important later.  The other thing that's important to note is that their access to AWS is controlled via SSO - which is backed by an identity provider.  So, things like AzureAD, Okta, I know there are a few others, erm, Google Workspace unfortunately doesn't quite work for this, because it's only got about a quarter of the features of a full-fledged IDP, but it's very easy to basically plug your IDP into AWS and have that manage SSO for you.  And that's what ProductCorp are currently doing.

Now Fly Money's AWS accounts, unfortunately for poor Ben, are a bit of a different beast.  They - their head office is actually in Melbourne, so they are an Australia-based company, but their primary market is sort of emerging markets.  So they're looking, they operate primarily in- Europe, ah, but also in Africa and South America.  And that means that their workloads are running across a total of seven different AWS regions.  When it comes to the accounts though, it's a bit of a Wild West.  Ah, not only do they have not really any particular controls over what can be deployed into their accounts, they've only got three of them, and they don't really have clear boundaries about what goes where.  I'm sure everyone at this point is starting to quietly groan to themselves.  What's even worse, though, is that their access is all managed by IAM users, and- I'm sure I don't need to go over again- why that's a problem, particularly for a payment processing company.

So Ben's a bit worried at the end of this meeting where everything is handed over to them, and they get on the phone to Marie Novotna.  And she's been promoted as well since we last checked in with her; she's now ProductCorp's security lead.  And- by the time that this phone call is finished, she's starting to get the feeling that she was sold a bit of a lemon, because she was told that Fly Money had excellent AWS governance.  And so she, with her multiple A- AWS security certs, is currently a little bit cross.

So they meet with Annie Jones, who we've also met before.  Ah, which is interesting because - she used to work for an auditing firm, and so of all of the people that work at ProductCorp, she is the one that is most familiar with auditing processes, which is one of Ben and Marie's first concerns.  And she has absolutely no idea how Fly Money ever passed a PCI audit, because it's quite easy, once you've got, you know if you've got PCI workloads that are properly isolated, it's quite easy to be able to pl- deploy other stuff to AWS accounts, and use some interesting attack vectors that you can't use from outside the eco- or, from outside that individual account.

So, there's an emergency meeting called with basically all of the C-suite in it, and Ben and Marie are given full blessing to go in and do whatever they need to do to fix up the situation with the Fly Money AWS accounts.  And they're going to fall back to the tried and true maxim of using existing patterns, and things that you already know you can get working.  Which in this case is AWS Control Tower.

So Ben and Marie's new plan is actually fairly similar to Ben and Marie's old plan - when we first checked in with them, a little bit over 12 months ago.  So the first thing that they're going to do, ah, with a little bit of fiddling around with- enrolling and unenrolling from organisations - is get all of Fly Money's accounts into the ProductCorp AWS organisation, in - isolated into one OU.  And then they're going to take that existing OU and they are going to enroll it into Control Tower.  Once they've done that, which at least gives them some basic governance, then they've got to do the legwork that Control Tower enables you to do around- splitting workloads out into multiple AWS accounts and actually making sure that they've got the structure in there that would allow them to pass a PCI audit without telling too many small fibs.  The other thing that they really want to do here is- get rid of all of the IAM users.  And their plan to do that is - all of the Fly Money employees have been issued with ProductCorp email addresses, so they're just going to migrate all of those logins - into the ProductCorp IDP, and have everything accessible via AWS SSO.

And the new Fly Money accounts are going to follow very much that same sort of split; separate accounts for each of the seven regions; one production and pre-prod account per region; one dev account per service per region, and those will have region restrictions as well on deployments because then it means if someone goes and breaches Fly Money's operations in South America, they don't necessarily get into everything else; and the old one lab account per team.

Now, the interesting thing here is that a lot has changed in Control Tower in the past- 12 months and change - and if ProductCorp had acquired Fly Money 12 months ago, they would have been faced with a very different proposition in terms of solving this problem for a few different reasons.  The first one is that 12 months ago, Control Tower had only just made it to Sydney, and that was actually - Sydney was in the first five regions to get access to Control Tower, and ProductCorp, er, Fly Money operates in a bunch of regions which Control Tower wasn't available in 12 months ago.  So that's actually quite a big change which has enabled this to happen ;it's now available in 15 of the 21 AWS regions, so well over half, and another big change that has happened is you can now select which regions you want to be governed with Control Tower.  Before, it was effectively all or nothing, and every time new regions were released you had to upgrade Control Tower.

The six regions that Control Tower's not available in are- Cape Town, so `af-south-1`, and Bahrain - Middle East - the thing with those two is there's actually no other AWS regions in those areas.  But for Europe, Asia Pacific, and America - `eu-south-1`, `us-west-1`, `ap-east-1`, and `ap-northeast-3` - the vast majority of companies that are using AWS in those regions are probably going to be using regions now that are covered by Control Tower.

I did mention though that one of Fly Money's markets was Africa.  So when the Cape Town region was released - which was fairly recently, actually - Fly Money immediately rushed to move all of their workloads into `af-south-1`.  Which is a bit of a problem for them, because that's one of the six regions that's not covered by Control Tower at the moment,

Fortunately for Ben and Marie, there is actually a work around here.  Because although Control Tower actions don't happen in regions where Control Tower hasn't been deployed yet, Control Tower itself as a solution is actually backed by Service Catalog, and that means that you can get lifecycle events out of it.  And AWS has really kindly - written up a whole blog post on how you can use those lifecycle events and other AWS services - basically by subscribing to the lifecycle events, you can then replicate a lot of Control Tower's workflows by triggering off other services.  And that's a pattern, if you're interested in having other things - basically, automation built on top of Control Tower - that's actually a pattern that you can build on as well.  So I definitely recommend, if you're interested in Control Tower, check out that blog post.  It's in the sources.

And as we sort of covered earlier, when you spin up Control Tower it's not an all or nothing thing anymore, but you do also have the ability to deselect regions.  So if Fly Money and Ben and Marie- effectively decide that they only want to- have Control Tower govern the regions that they're actually operating in, they could do that.  They could just go in and manually remove all of the other regions where they don't operate.  I'm not necessarily advocating that as a good idea, but if you want regions unenrolled from Control Tower for particular reasons, that's something you can do.

Now, guardrail updates.  Not a lot has changed here, but a lot of the changes that have happened are actually quite impactful.  One of which, we'll sort of cover the implications of later, but there have been updates to the IAM- guardrails around protecting Control Tower resources, which was set up that way to enable enrolment of existing OUs.  There have also been a number of updates to guardrail names and descriptions to clarify what they actually do, which is something when I was first testing this out last year I actually got caught out by.  So that's quite a nice change.  And another change that's happened is around the log archive account that's provisioned for governance. It used to be that- anything around S3 buckets and CloudTrail- you just couldn't do anything with.  And that was mandatory, that there were locks on there to disable configuration changes.  But that's been changed now, so if you want to do other things in the log archive account where you do want people to be able to go and change them, that's now an elective guardrail so you can opt in or opt out of that.  So that is good in the sense that it enables- ah, it enables you to do a lot more with the log archive account, and to decide how you want to actually operate that, which is something you couldn't have done before.

The OU changes are a pretty big one.  And as we see from the IAM guardrail changes, when I looked at this 12 months ago you would have to go in and create your own organisational units to get things to work with Control Tower, and then move accounts over: that is no longer the case.  You can now go ahead and enroll OUs that already exist into Control Tower directly.  You can also when you spin up Control Tower customise the names of the log archiving OU, and the security audit OU that are created at the beginning, with the new Control Tower landing zone.  And even better, the Control Tower accounts page used to only show OUs and accounts that were governed by Control Tower: it now shows you everything.  So it gives you a pretty easy way to go in and make sure that everything that should be governed by Control Tower is

Now, last year, I had a bit of a bee in my bonnet about lack of CLI support.  And there is a new feature in Control Tower which, while it doesn't fix everything- well, it doesn't give me everything that I asked for, does fix a lot of the main problems with that.  Before, if you wanted to update- run- roll out Control Tower updates to your accounts, they either had to be done manually with each account or via scripting.  But now you can do it by organisational unit.  So for ever- any OU that has 300 accounts or less in it, there is now a one-click button in the Control Tower console that allows you to roll out those updates across every account in the OU.  So that means it ensures that the version of Control Tower is updated, and it also ensures that all of the guardrails are properly applied.  So this is really important for making sure that you don't end up with drift in systems.

Customisations for Control Tower was - something that I hadn't looked at too much when I was playing around with last year, but as Control Tower has become a more compelling proposition, Customisations has actually got a lot better too.  There's been several bug fixes and general usability improvements, but the real big deal here is that Customisations version 2 actually introduced a simplified schema, so it's now much easier to set up resources using customisations if you want to.

There have been a couple of security changes.  Ah, Control Tower now automatically checks for drift against Control Tower service control policies.  So those are not - ah, mandatory enforced guardrails, but those are detective guardrails effectively rather than preventive; that's now checked for you every day and you can get reports of the results, you don't have to do that manually.  Another good change security-wise is, you used to have to use AWS managed keys for encrypting Control Tower's CloudTrail logs - erm, and apparently that's got me on the wrong slide, sorry about that.  You used to have to use AWS managed keys for encrypting CloudTrail, but now you can bring your own.  So you can use keys that are already existing, or you can create your own customer managed keys for that.  So that's quite nice in terms of, you know - if you have policies already for managing keys, it allows you to manage that yourself.

And another really good thing is, if you absolutely- dislike Control Tower and you decide that you want to get rid of it, you can now manually decommission Control Tower from within AWS.  And this, again, is something that I did; and it used to require a support ticket and then a little bit of waiting for AWS to get back to you.  So that's nice, because it gives people the flexibility now to try it out, and if they don't like it, it's quite easy to roll back.

There are also a few things that I talked about last year, though, that haven't changed.  And the first one of these is the old chestnut of CLI support.  Control Tower has improved quite a lot since I last looked at it, but it is still fundamentally a ClickOps tool.  It's not something where you can go ahead and roll it out using the CLI, and it's not something where you can make changes and roll out updates using the CLI.  So, probably the one use case where I would s- be advising you to hold off on looking at Control Tower, is large organisations that already have a lot of scripting and programmatic management of their account life cycle process.  Because Control Tower introduces ClickOps steps to that.  However, if you're in a smaller org, the bulk update feature actually quite effectively balances this out, because even though it is ClickOps you're clicking one button per OU.  There's no longer a lot of manual work that you have to do to do a lot of those updates.  So it's still not there, but there is some mitigation there.

The other thing that Control Tower doesn't have that I talked about last year is - allowing you to actually create custom Config rules and custom service control policies that are then managed by Control Tower.  This isn't really a problem though, because- the ecosystem of automation has grown around Control Tower, and there are a lot of examples out there already- that you can actually use and modify, to- have Control Tower life cycle events manage custom guardrails if you want them.  And you can do that with CloudFormation.  It's still something that I hope to see Control Tower do at some point, because having everything managed in one place is really nice, but there are ways to do it yourself that are fairly trivial.  It's a little bit of setup and then it's done.

So one of the big questions here is: does this change the use cases?  Because last year, Control Tower was bleeding edge software, and I would- I, I sort of had a fairly niche list of recommendations.  And I would really say that Control Tower works pretty well for everything except for- you know, as I mentioned, large organisations, but I think there are some very good specific use cases here.  So- if you know that you're never going to need to create new AWS accounts, or you're unlikely to need new AWS accounts in the foreseeable future, Control Tower is a really compelling proposition, because you can- set up your landing zone, and you can enroll everything in Control Tower and- have Control Tower do a lot of the management for you, and all you need to worry about then is the Control Tower updates.  And likewise, if you- are in an organisation that doesn't really have account creation processes, Control Tower is a really good piece of software- or a really good service to use, because it will basically give you an introductory- you know, it'll give you a lot of those account creation processes out of the box.  If you need to make major security improvements, Control Tower is really good for giving you a baseline around account setup that then allows you to go in and do some of the dirty work - so, it gives you logs that are set up properly, it gives you a security audit account, you can then go in and set up other AWS services- things like GuardDuty, some of the other security services.  And another good use case here is- if you get audited a lot, and if you find audits really annoying, you can basically just- you know, you can use the Control Tower documat- documentation, and basically say yeah, this is what we do.  So it saves you having to prove a lot of the work that you're doing.  Another good scenario is- if you have a cloud migration or a security overhaul that's being done by consultants, if you don't have a lot of people with specialised AWS knowledge in your organisation, the learning curve for Control Tower is not that steep.  So it's quite easy for consultants to come and set this up, and then for- staff who maybe have a bit less knowledge of AWS to maintain it.

So- that's a look at Control Tower 12 months on, and a look at the continued adventures of ProductCorp as they, ah, enter the world of mergers and acquisitions.  I'm assuming that we're going to have question time now, so throw them at me, and if you want to contact me later on, email address is- on the slides.

ROB: Thanks, Dawn!  That was awesome.  Erm, as always, your presentations are very very in-depth, erm, but they strike a good balance on, erm, telling a story and actually keeping- keeping it interesting.  It's really good.

We don't have any questions posted yet.  Now is the time to post them anywhere you like; we'll see them and we will ask them on your behalf.  So while we're doing that... Nope, no one yet.

DAWN: I don't see anything.

ROB: I was amazed, while we're waiting, erm, that there is a whole different- part of AWS that I've never touched.  And that's to do with- I've never had to go through an audit, or any sort of compliance or anything like that, and erm, just watching that makes me realise how little I know.  It's not really surprising I guess, but...

DAWN: Yeah it's- it's a whole- beast unto itself, and the- the- the Fly Money use case is one that I used here, because I actually think it's quite interesting to look at - how much, er, oh- I mean a lot of- a lot of cloud providers- but AWS in particular will do to help you with an audit, and how many places don't leverage that.

ROB: Yeah.  Yeah, I guess the- the more auditing and compliance tools you can use to your advantage, the easier the the audits become.

DAWN: Oh, absolutely.  And particularly with PCI, erm, you know, y- sometimes you do wonder, like- there are obviously companies that do this really well, and AWS will willingly abstract a lot of it away for you, but sometimes you do wonder, you know, how much of that- how much of that do people go in and manually do, or how much of it do they not do properly and they don't- you know, not even that they don't- ni-, not even that they lie, but they don't realise.

ROB: Yeah.  Yeah, I could- could certainly see how ignorance could be a big- a big big part of- part of this, because you just don't really know, well, you don't really know - like if I had to sit down and do an audit at the moment, I would probably- roll over and just cry for a while, because I wouldn't even know where to start.

DAWN: Yeah.

ROB: Erm, but now I do!  Start with Control Tower.

DAWN: [chuckles] Yeah, it's not a bad- it's actually not a bad starting point.  It's, it's definitely improved a lot since 12 months and change ago.

ROB: Yep, and as always with AWS, if they're keeping rolling out improvements, then it's only going to get better over time.

DAWN: Yeah.

ROB: Okay, doesn't look like we've had any questions come in.  Erm, but Dawn is always on slack, or usually around somewhere on slack if you do want to ask questions.  Erm, and there was the contact details that you posted just a moment ago as well.

DAWN: Yeah and I'll sit in the- I'm sitting in the, er, YouTube chat as well, so I will answer any other questions that are asked by text.

ROB: Awesome!  Cool.  Thanks again Dawn.

DAWN: Yeah, thank you for having me again.

[Back to main talks repo](https://github.com/lisushka/talks)