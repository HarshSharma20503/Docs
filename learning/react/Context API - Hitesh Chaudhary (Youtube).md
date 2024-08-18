**Reference Link** : [Youtube Link](https://www.youtube.com/watch?v=aAcI_FdfkA8&t=12s)

This guide will walk you through setting up and using React Context for managing global state across your application.
### 1. Create a Context

Start by creating a context. This will allow you to manage and share data across components without having to pass props down manually through each component.

**File:** `UserContext.js`

```javascript
import React from 'react';
const UserContext = React.createContext();
export default UserContext;
```
### 2. Create a Context Provider

Next, create a provider component that will wrap around your application or specific components that need access to the shared context. This provider will store the data and provide it to any component that consumes the context.

**File:** `UserContextProvider.jsx`

```javascript
import React, { useState } from 'react';
import UserContext from './UserContext';

const UserContextProvider = ({ children }) => {
    const [user, setUser] = useState(null);

    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
};

export default UserContextProvider;
```

### 3. Wrap Your Application with the Context Provider

To make the context available throughout your application, wrap your main component (e.g., `App.js`) with the `UserContextProvider`.

**File:** `App.js`

```javascript
import React from 'react';
import UserContextProvider from './context/UserContextProvider';

const App = () => {
    return (
        <UserContextProvider>
            {/* Your components here */}
        </UserContextProvider>
    );
};

export default App;
```

### 4. Using the Context in Components

To use the context data in a component, you can use the `useContext` hook. This allows you to access and update the shared state.

**Example:** `Login.js`

```javascript
import React, { useContext } from 'react';
import UserContext from '../context/UserContext';

function Login() {
    const { setUser } = useContext(UserContext);

    // Example usage
    const handleLogin = () => {
        setUser({ name: 'John Doe' }); // Set user data on login
    };

    return (
        <button onClick={handleLogin}>
            Login
        </button>
    );
}

export default Login;
```

### Summary

- **Create Context:** Define your context using `React.createContext()`.
- **Context Provider:** Create a provider component that manages the state and provides it to the child components.
- **Wrap Application:** Use the context provider to wrap the components that need access to the context data.
- **Consume Context:** Use `useContext` to access and manipulate context data in your components.

This pattern can be extended to manage any global state in your React application effectively.