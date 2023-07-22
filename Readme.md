# Learning TypeScript

This is a series of notes based on the following video.

[TypeScript Tutorial for Beginners](https://youtu.be/d56mG7DezGs)

## Outline

* Introduction to TypeScript
* Setting up a dev env
* Creating first TS program
* Configuring TS compiler
* Debugging TS apps

## What is TypeScript?

TypeScript is a language originally created by Microsoft to address some of the
issues with JavaScript. TypeScript is a superset of JavaScript, meaning every
JS file is a valid TS file.

TypeScript adds the following features to JavaScript:

* Static Typing
* Code Completion
* Refactoring
* Shorthand Notations

TypeScript is basically JavaScript with static typing. What is static typing?

### Static Typing

There are two types of programming languages, statically-typed and
dynamically-typed languages.

With statically typed languages, we have to explicitly set the variables type
when declaring it. These types are analyzed at compile time, which can catch
type errors before runtime, aiding in development.

For example, when we declare a variable that is an integer we cannot then go
somewhere else in the program and change that variable to a string or float
ect.

## TypeScript to JavaScript

In the year 2023, browsers do not understand native TypeScript. So TypeScript
has to be compiled - or rather transpiled - to JavaScript before it can be run
in a browser, or other JS runtime environment.

## Setup Dev Environment

We'll need to have `node` and `npm` installed in order to setup our development
environment.

```
sudo apt install nodejs npm
```

On linux you may need to change the node version to the latest or stable
version. You can use a node package called `n` to do this.

Install `n` globally:

```
sudo npm install n -g
```

Change node version to stable version.

```
sudo n stable
```

Then finally we'll want to install typescript globally.

```
sudo npm i -g typescript
```

To verify everything went according to plan we can check the version of the
TypeScript compiler.

```
> tsc -v
Version 5.1.6
```

## First TypeScript Program

So we'll write a simple TS program. We'll declare some variables. Notice the
explicit type declarations. Then we'll use console.log to print them to the
screen.

* `hello-world/index.ts`

```ts
let age: number = 28;
let realName: string = "John";
console.log(`Hello ${realName}!\nYou are ${age} years old.`);
```

Then we'll compile that to JS with the `tsc` tool.

```
tsc index.ts
```

* `hello-world/index.js`

```js
var age = 28;
var realName = "John";
console.log("Hello ".concat(realName, "!\nYou are ").concat(age, " years old."));
```

Finally we can run our js file with the node cli tool.

```
> node index.js
Hello John!
You are 28 years old.
```

You'll notice the resulting JS file does things a little differently than the
TS file.

First off the variables in the JS file are using the var keyword. This is
because our tsc is using an older version of JS. We'll change that in a minute.

Next in the JS there is no backtick enclosed string formatting notation, that's
also because of the older ECMA standard lacking support for template literals.
So instead in vanilla JS it has to use the concat function to format the
string.

## Configuring The TypeScript Compiler

We can create a customizable configuration file for the TSC using the command
below.

```
> tsc --init

Created a new tsconfig.json with:                  
                                                   TS 
  target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true


You can learn more at https://aka.ms/tsconfig
```

We can then open up the `tsconfig.json` file to edit the compiler settings.
There are a lot of options in there but lets go over a few.

* `"target": "es2016",`

This option sets which ECMAScript standard to use when compiling to JS. The
newer versions offer the benefit of being slightly smaller and faster resulting
in more concise JS. However, that comes at the cost of compatibility with all
browsers and REs.

* `"rootDir": "./src",`

The rootDir option sets where the projects main TS code lives. By convention
this is the src directory.

* `"outDir": "./dist",`

Similar to rootDir, there is an outDir option for specifying where the compiled
JS should go. It is convention to set this to the dist (short for
distributable) directory.

* `"removeComments": true,`

Strips the comments from the TS when compiling.

* `"noEmitOnError": true,`

This will ensure no JS is generated if the TS compile fails.

Now with all of those options set all we need to do to compile our project is
run `tsc` and it will compile everything in the src directory and put the
resulting JS in a new dist dir.

## Debugging TypeScript Errors

First thing we'll want to do is generate a source map of our TS program. A
source map is a representation of how our TS code compiles to JS code. We can
enable the source map by editing our `tsconfig.json` file.

* `"sourceMap": true,`

Then when we recompile with `tsc` we'll see a `dist/index.js.map` file is
generated. While this file is plain text its contents are mainly meant to be
parsed by debugger applications.

### Debugging in Chrome

This section is off script from the video because I don't use VS code. But its
okay chrome has TS debugging support builtin.

When compiling with the sourceMap option set to true, the tsc will append a
comment to the output JS containing the location of the sourcemap file.

In chrome we can then go to the page and hit F12 to open the developer
settings, and then go to:

  Sources > Page > src > index.ts

From there you can set break points, pause execution, step over functions, step
into and out of functions and the rest.

[More Info on Chrome TS Debugging](https://www.linkedin.com/pulse/how-debug-typescript-chrome-md-ala-uddin/)

## Overview of Fundamentals

* The any Type
* Arrays
* Tuples
* Enums
* Functions
* Objects

### More Types!

JavaScript has several builtin primitive types:

* number
* string
* boolean
* null
* undefined
* object

TypeScript extends this with its own set of primitive types:

* any
* unknown
* never
* enum
* tuple

### Annotating & Type Declaration

So far we've see how TS vars can be explicitly typed when they're declared.

```ts
let sales: number = 123_456_789;
let course: string = 'TypeScript';
let is_published: boolean = true;
```

However this is not actually necessary. We can remove the type and the tsc will
still know what type the variable is.

```ts
let sales = 123_456_789;
let course = 'TypeScript';
let is_published = true;
```

### The Any Type

In TypeScript once a variable has been declared as a type it has to stay as
that type. This is what they mean when they say the language is strongly typed
and its what provide the "type-safety" of TS. The only exception to this `any`
type.

If you declare a variable but never assign it to anything then it will be
automatically assigned to type any.

```ts
let level;
level = 1;
level = 'a';
```

This is sort of against the whole spirit of TS, but it has its uses. Overall
its just something to be aware of while writing TS.

### Arrays

By default in TS any array that is declared but not assigned a type or values
(from which it can infer an array type) is of type any, meaning it can be a
mixed type array. In an any type array the first three elements could be
numbers but the fourth could be a boolean. That can cause type problems.

In order to avoid this either explicitly declare the array's type.

```
let numbers: number[];
```

Or make sure the array is initialized with only values of that type.

```

let numbers = [1, 2, 3]
```

### Tuples

TS has tuples. A tuple is a typed array with a pre-defined length and types for
each index.

```ts
let user: [number, string] = [23, 'John'];
```

Its possible to put multiple values in a tuple however its advisable to only
use the to hold two or three values at a time in order to make them make more
sense to others who may have to read your code. With two values you can infer
an association. With 7 uniquely typed values that becomes harder.

### Enums

TS also has enums. Enums represent a list of related constants.

```ts
// Instead of this
const small = 1;
const medium = 2;
const large = 3;

// Use an Enum, Notice PascalCase
const enum Size { Small = 1, Medium, Large };
let mySize: Size = Size.Medium;
console.log(mySize);
```

The above will compile fine and when we look at the final output we'll see the
tsc has auto incremented the value of the items in the Size enum.

### Functions

```ts
function calculateTax(income: number): number {

  return income * 1.2;;
}
```

The above function has an explicitly defined return type, explicitly defined
parameter type. This is best practice for functions in TS.

By default JS functions will be of return type 'undefined'. Because of this we
want to make sure to always set a return. There is a compiler setting for this
we can turn on called `noImplicitReturns`.

Its possible to set default parameter values.

```ts
function calculateTax(income: number, taxYear = 2023): number {
  if (taxYear < 2022)
    return income * 1.2;

  return income * 1.3;
}

calculateTax(10_000):
```

With the above taxYear can be omitted and the function will use the default
value.

Its also possible in TS to use an optional function parameter. To do this use
the question mark sign '?' after the parameter name.

```ts
function calculateTax(income: number, taxYear?: number): number {
  if ((taxYear || 2022) < 2022)
    return income * 1.2;

  return income * 1.3;
}

calculateTax(10_000):
```

The above will allow the taxYear to be optional and will use a default value
via an or statement.

### Objects

```ts
let employee {
  readonly id: number,
  name: string,
  retire: (date: Date) => void,
} = { 
  1,
  'John',
  retire: (date: Date) => {
    console.log(date);
  }
};
```

The above is a TypeScript object. It has two parameters and a retire method.

The parameter id is set to readonly so it cannot be changed later on. The
retire keyword is used to define a method. In this case its an anonymous
function which returns void.


## Advanced Types

* Type Aliases 
* Unions and Intersections
* Type Narrowing
* Nullable Types
* The Unknown Type
* The Never Type

### Type Aliases

We can make the above object a little cleaner by using type aliases.

```ts
type Employee = {
  readonly id: number,
  name: string,
  retire: (date: Date) => void,
}

let employee: Employee = {
  1,
  'John',
  retire: (date: Date) => {
    console.log(date);
  }
}
```

Now we can reuse our Employee object to instantiate multiple employees. This is
the advantage of using a type alias.

### Union Types

With union types we can give a parameter more than one time. We can use a union
type by putting two parameter types on either side of a pipe character like,
'string|number'.

```ts
function kgToLbs(weight: number | string): number {
  // Narrowing
  if (typeof weight === 'number')
    return weight * 2.2;
  else
    return parseInt(weight) * 2.2;
}
kgToLbs(10);
kgToLbs('10kg');
```

### Intersection Types

Intersection types are used to combine types with the '&' char.

For example,

```ts
type Draggable = {
  drag: () => void
};

type Resizable = {
  resize: () => void
};

// Intersection Type
type UIWidget = Draggable & Resizable;

let textBox: UIWidget = {
  drag: () => {},
  resize: () => {}
}
```

In the above we've created a intersection type component called UIWidget that
implements both the Draggable type and the Resizable type.

### Literal Types

Say we want a variable to be either 50 or 100. We could annotate it with the
number type but then it could be set to any number. To make sure our var can
only be 50 or 100 we can use a literal type.

```ts
type Quantity = 50 | 100;
let quantity: Quantity = 100;

type Metric = 'cm' | 'inch';
```

We've made a type alias with a union to accept either of two literal values.

### Nullable Types

```ts
function greet(name: string | null | undefined) {
  if (name)
    console.log(name.toUpperCase());
  else
    console.log('Hola!');
}

greet(null);
```

### Optional Chaining

```ts
type Customer = {
  birthday: Date
};

function getCustomer(id: number): Customer | null | undefined {
  return id === 0 ? null : {birthday: new Date() };

}

let customer = getCustomer(0);
// Optional property access operator
console.log(customer?.birthday?.getFullYear());

// Optional element access operator
// customers?.[0]

// Optional call operator
// let log: any = null;
// log?.('a')
```

With the optional property operator '?.' we can call the method on the object
only if the object is not null or undefined. We can also chain these together.

The Optional element access operator prevents missing elements in the array
from messing things up.

The Optional call operator would return undefined if the function doesn't
exist.
