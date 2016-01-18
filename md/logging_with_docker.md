+++
date = 2016-01-18T12:07:58-05:00
draft = false
title = Logging with Docker
+++

As of late I've been trying to learn [Docker](https://www.docker.com). I've been using Ansible / Vagrant for quite some time now with great success. The only problem I have with the combination is that it takes up quite a bit of system resources to run each instance. The idea of running a single VM and individual containers for each service really speaks to me.

As such I've been trying to move one of our smaller services over to running in a container. This service is a standalone Java application that runs all of the time and manages sending push notifications to all users.

I've created a small Java app to play with for this post over on [GitHub](https://github.com/mrnickel/DockerLogLearning).

According to the holy rules passed down to us by [Heroku](https://www.heroku.com) in the form of writing [12 Factor Apps](http://12factor.net), [Thou must treat logs as event streams](http://12factor.net/logs). Essentially what this means is that all logs need to be out via STDOUT, (I also use STDERR). Nice and easy to deal with when developping the apps, and ultimiately I agree that the application shouldn't worry itself with where logs should be output. I also appreciate that, as a developer, it puts any errors or info messages directly in my face as I'm working on them.

As you can see in my [log4j2.xml](https://github.com/mrnickel/DockerLogLearning/blob/master/src/main/resources/log4j2.xml) config file, I do just that.

Building and running the docker container will reveal the wealth of data that I am generating to the world.

```
testproj 2016-01-17T17:19:18,680 INFO Timer-0 HelloWorld.run - Timer task started at:Sun Jan 17 17:19:18 UTC 2016
testproj 2016-01-17T17:19:18,678 INFO main HelloWorld.main - TimerTask started
testproj 2016-01-17T17:19:20,683 INFO Timer-0 HelloWorld.run - Timer task finished at:Sun Jan 17 17:19:20 UTC 2016
testproj 2016-01-17T17:19:20,684 ERROR Timer-0 HelloWorld.run - This is an error message...
java.lang.Exception: This is my exception, dawg
	at com.RyanNickel.TestDockerProj.App.run(App.java:16) [TestDockerProj.jar:?]
	at java.util.TimerThread.mainLoop(Timer.java:555) [?:1.8.0_66-internal]
	at java.util.TimerThread.run(Timer.java:505) [?:1.8.0_66-internal]
testproj 2016-01-17T17:19:28,675 INFO Timer-0 HelloWorld.run - Timer task started at:Sun Jan 17 17:19:28 UTC 2016
testproj 2016-01-17T17:19:28,683 INFO main HelloWorld.main - TimerTask cancelled
testproj 2016-01-17T17:19:30,677 INFO Timer-0 HelloWorld.run - Timer task finished at:Sun Jan 17 17:19:30 UTC 2016
testproj 2016-01-17T17:19:30,677 ERROR Timer-0 HelloWorld.run - This is an error message...
java.lang.Exception: This is my exception, dawg
	at com.RyanNickel.TestDockerProj.App.run(App.java:16) [TestDockerProj.jar:?]
	at java.util.TimerThread.mainLoop(Timer.java:555) [?:1.8.0_66-internal]
	at java.util.TimerThread.run(Timer.java:505) [?:1.8.0_66-internal]
```

As you can see I have 2 types of messages. INFO messages, and ERROR stack traces. Both crucial types of information that one would see in their own production apps.

Great place to start. Obviously I don't want to be looking at each container individually, especailly as our service scales and we are running multiple instances of this container. It's just not a very efficient way to track down issues. Enter log aggregators!

As far as I know, there are 3 main players in the log aggregation space:

1. [AWS Cloud Watch](https://aws.amazon.com/cloudwatch/)
2. [Loggly](https://www.loggly.com)
3. [papertrail]](https://papertrailapp.com)

It seemed to me as though the log drivers were the easiest way to go about this. Start the container with a log driver specified, and call it a day, right? They even offer an AWS CloudWatch driver. Awesome! Let's try to implement that shall we!

Now, I'm running a mac locally and prefer to have everything working on my own instances before popping them up to a more permanent location. So first step!

> configure the default logging driver by passing the --log-driver option to the Docker daemon:
> `docker daemon --log-driver=awslogs`

Wait... what? I can't do that on mac installation... the docker docs leave much to be desired here.

The easiest way that I've found to start playing with this logger is to create a new docker-machine with the appropriate log driver and enviornment variables set. Run the following command to create a new *awslogging* machine:

```
docker-machine create -d virtualbox \
 --engine-opt log-driver=awslogs \
 --engine-env AWS_ACCESS_KEY_ID=XXX \
 --engine-env AWS_SECRET_ACCESS_KEY=XXX \ 
 awslogging
```
Make sure that you enter your AWS creds in the above snippet.

Once the machine has been created you must use it via:

```
docker-machine start awslogging
eval $(docker-machine env awslogging)
```

Now we can build our container image

```
docker build -t test-docker-proj .
```

And finally we can run the container via:

```
docker run --log-driver=awslogs \
 --log-opt awslogs-region=us-west-2 \
 --log-opt awslogs-group=myLogGroup \
 --log-opt awslogs-stream=myContainerStream 
 test-docker-proj
```

Again, ensure that you change the aws-logs* log options to match your AWS setting.

Great! Now we're getting logs pushed up to CloudWatch Yay!

![AWS CloudWatch event stream](/images/aws_cloudwatch_event_stream.png)

So we're getting logs, the only problem is how they're stored. Each line of output is it's own Event. Well that's no good for our ERROR stack traces. That should be recongized as being a single event.

Now I know that it's possible using Amazon's official [log agent](https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py) to set some configuration options. The one I'm after is the *Timestamp format*. There don't seem to be any options that the docker logging driver offers to allow me to specify anything outside of the region, group and stream. Even taking a peek at the [source file](https://github.com/docker/docker/blob/master/daemon/logger/awslogs/cloudwatchlogs.go) led me to believe that there was nothing I could do.

So it looks as though the awslogs driver is a pretty basic implementation.

With the next blog post we'll look into popping logs up into Loggly and papertrail app to see if they offer anything a little more robust.