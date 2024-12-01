## Website Notification (React)

### Steps

1. Create firebase config file firebase.js and write the following code
```javascript
import { initializeApp } from "firebase/app";
import { getMessaging } from "firebase/messaging";

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
};

export const app = initializeApp(firebaseConfig);
export const messaging = getMessaging(app);
```
2. Set up the firebase project and copy the configuration file.
3. Create a file named firebase-messaging-sw.js in the public folder of the source code. write the following code.
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
4. Set the vapid key, you can get this from the project settings > cloud messaging > generate key pair and get the key.

5. Write the following code to request for notification and get the notification token required for pushing notification.
```typescript
  import { getToken } from "firebase/messaging";
  import { messaging } from "../../config/firebase";

    const requestNotificationPermission = async () => {
    try {
      const permission = await Notification.requestPermission();
      if (permission === "granted") {
        const vapidKey = import.meta.env.VITE_VAPID_KEY;
        if (!vapidKey) {
          console.error("VAPID key is not defined in the environment file.");
          return;
        }
        const notificationToken = await getToken(messaging, { vapidKey });
        if (notificationToken) {
          console.log("Notification Token:", notificationToken);
          const token = Storage.getData("token");
          const headers = {
            Authorization: `Bearer ${token}`,
          };
          const response = await apiCall(
            "POST",
            "api/v1/user/saveNotificationToken",
            { notificationToken },
            headers
          );
          if (response.success) {
            console.log("Notification token saved successfully.");
          } else {
            console.error("Failed to save notification token.");
          }
        } else {
          console.error("Failed to retrieve notification token.");
        }
      } else {
        toast.error(
          "Please allow notification permission to get notified about stock updates."
        );
      }
    } catch (err) {
      console.error("Error in requesting notification permission:", err);
    }
  };

  useEffect(() => {
    requestNotificationPermission();
  }, []);
```
This code defines an asynchronous function, requestNotificationPermission, that requests permission from the user to send notifications. If granted, it retrieves a notification token using Firebase Cloud Messaging and logs it to the console. If permission is denied, it displays an error message prompting the user

you can store the token in the database and use that token to send notification to the users.

`Note`: If you are using brave browser open the privacy and security settings and opt for allow google push messaging.

## Send Notification from server

1. Install `firebase-admin package`
```bash
npm install firebase-admin
```

2. Download the service account json file from firebase console under project settings and service account.
3. Initialise admin using the service account 

```javascript
import admin from "firebase-admin";
import serviceAccount from "../firebase-service-account-config.json" with {type: "json"};

const initializeFirebase = () => {
  try {
    admin.initializeApp({
      credential: admin.credential.cert(serviceAccount),
    });
    console.log("Firebase initialized successfully.");
    return admin;
  } catch (error) {
    console.error("Error initializing Firebase:", error);
    throw error; // Optionally rethrow the error if initialization should stop on failure
  }
};

export default initializeFirebase;
```

4. Send notification 
```javascript 
const sendNotification = async (token, admin) => {
  const message = {
    notification: {
      title: "Test message",
      body: "This is a test notification",
    },
    token: token, // Add the token here
  };

  try {
    const response = await admin.messaging().send(message); // Pass only the message object
    console.log("Notification sent successfully:", response);
  } catch (error) {
    console.error("Error sending notification:", error);
  }
};

const admin = initializeFirebase();
// Example usage of sendNotification
const deviceToken = "e2UjYjNlR9"; // Replace with the actual device token
await sendNotification(deviceToken, admin);
```

## Troubleshooting
1. Make sure to allow the notifications in browser and operating system.