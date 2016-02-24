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

Nowadays embedded devices are used in a range of applications of different subject and increase in a fast pace. As a result the security of these devices is a matter of importance and Firmalice was created to address this issue. Firmalice is binary analysis framework that supports the analysis of firmware on these devices. It tried to detect authentication bypasses which can be considered a user performing a privileged action without credentials. These bypasses are difficult to detect because the source code of the firmware is not available, it usually is in the form of a single binary image and the embedded devices frequently require their firmware to be cryptographically signed by the manufacturer. The advantages of this framework is that is does not require source code, its approach scales on multiple devices and it is not based on instrumentation and execution monitoring of firmware. Firmalice performs the following steps in order to detect vunlerabilities in firmware :

1. **Firmware loading**: Disassemble binary file and detect entry points
2. **Security policies**: A security policy will be provided and analysed to detect privileged access points
3. **Static Program Analysis**: A program dependency graph is generated during this phase. It includes a control flow between the execution states and the dependency between instructions and their correlated data.Finally an authentication slice is determined which  is a set of instructions between a proposed entry point and the privileged program point that the attacker tries to reach.
4. **Symbolic Execution Engine**: A symbolic state is an abstract representation of the values contained in memory (e.g., variables), registers, as well as constraints on these values, for any given point of the program (i.e., each program point has an independent state). In this step Firmalice tries to define a path that will reach a privileged point from  an entry point
5. **Authentication Bypass Check** : The privileged states provided by the previous step are checked in order to define if the user input that is required is deterministic. This means that the user input , that leads to the privileged state, can be crafted by an attacker using information from  firmware image and information that is revealed to them via device output.
