# Installation

Install the package

```console
npm install react-toastify
```

---
# Usage

Inside your main file (App.jsx)

```javascript
import { ToastContainer } from 'react-toastify';
import "react-toastify/dist/ReactToastify.css";

function App() {
  return (
    <div className="App">
      <ToastContainer position='top-center'/>
    </div>
);
}
```

Now to show toast you can import toast and use it wherever you want

```javascipt
import { toast} from 'react-toastify';

function Component() {
  return (
    <div className="Section">
      toast.info("Loged in Successfully");
      toast.error('Enter College Email-ID (Should have @mail in it)');
      toast.success("Logging successfull");
    </div>
);
```

---
# Customisation

For Customisation you can add property to the ToastContainer or toast itself

```
<ToastContainer
	position="top-right"
	autoClose={2000}
	hideProgressBar={false}
	newestOnTop={false}
	closeOnClick
	rtl={false}
	pauseOnFocusLoss
	draggable
	pauseOnHover
	theme="light"
/>
toast('🦄 Wow so easy!', {
	position: "top-right",
	autoClose: 5000,
	hideProgressBar: false,
	closeOnClick: true,
	pauseOnHover: true,
	draggable: true,
	progress: undefined,
	theme: "light",
});
```

---
# Reference

[npm package docs](https://www.npmjs.com/package/react-toastify)
