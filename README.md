
> Based on **[Eloquent JavaScript](https://eloquentjavascript.net/index.html)** by [Marijn Haverbeke](https://github.com/marijnh/Eloquent-JavaScript).

## 01. Values, Types, and Operators

- Special numbers: `Infinity`, `-Infinity`, `NaN`
    - `NaN` is not equal to itself
        - `NaN == NaN` → `false`
        - `NaN === NaN` → `false`
    - Arithmetic operations on `NaN` produce `NaN`
        - `NaN * 2` → `NaN`
    - `Number.isNaN()` returns `true` only if argument given is `NaN`
        - `Number.isNaN(NaN)` → `true`
  {: .force-newline}

- String quotes: `""`, `''`, ``` `` ```
    - ``` `` ```(template literals) can span multiple lines and embed expressions
        - `` `half of 100 is ${100 / 2}` `` → `"half of 100 is 50"`
  {: .force-newline}

- `typeof` operator
    - `typeof 4.5` → `"number"`
  {: .force-newline}

- Empty values: `null`, `undefined`
    - `null` & `undefined` don't have properties
        - `null.toString()` → TypeError
    - Cf.
        - `typeof null` → `"object"` *(officially recognized error, kept for compatibility)*
        - `typeof undefined` → `"undefined"`
  {: .force-newline}

- Automatic type conversion
    - `8 * null` → `0`
    - `"5" + 1` → `"51"`
    - `"5" - 1` → `4`
    - `"five" * 2` → `NaN`
    - `false == 0` → `true`
  {: .force-newline}

- Explicit type conversion
    - `Number("32")` → `32`
    - `String(23)` → `"23"`
    - `Boolean(1)` → `true`
  {: .force-newline}

- `==`/`!=` operators
    - If either side is `null` or `undefined`, equality holds only when both are `null` or `undefined`
        - `null == undefined` → `true`
        - `null == 0` → `false`
    - `0` == `""` == `false`
  {: .force-newline}

- `===`/`!==` operators (precise equality)
    - `0` !== `""` !== `false`
  {: .force-newline}

- Fall back to a default value using `||`
    - `null || "abc"` → `"abc"`
        - works with `null`, `undefined`, `NaN`, `0`, `""`, `false`
  {: .force-newline}

## 02. Program Structure

- JavaScript keywords (FYI)
  ```
  break case catch class const continue debugger default
  delete do else enum export extends false finally for
  function if implements import interface in instanceof let
  new package private protected public return static super
  switch this throw true try typeof var void while with yield
  ```
  {: .force-newline}

- I/O functions (for debugging)
  - prompt
    ```js
    prompt("Enter passcode");
    ```
  - console.log
    ```js
    console.log("The year is", 2020);
    // → The year is 2020
    ```

- Scope (`let` vs `var`)
  - Bindings declared with `let` and `const` are local to the block they are declared in. In pre-2015 JavaScript, only *functions* created new scopes, so old-style bindings created with `var` are visible throughout the whole function (or global scope).
    ```js
    let x = 1;
    if (true) {
      let y = 2;
      var z = 3;

      console.log(x + y + z);
      // → 6
    }

    // y is not visible here
    console.log(x + z);
    // → 4
    ```

## 03. Functions

- 3 ways to create functions
  ```js
  // Function assignment
  const f = function(a) {
    console.log(a + 2);
  };

  // Function declaration
  function g(a, b) {
    return a * b * 3.5;
  }

  // Arrow functions
  let h = a => a % 3;
  ```
  {: .force-newline}

- Subtlety of the **function declaration** notation: \\
  Function declarations are conceptually moved to the top of their scope, and can be used by all code in that scope.
  ```js
  console.log("The future says:", future());
  // → The future says: You'll never have flying cars

  function future() {
    return "You'll never have flying cars";
  }
  ```
  {: .force-newline}

- More on **arrow functions**
  ```js
  const square1 = (x) => { return x * x; };
  const square2 = x => x * x;
  console.log(square1(3), square2(3));
  // → 9 9

  const horn = () => { console.log("Toot"); };
  horn();
  // → Toot
  ```
  {: .force-newline}

- Optional arguments: \\
  Pass too many, extras are ignored. Pass too few, missing parameters get assigned `undefined`.
  ```js
  function shout(x) { return x + "!"; }

  console.log(shout(4, true, "hedgehog"));
  // → 4!
  console.log(shout());
  // → undefined!
  ```
  {: .force-newline}

- Default arguments
  ```js
  function power(base, exponent = 2) {
    let result = 1;
    for (let count = 0; count < exponent; count++) {
      result *= base;
    }
    return result;
  }

  console.log(power(4));
  // → 16
  console.log(power(2, 6));
  // → 64
  ```
  {: .force-newline}

- Rest parameters: \\
  The *rest parameter* is bound to an array containing all further arguments. If there are other parameters before it, their values aren't part of that array.
  ```js
  function max(...numbers) {
    let result = -Infinity;
    for (let number of numbers) {
      if (number > result) result = number;
    }
    return result;
  }
  console.log(max(4, 1, 9, -2));
  // → 9
  ```
  {: .force-newline}

- Cf. Use `...` operator to spread array values
  ```js
  let words = ["will", "always"];
  console.log(...["I", ...words, "remember"]);
  // → I will always remember
  ```

## 04. Data Structures: Objects and Arrays

- Objects and properties
  ```js
  let obj = {
    property: 1,
    "prop 2": 2,
    "¯\_(ツ)_/¯": 3
  };
  ```
  {: .force-newline}

- Shorthand notation
  ```js
  let events = ["weekend", "movies", "karaoke", "pizza", "beer"];
  let happiness = 10;

  let today = {events, happiness};
  ```
  {: .force-newline}

- 2 ways to access properties
  - `array.length`
  - `array["length"]`
  {: .force-newline}

- Arrays are objects too, with numbers as property names (i.e. indices).
  - `typeof []` → `"object"`
  {: .force-newline}

- Reading a property that doesn't exist will give you the value `undefined`.

- Mutability
  - Immutable: numbers, strings, Booleans
    ```js
    let kim = "Kim";
    kim.age = 88;
    console.log(kim.age);
    // → undefined
    ```
  - Mutable: objects (including arrays)
    ```js
    let anObject = {left: 1, right: 2};
    console.log(anObject.left);
    // → 1
    delete anObject.left;
    console.log(anObject.left);
    // → undefined
    console.log("left" in anObject);
    // → false
    console.log("right" in anObject);
    // → true
    ```

- More on `==`/`===` operators
  - Immutable types
    ```js
    let s1 = "Hello, world!";
    let s2 = "Hello, world!";
    console.log(s1 == s2);
    // → true
    console.log(s1 === s2);
    // → true
    ```
  - Mutable objects
    ```js
    let object1 = {value: 10};
    let object2 = {value: 10};
    let object3 = object1;

    console.log(object1 == object2);
    // → false
    console.log(object1 === object2);
    // → false
    console.log(object1 == object3);
    // → true
    console.log(object1 === object3);
    // → true
    ```

- Helpful methods
  - Strings
    ```js
    string.toUpperCase()
    string.toLowerCase()
    string.trim()
    string.repeat(n)
    string.padStart(n, char)
    ```
  - Arrays
    ```js
    array.push(x)     // append x to end
    array.pop()       // pop from end
    array.unshift(x)  // append x to front
    array.shift()     // pop from front
    ```
  - Strings & Arrays
    ```js
    obj.includes(x)
    obj.indexOf(x[, from])
    obj.lastIndexOf(x[, from])

    obj.slice([start[, end]])  // start: inclusive, end: exclusive
    obj1.concat(obj2)          // returns new instance

    string.split(char)  // returns array
    array.join(char)    // returns string
    ```

- Helpful functions regarding objects
  - Object.keys
    ```js
    console.log(Object.keys({x: 0, y: 0, z: 2}));
    // → ["x", "y", "z"]
    ```
  - Object.assign
    ```js
    let obj = {a: 1, b: 1};
    Object.assign(obj, {b: 2, c: 2});
    console.log(obj);
    // → {a: 1, b: 2, c: 2}
    ```

- The `Math` object
  - `Math.max(...)`
  - `Math.min(...)`
  - `Math.sqrt(x)`
  - `Math.PI`
  - `Math.random()` → \[0, 1)
  - `Math.round(x)`
  - `Math.ceil(x)`
  - `Math.floor(x)`
  - `Math.abs(x)`
  {: .force-newline}

- JSON
  - JSON.stringify *(= serialize)*
    ```js
    let object = {events: ["work"], happiness: 5};
    console.log(JSON.stringify(object));
    // → {"events":["work"],"happiness":5}
    ```
  - JSON.parse
    ```js
    let string = '{"events":["work"],"happiness":5}';
    console.log(JSON.parse(string).happiness);
    // → 5
    ```

- Destructuring
  - Using [ ]
    ```js
    let [_, second] = [1, 2, 3, 4, 5];
    console.log(second);
    // → 2
    ```
  - Using { }
    ```js
    function print_name({name}) {
      console.log(name);
    }
    print_name({name: "Jethro", age: 25});
    // → Jethro
    ```

## 05. Higher-Order Functions

- Functions that create new functions
  ```js
  function greaterThan(n) {
    return m => m > n;
  }
  let greaterThan10 = greaterThan(10);
  console.log(greaterThan10(11));
  // → true
  ```
  {: .force-newline}

- Functions that change other functions
  ```js
  function noisy(f) {
    return (...args) => {
      console.log("calling with", args);
      let result = f(...args);
      console.log("returned", result);
      return result;
    };
  }
  noisy(Math.min)(3, 2, 1);
  // → calling with [3, 2, 1]
  // → returned 1
  ```
  {: .force-newline}

- Functions that provide new types of control flow
  ```js
  function unless(test, then) {
    if (!test) then();
  }

  [1, 2, 3, 4, 5].forEach(n => {
    unless(n % 2 == 0, () => {
      console.log(n, "is odd");
    });
  });
  // → 1 is odd
  // → 3 is odd
  // → 5 is odd
  ```
  {: .force-newline}

- forEach
  ```js
  ["A", "B"].forEach(l => console.log(l));
  // → A
  // → B
  ```
  {: .force-newline}

- filter
  - `[1, 2, 3, 4, 5].filter(n => n % 2)` → `[1, 3, 5]`
  {: .force-newline}

- map
  - `[1, 2, 3, 4, 5].map(n => n * 2)` → `[2, 4, 6, 8, 10]`
  {: .force-newline}

- reduce
  - `[1, 2, 3, 4, 5].reduce((a, b) => a + b)` → `15`
  - `[1, 2, 3, 4, 5].reduce((a, b) => a + b, 10000)` → `10015`
  {: .force-newline}

## (Misc.) JavaScript strings use UTF-16 format

JavaScript strings are encoded as a sequence of 16-bit numbers. These are called *code units*. A Unicode character code was initially supposed to fit within such a unit (which gives you a little over 65,000 characters). When it became clear that wasn’t going to be enough, many people balked at the need to use more memory per character. To address these concerns, UTF-16, the format used by JavaScript strings, was invented. It describes most common characters using a single 16-bit code unit but uses a pair of two such units for others.

UTF-16 is generally considered a bad idea today. It seems almost intentionally designed to invite mistakes. It’s easy to write programs that pretend code units and characters are the same thing. And if your language doesn’t use two-unit characters, that will appear to work just fine. But as soon as someone tries to use such a program with some less common Chinese characters, it breaks. Fortunately, with the advent of emoji, everybody has started using two-unit characters, and the burden of dealing with such problems is more fairly distributed.

Unfortunately, obvious operations on JavaScript strings, such as getting their length through the `length` property and accessing their content using square brackets, deal only with code units.

```js
// Two emoji characters, horse and shoe
let horseShoe = "🐴👟";
console.log(horseShoe.length);
// → 4
console.log(horseShoe[0]);
// → (Invalid half-character)
console.log(horseShoe.charCodeAt(0));
// → 55357 (Code of the half-character)
console.log(horseShoe.codePointAt(0));
// → 128052 (Actual code for horse emoji)
```

JavaScript’s `charCodeAt` method gives you a code unit, not a full character code. The `codePointAt` method, added later, does give a full Unicode character. So we could use that to get characters from a string. But the argument passed to `codePointAt` is still an index into the sequence of code units. So to run over all characters in a string, we’d still need to deal with the question of whether a character takes up one or two code units.

Luckily, the `for/of` loop was introduced at a time where people were acutely aware of the problems with UTF-16. When you use it to loop over a string, it gives you real characters, not code units.

```js
let roseDragon = "🌹🐉";
for (let char of roseDragon) {
  console.log(char);
}
// → 🌹
// → 🐉
```

If you have a single character (a string of one or two code units), you can use `codePointAt(0)` to get its code.

## 06. The Secret Life of Objects

### Encapsulation

- In JavaScript, it is common to put an underscore (`_`) character at the start of property names to indicate that those properties are *private*. (Although this doesn't actually prevent anyone from accessing them.)

### Methods

- Methods are nothing more than properties that hold function values.
  ```js
  let rabbit = {};
  rabbit.speak = function(line) {
    console.log(`The rabbit says '${line}'`);
  };

  rabbit.speak("I'm alive.");
  // → The rabbit says 'I'm alive.'
  ```
  {: .force-newline}

- When a function is called as a method, the binding called `this` in its body automatically points at the object that it was called on.
  ```js
  function speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
  let whiteRabbit = {type: "white", speak};
  let hungryRabbit = {type: "hungry", speak};

  whiteRabbit.speak("Oh my ears and whiskers, how late it's getting!");
  // → The white rabbit says 'Oh my ears and whiskers, how late it's getting!'
  hungryRabbit.speak("I could use a carrot right now.");
  // → The hungry rabbit says 'I could use a carrot right now.'
  ```
  {: .force-newline}

- You can think of `this` as an extra parameter that is passed in a different way. If you want to pass it explicitly, use the function’s `call` method, which takes the `this` value as its first argument and treats further arguments as normal parameters.
  ```js
  speak.call({type: "well-eaten"}, "Burp!");
  // → The well-eaten rabbit says 'Burp!'
  ```
  {: .force-newline}

- Since each function has its own `this` binding, whose value depends on the way it is called, you cannot refer to the `this` of the wrapping scope in a regular function defined with the `function` keyword. *But arrow functions are different*—they do not bind their own `this` but can see the `this` binding of the scope around them.
  ```js
  function normalize() {
    console.log(this.coords.map(n => n / this.length));
  }
  normalize.call({coords: [0, 2, 3], length: 10});
  // → [0, 0.2, 0.3]
  ```
  {: .force-newline}

### Prototypes

- Objects derive from a *prototype*. A prototype is another object that is used as a fallback source of properties. When an object gets a request for a property that it does not have, its prototype will be searched for the property, then the prototype’s prototype, and so on.
  ```js
  let empty = {};
  console.log(empty.toString);
  // → function toString(){…}
  console.log(empty.toString());
  // → [object Object]
  ```
  {: .force-newline}

- `Object.prototype` is the great ancestral prototype, the entity behind almost all objects.
  ```js
  console.log(Object.getPrototypeOf({}) == Object.prototype);
  // → true
  console.log(Object.getPrototypeOf(Object.prototype) == null);
  // → true
  ```
  {: .force-newline}

- Arrays derive from `Array.prototype`, and functions derive from `Function.prototype`.
  ```js
  console.log(Object.getPrototypeOf([]) == Array.prototype);
  // → true
  console.log(Object.getPrototypeOf(Math.max) == Function.prototype);
  // → true

  console.log(Object.getPrototypeOf(Array.prototype) == Object.prototype);
  // → true
  console.log(Object.getPrototypeOf(Function.prototype) == Object.prototype);
  // → true
  ```
  {: .force-newline}

### Classes

- Class notation *(post-2015)*
  ```js
  class Rabbit {
    constructor(type) {
      this.type = type;
    }
    speak(line) {
      console.log(`The ${this.type} rabbit says '${line}'`);
    }
  }

  let killerRabbit = new Rabbit("killer");
  let blackRabbit = new Rabbit("black");
  ```
  {: .force-newline}

- In JavaScript, instantiating a class means you're creating an object out of the constructor's prototype. (The constructor method is bound to the name of the class.)
  ```js
  console.log(Object.getPrototypeOf(blackRabbit) == Rabbit.prototype);
  // → true

  console.log(Object.getPrototypeOf(Rabbit) == Function.prototype);
  // → true
  ```
  {: .force-newline}

- Class declarations currently allow only *methods* to be added to the prototype. Add non-function properties or *attributes* by directly manipulating the prototype after defining the class.
  ```js
  Rabbit.prototype.teeth = "small";

  console.log(blackRabbit.teeth);
  // → small
  ```
  {: .force-newline}

- As in the example above, beware that *attributes added to the class after instance creation will still affect previously created instances.* (This is because properties not found in an object are looked for in its prototype.)
  {: .force-newline}

- Dynamic type creation
  ```js
  let obj = new class { getWord() { return "hello"; } };
  console.log(obj.getWord());
  // → hello
  ```
  {: .force-newline}

### Overriding derived properties

- Overriding attributes
  ```js
  killerRabbit.teeth = "long, sharp, and bloody";
  console.log(killerRabbit.teeth);
  // → long, sharp, and bloody
  console.log(blackRabbit.teeth);
  // → small
  ```
  {: .force-newline}

- Overriding methods
  ```js
  console.log(Array.prototype.toString == Object.prototype.toString);
  // → false
  console.log([1, 2].toString());
  // → 1,2
  ```
  {: .force-newline}

- Calling a property of the prototype
  ```js
  console.log(killerRabbit.teeth);
  // → long, sharp, and bloody
  console.log(Object.getPrototypeOf(killerRabbit).teeth);
  // → small
  ```
  {: .force-newline}

### Maps

- A *map* (noun) is a data structure that associates keys with values. (For example, you might want to map names to ages.) It is possible to use objects for this, although there is a liability regarding derived properties.
  ```js
  let ages = {
    Boris: 39,
    Liang: 22,
    Julia: 62
  };

  console.log(`Julia is ${ages["Julia"]}`);
  // → Julia is 62

  console.log("Jack" in ages);
  // → false
  console.log("toString" in ages);
  // → true
  ```
  {: .force-newline}

- There are several possible ways to avoid this problem. First, as an alternative to the `in` operator, you can use the `hasOwnProperty` method, which ignores the object’s prototype.
  ```js
  console.log(ages.hasOwnProperty("Jack"));
  // → false
  console.log(ages.hasOwnProperty("toString"));
  // → false
  ```
  {: .force-newline}

- It’s also useful to know that `Object.keys` returns only an object’s *own* keys, not those in the prototype.
  ```js
  console.log(Object.keys(ages));
  // → ["Boris", "Liang", "Julia"]
  ```
  {: .force-newline}

- However, you may rather want to create an object with *no* prototype. If you pass `null` to `Object.create`, the resulting object will not derive from `Object.prototype` and can safely be used as a map.
  ```js
  console.log("toString" in Object.create(null));
  // → false
  ```
  {: .force-newline}

- BTW, you can use `Object.create` to create an object using some other object as a prototype.
  ```js
  let obj = Object.create({a: 1, b: 2, c: 3, f: Math.max});
  console.log(obj.f(obj.a, obj.b, obj.c));
  // → 3
  ```
  {: .force-newline}

- JavaScript ships with a class called *Map*, that is interfaced with methods: `get`, `set`, `has`, etc.
  ```js
  let ages = new Map();
  ages.set("Boris", 39);
  ages.set("Liang", 22);
  ages.set("Julia", 62);

  console.log(`Julia is ${ages.get("Julia")}`);
  // → Julia is 62
  console.log(ages.has("Jack"));
  // → false
  console.log(ages.has("toString"));
  // → false
  ```
  {: .force-newline}

### Polymorphism

- Calling the `String` function (which converts a value to a string) on an object will call the `toString` method on that object to try to create a meaningful string from it.
  ```js
  Rabbit.prototype.toString = function() {
    return `a ${this.type} rabbit`;
  };

  console.log(String(blackRabbit));
  // → a black rabbit
  ```
  {: .force-newline}

### Symbols

- NOTE: Property names can be strings *or symbols*.
  {: .force-newline}

- Symbols are values created with the `Symbol` function. Unlike strings, newly created symbols are unique—you cannot create the same symbol twice.
  ```js
  let sym = Symbol("symbol name");
  console.log(sym == Symbol("symbol name"));
  // → false

  Rabbit.prototype[sym] = 55;
  console.log(blackRabbit[sym]);
  // → 55
  ```
  {: .force-newline}

- The string you pass to `Symbol` is included when you convert it to a string and can make it easier to recognize a symbol when, for example, showing it in the console. But it has no meaning beyond that—multiple symbols may have the same name.
  ```js
  console.log(sym.toString());
  // → Symbol(symbol name)
  ```
  {: .force-newline}

- Symbol usages
  ```js
  const toStringSymbol = Symbol("toString");
  Array.prototype[toStringSymbol] = function() {
    return `${this.length} cm of blue yarn`;
  };
  console.log([1, 2].toString());
  // → 1,2
  console.log([1, 2][toStringSymbol]());
  // → 2 cm of blue yarn

  let stringObject = {
    [toStringSymbol]() { return "a jute rope"; }
  };
  console.log(stringObject[toStringSymbol]());
  // → a jute rope
  ```
  {: .force-newline}

### The iterator interface

- An object given to a `for/of` loop is expected to be *iterable*. This means it has a method named with the `Symbol.iterator` symbol (a symbol value defined by the language, stored as a property of the `Symbol` function).
  When called, that method should return an *iterator* object—the actual thing that iterates. It has a `next` method that returns the next result. That result should be an object with a `value` property that provides the next value, if there is one, and a `done` property, which should be true when there are no more results and false otherwise.
  (Note that the `next`, `value`, and `done` property names are plain strings, not symbols. Only `Symbol.iterator`, which is likely to be added to a lot of different objects, is an actual symbol.)

- Example usage
  ```js
  let okIterator = "OK"[Symbol.iterator]();
  console.log(okIterator.next());
  // → {value: "O", done: false}
  console.log(okIterator.next());
  // → {value: "K", done: false}
  console.log(okIterator.next());
  // → {value: undefined, done: true}
  ```
  {: .force-newline}

- Custom implementation of an iterable data-structure
  ```js
  class Matrix {
    constructor(width, height, element = (x, y) => undefined) {
      this.width = width;
      this.height = height;
      this.content = [];

      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          this.content[y * width + x] = element(x, y);
        }
      }
    }

    get(x, y) {
      return this.content[y * this.width + x];
    }
    set(x, y, value) {
      this.content[y * this.width + x] = value;
    }

    // iterator interface
    [Symbol.iterator]() {
      return new class {
        constructor(matrix) {
          this.x = 0;
          this.y = 0;
          this.matrix = matrix;
        }

        next() {
          if (this.y == this.matrix.height) return {done: true};

          let value = {x: this.x,
                       y: this.y,
                       value: this.matrix.get(this.x, this.y)};
          this.x++;
          if (this.x == this.matrix.width) {
            this.x = 0;
            this.y++;
          }
          return {value, done: false};
        }
      }(this);
    }
  }

  let matrix = new Matrix(2, 2, (x, y) => `value ${x},${y}`);
  for (let {x, y, value} of matrix) {
    console.log(x, y, value);
  }
  // → 0 0 value 0,0
  // → 1 0 value 1,0
  // → 0 1 value 0,1
  // → 1 1 value 1,1
  ```
  {: .force-newline}

### Getters, setters, and statics

- Getters are properties that hide a method call. They are defined by writing `get` in front of the method name in an object expression or class declaration.
  ```js
  let varyingSize = {
    get size() {
      return Math.floor(Math.random() * 100);
    }
  };

  console.log(varyingSize.size);
  // → 73
  console.log(varyingSize.size);
  // → 49
  ```
  {: .force-newline}

- Whenever an object’s *getter* property is read or *setter* property written to, the associated method is called. For example, the following `Temperature` class allows reading and writing temperature in either degrees Celsius or degrees Fahrenheit, but internally only stores the value in degrees Celsius while converting to and from Celsius in the `fahrenheit` getter and setter methods.
  ```js
  class Temperature {
    constructor(celsius) {
      this.celsius = celsius;
    }
    get fahrenheit() {
      return this.celsius * 1.8 + 32;
    }
    set fahrenheit(value) {
      this.celsius = (value - 32) / 1.8;
    }

    static fromFahrenheit(value) {
      return new Temperature((value - 32) / 1.8);
    }
  }

  let temp = new Temperature(22);
  console.log(temp.celsius);
  // → 22
  console.log(temp.fahrenheit);
  // → 71.6

  temp.fahrenheit = 86;
  console.log(temp.celsius);
  // → 30
  console.log(temp.fahrenheit);
  // → 86
  ```
  {: .force-newline}

- Inside a class declaration, methods that have `static` written before their name are stored on the constructor. So the `Temperature` class above allows you to write `Temperature.fromFahrenheit(100)` to initialize a temperature using degrees Fahrenheit. (Unlike Java, static methods are not passed on to the individual class instances.)

### Inheritance

- Via the `extends` keyword
  ```js
  class SymmetricMatrix extends Matrix {
    constructor(size, element = (x, y) => undefined) {
      super(size, size, (x, y) => {
        if (x < y) return element(y, x);
        else return element(x, y);
      });
    }

    set(x, y, value) {
      super.set(x, y, value);
      if (x != y) {
        super.set(y, x, value);
      }
    }
  }

  let matrix = new SymmetricMatrix(5, (x, y) => `${x},${y}`);
  console.log(matrix.get(2, 3));
  // → 3,2
  ```
  {: .force-newline}

### The instanceof operator

- `instanceof` (binary operator)
  ```js
  console.log(new SymmetricMatrix(2) instanceof SymmetricMatrix);
  // → true
  console.log(new SymmetricMatrix(2) instanceof Matrix);
  // → true
  console.log(new Matrix(2, 2) instanceof SymmetricMatrix);
  // → false
  ```
  {: .force-newline}

## [07. Project: A Robot](https://eloquentjavascript.net/07_robot.html)

> Our project in this chapter is to build an automaton, a little program that performs a task in a virtual world. Our automaton will be a mail-delivery robot picking up and dropping off parcels.

## (Misc.) Additional takeaways

- Object.freeze
  ```js
  let object = Object.freeze({value: 5});
  object.value = 10;
  console.log(object.value);
  // → 5
  ```
  {: .force-newline}
