const express = require("express");
const nodemailer = require("nodemailer");
const bodyParser = require("body-parser");
const cors = require("cors");

const app = express();
const PORT = 3000;

app.use(cors());
app.use(bodyParser.json());

// Configure your email transporter
const transporter = nodemailer.createTransport({
    service: "gmail",
    auth: {
        user: "gsobi77@gmail.com", // Your Gmail address
        pass: "your_email_password" // Replace with your app-specific password
    }
});

app.post("/send-email", (req, res) => {
    const { name, email, subject, message } = req.body;

    const mailOptions = {
        from: email,
        to: "gsobi77@gmail.com",
        subject: `New Message from ${name}: ${subject}`,
        text: `You received a new message:\n\n${message}\n\nFrom: ${name} (${email})`
    };

    transporter.sendMail(mailOptions, (error, info) => {
        if (error) {
            console.error(error);
            res.status(500).send("Failed to send email.");
        } else {
            console.log("Email sent: " + info.response);
            res.status(200).send("Email sent successfully.");
        }
    });
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
