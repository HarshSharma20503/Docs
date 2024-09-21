Create a file named UserContext.js or any Context you need

```javascript
import React from 'react';

const UserContext = React.createContext();

export default UserContext;
```

Create a file named UserContextProvider.jsx  or any context provider you need

```javascript
import React, {userState} from 'react'

import UserContext from './UserContext'

const UserContextProvider = ({children}) =>{
	const [user, setUser] = userState(null);
	return (
		<UserContext.Provider value={{user, setUser}}>
			{children}
		</UserContext.Provider>
	)
}

export default UserContextProvider
```


Now wrap the component app.js with the context provider

```javascript
const App = () =>{
	return (
		<UserContextProvider>
			...
		</UserContextProvider>
	)
}
```

Now how to use this data

```javascript
import {useContext} from 'react'
import UserContext from '../context/UserContext'

function Login(){
	const {setUser} = useContext(UserContext)
}
```