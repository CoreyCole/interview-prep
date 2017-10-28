# Launching Ignition and TurboFan
Blog post from V8 team (May 15, 2017) found [here](https://v8project.blogspot.bg/2017/05/launching-ignition-and-turbofan.html)

- Full-codegen and Crankshaft, the technologies that served V8 well since 2010, are no longer used in V8 for JavaScript execution
  - they no longer are able to keep pace with new JavaScript language features and the optimizations those features require

## A Long Journey
- Ignition and TurboFan have been in development for almost 3.5 years
- carefully considered the shortcomings of full-codegen and Crankshaft
- V8 team used to be maintaining >10k lines of code per chip architecture
- **TurboFan** was designed to optimize for features to come in ES2015 and beyond
  - introduces layered compiler design
  - clean separation between high-level and low-level compiler optimizations
  - easy to add new language features without modifying architecture-specific code
  - TurboFan adds an explicit instruction selection phase
    - enables much less architecture-specific code
    - architecture-specific code is written once and it rarely needs to be changed
- **Ignition Interpreter** originally designed to reduce memory consumption on mobile devices
  - Before Ignition, V8's full-codegen based compiler typically occupied almost 1/3 of the overall js heap in chrome
  - Ignition on Android devices with limited ram **reduced memory footprint for baseline, non-optimized js by a factor of NINE** (ARM64-based mobile devices)
  - Coupled design of Ignition and TurboFan closely to maximize performance optimizations
  - **Taking advantage of Ignition's bytecode:**
    - to generate optimized machine code with TurboFan directly rather than having to re-compile from source code as Crankshaft did
      - get a cleaner and less error-prone baseline execution model
    - simplify the deoptimization mechanism that is a key feature of V8's adaptive optimization
    - generally improves script startup times and in turn, web page loads
