# Securing CDK bootstrap stacks - Transcript

ARJEN: It looks like the stream is actually working, so I-

DANIEL: Yeah.

ARJEN: [inaudible]

DAWN: So the stream is- the- the stream is working, but this is not?

ARJEN: Yes.

DANIEL: The screen is turned off.

DAWN: I can work with that.  Everyone happy to work with that?

AUDIENCE: [agreement]

DAWN: Cool!  [pause] Do you want to intro me, or am I just doing this?

ARJEN: Oh, I'll give you an intro.  [inaudible]

DAWN: [laughs]

ARJEN: Cool, this one looks better.  Many of you will already know Dawn, but I'll do an introduction anyway.  Dawn likes to tinker with cloud infrastructure and security, and regularly goes down rabbit holes in a futile search for ways to develop systems that are both reliable and impenetrable.  As well as accidental accessibility advocacy, Dawn can regularly be found sharing knowledge with the Melbourne cloud infrastructure and DevOps communities, such as this very one!  And outside work, Dawn is an occasional author, kitchen alchemist, and raging sportsball fan.

DAWN: And today, contrary to my usual presentation format, I am not doing this with slides.  I am also, you would notice, for those of you who have seen me present before, actually not wearing sportsball kit this time.  There are two reasons for that: the first one is this is a lightning talk, I have no slides; the second one is waistcoats are always an ideal sartorial choice.

Every system that we build is predicated on trade-offs.  And that is true also, probably, for a very large percentage of the systems that we build outside IT.  But in this case the systems that we care about are IT; AWS; specifically, as you would know from the talk topic, CDK.

I don't have the slides, so let me give you the very brief introduction to our friends at ProductCorp.  Many of you will already know who they are.  For those of you who don't, they are a startup headquartered in Sydney, they build e-commerce software.  And one of the primary people that we tend to hear about a lot is ProductCorp's Head of Platform and that is ah, a person who goes by the name of Ben Nguyen.  Ben Nguyen has recently re-architected a lot of the way that they have this all set up, and they have decided that for some of the work that they are doing, they are going to do it with CDK.

CDK, as Peter said earlier, is basically a way to do your infrastructure as code using standard programming languages - the types of things that your devs would use.  The advantage of CDK in this instance being that then, you can actually sit down with your devs and go 'hey, you are going to build your own infrastructure in the programming languages that you already understand'.  This is really powerful.  It also means that you do not have to hire a bunch of people to maintain your CloudFormation or Terraform, nor do you have to train a bunch of people in CloudFormation and Terraform.  You can build your resources in languages that people already understand.  It's the DevOps of platform teams? The platform team of DevOps? I don't know, I haven't really finished thinking that thought through.

One of the problems with CDK, or- one of the interesting rabbit holes that I went down when I started using CDK, is that- in order to actually get CDK to do pretty much anything of any significant complexity, you've got to build out a few things first.  And these things are all built out, generally on the CLI, using the `cdk bootstrap` command.  Typically, if you want to do this, you run the command, you will get back a bootstrap stack, and all of that is hidden from the view of everyone who uses the system.  But if we put ourselves for a moment in Ben's shoes, one of their primary concerns with this CDK bootstrap stack is actually understanding what it does.

And AWS will very kindly give you the whole template in CloudFormation so that you can edit it yourself.  This is what, if you're looking on the livestream, you'll now be able to see.  It's got a description, it's got some parameters.  Many of these, erm, th- they have actually done a decent job of naming them so that it's self-explanatory, they have good descriptions.  You can do all sorts of things with the these parameters.  You can actually basically do cross-account trust, which is set up, then, in the conditions block, which we can see at the top.  But then, in terms of what this actually creates, you get pretty much three different units of stuff.  So you get- a bucket which is used for assets - essentially the first two lines - and that gives you- that that bucket is basically 'alright, you want to build something with CDK and you have to upload some data to it, it goes there'.  You get a bucket where you can handle templates, and that's essentially the 'alright, you want to build a really big CDK template'.  When we convert it into CloudFormation we need to put that somewhere rather than fiddling around with things.  Here is an intermediary bucket.  This is where it goes.  CloudFormation can then pull the data out.

There are a few problems with this.  There are a few security holes.  There are a few rabbit holes which you can go down, and you'll not have a hell of a lot of fun going down them.  Because those buckets are, by default, pretty much open to everyone who has access to S3.  The other problem with this is that when you create a CDK bootstrap stack, it actually creates two separate IAM roles.  It creates the one that you assume into, and that role then goes away and assumes a CloudFormation execution role, which actually executes the code that you want to run.  This is great.  Anyone know what the name of the AWS managed policy that gives you full access to everything is?

AUDIENCE MEMBER: [inaudible]

DAWN: `AdministratorAccess`.  Anyone want to take a guess what the name of the managed policy that's attached to the CloudFormation execution role by default is?

ARJEN: I'm guessing it's not read-only.

DAWN: [laughs] Arjen is absolutely correct; it is not read-only.  It is, in fact, `AdministratorAccess`.  So this means that if you go in, and you deploy a CDK bootstrap stack, by default, anyone who has access to assume IAM roles in your AWS account can go in and deploy whatever they want using the CDK.  That might be a bit of a problem for several different reasons.

There are a couple of different ways that you can fix this problem; one of which I am going to demonstrate, one of which I am not going to.  The one that I'm not going to demonstrate, the one I'm just going to explain is- there are two different roles- basically, there are two different IAM roles which you can manipulate here.  You can change the policies on the CDK deployment role so that only specific people can assume it.  You might change the policy so that the only people who assume your CDK deployment role are the people who have administrator access on your account.  Another way of doing it, which I'm not going to demonstrate, is that you might actually set it up so that you have to use an OIDC provider to assume into that role.  Basically- the the workflow that that's essentially enforcing is, you can only deploy stuff with CDK from Github or Bitbucket or Gitlab or whatever your other CI/CD provider or third-party tool that you are using to do this is.

But that still leaves you with the problem that CDK- the CDK bootstrap stack at that point can do everything.  Fortunately, because AWS actually provides you with the template for the CDK bootstrap stack, you don't have to use the `cdk bootstrap` command on the CLI.  You can actually go in and do whatever you want to that bootstrap template.  So can everyone- because I can't see here, can everyone now see the `CloudFormationExecutionRoleDefaultPolicy` on the stream?

AUDIENCE: [inaudible]

DAWN: Awesome.  So what you can do with this, and erm- what you can do with this is you can pretty much set it up so you can just create an IAM policy which gets attached to the- which gets attached to the `CloudFormationExecutionRole` in the bootstrap template which will then allow you to limit the scope of actions that people can perform here.

In terms of limiting the scope of actions, this particular policy which is up on the screen at the moment is allowing organisations, EC2, ECR, but we don't really care about what it allows.  We care more so about, erm, if I actually drop all of these down- and if you're doing stuff with, erm, if you're doing stuff with SSM, do be careful to make sure that you don't give full permissions to everything in SSM, because that will stuff you up.  Pretty much all you need to do is you create a policy document, you set the statements that you want - you basically say these are the things I want you to be able to do - and then you attach that default policy to the `CloudFormationExecutionRole`.  If you do that, you have now scoped down what you can actually do with this.

Another thing that you'll notice in the bootstrap template is- right up here, on line 27, you've actually got a qualifier, and that allows you to differentiate different CDK bootstrap stacks from each other.  So if you really want to you can deploy 15 or 20 of these, all with different permissions, to be used in different scenarios by different teams.  And some of those may be scoped down using OIDC providers.

Just- in this case, don't fall into the trap of doing this the way that AWS does, and thinking that the CDK bootstrap stack is just a magic abstraction.  Because as Ben Nguyen would tell you in their time at ProductCorp, actually understanding how the systems work is critical to being able to produce something that doesn't give us problems like all of the data breaches that we've been hearing about in the news recently.  So, if there's one takeaway from this, please don't be Optus or Medibank!

AUDIENCE MEMBER: [inaudible]

DAWN: [laughs] Yeah.  There's a new one every minute!  [pause] Anyone have any questions?

AUDIENCE MEMBER: So do you see the end game for an organisation that's using this instead of something like Terraform is to create their own sort of domain/organisation specific sort of Terraform thing?

DAWN: I absolutely do not care which tool you pick.  I just care that if you decide that you are going to use CDK, you are aware that CDK does not use the permissions that attach to users.  I don't think that CDK itself inherently is a tool that makes it easier to do that; it's merely that CloudFormation and Terraform take the permissions of the user, or take- take the permissions of basically the IAM entity directly; CDK doesn't.  And if you want to use CDK, that's a really important thing to understand.

AUDIENCE MEMBER: Okay, so you don't have a lot of erm, I guess, knowledge about how organisations have used one or the other and whether it's been better or worse or - apart from the security implications you're talking about?

DAWN: I don't th- ah, the choice of tool; I actually gave a presentation about this at the AWS User Group, oh, three, four months ago?  Maybe closer to five?  Erm, the choice of tool; CloudFormation, Terraform, CDK; the TL;DR on that is, it should be entirely about how quickly you want to deploy things, how quickly you want them to actually deploy, what the skills are that you have in your teams, and how you want to divide up your teams.  Erm, the security implications shouldn't enter into it; this is just a fun little rabbit hole that I went down.

AUDIENCE MEMBER: Okay, thanks.

DAWN: Well, the security implications should enter into it in that you need to understand what they are.

ARJEN: Anyone else?  No?  Okay, thank you very much Dawn!

DAWN: And I will leave you to it!

AUDIENCE: [applauds]