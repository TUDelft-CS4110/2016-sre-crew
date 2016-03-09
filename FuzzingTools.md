# Fuzzing Tools

* **Intent Fuzzer** (ncc group)  
Intent Fuzzer is a tool that can be used on any device using the Google Android operating system (OS). Intent Fuzzer is exactly what is seems, which is a fuzzer. It often finds bugs that cause the system to crash or performance issues on the device. The tool can either fuzz a single component or all components. It works well on Broadcast receivers, and average on Services. For Activities, only single Activities can be fuzzed, not all them. Instrumentations can also be started using this interface, and content providers are listed, but are not an Intent based IPC mechanism.  
https://www.nccgroup.trust/us/about-us/resources/intent-fuzzer/

* **Honggfuzz**  
Honggfuzz is a general-purpose fuzzing tool. Given a starting corpus of test files, honggfuzz supplies and modifies input to a test program and utilize the ptrace() API/POSIX signal interface to detect and log crashes.  
Now also has support for Android.  
Honggfuzz (since its version 0.5) is capable of performing feedback-driven fuzzing. It utilizes Linux perf subsystem and hardware CPU counters to achieve the best outcomes.  
Developers can provide their own initial file (-f flag) which will be gradually improved upon. Alternatively, honggfuzz is capable of starting with just empty buffer, and work its way through, creating a valid fuzzing input in the process.
http://google.github.io/honggfuzz/

* **MFFA** (Media Fuzzing Framework for Android)  
The main idea behind this project is to create corrupt but structurally valid media files, direct them to the appropriate software components in Android to be decoded and/or played and monitor the system for potential issues (i.e system crashes) that may lead to exploitable vulnerabilities. Custom developed Python scripts are used to send the malformed data across a distributed infrastructure of Android devices, log the findings and monitor for possible issues, in an automated manner. The actual decoding of the media files on the Android devices is done using the Stagefright command line interface. The results are sorted out, in an attempt to find only the unique issues, using a custom built triage mechanism.  
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
