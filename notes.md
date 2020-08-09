# Table of Contents
* [Purpose](https://github.com/nickmccarty/learning-react/notes#Chapter&3)

# [Chapter 1](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch01.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-01))

## Key Takeaways:

* Functional Javascript (declarative programming?), as a paradigm, gave birth to React
    * Patterns that are readable, reusable, and testable
    * How to build out a user interface with components
* React is “the V in MVC”
    * React components act as the view layer -- the user interface -- for JavaScript applications
* Redux handles tasks like routing and state management (as it's a state container)
* Hooks allow for the adding and sharing of stateful logic across components
* Suspense offers a way to optimize asynchronous rendering with React

# [Chapter 2](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch02.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-02))

## Key Takeaways:

### About lexical variable scope:

* If a variable is created inside of an if/else block, that variable is not scoped to the block
    * [With the let keyword](https://jsbin.com/gapoxa/4/edit?js,console), we can scope a variable to any code block
    * Using let protects the value of the global variable
* Another area where curly braces don’t block off a variable’s scope is in for loops
    * [Declaring the loop counter i with let](https://jsbin.com/gapoxa/6/edit?js,output) instead of var does block off the scope of i
    
### About Functions:

* Function expressions are simply functions created as variables, such as tht shown below (see console in DevTools for output):

```
const logCompliment = function() {
    
  console.log("You're doing great!");
    
};

logCompliment();
```

* Function declarations are hoisted and function expressions are not
    * In other words, you can invoke a function before you write a function declaration
    * You cannot invoke a function created by a function expression, as it will cause an error (see example below):
    
```
// Invoking the function before it's declared

hey();

// Function Declaration

function hey() {
    
  alert("hey!");
    
}

// Alert pops up, as intended
```

Alternatively, and ineffectually:

```
// Invoking the function before it's declared

hey();

// Function Expression

const hey = function() {
    
  alert("hey!");
    
};

// Errors ensue

Javascript error adding output!
ReferenceError: Cannot access 'hey' before initialization
See your browser Javascript console for more details.
```

TypeErrors and ReferenceErrors can arise when importing files and functions in a project. If you see it, you can always refactor as a declaration.

### Returning Objects

Consider a function called person that builds an object based on parameters passed in for firstName and lastName:

```
const person = (firstName, lastName) =>
    {
        first: firstName,
        last: lastName
    }

console.log(person("Brad", "Janson"));
```

As soon as you run this, you’ll see the error:

```
Uncaught SyntaxError: Unexpected token :
```

To fix this, just wrap the object you’re returning with parentheses:

```
const person = (firstName, lastName) => ({
  first: firstName,
  last: lastName
});

console.log(person("Flad", "Hanson"));
```

These missing parentheses are the source of countless bugs in JavaScript and React apps, so it’s important to remember this one!

### Arrow Functions and Scope

Regular functions do not block `this`. For example, `this` becomes something else in the setTimeout callback, not the tahoe object:

```
const tahoe = {
  mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
  print: function(delay = 1000) {
    setTimeout(function() {
      console.log(this.mountains.join(", "));
    }, delay);
  }
};

tahoe.print(); // Uncaught TypeError: Cannot read property 'join' of undefined
```

This error is thrown because it’s trying to use the .join method on what `this` is. If we log `this`, we’ll see that it refers to the Window object:

```
console.log(this); // Window {}
```

To solve this problem, we can use the arrow function syntax to protect the scope of `this`:

```
const tahoe = {
  mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
  print: function(delay = 1000) {
    setTimeout(() => {
      console.log(this.mountains.join(", "));
    }, delay);
  }
};

tahoe.print(); // Freel, Rose, Tallac, Rubicon, Silver
```

This works as expected, and we can .join the resorts with a comma. Be careful that you’re always keeping scope in mind. Arrow functions do not block off the scope of `this`:

```
const tahoe = {
  mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
  print: (delay = 1000) => {
    setTimeout(() => {
      console.log(this.mountains.join(", "));
    }, delay);
  }
};

tahoe.print(); // Uncaught TypeError: Cannot read property 'join' of undefined
```

Changing the print function to an arrow function means that `this` is actually the window.

### Compiling JavaScript

Babel made it possible to use the latest features of JavaScript right away by compiling our code into syntax that can be interpreted by a wider range of browsers.

As an example, let’s look at an arrow function with some default arguments:

```
const add = (x = 5, y = 10) => console.log(x + y);
```

If we run Babel on this code, it will generate the following:

```
"use strict";

var add = function add() {
  var x =
    arguments.length <= 0 || arguments[0] === undefined ? 5 : arguments[0];
  var y =
    arguments.length <= 1 || arguments[1] === undefined ? 10 : arguments[1];
  return console.log(x + y);
};
```

Babel added a “use strict” declaration to run in strict mode. The variables x and y are defaulted using the arguments array, a technique you may be familiar with. The resulting JavaScript is more widely supported.

### Simple Promises with `.fetch()`

Let’s get some dummy data from the randomuser.me API:

```
console.log(fetch("https://api.randomuser.me/?nat=US&results=1"));
```

When we log this, we see that there is a pending promise. Promises give us a way to make sense out of asynchronous behavior in JavaScript. The promise is an object that represents whether the async operation is pending, has been completed, or has failed.

The pending promise represents a state before the data has been fetched. We need to chain on a function called `.then()`. This function will take in a callback function that will run if the previous operation was successful.

The something else we want to do is turn the response into JSON:

```
fetch("https://api.randomuser.me/?nat=US&results=1").then(res =>
  console.log(res.json())
);
```

The `then` method will invoke the callback function once the promise has resolved. Whatever you return from this function becomes the argument of the next then function. So we can chain together then functions to handle a promise that has been successfully resolved:

```
fetch("https://api.randomuser.me/?nat=US&results=1")
  .then(res => res.json())
  .then(json => json.results)
  .then(console.log)
  .catch(console.error);
```

First, we use fetch to make a GET request to randomuser.me. If the request is successful, we’ll then convert the response body to JSON. Next, we’ll take the JSON data and return the results, then we’ll send the results to the console.log function, which will log them to the console. Finally, there is a catch function that invokes a callback if the fetch did not resolve successfully. Any error that occurred while fetching data from randomuser.me will be based on that callback. Here, we simply log the error to the console using `console.error`.

### `.async()`/`await`

Another popular approach for handling promises is to create an `async` function. Some developers prefer the syntax of `async` functions because it looks more familiar, like code that’s found in a synchronous function. Instead of waiting for the results of a promise to resolve and handling it with a chain of then functions, `async` functions can be told to wait for the promise to resolve before further executing any code found in the function.

Let’s make another API request but wrap the functionality with an `async` function:

```
const getFakePerson = async () => {
  let res = await fetch("https://api.randomuser.me/?nat=US&results=1");
  let { results } = res.json();
  console.log(results);
};

getFakePerson();
```

Notice that the getFakePerson function is declared using the `async` keyword. This makes it an asynchronous function that can wait for promises to resolve before executing the code any further. The `await` keyword is used before promise calls. This tells the function to wait for the promise to resolve. The code below, therefore, accomplishes the exact same task as the code in the previous section that uses `then` functions.

```
const getFakePerson = async () => {
  try {
    let res = await fetch("https://api.randomuser.me/?nat=US&results=1");
    let { results } = res.json();
    console.log(results);
  } catch (error) {
    console.error(error);
  }
};

getFakePerson();
```

### Building Promises

When making an asynchronous request, one of two things can happen: everything goes as we hope, or there’s an error. There can be many different types of successful or unsuccessful requests. For example, we could try several ways to obtain the data to reach success. We could also receive multiple types of errors. Promises give us a way to simplify back to a simple pass or fail.

The getPeople function returns a new promise. The promise makes a request to the API. If the promise is successful, the data will load. If the promise is unsuccessful, an error will occur:

```
const getPeople = count =>
  new Promise((resolves, rejects) => {
    const api = `https://api.randomuser.me/?nat=US&results=${count}`;
    const request = new XMLHttpRequest();
    request.open("GET", api);
    request.onload = () =>
      request.status === 200
        ? resolves(JSON.parse(request.response).results)
        : reject(Error(request.statusText));
    request.onerror = err => rejects(err);
    request.send();
  });
```

With that, the promise has been created, but it hasn’t been used yet. We can use the promise by calling the getPeople function and passing in the number of members that should be loaded. The `then` function can be chained on to do something once the promise has been fulfilled. When a promise is rejected, any details are passed back to the `catch` function, or the `catch` block if using async/await syntax:

```
getPeople(5)
  .then(members => console.log(members))
  .catch(error => console.error(`getPeople failed: ${error.message}`))
);
```

# [Chapter 3](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch03.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-03))

## Key Takeaways:

# [Chapter 4](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch04.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-04))

## Key Takeaways:

# [Chapter 5](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch05.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-05))

## Key Takeaways:

# [Chapter 6](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch06.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-06))

## Key Takeaways:

# [Chapter 7](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch07.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-07))

## Key Takeaways:

# [Chapter 8](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch08.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-08))

## Key Takeaways:

# [Chapter 9](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch09.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-09))

## Key Takeaways:

# [Chapter 10](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch10.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-10))

## Key Takeaways:

# [Chapter 11](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch11.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-11))

## Key Takeaways:

# [Chapter 12](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch12.html) ([Code](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-12))

## Key Takeaways:
