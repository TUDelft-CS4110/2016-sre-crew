# Fuzzing and Concolic testing

Fuzzing is an automated testing technique used to cover boundary test cases providing the application with newly generated inputs.
Creating negative test cases (test cases that check that the system doesn't do something it is not supposed to do) is an incredibly difficult task.
Fuzz testing helps developers in this task creating _semi-valid_ input for tests.
These inputs are valid in the sense that they pass parser checks and they are not stopped before getting to the part of code under test, however they are malformed enough to cause unexpected behaviors in the application.
The process of fuzzing consists in 4 main steps:
1. Get malformed data  
2. Submit the data to the application under test
3. Check if the application fails
4. If the application fails the submitted data would be stored for further analysis.  

This process is represented in the picture below.   
![Fuzzing flowchart](/img/fuzzing flowchart.png)  
Performing this task manually would require a lot of time.
Being able to automate the whole process let developers and quality assurance teams take care just of the final analysis while the fuzzer perform its task hundred or even thousands of times.

The main purpose of fuzzing is to test trust boundary conditions.
These are the most relevant conditions since their locations in code are those where vulnerabilities would result in elevation of privileges.  
Therefore before getting into the 4 steps aforementioned a proper threat analysis is needed.
All input sources (files, APIs, user interfaces, command line arguments, etc.) are possible target for fuzzing however it is worth to take some time to analyze those when it comes to fuzzing to prioritize the most important in term of trust boundaries.  

Once chosen the inputs to fuzz it is possible to get the data (step 1) in two ways:
- **Data generation:** It consists in generating data based on a specification that defines how the data should look.
This specification can be of different natures.
The only things that matter is that it has to be unambiguous so that the fuzzer will generate proper data to pass to the application.

- **Data mutation:** This, instead, consists in modifying a set of data that is already of team's property.
These data are going to be used as template for malformed data creation so that the possibility of passing parsers' checks are higher.
When a good dataset is already present data mutation usually is the preferred path to follow since it requires less effort in creating the fuzzer.
On the other hands sometimes trying to mutate data that has to follow strict protocols might require much effort than creating the data directly from scratch.

Finally once decided whether to generate new data or mutate previously held data there are 3 possible implementation of a fuzzer:
- **Unintelligent fuzzer:** Generates random inputs to submit to the application (sometimes this is preferred cause it can reach conditions totally unexpected).

- **Intelligent fuzzer:** Generates data that conform specifics of the application so that it knows that it with those data it will pass specific checks.  

- **Pattern-based fuzzer:** The most cost-effective solution that provides results almost at the level of the intelligent fuzzer and consists in identifying specific patterns in the results of the execution and, once identified, use these latter to generate the next inputs.

## Smart Fuzzing
Although fuzzing is a fast technique to detect error, there is room for improvement. The main drawbacks of fuzz testing are its poor coverage and quality of tests.In [CITE PAPER] there have been various suggestions to improve fuzzing.

### Vulnerability Pattern
By analysing the binary code, potentially vulnerable sequences of code can be discovered that can lead to a fault in the execution of the program. The discovery of  these patterns will help to identify interesting points in the binary that require more focus. For example a vulnerable pattern can be a code that uses vulnerable functions such as _strcmp_.

### Taint Analysis
Taint Analysis consists in marking data from external sources e.g. network or user input as tainted. Afterwards, during the execution of the binary these data are tracked as well as the path they follow. This will provide insight about where this data is used and if it used in dangerous  segments of the binary e.g. overwrite a return address. More specifically, the vulnerability patterns can provide information about the aforementioned dangerous segments.

### Test Generation
The test generation is the most important part of fuzzing since the input provided to the binary will determine whether there is a weak point or not. In order to cover as many paths as possible two different methods have been suggested.
  * Concolic execution
  * Search algorithms  e.g. genetic algorithms


### Fuzzing Evaluation
In order to improve fuzzing with these techniques we need to be able to measure their effectiveness.
* Monitor for  violations in runtime
* Analyze stack, heap content when a vulnerability is triggered



## Sage

## Android Section
Fuzzing is employed as testing technique also in mobile environment.
Mahmood R. et al. [cite android paper] developed a fuzzing system that can be used to test Android applications.
This can be employed for testing code under development but also already existing apps (in this case the apk needs to be decompiled in order to obtain from the java bytecode a representation of the source code).  
As explained above, before starting with the process of fuzzing it is necessary to identify the input surface.
To do so the _architectural model_ and the _call graph model_ [Picture 2] come into place.
The architectural model tells the fuzzer which is the main _activity_ (activities are screen presented to the user) of the application.
Using the call graph model, instead, the input surface is defined identifying GUI inputs accepted by activities and all the other inputs accepted by _services_ (services are processes run in the background from the application).  
Retrieved the input surface test cases are generated mixing this information with templates already existing in the system.  
As mentioned above code coverage represents a big issue while testing, for this reason different inputs are generated using ad-hoc fuzzers (these generate inputs based on the specific input domain: text, numbers, etc.), and improved every round checking the current test coverage.  
With the intent of optimizing the process these generated test cases are run on multiple parallel virtual machines on a cloud environment.   
Finally relevant outputs are saved for further analysis under 4 different categories: _Interface_, _Interaction_, _Permissions_ and _Resources_.

![Android test generation](/img/android.png)
## Firmalice

Nowadays embedded devices are used in a range of applications of different subject and increase in a fast pace. As a result the security of these devices is a matter of importance and Firmalice was created to address this issue. Firmalice is binary analysis framework that supports the analysis of firmware on these devices. It tried to detect authentication bypasses which can be considered a user performing a privileged action without credentials. These bypasses are difficult to detect because the source code of the firmware is not available, it usually is in the form of a single binary image and the embedded devices frequently require their firmware to be cryptographically signed by the manufacturer. The advantages of this framework is that is does not require source code, its approach scales on multiple devices and it is not based on instrumentation and execution monitoring of firmware. Firmalice performs the following steps in order to detect vunlerabilities in firmware :

1. **Firmware loading**: Disassemble binary file and detect entry points
2. **Security policies**: A security policy will be provided and analysed to detect privileged access points
3. **Static Program Analysis**: A program dependency graph is generated during this phase. It includes a control flow between the execution states and the dependency between instructions and their correlated data.Finally an authentication slice is determined which  is a set of instructions between a proposed entry point and the privileged program point that the attacker tries to reach.
4. **Symbolic Execution Engine**: A symbolic state is an abstract representation of the values contained in memory (e.g., variables), registers, as well as constraints on these values, for any given point of the program (i.e., each program point has an independent state). In this step Firmalice tries to define a path that will reach a privileged point from  an entry point
5. **Authentication Bypass Check** : The privileged states provided by the previous step are checked in order to define if the user input that is required is deterministic. This means that the user input , that leads to the privileged state, can be crafted by an attacker using information from  firmware image and information that is revealed to them via device output.
