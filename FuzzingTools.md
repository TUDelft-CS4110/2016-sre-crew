# Fuzzing Tools

* **Intent Fuzzer** (ncc group)  
https://www.nccgroup.trust/us/about-us/resources/intent-fuzzer/

* **Honggfuzz**  
http://google.github.io/honggfuzz/

* **MFFA**  
Media Fuzzing Framework for Android  
https://github.com/fuzzing/MFFA


# General Tools

* **jFuzz**  
http://people.csail.mit.edu/akiezun/jfuzz/documentation.html

* **American Fuzzy Lop**  
https://fuzzing-project.org/tutorial3.html

* **angr**  
http://angr.io/

* **KLEE**  
http://klee.github.io/

* **antiparser**
http://antiparser.sourceforge.net/

    **Antiparser** is a fuzz testing and fault injection API. Fuzz testing has application as a security research methodology and for software quality assurance purposes.

    The purpose of antiparser is to provide an API that can be used to model network protocols and file formats by their composite data types. Once a model has been created, the antiparser has various methods for creating random sets of data that deviates in ways that will ideally trigger software bugs or security vulnerabilities.

    The antiparser itself is a container of data objects, each representing a part of the protocol or file format. These data objects have their own properties that may be used to define the bounds and the parameters of the data, such as whether or not the data is a static field, how small or large the data may be, the legal range of values that may be randomly generated or "permuted". Each specific instance of permuted data may then be saved to a file and loaded at a later time for replay if required. This provides an easy means of organizing the results of a particular fuzz test.

* **Basic Fuzzing Framework (BFF)**
https://www.cert.org/vulnerability-analysis/tools/bff.cfm?

  The CERT Basic Fuzzing Framework (BFF) is a software testing tool that finds defects in applications that run on the Linux and Mac OS X platforms. BFF performs mutational fuzzing on software that consumes file input. (Mutational fuzzing is the act of taking well-formed input data and corrupting it in various ways, looking for cases that cause crashes.) The BFF automatically collects test cases that cause software to crash in unique ways, as well as debugging information associated with the crashes. The goal of BFF is to minimize the effort required for software vendors and security researchers to efficiently discover and analyze security vulnerabilities found via fuzzing.
