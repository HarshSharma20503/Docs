>[!SUMMARY]- Table of Contents
>- [[Setup Postman#Launch Postman: |Launch Postman: ]]
>- [[Setup Postman#Create a Collection:|Create a Collection:]]
>- [[Setup Postman#Create Requests:|Create Requests:]]
>- [[Setup Postman#Set Environment Variables:|Set Environment Variables:]]
>- [[Setup Postman#Use Environment Variables in Requests:|Use Environment Variables in Requests:]]
>- [[Setup Postman#Send Requests:|Send Requests:]]
# Launch Postman: 

Once installed, launch the Postman application.

---
# Create a Collection:

- Click on the "Collections" tab on the left sidebar.
- Click on the "New Collection" button.
- Give your collection a name and optionally provide a description.
- Click "Create" to create the collection.
---
# Create Requests:

- Inside your newly created collection, click on the "Add Request" button.
- Give your request a name and specify the HTTP method (e.g., POST, GET, PUT, DELETE).
- Enter the request URL.
- Add any necessary headers, request body, query parameters, etc.
- Click "Save" to save the request to your collection.
---
# Set Environment Variables:

- Click on the gear icon in the top-right corner to access settings.
- Select "Manage Environments" from the dropdown menu.
- Click on "Add" to create a new environment.
- Give your environment a name (e.g., Development, Production).
- Add variables by providing a key-value pair (e.g., baseURL: http://localhost:3000).
- Click "Add" to save the environment.
---
# Use Environment Variables in Requests:

- While editing a request, use double curly braces to enclose the variable name (e.g., {{baseURL}}/api/v1/users/register).
- Postman will replace these variables with their corresponding values when sending requests.
---
# Send Requests:

- Click on the request you want to send from your collection. 
- If necessary, select the appropriate environment from the dropdown menu in the top-right corner.
- Click on the "Send" button to send the request.
- Postman will display the response received from the server.
