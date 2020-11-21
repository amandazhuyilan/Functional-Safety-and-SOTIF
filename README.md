# Software Functional Safety and SOTIF

This repo contains notes and related coursework based on Functional Safety and SOTIF for embedded software offered by Chris Hobbs.

## 1. Introduction
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