Your data will not disappear if you reload while you are using context api.
#### Steps

1. Fetch the data when login and save the data in local storage by the below method
```javascript
try {
      const config = {
        headers: {
          "Content-type": "application/json",
        },
      };
      const { data } = await axios.post("http://localhost:5000/api/auth/login", { email, password }, config);
      toast.success("Login Successful");
      // setUser(data.data);
      localStorage.setItem("userInfo", JSON.stringify(data.data));
      setLoading(false);
      navigate("/chats");
    } catch (error) {
      toast.error("Error Occured!");
      setLoading(false);
    }
```
2. Now inside the context provider paste the following code
```javascript
import React, { useState, useEffect, createContext, useContext } from "react";
import { useNavigate } from "react-router-dom";
import { toast } from "react-toastify";

const UserContext = createContext();

const UserContextProvider = ({ children }) => {
  const [user, setUser] = useState(null);

  const navigate = useNavigate();

  useEffect(() => {
    const handleUrlChange = () => {
      // Your logic to run when there is a change in the URL
      console.log("URL has changed:", window.location.href);
    };
    window.addEventListener("popstate", handleUrlChange);
    const userInfoString = localStorage.getItem("userInfo");
    if (userInfoString) {
      try {
        const userInfo = JSON.parse(userInfoString);
        setUser(userInfo);
      } catch (error) {
        console.error("Error parsing userInfo:", error);
      }
    } else {
      const url = window.location.href.split("/").pop();
      if (url !== "about_us" && url !== "#why-skill-swap" && url === "") {
        navigate("/login");
      }
    }
    return () => {
      window.removeEventListener("popstate", handleUrlChange);
    };
  }, [window.location.href]);

  return <UserContext.Provider value={{ user, setUser }}>{children}</UserContext.Provider>;
};

const useUser = () => {
  return useContext(UserContext);
};

export { UserContextProvider, useUser };
```
3. Now use the user context api state anywhere you want without fear of reloading and data disappearing
4. Do not forget to remove the user data from local storage when logging out. 