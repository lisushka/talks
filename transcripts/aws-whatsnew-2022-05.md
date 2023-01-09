# What's New in AWS - May 2022 - Transcript

ROB: You good to go?

ARJEN: Good to go.

DAWN: Fewer technical difficulties?

ARJEN: Cool.

ROB: Ah, nope.  [pause] Hey, there you go!

ARJEN [mumbles] Oh, whatever.  [audible] Cool.  What's new: a working laptop.

DAWN, ROB, AUDIENCE: [laughs]

ARJEN: So, erm, as Rob said, welcome everybody!  It's good to see so many faces in person - still getting used to it.  I was at the summit last week; there were even more people there, and it was a bit scary almost.

DAWN: [chuckles]

ARJEN: But let's talk about What's New in AWS since last month!  For those of you new, erm, the first slide is always reserved for 'Finally in Sydney' - services that were not in Sydney before.  Erm, they already existed, but we did not have access to them.  But now we do!  And there's a couple of them to- er, this time.  So, first of all, the new RDS Multi-AZ option is available, in case you missed the announcement for that.  I think it was last month?

DAWN: Yeah.

ARJEN: Yep.  Erm, in case you missed that, it is the ability, instead of having an RDS that doesn't do anything, that is only there for failover, you can have two extra RDS instances that run as, erm, read-only options.

DAWN: Mmm. I expect that AWS are going to have to rewrite a lot of their exams.  This may be somewhat connected to the announcement that, ah, the Solutions Architect Professional exam is changing.  It is going to drastically change the way that pretty much everyone running on AWS with, ah, any sort of redundancy in their RDS architects their systems.  It's a huge improvement.

ARJEN: Yep.  It sure is.  The other ones are several new instance types.  Erm, I can't recall at the moment what the `r5b` instances are.  Did you?

DAWN: Not off the top of my head.  The smorgasbord of letters and numbers is getting so complex that it's extremely difficult to keep track of them.

ARJEN: Yeah.  It certainly is.  Erm, and that's exemplified especially by the last ones which are the `x2idn` and `x2iedn`.  These are two instance types specialised in extra memory as well as extra network bandwidth, and the `e` stands for extra.  So that one has extra extra memory.

DAWN: [chuckles]

ARJEN: You know, in case you need it.  Erm, it's basically designed for SAP applications, because no- nothing else needs that kind of memory.

DAWN: Yeah.  It's a very niche use-case, and the general expectation would be that very few people will use these.

ARJEN: Yep.  So, let's move on to more general things.  Serverless.

CHRIS: [inaudible]

ARJEN: Thank you Chris.

DAWN: [laughs]

ARJEN: Erm, Lambda supports Node.js 16.  This is good if you're using Node.  Erm, I don't, I try to avoid it, but- Don't hate me for that.  But yeah it's great new versions always good

DAWN: yeah um the managed uh I can't remember what the uh exact terminology for this one actually is but this is the AWS managed kafka service um is the serverless version of that is now general availability I don't know how many people actually use AWS managed kafka servers but theoretically being able to do this in a serverless manner should save you some money.

ARJEN: yeah just keep in mind that it's serverless in some air-quotes because it's not serverless like Lambda as in you still have things running

DAWN: As I said: theoretically will save you some money.  Erm, your mileage may vary.  And do make sure that you work out the pricing before you actually switch over.

ARJEN: Yep.  Ah, the other two announcements here are more quality of life improvements.  Erm, the SAM CLI supports being able to turn on X-Ray, which is great, and Step Functions has a new console for- that is especially good for debugging.  Erm, I could tell you about it but I recommend just reading the blog post because that actually shows things, and it's hard to paint a word picture around how well this works.

DAWN: Yeah.  Definitely glad to see Step Functions getting a little bit more love.

ARJEN: On the container side everybody's favorite servers except for matt in the back there EKS the EKS console that you see all standard kubernetes resources so in the past it would hide some of the basic stuff for you now it just shows everything

DAWN: for people who use Kubernetes this is definitely a good thing it's something of a personal bugbear of mine when cloud services abstract away things that they really shouldn't particularly for people who are just getting started with this sort of service it makes it much more difficult to follow what's actually going on so I'm all for this one.

ARJEN: yep.  Kubeflow on AWS this is a project from AWS to did you remember what this one was we're not sorry

DAWN: we're not doing very well this month are we yeah yeah that would thank you yes it's a machine learning tool to run your machine learning things more easily on Kubernetes and this is specialised is focused really for running it on EKS or just in the AWS cloud

DAWN: yeah probably in the vast majority of cases if you are running a machine learning workload there are almost certainly better ways to do it than on kubernetes so unless you're google probably not a tool that I would recommend that anyone try

ARJEN: Erm, app mesh supports IPV6 as kubernetes and vpcs and all that got um better IPV6 support in the past it's good that the app match which is the service mesh service from AWS now actually supports being able to call those endpoints it's kind of a hard thing otherwise to use this in combination

DAWN: yeah it remains to be seen whether the improved IPV6 support in AWS is going to result in more of the internet moving towaRDS it but considering how long it's been a standard, I'm not holding my breath

ARJEN on the Dev and Ops then we've got CDK support for service catalog this is actually in two ways you can call server schedule you can use service catalog products items within your CDK applications and you can write your service catalog items in CDK instead of just pure CloudFormation

DAWN: I really like this this is this is a very very good quality of life improvement particularly if you have organisations where most people working there are developers and don't necessarily have notable ops experience being able to roll everything into CDK is really important for them so seeing more and more of the ops-y AWS services get CDK support I think is overall very very positive for the ecosystem

ARJEN: yep um CodeGuru reviewer allows suppression of files folders if you use code google reviewer oh well I suspect not many people here you can now tell it to not look at certain thousand folders this can be useful if you have some legacy code that you're never going to clean up or things like that

DAWN: yep that's about it there's not really much else to say about that one yep did you want to talk about any of the others yeah the um the incident manager expanding the runbook automation is sort of interesting in a very niche way to me because this is a service that I have looked at a few times and there's just it's one of those services where you feel like it could be so much more than it is so I'm glad to see that there's a little bit going into that now and I'm really hoping that I'm hoping that there's more automation that goes into that service in the near future

ARJEN: cool then on the security side one of my favorite announcements in a long time is the SSO delegated admin

DAWN: yeah this is the this slide's probably more my wheelhouse than most of what we've had before SSO delegated admin is an absolutely amazing this is this is fantastic what it essentially allows you to do is rather than having to manage your AWS SSO configuration from your org management account you can now delegate the management of the SSO into a separate account if you are dealing with a really big enterprise where say you have one team managing the AWS platform but you're doing identity using something like AzureAD or Okta, this now allows you to delegate an account to the team that essentially your team that's doing identity management so if they can manage all of the SSO themselves.  I expect this to be adopted very, very quickly in a lot of big orgs where that sort of communication between those two teams has been a massive bottleneck to things actually happening I've been in that situation once myself I wish we could have just delegated it so this is this is a really really good announcement

ARJEN: yep I definitely wish it had been around for a longer time as this is so much in your wheelhouse anything else here you want to

DAWN: yeah um secrets manager with the usage metrics is one that I think is interesting I think there will be that's another one where I think your primary use case is going to be in really big orgs that want to understand how they're actually utilising secrets manager and potentially pick up some of those patterns that they might get where information is in there but people are sort of exfiltrating it and they're doing things in ways that they shouldn't be that's really positive for getting everyone around and being able to actually audit are people doing things in the way that they should be and Control Tower my old nemesis at the bottom there I'm very very glad to see that as slow as it has been the promised evolution of Control Tower is actually happening uh the customer provided core accounts in particular if you're trying to migrate from an AWS landing zone this makes it orders of magnitude easier to do because you can just take the accounts that Landing Zone generated for you and plug them straight into Control Tower so it's reaching the point where it's a usable service the other thing of note with the Control Tower is the Python 3.9 um basically last month Python 3.7 went end of life for Lambda but all of Control Tower all the Lambda functions it creates were Python 3.7 so a couple weEKS later they upgraded all of those to 3.9 so you'd stop getting messages from AWS that you should update your Lambdas.

DAWN: Yeah it would be nice if next time they updated the lime just before they end of life them but we'll see how that goes

ARJEN: yep and then our last slide new instances so the big one here is probably the c7g these are the Graviton 3 instances that it was pre-announced at re invent they are I think 25% faster use like half the power from the c6t instances even bigger differences with the Intel machines or AMD um a nice side effect of this as well is that AWS once again made the t4g instance t4g micro instance parts of the free tier so you can play with ARM-based instances free just to see how it goes/

DAWN: yeah definitely seeing um the AWS Graviton instances get first-class support now which is overall I think going to be very beneficial in most use cases

ARJEN: and the other one that I want to call out here because it's pretty cool um is the RDS for postgres cascading read replicas this basically means so you can in general you have you have five [muffled] read replicas for replicas for your instance now each of those five read replicas can have five read replicas and those can have five read replicas as well so that gives you a total of 155 read replicas if you have a workload like that please talk to me I would love to see that.

DAWN; [laughs] yeah I'm absolutely in agreement with that one it's funny how the same things caught our eyes this is I really want to see what workload is so read-heavy that you would need 155 dedicated read replicas I also really want to know how much architecting time went into this because this must have been a hell of a feat to pull off but there has to have been some customer request somewhere hey this is what we're looking for for this to actually come out so kudos to them I guess

ARJEN: yep and I think with that that was all the news so thank you Dawn.

DAWN: Thank you, Arjen.

ARJEN: And on to the speakers!

AUDIENCE: [Applause]
