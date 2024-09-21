# Installation 
```console
npm i nodemailer dotenv
```

# Usage
- NOTE - Does not work on College WiFi
- To Send the mail from an id first you have to enable two factor authentication from the gmail account you want to send mail from.
- Go to the security and then click on Two Factor Authentication 
- Generate the App password key which will be used in the nodemailer auth code

- Create a .env file and have the following things
```javascript
emailID = harshsharma20503@gmail.com

APP_PASSWORD = ---- ---- ---- ----
```

- now you can use the nodemailer

```console
import nodemailer from 'nodemailer';
import path from 'path';
import dotenv from 'dotenv';

dotenv.config();

const transporter = nodemailer.createTransport({
    service: 'gmail',
    host: 'smtp.gmail.com',
    port: 465,
    auth: {
        user: process.env.emailID,
        pass: process.env.APP_PASSWORD
    }
});

const mailOptions = {
    from: {
        name: 'Harsh Sharma',
        address: process.env.USER
    },
    to: ['ha9999377507@gmail.com'],
    subject: 'Test Email',
    text: 'This is a test email sent using Nodemailer.',
    // http: '',
    // attachments: [
    //     {
    //         filename: 'test.txt',
    //         path: path.join(__dirname, 'test.pdf'),
    //         contentType: 'application/pdf'
    //     },
    //     {
    //         filename: 'test.txt',
    //         path: path.join(__dirname, 'test.txt'),
    //         contentType: 'text/plain'
    //     }
    // ]
};

const sendMail = async (mailOptions) => {
    try {
        await transporter.sendMail(mailOptions);
        console.log('Email sent successfully');
    }
    catch(error){
        console.log("Error ", error);
    }
}

sendMail(mailOptions);
```