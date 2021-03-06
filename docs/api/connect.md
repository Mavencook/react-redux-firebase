<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [createFirebaseConnect](#createfirebaseconnect)
-   [firebaseConnect](#firebaseconnect)

## createFirebaseConnect

Function that creates a Higher Order Component that
automatically listens/unListens to provided firebase paths using
React's Lifecycle hooks.
**WARNING!!** This is an advanced feature, and should only be used when
needing to access a firebase instance created under a different store key.

**Parameters**

-   `storeKey` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Name of redux store which contains
    Firebase state (state.firebase) (optional, default `'store'`)

**Examples**

_Basic_

```javascript
// this.props.firebase set on App component as firebase object with helpers
import { createFirebaseConnect } from 'react-redux-firebase'
// create firebase connect that uses another redux store
const firebaseConnect = createFirebaseConnect('anotherStore')
// use the firebaseConnect to wrap a component
export default firebaseConnect()(SomeComponent)
```

Returns **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** HOC that accepts a watchArray and wraps a component

## firebaseConnect

**Extends React.Component**

Higher Order Component that automatically listens/unListens
to provided firebase paths using React's Lifecycle hooks.

**Parameters**

-   `watchArray` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** Array of objects or strings for paths to sync
    from Firebase. Can also be a function that returns the array. The function
    is passed the current props and the firebase object.

**Examples**

_Basic_

```javascript
// props.firebase set on App component as firebase object with helpers
import { firebaseConnect } from 'react-redux-firebase'
export default firebaseConnect()(App)
```

_Data_

```javascript
import { compose } from 'redux'
import { connect } from 'react-redux'
import { firebaseConnect } from 'react-redux-firebase'

const enhance = compose(
  firebaseConnect([
    'todos' // sync /todos from firebase into redux
  ]),
  connect((state) => ({
    todos: state.firebase.ordered.todos
  })
)
// use enhnace to pass todos list as props.todos
const Todos = enhance(({ todos })) =>
  <div>
    {JSON.stringify(todos, null, 2)}
  </div>
)
```

_Data that depends on props_

```javascript
import { compose } from 'redux'
import { connect } from 'react-redux'
import { firebaseConnect } from 'react-redux-firebase'

const enhance = compose(
  firebaseConnect((props) => ([
    `posts/${props.postId}` // sync /posts/postId from firebase into redux
  ]),
  connect(({ firebase: { data } }, props) => ({
    post: data.posts && data.posts[postId],
  })
)

const Posts = ({ done, text, author }) => (
  <article>
    <h1>{title}</h1>
    <h2>By {author.name}</h2>
    <div>{content}</div>
  </article>
)

export default enhance(Posts)
```

_Data that depends on state_

```javascript
import { compose } from 'redux'
import { connect } from 'react-redux'
import { firebaseConnect } from 'react-redux-firebase'

export default compose(
  firebaseConnect((props, store) => ([
    `todos/${store.getState().firebase.auth.uid}`
  ]),
  connect(({ firebase: { data, auth } }) => ({
    todosList: data.todos && data.todos[auth.uid],
  }))
)(SomeComponent)
```

Returns **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** that accepts a component to wrap and returns the wrapped component
