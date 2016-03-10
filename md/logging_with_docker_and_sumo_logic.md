---
date: 2016-02-18T16:55:07-05:00
draft: true
title: Logging with Docker and Sumo Logic
---

I was asked by Houston of [Sumo Logic](https://www.sumologic.com) to do a writeup about linking docker logging with their service.

I take a peek at their website and it looks like a pretty solid product. The dashboard looks comprehensive, robust and professional.

Again, my goal for these posts is to investigate logging aggregators with the twelve-factor app's idea of sending logs to STDOUT.

At first blanche, it looks as though their docker offering is pretty straight forward. Check out their [GitHub repo](https://github.com/SumoLogic/sumologic-collector-docker) for some basic information.

I'm following their [New Docker Logging Drivers](https://www.sumologic.com/2015/04/16/new-docker-logging-drivers/) post on how to set this up.

(Just so we're all on the same page, and no previous docker-machine modifications get in our way, I'll be working with a brand new clean machine titled **syslogbox**.)

```
docker-machine create -d virtualbox \
 --engine-opt log-driver=syslog \
 syslogbox
```

## Step 1: Enable **syslogd** on the docker-machine
It would seem logical that this would be enabled when specifying our log-driver during the machine creation process, but it isn't.

```
$ docker-machine ssh syslogbox
$ sudo -i
$ echo "syslogd -n &" >> /var/lib/boot2docker/profile
$ exit
$ docker-machine stop syslogbox
$ docker-machine start syslogbox
$ eval $(docker-machine env syslogbox)
```

Now if you want to confirm that your STDOUT messages are in fact going to syslog on your docker-machine:

In one terminal:
```
$ docker-machine ssh syslogbox
$ sudu -i
$ tail -f /var/log/messages
```

(Some blogs will tell you that the output is found at `/var/log/syslog`, this is not so as of **docker v 1.9.1** running in a virtualbox VM)

In another terminal:
```
$ docker run -d --log-driver=syslog ubuntu echo "Hello"
```

(You may need to re-run `eval $(docker-machine env syslogbox)` in your second terminal to ensure both terminals are communicating with the same machine.)

In the first terminal, you should see output similar to:
```
Mar  1 22:54:40 syslogbox daemon.info docker/9bbb8798f960[1373]: Hello
```

Where **9bbb8798f960** is the first 12 characters of the container ID.

Great! Syslog is now running!

## Step 2: Send syslog to sumo logic

It's time to send the syslog data up to Sumo Logic in order to actually work with it, as outlined in *Integrating the Sumo Logic Collector With the New Syslog Logging Driver*.

Following along with the post, I should be able to fire up the sumologic collector container, and everything should pop on up to my account:
```
$ docker run -v /var/log/messages:/syslog -d \
    --name="sumo-logic-collector5" \
    sumologic/collector:latest-logging-driver-syslog \
    [Access ID] [Access Key]
```

I'll now start up a container that will add very basic content to the syslog every second:
```
$ CID=$(docker run -d --log-driver=syslog ubuntu \
         /bin/bash -c 'while true; do echo "Hello"; sleep 1; done')
```

Okay, it's running, but how do we confirm that everything within the container is a-ok?
```
$ sudo docker exec -it $CID /bin/bash
$ sudo tail -f /syslog
```

(If you're seeing **Hello** echo'd out every second, then all is well!)

Now log into your Sumu Logic account and see the logs come in!

... wait... wait... wait... nothing... wait... still nothing... get a coffee (beer)... wait... drink coffee (beer)... wait... still nothing.

I tried to see if there was any indication that the Sumo Logic container was running things properly, but outside of a few lines in the syslog saying that it was running, and a new collection showing up, I had nothing.

No indication that anything was either wrong, or right.

When things were at their darkest and all seemed lost, the lovely folks of Sumo Logic reached out to me to see if they could offer any help. Turns out they could!

They pointed out an updated document in their official help section [Collecting Events and Statistics for Docker](https://service.us2.sumologic.com/help/Default.htm#Collecting_Logs_for_Docker.htm?Highlight=docker). This was considerably better and FAR more straight forward.

```
docker run -d -v /var/run/docker.sock:/var/run/docker.sock --name="sumologic-docker" sumologic/appcollector:latest [Access ID] [Access Key]
```

This is great. Simply mount the host machines docker.sock to the Sumo Logic container and go from there. When I checked my account, I had a beer in hand prepared for the prolonged wait. To my relief, what do you know?? My logs showed up!

## Conclusion

Don't worry about setting up syslog and all the trouble that comes with it. Simply mount the .sock file and go on with life. This seems to be how most logging containers are going. It makes sense to use a dedicated container to aggregate and push logs to the respective platform, as opposed to modifying each container, or even managing the logging mechanisms of the host container.

In my next post I'll write up my findings on using the Sumo Logic service.