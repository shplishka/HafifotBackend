

# some JavaScript basic concepts:
## callbacks:
Callback function definition:

> _A reference to executable code, or a piece of executable code, that is passed as an argument to other code._

From the above definition, the callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

    function greeting(name) {
	    console.log('Hello ' + name);  
    }
    function processUserInput(callback) {  
        //var name = prompt('Please enter your name.');  
        name = 'Sierra';  
        callback(name);  
    }  
    processUserInput(greeting); //output Hello Sierra
    
In this ab above program, function greeting passed as an argument to the processUserInput function, so I hope you now understood callback function in JavaScript.
## Promises :

Promise definition:

> **The** `Promise` **object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.**

**A promise**  represents the result of the asynchronous function. Promises can be used to avoid chaining callbacks. In JavaScript,

so whenever JavaScript Code Execute as Asynchronously, need to handle an operation as one of the ways would be using promises.

A  `Promise`  is in one of these states:

-   _pending_: initial state, neither fulfilled nor rejected.
-   _fulfilled_: meaning that the operation completed successfully.
-   _rejected_: meaning that the operation failed.
```
    var promise1 = new Promise(function(resolve, reject) {  
        isDbOperationCompleted = false;  
        if (isDbOperationCompleted) {  
            resolve('Completed');  
        } else {  
            reject('Not completed');  
        }  
    });
    promise1.then(function(result) {  
        console.log(result); //Output : Completed  
    }).catch(function(error) {  
        console.log(error); //if isDbOperationCompleted=FALSE                                                    
        //Output : Not Completed  
    })
```
consider above code for a sample promise assume like doing DB Operation as asynchronously, In that promise Object arguments as two function resolve and reject, whenever we execute that promise using .then() and .catch() as callback function In order to get resolve or reject values.
As the ``[`Promise.prototype.then()`]`` and ``[`Promise.prototype.catch()`]`` methods return promises, they can be chained.

![](https://mdn.mozillademos.org/files/15911/promises.png)


## Async functions 


Async functions are enabled by default in Chrome 55 and they're quite frankly marvelous. They allow you to write promise-based code as if it were synchronous, but without blocking the main thread. They make your asynchronous code less "clever" and more readable.

Async functions work like this:

```
async function myFirstAsyncFunction() {  
	try {const fulfilledValue = await promise;  
	}  
    catch (rejectedValue) {
	    // …  
	   }
	}
```

If you use the  `async`  keyword before a function definition, you can then use  `await`  within the function. When you  `await`  a promise, the function is paused in a non-blocking way until the promise settles. If the promise fulfills, you get the value back. If the promise rejects, the rejected value is thrown.

### **Example: Logging a fetch**

Say we wanted to fetch a URL and log the response as text. Here's how it looks using promises:

```
function logFetch(url) {
  return fetch(url)
  .then(response => response.text())
  .then(text => {      
	  console.log(text);    
	})
	.catch(err => {
			console.error('fetch failed', err);    
			}
		  );
}
```

And here's the same thing using async functions:

```
async function logFetch(url) {
  try {
	  const response = await fetch(url);    
	  console.log(await response.text());  
	}  
  catch(err){    
	  console.log('fetch failed', err);  
	  }
	}
```

It's the same number of lines, but all the callbacks are gone. This makes it way easier to read, especially for those less familiar with promises.

**Note:** Anything you  `await`  is passed through  `Promise.resolve()`, so you can safely  `await`  non-native promises.

### **Async return values**

Async functions  _always_  return a promise, whether you use  `await`  or not. That promise resolves with whatever the async function returns, or rejects with whatever the async function throws. So with:

```
// wait ms millisecondsfunction
wait(ms) {
  return new Promise(r => setTimeout(r, ms));
  }
async function hello() {  
	await wait(500);  
	return 'world';
	}
```

…calling  `hello()`  returns a promise that  _fulfills_  with  `"world"`.

```
async function foo() {
	await wait(500);  throw Error('bar');
}
```

…calling  `foo()`  returns a promise that  _rejects_  with  `Error('bar')`.
