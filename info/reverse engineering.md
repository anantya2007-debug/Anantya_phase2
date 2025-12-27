## Forward Engineering tools
- vim /an IDE (Integrated Development Environment)
- gcc
- strings
- strip

Stuff gets lost in the process of compiling and assembling 
- preprocessor (C/C++ → preprocessed C) : removes comments, macro names and intent, file boundaries (#include), original formatting/structure
- Compilation (C → Assembly) : lose variable names, type info, high-level control structures (for, while, if, switch), sequence of instructions 
- Assembly → Object Files : lose logical blocks, symbolic expressions
- Linking (Object Files → Executable) : lose source file boundaries, function-to-file mapping, static library identity 
