# Transform API

# Motivation
To change/enrich/reduce data a developer has to use the ResultDispatcher.addCallback(topic, fn) API. +
In it's current state this API passes a reference according to the supplied topic.
Let's consider we retrieve an array through this API and we want to filter the items according to our business needs.

One would expect to filter the array like this:
```javascript
(array) => array.filter(element => CONDITION)
```

In fact we are currently forced to do it somehow like that:
```javascript
(array) => {
    for (let i = 0; i < array.length; i++) {
      const element = array[i];
      if(CONDITION)
        element.splice(i, 1);
    }
}
```

In addition, working directly on the references without letting people know that they change state for future invocations of the ResultDispatcher API, comes with more problems like increased boilerplate.
That's why we want to introduce **ResultDispatcher.transform**.

# Usage
Just like in the example above the usage is pretty straightforward:
```javascript
        document.addEventListener("ffReady", function ({resultDispatcher}) {
            const {transform} = resultDispatcher;
            transform((Phase, Pipeline) => ({
                topic: "asn",
                phase: Phase.BeforeMiddleware,
                onException: Pipeline.Skip, // default Pipeline.Halt
                handler(oldState, event) {
                    if (oldState.isSchrott)
                        return Pipeline.Skip; // Skip this transform call
                    else if (oldState.isDoubleSchrott)
                        return Pipeline.Halt; // stop further propagation i.e. the data will not be dispatched to any further callback or middleware and therefore doesn't reaches any custom element
                    else
                        return {...oldState}; // return new state
                }
            }));
        });
```

Retrieve old state, return new state. 
In addition it's possible to explicitly `Skip` the execution or `Halt` the entire process chain. 

**It is not possible to return the same reference of data.** 

If the same reference is returned, `Halt` the execution and throw an appropriate error.**

### Phase
* **BeforeMiddleware** 
* **AfterMiddleware**

### Pipeline
* **Halt** This will stop all further execution of callbacks and middleware. The data will not be dispatched to any custom element.
* **Skip** Return this state if you do not want change the state.

## Promise based Usage
It's a common use case to enrich data from 3rd party services. Therefore the transform function should prepare to accept promises. 
If a promise is retrieved the execution is paused until the promise resolves, which will trigger the next step with the new state or the promise is rejected which will trigger the next step with the old state.
```javascript
 document.addEventListener("ffReady", function ({resultDispatcher}) {
            const {transform} = resultDispatcher;
            transform((Phase, Pipeline) => ({
                topic: "asn",
                phase: Phase.BeforeMiddleware,
                onException: Pipeline.Skip, // default Pipeline.Halt
                handler(oldState, event) {
                    return new Promise((resolve, reject) => {
                        if (oldState.isSchrott)
                            resolve(Pipeline.Skip); // Skip this transform call
                        else if (oldState.isDoubleSchrott)
                            resolve(Pipeline.Halt); // stop further propagation i.e. the data will not be dispatched to any further callback or middleware and therefore doesn't reaches any custom element
                        else
                            resolve({...oldState}); // return new state
                    })
                }
            }));
        });
```

# Exceptions
If an exception is thrown in a user supplied callback the exception shall be caught and the name of the callback function, if any, and the error should be logged to the browser console in an error fashion style.
An exception shall not break the transform execution nor should it stop the ResultDispatching process. Treat an exception like a `Pipeline.Skip`.

**It's important to highlight the exception as a user caused exception.** 

# Warnings
If the old state is not undefined and the transform callback returns undefined a warning should be logged telling the developer that this might break things.

# Special Cases
1. `undefined` is dispatched
A transform callback shall not be executed if the new value is `undefined`. This is duo to the fact that `undefined` is used to hide element's and is therefore not considered a valid use case for the transform API.   

# Scope - transform, addCallback, subscribe 
In the future 'transform' is going to replace 'addCallback' entirely and we want to provide a smooth transition from addCallback.

The execution of transform callbacks shall take place before addCallback callbacks which themselfs take place before subscribe callbacks.
The functionality of addCallback and subscribe have to be retained, i.e. references are passed to them.

It's planned to pass deep copies to subscribe in a future breaking change release which will render them useless for data manipulation but as of now too many people are relying on addCallback and subscribe to change data.
 