---
date: 2020-03-25T16:06:17-04:00
draft: false
title: On Getting AWS Solutions Architect Certified
---

Having accomplished my personal challenge of completing an olympic triathlon (while simultaneously rediscovering my love of cycling), I decided to tackle a professional challenge. 

So I went in search of something new. 

At the time, my team was  knee-deep in a lift and shift of our platform from MediaTemple to AWS. The process was going very smoothly and I found myself  intrigued by the number of overwhelming-yet-fascinating services they offer. 

An abundance of shiny new tools to  take our platform to the next level. I was hooked!  The search was over.  My professional challenge was to get my Solutions Architect Certification on AWS. 

## THE PREPARATION

### Online Course

Just like anything new, I had to do my research and gather learning resources. 

Amazon offered a number of white papers on the subject, but I found them to be too dry for my liking. I generally prefer to get my hands dirty and at least see some live examples. 

This led me to Pluralsight’s [AWS Certified Solutions Architect - Associate](https://www.pluralsight.com/courses/aws-certified-solutions-architect-associate) course by [Elias Khnaser](https://twitter.com/ekhnaser). Other than the disappointment of marginally dated content, it was an overall solid course that allowed me to wrap my head around the networking and IAM concepts to help me improve the overall security of our platform.

### Linux Academy

I also played around with [Linux Academy](https://linuxacademy.com/) as I was captivated  by the prospect of working through exercises on a live AWS account and having my solution verified. However, at the time the platform was quite new and the results were inconsistent and quite often incorrect.

I wish I could recall more details as to what my issues were but this was a long time ago and I'm certain the platform has gotten much better. I'd be curious to try it again when I decide to get another certification.

### Study Guide

The final piece of learning material I picked up was the official [AWS Certified Solutions Architect Study Guide](https://www.amazon.ca/Certified-Solutions-Architect-Study-Guide/dp/111950421X/ref=sr_1_1?keywords=solutions+architect+study+guide&qid=1580314684&sr=8-1). It should come as no surprise that this book covered everything that one would be tested on. It was  valuable at presenting the various concepts and topics and offered some  fundamental study materials.

### Practical Application

Now that I had the learning materials, I wanted to apply best practices and gain practical experience before I wrote the exam. 

Some of the goals I wanted to achieve with this were:

- Secure networking
- Secure IAM roles and security groups
- Autoscaling
- Reduce costs

As I don't have a background in networking, that was the part I struggled with the most (Well... that and IAM). At the beginning I was excited just to get things up and running so my security groups weren't exactly up to snuff. Open to too many services and external network attacks. RDS was the biggest culprit for this as I was used to opening up my SQL app and connecting directly to it in order to debug any issues. I had contemplated setting up a VPN but considering one of my goals was to reduce costs I settled on locking down requests to my own IP address. As our platform was quite stable this was acceptable. If I or my team were connecting regularly I would have set up the VPN.

Setting up proper networking was a hurdle for me. As the [Volu.me](https://Volu.me) back end grew organically into a monolith, networking was never a thing I had to think about. It's easy to build and deploy a single codebase to a single server when everything is synchronous and request based. However as soon as your platform needs to scale you need to start thinking about being more reactive and asynchronous. This means at the very least running on difference processes, which quickly leads to overburdening a single machine. In the short term this can be mitigated by increasing the size of your EC2 instance, but this is obviously not sustainable. Best practice dictates to use multiple smaller EC2 instances which means putting more thought into networking. My goal was to only have two services publically available: S3 and our ELB. Our system was essentially a monolith with a number of cron jobs running as services so networking was pretty straight forward. I could go into more technical detail about our solution, but that would be better left to another blog post.

My next hurdle was properly setting up IAM across our system. Again, because our system was essentially a monolith that was now distributed it gave me a good opportunity to ideally have services split up. I created the appropriate IAM accounts with the ideal microservice setup that we were aiming for and after some stressful afternoons we got everything running.


## THE DEEP BREATH 

Now that I had some practical experience setting up a non-trivial system on AWS following best practice I had one last hurdle to jump: My negative internal dialog constantly telling me that I wasn't ready.

I hadn't written any tests in 10+ years! How would I know if I've prepared enough? How do I know if I'm really ready? I spent far too much time procrastinating by convincing myself that I wasn't prepared. 

After months of putting it off I dug my heels in, logged into the portal and booked a date.  There was no turning back. I told myself that I wasn't going to reschedule no matter what. I then did what I always do just before tests and crammed the entire week prior. Ran through all of the practice exams, exercises and anything else I could find.

The night before the exam, I couldn’t focus on anything else.  My palms were sweaty, knees weak, arms were heavy… 

## THE EXAM 

The day of the exam came and that familiar fluttering feeling in my stomach returned. 

I went through the test and I realized that as per usual I had over-prepared. In retrospect If it wasn’t for my fear,  I could have written the test months earlier. 

To my relief, I passed the test and was very thankful that I got the results instantly.

## THE CONCLUSION

Although the [Volu.me](https://Volu.me) (my company) platform has since been taken down, I am still an avid user of AWS and always recommend it to clients. 

I am concerned about vendor lockin, and to some extent it is unavoidable, but it can be mitigated with some effort... but again that is another blog post for another day.

There it is; I tackled my professional challenge. I think my next personal (physical) challenge this year will either be learning to fly-fish, or run a full marathon Maybe both!