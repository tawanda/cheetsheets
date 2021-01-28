### Decode an AWS Encrypted Authorization/Error message

Some actions that involve IAM permissions may return a Client.UnauthorizedOperation response (an HTTP 403 response).

eg

```
You are not authorized to perform this operation. 
Encoded authorization failure message: 
4Ai9qvVQYCx3kKoQaOtnKpzKCvrDlzzn_8R3vhbWP-T....
```

to decode run (the raw encoded mesage without the <>)

`aws sts decode-authorization-message --encoded-message <encoded message from error> --profile my-aruthorized-profile`

The error message is actually encoded JSON inside "", by default the embdeded quotes (") are escaped as \",
to facilitate reading the error extract the message portion and use a text editor to replace \" with "

REF: https://support.turbot.com/hc/en-us/articles/360042567891-Decode-an-Encrypted-Authorization-Error-message


### Add swap space to Amazon Linux

https://aws.amazon.com/premiumsupport/knowledge-center/ec2-memory-swap-file/
