1. Create a project in google console
2. Create a oauth consent screen with the test user as yourself
3. Create a oauth ID with the desktop as the service and save it as `oauth_credentials.json`

```javascript
// Taken reference from here https://github.com/abhishekchhibber/Gmail-Api-through-Python/tree/master

import fs from "fs/promises";
import path from "path";
import { authenticate } from "@google-cloud/local-auth";
import process from "process";
import { google } from "googleapis";

// If modifying these scopes, delete token.json.
const SCOPES = ["https://www.googleapis.com/auth/gmail.readonly"];
// The file token.json stores the user's access and refresh tokens, and is
// created automatically when the authorization flow completes for the first
// time.
console.log("Process.cwd: ", process.cwd());
const TOKEN_PATH = path.join(process.cwd(), "token.json");
const CREDENTIALS_PATH = path.join(process.cwd(), "oauth_credentials.json");

/**
 * Reads previously authorized credentials from the save file.
 *
 * @return {Promise<OAuth2Client|null>}
 */
async function loadSavedCredentialsIfExist() {
  console.log("Inside Load Saved Credentials Function");
  try {
    const content = await fs.readFile(TOKEN_PATH);
    // console.log("Content: ", content);
    const credentials = await JSON.parse(content);
    // console.log("Credentials", credentials);
    return google.auth.fromJSON(credentials);
  } catch (err) {
    return null;
  }
}

/**
 * Serializes credentials to a file compatible with GoogleAuth.fromJSON.
 *
 * @param {OAuth2Client} client
 * @return {Promise<void>}
 */
async function saveCredentials(client) {
  console.log("Inside Saving credentials Function");

  const content = await fs.readFile(CREDENTIALS_PATH);
  const keys = JSON.parse(content);
  const key = keys.installed || keys.web;
  const payload = JSON.stringify({
    type: "authorized_user",
    client_id: key.client_id,
    client_secret: key.client_secret,
    refresh_token: client.credentials.refresh_token,
  });
  await fs.writeFile(TOKEN_PATH, payload);
}

/**
 * Load or request or authorization to call APIs.
 *
 */
async function authorize() {
  console.log("Inside Authorize Function");
  let client = await loadSavedCredentialsIfExist();
  // console.log("Client from Storage: ", client);
  if (client) {
    return client;
  }
  client = await authenticate({
    keyfilePath: CREDENTIALS_PATH,
    scopes: SCOPES,
  });
  // console.log("Client after authentication: ", client);
  if (client.credentials) {
    await saveCredentials(client);
  }
  return client;
}

/**
 * Lists the labels in the user's account.
 *
 * @param {google.auth.OAuth2} auth An authorized OAuth2 client.
 */
async function listMessages(auth) {
  console.log("Inside List Labels Function");

  const gmail = google.gmail({ version: "v1", auth });
  // console.log("Gmail: ", gmail.users.messages);
  // const res = await gmail.users.labels.list({
  //   userId: "me",
  // });
  const res = await gmail.users.messages.list({
    userId: "me",
    labelIds: ["INBOX", "UNREAD"],
  });
  // console.log("response: ", res);
  const messages = res.data.messages;
  if (!messages || messages.length === 0) {
    console.log("No messages found.");
    return;
  }
  console.log("-------------messages:-----------------");
  messages.forEach(async (message, index) => {
    // console.log(`${index + 1} ${message.id}`);
    const messageRes = await gmail.users.messages.get({
      userId: "me",
      id: message.id,
    });
    // console.log("Message Response: ", messageRes);
    const payload = messageRes.data.payload;
    // console.log("Payload: ", payload);
    const headers = payload.headers;

    for (let i = 0; i < headers.length; i++) {
      if (headers[i].name === "From") {
        console.log(headers[i].name, headers[i].value);
      }
      if (headers[i].name === "Subject") {
        console.log(headers[i].name, headers[i].value);
      }
      if (headers[i].name === "Date") {
        console.log(headers[i].name, headers[i].value);
      }
    }
    console.log("-------------------------------------------------");
  });
}

authorize().then(listMessages).catch(console.error);
```