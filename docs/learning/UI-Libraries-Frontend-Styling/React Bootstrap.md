## Installation

To get started with React Bootstrap, you need to install both react-bootstrap and bootstrap. Run the following commands to install them:
```bash
npm install react react-bootstrap
```

After installation, you need to import Bootstrap’s CSS in your project. Add the following line to your src/index.tsx or src/App.tsx file:

```typescript
import 'bootstrap/dist/css/bootstrap.min.css';
```

## Basic Components

### Navbar
The Navbar component provides a responsive navigation header.
```typescript
import React from 'react';
import { Navbar, Nav } from 'react-bootstrap';

const AppNavbar: React.FC = () => (
  <Navbar bg="dark" variant="dark" expand="lg">
    <Navbar.Brand href="#home">React Bootstrap</Navbar.Brand>
    <Navbar.Toggle aria-controls="basic-navbar-nav" />
    <Navbar.Collapse id="basic-navbar-nav">
      <Nav className="mr-auto">
        <Nav.Link href="#home">Home</Nav.Link>
        <Nav.Link href="#link">Link</Nav.Link>
      </Nav>
    </Navbar.Collapse>
  </Navbar>
);

export default AppNavbar;
```

### Modals
The Modal component is used to create dialogs or popups.
```typescript
import React, { useState } from 'react';
import { Button, Modal } from 'react-bootstrap';

const AppModal: React.FC = () => {
  const [show, setShow] = useState(false);

  const handleClose = () => setShow(false);
  const handleShow = () => setShow(true);

  return (
    <>
      <Button variant="primary" onClick={handleShow}>
        Launch Modal
      </Button>

      <Modal show={show} onHide={handleClose}>
        <Modal.Header closeButton>
          <Modal.Title>Modal Title</Modal.Title>
        </Modal.Header>
        <Modal.Body>Modal Body Text</Modal.Body>
        <Modal.Footer>
          <Button variant="secondary" onClick={handleClose}>
            Close
          </Button>
          <Button variant="primary" onClick={handleClose}>
            Save Changes
          </Button>
        </Modal.Footer>
      </Modal>
    </>
  );
};

export default AppModal;
```

### Form
The Form component allows you to create forms with various input types.
```typescript
import React from 'react';
import { Form, Button } from 'react-bootstrap';

const AppForm: React.FC = () => (
  <Form>
    <Form.Group controlId="formBasicEmail">
      <Form.Label>Email address</Form.Label>
      <Form.Control type="email" placeholder="Enter email" />
      <Form.Text className="text-muted">
        We'll never share your email with anyone else.
      </Form.Text>
    </Form.Group>

    <Form.Group controlId="formBasicPassword">
      <Form.Label>Password</Form.Label>
      <Form.Control type="password" placeholder="Password" />
    </Form.Group>

    <Form.Group controlId="formBasicCheckbox">
      <Form.Check type="checkbox" label="Check me out" />
    </Form.Group>

    <Button variant="primary" type="submit">
      Submit
    </Button>
  </Form>
);

export default AppForm;
```

### Dropdown
The Dropdown component allows users to select an option from a dropdown list.
```typescript
import React from 'react';
import { Dropdown } from 'react-bootstrap';

const AppDropdown: React.FC = () => (
  <Dropdown>
    <Dropdown.Toggle variant="success" id="dropdown-basic">
      Dropdown Button
    </Dropdown.Toggle>

    <Dropdown.Menu>
      <Dropdown.Item href="#action/1">Action</Dropdown.Item>
      <Dropdown.Item href="#action/2">Another action</Dropdown.Item>
      <Dropdown.Item href="#action/3">Something else here</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
);

export default AppDropdown;
```
