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

DAWN: Yeah.  Erm, the, erm, managed, er, I can't remember what the, er, exact terminology for this one actually is, but this is the, ah, AWS Managed Kafka service, erm, is- the serverless version of that is now general availability.  I don't know how many people actually use AWS Managed Kafka servers, but theoretically being able to do this in a serverless manner should save you some money.

ARJEN: Yeah.  Just keep in mind that it's 'serverless' - in some air-quotes - because it's not serverless like Lambda.  As in, you still have things running.

DAWN: As I said: theoretically will save you some money.  Erm, your mileage may vary.  And do make sure that you work out the pricing before you actually switch over.

ARJEN: Yep.  Ah, the other two announcements here are more quality of life improvements.  Erm, the SAM CLI supports being able to turn on X-Ray, which is great, and Step Functions has a new console for- that is especially good for debugging.  Erm, I could tell you about it, but I recommend just reading the blog post because that actually shows things, and it's hard to paint a word picture around how well this works.

DAWN: Yeah.  Definitely glad to see Step Functions getting a little bit more love.

ARJEN: On the container side: everybody's favorite servers, except for Matt in the back there, erm, EKS!  The EKS console lets you see all standard Kubernetes resources.  So in the past, it would hide some of the basic stuff for you; now it just shows everything.

DAWN: For people who use Kubernetes, this is definitely a good thing.  It's something of a personal bugbear of mine when cloud services abstract away things that they really shouldn't.  Particularly for people who are just getting started with this sort of service, it makes it much more difficult to follow what's actually going on.  So I'm all for this one.

ARJEN: Yep.  Kubeflow, erm, on AWS.  Ah, this is a project from AWS, ah, to- did you remember what this one was?

DAWN: We're not- [cough]

ARJEN: Sorry!

DAWN: We're not doing very well this month, are we?  Yeah.

AUDIENCE MEMBER: [inaudible]

ARJEN: Yeah, that would- thank you.  Yes.  It's a machine learning, erm, tool, to run your machine learning, ah, things more easily on Kubernetes.  And this is specialised- is focused, really, for running it on EKS, or just in the AWS cloud.

DAWN: Yeah.  Probably, in the vast majority of cases, if you are running a machine learning workload, there are almost certainly better ways to do it than on Kubernetes.  So, unless you're Google, probably not a tool that I would recommend that anyone try.

ARJEN: Erm, App Mesh supports IPV6.  As Kubernetes and VPCs and all that got, erm, IPV- better IPV6 support in the past, it's good that their, er, App Mesh, which is the- erm, well, service mesh service from AWS, now actually supports being able to call those endpoints.  Erm, it's kind of a hard thing otherwise to use this in combination.

DAWN: Yeah.  It remains to be seen whether the improved IP- IPV6 support in AWS is going to result in more of the internet moving towards it, but considering how long it's been a standard, I'm not holding my breath.

ARJEN: Yep.  On the Dev and Ops side, then: we've got CDK support for Service Catalog.  This is actually in two ways: you can call Service Cata- you can use Service Catalog products - items - within your CDK applications, and you can write your Service Catalog items in CDK instead of just pure CloudFormation.
