# Airbnb JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript*

[For the ES5-only guide click here](es5/).

## Table of Contents

    1. [Types](#types)
    1. [References](#references)
    1. [Objects](#objects)
    1. [Arrays](#arrays)
    1. [Destructuring](#destructuring)
    1. [Strings](#strings)
    1. [Functions](#functions)
    1. [Arrow Functions](#arrow-functions)
    1. [Constructors](#constructors)
    1. [Iterators and Generators](#iterators-and-generators)
    1. [Properties](#properties)
    1. [Variables](#variables)
    1. [Comparison Operators & Equality](#comparison-operators--equality)
    1. [Blocks](#blocks)
    1. [Comments](#comments)
    1. [Whitespace](#whitespace)
    1. [Commas](#commas)
    1. [Semicolons](#semicolons)
    1. [Type Casting & Coercion](#type-casting--coercion)
    1. [Naming Conventions](#naming-conventions)
    1. [Accessors](#accessors)
    1. [Events](#events)
    1. [jQuery](#jquery)
    1. [Dialogs](#dialogs)
    1. [Forms](#forms)
    1. [Components](#components)
    1. [Storages](#storages)

## Types

    - **Primitives**: When you access a primitive type you work directly on its value.

        + `string`
        + `number`
        + `boolean`
        + `null`
        + `undefined`

        ```javascript
        const foo = 1;
        var bar = foo;

        bar = 9;

        console.log(foo, bar); // => 1, 9
        ```
    - **Complex**: When you access a complex type you work on a reference to its value.

        + `object`
        + `array`
        + `function`

        ```javascript
        const foo = [1, 2];
        const bar = foo;

        bar[0] = 9;

        console.log(foo[0], bar[0]); // => 9, 9
        ```

**[⬆ back to top](#table-of-contents)**

## References

    - Use `const` for all of your references; avoid using `var`.

    > Why? This ensures that you can't reassign your references (mutation), which can lead to bugs and difficult to comprehend code.

        ```javascript
        // bad
        var a = 1;
        var b = 2;

        // good
        const a = 1;
        const b = 2;
        ```

**[⬆ back to top](#table-of-contents)**

## Objects

    - Use the literal syntax for object creation.

        ```javascript
        // bad
        const item = new Object();

        // good
        const item = {};
        ```

    - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61).

        ```javascript
        // bad
        const superman = {
            default: { clark: 'kent' },
            private: true
        };

        // good
        const superman = {
            defaults: { clark: 'kent' },
            hidden: true
        };
        ```

    - Use readable synonyms in place of reserved words.

        ```javascript
        // bad
        const superman = {
            class: 'alien'
        };

        // bad
        const superman = {
            klass: 'alien'
        };

        // good
        const superman = {
            type: 'alien'
        };
        ```

    <a name="es6-computed-properties"></a>
    - Use computed property names when creating objects with dynamic property names.

    > Why? They allow you to define all the properties of an object in one place.

        ```javascript

        function getKey(k) {
            return `a key named ${k}`;
        }

        // bad
        const obj = {
            id: 5,
            name: 'San Francisco',
        };
        obj[getKey('enabled')] = true;

        // good
        const obj = {
            id: 5,
            name: 'San Francisco',
            [getKey('enabled')]: true,
        };
        ```

    <a name="es6-object-shorthand"></a>
    - Use object method shorthand.

        ```javascript
        // bad
        const atom = {
            value: 1,

            addValue: function (value) {
                return atom.value + value;
            },
        };

        // good
        const atom = {
            value: 1,

            addValue(value) {
                return atom.value + value;
            },
        };
        ```

    <a name="es6-object-concise"></a>
    - Use property value shorthand.

    > Why? It is shorter to write and descriptive.

        ```javascript
        const lukeSkywalker = 'Luke Skywalker';

        // bad
        const obj = {
            lukeSkywalker: lukeSkywalker
        };

        // good
        const obj = {
            lukeSkywalker
        };
        ```

    - Group your shorthand properties at the beginning of your object declaration.

    > Why? It's easier to tell which properties are using the shorthand.

        ```javascript
        const anakinSkywalker = 'Anakin Skywalker';
        const lukeSkywalker = 'Luke Skywalker';

        // bad
        const obj = {
            episodeOne: 1,
            twoJedisWalkIntoACantina: 2,
            lukeSkywalker,
            episodeThree: 3,
            mayTheFourth: 4,
            anakinSkywalker,
        };

        // good
        const obj = {
            lukeSkywalker,
            anakinSkywalker,
            episodeOne: 1,
            twoJedisWalkIntoACantina: 2,
            episodeThree: 3,
            mayTheFourth: 4,
        };
        ```

**[⬆ back to top](#table-of-contents)**

## Arrays

    - Use the literal syntax for array creation.

        ```javascript
        // bad
        const items = new Array();

        // good
        const items = [];
        ```

    - Use Array#push instead of direct assignment to add items to an array.

        ```javascript
        const someStack = [];


        // bad
        someStack[someStack.length] = 'abracadabra';

        // good
        someStack.push('abracadabra');
        ```

    <a name="es6-array-spreads"></a>
    - Use array spreads `...` to copy arrays.

        ```javascript
        // bad
        const len = items.length;
        const itemsCopy = [];
        var i;

        for (i = 0; i < len; i++) {
            itemsCopy[i] = items[i];
        }

        // good
        const itemsCopy = [...items];
        ```
    - To convert an array-like object to an array, use Array#from.

        ```javascript
        const foo = document.querySelectorAll('.foo');
        const nodes = Array.from(foo);
        ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

    - Use object destructuring when accessing and using multiple properties of an object.

    > Why? Destructuring saves you from creating temporary references for those properties.

        ```javascript
        // bad
        function getFullName(user) {
            const firstName = user.firstName;
            const lastName = user.lastName;

            return `${firstName} ${lastName}`;
        }

        // good
        function getFullName(obj) {
            const { firstName, lastName } = obj;
            return `${firstName} ${lastName}`;
        }

        // best
        function getFullName({ firstName, lastName }) {
            return `${firstName} ${lastName}`;
        }
        ```

    - Use array destructuring.

        ```javascript
        const arr = [1, 2, 3, 4];

        // bad
        const first = arr[0];
        const second = arr[1];

        // good
        const [first, second] = arr;
        ```

    - Use object destructuring for multiple return values, not array destructuring.

    > Why? You can add new properties over time or change the order of things without breaking call sites.

        ```javascript
        // bad
        function processInput(input) {
            // then a miracle occurs
            return [left, right, top, bottom];
        }

        // the caller needs to think about the order of return data
        const [left, __, top] = processInput(input);

        // good
        function processInput(input) {
            // then a miracle occurs
            return { left, right, top, bottom };
        }

        // the caller selects only the data they need
        const { left, right } = processInput(input);
        ```


**[⬆ back to top](#table-of-contents)**

## Strings

    - Use single quotes `''` for strings.

        ```javascript
        // bad
        const name = "Capt. Janeway";

        // good
        const name = 'Capt. Janeway';
        ```

    - Strings longer than 80 characters should be written across multiple lines using string concatenation.
    - Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

        ```javascript
        // bad
        const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

        // bad
        const errorMessage = 'This is a super long error that was thrown because \
        of Batman. When you stop to think about how Batman had anything to do \
        with this, you would get nowhere \
        fast.';

        // good
        const errorMessage = 'This is a super long error that was thrown because ' +
            'of Batman. When you stop to think about how Batman had anything to do ' +
            'with this, you would get nowhere fast.';
        ```

    <a name="es6-template-literals"></a>
    - When programmatically building up strings, use template strings instead of concatenation.

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

        ```javascript
        // bad
        function sayHi(name) {
            return 'How are you, ' + name + '?';
        }

        // bad
        function sayHi(name) {
            return ['How are you, ', name, '?'].join();
        }

        // good
        function sayHi(name) {
            return `How are you, ${name}?`;
        }
        ```

**[⬆ back to top](#table-of-contents)**


## Functions

    - Use function declarations instead of function expressions.

    > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-functions) in place of function expressions.

        ```javascript
        // bad
        const foo = function () {
        };

        // good
        function foo() {
        }
        ```

    - Function expressions:

        ```javascript
        // immediately-invoked function expression (IIFE)
        (() => {
            console.log('Welcome to the Internet. Please follow me.');
        })();
        ```

    - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
    - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

        ```javascript
        // bad
        if (currentUser) {
            function test() {
                console.log('Nope.');
            }
        }

        // good
        var test;
        if (currentUser) {
            test = () => {
                console.log('Yup.');
            };
        }
        ```

    - Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

        ```javascript
        // bad
        function nope(name, options, arguments) {
            // ...stuff...
        }

        // good
        function yup(name, options, args) {
            // ...stuff...
        }
        ```

    <a name="es6-rest"></a>
    - Never use `arguments`, opt to use rest syntax `...` instead.

    > Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

        ```javascript
        // bad
        function concatenateAll() {
            const args = Array.prototype.slice.call(arguments);
            return args.join('');
        }

        // good
        function concatenateAll(...args) {
            return args.join('');
        }
        ```

    <a name="es6-default-parameters"></a>
    - Use default parameter syntax rather than mutating function arguments.

        ```javascript
        // really bad
        function handleThings(opts) {
            // No! We shouldn't mutate function arguments.
            // Double bad: if opts is falsy it'll be set to an object which may
            // be what you want but it can introduce subtle bugs.
            opts = opts || {};
            // ...
        }

        // still bad
        function handleThings(opts) {
            if (opts === void 0) {
                opts = {};
            }
            // ...
        }

        // good
        function handleThings(opts = {}) {
            // ...
        }
        ```

    - Avoid side effects with default parameters

    > Why? They are confusing to reason about.

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
        console.log(a);
    }
    count();    // 1
    count();    // 2
    count(3); // 3
    count();    // 3
    ```


**[⬆ back to top](#table-of-contents)**

## Arrow Functions

    - When you must use function expressions (as when passing an anonymous function), use arrow function notation.

    > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

    > Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

        ```javascript
        // bad
        [1, 2, 3].map(function (x) {
            return x * x;
        });

        // good
        [1, 2, 3].map((x) => {
            return x * x;
        });
        ```

    - If the function body fits on one line, feel free to omit the braces and use implicit return. Otherwise, add the braces and use a `return` statement.

    > Why? Syntactic sugar. It reads well when multiple functions are chained together.

    > Why not? If you plan on returning an object.

        ```javascript
        // good
        [1, 2, 3].map((x) => x * x);

        // good
        [1, 2, 3].map((x) => {
            return { number: x };
        });
        ```

    - Always use parentheses around the arguments. Omitting the parentheses makes the functions less readable and only works for single arguments.

    > Why? These declarations read better with parentheses. They are also required when you have multiple parameters so this enforces consistency.

        ```javascript
        // bad
        [1, 2, 3].map(x => x * x);

        // good
        [1, 2, 3].map((x) => x * x);
        ```

**[⬆ back to top](#table-of-contents)**


## Constructors

    - Always use `class`. Avoid manipulating `prototype` directly.

    > Why? `class` syntax is more concise and easier to reason about.

        ```javascript
        // bad
        function Queue(contents = []) {
            this._queue = [...contents];
        }
        Queue.prototype.pop = function() {
            const value = this._queue[0];
            this._queue.splice(0, 1);
            return value;
        }


        // good
        class Queue {
            constructor(contents = []) {
                this._queue = [...contents];
            }
            pop() {
                const value = this._queue[0];
                this._queue.splice(0, 1);
                return value;
            }
        }
        ```

    - Use `extends` for inheritance.

    > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

        ```javascript
        // bad
        const inherits = require('inherits');
        function PeekableQueue(contents) {
            Queue.apply(this, contents);
        }
        inherits(PeekableQueue, Queue);
        PeekableQueue.prototype.peek = function() {
            return this._queue[0];
        }

        // good
        class PeekableQueue extends Queue {
            peek() {
                return this._queue[0];
            }
        }
        ```

    - Methods can return `this` to help with method chaining.

        ```javascript
        // bad
        Jedi.prototype.jump = function() {
            this.jumping = true;
            return true;
        };

        Jedi.prototype.setHeight = function(height) {
            this.height = height;
        };

        const luke = new Jedi();
        luke.jump(); // => true
        luke.setHeight(20); // => undefined

        // good
        class Jedi {
            jump() {
                this.jumping = true;
                return this;
            }

            setHeight(height) {
                this.height = height;
                return this;
            }
        }

        const luke = new Jedi();

        luke.jump()
            .setHeight(20);
        ```


    - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

        ```javascript
        class Jedi {
            contructor(options = {}) {
                this.name = options.name || 'no name';
            }

            getName() {
                return this.name;
            }

            toString() {
                return `Jedi - ${this.getName()}`;
            }
        }
        ```

**[⬆ back to top](#table-of-contents)**


## Iterators and Generators

    - Don't use iterators. Prefer JavaScript's higher-order functions like `map()` and `reduce()` instead of loops like `for-of`.

    > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side-effects.

        ```javascript
        const numbers = [1, 2, 3, 4, 5];

        // bad
        var sum = 0;
        for (var num of numbers) {
            sum += num;
        }

        sum === 15;

        // good
        var sum = 0;
        numbers.forEach((num) => sum += num);
        sum === 15;

        // best (use the functional force)
        const sum = numbers.reduce((total, num) => total + num, 0);
        sum === 15;
        ```

    - Don't use generators for now.

    > Why? They don't transpile well to ES5.

**[⬆ back to top](#table-of-contents)**


## Properties

    - Use dot notation when accessing properties.

        ```javascript
        const luke = {
            jedi: true,
            age: 28,
        };

        // bad
        const isJedi = luke['jedi'];

        // good
        const isJedi = luke.jedi;
        ```

    - Use subscript notation `[]` when accessing properties with a variable.

        ```javascript
        const luke = {
            jedi: true,
            age: 28,
        };

        function getProp(prop) {
            return luke[prop];
        }

        const isJedi = getProp('jedi');
        ```

**[⬆ back to top](#table-of-contents)**


## Variables

    - Always use `const` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

        ```javascript
        // bad
        superPower = new SuperPower();

        // good
        const superPower = new SuperPower();
        ```

    - Use one `const` declaration per variable.

        > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

        ```javascript
        // bad
        const items = getItems(),
                goSportsTeam = true,
                dragonball = 'z';

        // bad
        // (compare to above, and try to spot the mistake)
        const items = getItems(),
                goSportsTeam = true;
                dragonball = 'z';

        // good
        const items = getItems();
        const goSportsTeam = true;
        const dragonball = 'z';
        ```

    - Group all your `const`s and then group all your `var`s.

    > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

        ```javascript
        // bad
        var i, len, dragonball,
                items = getItems(),
                goSportsTeam = true;

        // bad
        var i;
        const items = getItems();
        var dragonball;
        const goSportsTeam = true;
        var len;

        // good
        const goSportsTeam = true;
        const items = getItems();
        var dragonball;
        var i;
        var length;
        ```

    - Assign variables where you need them, but place them in a reasonable place.

        ```javascript
        // good
        function() {
            test();
            console.log('doing stuff..');

            //..other stuff..

            const name = getName();

            if (name === 'test') {
                return false;
            }

            return name;
        }

        // bad - unnessary function call
        function(hasName) {
            const name = getName();

            if (!hasName) {
                return false;
            }

            this.setFirstName(name);

            return true;
        }

        // good
        function(hasName) {
            if (!hasName) {
                return false;
            }

            const name = getName();
            this.setFirstName(name);

            return true;
        }
        ```

**[⬆ back to top](#table-of-contents)**


## Comparison Operators & Equality

    - Use `===` and `!==` over `==` and `!=`.
    - Comparison operators are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

        + **Objects** evaluate to **true**
        + **Undefined** evaluates to **false**
        + **Null** evaluates to **false**
        + **Booleans** evaluate to **the value of the boolean**
        + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
        + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

        ```javascript
        if ([0]) {
            // true
            // An array is an object, objects evaluate to true
        }
        ```

    - Use shortcuts.

        ```javascript
        // bad
        if (name !== '') {
            // ...stuff...
        }

        // good
        if (name) {
            // ...stuff...
        }

        // bad
        if (collection.length > 0) {
            // ...stuff...
        }

        // good
        if (collection.length) {
            // ...stuff...
        }
        ```

    - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

    - Use braces with all multi-line blocks.

        ```javascript
        // bad
        if (test)
            return false;

        // good
        if (test) return false;

        // good
        if (test) {
            return false;
        }

        // bad
        function() { return false; }

        // good
        function() {
            return false;
        }
        ```

    - If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
        `if` block's closing brace.

        ```javascript
        // bad
        if (test) {
            thing1();
            thing2();
        }
        else {
            thing3();
        }

        // good
        if (test) {
            thing1();
            thing2();
        } else {
            thing3();
        }
        ```


**[⬆ back to top](#table-of-contents)**


## Comments

    - Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.

        ```javascript
        // bad
        // make() returns a new element
        // based on the passed in tag name
        //
        // @param {String} tag
        // @return {Element} element
        function make(tag) {

            // ...stuff...

            return element;
        }

        // good
        /**
         * make() returns a new element
         * based on the passed in tag name
         *
         * @param {String} tag
         * @return {Element} element
         */
        function make(tag) {

            // ...stuff...

            return element;
        }
        ```

    - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

        ```javascript
        // bad
        const active = true;    // is current tab

        // good
        // is current tab
        const active = true;

        // bad
        function getType() {
            console.log('fetching type...');
            // set the default type to 'no type'
            const type = this._type || 'no type';

            return type;
        }

        // good
        function getType() {
            console.log('fetching type...');

            // set the default type to 'no type'
            const type = this._type || 'no type';

            return type;
        }
        ```

    - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

    - Use `// FIXME:` to annotate problems.

        ```javascript
        class Calculator {
            constructor() {
                // FIXME: shouldn't use a global here
                total = 0;
            }
        }
        ```

    - Use `// TODO:` to annotate solutions to problems.

        ```javascript
        class Calculator {
            constructor() {
                // TODO: total should be configurable by an options param
                this.total = 0;
            }
        }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

    - Use tabs set to 4 spaces.

        ```javascript
        // bad
        function() {
        ∙∙const name;
        }

        // bad
        function() {
        ∙const name;
        }

        // good
        function() {
        ∙∙∙∙const name;
        }
        ```

    - Place 1 space before the leading brace.

        ```javascript
        // bad
        function test(){
            console.log('test');
        }

        // good
        function test() {
            console.log('test');
        }

        // bad
        dog.set('attr',{
            age: '1 year',
            breed: 'Bernese Mountain Dog'
        });

        // good
        dog.set('attr', {
            age: '1 year',
            breed: 'Bernese Mountain Dog'
        });
        ```

    - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.

        ```javascript
        // bad
        if(isJedi) {
            fight ();
        }

        // good
        if (isJedi) {
            fight();
        }

        // bad
        function fight () {
            console.log ('Swooosh!');
        }

        // good
        function fight() {
            console.log('Swooosh!');
        }
        ```

    - Set off operators with spaces.

        ```javascript
        // bad
        const x=y+5;

        // good
        const x = y + 5;
        ```

    - End files with a single newline character.

        ```javascript
        // bad
        (function(global) {
            // ...stuff...
        })(this);
        ```

        ```javascript
        // bad
        (function(global) {
            // ...stuff...
        })(this);↵
        ↵
        ```

        ```javascript
        // good
        (function(global) {
            // ...stuff...
        })(this);↵
        ```

    - Use indentation when making long method chains. Use a leading dot, which
        emphasizes that the line is a method call, not a new statement.

        ```javascript
        // bad
        $('#items').find('.selected').highlight().end().find('.open').updateCount();

        // bad
        $('#items').
            find('.selected').
                highlight().
                end().
            find('.open').
                updateCount();

        // good
        $('#items')
            .find('.selected')
                .highlight()
                .end()
            .find('.open')
                .updateCount();

        // bad
        const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
                .attr('width', (radius + margin) * 2).append('svg:g')
                .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
                .call(tron.led);

        // good
        const leds = stage.selectAll('.led')
                .data(data)
            .enter().append('svg:svg')
                .classed('led', true)
                .attr('width', (radius + margin) * 2)
            .append('svg:g')
                .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
                .call(tron.led);
        ```

    - Leave a blank line after blocks and before the next statement

        ```javascript
        // bad
        if (foo) {
            return bar;
        }
        return baz;

        // good
        if (foo) {
            return bar;
        }

        return baz;

        // bad
        const obj = {
            foo() {
            },
            bar() {
            },
        };
        return obj;

        // good
        const obj = {
            foo() {
            },

            bar() {
            },
        };

        return obj;
        ```


**[⬆ back to top](#table-of-contents)**

## Commas

    - Leading commas: **Nope.**

        ```javascript
        // bad
        const story = [
                once
            , upon
            , aTime
        ];

        // good
        const story = [
            once,
            upon,
            aTime,
        ];

        // bad
        const hero = {
                firstName: 'Ada'
            , lastName: 'Lovelace'
            , birthYear: 1815
            , superPower: 'computers'
        };

        // good
        const hero = {
            firstName: 'Ada',
            lastName: 'Lovelace',
            birthYear: 1815,
            superPower: 'computers',
        };
        ```

    - Additional trailing comma: **Yup.**

    > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](es5/README.md#commas) in legacy browsers.

        ```javascript
        // bad - git diff without trailing comma
        const hero = {
                 firstName: 'Florence',
        -        lastName: 'Nightingale'
        +        lastName: 'Nightingale',
        +        inventorOf: ['coxcomb graph', 'mordern nursing']
        }

        // good - git diff with trailing comma
        const hero = {
                 firstName: 'Florence',
                 lastName: 'Nightingale',
        +        inventorOf: ['coxcomb chart', 'mordern nursing'],
        }

        // bad
        const hero = {
            firstName: 'Dana',
            lastName: 'Scully'
        };

        const heroes = [
            'Batman',
            'Superman'
        ];

        // good
        const hero = {
            firstName: 'Dana',
            lastName: 'Scully',
        };

        const heroes = [
            'Batman',
            'Superman',
        ];
        ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

    - **Yup.**

        ```javascript
        // bad
        (function() {
            const name = 'Skywalker'
            return name
        })()

        // good
        (() => {
            const name = 'Skywalker';
            return name;
        })();

        // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
        ;(() => {
            const name = 'Skywalker';
            return name;
        })();
        ```

        [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

    - Perform type coercion at the beginning of the statement.
    - Strings:

        ```javascript
        //    => this.reviewScore = 9;

        // bad
        const totalScore = this.reviewScore + '';

        // good
        const totalScore = String(this.reviewScore);
        ```

    - Use `parseInt` for Numbers and always with a radix for type casting.

        ```javascript
        const inputValue = '4';

        // bad
        const val = new Number(inputValue);

        // bad
        const val = +inputValue;

        // bad
        const val = inputValue >> 0;

        // bad
        const val = parseInt(inputValue);

        // good
        const val = Number(inputValue);

        // good
        const val = parseInt(inputValue, 10);
        ```

    - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

        ```javascript
        // good
        /**
         * parseInt was the reason my code was slow.
         * Bitshifting the String to coerce it to a
         * Number made it a lot faster.
         */
        const val = inputValue >> 0;
        ```

    - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

        ```javascript
        2147483647 >> 0 //=> 2147483647
        2147483648 >> 0 //=> -2147483648
        2147483649 >> 0 //=> -2147483647
        ```

    - Booleans:

        ```javascript
        const age = 0;

        // bad
        const hasAge = new Boolean(age);

        // good
        const hasAge = Boolean(age);

        // good
        const hasAge = !!age;
        ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

    - Avoid single letter names. Be descriptive with your naming.

        ```javascript
        // bad
        function q() {
            // ...stuff...
        }

        // good
        function query() {
            // ..stuff..
        }
        ```

    - Use camelCase when naming objects, functions, and instances.

        ```javascript
        // bad
        const OBJEcttsssss = {};
        const this_is_my_object = {};
        function c() {}

        // good
        const thisIsMyObject = {};
        function thisIsMyFunction() {}
        ```

    - Use PascalCase when naming constructors or classes.

        ```javascript
        // bad
        function user(options) {
            this.name = options.name;
        }

        const bad = new user({
            name: 'nope',
        });

        // good
        class User {
            constructor(options) {
                this.name = options.name;
            }
        }

        const good = new User({
            name: 'yup',
        });
        ```

    - Use a leading underscore `_` when naming private properties.

        ```javascript
        // bad
        this.__firstName__ = 'Panda';
        this.firstName_ = 'Panda';

        // good
        this._firstName = 'Panda';
        ```

    - Don't save references to `this`. Use arrow functions or Function#bind.

        ```javascript
        // bad
        function foo() {
            const self = this;
            return function() {
                console.log(self);
            };
        }

        // bad
        function foo() {
            const that = this;
            return function() {
                console.log(that);
            };
        }

        // good
        function foo() {
            return () => {
                console.log(this);
            };
        }
        ```


**[⬆ back to top](#table-of-contents)**


## Accessors

    - Accessor functions for properties are not required.
    - If you do make accessor functions use getVal() and setVal('hello').

        ```javascript
        // bad
        dragon.age();

        // good
        dragon.getAge();

        // bad
        dragon.age(25);

        // good
        dragon.setAge(25);
        ```

    - If the property is a boolean, use isVal() or hasVal().

        ```javascript
        // bad
        if (!dragon.age()) {
            return false;
        }

        // good
        if (!dragon.hasAge()) {
            return false;
        }
        ```

    - It's okay to create get() and set() functions, but be consistent.

        ```javascript
        class Jedi {
            constructor(options = {}) {
                const lightsaber = options.lightsaber || 'blue';
                this.set('lightsaber', lightsaber);
            }

            set(key, val) {
                this[key] = val;
            }

            get(key) {
                return this[key];
            }
        }
        ```

**[⬆ back to top](#table-of-contents)**


## Events

    - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

        ```javascript
        // bad
        $(this).trigger('listingUpdated', listing.id);

        ...

        $(this).on('listingUpdated', function(e, listingId) {
            // do something with listingId
        });
        ```

        prefer:

        ```javascript
        // good
        $(this).trigger('listingUpdated', { listingId : listing.id });

        ...

        $(this).on('listingUpdated', function(e, data) {
            // do something with data.listingId
        });
        ```

    **[⬆ back to top](#table-of-contents)**


## jQuery

    - Prefix jQuery object variables with a `$`.

        ```javascript
        // bad
        const sidebar = $('.sidebar');

        // good
        const $sidebar = $('.sidebar');
        ```

    - Cache jQuery lookups.

        ```javascript
        // bad
        function setSidebar() {
            $('.sidebar').hide();

            // ...stuff...

            $('.sidebar').css({
                'background-color': 'pink'
            });
        }

        // good
        function setSidebar() {
            const $sidebar = $('.sidebar');
            $sidebar.hide();

            // ...stuff...

            $sidebar.css({
                'background-color': 'pink'
            });
        }
        ```

    - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
    - Use `find` with scoped jQuery object queries.

        ```javascript
        // bad
        $('ul', '.sidebar').hide();

        // bad
        $('.sidebar').find('ul').hide();

        // good
        $('.sidebar ul').hide();

        // good
        $('.sidebar > ul').hide();

        // good
        $sidebar.find('ul').hide();
        ```

**[⬆ back to top](#table-of-contents)**


## Dialogs

        - all dialogs must be in /assets/js/dialogs/<module name>/<dialog name>.js use tire for separating
        
        ```javascript
        /assets/js/dialogs/client-request/blocked-change-form.js
        ```
        
        - use PascalCase when naming dialogs
        
        ```javascript
        new IxusTrade.Dialog('BlockedChangeForm', function(dialogName, args, resultDeffered) {
        ...
        });
        ```
        
        - usualy dilog contains Ext.Window and Ext.FormPanel for naming use camelCase
        
        ```javascript
        var blockedChangeWindow;
        var blockedChangeForm;
        ```
        
        - declare all form, stores, grids and other objects used in dialog in the top of dialog body, group them by the type
        
        ```javascript
         new IxusTrade.Dialog('BlockedChangeForm', function(dialogName, args, resultDeffered) {
         var blockedChangeWindow;
         var blockedChangeForm;
         
         blockedChangeForm = new Ext.FormPanel({
         
         ...
         ```
         
         - calling the dialog, in module name tire, in dialog name camelCase 
         
         ```javascript
         IxusTrade.utils.dialog.open('dialogs.<module name>.<dialog name>', <object of arguments>);
         ```


**[⬆ back to top](#table-of-contents)**


## Forms

        - for initialization forms use .getForm().load(
        
        ```javascript
        clientRequestChangeForm.getForm().load({
            url: URL_GET_DATA,
            params: params,
            method: 'POST',
            waitMsg: 'Loading data...',
            success: function(e, response) {
        ```
        
        
        

**[⬆ back to top](#table-of-contents)**

## Components

        - all components must be in /assets/js/components/<module name>/<dialog name>.js use tire for separating, components contains grids and storages for them
        
        ```javascript
        /assets/js/components/client-request/history-grid.js
        ```
        
        - use camelCase to naming components
        
        ```javascript
        pnEventsGrid
        ```
        
        - declare all form, stores, grids and other objects used in component in the top of dialog body, group them by the type
        
        ```javascript
        IxusTrade.components.nomenclature.PNEventsGrid = function() {
            var pnEventsStore;
            var pnEventsGrid;
            var pnEventsTbar;
        ```   
            
        - calling the component, in component name camelCase 
                 
         ```javascript
         pnEventsGrid = new IxusTrade.components.nomenclature.pnEventsGrid();
         ```
        

**[⬆ back to top](#table-of-contents)**

## Storages

        - all storages must be in /assets/js/storages/<module name>/<dialog name>.js use tire for separating, components contains grids and storages for them
                
        ```javascript
        /assets/js/storages/inventory/inventory.js
        ```
        
        - use camelCase to naming components
        
        ```javascript
        pnEventsGrid
        ```
        
        - declare all form, stores, grids and other objects used in component in the top of dialog body, group them by the type
        
        ```javascript
        IxusTrade.components.nomenclature.pnEventsGrid = function() {
            var pnEventsStore;
            var pnEventsGrid;
            var pnEventsTbar;
        ```   
            
        - calling the component, in component name camelCase 
                 
         ```javascript
         pnEventsGrid = new IxusTrade.components.nomenclature.pnEventsGrid();
         ```