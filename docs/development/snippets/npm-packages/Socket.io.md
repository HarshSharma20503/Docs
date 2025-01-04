# Socket IO Snippet

1. Install the socket.io package inside the backend folder `npm i socket.io`

2. inside the index.js create the socket server

    ```javascript
    import dotenv from "dotenv";
    import connectDB from "./config/connectDB.js";
    import { app } from "./app.js";
    import { Server } from "socket.io";

    dotenv.config();

    const port = process.env.PORT || 8000;

    connectDB()
      .then(() => {
        console.log("Database connected");
        const server = app.listen(port, () => {
          console.log(`Server listening on port ${port}`);
        });

        const io = new Server(server, {
          pingTimeout: 60000,
          cors: {
            origin: "*",
          },
        });

        io.on("connection", (socket) => {
          console.log("Connected to socket");
          socket.on("setup", (userData) => {
            console.log("Connected to socket in setup: ", userData.username);
            socket.join(userData._id);
            socket.emit("connected");
          });
          socket.off("setup", () => {
            console.log("Disconnected from socket");
            socket.leave(userData._id);
          });
        });
      })
      .catch((err) => {
        console.log(err);
      });

    ```

3. socket.on means creating a listener that will listen to the event from the client

4. connection is special listener

    ```javascript
    io.on("connection", (socket) => {
      console.log("Connected to socket");
      
    });
    ```

5. Corresponding the frontend will look like

    ```javscript
    socket = io("url of the backend");
    ```

6. After connection you can create different event listeners using the socket

    ```javascript
    io.on("connection", (socket) => {
      console.log("Connected to socket");
      
      socket.on("join chat", (room) => {
      console.log("Joining chat: ", room);
      socket.join(room);
      console.log("Joined chat: ", room);
      });

    });
    ```

7. Inside the frontend you can call for the event using emit

    ```javascript
    socket.emit("join chat", id);
    ```

8. similarly you can define event listeners in the frontend and than emit message or something to them from backend.
9. `io.to(id).emit("message recieved", newMessage)` can be used to send message to a particular room;

10. make sure to remove the event listeners using off

    ```javascript
    useEffect(() => {
      socket = io(axios.defaults.baseURL);
      if (user) {
        socket.emit("setup", user);
      }
      socket.on("connected", () => setSocketConnected(true));
      socket.on("typing", () => setIsTyping(true));
      socket.on("stop typing", () => setIsTyping(false));
      socket.on("message recieved", (newMessageRecieved) => {
        console.log("New Message Recieved: ", newMessageRecieved);
        console.log("Selected Chat: ", selectedChat);
        console.log("Selected Chat ID: ", selectedChat.id);
        console.log("New Message Chat ID: ", newMessageRecieved.chatId._id);
        if (selectedChat && selectedChat.id === newMessageRecieved.chatId._id) {
        setChatMessages((prevState) => [...prevState, newMessageRecieved]);
        }
      });
      return () => {
        socket.off("connected");
        socket.off("typing");
        socket.off("stop typing");
        socket.off("message recieved");
      };
    }, [selectedChat]);
    ```
