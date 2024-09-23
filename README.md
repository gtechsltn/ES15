# ECMAScript 2024 (ES15): Unveiling the Latest JavaScript Features

ECMAScript 2024 (ES15): Unveiling the Latest JavaScript Features

https://javascript.plainenglish.io/ecmascript-2024-es15-unveiling-the-latest-javascript-features-9186d72a10ae

## 1. Improved Unicode String Handling
```
const textSamples = [  
  "example\uD800text",   // Invalid surrogate pair  
  "good\uDC00morning",   // Trailing surrogate  
  "welcome",             // Well-formed string  
  "emoji\uD83D\uDE00"    // Complete surrogate pair  
];  
  
textSamples.forEach(str => {  
  console.log(`Original: ${str}, Well-formed: ${str.toWellFormed()}`);  
});
```


## 2. Synchronized Atomic Operations
```
const sharedBuffer = new Int32Array(new SharedArrayBuffer(1024));  
function synchronizedUpdate(index, value) {  
    Atomics.waitSync(sharedBuffer, index, 0);  
    sharedBuffer[index] = value;  
    Atomics.notify(sharedBuffer, index, 1);  
}  
synchronizedUpdate(0, 42);
```


## 3. Improved Regular Expressions with the v Flag
```
const regex = /\p{Script=Latin}\p{Mark}*\p{Letter}/v;  
const text = "áéíóú";  
  
console.log(text.match(regex)); // Matches accented letters
```


## 4. Top-Level await
```
const response = await fetch('https://api.example.com/data');  
const jsonData = await response.json();  
console.log(jsonData);
```


## 5. Pipeline Operator (|>)
```
const result = [1, 2, 3, 4, 5]  
  |> (_ => _.map(n => n * 2))
  |> (_ => _.filter(n => n > 5))
  |> (_ => _.reduce((acc, n) => acc + n, 0));

console.log(result); // 18
```

## 6. Records and Tuples
```
const user = #{ name: "Alice", age: 30 };  
const updatedUser = user.with({ age: 31 });  
  
console.log(updatedUser); // #{ name: "Alice", age: 31 }  
console.log(user); // #{ name: "Alice", age: 30 }  
  
const coordinates = #[10, 20];  
const newCoordinates = coordinates.with(1, 25);  
  
console.log(newCoordinates); // #[10, 25]  
console.log(coordinates); // #[10, 20]
```

## 7. Decorators
```
function log(target, key, descriptor) {  
  const originalMethod = descriptor.value;  
  descriptor.value = function(...args) {  
    console.log(`Calling ${key} with`, args);  
    return originalMethod.apply(this, args);  
  };  
  return descriptor;  
}  
  
class MathOperations {  
  @log  
  add(a, b) {  
    return a + b;  
  }  
}  
  
const math = new MathOperations();  
console.log(math.add(5, 3)); // Logs: Calling add with [5, 3] and returns 8
```


## 8. Array Grouping by groupBy and groupByToMap
```
const fruits = [  
  { name: 'Apple', color: 'Red' },  
  { name: 'Banana', color: 'Yellow' },  
  { name: 'Cherry', color: 'Red' },  
  { name: 'Lemon', color: 'Yellow' },  
];  
  
const groupedByColor = fruits.groupBy(fruit => fruit.color);  
console.log(groupedByColor);  
/*  
{  
  Red: [{ name: 'Apple', color: 'Red' }, { name: 'Cherry', color: 'Red' }],  
  Yellow: [{ name: 'Banana', color: 'Yellow' }, { name: 'Lemon', color: 'Yellow' }]  
}  
*/
```


## 9. Temporal API for Date and Time Handling
```
const now = Temporal.Now.plainDateISO();  
const nextMonth = now.add({ months: 1 });  
  
console.log(now.toString()); // Current date  
console.log(nextMonth.toString()); // Date one month from now
```


## 10. RegExp Match Indices
```
const regex = /(\d+)/d;  
const input = "The answer is 42";  
const match = regex.exec(input);  
  
console.log(match.indices); // [[14, 16], [14, 16]]
```


## 11. Enhanced Error Cause
```
try {  
  try {  
    throw new Error('Initial error');  
  } catch (err) {  
    throw new Error('Secondary error', { cause: err });  
  }  
} catch (err) {  
  console.error(err.message); // Secondary error  
  console.error(err.cause.message); // Initial error  
}
```


## 12. Symbol.prototype.description
```
const mySymbol = Symbol('uniqueSymbol');  
console.log(mySymbol.description); // 'uniqueSymbol'
```


## 13. Logical Assignment Operators
```
let username = null;  
let defaultUsername = "Guest";  
  
username ||= defaultUsername;  
console.log(username); // 'Guest'  
  
let isVerified = false;  
isVerified &&= true;  
console.log(isVerified); // false  
  
let value = undefined;  
value ??= 100;  
console.log(value); // 100
```


## 14. Class Fields Enhancement
```
class Counter {  
  static #count = 0;  
  
  static increment() {  
    this.#count++;  
  }  
  
  static getCount() {  
    return this.#count;  
  }  
}  
  
Counter.increment();  
console.log(Counter.getCount()); // 1
```


## 15. WeakRefs and Finalization Registry
```
const registry = new FinalizationRegistry((heldValue) => {  
  console.log(`Cleaning up ${heldValue}`);  
});  
  
let myObj = { name: "temporary" };  
const weakRef = new WeakRef(myObj);  
  
registry.register(myObj, "myObj cleanup");  
  
myObj = null; // Dereference the object  
// At this point, the registry callback will eventually run
```
