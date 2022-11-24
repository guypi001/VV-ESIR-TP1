# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
Questions 1: Général information about bugs article
#------------------------------------------------------------------------------------------
   On October 26, 1992, shortly after its implementation, the London Ambulance Service (LAS) Computer Aided Dispatch (CAD) system experienced a dramatic failure. 
This system, which was not previously automated, consisted of taking calls (recording the location on a form), identifying resources (those available and those required) and mobilizing resources. 
Following the automation of the DAC, it turned out that after a while the system was no longer able to :
	-cope with the load imposed on it by normal use;
	-Respond to emergency calls in a timely manner;
Also note that ambulance communications failed and some were lost in the system
#----------------------------------------------------------------------------------------------------------------------------------------------------------
Let us recall that the said system is composed of a software and hardware part of CAD, a software of geographical directory and cartography, a communication interface, a radio system. We can therefore without hesitation say that it is a global bug because it is the result of a bad interaction between the different parts that compose the system.
#----------------------------------------------------------------------------------------------------------------------------------------------------------- 
Among the flaws that made it possible to realize the presence of the bug, we can mention the ambulances that could not communicate, the emergency calls that took more than an hour before getting answers.
#------------------------------------------------------------------------------------------------------------------------------------------------------------
This bug has naturally had disastrous consequences, such as: Several deaths because the system did not react in time
#------------------------------------------------------------------------------------------------------------------------------------------------------------
I find that tests could have avoided this bug as the designers did not take into account this overload

Question 2: Appache server

The Apache Commons Collections package contains types that extend and augment the Java Collections Framework.
One of the bug that they resolve on the page is the the lack of a function to test if a hasher instance is empty.
Developpers add a function named isEmpty. This is a local bug because it is due to an omission of the developers
The Hasher interface of the bloomfilter package has no way of determining whether the hasher is empty. For the SimpleHasher implementation that is not a problem, however, the HasherCollection can be empty, and there is no guarantee that any other Hasher implementation can have an empty state. So to fix  this bug they just add a funtion to test if the hasher is empty

 -The developers have indeed added a new test to counteract the bug
 
Question 3: Chaos engineering
Après lecture du texte sur le chaos engineering
- Description des expériences concrètes réalisées
- Donnons les exigences pour ces expériences
- Quelles sont les variables observées
- Donnons les principaux résultats obtenus
- NETFLIX est elle la seule boite à faire du chaos engineering
- Spéculons: comment ces expériences pourraient être meniée dans d'autres organisations en terme de type d'expériences, et de variables observées

Question 4: WebAssembly
   -Expliquons les principaux avantages d'avoir une spécification formelle pour webAssembly 
   -Celà veut - il dire que les implémentations de webAssembly ne devraient pas être testées?

Question 5: 
-what are the main advantages of the mechanized specification
-Did it help improving the original formal specification of the language
