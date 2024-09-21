```javascript
import Container from "react-bootstrap/Container";
import Navbar from "react-bootstrap/Navbar";

const Footer = () => {
  return (
    <>
      <Navbar sticky="bottom" bg="primary" data-bs-theme="dark">
        <Container className="mx-auto w-100 d-flex justify-content-center">
          <div className="text-center">Copyright &copy; 2024 SkillSwap. All rights reserved.</div>
        </Container>
      </Navbar>
    </>
  );
};

export default Footer;

```


### How to make Footer not collapse
set height of the page to 91vh

**Inside app.jsx**
```javascript
const App = () => {
  return (
    <>
      <Header />
      <Routes>
        <Route path="/" element={<LandingPage />} />
        <Route path="/login" element={<Login />} />
        <Route path="/register" element={<Register />} />
        <Route path="/discover" element={<Discover />} />
        <Route path="/about_us" element={<AboutUs />} />
        <Route path="/why_skillswap" element={<WhySkillSwap />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
      <Footer />
    </>
  );
};
```

**Inside login page**
```javascript
import Button from "react-bootstrap/Button";
import { FaGoogle } from "react-icons/fa";

const Login = () => {
  const handleGoogleLogin = () => {
    window.location.href = "http://localhost:8000/auth/google";
  };

  return (
    <>
      <div style={{ height: "90vh" }} className="d-flex justify-content-center align-items-center ">
        <div style={{ height: "300px" }} className="d-flex flex-column justify-content-between p-3 border rounded">
          <h1 style={{ textDecoration: "underline" }} className="text-center">
            Login
          </h1>
          <div className="h-100 d-flex justify-content-center align-items-center">
            <Button variant="primary" className="mx-auto" onClick={handleGoogleLogin}>
              <FaGoogle />
              &nbsp; Login with Google
            </Button>
          </div>
        </div>
      </div>
    </>
  );
};
export default Login;
```