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
ResultDispatcher.transform("topic", (oldState) => {
                                        ...
                                        return newState;
                                    });
```

Retrieve old state, return new state.

# Exceptions
If an exception is thrown in a user supplied callback the exception shall be catched and the name of the callback function, if any, and the error should be logged to the browser console in an error fashion style.
An exception shall not break the transform execution nor should it stop the ResultDispatching process.

# Warnings
If the old state is not undefined and the transform callback returns undefined a warning should be logged telling the developer that this might break things.