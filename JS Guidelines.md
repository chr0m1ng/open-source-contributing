# 🔡 JavaScript Guidelines

## Contents

- [Overview](#overview)
- [Some reinforcements](#some-reinforcements)
- [Case Convention](#case-convention)
- [Arrow functions](#arrow-functions)
- [Classes and Functions declarations](#classes-and-functions-declarations)
- [Async/Await on Promises](#async-await-on-promises)
- [Object properties](#object-properties)
- [Strings concatenating](#strings-concatenating)
- [Arrays/Objects concatenating](#arrays-objects-concatenating)
- [Curly brackets](#curly-brackets)
- [Whitespace](#whitespace)
- [Eslint](#eslint)
- [Project template](#project-template)
- [References](#references)

---

## Overview

Our conventions are based on [Airbnb][airbnb-guideline] and [Google][google-guideline] JavaScript guidelines with some adaptations. This article exposes the differences, if you need more information than this article provides you can always check both [Airbnb][airbnb-guideline] and [Google][google-guideline] guides.

## Some reinforcements

These are standard rules of both [Airbnb][airbnb-guideline] and [Google][google-guideline] but it's valid to reinforce:

- **NEVER** use `var` to declare a variable, **always** prefer `const` and `let`;
- Prefer `const` over `let`, only use `let` if you **need to reassign** the variable value;
- **ALWAYS** use **semicolons**, this avoid miss interpretation of the JS interpreter, in doubt? ~~[really dude?][semi-joke]~~ check [here why][semi-or-not];
- **ALWAYS** prefer to use `async/await` instead of `then/catch` and `callback`, this avoid a lot of indentation;
- Prefer `map`, `filter` and `reduce` instead of iterating a `array`, if they don't fit then use `array's` `forEach` extension;
- ES6+ > ES5-, avoid the ugly JS, always check for ES6+ solutions.

## Case Convention

Our case convention is a bit different of Airbnb's and Google's. Check out our pattern:

| Type              | Convention   |
|-------------------|--------------|
| Variables         | `snake_case` |
| Global constants  | `UPPER_CASE` |
| Functions/Methods | `camelCase`  |
| Classes           | `PascalCase` |
| Files             | `kebab-case` |
| Folders/Modules   | `kebab-case` |

## Arrow functions

Always prefer `arrow functions` over normal `functions`. Only use normal `functions` if you need to use the `this` statement.

```js
// bad
function foo() {
    return 'bar';
}

// good
const foo = () => {
    return 'bar';
};

const bar = function() {
    return `foo ${this}`;
};
```

Always use `parenthesis`, even if your `arrow function` only takes 1 parameter.

```js
// bad
const foo = bar => {};

// good
const foo = (bar) => {};
```

## Classes and Functions declarations

Declare `functions` with assignments.

```js
// bad
function bar() {}

// good
const bar = () => {};
```

`Class` declaration follow the normal pattern.

```js
// bad
const foo = class {};

// good
class foo {}
```

## Async/Await on Promises

Avoid the usage of `then` and `catch` when resolving `promises`.

```js
// bad
const myMethod = () => {
    methodThatReturnsPromise()
        .then(result => console.log(result))
        .catch(err => console.error(err));
};

// good
const myMethod = async () => {
    try {
        const result = await methodThatReturnsPromise();
        console.log(result);
    } catch (err) {
        console.error(err);
    }
};
```

## Object properties

Avoid the usage of `quotes` while declaring `objects`.

```js
// bad
const foo = { 'a': 'bar' };

// good
const foo = { a: 'bar' };
const my_header = { 'content-type': 'text/plain' };
```

Prefer to access `object's` properties with `dot` over `brackets`.

```js
// bad
const foo = { my_bar: 1 };
console.log(foo['my_bar']);

// good
const foo = { my_bar: 1, 'content-type': 'text/plain' };
console.log(foo.my_bar);
console.log(foo['content-type']);
```

## Strings concatenating

Always prefer to use `backtick` strings when concatenating strings.

```js
// bad
const foo = 'foo';
const bar = foo + ' bar';

// good
const foo = 'foo';
const bar = `${foo} bar`;
```

## Arrays/Objects concatenating

Prefer to use [spread operator][spread-operator] over `concat` `arrays` method and `objects's` `assign` method.

```js
// bad
const foo = [1, 2, 3];
const bar = foo.concat([4, 5, 6]);

// good
const foo = [1, 2, 3];
const bar = [...foo, 4, 5, 6];
```

```js
// bad
const foo = { a: 1 };
const bar = { b: 2 };
Object.assign(bar, foo);

// good
const foo = { a: 1 };
const bar = {...foo, b: 2 }; 
```

## Curly brackets

Always use curly brackets even if in one-line `if`/`else`.

```js
// bad
if (true) return 'foo';
else return 'bar';

// good
if (true) {
    return 'foo';
} else {
    return 'bar';
}
```

```js
// bad
if (true) 
    return 'foo';

// good
if (true) {
    return 'foo';
}
```

## Whitespace

Same as [airbnb´s][airbnb-whitespace] except that we use **tabs with 4 spaces**.

---

## Eslint

Here's our `.eslintrc.json` configuration that follows our patterns.

```json
{
    "extends": ["airbnb-base", "plugin:prettier/recommended"],
    "env": {
        "es6": true,
        "jest": true
    },
    "rules": {
        "indent": ["error", 4, { "SwitchCase": 2 }],
        "no-console": "off",
        "camelcase": "off",
        "class-methods-use-this": "off",
        "radix": "off",
        "consistent-return": "off",
        "no-underscore-dangle": "off",
        "no-var": ["error"],
        "semi": ["error", "always"],
        "comma-dangle": ["error", "never"]
    }
}
```

Also, if you use [Prettier][prettier] you can add the `.prettierrc` configuration file to your project.

```json
{
    "arrowParens": "always",
    "singleQuote": true,
    "tabWidth": 4,
    "semi": true,
    "trailingComma": "none"
}
```

## Project template

Our NodeJS **blip/kubernetes/swagger/seq ready** template can be found [here][template-api], the instructions on how to kickstart your development are available at the [main readme](https://github.com/chr0m1ng/generator-blip-api-kates/blob/master/README.md#usage) of the repository.

Now we also have a **React blip Plugin template** that can be found [here][template-plugin].

---

## References

- [Airbnb's JS guidelines][airbnb-guideline]
- [Google's JS guidelines][google-guideline]
- [Prettier][prettier]
- [Spread operators][spread-operator]
- [Use semicolons or not?][semi-or-not]

[airbnb-guideline]: https://github.com/airbnb/javascript
[airbnb-whitespace]: https://github.com/airbnb/javascript#whitespace
[google-guideline]: https://google.github.io/styleguide/jsguide.html
[semi-or-not]: https://dev.to/adriennemiller/semicolons-in-javascript-to-use-or-not-to-use-2nli
[spread-operator]: https://dev.to/sagar/three-dots---in-javascript-26ci
[semi-joke]: https://stackoverflow.com/a/444082
[prettier]: https://prettier.io/
[template-api]: https://github.com/chr0m1ng/generator-blip-api-kates
[template-plugin]: https://github.com/axeldouglas/cra-template-blip-plugin
