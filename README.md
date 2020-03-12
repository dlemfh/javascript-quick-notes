
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
  - Object.freeze
    ```js
    let obj = Object.freeze({value: 5});
    obj.value = 10;
    console.log(obj.value);
    // → 5
    ```

- Cf. Combining objects using `...`
  ```js
  let o1 = {a: 1, b: 1};
  let o2 = {b: 2, c: 2};
  console.log({...o1, ...o2});
  // → {a: 1, b: 2, c: 2}

  let a1 = [1, 2];
  let a2 = [3, 4];
  console.log([...a1, ...a2]);
  // → [1, 2, 3, 4]
  ```
  {: .force-newline}

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

## 08. Bugs and Errors

### Strict mode

- JavaScript can be made a little stricter by enabling *strict mode*. This is done by putting the string `"use strict"` at the top of a file or a function body.

- Normally, when you forget to put `let` in front of your binding, as in the following example, JavaScript quietly creates a global binding and uses that. In strict mode, an error is reported instead. (Note, however, that this doesn’t work when the binding in question already exists as a global binding. In that case, the loop will still quietly overwrite the value of the binding.)
  ```js
  function canYouSpotTheProblem() {
    "use strict";
    for (counter = 0; counter < 10; counter++) {
      console.log("Happy happy");
    }
  }

  canYouSpotTheProblem();
  // → ReferenceError: counter is not defined
  ```
  {: .force-newline}

- Another change in strict mode is that the `this` binding holds the value `undefined` in functions that are not called as methods. When making such a call outside of strict mode, `this` refers to the global scope object, which is an object whose properties are the global bindings.
  ```js
  function Person(name) { this.name = name; }
  let ferdinand = Person("Ferdinand"); // forgot new
  console.log(name);
  // → Ferdinand
  ```
  vs
  ```js
  "use strict";
  function Person(name) { this.name = name; }
  let ferdinand = Person("Ferdinand"); // forgot new
  // → TypeError: Cannot set property 'name' of undefined
  ```
  {: .force-newline}

### Debugging

- When coding in JavaScript, you can set a *breakpoint* by including a `debugger` statement (consisting of simply that keyword) in your program. If the developer tools of your browser are active, the program will pause whenever it reaches such a statement.

- Most browsers come with the ability to set a breakpoint on a specific line of your code. When the execution of the program reaches a line with a breakpoint, it is paused, and you can inspect the values of bindings at that point.

### Exceptions

- Throwing exceptions (`throw`):
  ```js
  function promptDirection(question) {
    let result = prompt(question);
    if (result.toLowerCase() == "left") return "L";
    if (result.toLowerCase() == "right") return "R";
    throw new Error("Invalid direction: " + result);
  }

  function look() {
    if (promptDirection("Which way?") == "L") {
      return "a house";
    } else {
      return "two angry bears";
    }
  }
  ```
  {: .force-newline}

- Catching exceptions (`try/catch`):
  ```js
  try {
    console.log("You see", look());
  } catch (error) {
    console.log(error);
  }
  ```
  {: .force-newline}

- Error message:
  ```js
  console.log(error);
  // → Error: Invalid direction: Down
  ```
  {: .force-newline}

- Stack trace (stored in `stack` property of caught error):
  ```js
  console.log(error.stack);
  // → Error: Invalid direction: Down
  //     at promptDirection (codef70ca2a6:5:9)
  //     at look (codef70ca2a6:9:7)
  //     at eval (codef70ca2a6:17:26)
  //     ...
  ```
  {: .force-newline}

- When an exception makes it all the way to the bottom of the stack without being caught, it gets handled by the environment. What this means differs between environments. In browsers, a description of the error typically gets written to the JavaScript console (reachable through the browser’s Tools or Developer menu). Node.js is more careful about data corruption. It aborts the whole process when an unhandled exception occurs.

- A `finally` block may follow a `try` block, *instead of* or *in addition to* a `catch` block. It indicates "no matter *what* happens, run this code after trying to run the code in the `try` block." After the `finally` block runs, the stack continues unwinding.
  ```js
  try {
    // ...
  } finally {
    // ...
  }
  ```
  {: .force-newline}

- Writing custom exceptions:
  ```js
  class InputError extends Error {}

  function promptDirection(question) {
    let result = prompt(question);
    if (result.toLowerCase() == "left") return "L";
    if (result.toLowerCase() == "right") return "R";
    throw new InputError("Invalid direction: " + result);
  }
  ```
  {: .force-newline}

- Selectively catching exceptions:
  ```js
  for (;;) {
    try {
      let dir = promptDirection("Where?");
      console.log("You chose", dir);
      break;
    } catch (e) {
      if (e instanceof InputError) {
        console.log("Not a valid direction. Try again.");
      } else {
        throw e;
      }
    }
  }
  ```
  {: .force-newline}

## (Misc.) JavaScript Class for Representing Dates

- Current date and time
  ```js
  console.log(new Date());
  // → Wed Mar 11 2020 16:19:11 GMT+0900 (Korean Standard Time)
  ```
  {: .force-newline}

- Specific date and time
  ```js
  console.log(new Date(2009, 11, 9));
  // → Wed Dec 09 2009 00:00:00 GMT+0100 (CET)
  console.log(new Date(2009, 11, 9, 12, 59, 59, 999));
  // → Wed Dec 09 2009 12:59:59 GMT+0100 (CET)
  ```
  {: .force-newline}

- JavaScript uses a convention where **month numbers start at zero** (so December is 11), yet **day numbers start at one**. This is confusing and silly. **Be careful!!!!!!!** The last four arguments (hours, minutes, seconds, and milliseconds) to `Date` are optional and taken to be zero when not given.

- Timestamp ("Unix time") of current date and time
  ```js
  console.log(Date.now());
  // → 1583856914237
  ```
  {: .force-newline}

- Timestamp to and from `Date` object
  ```js
  console.log(new Date(2013, 11, 19).getTime());
  // → 1387407600000
  console.log(new Date(1387407600000));
  // → Thu Dec 19 2013 00:00:00 GMT+0100 (CET)
  ```
  {: .force-newline}

- `Date` methods
  ```js
  now = new Date();

  console.log(now.getFullYear());
  // → 2020
  console.log(now.getMonth());
  // → 2  (*March*)
  console.log(now.getDate());
  // → 14
  console.log(now.getHours());
  // → 16
  console.log(now.getMinutes());
  // → 20
  console.log(now.getSeconds());
  // → 42
  ```
  {: .force-newline}

## 09. Regular Expressions

- RegExp object
  ```js
  let re1 = new RegExp("abc");
  let re2 = /abc/;

  console.log(re1.source == re2.source);
  // → true
  ```
  {: .force-newline}

- `test`: returns `true/false`
  ```js
  console.log(/abc/.test("0abcde"));
  // → true
  console.log(/abc/.test("0abxde"));
  // → false
  ```
  {: .force-newline}

- `exec`: returns a match object (array) or `null`
  ```js
  let match = /\d+/.exec("one two 100");
  console.log(match);
  // → ["100"]
  console.log(match.index);
  // → 8

  let noMatch = /\d+/.exec("one two");
  console.log(noMatch);
  // → null
  ```
  {: .force-newline}

- Match groups
  ```js
  let m = /(\d{4})-(\d{1,2})-(\d{1,2})/.exec("2020-1-31");
  console.log(m);
  // → ["2020-1-31", "2020", "1", "31"]
  ```
  {: .force-newline}

- `string.match(/regex/)`: same as `/regex/.exec(string)`
  ```js
  let m = "2020-1-31".match(/(\d{4})-(\d{1,2})-(\d{1,2})/);
  console.log(m);
  // → ["2020-1-31", "2020", "1", "31"]
  ```
  {: .force-newline}

- `string.search()`: returns index of match or -1
  ```js
  console.log("  word".search(/\S/));
  // → 2
  console.log("      ".search(/\S/));
  // → -1
  ```
  {: .force-newline}

- `string.replace()`: returns new string
  ```js
  console.log("Borobudur".replace(/[ou]/, "a"));
  // → Barobudur
  console.log("Borobudur".replace(/[ou]/g, "a"));
  // → Barabadar
  ```
  {: .force-newline}

- Referring to matched groups
  ```js
  console.log(
    "Liskov, Barbara\nMcCarthy, John\nWadler, Philip"
      .replace(/(\w+), (\w+)/g, "$2 $1"));
  // → Barbara Liskov
  //   John McCarthy
  //   Philip Wadler

  // Note: The whole match can be referred to with $&.
  ```
  {: .force-newline}

- Matching from a given position
  - Requirement: `exec` method, with global (`g`) or sticky (`y`) option enabled
  - Using: the `lastIndex` property of the `RegExp` object
    ```js
    let pattern = /y/g;
    pattern.lastIndex = 3;

    let match = pattern.exec("xyzzy");
    console.log(match.index);
    // → 4
    console.log(pattern.lastIndex);
    // → 5
    ```
  - If the match was successful, the call to `exec` automatically updates the `lastIndex` property to point after the match. If no match was found, `lastIndex` is set back to zero, which is also the value it has in a newly constructed regular expression object.

- Looping over matches
  ```js
  let input = "A string with 3 numbers in it... 42 and 88.";
  let number = /\b\d+\b/g;

  let match;
  while (match = number.exec(input)) {
    console.log("Found", match[0], "at", match.index);
  }
  // → Found 3 at 14
  //   Found 42 at 33
  //   Found 88 at 40
  ```
  {: .force-newline}

- Word boundary (`\b`)
  ```js
  console.log(/cat/.test("concatenate"));
  // → true
  console.log(/\bcat\b/.test("concatenate"));
  // → false

  // Note: A boundary marker doesn't match an actual character.
  ```
  {: .force-newline}

- Unicode property (`\p`): \\
  Unicode defines a number of useful properties, though finding the one that you need may not always be trivial. You can use the `\p{Property=Value}` notation to match any character that has the given value for that property. If the property name is left off, as in `\p{Name}`, the name is assumed to be either a binary property such as `Alphabetic` or a category such as `Number`.
  ```js
  console.log(/\p{Script=Greek}/u.test("α"));
  // → true
  console.log(/\p{Script=Arabic}/u.test("α"));
  // → false
  console.log(/\p{Alphabetic}/u.test("α"));
  // → true
  console.log(/\p{Alphabetic}/u.test("!"));
  // → false
  ```
  {: .force-newline}

- Flags
  - `i`: case-insensitive
    ```js
    let cartoonCrying = /boo+(hoo+)+/i;
    console.log(cartoonCrying.test("Boohoooohoohooo"));
    // → true
    ```
  - `g`: global
    ```js
    let s = "the cia and fbi";
    console.log(s.replace(/\b(cia|fbi)\b/g,
                str => str.toUpperCase()));
    // → the CIA and FBI
    ```
  - `y`: sticky (no lookahead)
    ```js
    let normal = /abc/;
    console.log(normal.exec("xyz abc"));
    // → ["abc"]
    let sticky = /abc/y;
    console.log(sticky.exec("xyz abc"));
    // → null
    ```
  - `u`: unicode
    ```js
    console.log(/🍎{3}/.test("🍎🍎🍎"));
    // → false
    console.log(/<.>/.test("<🌹>"));
    // → false
    console.log(/<.>/u.test("<🌹>"));
    // → true
    ```
  - Constructing RegExp with flags
    ```js
    let name = "harry";
    let text = "Harry is a suspicious character.";
    let regexp = new RegExp("\\b(" + name + ")\\b", "gi");
    console.log(text.replace(regexp, "_$1_"));
    // → _Harry_ is a suspicious character.
    ```
    {: .force-newline}

- Caveats
  - When using a shared regular expression value for multiple `exec` calls, these automatic updates to the `lastIndex` property can cause problems. Your regular expression might be accidentally starting at an index that was left over from a previous call.
    ```js
    let digit = /\d/g;
    console.log(digit.exec("here it is: 1"));
    // → ["1"]
    console.log(digit.exec("and now: 1"));
    // → null
    ```
  - Another interesting effect of the global option is that it changes the way the `match` method on strings works. When called with a global expression, instead of returning an array similar to that returned by `exec`, `match` will find *all* matches of the pattern in the string and return an array containing the matched strings.
    ```js
    console.log("Banana".match(/an/g));
    // → ["an", "an"]
    ```
    {: .force-newline}

## (Misc.) INI File Format

- Example
  ```
  searchengine=https://duckduckgo.com/?q=$1
  spitefulness=9.7

  ; comments are preceded by a semicolon...
  ; each section concerns an individual enemy
  [larry]
  fullname=Larry Doe
  type=kindergarten bully
  website=http://www.geocities.com/CapeCanaveral/11451

  [davaeorn]
  fullname=Davaeorn
  type=evil wizard
  outputdir=/home/marijn/enemies/davaeorn
  ```
  {: .force-newline}

- Blank lines and lines starting with semicolons are ignored.
- Lines wrapped in `[` and `]` start a new section.
- Lines containing an alphanumeric identifier followed by an `=` character add a setting to the current section.
- Anything else is invalid.
