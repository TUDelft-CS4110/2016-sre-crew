# Fuzzing and Concolic testing

## Fuzzing
Fuzzing is an automated testing technique used to cover boundary test cases providing the application with newly generated inputs.
Creating negative test cases (test cases that check that the system doesn't do something it is not supposed to do) is an incredibly difficult task.
Fuzz testing helps developers in this task creating _semi-valid_ input for tests.
These inputs are valid in the sense that they pass parser checks and they are not stopped before getting to the part of code under test, however they are malformed enough to cause unexpected behaviors in the application.
The process of fuzzing consists in 4 main steps [Pic. 1]:  

1. Getting malformed data  
2. Submitting the data to the application under test
3. Checking if the application fails
4. And, if the application fails, storing the submitted data for further analysis.  

<div >
<img align="right" style="margin-left:10px; margin-top:10px" src="/img/fuzzing flowchart.png">

</div>
Performing this task manually would require a lot of time.
Being able to automate the whole process lets developers and quality assurance teams to take care just of the final analysis while the fuzzer performs its task hundred or even thousands of times.

The main purpose of fuzzing is to test trust boundary conditions.
These are the most relevant conditions since their locations in the code are those where vulnerabilities would result in elevation of privileges.  
Therefore before getting into the 4 steps aforementioned a proper threat analysis is needed.
All input sources (files, APIs, user interfaces, command line arguments, etc.) are possible target for fuzzing. Nonetheless, it is worth to take some time to analyze these when it comes to fuzzing, to prioritize the most important in term of trust boundaries.  

Once chosen the inputs to fuzz it is possible to get the data (step 1) in two ways:
- **Data generation:** It consists in generating data based on a specification that defines how the data should look.
This specification can be of different natures, it just needs to be unambiguous so that the fuzzer will generate proper data to pass to the application.

- **Data mutation:** This, instead, consists in modifying a set of data that is already of team's property.
These data are going to be used as templates for malformed data creation so that the possibility of passing parsers' checks are higher.
When a good dataset is already present data mutation usually is the preferred path to follow since it requires less effort in creating the fuzzer.
On the other hand sometimes trying to mutate data that has to follow strict protocols might require much effort than creating the data directly from scratch.


## Symbolic Testing
With black box fuzzing it can be difficult to get good code coverage since it is sometimes very unlikely to reach a certain statement of a program without looking at the source code. If the source code of a program is known it is possible to utilize that knowledge with symbolic execution.

The goal of symbolic execution is to explore as many program paths as possible in a given amount of time and for each path generate a set of concrete input values exercising that path. It should also check for various kinds of errors like assertion violations, uncaught exceptions, security vulnerabilities and memory corruption. The idea behind symbolic execution is to use symbolic values instead of concrete data values as input, which results in making output values a function of the symbolic input values, and to represent the values of program variables as symbolic expressions over the symbolic input values. During symbolic execution a state σ (sigma) is maintained which maps variables to symbolic expression. A path constraint, PC, is also maintained which is a quantifier-free first-order formula over symbolic expressions. The state σ is initialized to an empty map and is updated at every assignment to contain something like {z -> 2y}. PC is initialized to true, updated with every conditional statement and in the end it is solved with a constraint solver to generate a concrete input for that execution path. At every update, PC becomes PC ^ σ(e) for the “then” branch and a new path constraint PC’ is created which is initialized to PC ^ ¬σ(e). If PC or PC’ are satisfiable for some assignment of concrete to symbolic values, symbolic execution continues for PC in the “then” branch or is created for PC’ for the “else” branch. Symbolic execution terminates if any of PC or PC’ is not satisfiable. If the symbolic execution hits an exit statement or an error, the execution is terminated and a constraint solver is used to form an input from the PC. 

The main disadvantage to classical symbolic execution is that it cannot create an input if the PC along an execution path contains formulas that can not be efficiently solved with a constraint solver. For example, if a program contains the statement x = twice(y), where twice is an uninterpreted function in a library, the constraint solver can not solve it since they don’t know how to manipulate x. In this case, the symbolic execution will fail to generate any input.
There are two modern solutions to these problems which are **Concolic Testing** and **Execution-Generated Testing** (EGT). Their technique is to mix concrete and symbolic execution.

 **Concolic Testing**, also named Directed Automated Random Testing (DART), performs symbolic execution dynamically but executes the program on concrete input values. While Concolic Testing executes the program symbolically and concretely, **EGT** makes a distinction between the symbolic and concrete state. It checks before every operation if the values are all concrete or not whereas if not all are concrete, the operation is performed symbolically.
 The main advantage of using these dynamic techniques such as Concolic Testing and EGT is that by using concrete values it is possible to alleviate imprecision due to external code and constraint solving timeouts. For the former example of x = twice(y), now it is possible to use the concrete value of y to get an actual value of x, e.g. 18 = twice(9).



### **Key challenges of symbolic execution**
 * **Path Explosion**: In all but very small programs the number of program paths can be huge (usually exponential in the number of static branches). Therefore it is necessary to explore the most relevant paths first. Symbolic execution already filters out paths which do not depend on the symbolic input and are infeasible given current path constraints. In addition to solve this problem, **heuristic techniques** and **sound program analysis techniques** have been used. **Heuristic techniques** include using **static control-flow graphs (CFG), random exploration, random testing, evolutionary search** and **mutation testing**. **Sound program analysis techniques**  reduce the complexity of path exploration by using ideas from program analysis and software verification. These techniques involve merging explored paths or using compositional techniques by caching and reusing analysis of lower-level functions in subsequent computations
 * **Constraint Solving**: Even though recent advances in Constraint Solving have allowed for practical Symbolic Execution it still remains as the biggest bottleneck. Two types of optimizations are commonly used for this problem; **Irrelevant constraint elimination** and **Increment solving**. The former tries to remove from the path conditions the constraints that are irrelevant in deciding the outcome of the current branch. The latter tries to reuse the results of previous similar queries. The counter-example caching scheme is an example of the latter which utilizes cached supersets and subsets of constraints.
 * Other problems involve **Memory modeling** and **Handling concurrency**.

### Tools
There are several modern tools that implement dynamic symbolic execution.
* **Dart** was the first concolic testing tool to combine dynamic test generation with random testing and modeling techniques. **CUTE** extends DART to handle multi-threaded programs that manipulate dynamic data structures using pointer operations. **CREST** is an open source tool for concolic testing of C programs which you can use to experiment with heuristics for selecting which paths to explore
* **EXE** is a symbolic execution tool for C programs made especially to test complex software with an emphasis on system code. In order to do so, EXE models memory with bit-level accuracy. **KLEE** is a redesign of EXE but has the capability to store a much larger number of concurrent states.
* Other industry tools include **SAGE** (Microsoft) for x86 binaries and **PEX** for .NET.

## Improvements
Although fuzzing is a fast technique to detect error, there is room for improvement. The main drawbacks of fuzz testing are its poor coverage and quality of tests. There have been various suggestions to improve fuzzing and below we list some of them :

### Vulnerability Pattern
By analysing the binary code, potentially vulnerable sequences of code can be discovered that can lead to a fault in the execution of the program. The discovery of  these patterns will help to identify interesting points in the binary that require more focus. For example a vulnerable pattern can be a code that uses vulnerable functions such as _strcmp_.

### Taint Analysis
Taint Analysis consists of marking data from external sources e.g. network or user input as tainted. Afterwards, during the execution of the binary these data are tracked as well as the path they follow. This will provide insight about where this data is used and if it used in dangerous  segments of the binary e.g. overwrite a return address. More specifically, the vulnerability patterns can provide information about the aforementioned dangerous segments.

### Test Generation
The test generation is the most important part of fuzzing since the input provided to the binary will determine whether there is a weak point or not. In order to cover as many paths as possible two different methods have been suggested.
  * The aforementioned Concolic execution
  * Search algorithms  e.g. genetic algorithms


### Fuzzing Evaluation
In order to improve fuzzing with these techniques we need to be able to measure their effectiveness.
* Monitor for  violations in runtime
* Analyze stack, heap content when a vulnerability is triggered



## Sage : Whitebox Fuzzing for Security Testing
With whitebox fuzzing the program is dynamically executed symbolically, gathering constraints on inputs from conditional branches encountered in the execution (like described in symbolic testing). 
These constraints are then negated and solved with a constraint solver, which is mapped into new inputs to execute the program. 
This can be very effective to verify programs and can discovers many corner cases. 
This process can however be imprecise due to complex program statements and incomplete due to a large amount of execution paths in a program.

The first implementation of whitebox fuzzing was SAGE (Scalable Automated Guided Execution). 
To counter the problem of the huge number of execution paths, SAGE implements generational search that maximizes the number of new input test generated for each symbolic execution. 
With a normal breadth-first or depth-first search it takes too long time if only one input is found per execution since there can be a great amount of execution paths. 
It will therefore take less time to run SAGE since it will do fewer symbolic executions.
This is done by negating all constraints one by one on an execution path and placing them in a conjunction with the previous negations of the path. 
These are then solved by a constraint solver and multiple tests are created. 
Furthermore, to counter the huge execution traces, SAGE applies many optimizations such as symbolic-expression caching, unrelated constraint elimination and local constraint caching.
Additionally, SAGE works at the x86 binary level so it can be used on any program regardless of its source language or build process.

SAGE has been used extensively by Microsoft. 
With Windows 7, SAGE is run last to find bugs after static program analysis and blackbox fuzzing, but still managed to find one-third of all bugs in Windows 7 discovered by file fuzzing. 
In general, blackbox fuzzing is simple, lightweight and fast but whitebox fuzzing smarter but more complex. 
It is therefore good practice to first apply blackbox fuzzing to find the easier bugs and then apply whitebox fuzzing to find more bugs.

## Fuzzing on Android Devices
Fuzzing is employed as testing technique also in mobile environment.
Mahmood R. et al. developed a fuzzing system that can be used to test Android applications.
This can be employed for testing code under development but also already existing apps (in this case the apk needs to be decompiled in order to obtain from the java bytecode a representation of the source code).  
As explained above, before starting with the process of fuzzing it is necessary to identify the input surface.
To do so the _architectural model_ and the _call graph model_ [Pic. 2] come into place.
The architectural model tells the fuzzer which is the main _activity_ (activities are screen presented to the user) of the application.
Using the call graph model, instead, the input surface is defined identifying GUI inputs accepted by activities and all the other inputs accepted by _services_ (services are processes run in the background from the application).  
Retrieved the input surface, test cases are generated mixing this information with templates already existing in the system.  
As mentioned above code coverage represents a big issue while testing, for this reason different inputs are generated using ad-hoc fuzzers (these generate inputs based on the specific input domain: text, numbers, etc.), and improved every round checking the current test coverage.  
With the intent of optimizing the process these generated test cases are run on multiple parallel virtual machines on a cloud environment.   
Finally relevant outputs are saved for further analysis under 4 different categories: _Interface_, _Interaction_, _Permissions_ and _Resources_.

![Android test generation](/img/android.png)

<p align="center">Pic. 2 Android test generation</p>

## Fuzzing on embedded devices (Firmalice)

Nowadays embedded devices are used in a range of applications of different subject and increase in a fast pace. As a result the security of these devices is a matter of importance and Firmalice was created to address this issue. Firmalice is a binary analysis framework that supports the analysis of firmware on these devices. It tried to detect authentication bypasses which can be considered as a user performing a privileged action without credentials. These bypasses are difficult to detect because the source code of the firmware is not available, it usually is in the form of a single binary image and the embedded devices frequently require their firmware to be cryptographically signed by the manufacturer. The advantages of this framework is that is does not require source code, its approach scales on multiple devices and it is not based on  the instrumentation and execution monitoring of the firmware. Firmalice performs the following steps in order to detect vulnerabilities in firmware :

1. **Firmware loading**: Disassemble binary file and detect entry points
2. **Security policies**: A security policy will be provided and analyzed to detect privileged access points
3. **Static Program Analysis**: A program dependency graph is generated during this phase. It includes a control flow between the execution states and the dependency between instructions and their correlated data.Finally an authentication slice is determined which  is a set of instructions between a proposed entry point and the privileged program point that the attacker tries to reach.
4. **Symbolic Execution Engine**: A symbolic state is an abstract representation of the values contained in memory (e.g., variables), registers, as well as constraints on these values, for any given point of the program (i.e., each program point has an independent state). In this step Firmalice tries to define a path that will reach a privileged point from  an entry point
5. **Authentication Bypass Check** : The privileged states provided by the previous step are checked in order to define if the user input that is required is deterministic. This means that the user input , that leads to the privileged state, can be crafted by an attacker using information from  firmware image and information that is revealed to them via device output.

# References

1. Godefroid, Patrice, Michael Y. Levin, and David Molnar. "SAGE: whitebox fuzzing for security testing." Queue 10.1 (2012): 20.
2. Cadar, Cristian, and Koushik Sen. "Symbolic execution for software testing: three decades later." Communications of the ACM 56.2 (2013): 82-90.
3. Yan Shoshitaishvili, Ruoyu Wang, Christophe Hauser, Christopher Kruegel, and Giovanni Vigna. 2015. Firmalice - Automatic Detection of Authentication Bypass Vulnerabilities in Binary Firmware. In Proceedings 2015 Network and Distributed System Security Symposium. Reston, VA: Internet Society.
4. Oehlert, P. (2005). Violating assumptions with fuzzing. IEEE Security and Privacy, 3(2), 58-62. doi:10.1109/MSP.2005.55
5. Bekrar, S., Bekrar, C., Groz, R., & Mounier, L. (2011). Finding software vulnerabilities by smart fuzzing. Paper presented at the Proceedings - 4th IEEE International Conference on Software Testing, Verification, and Validation, ICST 2011, 427-430. doi:10.1109/ICST.2011.48
6. Mahmood, R., Esfahani, N., Kacem, T., Mirzaei, N., Malek, S., & Stavrou, A. (2012). A whitebox approach for automated security testing of android applications on the cloud. Paper presented at the 2012 7th International Workshop on Automation of Software Test, AST 2012 - Proceedings, 22-28. doi:10.1109/IWAST.2012.6228986
