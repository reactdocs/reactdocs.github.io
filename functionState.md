#### Function Component has No State

> A function component is just a javascript function that taken in props optionally and returns some JSX.

#### State is added to Function Component using Hooks

> Recently, hooks added the new way for Function Components to use State within them.

*Example-*

```javascript
import React, { useState } from 'react';

function ExampleFunctionComponent () {
    const [reader, setReader] = useState('Student');

    return (
        <h1 onClick={() => setReader('Clicker')}>Hello, {reader}</h1>
        );
}
```

#### State hook: useState

> useState returns the current state value and the function to change the value, and accepts the initial state value.


#### Getting State

> Note above how Function components directly use the value like ```reader``` to access state properties.

#### Setting State

> Directly using the function returned by useState hook like ```setReader('Clicker')``` above.


Previous [Class Component State](classState.md) | Next: [Lifecycle Methods](lifecycle.md)

