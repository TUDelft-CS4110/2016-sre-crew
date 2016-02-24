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
