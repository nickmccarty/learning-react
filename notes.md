# [Chapter 1](https://learning.oreilly.com/library/view/learning-react-2nd/9781492051718/ch01.html) ([GH Repo](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-01))

## Key Takeaways:

* Functional Javascript (declarative programming?), as a paradigm, gave birth to React
    * Patterns that are readable, reusable, and testable
    * How to build out a user interface with components
* React is “the V in MVC”
    * React components act as the view layer -- the user interface -- for JavaScript applications
* Redux handles tasks like routing and state management (as it's a state container)
* Hooks allow for the adding and sharing of stateful logic across components
* Suspense offers a way to optimize asynchronous rendering with React

# [Chapter 2](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-02) ([GH Repo](https://github.com/MoonHighway/learning-react/tree/second-edition/chapter-02))

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
