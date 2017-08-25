---
layout: single 
title: "Scalar Conference 2016 Summary"
date: 2016-04-23 22:31:00
categories: scala
---

Last week I attended third edition of Scalar conference in Warsaw.
It was my second time in Warsaw and I think organizers did great job again.
Conference moved to new, shiny venue in Museum of history of polish jews.
Museum was selected as european museum of the year 2016 and was really great choice.

Day started with Pawel Szulc introduction to type classes in 4 acts.
Pawel walk us through story of your average Joe the programmer, that discovers
type class pattern on his own after terrible crash of Stack Overflow.
Talk was really well prepared, fun and interesting.
Afterwards Mathias Doenitz took the stage and introduced his new implementation
of reactive streams, that focuses on simplicity and usage on single machine.
Mathias showed us couple of examples of typical use cases of streams like
creating, faning in, faning out and other transformation.
Library was open-sourced on day of conference and currently supports only synchronous
stream, but Mathias said he would like to release version 1.0 during Scala Days
in Berlin.

After break, Eric Torreborre talked about new way of composing monads by using
Eff monad. Eric pointed out some common problems with monad transformers and
than procedeed. Honestly I didn't get all details of Eff monad, but it's definitely
something I would like to investigate in future.
Before lunch break, we also learned about intricases of incremental compilation
from Krzysztof Romanowski, which brought some old memories of compiler university course.
No conference is complete without at least one talk about containers and Scalar
was not exception. Maciej Bilas showed us how to deploy and discover nodes
in Mesos/Kubernetes cluster using Akka ETCD.

After lunch, we got back to web development in scala using Lift framework.
Lukasz Lenart declared that he's better coder than speaker and continued with
30 minutes live coding Lift application session. Demo gods weren't on our side
at first, but everything ended pretty well.
Than Valentin Kasas tried to show us that Shapeless is not that difficult
and that you should get use to type being longer than data if you use it. :)
This session concluded with my favorite talk of whole conference by Amira Lakhal.
Amira talked about her project combining Internet of Things, time series and
predictions using Spark, Cassandra and Android. Amira used accelerometer on her phone
to record her movements over all three axes, stored it in Cassandra and than used
Spark and MLlib to predict if she is standing, walking, jogging or running based
on previously recorded and labeled data. And there was also some running on stage. :)

In last session, Dmytro Petrashko, one of members of Dotty compiler showed us
than initialization order in Scala and how they might change some current problems
in Dotty. Than we stepped out of low-level world of compilers to high-level
of functional libraries Cats and Scalaz. Jan Pustelnik demonstrated usefulness
of these in some simple examples and that all they do is provided "design patterns"
for functional programming.
Marco Borst and Slava Schmidt from Zalando talked about how we can use session types
to write typesafe contract of RESTful services. Last talk of conference case study
of Scala, Elastic Search and Neo4j to scale massive social network application
with 800 milion of nodes. Artur Bankowski went through whole process of selecting
these technologies to implement prototype, performance improvements and data migration.

Overall, I enjoyed conference a lot and I am looking forward to next edition.
Thanks to all organizers, speakers and sponsors who made this Saturday great
day of learning about Scala ecosystem.
