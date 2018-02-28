# The Muggle Programming Language

An ultra-general-purpose programming language designed to be friendly to non-professional programmers ("muggles"), and at the same time productive for professional software developers.

**Main audience:**
- professionals writing code without having a Computer Science or Software Engineering background - think physicists, astronomists, chemists, biologist, machine learning reasearches, economists, psychologist etc., and also... hobbyists
- self-thought software developers

**Problems it aims to solve:**
- do not sacrifice **correctness**, **code undertandability** and **debuggability** for *"ease of use"* or *"ease of learning":* we ***can have the cake and eat it too!*** :cake:
- have a **friendly and familiar syntax**: not in the sense of looking a lot like other programming languages, but in the sense that the notation is *similar* to that used in other domains like *math* or **physics**, *easy to learn for mosts*, and, *very* easy to read and understand (by other peoples than who wrote the code originally)
- **make it easy to understand the intention/reason, general logic and cotrol flow of a program** even in the absence of extensive comments and documentation
- ability to optionally but easily code in some **logical correctness guarantees** (types, constraints etc.), even if these *may be severely detrimental to performance* (eg. if "typing is checked at runtime" etc. - someone will probably be able to optimize the code later, once you get it *correct!*)
- **target multiple platforms** including **browsers** (transpilation to *readable* JavaScript), **mobile** (both Android and iOS), **desktop** and **server** (transpiling to JS and using Node.js to run might be enough for start)
- **highly interactive** for *web* and *desktop* version - must feel like an **interpreted language** with a **capable REPL** (all running apps should be able to "spawn a console" and have injected into them at runtime through it) and **integrated debugger**
- **batteries included** - capable and comprehensive standard library, including solid solutions for **web and network**, **math oriented programming** and **machine learning**
- **powerful abstractions capabilities** (including meta-linguistical, eg. "macros"), that should be *easy to use* (eg. for "library users"), even if they may be clunkier to implement
