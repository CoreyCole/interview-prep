# How JavaScript Works
multipart series starting [here](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)

## Part 1: an overview of the engine, the runtime, and the call stack
- js is single-threaded
- js uses a callback queue

### The JS Engine: V8
- V8 is used inside Chrome and Node.js
- engine consists of two main components
  - memory heap: where all memory allocation happens
  - call stack: where your stack frames are as your code executes

### The Runtime
![detailed-diagram](https://cdn-images-1.medium.com/max/800/1*4lHHyfEhVB0LnQ3HlhSs8g.png)
- APIs in browser like `setTimeout()` are not provided by the engine
- Web APIs provided by browsers, like the DOM, AJAX, setTimout, etc.

### The Call Stack
- single threaded => single call stack
  - "where in the program we are"
  - step into function => put it on the top of the stack
  - return from a function => pop off the top of the stack
- each entry in the call stack (function) is called a **stack frame**
- "blowing the stack" (stack overflow)
  - happens when you reach the maximum call stack size

### Concurrency & the Event Loop
- rendering in the browser can't be blocked by long-running functions
  - sometimes browsers might even throw an error saying the page is unresponsive
- using **asynchronous callbacks** we can execute heavy code without blocking he UI

## Part 2: inside the V8 engine + 5 tips on how to write optimized code
Found [here](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e)
- js engine = program or interpreter which executes js code
  - can be implemented as a standard interpreter
  - or JIT compiler that compiles js to bytecode in some form
- list of popular projects that are implementing a js engine:
  - V8 — open source, developed by Google, written in C++
  - Rhino — managed by the Mozilla Foundation, open source, developed entirely in Java
  - SpiderMonkey — the first JavaScript engine, which back in the days powered Netscape Navigator, and today powers Firefox
  - JavaScriptCore — open source, marketed as Nitro and developed by Apple for Safari
  - KJS — KDE’s engine originally developed by Harri Porten for the KDE project’s Konqueror web browser
  - Chakra (JScript9) — Internet Explorer
  - Chakra (JavaScript) — Microsoft Edge
  - Nashorn, open source as part of OpenJDK, written by Oracle Java Languages and Tool Group
  - JerryScript — is a lightweight engine for the Internet of Things.

### Why was the V8 Engine created?
- V8 translates js to machine code instead of using an interpreter
- uses a JIT compiler
- doesn't produce bytecode or any intermediate code
- V8 used to have two compilers before version 5.9 (released earlier 2017)
  - full-codegen — a simple and very fast compiler that produced simple and relatively slow machine code.
  - Crankshaft — a more complex (Just-In-Time) optimizing compiler that produced highly-optimized code.
- V8 Engine uses several threads internally
  - main thread: fetch code, compile it & execute it
  - compilation thread: main thread can keep executing while compiling concurrently
  - profiler thread: tell runtime which functions we spend a lot of time in so Crankshaft can optimize them
  - a few garbage collection sweeping threads

#### The process
1) leverage full-codegen to translate parsed js into machine code without any transformation
  - does not use intermediate bytecode representation, so removes the need for an interpreter
2) after running for some time, profiler thread starts to look for functions to optimize
3) Crankshaft optimizations begin in another thread
  - translates the js abstract syntax tree to a high-level static single-assignment (SSA) representations called Hydrogen
  - tries to optimize that Hydrogen graph
  - most optimizations are done at this level

#### Optimizations
- **Inlining**
  - replacing the call site with the body of the called function
- **Hidden class**
  - js is a prototype-based language: there are **no classes* and object are created using a cloning process
  - js is dynamic: properties can be easily added or removed from an object after its instantiation
    - most js interpreters use dictionary-like structures (hash function based) to store the location of object property values in memory
    - makes retrieving the value of a property more expensive than in non-dynamic languages (Java, C#)
  - V8 uses a different method instead: **hidden classes**
    - like fixed object layouts in languages like Java, but created at runtime
    - uses a cached decision tree based on the order you set properties on the object
      - stores memory location offsets
    - **initialize dynamic properties in the same order so that the hidden classes can be reused**
- **Inline Caching**
  - repeated called to the same function tend to occur on the same type of object
  - V8 maintains a cache of the type of objects that were passed as a parameter in recent function calls
    - uses this info to make an assumption about the type of object that will be passed as a parameter in the future
    - => can use hidden class to figure out how to access the object's properties
  - after two successful calls of the same method to the same hidden class, V8 omits the hidden class lookup and simply adds the offset of the property to the object pointer itself
    - for all future calls of that function, V8 engine **assumes** that the hidden class hasn't changed
    - can jump right to the memory address for a specific property using the offsets stored from previous lookups
  - **important to ensure the same hidden class is created for the same types so this caching can be used effectively**
- **Compilation to Machine Code**
  - Once the Hydrogen graph is optimized, Crankshaft lowers it to a lower-level representation called Lithium
  - Most of Lithium implementation is architecture-specific
    - register allocation happens at this level
  - Lithium is compiled into machine code
  - Once machine code is compiled, can transform the context we have (stack registers) while running
    - **we can switch to the optimized version in the middle of execution**
  - NOTE: safeguards called deoptimizations to make the opposite transformation in case an assumption the engine made doesn't hold true anymore
- **Garbage Collection**
  - V8 uses traditional mark-and-sweep approach
    - marking phase normally supposed to stop main thread execution
    - in order to control GC costs, V8 uses incremental marking
      - only walk part of the heap, then resume main thread
      - next GC stop will continue from where the previous left off
- **Ignition and TurboFan**
  - new pipeline in V8 5.0 (released earlier in 2017)
  - achieves even bigger performance improvements and significant memory savings **in real-world** js
  - full-codegen & Crankshaft no longer used
  - designed to be more simple and more maintainable for adapting to new js features to come in the future
  - **Ignition**: V8's interpreter
  - **TurboFan**: V8's newest optimization compiler

### How to write optimized JavaScript
1) **Order of object properties**: always instantiate your object properties in the same order so that hidden classes, and subsequently optimized code, can be shared
2) **Dynamic properties**: adding properties to an object after instantiation will force a hidden class change and slow down any methods that were optimized for the previous hidden class. Instead, assign all of an object's properties in its constructor.
3) **Functions**: code that executes the same method repeatedly will fun faster than code that executes many different functions only once (due to inline caching)
4) **Arrays**: avoid sparse arrays where keys are not incremental numbers. Sparse arrays which don't have every element inside them are a hash table. Elements in such arrays are more expensive to access. Also, try to avoid pre-allocating large arrays. It's better to grow as you go. Finally, don't delete elements in arrays. It makes the keys sparse.
5) **Tagged values**: V8 represents object and numbers with 32 bits. It uses a bit to know if it is an object (flag = 1) or an integer (flag = 0) called SMI (SMall Integer) because of its 31 bits. Then, if a numeric value is bigger than 31 bits, V8 will box the number, turning it into a double and creating a new object to put the number inside. Try to use 31 bit signed numbers whenever possible to avoid the expensive boxing operation into a JS object.
