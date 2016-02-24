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
These are the most relevant conditions since their locations in code are locations where vulnerabilities would result in elevation of privileges.  
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

## Firmalice
