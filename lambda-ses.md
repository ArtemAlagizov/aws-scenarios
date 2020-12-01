## how to send email using ses and lambda

* create lambda
```js
const aws = require("aws-sdk");
const ses = new aws.SES({ region: "us-east-2" });

exports.handler = async (event) => {
  const params = {
    Destination: {
      ToAddresses: [event.email],
    },
    Message: {
      Body: {
        Text: { 
          Data: "Hi " + event.username + "!\n\nClick the following url to verify your registration at mentor-club: " + event.confirmationUrl + "\n\n\nIf you have any problems verifying your account, feel free to email us at alagizov@gmail.com"
        },
        Html: {
          Data: `<div style="margin:0 20px"><div class="adM">
      </div><div style="margin:10px 0"><strong>Welcome ${event.username}!</strong></div>
      <div style="margin:10px 0">
        Click the button below to <span class="il">confirm</span> your email address. If you did not register at mentor-club with this email address, please ignore this email.</div>
      <div style="margin:10px 0">
        <a href="${event.confirmationUrl}" style="text-decoration:none;background-color:#205081;padding:10px 15px;color:white;display:inline-block" target="_blank" data-saferedirecturl="${event.confirmationUrl}">Verify email</a>
      </div>
      <div style="margin:10px 0">
        
      </div>
      <div style="margin:10px 0">
        If you have any problems verifying your account, feel free to email us at <a href="mailto:alagizov@gmail.com" target="_blank">alagizov@gmail.com</a>.
      </div>
      <div style="margin:10px 0;color:grey;font-size:0.9em">
        Thanks,<br>
        The Mentors
      </div>
    </div>`
        }
      },

      Subject: { Data: "Verify your email for Mentor-club" },
    },
    Source: "no-reply@artem-alagizov.com",
  };
 
  return ses.sendEmail(params).promise();
};
```
* give lambda permissions to send emails -> create role with policy (make resource more scrict)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ses:SendEmail",
                "ses:SendRawEmail"
            ],
            "Resource": "*"
        }
    ]
}
```
* attach role to the lambda
* give ec2/app permissions to call lambda
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction",
                "lambda:InvokeAsync"
            ],
            "Resource": "arn:aws:lambda:us-east-2:260654294406:function:ses"
        }
    ]
}
```
