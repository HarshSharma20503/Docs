## Website Notification (React)

### Steps

1. Set up the firebase project and copy the configuration file.
2. Create a file named firebase-messaging-sw.js in the public folder of the source code. write the following code.
```javascript
importScripts(
  "https://www.gstatic.com/firebasejs/9.16.0/firebase-app-compat.js"
);
importScripts(
  "https://www.gstatic.com/firebasejs/9.16.0/firebase-messaging-compat.js"
);

var firebaseConfig = {
  apiKey: "AIzaSyD6YDqr6n-xQIJgg3MH6Zwb511LeuY_Faw",
  authDomain: "red-alert-a6d6f.firebaseapp.com",
  projectId: "red-alert-a6d6f",
  storageBucket: "red-alert-a6d6f.appspot.com",
  messagingSenderId: "373170844915",
  appId: "1:373170844915:web:828a276a71efbbfd547386",
};

firebase.initializeApp(firebaseConfig);
const messaging = firebase.messaging();

messaging.onBackgroundMessage((payload) => {
  console.log("Received background message ", payload);

  const notificationTitle = payload.data.title;
  const notificationOptions = {
    body: payload.data.body,
    icon: "./images/logo.png",
    vibrate: [200, 100, 200, 100, 200, 100, 200],
    tag: "test-tag",
    data: {
      url: "https://web.dev/push-notifications-overview/",
    },
  };

  return self.registration.showNotification(
    notificationTitle,
    notificationOptions
  );
});
```

This code sets up Firebase Cloud Messaging (FCM) for a web application to handle background notifications. It initialises Firebase with the provided configuration and listens for incoming messages while the app is in the background. When a background message is received, it displays a notification with a title and body, along with an icon and vibration pattern. The notification also includes a URL for further action when clicked.

3. Write the following code to request for notification and get the notification token required for pushing notification.
```typescript
  const requestNotificationPermission = async () => {
    try {
      const permission = await Notification.requestPermission();
      if (permission === "granted") {
        console.log("Notification permission granted");
        const notificationToken = await getToken(messaging, {
          vapidKey: import.meta.env.VITE_VAPID_KEY,
        });
        console.log("Token", notificationToken);
        // const response = await apiCall({
        //   url: "/api/user/notification-token",
        //   method: "POST",
        //   data: { notificationToken },
        // });
        // console.log("Response", response);
      } else if (permission === "denied") {
        toast.error(
          "Please allow notification permission to get notified about stock updates"
        );
      }
    } catch (err) {
      console.log("Error in requesting notification permission");
      console.log(err);
    }
  };

	useEffect(()=>{
		requestNotificationPermission();
	},[])
```
This code defines an asynchronous function, requestNotificationPermission, that requests permission from the user to send notifications. If granted, it retrieves a notification token using Firebase Cloud Messaging and logs it to the console. If permission is denied, it displays an error message prompting the user

you can store the token in the database and use that token to send notification to the users.

## Send Notification from server





## Troubleshooting
1. Make sure to allow the notifications in browser and operating system.