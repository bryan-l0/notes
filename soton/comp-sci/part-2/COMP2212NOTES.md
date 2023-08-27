# Structure of Programming Languages

## Introduction

### Application Domains
1. Scientific Computing (MatLab, Mathematica)
1. Business Applications (COBOL)
1. Artificial Intelligence (Machine Learning in Python, R, Julia)
1. Computer Systems (C, C++, Rust)
1. Web Development (Markup - HTML, CSS. Scripting - Perl, PHP, Javascript.)

### Programming Language Families
1. Imperative Languages (C, C++, Java, Rust)
1. Declarative Languages
    1. Functinoal Programming (OCaml, Haskell, ML)
    1. Logic Programming (Prolog)
    1. Domain Specific (HTML, SQL, SOAP)

### Software Design Methodologies
- Data Oriented
    - Abstract Data Types
    - Object Oriented: Encapsulate data with code, concepts of classes, objects, inheritance...
- Procedure Oriented
    - Emphasis on decomposing code into logically independent actions, emphasis in concurrent programming.

### Evaluation Criteria
**Readability:** How easy is it to read and understand code. Readability vs Convenience, Obfuscation, Simplicity can be deceiving, Feature multiplicity (e.g. i++), Orthogonality (arrays in C).  
**Abstraction:** Trend of programming is going towards higher-level languages. Separation from machine, achieved via smart compilers, etc...  
**Efficiency:** Certain language features are difficult to implement efficienty, or difficult to profile code execution (i.e. lazy evaluation in Haskell makes it hard to find bottlenecks). Good compilers usually optimise code really well.  

### What's Next
- Languages that support big data computation.
- Component-based programming and frameworks to bridge the gap between hardware and software design.
- Libraries are important, but need a rethink due to scalability and orthogonality issues.
- Compatibility with other language features is a challenge with concurrency.
- Types for reliable, safe, and secure code, is becoming more prominent.

## Variables
Originally meant a reference to a memory location whose contents could change. Now refers to a placeholder to some type. Aliasing refers to two variables pointing to the same memory location (but should be avoided, is not good for memory management due to the computer having to reload variables constantly). Typically have these attributes:

1. Name
1. Address (LHS)
1. Value (RHS)
1. Type
1. Extent
1. Scope

### Binding
An association between an entity and some attribute.  
**Static Binding** occurs before compile time and remains unchanged throughout execution.  
**Dynamic Binding** first occurs during runtime and changing throughout.  
**Allocation** is a static/dynamic binding of a variable to its address. Unbinding of this variable to its address is dynamic, called **deallocation**.  
A variable's **extent** is the time between allocation and deallocation.

### Variable Types
**Static/Global Variables:** Bound to a memory location at initialisation time.  
**Stack-Dynamic/Local Variables:** Allocated from runtime stack and bound when a declaration is executed. Deallocated when the procedure block returns.  
**Explicit Heap-Dynamic Variables:** Nameless abstract memory locations that are allocated/deallocated by explicit runetime commands, e.g. malloc/free, new/delete, new().  
**Implicit Heap-Dynamic Variables:** Memory in heap is allocated when variables are assigned to variables, deallocated and reallocated with re-assignment. Error prone and inefficient. E.g. JS Arrays.

### Type Binding
**Static Type Binding:** Most commonly done with *type declarations*. A variable is introduced with an explicit type and potentially initial value. Can also be done with *type inference*, where the type is inferred from usage of the variable, or by following a fixed naming scheme, e.g. Fortran.  
**Dynamic Type Binding:** Variables assigned at runtime, commonly seen in Python/JS. Efficiency implications due to runtime type checking, arguably has advantages wrt coding convenience and readability.

### Extent
**Static Variables** have an extent of the whole program execution.  
**Stack-Dynamic Variables** have an extent of a particular stack frame/procedure call.  
**Explicit Heap-Dynamic Variables** have an extent from explicit allocation to explicit deallocation.  
**Imlicit Heap-Dynamic Variables** have an extent from implicit allocation to implicit allocation (values may persist in memory but addresses are freed).

### Scope
The scope of a variable is where it can be referenced. It also affects its extent, for a non-referencable value may be considered a meaningless value, which gets garbage collected.  
**Local Variables** are declared within a program block, which is their scope. **Static variables** have global scope, but are hidden by a locally scoped variable with the same name.  
**Lexical Scope** is aligned to statically determined areas of code, e.g. class definition, code block, method body. It is a lexical, not semantic, concept.  
**Dynamic Scope** is supportec by some languages. It is determined at runtime only, as it depends on control flow, and uses the most recent binding of that variable in the call stack.

## Syntax
The structure of statements in a program, and usually follows a grammar based upon certain lexical symbols. Interpreters transform syntax into semantics, which is the meaning of programs, and how they execute.

### (Non-)Terminals
BNF is commonly used as a metalanguage to define the grammar of languages.  
**Non-terminals** represent different states of determining whether a string is accepted by the grammar, e.g. class level declarations, method declarations, statements, and expressions.  
**Terminals** represent the actual symbols that appear in the strings accepted by the grammer. They are also called *tokens* or *lexemes*, and refer to the reserved words, variable names, and literals, of our programs.

### Parse Trees
Legal programs of a language have a derivation in BNF, which can be represented by a tree. Each node represents the rule of the grammar that has been used to continue the derivation. A typical program is parsed as follows:

1. Lexing - Transforming symbols into tokens.
1. Parsing - Translating tokens into a tree, which follows the rules of the grammar.

**Abstract Syntax Trees:** ASTs are more commonly used, it removes unnecessary detail regarding how a term was parsed, compared to parse trees.

### Ambigious Grammars
A grammar G is *ambigious* if exists string `s` for which exists two or more parse trees, e.g. 2 + 3 \* 4 w/o operator precedence.  
To resolve abiguity, operator precedences are used, by allowing certain operators to *bind* more tightly, e.g. \* binds tighter than +, which prioritises the \* operation over +. This moves the higher precedence operators lower into the parse tree. Parentheses can also be used to reset precedence.

    <expr> ::= <mexpr> + <expr> | <mexpr>       Right Associative
    <expr> ::= <expr> + <mexpr> | <mexpr>       Left Associative

### Dangling Else Problem
Many programming languages allow for `if` statements without an accompanying `else` statement. Java solves this by having additional non-terminals to determine the precedence when dealing with nested conditional statements within the body of an `if` statement's `then` branch, to prevent it from using a single branch conditional.

    <if-then>                  ::= if ( <expr> ) <stmt>
    <if-then-else>             ::= if ( <expr> ) <stmt-no-short-if> else <stmt>
    <if-then-else-no-short-if> ::= if ( <expr> ) <stmt-no-short-if> else <stmt-no-short-if>

## Lexing
Process of transforming symbols into tokens. A ***lexeme*** is a pattern of characters in the input string that is meaningful to the language, which may be defined by RegEx. A ***token*** is a *lexeme* that has been identified and "tagged" with a meaning and possibly include a value.  

    `while` is a lexeme of the characters w, h, i, l, e, which has been identified as a `whileToken`. 
    `True` is a `booleanToken` with value `true`.

Lexemes generally use a scanner that does pattern matching using *maximal munch*, where it matches as much as possible from each rule.  
Tokens are created using an ***evaluator*** which analyses the lexemes, tags them, and identifies any associated value.  
Lexers can be reasonable generic if we know what the lexemes look like, and know which lexemes correspond to which tokens and how to associate values with them, which allows us to automatically code that creates these tokens.

## Parsing
The process of converting a stream of lexical tokens to an abstract syntax tree representation of a program by matching it against the grammar of the language.  
This is done after lexical analysis in the compiler/interpreter tool chain.  
**Top Down/Recursive Descent Parsing:** Builds parse tree from the root down. Left-right leftmost derivations.  
**Bottom Up Parsing:** Builds parse tree from leaves up. Left-right rightmost derivations. More complicated but handles a wider range of grammars.  
**Shift-Reduce Parsing:** Used to implement LR parsers. Scans token stream and builds subtrees bottom up. At each stage the parser either *shifts* (reads next input token), or *reduces* (applies a completed grammar rule to join subtrees). This repeats until it runs out of tokens, and a single tree is formed. State machines with 4 actions: *ERROR*, *SHIFT*, *REDUCE*, *STOP*. Conflicts may occur when the state machine is non-deterministic.

\newpage
# Type Systems
A tractable syntactic method for proving the absence of certain program behaviours by classifying program phrases according to the kinds of values they compute.  
It allows us to guarantee the absence of certain behaviours, e.g. application of an arithmetic expression to the wrong kind of data, missing method or field when invoked on a particular object, and array lookups of the wrong dimension.  
We can also enforce higher-level modularity properties, maintain integrity of data abstractions, and check for violation of information hiding.  
Type systems enforce disciplined programming, and forms the backbone of module based languages for large-scale composition. Types are the interfaces between the modules, and encourages abstract design. Types are also a form of documentation.
 
## Introduction
Types are ***abstract*** descriptions of programs, allows us to study the correctness properties of programs.  
Types are also ***precise*** descriptions of program behaviours, allowing us to use mathematical tools to formalise and check these properties.  

### Approaches to Types
**Strongly Typed:** Languages check use of data to prevent program errors.  
**Weakly Typed:** May not prevent errors, but uses types for other compilation purposes, such as memory layout.  
**Static Typing:** Type checking occurs during compile time.  
**Dynamic Typing:** Type checking occurs during run time.  

### Run Time
***Statically typed*** languages necessarily use an *approximation* of the runtime types of values, because statically determining control flow is undecidable in sufficiently rich languages. However, compile time checking can avoid costly run time errors. Static typing is more appropriate in languages where types are used for memory layout, i.e. C.  
***Dynamically typed*** languages check the types of data at point of use during runtime. Exact types can be used, which implies no false negative type errors. Variables can be allowed to change type, or objects growing new methods. Should not be used for critical systems as errors may be detected too late.

### Type Safety
*Well typed programs never go wrong.*  

\newpage
# Concurrency
Concurrency is different to *parallel computing* because the multiple computations can potentially interact with each other, typically on a single machine.

## Definitions
Each executing sequence of control is a **thread**. For threads to interact they need to *synchronise* by sending and receiving signals between them.

### Signal and Wait
**Synchronous Communication:** A thread signals another thread, then enters a waiting state, until the signal is received by the recipient thread. All threads rendezvous at a given point in their execution.

### Signal and Continue
**Asynchronous Communication:** A thread signals another thread, then continues its execution immediately after.

### Semantics of Concurrency
Semantics of each individual thread must consider the interactions it makes with the environment in which it executes. This may include interactions with memory, and interactions with other threads, etc...  
This can be defined as a *small step operational semantics* where the relation includes an indication of any interaction the thread makes, as well as computational steps internal to the thread.  
For n threads, a thread $T_n$ executes *action-n-m*, where each step $m$ moves a thread to state $T^m$.  
Actions can also be synchronised, where *action-i-k* and *action-j-k* may be complementary send/receive actions.

### Calculus of Communicating Systems (CCS)
$P ::= nil \ \vert \ a \ ! \ P \ \vert \ a \ ? \ P \ \vert \ \tau \ P \ \vert \ P \ \Vert \ P \ \vert \ P \ \backslash \ a \ \vert \ X \ \vert \ rec \ X \ . \ P$  
*nil* is a terminated, intert process.  
*a!P* sends a signal on channel `a` and continues execution as P.  
*a?P* receives a signal on channel `a` and continues execution as P.  
*$\tau$ P* takes an internal computation step.  
*P || P* represents two concurrent processes, which includes modelling thread spawning.  
*P \ a* is a restriction operation that disallows any communication on channel `a` originating inside of P from being seen.  
*X* and *rec X . P* are used for recursive behaviour, where `rec X . a! b ? X` repeatedly sends a signal on `a` and receives a signal on `b`.