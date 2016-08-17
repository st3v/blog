---
authors:
- stev
categories:
- Cloud Foundry
- Microservices
- Golang
- Spring
date: 2016-08-16T14:12:43+01:00
draft: true
short: |
  This is the first in a series of blog posts covering the development of an application that consists of multiple Go microservices running on Cloud Foundry.
title: Running Go microservices on Cloud Foundry - Part 1
---

## Problem Statement
* Cloud Foundry platform helps to deploy and run microservices at scale
* Spring Boot offers a great toolkit for microservice authors using Java
* Big parts of CF are written in Go and we love the language for its simplicity, etc
* So we asked ourself the question, could we achieve a similar developer experience for Go developers creating microservice-based applications and running them on Cloud Foundry

## Spring Boot & Spring Cloud
* Take a step back. What does Spring Boot give us:
	* Service Connectors
	* Service Registry
	* Circuit Breakers
	* Configuration Server
	* Request Tracing

## Connectors
* Lets start with service connectors. Reading JSON from the environment and parsing it is pretty straight-forward in Go. However, it involves quite a bit of boiler-plate code that is pretty much the same for all apps. Let's try to pull this out into a package
	* env.Service(env.WithTag("tag"), env.WithLabel("label"), ...)
	* env.MustHaveService(....)
* That's pretty good but we would still have to know about the specifics of certain service bindings and type-cast credential fields. So, lets go a step further and introduce what we call Service Adapters:
	* srv.OpenDB
	* srv.ConnectToDB
	* srv.MustConnectToDB

## Example

## Outlook



## Introduction

There have been a number of previous blog posts and presentations showing how Cloud Foundry helps developers create and operate microservice-based applications at scale [[1](https://blog.pivotal.io/pivotal-cloud-foundry/features/deploying-microservice-architectures-with-pivotal-cloud-foundry), ...]. Out-of-the-box, the platform offers easy deployment for a variety of programming languages, on-demand scaling, routing, load balancing, failover and resiliency, centralized logging, and on-demand provision of data services. While these platform features cover some of the fundamental concerns of cloud native applications, there are other requirements more specific to microservice architectures that must be addressed at the application level. This includes things like service discovery, circuit breaking, centralized configuration management, and distributed tracing. 

There are multiple frameworks and toolkits that attempt to free developers from having to implement these rather common microservice patterns by themselves. Instead of writing lots of boiler-plate code that is easy to get wrong, developers can focus on the actual business problem they are trying to solve. One such framework is Spring Cloud [2]. It gives Java developers access to a set of tools and annotations they can use to enable many of the most common architectural patterns in distributed systems for their own services. While Spring Cloud is by no means specific to Cloud Foundry, it works very well on top of the platform. In combination with Spring Cloud Services [3] it provides Java developers that use Cloud Foundry with what is argubaly one of the best experiences in the industry when it comes to writing microservice applications and running them in the cloud. It enables developers to implement and deploy microservices at a rapid pace while at the same time making sure that these services are maintainable and easy to operate.

Now even though Cloud Foundry works very well with Java microservices using the Spring ecosystem, the platform itself has always been supportive of polyglot development by offering buildpacks for a variety of languages [6]. With that in mind it is somewhat obvious to wonder what the Cloud Foundry developer experience would look like for microservice-based applications written in languages other than Java. In particular Go [5] is of interest here, not only because it is the language most of Cloud Foundry itself is written in but also because it has seen wide adoption in the general cloud infrastructure and microservices space over the last couple of years [7, 8]. With that in mind, we set out to develop a microservice reference application in Go and run it on Cloud Foundry. As part of this journey we used Spring as a reference point to identify gaps in the developer experience when it comes to creating and running Go microservices on top of Cloud Foundry. We then came up with a set of Go packages that attempt to close the gaps we found. This blog post will be the first in a series that is going to document what we have learned and done throughout our journey.

## Spring and Cloud Foundry 

Before we start talking more about microservice development in Go let's take brief look at what Spring offers to Cloud Foundry application developers. After all, this is the framework that we are going to use as our reference point when looking at the experience for Go developers. As mentioned before, there are two components of the Spring ecosystem that are of particular interest to us: Spring Cloud and Spring Cloud Services.

### Spring Cloud
* Spring Cloud for Cloud Foundry
	* Discovery Client	 
* Spring Cloud Connectors

### Spring Cloud Services

## Links and References
* [1] [Deploying Microservice Architectures with Pivotal Cloud Foundry](https://blog.pivotal.io/pivotal-cloud-foundry/features/deploying-microservice-architectures-with-pivotal-cloud-foundry)
* [2] [Spring Cloud](http://cloud.spring.io)
* [3] [Spring Cloud Services](http://docs.pivotal.io/spring-cloud-services/)
* [4] [Spring Boot](http://projects.spring.io/spring-boot/)
* [5] [The Go Programming Language](https://golang.org/)
* [6] [Coud Foundry Buildpacks](https://docs.cloudfoundry.org/buildpacks/)
* [7] [Go: the emerging language of cloud infrastructure](http://redmonk.com/dberkholz/2014/03/18/go-the-emerging-language-of-cloud-infrastructure/)
* [8] [Google Go: Why Google’s programming language can rival Java in the enterprise](http://www.techworld.com/apps/why-googles-go-programming-language-could-rival-java-in-enterprise-3626140/)




Cloud Foundry is a pretty great platform to run distributed applications consisting of a large number of microservices. The platform enables developers to spend most of their time to work on actual business problems instead of having to deal with rather generic issues such a service deployment and scaling, monitoring and centralized logging, or security and multi-tenancy. Originally written in Ruby, big parts of Cloud Foundry have since been moved to Go. With that in mind it is a somewhat obvious to wonder how easy it would be to develop a microservices application in Go and run it on Cloud Foundry. The platform offers buildpacks for a variety of languages, amongst others Go. Thus, writing a simple application in Go and deploying it onto Cloud Foundry is pretty straight-forward [link]. But what about a more complex application that employs a set of distributed microservices? Such application usually pose more complicated problems such as service resiliency, availability, discoverability ... In this blog series we want to dive into this question and attempt to build out a sample application that runs as a set of distinct microservices on Cloud Foundry.

At Pivotal we love Go. Many parts of Cloud Foundry the Platform-as-a-Service offering we work on are written in Go.





## Problem Statement

Posts are written in [Markdown](https://help.github.com/articles/github-flavored-markdown/) -- tips below:

## Writing a _Good_ Post

_copied from [the README](https://github.com/pivotal/blog#writing-a-good-post)..._

### Keep it technical.

People want to to be educated and enlightened.  Our audience are engineers, so the way to reach them is through code.  The more code samples, the better.

### Nobody likes a wall of text.

{{< responsive-figure src="/images/pairing.jpg" class="right small" >}}

Use headers to break up your text.  Each image you add to your post increases its XP by 100.  Diagrams, screen shots, or humorous "meme" (_|memā|_) gifs...  They all add color.  If you don't have [OmniGraffle](https://www.omnigroup.com/omnigraffle), then submit an ask ticket.  There's no excuse for monotony.

### Your 10th grade teacher was right.

Make use of the hamburger technique.  Your audience doesn't have a lot of time.  Tell them what you're going to write, write it, and then tell them what you've written.  Spend time on your opening.  Make it click.

### Pair all the time.

We do everything as a team, and this is no different.  Get feedback from your friends and coworkers.  Show them the post on the staging site, and ask them to tear it apart.

### Make it pretty.

Pivotal-ui comes with a bunch of nice helpers.  Make use of them.  Check out the example styles below:

---

# Header 1 

Don't use this, since it looks like a main title.

## Header 2

### Header 3

#### Header 4

##### Header 5

###### Header 6

{{< responsive-figure src="/images/pairing.jpg" class="left" >}}

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

~~~ruby
instance = Class.new("foo")
~~~

| Header 1        | Header 2  | ...        |
| --------------  | --------- | ---------: |
| SSH (22)        | TCP (6)   | 22         |
| HTTP (80)       | TCP (6)   | 80         |
| HTTPS (443)     | TCP (6)   | 443        |
| Custom TCP Rule | TCP (6)   | 2222       |
| Custom TCP Rule | TCP (6)   | 6868       |

{{< responsive-figure src="/images/pairing.jpg" >}}

