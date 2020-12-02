# Software Functional Safety and SOTIF

This repo contains notes and related coursework based on Functional Safety and SOTIF for embedded software offered by Chris Hobbs.

## Content
- __[Introduction](#introduction)__
- __[Safety Standards](#safety-standards)__
- __[Bayesian Network](#bayesian-network)__
- __[Safety Cases](#safety-cases)__
- __[Risk Analysis](#risk-analysis)__
- __[Discrete-Event Simulation](#discrete-event-simulation)__
- __[Programming Languages and Compilers](#programming-languages-and-compilers)__

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

##

## Discrete Event Simulation

Old, semi-formal technique to produce statistical values for a deterministic problem - one that has an exact answer. 
- It can be used to produce statistical values for a probabilistic problem.
- It can be used to estimate system throughput, resource usage, availability, behaviour under error conditions and other factors.
- It is recommended in IEC 61508 and ISO 26262 and considered essential in UL 4600.
- It can be a source of system test cases.

### Why model?
- To verify an algorithm or protocol 
   - ex: Will there be race conditions? Will this algorithm always produce the same answer? Can this system handle 2000 transaction per second?
- To design a system
   - how many buffers do I need to be 99% confident that there will be no buffer overflows?
- To generate code and test cases

### Why Simulate?
We use simulation when you cannot handle the problem formally - simulation is a semi-formal method, where the computer pretends to be the system.

Think of simulation as a rough-edged axe, and the formal method is a sharp sword for specific use cases.

### Simulation from Safety Standards
[From IEC 61508-7]
__Aim__ - To simulate a real world phenomena by generating random numbers when analytical methods are not applicable.
__Description__ - _Monte Carlo_ simulations are used to solve two classes of problems: 
   - __Deterministic__ : where random numbers are used to generate a stochastic phenomena, and
   - __Probabilistic__: whihch are mathematically translated into an equivalent probabilistic problem (e.g. integral calculations).

[From ISO-26262]
In ISO 26262, "Simulation of dynamic behavior of the design" is listed as one of the _methods for the verification of the software architectural design_.

[From UL 4600 (draft)]
__Simulation vs. Testing__
_Autonomous item complexity is such that relying upon simulation results ... is typically a practical necessity. Simulation results can be used so long as their accuracy is justified, simulation run coverage is justified, and an appropriate non-zero amount of physical testing is used to validate simulation results.

__Pitfall__
Arguing low risk based upon unvalidated simulation results alone is prone to missing risks due to simulation defects, modelling faults, and simplications made in the abstraction process to create the simulation.

### Types of Simulation
- (Described in IEC 61508) Probabilitistic vs Deterministic
- Transaction-Based ("birth -> death")
   - Transaction enter system, enters a queue waiting for processor to be available, seizes a processor, is serviced ...
- Event-Based
   - System starts - schedule the "system fail" event
   - System fails - schedule the "system repair" event

### Discrete Event Simulation Tools
- Common used Discrete-Event
Simulation Language: [General Purpose Simlation System (GPSS)](http://www.cs.bilkent.edu.tr/~cagatay/cs503/_M&S_08_General_Purpose_Simulation_System.pdf).
   - Example syntaxt: `GENERATE()`, `SEIZE()`, `RELEASE()`, `TERMINATE()`
- process-based discrete-event simulation framework based on Python - [`simpy`](https://simpy.readthedocs.io/en/latest/).


#### Confidence Interval
- [_Student t Distribution_](https://en.wikipedia.org/wiki/Student%27s_t-distribution).
It tells us how confident the simulation reflects the real system we are simulating.

#### When to Start Simulating?
When a system has a steady state - the simulation statistics should be reset once the system has a steady state. The simulation statistics should be reset once the system has reached a steady state.

### Back-to-back Modelling

As stated in ISO 26262, "Back-to-back comparison test between model and code, if applicable." is one of the listed methods for verification of software integration.
#### Generate System-test cases
1. Programmer produces a simulation model.
2. Simulation is executed and lists events (in forms of traces).
3. Traces are scanned and equivalence classes are extracted.
4. Each equivalance class becomes a test program.
5. Test programs are executed.
6. Test results are used to improve the model.
7. Return to step 2.

## Programming Languages and Compilers
 The choice of programming language can affect the amount of work you must do to produce a certified product:
 - Some languages are more efficient to code;
 - Some languages support better formal analysis;
 - Some languages support more thorough static analysis;
 - Some compliers (and libraries) are very difficult to qualify. 

#### What is needed in a Language?
- Fully-defined?
- Procedual (`Ada`, `FORTRAN`, `ALGOL`, `C`, `C++`, `Java`, `Rust`) or Functional (`LISP`, `Scheme`, `Clojure`, `Haskell`...)?
- Strong or weak typing?
- Static or dynamic typing?
- Write-once variable?
- Very high-level? (Ex: C++ has a lot of things going behind the scenes: constructors etc.)
- Object oriented?
- Automatic garbage collection?
   - Garbage collection: `Java`, `C#`, `Go`
   - Garbage collecting / manual: `Ada`, `Modula3`, `C++`
   - Manual: `C`
- Exceptions or return codes (functional programming uses return codes):
   - Function that finds the problem loses control of the exchange - connot force the caller to check the return code.
   - An error return code must be something that cannot be a correct answer so it differs from function to function.
- Support for formal contracts?
- Long history of the compiler / linker?
- Good for static analysis (no functional pointers!)?
- Compiled to machine code or using VM?

### Qualifying and Certifying
We __certify__ a product, but we __qualify__ a tool. For a tool, the report will say something like:
_This tool has been qualified as being suitable for use in developments in accordance with ISO 26262:2018 at ASILs up to ASIL C_.

#### Tool Levels: ISO 26262 - Tool Confidence Levels
- Impact Level: TI1 - no impact; TI2 - could impact.
- Error Detection: TD1: almost certainly be noticed; TD2: probably noticed; TD3: other.

#### Tools used in Development Process
- Editors
- Code and document repository
   - how do you know that it's not losing code that you checked in?
- Bug report system
   - how do you know that it's not losing bugs you reported?
- Build system
   - how do you know that it is building from the latest source code?
- Code and documenation review system
   - how do you know that your reviews are not lost?
- Compilers and assemblers
   - how do you know that they are producing correct object code?
   - __"Every compiler we tested was found to crash and also to silently generate wrong code when presented with valid input."__ --["Finding and Understanding Bugs in C Compilers"](https://www.cs.utah.edu/~regehr/papers/pldi11-preprint.pdf)
      - With `IEEE 754` `float` (32 bits) there is no such number as 16777217.0.
      - With `IEEE 754` `double` (64 bits) there is no such number as 9007199254740993.0.
      - Finite-element analysis was carried out using 64-bit floating point number, the calculation was wrong by up to 47%.

### Floating Point Operations
Floating point operations are not always obvious. Using them in safety-critical calculations requires thought and analysis. In particular:
- Never check that 2 floating point numbers are equal (why - internal error when rounding up of floating points).
```cpp
    // Correct method to compare 
    // floating-point numbers 
    if (abs(a - b) < 1e-9) { 
        cout << "The numbers are equal "
             << endl; 
    } 
    else { 
        cout << "The numbers are not equal "
             << endl; 
    } 
}
```
- Don't assume that using floating point integers will make a calculation accurate.
- If possible, use integer arithmetic.
- `unum` (universal numbers, originally from [_The End of Error_](https://www.amazon.com/End-Error-Computing-Chapman-Computational/dp/1482239868)), but not supported by hardware.
- `unum` is a variable length floating-point format that is able to keep track of the precision during operations, offering better result reliability than [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754#:~:text=The%20IEEE%20Standard%20for%20Floating,and%20Electronics%20Engineers%20(IEEE).&text=Many%20hardware%20floating%2Dpoint%20units%20use%20the%20IEEE%20754%20standard.).