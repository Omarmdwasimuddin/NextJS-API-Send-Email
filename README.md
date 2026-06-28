## NextJS API--->Send Email

#### Visit---> https://www.npmjs.com/package/nodemailer
### install
```bash
npm i nodemailer
```
---
### install for typescript
```bash
npm install --save-dev @types/nodemailer
```
### short version
```bash
npm i -D @types/nodemailer
```
---

### api/email/route.js
![](https://imgur.com/sHKAN3c.png)


```bash
import { NextRequest, NextResponse } from "next/server";
import nodemailer from "nodemailer";


export async function GET(request: NextRequest) {

    const {searchParams} = new URL(request.url);
    const ToEmail = searchParams.get("email");

    if (!ToEmail) {
        return NextResponse.json(
            { message: "Email query parameter is required" },
            { status: 400 }
        );
    }


    // Transporter
    let Transporter = nodemailer.createTransport({
        host:"mail.teamrabbil.com",
        port:25,
        secure:false,
        auth: {
            user:"info@teamrabbil.com",
            pass:"sR4[bhaC[Qs",
        },
        tls:{ rejectUnauthorized: false }
    })

    // Prepar Email
    let myEmail = {
        from:"Test email from next.js application<info@teamrabbil.com>",
        to:ToEmail,
        subject:"Test email from next.js application",
        text:"Test email from next.js application..."
    }


    try {
        await Transporter.sendMail(myEmail);
        return NextResponse.json(
            {message: "success"}
        )
    } catch (error) {
        console.log("Failed messaging", error)
        return NextResponse.json(
            {message: "Fail!"}
        )
    }
}
```
---

###
![](https://imgur.com/JXS09g7.png)

```bash
import { NextRequest, NextResponse } from "next/server";
import nodemailer from "nodemailer";


export async function GET(request:NextRequest) {
    const {searchParams} = new URL(request.url);
    const ToEmail = searchParams.get("email");

    if(!ToEmail) {
        return NextResponse.json(
            {message: "Email query parameter is required"},
            {status: 400}
        )
    }

    
    // Transporter
    let Transporter = nodemailer.createTransport({
        service: "gmail",
        auth: {
            user: process.env.GMAIL_USER,
            pass: process.env.GMAIL_APP_PASSWORD,
        },
    });

    // Prepare
    let myEmail = {
        from: `Test Email <${process.env.GMAIL_USER}>`,
        to: ToEmail,
        subject: "Test email from next.js application",
        text: "Test email from next.js application...",
    }

    try {
        let result = await Transporter.sendMail(myEmail);
        return NextResponse.json(
            {message: "Success", info: result.messageId}
        )
    } catch (error) {
        console.log("Failed messaging", error)
        return NextResponse.json(
            {message: "Fail"},
            {status: 500}
        )
    }

}
```
### .env
```bash
GMAIL_USER=yourgmailid@gmail.com
GMAIL_APP_PASSWORD=your app password collect from google account
```
---
