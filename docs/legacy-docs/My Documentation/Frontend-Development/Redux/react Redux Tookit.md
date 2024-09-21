# Installation
```console
npm install @reduxjs/toolkit react-redux
```

# Steps

1. Create a app folder in the root file and create a store file 
```javascript
import {configureStore} from '@reduxjs/toolkit';
import todoReducer from '../features/todo/todoSlice';

export const store = configureStore({
    reducer: todoReducer
})
```

2. Create a feature folder inside that create a todoSlice.js 
```javascript
import { createSlice, nanoid } from '@reduxjs/toolkit';

// Initial state with a sample todo item
const initialState = {
    todos: [{ id: 1, text: "Hello world" }]
}

// Create a slice of the Redux store for todos
export const todoSlice = createSlice({
    name: 'todo', // Name of the slice
    initialState, // Initial state for the slice
    reducers: {
        // Reducer to add a new todo
        addTodo: (state, action) => {
            const todo = {
                id: nanoid(), // Generate a unique ID for the new todo
                text: action.payload // Use the payload as the text of the todo
            }
            state.todos.push(todo) // Add the new todo to the state's todos array
        },`
        // Reducer to remove a todo by ID
        removeTodo: (state, action) => {
            state.todos = state.todos.filter((todo) => todo.id !== action.payload)
            // Filter out the todo with the matching ID from the state's todos array
        },
    }
})

// Export the action creators for adding and removing todos
export const { addTodo, removeTodo } = todoSlice.actions

// Export the reducer to be included in the Redux store
export default todoSlice.reducer

```

3. Now wrap the component with the provider to use the store and its functionality inside the main.jsx
```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { Provider } from 'react-redux'
import {store} from './app/store'

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>,
)
```


4. Now to use the function create a component addTodo using the useDispatch
```javascript
import React, { useState } from 'react'
import { useDispatch } from 'react-redux'
import { addTodo } from '../features/todo/todoSlice' 

function AddTodo() {
    // Local state to manage input value
    const [input, setInput] = useState('')
    
    // useDispatch hook to get the dispatch function
    const dispatch = useDispatch()

    // Handler for form submission to add a new todo
    const addTodoHandler = (e) => {
        e.preventDefault() // Prevent the default form submission behavior
        dispatch(addTodo(input)) // Dispatch the addTodo action with the input value
        setInput('') // Clear the input field
    }

    return (
        <form onSubmit={addTodoHandler} className="space-x-3 mt-12">
            <input
                type="text"
                className="bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 text-base outline-none text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
                placeholder="Enter a Todo..."
                value={input} // Bind input value to the local state
                onChange={(e) => setInput(e.target.value)} // Update state on input change
            />
            <button
                type="submit"
                className="text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg"
            >
                Add Todo
            </button>
        </form>
    )
}
export default AddTodo
```

5. Use Use-selector by creating todo.jsx file 
```javascript
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { removeTodo } from '../features/todo/todoSlice'

function Todos() {
    // useSelector hook to get the todos from the Redux store
    const todos = useSelector(state => state.todos)
    
    // useDispatch hook to get the dispatch function
    const dispatch = useDispatch()

    return (
        <>
        <div>Todos</div>
        <ul className="list-none">
            {todos.map((todo) => (
                <li key={todo.id} // Unique key for each todo item>
                    <div className='text-white'>{todo.text}</div>
                    <button
                        onClick={() => dispatch(removeTodo(todo.id))} // Dispatch removeTodo action with todo id
                    >
                    Delete TODO
                    </button>
                </li>
            ))}
        </ul>
        </>
    )
}

export default Todos
```

```
```