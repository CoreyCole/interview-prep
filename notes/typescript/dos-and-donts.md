# Do's and Dont's
From TS docs [here](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)

## General Types
- Don't ever use types `Number`, `String`, `Boolean`, and `Object`
  - They refer to non-primitive boxed objects that are almost never used appropriately in js code
  - Use `number`, `string`, and `boolean` instead

## Use union types
```javascript
/* WRONG */
interface Moment {
    utcOffset(): number;
    utcOffset(b: number): Moment;
    utcOffset(b: string): Moment;
}
/* OK */
interface Moment {
    utcOffset(): number;
    utcOffset(b: number|string): Moment;
}
```