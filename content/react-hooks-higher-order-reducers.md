---
title: Higher Order Reducers with React Hooks
date: '2019-06-22'
path: '/react-hooks-higher-order-reducers/'
image: './img/higher-order-reducer.jpg'
description: 'Explore how higher order functions and reducers can make custom React hooks more flexible and re-useable.'
featured: true
imagewidth: 920
imageheight: 503
---

When managing React state with the new `useReducer` hook, you may find that there are commonly repeated action types and logic in your reducer functions. Here is an easy way of using higher order functions to make your custom hooks and reducer logic more flexible and re-useable.

To demonstrate this, let's pretend that we're fetching some `Todos` and will be controlling the loading and error states. We will also be able to delete a todo using the `id`.

```javascript
import React, { useEffect, useReducer } from 'react';

const initialState = {
  loading: false,
  error: false,
  data: []
};

function todosReducer(state, action) {
  switch (action.type) {
    case 'LOADING':
      return {
        ...state,
        loading: action.loading // Should be true/false
      };
    case 'ERROR':
      return {
        ...state,
        loading: false,
        error: action.error
      };
    case 'SET_DATA':
      return {
        loading: false,
        error: false,
        data: action.data
      };
    case 'DELETE_DATA':
      return {
        ...state,
        data: state.data.filter(datum => datum.id !== action.id)
      };
    default:
      return {
        ...state
      };
  }
}

const TodosPage = () => {
  const [state, dispatch] = useReducer(todosReducer, initialState);
  return (
    <div>
      {state.data.map(todo => (
        <TodoComponent key={todo.id} />
      ))}
    </div>
  );
};

export default TodosPage;
```

To keep this example simple, I'm not going to actually fetch any data, we'll just pretend that it looks something like this:

```javascript
// Sample Todos Data
const todos = [
  {
    id: 1,
    title: 'Go Shopping'
  },
  {
    id: 2,
    title: 'Go To Gym'
  }
];
```

This is pretty standard when dealing with fetching data of any kind. If there are multiple pages that need this reducer logic, we can pull it out into a custom hook.

```javascript
// Our useFetchData Custom Hook
import React, { useEffect, useReducer } from 'react';

const initialState = {
  loading: false,
  error: false,
  data: []
};

function dataReducer(state, action) {
  switch (action.type) {
    case 'LOADING':
      return {
        ...state,
        loading: action.loading
      };
    case 'ERROR':
      return {
        ...state,
        loading: false,
        error: action.error
      };
    case 'SET_DATA':
      return {
        loading: false,
        error: false,
        data: action.data
      };
    case 'DELETE_DATA':
      return {
        ...state,
        data: state.data.filter(datum => datum.id !== action.id)
      };
    default:
      return {
        ...state
      };
  }
}

const useFetchData = ({ url }) => {
  const [state, dispatch] = useReducer(dataReducer, initialState);

  useEffect(() => {
    const getInitialData = async () => {
      try {
        const response = await fetch(url);
        const data = await response.json();
        dispatch({
          type: 'SET_DATA',
          data
        });
      } catch (error) {
        dispatch({ type: 'ERROR', error });
      }
    };
    getInitialData();
  }, [url]);

  return [state, dispatch];
};

export default useFetchData;
```

To use the custom hook in the original `TodosPage` looks like this:

```javascript
import useFetchData from '../hooks/useFetchData';

const TodosPage = () => {
  const [state, dispatch] = useFetchData({
    url: 'https://someTodosApi'
  });

  return (
    <div>
      {state.data.map(todo => (
        <TodoComponent key={todo.id} />
      ))}
    </div>
  );
};
```

So far we haven't done anything tricky yet. If we have a different page, we can easily re-use the custom hook by passing in a different url for the api. However, what if on the other page the data looks a bit different? Instead of `Todos`, what if there were `Contacts` that needs to be displayed and deleted?

```javascript
// Sample Contacts Data
const contacts = [
  {
    contactId: 1,
    name: 'John Doe'
  },
  {
    contactId: 2,
    name: 'Jane Doe'
  }
];
```

Notice how the keys are now `contactId` instead of just `id`. This is just one of many examples of how data can be slightly different. We can still use most of our custom hook, but when we go to delete the data, we'll need to use `contactId` instead of `id`.

```javascript
case 'DELETE_DATA':
  return {
    ...state,
    data: state.data.filter(datum => datum.contactId !== action.id) // highlight-line
  };
```

How can we tweek just this tiny part of our custom hook so that we can re-use it? Well, since a reducer is _just a function_, we can call upon the power of higher order functions in Javascript by having our `dataReducer` function return another function. Some call this a higher order _reducer_.

What we want is, on the Contacts page, to pass in a string of whatever the key is so we can filter on that string, instead of the hard coded `id` that is currently in our hook.

```javascript
// Contacts Page
const ContactsPage = () => {
  const [state, dispatch] = useFetchData({
    url: 'https://someContactsApi',
    recordKey: 'contactId' // highlight-line
  });

  return (
    <div>
      {state.data.map(contact => (
        <ContactComponent key={contact.contactId} />
      ))}
    </div>
  );
};
```

We'll need to adjust our custom hook to take in this new `recordKey` variable and use it in our `dataReducer`.

```javascript
import React, { useEffect, useReducer } from 'react';

const initialState = {
  loading: false,
  error: false,
  data: []
};

// highlight-start
function dataReducer(recordKey) {
  return function(state, action) {
    // highlight-end
    switch (action.type) {
      case 'LOADING':
        return {
          ...state,
          loading: action.loading
        };
      case 'ERROR':
        return {
          ...state,
          loading: false,
          error: action.error
        };
      case 'SET_DATA':
        return {
          loading: false,
          error: false,
          data: action.data
        };
      case 'DELETE_DATA':
        return {
          ...state,
          data: state.data.filter(datum => datum[recordKey] !== action.id) // highlight-line
        };
      default:
        return {
          ...state
        };
    }
  };
}

// highlight-start
const useFetchData = ({ url, recordKey }) => {
  const [state, dispatch] = useReducer(dataReducer(recordKey), initialState);
  // highlight-end

  useEffect(() => {
    const getInitialData = async () => {
      try {
        const response = await fetch(url);
        const data = await response.json();
        dispatch({
          type: 'SET_DATA',
          data
        });
      } catch (error) {
        dispatch({ type: 'ERROR', error });
      }
    };
    getInitialData();
  }, [url]);

  return [state, dispatch];
};

export default useFetchData;
```

Our custom hook can now handle any kind of keys we throw at it! This was a pretty simple example, but keep in mind we can pass _anything_ into our higher order reducer and have the conditional logic live inside the returned reducer function. With React hooks, it's a lot easier to recognize common logic that is shared between components. It's also easier to re-use component logic and share that throughout your application.
