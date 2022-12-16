# Securing CDK bootstrap stacks - Transcript

ARJEN: It looks like the stream is actually working, so I-

DANIEL: Yeah.

ARJEN: [inaudible] to sit down?

DAWN: So the stream is- the- the stream is working, but this is not?

ARJEN: Yes.

DANIEL: [inaudible] turned off.

DAWN: I can work with that.  Everyone happy to work with that?

AUDIENCE: [agreement]

DAWN: Cool!  [pause] Do you want to intro me, or am I just doing this?

ARJEN: Oh, I'll give you an intro.  [inaudible]

DAWN: [laughs]

ARJEN: Cool, this one looks better.  Many of you already know Dawn, but I'll do an introduction anyway.  Dawn likes to tinker with cloud infrastructure and security, and regularly goes down rabbitholes in a futile search for ways to develop systems that are both reliable and impenetrable.  As well as accidental accessibility advocacy, Dawn can regularly be found sharing knowledge with the Melbourne cloud infrastructure and DevOps communities, such as this very one!  And outside work, Dawn is an occasional author, kitchen alchemist, and raging sportsball fan.

DAWN: And today, contrary to my usual presentation format, I am not doing this with slides.  I am also, you would notice, for those of you who have seen me present before, actually not wearing sportsball kit this time.  There are two reasons for that: the first one is this is a lightning talk, I have no slides; the second one is waistcoats are always an ideal sartorial choice.

Every system that we build is predicated on trade-offs.  And that is true, also, probably, for a very large percentage of the systems that we build outside IT.  But in this case the systems that we care about are IT; AWS; specifically, as you would know from the talk topic, CDK.

I don't have the slides, so let me give you the very brief introduction to our friends at ProductCorp.  Many of you will already know who they are.  For those of you who don't; they are a startup headquartered in Sydney, they build e-commerce software.  And one of the primary people that we tend to hear about a lot is ProductCorp's Head of Platform and that is ah, a person who goes by the name of Ben Nguyen.  Ben Nguyen has recently re-architected a lot of the way that they have this all set up, and they have decided that for some of the work that they are doing, they are going to do it with CDK.

CDK, as Peter said earlier, is basically a way to do your infrastructure as code using standard programming languages - the types of things that your devs would use.  The advantage of CDK in this instance being that then, you can actually sit down with your devs and go 'hey, you are going to build your own infrastructure in the programming languages that you already understand'.  This is really powerful.  It also means that you do not have to hire a bunch of people to maintain your CloudFormation or Terraform, nor do you have to train a bunch of people in CloudFormation and Terraform.  You can build your resources in languages that people already understand.  It's the DevOps of platform teams? The platform team of DevOps? I don't know, I haven't really finished thinking that thought through.

One of the problems with CDK, or- one of the interesting rabbitholes that I went down when I started using CDK, is that- in order to actually get CDK to do pretty much anything of any significant complexity, you've got to build out a few things first.  And these things are all built out, generally on the CLI, using the CDK bootstrap command.  Typically, if you want to do this, you run the command, you will get back a bootstrap stack, and all of that is hidden from the view of everyone who uses the system.  But if we put ourselves for a moment in Ben's shoes, one of their primary concerns with this CDK bootstrap stack is actually understanding what it does.
