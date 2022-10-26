
[npm](https://www.npmjs.com/package/redux), [github](https://github.com/reduxjs/redux)

## Example - separate files

`actionBuilders.js`
```js
import * as actionTypes from './actionTypes';

export const bugAdded(description) => ({
  type: actionTypes.BUG_ADDED,
  payload: { description },
});

export const bugRemoved(id) => ({
  type: actionTypes.BUG_REMOVED,
  payload: { id },
});

export const bugResolved(id) => ({
  type: actionTypes.BUG_RESOLVED,
  payload: { id },
});
```

`actionTypes.js`
```js
export const BIG_ADDED = 'BUG_ADDED';
export const BUG_REMOVED = 'BUG_REMOVED';
export const BUG_RESOLVED = 'BUG_RESOLVED';
```

`reducer.js`
```js
import * as actionTypes from './actionTypes';

const prevId = 1;
const nextId = () => prevId++;

export default function reducer(state, action) {
  switch(action.type) {
    case actionTypes.BUG_ADDED:
      return [...state, { id: nextId(), description: action.payload.description, resolved: false }];
    case actionTypes.BUG_REMOVED:
      return state.filter((b) => bug.id !== action.payload.id);
    case actionTypes.BUG_RESOLVED:
      return state.map((b) => (b.id === action.payload.id ? { ...b, resolved: true } : b))
    default:
      return state;
  }
};
```

`store.js`
```js
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);

export default store;
```

`example.js`
```js
import store from './store';
import { bugAdded, bugRemoved, bugResolved } from './actionBuilders';

store.subscribe(() => {
  console.log(`store changed >> ${JSON.stringify(store.getState())}`);
});

store.dispatch(bugAdded('DESCRIPTION'));
store.dispatch(bugResolved(1));
store.dispatch(bugRemoved(1));
```

## Example - one file

`bugStore.js`
```js
import { createStore } from 'redux';

export const BIG_ADDED = 'BUG_ADDED';
export const BUG_REMOVED = 'BUG_REMOVED';
export const BUG_RESOLVED = 'BUG_RESOLVED';

const prevId = 1;
const nextId = () => prevId++;

export function reducer(state, action) {
  switch(action.type) {
    case BUG_ADDED:
      return [...state, { id: nextId(), description: action.payload.description, resolved: false }];
    case BUG_REMOVED:
      return state.filter((b) => bug.id !== action.payload.id);
    case BUG_RESOLVED:
      return state.map((b) => (b.id === action.payload.id ? { ...b, resolved: true } : b))
    default:
      return state;
  }
};

export const store = createStore(reducer);
export default store;

export const bugAdded(description) => ({
  type: BUG_ADDED,
  payload: { description },
});

export const bugRemoved(id) => ({
  type: BUG_REMOVED,
  payload: { id },
});

export const bugResolved(id) => ({
  type: BUG_RESOLVED,
  payload: { id },
});
```
