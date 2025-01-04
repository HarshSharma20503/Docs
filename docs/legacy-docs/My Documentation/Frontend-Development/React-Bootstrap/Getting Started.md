>[!SUMMARY]- Table of Contents
>- [[Getting Started#Installation|Installation]]
>- [[Getting Started#React-Bootstrap Components|React-Bootstrap Components]]
# Installation

Install package

```console
npm i react-bootstrap bootstrap
```

Import CSS file in the main.jsx

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./index.css";
import "bootstrap/dist/css/bootstrap.min.css";

ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```

Now you can start using react-bootstrap components.

---
# React-Bootstrap Components
[[Navbar]]
[Footer](./Footer)

## Flowbite-React Components

Navbar

```javascript
import React, { useState } from "react";
import { Button, Navbar } from "flowbite-react";
import { Link } from "react-router-dom";

const Header = () => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);

  return (
    <div className="relative">
      <Navbar
        fluid
        className="fixed w-full z-20 top-0 bg-white dark:bg-gray-900"
      >
        <Navbar.Brand>
          <Link to="/">
            <img
              src="https://spardha.org.in/images/logo/spardha-logo-white.png"
              className="mr-3 h-6 sm:h-9"
              alt="ShauryAutsav"
            />
          </Link>
          <span className="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
            Shauryautsav
          </span>
        </Navbar.Brand>
        <div className="flex md:order-2">
          <Link to="/register">
            <Button>Register</Button>
          </Link>
          <button
            onClick={() => setIsMenuOpen(!isMenuOpen)}
            className="md:hidden ml-3 p-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 rounded-lg"
          >
            <svg className="w-6 h-6" fill="currentColor" viewBox="0 0 20 20">
              <path
                fillRule="evenodd"
                d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                clipRule="evenodd"
              />
            </svg>
          </button>
        </div>

        {/* Desktop Menu */}
        <div className="hidden md:flex md:w-auto md:order-1">
          <div className="flex flex-col md:flex-row md:space-x-8 md:mt-0">
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/"
            >
              Home
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/about"
            >
              About Us
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/events"
            >
              Events
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/team"
            >
              Team
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/sponsors"
            >
              Sponsors
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/contact"
            >
              Contact Us
            </Link>
          </div>
        </div>

        {/* Mobile Menu */}
        <div
          className={`fixed top-0 right-0 h-full w-64 bg-white dark:bg-gray-900 shadow-lg transform transition-transform duration-300 ease-in-out ${
            isMenuOpen ? "translate-x-0" : "translate-x-full"
          } md:hidden`}
        >
          <div className="flex flex-col p-4 space-y-4">
            <button
              onClick={() => setIsMenuOpen(false)}
              className="self-end p-2 text-gray-500 hover:bg-gray-100 rounded-lg"
            >
              <svg
                className="w-6 h-6"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  strokeWidth="2"
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            </button>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/"
              onClick={() => setIsMenuOpen(false)}
            >
              Home
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/about"
              onClick={() => setIsMenuOpen(false)}
            >
              About Us
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/events"
              onClick={() => setIsMenuOpen(false)}
            >
              Events
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/team"
              onClick={() => setIsMenuOpen(false)}
            >
              Team
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/sponsors"
              onClick={() => setIsMenuOpen(false)}
            >
              Sponsors
            </Link>
            <Link
              className="text-gray-700 hover:text-blue-500 dark:text-white py-2"
              to="/contact"
              onClick={() => setIsMenuOpen(false)}
            >
              Contact Us
            </Link>
          </div>
        </div>
      </Navbar>

      {/* Overlay for mobile menu */}
      {isMenuOpen && (
        <div
          className="fixed inset-0 bg-black bg-opacity-50 z-10 md:hidden"
          onClick={() => setIsMenuOpen(false)}
        />
      )}
    </div>
  );
};

export default Header;
```