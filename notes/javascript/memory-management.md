# Memory Management
From MDN web docs [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

- JavaScript is garbage collected, like java
- not done by reference counting, because cycles
- done with mark-and-sweep algorithm
  - "an object is unreachable"
  - crawl from the global object (roots) to find all reachable objects and release non-reachable objects
  - as of 2012, all modern browsers ship with a mark-and-sweep garbage-collector
