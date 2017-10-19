# A re-introduction to JavaScript (JS tutorial)
From mozilla dev docs [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)
## Types
- Number
- String
- Boolean
- Symbol
- Object
  - Function
  - Array
  - Date
  - RegExp
- null
- undefined

### Numbers
- numbers in js are "double-precision 64-bit format IEEE 754 values
![IEEE-754](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/IEEE_754_Double_Floating_Point_Format.svg/618px-IEEE_754_Double_Floating_Point_Format.svg.png)
- integers stored as 32 bit until an operation is done that is only valid for Number, not int
- using `parseInt()` should always provide base
```javascript
// base 10
parseInt('101', 10) => 10

// octal
parseInt('010')     => 8 // (in some old browsers)

// hex
parseInt('0x10')    => 16

// binary
parseInt('11', 2)   => 3
```
- `parseFloat()` always uses base 10
- `isNaN(NaN) => true`
  - NaN is toxic i.e. `NaN + 5 = NaN`
- `isFinite()`
```javascript
1/0           => Infinity
-1/0          => -Infinity
isFinite(1/0) => false
isFinite(NaN) => false
```
- `parseInt()` and `parseFloat()` parse up until a invalid character
```javascript
parseFloat('12.3abc') => 12.3
```

### Strings
- sequences of unicode characters (UTF-8)
```javascript
'hello'.charAt(0) => "h"
'hello, world'.replace('hello', 'goodbye') => "goodbye, world"
'hello'.toUpperCase() => "HELLO"
'3' + 4 + 5 => "345"
3 + 3 + '5' => "75"
```

### Booleans
- false, 0, empty strings, NaN, null, undefined => false
- everything else => true

### Variables
- let is block scope
- const is block scope (can't be reassigned, but can be mutated)
- var is available from within the *function* it is declared in
  - in js block do not have scope, only functions

### Operators
- `%` = remainder operator
  - not same as modulo because it always takes the sign of the dividend, not divisor
```javascript
-1%2 => -1
-4%2 => -0
NaN%2 => NaN
```

### Comparisons
- `==` does type coercion
```javascript
123 == '123' => true
1 == true    => true
```

### Bitwise Operators
- treat operands as 32 bit, but return standard is still Number
```javascript
&       // AND
|       // OR
^       // XOR
~       // NOT
a << b
// left shift
// moves b (<32) bits to left, zero from right

a >> b 
// sign propagating right shift
// moves b (<32) bit to right, discarding bits shifted off

a >>> b
// zero fill right shift
// shifts zeros in from left
```
#### examples
```javascript
9        => 0...1001
9 >> 2   => 0...0010 => 2
-9       => 1...0111
-9 >> 2  => 1...1101 => -3
9 >>> 2  => 0...0010 => 2
-9 >>> 2 => 0...0011 => 3

8  << 3 === 8  * (2^3) === 64
64 >> 3 === 64 / (2^3) === 64
```

### Objects
- everything in js is a hash map, besides core types
- bracket notation prevents js engine and minifier optimizations, use dot instead
```javascript
obj['property'] !== obj.property
```
- can use `for...in` loops over object child properties

### Arrays
- arrays in js are a special type of object
- numerical properties accessed with `[]`
- magical property length is always 1 more than the highest index
- if you query non-existent array index, you'll get `undefined`
```javascript
// toString() of each element separated by commas
arr.toString()

// toString() of each element separated by `sep`
arr.join(sep)

// returns new array with items added on to it
arr.concat(arr2[], arr3[], ... arrN[])

// removes and returns last item
arr.pop()

// removes and returns the first item
a.shift()

// appends to end
arr.push(item1, ..., itemN)

// prepends to the front
arr.unshift(item1, item2, ..., itemN)

// reverses the array
arr.reverse()

// returns a new sub-array
a.slice(start, end?)

// modifies existing array by deleting a section and replacing it with more items
a.splice(start, deleteCount, item1, ..., itemN)

// sorts, takes optional comparison function
a.sort(compFunc())
```

### Functions
- functions hav access to `arguments` which is a list of parameters
```javascript
function add() {
  for (const i; i < arguments.length; i++) {
    sum += arguments[i]
  }
}
```
- "Rest parameter" syntax
```javascript
function add(...args) {
  for (const value of args) {
    sum += value
  }
}
```
  - put other parameters *before* rest syntax `...args`
  - when calling same `add()` function, use "Spread" syntax
```javascript
var arr = [1,2,3,4]
add(...arr)
```

### Custom Objects
#### Constructor Functions
- good style to capitalize these (remember to use `new`)
- `new` creates brand new empty object, constructor function mutates it
- share functions across all objects instead of duplicated code with prototypical inheritance
```javascript
function Person(first, last) {
  this.first = first;
  this.last = last;
}
Person.prototype.fullName = function() {
  return this.first + ' ' + this.last;
};
Person.prototype.fullNameReversed = function() {
  return this.last + ', ' + this.first;
};
Person.prototype.toString = function() {
  return '<Person: ' + this.fullName() + '>';
}
```

### Inner Functions
- nested functions can access variables in their parent function's scope
- can use this technique to avoid polluting the global scope, but avoid global variables if at all possible
```javascript
function parentFunc() {
  var a = 1;

  function nestedFunc() {
    var b = 4; // parentFunc can't use this
    return a + b; 
  }
  return nestedFunc(); // 5
}
```

### Closures
- local variables persist attached to function in scope object
  - functions with preserved data
- returned function makes a reference to that scope object, so garbage collector keeps it
- scope object form a chain just like the prototype chain


