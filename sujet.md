# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
________________________________________________________________________________________________________________________________________________________

1. #--------------------Answer 1---------------------------------------------------#
For this first question we have choosen to describe the London Ambulance System (LAS) bug. In fact the LAS  is the largest ambulance service worldwide responding to between 2000 and 2500 calls per day with a fleet of 750 vehicles under its disposal. The service covers the greater London area (600 square miles) with the cooperation of the London Fire and Civil Defence Authority and the Metropolitan Police. It compromises of the Accident and Emergency Service
(A&E) and Patient Transport Service (PTS).

On the morning of Monday 26th October 1992 the LAS CAD system went live for the first time. Unfortunately there were 81 known bugs in the system at that time and it had been 10 months since the control room staff were first trained to use the software. The system had 4 primary flaws when it went live; it did not function well when it was given incomplete data regarding the status of the ambulances and the system did not accept errors made that occurred in normal day-to-day use of a computer system. Furthermore, the user interface had black spots which meant that the user could not see all the information on screen and finally, the most important failure, the system stored incident information even after it was not needed, which caused the system to fill up memory and fail.

Many factors were responsible for the unfortunate failure of the LAS CAD System as discussed below:

	-Bad data checks
	-Memory leaks
	-Graphical User Interface issues
	-Bad Hardware reuse
	-...
In view of all these elements, we can conclude that this was **a global bug.**

The first of these problems began to show during the morning rush of calls when it became apparent to all involved in the use of the system that things were about to go wrong. There were a number of duplicate calls being made as the system was losing accepted emergency calls, which lead to a number of distraught callers
being kept waiting in the call-queuing system for up to 30 minutes. The system created further delays when dispatching ambulances. It failed to recognise certain roads and routes, which meant the drivers had to revert back to using maps to navigate their way or call the ambulance dispatch. These system errors led to the late
arrival of ambulances, or two ambulances turning up at the same time, or worse not at all. By Monday evening, the number of system failures had increased due to the number of new emergency calls which began to overwrite old ones that were not dealt with. There were further delays in the system created by exception

this bug has naturally had various consequences such as:
		
	-In humans terms: Claims were made in the press that up to 20-30 people may have died as a result of ambulances arriving to 
	 late on the scene.
	-In economic terms: The software is estimated to have cost between £1.1 and £1.5
	
Although there was functional testing of various components, integration testing to ensure that the system can operate together was not carried out. Thus, there was no attempt to test how the system would react to different circumstances such as high call rate, multiple incident reporting, vehicle location problems, falling back to backup servers, etc. It seems clear that if the tests had been done correctly they would have avoided this disaster

_________________________________________________________________________________________________________________________________________________________

2. #--------------------Answer 2---------------------------------------------------#

One of the bug that they resolve on the page and we have choossen to talk about  is the lack of a function to test if a hasher instance is empty.In fact this function is really important because some operations are supposed to be done on empty hasher so if we can't test this criteria it's a problem. 


This is #a local bug#  because it is due to an omission of the developers.

The Hasher interface of the bloomfilter package has no way of determining whether the hasher is empty. For the SimpleHasher implementation that is not a problem, however, the HasherCollection can be empty, and there is no guarantee that any other Hasher implementation can have an empty state. So to fix  this bug they just add a funtion to test if the hasher is empty

The developers have indeed added a new test to counteract the bug
 

3. Après lecture du texte sur le chaos engineering

Description des expériences concrètes réalisées. Netflix has been running an internal service called Chaos Monkey which randomly selects virtual machine instances that host our production services and terminates them

Chaos Monkey is only active during normal working hours so that engineers can respond quickly if a service fails due to an instance termination.

Quelles sont les variables observées
	La qualité du service rendu, la resillence des serveurs aux pannes
Donnons les principaux résultats obtenus

NETFLIX est elle la seule boite à faire du chaos engineering

Pas du tout Amazon, Facebook le font tous
For example, we perform "Chaos Kong" exercises that simulate the failure of an entire Amazon EC2
region, and we also run Failure Injection Testing (FIT) exercises where we cause requests
between Netflix services to fail and verify that the system degrades gracefully.

- Spéculons: comment ces expériences pourraient être meniée dans d'autres organisations en terme de type d'expériences, et de variables observées

Question 4: WebAssembly

WebAssembly addresses the problem of safe, fast, portable low-level code on the Web.

Safe, fast, and portable semantics:
safe to execute
fast to execute
language-, hardware-, and platform-independent
deterministic and easy to reason about
simple interoperability with the Web platform

--------------------------------------

Safe and efficient representation:
compact and easy to decode
easy to validate and compile
easy to generate for producers
streamable and parallelizable
Why are these goals important? Why are

-------------------------------------

This language has a lot of quality, but you can never be sure of a developer's mistake. So it would be wise to have tests

Question 5: 
-what are the main advantages of the mechanized specification


-Did it help improving the original formal specification of the language
