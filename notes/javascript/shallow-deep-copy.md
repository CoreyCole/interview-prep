# Understanding Deep and Shallow Copy in Javascript
From medium post [here](https://we-are.bookmyshow.com/understanding-deep-and-shallow-copy-in-javascript-13438bad941c)

## Shallow Copy
Object properties are copied as references

## Deep Copy
Object properties are copied. This occurs when an object is copied along with the object to which it refers.

## Example
```javascript
var employeeDetailsOriginal = {
  name: 'Manjula',
  age: 25,
  profession: 'Software Engineer'
};
// shallow copy
var employeeDetailsShallow = employeeDetailsOriginal;
// deep copy
var employeeDetailsDeep = {
  name: employeeDetailsOriginal.name,
  age: employeeDetailsOriginal.age,
  profession: employeeDetailsOriginal.profession
};
```

## New features
### Object.assign()
- Only does shallow clone
- For deep cloning, we need to use other alternatives because Object.assign() copies property values
  - If the source value is a reference to an object, it only copies that reference value