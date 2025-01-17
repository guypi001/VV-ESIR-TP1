# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.
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
2.
The Apache Commons Collections package contains types that extend and augment the Java Collections Framework.It use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. Here we chose to explain one of them.
The bug was that The CollectionUtils.removeAll(Collection<E> collection, Collection<?> remove) does not throw a NullPointerException(NPE) when the “remove” parameters is null, but only if the “collection” parameter is empty. In the documentation it is stated that an NPE will be thrown if any of the parameters is null while this is not always the case.

To solve the problem, they add null check in ListUtil.removeAll() as shown below:


	Objects.requireNonNull(collection, "collection");
	Objects.requireNonNull(remove, "remove");


The developers have indeed added  new test as you can also see:

	assertThrows(NullPointerException.class, () -> ListUtils.removeAll(null, new ArrayList<Object>()),
                "expecting NullPointerException");

        assertThrows(NullPointerException.class, () -> ListUtils.removeAll(new ArrayList<Object>(), null),
                "expecting NullPointerException");
 
_________________________________________________________________________________________________________________________________________________________
3.
Chaos Engineering is the discipline of experimenting on a distributed system in order to build confidence in its capability to withstand turbulent conditions in production.Netflix is one of the company famous for the popularization of chaos engineering. For years, Netflix has been running an internal service called Chaos Monkey which randomly selects virtual machine instances that host our production services and terminates them. Chaos Monkey's purpose was to encourage Netflix engineers to design software services that can withstand failures of individual instances. The success of this practice encouraged Netflix to extend the approach of injecting failures into the production system in order to improve reliability. For
example, they perform **"Chaos Kong"** exercises that simulate the failure of an entire Amazon EC2 region, and they also run Failure Injection Testing (FIT) exercises where they cause requests between Netflix services to fail and verify that the system degrades gracefully.
Concretely, all these experiences are based on four principles:

		-Build a hypothesis around steady state behavior
		-Vary realworld
		-events
		-Run experiments in production
		-Automate experiments to run continuously

The main requirement is that Chaos engineering is only active during normal working hours so that engineers can respond quickly if a service fails due to an instance termination.

At Netflix, they are using Chaos Engineering to ensure that the system still works properly under a variety of different conditions. For this, there are certain variables that engineers observe. We can mention SPS (stream starts Per Second) which show how many users start streaming a video each second or new account signups per second.

The SPS metric varies slowly and predictably over the course of a day, as shown in this Figure 

![image](https://user-images.githubusercontent.com/107374001/205501116-0509bee0-1e74-4f05-98b4-1b87fd00b87a.png)

Engineers at Netflix spend so much time looking at SPS that they have developed an intuition about whether a given fluctuation is within the standard range of variation or whether it is cause for concern. If engineers observe an unexpected change in SPS, we know there is a problem with the system.

Netflix is not the only company to apply chaos engineering. Organizations such as Amazon, Google, Microsoft, and Facebook apply similar techniques to test the resilience of their own systems.

For companies selling their services Chaos Engineering is the ideal technique to test the resilience of their system. for example an e commerce
site might use a metric such as number of completed purchases per second.

_________________________________________________________________________________________________________________________________________________________
4.
WebAssembly (Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications. 

WebAssembly addresses the problem of safe, fast, portable low-level code on the Web. Previous attempts at solving it, from ActiveX to Native Client to asm.js, have fallen short of properties that a low-level compilation target should have:

	.Safe, fast, and portable semantics:
		-safe to execute
		-fast to execute
		-language-, hardware-, and platform-independent
		-deterministic and easy to reason about
		-simple interoperability with the Web platform

	.Safe and efficient representation:
		-compact and easy to decode
		-easy to validate and compile
		-easy to generate for producers
		-streamable and parallelizable

We find that many classic vulnerabilities which, due to common mitigations, are no longer exploitable in native binaries, are completely exposed in WebAssembly but you can never be sure of a developer's mistake. So it would be wise to have tests.

_________________________________________________________________________________________________________________________________________________________
5.
The main advantage is that the core of the mechanisation is our definition of two inductive relations, which correspond to the WebAssembly specification’s reduction and typing rules. These relations are not directly executable, but we define separate executable functions for an interpreter and type checker, and prove them correct with respect to their corresponding relation.

Extending our mechanisation to model this feature revealed a deficiency in the WebAssembly specification that sabotaged the soundness of the type system.so this specification helped to improve it

We have also defined a separate verified executable interpreter and type checker. Like many verified language implementations, these artefacts require integration with an external parser and linker to run as standalone programs, which introduces an untrusted interface
 

we use to make our verified interpreter executable as a standalone program, since its internal state is more heavily based on the draft specification. In particular, the paper formalisation stores functions declared within WebAssembly programs within each instance itself. This is possible because the formalisation gives only a sketch description of the instantiation process. In the full draft specification, multiple instances may share the same store, and one may export a function that is imported by another. Instances cannot directly access each other, and therefore a single copy of the function can be held directly in the store, with each instance maintaining a reference to it.

This new specification doesn't removes the need of testing because we have conducted differential testing of our executable interpreter against several major Web- Assembly engines. This was done both with the purpose of validating our interpreter, and potentially discovering semantic bugs in commercial WebAssembly engines. Tests were generated using the CSmith tool [Yang et al. 2011], combined with the official Binaryen toolchain [WebAssembly Community Group 2017a] to convert the generated C tests intoWeb- Assembly. This mimics how most WebAssembly programs will be produced “in the wild". No errors were found either in our implementation or any commercial engine, although a crash bug was discovered within the Binaryen toolchain itself, which was reported and fixed by the developer.
