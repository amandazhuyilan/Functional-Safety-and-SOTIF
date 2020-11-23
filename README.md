# Software Functional Safety and SOTIF

This repo contains notes and related coursework based on Functional Safety and SOTIF for embedded software offered by Chris Hobbs.

## Content
- __[Introduction](#introduction)__
- __[Safety Standards](#safety-standards)__
- __[Bayesian Network](#bayesian-network)__
- __[Safety Cases](#safety-cases)__

## Introduction
- __Functional Safety__ - Something that functions to keep the system safe.
- __Safety of the Intended Safety (SOTIF)__ - Everything worked as intended but a dangerous situation still occurred. 
This is not covered in ISO26262, as ISO26262 defines "hazard" as "potential source of harm caused by malfuctioning behavior of an item."  

- __Risk__ - A combination of the probability of occurrence of harm and severity of that harm.

- __Unreasonable risk__: "risk judged to be unacceptable in a certain context according to valid societal moral concepts."

- __Safety__ - absence of unreasonable risk.

- __Real-time__ - A system is "real-time" if it must complete its execution within a certain time, it is a system that has a _deadline_. "Real-time" does not mean "fast", real-time systems are usually slower than non-real-time ones. 

- __Avaliability__ - does the system gives an answer?
- __Reliabilty__ - is the answer that the system gives correct?

  - A system where availability more important than reliability (search engine, self-healing systems like internet routing) or vice versa (machine in hospital for medicial treatment etc.).
  - When the reliability of the system increases, the reliability decreases and vice versa.

- __Fault__ - A passive error that sits there in the code.
- __Error__ - sometimes a fault causes a failure.
- __Failure__ - sometimes an error causes a failure. It is difficult to trace back to faults from a failure.

- __Heisenbug__ - Bugs that are reported as "unreproducible" and only happens occasionally.
   - Heisenbugs will exist in our software that we ship, testing does not detect Heisenbugs.
- __Bohrbug__ - Solid bugs that always happen with a particular input.

### Challenges of Writing Safety Critical Software
- We don't actually know how to do software development. We have no evidence that it costs more to fix a problem late in the development cycle.
- Verification by Testing. Currently, most systems have too many states for testing to cover more than a very few of them ("digital homeopathy").
   - We also don't know what to test for machine learning systems.
- __Unreliable hardware__ - modern processors come with pages of errata (error) that are not published but shared with some companies under NDS. Memory chips are likely to experience between 25,000 - 75,000 errors/bit flips per Mebibytes(2^20 bytes) per 10^9 hours (one bit flip per 1.5 hour).
   - _"There is no such thing as a deterministic software running on a modern processor."_
- Data - We don't know how to safely handle data.
- Machine Learning - we don't know what the machine learning systems are learning. For machine learning systems, requirements are impossible to define, there is no low level design, and the behavior of the final system is invisible. All of the about contradict most of the existing software development procedures.

### The three "E"s
- __Explicit Claims__ - Make claims clear for your software.
- __Evidence__ - Once we made our evidence clear, evidence must be provided.
- __Expertise__ - Expertise in:
   - software design and development,
   - the targeted area,
   - with safety standards and their applications. 


## Safety Standards

   "_Safety is demonstrated not by compliance with prescribed processes, but by accessing hazard, mitigating those hazards, and showing that the residual risk is acceptable._"

__What's a Standard?__ - A number of tables.  

### Some Safety Standards
- Functional Safety: ISO 26262 (on road vehicles), EN 5012x (railways)
- Medical devices: IEC 62304, ISO 14971
- Safety of Intended Functionality (SOTIF) - ISO 21448 + UL 4600
- Quality Management systems - ISO 9001 / ATF 16946 or ASPICE or CMMI.

![Alt text](figures/safety-standards.png?raw=true)


IEC 61508 is a foundation is a foundation for many market-specific standards, ISO 26262 for road vehicles and EN 5012x for railways.


#### The Process of Certification and Accrediation
1. Choose a certification body (TUV etc.) to ask for certification for a process and or a product.
2. As the accreditation authority (ANSI - USA, Japan - JAB, Germany - DAkkS) accredits only certain certification bodies, make sure that the selected certification body is accredited by the expected accreditation authority.

- __Safety Integrity Level (SIL)__ - How often can we tolerate failure of the safety function? It is divided into __Probability of Failure on Demand__ and __Probability of Failure per Hour__. SIL is used as acceptable levels determined by ALARP, GAMAB, MEM, Layers of Proection Analysis, Risk Graphs etc. 

   |  SIL  | Probability of Failure on Demand  | Probability of Failure Per Hour  |
   |-------|-----------------------------------|----------------------------------|
   |   1   |        < 10 <sup>-1</sup>         |        < 10 <sup>-5</sup>        |
   |   2   |        < 10 <sup>-2</sup>         |        < 10 <sup>-6</sup>        |
   |   3   |        < 10 <sup>-3</sup>         |        < 10 <sup>-7</sup>        |
   |   4   |        < 10 <sup>-4</sup>         |        < 10 <sup>-8</sup>        |


- __As Low As Reasonably Practical (ALARP)__
![Alt text](figures/ALARP.png?raw=true)

- __Minimum Endogenous Mortaliy (MEM)__
If you are alive on 1st January, what is the possibility you will not be alive on 31 December because of accidents (not diseases etc.) The value is usually 2 <sup>-4</sup>.

#### Problem with Standards
- __Expensive__ - ex: ~$2500 for ISO26262, with Single-Reader License. 
- __Contradictory__ - both internally (ex: in ISO26262, Safety case is a list of artifacts, claim, argument and evidence) and between different standards (ex: ISO 29119 [requirement-based testing not sufficient] contradicts the approaches to verification in ISO 26262 and IEC 61508 [requirement-based testing]).
- __Restrictive__ - ISO26262 only covers dangerous situations caused by malfunctions.
- __Incorporate obsolete ideas__ - it takes about 10+ years to develop a standard, but software development is rapid. Neither ISO 26262 or IEC 61508 accepts random software failure.
- __Driven by corporations__  - no more open standards of developments, replaced by "standardization by corporation" instead.
- __Fragmented__ - ex: no single standard covering vehicle-to-vehicle communication.

## Bayesian Network 
### Bayes Theorem
![Alt text](figures/bayesian-theorem.png?raw=true)
- Allow us to work from effect to cause - X happened, what might have caused it?
- Instead of talking about the probability of something happening, it talks about the strength of your belief in the event to be occured.
## How to use a Bayesian Network?
- For the arguments in a __Safety Case__(put together to explain why the system is sufficiently safe).
   - "I claim that X and here are my arguments".
- For the interdepencies in a Fault Tree; Machine Learning (learning from data; anomaly detection (learning from human experts); project planning etc.
- Tools: (commercial) [Hugin Expert](https://www.hugin.com/).

## Safety Cases

A Safety Case presents your argument as to why your system is sufficiently safe. The Safety Case must be prepared throughout the development.

__[Definition by Chris Hobbs]__
Also known as "Safey Assurance Case", consists of four elements:
- Boundary of the system
   - _My system includes ... and excludes ..._
   - Is human operator included and trained? Is software updated included?
- Claim of the system
   - _I claim that my system, when used as described in the Safety Manual ..._
   - Determining what to claim is difficult.
- The Argument
   - _I demonstrate that I meet my claims as follows ..._
   - Build an argument without making collecting evidence for it
      - Pitfall: Prone to "p-hacking" - hacking the _p_ value of your system without previous research, a practise where researchers select the analysis that gives a pleasing result. 
- The Evidence
   - _The evidence to back up my argument are as follows ..._

__[Definition by ISO26262-10]__
   _The purpose of a safety case is to provide a clear, comprehensive and defensible argument, supported by evidence, that an item is free from unreasonable risk when operated in an intended context._

There are three principal elements of a safety case, namely:
- __safety goals__ and related safety requirements (safety objectives of the item or element)
-__safety argument__
- the ISO 26262 series of standards work product (the __evidence__).

[Definition by UL 4600 (draft)]
The safety case shall be a structured explanation in the form of __goals__, supported by __arguments__ and __evidence__, that justifies that the item is acceptably safe within a defined operational design domain, and it covers the item's lifecycle.

- The Safety Case shall used a defined, __consistent__ format for claims, arguments and evidence.
- The evidence used shall conform to __defined__, __auditable__ formats.
- The claims and arguments in the safety case shall be __clear__ and __consistent__.

__[Table of Content for Safety Case by EN 50129]__
- Part 1: Definition of a System (or sub-system / equipment)
- Part 2: Quality management report
- Part 3: Safety management report
- Part 4: Technical safety report
- Part 5: Related safety cases
- Part 6: Conclusion

   _This shall summarize the evidence presented in the previous parts of the Safety Case, and argue that the relevant system/sub-system/equipment is adequately safe, subject to compliance with the specified application conditions.

### A Strategy
1. Define the boundary of the system.
2. Define the top-level claim.
3. Build an argument without collecting evidence.
4. Ask the auditor and other stakeholders: "if I gave you the evidence for this argument, would you be convinced?"
5. Repeat from step 3 until everyone is satisfied.
6. Collect the evidence.

### Confirmation Bias
We tend to only look got evidence that confirms what we already believe. Confirmation Bias makes it difficult for humans to create honest Safety Case.

   _"The general root of superstition is that men observe when things hit, and not when they miss, and commit to memory the one, and pass over the other."_ -- Francis Bacon

When a safety engineer is told to create a Safety Case to demonstrate that the system is safe, he will look for evidence that the system is _safe_. 

To use Confirmation Bias positively, tell the engineer to list all the doubts he has about the system's safety, then try to eliminate those doubts.

### Notations for Safety Case Arguments
1. __Goal Structuring Notation__
![Alt text](figures/GSN-example.png?raw=true)

Limitations:
- Not quantified - ex: the strength of the evidence, cannot say that one sub-argument is more important than the other one.
- Cannot express doubt (See [_Eliminative Induction: A Basis for Arguing System Confidence_](https://resources.sei.cmu.edu/asset_files/ConferencePaper/2013_021_001_88010.pdf)).
   - __Rebutting__ - claim or prove that (evidence or an accusation) is false.
      - "_That claim is wrong, I can find a counter-example."_ 
   - __Undermining__ - erode the base or foundation of something. 
      - _"The evidence does not convince me."_
   - __Undercutting__ - to damage something or to make it fail.
      - _"The evidence is fine, but it does not support the claim."_


2. __Bayesian Network___
   A common pattern, _the strength of belief_.
3. __Structured Assurance Case Metamodel (SACM)__ 
An attempt by the Object Management Group to put together a set of classes of things for putting a safety case together.

![Alt text](figures/SACM.png?raw=true)
("Always incredibly dull." - Chris H.)

SACM standardlizes the Safety Case notation and provides interoperability. Advantages:
- Portability - export the safety case in XMI or XML for other SACM tools to import.
- Definition of terms - SACM forces users to define terms, "what is sufficiently safe?"
- Patterns/Templates - make the Safety Case more dummy-friendly (readable to inexperienced users)
- Verification of Safety Cases - syntactically and semantically.
