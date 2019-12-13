# Nodemailer + PowerMTA

Implement the PowerMTA Mailmerge SMTP extensions over Nodemailer.

Recipients Mailmerge values are stored in a JS Map and passed
as a parameter with the message.


```javascript
'use strict';

const nodemailer = require("./lib/nodemailer");

const transporter = nodemailer.createTransport({
	host: "your-power-mta-host",
	port: 11111,
	auth: {
		user: "xXxXxXxX",
		pass: "yYyYyYyY"
	}
});

let recipients_data = new Map();

recipients_data.set("recipient_1@example.com", [
    ["id", 123456],
    ["firstname", "Sponge"]

]);

recipients_data.set("recipient_2@example.com", [
    ["id", 456789],
    ["firstname", "Bob"]

]);

let message = {
    envelope: {
        from: "sender@yourdomain.com",
        to: [...recipients_data.keys()],
        recipients_data: recipients_data
    },
    from: "sender@yourdomain.com",
    subject: "A special message to [*to]",
    html: "<p>Hello [firstname],<br>Your email unique ID is [id]</p>",
    text: "Hello [firstname],\nYour email unique ID is [id]"
};

transporter.sendMail(message);
```