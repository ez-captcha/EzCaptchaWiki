
## How to get reCaptcha v3 High Scores (0.7–0.9)

#### 1. What is Google reCaptcha v3?

Google reCaptcha v3 is an advanced risk analysis tool designed to distinguish between human users and automated bots. Unlike earlier versions, it does not present users with visible challenges. Instead, it silently analyzes user behavior on the webpage and assigns a score between 0.0 (likely a bot) and 1.0 (highly likely a human). Website owners can set custom threshold levels based on this score to balance security and user experience, effectively identifying and blocking suspicious traffic without disrupting legitimate users.

#### 2. EzCaptcha Usage Workflow

1. **Register an Account** 
   Visit the official [EzCaptcha](https://www.ez-captcha.com/) website, click the “Register” button, enter your email address and set a password, then submit your details to create your account.
2. **Obtain Your API Key** 
   Upon successful registration, the system will automatically generate a unique API Key, which you will use to authenticate requests to the verification service.
3. **Contact Support for Test Credits** 
   Reach out to customer support via live chat to request approximately $0.50 in test credits, allowing you to evaluate the service’s performance.

---

To obtain a high-score reCaptcha v3 token using the EzCaptcha API, send a request containing your website URL and API key, using the specialized **ReCaptchaV3TaskProxylessS9** task type. Example request:

```
POST https://api.ez-captcha.com/createTask  
Content-Type: application/json

{
    "clientKey": "your clientkey",
    "task": {
        "websiteURL": "https://recaptcha-demo.appspot.com/recaptcha-v3-request-scores.php",
        "websiteKey": "6LdyC2cUAAAAACGuDKpXeDorzUDWXmdqeg-xy696",
        "type": "ReCaptchaV3TaskProxylessS9",
        "isInvisible": true,
        "pageAction": "examples/v3scores"
    }
}
```

The API will return a token and score, helping you determine whether the interaction is from a human or a bot. Sample response:

```
{
    "errorId": 0,
    "errorCode": "",
    "errorDescription": "",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006" // Please record this ID
}
```

To retrieve the result, make the following request:

```
POST https://api.ez-captcha.com/getTaskResult  
Content-Type: application/json

{
    "clientKey": "your clientkey",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}
```

Sample successful response:

```
{
    "errorId": 0,
    "errorCode": null,
    "errorDescription": null,
    "solution": {
        "gRecaptchaResponse": "03AGdBq25SxXT-pmSeBXjzScW-EiocHwwpwqtk1QXlJnGnU......"
    },
    "status": "ready"
}
```

---

### How to Ensure High reCaptcha v3 Scores from EzCaptcha

- **Set Parameters Correctly**: Ensure all parameters (such as `pageAction`, `isInvisible`, etc.) are accurately configured to match the target website’s context.
- **Contact Support**: If you consistently receive low scores, reach out to EzCaptcha’s technical support team for assistance and optimization guidance.
