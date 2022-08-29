# What's New in AWS - July 2022 - Transcript

ARJEN: Thanks Rob, and thanks everybody for coming!

ROB: Arjen, you're too tall!

ARJEN, DAWN, AUDIENCE: [laughs]

ARJEN: Thank you.  I'll just stand to the back - more to the front?

DAWN: [inaudible] No, that works.

ARJEN: Excellent.

DAWN: We're visible now.

ARJEN: Cool.  Then - let's go through the news!  What's been new since we last met?

Erm, this time we do have several new announcements of things that came to Sydney, erm, the first of which is `i4i` instances.  These are Intel-based instances, erm, based on having internal storage.  Erm, it's the newest generation, `i3`s were here before.  Basically, if you have `i3`s, update them.  If you don't use them, you're probably not that interested in this announcement.

DAWN: Unless you want to set up a new database.  In which case, this is one of your better options.

ARJEN: Yep.

DAWN: And - we've got, er, WorkSpaces Web has come to Sydney.  That is, er, sort of a - built on top of Amazon WorkSpaces, if you want to have WorkSpaces clients which are entirely accessible over the web, that is the tool that you are looking for.  So if you do do things based on Amazon WorkSpaces, you probably want to have a look at that.

ARJEN: Yep.  And the last one here is the new RDS multi-AZ option.  Erm, we've spoken about this a couple of times in the past, but basically, this is where instead of having a useless RDS instance that is only there, erm, in case something fails ov- in c- for failover, you have two reader instances that it will fail over to in different AZs.

DAWN: Very very happy that this one is actually in Sydney, so many of us consultants will probably get to play with it soon.  Which is nice.

ARJEN: Yep.  Cool!  Then - serverless!  Erm, SAM Accelerate is generally available, and I forgot what that one was.

DAWN: Erm, that is - mmm, I shouldn't mental blank on this!  It's, erm, built on top of the Serverless Application Model, and it allows you to do, essentially, erm, cloud-based testing of your functions locally, so you can mirror your local environment up to the cloud.  It - if I was doing a lot of serverless, it seems like the sort of thing that I would probably go and play around with.  So if you are doing a lot of serverless work, it supports all of the usual culprits; your Lambda, your Step Functions, I think API Gateway?  If- if you're doing a lot of serverless workloads,  I would be very very curious to- to have someone come and give a talk at some point about how well this works!

ARJEN: Yep.  And [clears throat] erm, then we've got Lambda Powertools for TypeScript.  Erm, a couple of months ago we had Michael Walmsley give a talk here about Lambda Powertools for Python.  Hopefully everybody, ah, was there for it, or otherwise you can watch it on YouTube.  Erm, basically he showed how useful it is - and that was just the Python version.  If you like to use TypeScript instead, that is now also a fully - ah, well, generally available - it was in beta for a long time I believe, now you can use it.

DAWN: Yeah.  The other two are both, sort of, security-ish or security-tangential, erm, so probably more in my domain.  Erm, the Lambda source function ARN is quite interesting, in the sense that now, whenever you run a Lambda, it will have a source function- source function ARN identifier attached to it.  So if you are doing policy-based scoping, it allows you to actually scope that down to not just traffic coming from a particular VPC, but tr- combinations of specific function, specific VPC, you can get quite - have I lost the microphone?  No.  You can get quite granular with that now, which is nice.  And the attribute-ba- based access control basically allows you to do - permissions for Lambda in bulk, based on attributes of users, roles, or groups, rather than having to manually set it up every time.  So, also always a good thing.

ARJEN: Yep.  Definitely good.  Not much on the container side, erm, you can run EKS Anywhere now on bare metal.  Erm, I could go on a long rant about that, but I would say don't.

DAWN: Yeah, I'll- I'll endorse don't.   Erm, if you are doing Kubernetes - if you do have one of the valid use cases for Kubernetes - and you're thinking you might like to try this, think very carefully before you do it would be my advice.

ARJEN: Yep.  So, moving on to more interesting stuff.  A lot in here, erm, on Dev and Ops.  My favorite is going to be
32:09
the CloudFormation event notifications with EventBridge so what has happened here is that you can now
32:16
CloudFormation on certain actions like stack changes will you can hook it up to EventBridge so
32:23
that EventBridge sends notifications and you can hook that up as you know if your fan bridge basically connects to every
32:30
aws service there is so you can do a lot of fun stuff with this
32:35
um from basic stuff like you have a stack that goes into delete
32:42
field you get a notification on your slack channels there's
32:48
so much to play around with here i've got many ideas i'm not going to
32:54
talk about them all but it's good
33:02
[Music] ah i my second favorite on here because i
33:08
think iron and i share a favor is being able to programmatically manage the contact information for your aws
33:14
accounts you can do that both for an individual aws account and from organizations
33:20
i love this because it's such a simple change which is likely to um a lot of
33:25
people don't even know that you can manage the contact information from within orgs
33:31
it's a simple change which means that if you ever get notifications from aws going something has really completely
33:37
blown up you can now make sure that the contacts for that are actually going to the right
33:42
place if you have a big org with lots of contact information and you've never looked at this now you can do it
33:48
programmatically it's well worth having a look at because it's a very small change that can potentially save you if
33:54
aws gets grumpy with you about anything yep um the other one i'd like to point out
34:01
here is just the cloud watch looks integration for visual studio code
34:07
it's nice if when you're able to just look at your logs in your editor instead of having to
34:13
go through and go through the cloud watch
34:19
in the cloud which logs interface although i'd always say just use [Music] cloudwatch logs
34:25
insights i think where you can properly search it which is way better
34:31
but i on time let's move to security
34:37
oh there's there's some interesting stuff here the network fireball supporting vpc prefix lists was
34:44
something that when i was looking at the announcements i sort of glazed over and then on the second look back i went and
34:50
had another look at it and thought actually this is really really powerful because it allows you to
34:57
scope everything down again with a lot of these security and security-ish
35:03
things that we've got on the slides here it allows you to scope everything down much more carefully
35:08
which is always a very good thing probably not all that many people are using network firewalls and would need this
35:16
but if you've got edge cases this is going to be quite good yep
35:22
my so something i enjoy here first um
35:27
ss aws sso as per this morning has been renamed to iam identity center
35:34
i propose that everybody forgets that part iam is in there because it doesn't make any sense to be
35:41
called i the identity access manager identity center so just call it entity center
35:48
basically treat it like session manager atm machine
35:54
um but bigger news there is it now finally supports customer managed
36:01
policies as well as
36:10
it supports customer managed policies uh it also supports permissions boundaries it's the other news that's the name yeah
36:16
i was looking for basically previously all you could do is attach
36:22
specific aws provided policies or have one big inline policy
36:27
and by specific they were very very limited being able to attach customer managed policies it's now much less
36:33
limited it gives you a lot more flexibility and it means that people are much more likely to actually be able to
36:40
go in and edit what they're doing rather than attaching the same inline policy to 10 or 20 different groups so the name
36:48
change we can take or leave but the customer managed policies and the permissions boundaries this is quite
36:53
good you can now manage all of that much more readily and shoot yourself in the foot less often yep
37:00
one major thing about this though
37:05
the way it works is that you have to provision those policies and permission boundaries to each account that you're
37:12
going to use it in yourself so it's not that you
37:18
define them once you have to deploy them to all your accounts
37:24
so um automate that yeah potentially a use case for stack sets
37:29
the firewall manager supporting the um additional configurations is something
37:35
that again probably not a lot of people are going to use but i'm vaguely aware of at least one case where particularly
37:42
with the strict rule order what used to happen was a my understanding is what used to happen
37:47
was aws would just evaluate this based on an aws predetermined order that
37:54
used to be a use case for a lot of third-party firewall applications it may well be that with this that's now no
38:00
longer the case and you can do some of those things that you used to have to offload to third-party firewalls through
38:06
firewall manager so that would be super interesting to see where that lands cool
38:12
and the last one i would want to discuss here is for those of us who don't have
38:18
absolutely everything in aws i am roles anywhere basically is a new
38:24
way to be able to assume roles within aws so
38:30
if you're on-prem or at first party services that needed the connection within aws
38:36
or work on other clouds that make a connection with aws
38:43
you always had to provide api keys that was not exactly seen as you know best practice security
38:51
wise because nobody ever ever rotates those keys
38:57
now you can set it up in such a way um there's a whole thing around it and
39:03
you have to use a private certificate manager but you can provision
39:09
temporary roles that can be assumed from your on-prem or otherwise provided
39:14
systems which is way more secure if you need this kind of access so
39:20
if you used if you have a situation like that definitely look into this it's a good blog post on how everything
39:27
works how you set it up it's a lot of it's a bit of work to set it up but after that it's definitely
39:33
worth it from a security standpoint yeah that's it's good if you're not in the walled
39:40
garden
39:45

ARJEN: Other cool stuff; erm, now that Apple has released M2 Macs, it's good that AWS now supports the M1s!

DAWN: [laughs]

ARJEN: Erm, I joke, but it is nice to see these finally released.  Erm, as well as `r6a` instances, which are the AMD version of the `r6` type, which is the memory-optimised instances.

DAWN: The VPC flow logs now supporting Transit Gateway is another one of those sort of security-ish announcements.  It's something that I expect, in environments that have those sorts of compliance or regulatory needs, that it probably is going to be something that people look at.  I believe that that actually used to be an argument against using Transit Gateway for things is you couldn't plug it, erm, you didn't have the VPC flow log support.  Now you do.  That's likely to significantly, erm, simplify a lot of people's networking, shall we say.

ARJEN: Yep.  And Redshift Serverless is out.  This is the last of the four 'not-so-serverless serverless' vers- er, services that were announced at re:Invent.

DAWN: [laughs]

ARJEN: Erm, yes it is- there's less to manage, it's not completely serverless, erm, still, if you use Redshift, check out- check it out and see if it suits your needs.  As always with these things, there is - different use cases where it will be useful.

DAWN: Yeah.  And the Well-Architected Tool integrating with AWS Organisations, that's - in some respects that's a cosmetic change, but it's the sort of cosmetic change that is going to make everyone's life much easier again.  And I always like seeing those announcements where AWS makes the changes that people have wanted, or that people have been asking for for ages.

ARJEN: Yep.

DAWN: And I think that's us!

ARJEN: Yes, it is!  We even lost the screen!

DAWN: [laughs]

ARJEN: I don't know how that happened.  That's not my background!

AUDIENCE: [applause]
