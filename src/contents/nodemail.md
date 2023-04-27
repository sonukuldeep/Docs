---
author: internat
datetime: 2023-04-27
title: Node Mailer
slug: nodemail
featured: false
draft: false
tags:
  - smtp
ogImage: ""
description: How to send mail with nodemailer and smtp server
---

# Node Mailer

We are using node mailer to send emails from our server

## Table of contents

## Installation

- install express and other related stuff
- install node mailer with "npm i nodemailer"
- visit [docs](https://nodemailer.com/about/) for more info
- to add gmail smtp server [visit](https://nodemailer.com/usage/using-gmail/)
- create application specific password from [here](https://security.google.com/settings/security/apppasswords)
- use gamil and pplication password only if you did not buy smtp service from domain provider
- example use in express "app.get('/mail', sendMail)" sendMail is a controller here

## Default setup

```js
"use strict";
const nodemailer = require("nodemailer");

// async..await is not allowed in global scope, must use a wrapper
async function main() {
  // Generate test SMTP service account from ethereal.email
  // Only needed if you don't have a real mail account for testing
  let testAccount = await nodemailer.createTestAccount();

  // create reusable transporter object using the default SMTP transport
  let transporter = nodemailer.createTransport({
    host: "smtp.ethereal.email",
    port: 587,
    secure: false, // true for 465, false for other ports
    auth: {
      user: testAccount.user, // generated ethereal user
      pass: testAccount.pass, // generated ethereal password
    },
  });

  // send mail with defined transport object
  let info = await transporter.sendMail({
    from: '"Fred Foo 👻" <foo@example.com>', // sender address
    to: "bar@example.com, baz@example.com", // list of receivers
    subject: "Hello ✔", // Subject line
    text: "Hello world?", // plain text body
    html: "<b>Hello world?</b>", // html body - mail template can also be used here
  });

  console.log("Message sent: %s", info.messageId);
  // Message sent: <b658f8ca-6296-ccf4-8306-87d57a0b4321@example.com>

  // Preview only available when sending through an Ethereal account
  console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
  // Preview URL: https://ethereal.email/message/WaQKMgKddxQDoou...
}

main().catch(console.error);
```

## Google setup

```js
const nodemailer = require("nodemailer");

// async..await is not allowed in global scope, must use a wrapper
async function mailFunction() {
  try {
    // create reusable transporter object using the default SMTP transport
    let transporter = nodemailer.createTransport({
      service: "gmail",
      auth: {
        user: process.env.MAIL, // username
        pass: process.env.SMTP_PW, // password
      },
    });

    // send mail with defined transport object
    let info = await transporter.sendMail({
      from: `"Name here 👻" <${process.env.MAIL}>`, // from email must match mail from google smtp
      to: `${process.env.MAIL}`, // list of receivers
      subject: "Hello ✔", // Subject line
      text: "Hello world?", // plain text body
      html: "<b>Hello world?</b>", // html body - mail template can also be used here
    });

    // console logs
    console.log("Message sent: %s", info.messageId);
    console.log("Message accepted", info.accepted);
  } catch (error) {
    console.group(error.message);
  }
}

const sendMail = async (req, res) => {
  await mailFunction();
  res.send("msg sent");
};

module.exports = sendMail;
```

## useful blog post on nodemail and google smtp

[link](https://miracleio.me/snippets/use-gmail-with-nodemailer/)

<br>

## Mail template

```js
const mailTemplate = `
        <div>
            <h1>Welcome to XYZ Company</h1>
            <p>Dear User,</p>
            <p>
                We are thrilled to welcome you to XYZ Company As a valued customer,
                we want to make sure that you feel right at home from day one. We
                appreciate your trust in us and we're committed to providing you with
                excellent service and support.
            </p>
            <p>As a quick reminder, here are a few things you can expect from us:</p>
            <ul>
                <li>Quality products and services that meet your needs</li>
                <li>Clear and transparent communication</li>
                <li>Timely responses to your inquiries and concerns</li>
                <li>A friendly and knowledgeable team that is always here to help</li>
            </ul>
            <p>
                If you have any questions or concerns, please don't hesitate to reach out
                to us at <a href="mailto:example.com">xyz.com</a>. We're
                always happy to hear from you.
            </p>
            <p>
                Thank you for choosing us as your service provider.
                We look forward to serving you and building a long-lasting relationship
                with you.
            </p>
            <p>Best regards,</p>
            <p>xyz<br />xyz.com</p>
        </div>`;
```
