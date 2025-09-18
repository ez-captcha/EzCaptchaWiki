## What is the `sa` Parameter in Google reCAPTCHA?

Why do we need to provide the correct `sa` parameter when solving reCAPTCHA challenges?

### The Purpose of the `sa` Parameter

On some websites that integrate Google reCAPTCHA v2, the `sa` parameter may be included when requesting the anchor endpoint, for example, the reCAPTCHA Demo: https://www.google.com/recaptcha/api2/demo. The `sa` parameter generally stands for **site action**, which is used to describe the "action" or context of the current page or user operation, such as `"login"`, `"signup"`, or `"homepage"`.

- This "action" value is usually **specified by the developer when calling reCAPTCHA**.
- Its role is similar to the `pageAction` field in reCAPTCHA v3, indicating the business scenario for this CAPTCHA or risk assessment.

### Common Examples of the `sa` Parameter

In the anchor URL, parameters like `sa=login`, `sa=register`, or `sa=resetPassword` are quite common.

**Example:**

```
https://www.google.com/recaptcha/api2/anchor?ar=1&k=xxxx&co=xxx&sa=login  
```

### Why Must the `sa` Parameter Be Correct?

- **The `sa` parameter declares the business scenario.** Google verifies this parameter to ensure it matches the expected action on both the frontend request and the backend, maintaining consistency in risk control.
- If the `sa` parameter is incorrect, Google may identify the request as a forged page action or reject the response, failing to return a valid score or even throwing an error.

------

### How to Solve reCAPTCHA Sites with the `sa` Parameter Using EzCaptcha?

Please refer to our usage tutorial. You can pass the actual `sa` parameter of the website by adding the "sa" property to the task parameter object. We will use the `sa` you provide to solve the CAPTCHA.

**Task Types**

For reCAPTCHA v2 solutions, we provide the following task types:

| **Task Type**                       | **Description**                                              | **Price (USD)** |
| :---------------------------------- | :----------------------------------------------------------- | :-------------- |
| ReCaptchaV2TaskProxyless            | reCAPTCHA v2 solution, using server-side proxy               | $0.6/1k         |
| ReCaptchaV2TaskProxylessS9          | reCAPTCHA v2 high-score solution (score ≥ 0.9), using server-side proxy | $1.2/1k         |
| ReCaptchaV2STaskProxyless           | reCAPTCHA v2 with `s` parameter, using server-side proxy     | $0.6/1k         |
| ReCaptchaV2EnterpriseTaskProxyless  | reCAPTCHA v2 Enterprise solution, using server-side proxy    | $1.2/1k         |
| ReCaptchaV2SEnterpriseTaskProxyless | reCAPTCHA v2 Enterprise with `s` parameter, using server-side proxy | $1.2/1k         |

**Create a Task**

Create a recognition task using the `createTask` method.

Main endpoint: `https://api.ez-captcha.com`

Request URL: `https://api.ez-captcha.com/createTask`

China-optimized endpoint: `http://47.115.166.118:15000/createTask`

Request format: `POST` `application/json`

**Object Structure**

| **Property** | **Type** | **Required** | **Description**                                              |
| :----------- | :------- | :----------- | :----------------------------------------------------------- |
| clientKey    | string   | Yes          | Your account API key                                         |
| type         | string   | Yes          | Task type, e.g., ReCaptchaV2TaskProxyless                    |
| websiteURL   | string   | Yes          | The URL of the page with reCAPTCHA, usually a fixed value.   |
| websiteKey   | string   | Yes          | The site key for reCAPTCHA, a fixed value.                   |
| isInvisible  | Bool     | No           | If solving invisible reCAPTCHA v2, add this parameter and set to true; defaults to false if omitted |
| sa           | string   | No           | If the **sa** parameter appears in the **anchor** request's query string, set this value to match the sa parameter. It is typically a string. **Note: Some sites may strictly verify this value.** |
| s            | string   | No           | For solving reCAPTCHA v2 with the `s` parameter (including v2 and v2 Enterprise), set the type to ReCaptchaV2STaskProxyless or ReCaptchaV2SEnterpriseTaskProxyless, and set this value from the website's network data. |
| websiteTitle | string   | No           | The title of the page loading reCAPTCHA; use `document.title` in the browser console to get it. |
| proxy        | string   | No           | Custom proxy in the format: protocol:username:password@ip(domain):port, e.g., `http://username:password@test.com:5555` |

**Request Example**

```
POST https://api.ez-captcha.com/createTask  

Content-Type: application/json  

{  
    "clientKey": "cc9c18d3e263515c2c072b36a7125eecc078618f",  
    "task": {  
        "websiteURL": "https://www.google.com/recaptcha/api2/demo",  
        "websiteKey": "6Le-wvkSAAAAAPBMRTvw0Q4Muexq9bi0DJwx_mJ-",  
        "type": "ReCaptchaV2TaskProxyless",  
        "sa": "action",  
        "isInvisible": false // set true for invisible reCAPTCHA v2  
    }  
}  
```

**Response Example**

```
{  
    "errorId": 0,  
    "errorCode": "",  
    "errorDescription": "",  
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006" // Please record this ID  
}  
```

**Get the Result**

Use the `getTaskResult` method to retrieve the recognition result.

Main endpoint: `https://api.ez-captcha.com`

Request URL: `https://api.ez-captcha.com/getTaskResult`

China-optimized endpoint: `http://47.115.166.118:15000/getTaskResult`

Request format: `POST` `application/json`

Depending on system load, you will receive the result in about 1 to 60 seconds, with a 120-second timeout.

**Request Example**

```
POST https://api.ez-captcha.com/getTaskResult  

Content-Type: application/json  
   
{  
    "clientKey":"YOUR_API_KEY",  
    "taskId": "TASKID OF CREATETASK" // The ID returned by createTask  
}  
```

**Response Fields**

| **Parameter**               | **Type** | **Description**                                              |
| :-------------------------- | :------- | :----------------------------------------------------------- |
| errorId                     | Integer  | Error hint: 0 - no error, 1 - error                          |
| errorCode                   | string   | Error code                                                   |
| errorDescription            | string   | Error description                                            |
| status                      | String   | **processing** - recognition in progress, retry after 3 seconds; **ready** - recognition complete, result in the *solution* field |
| solution                    | Object   | Recognition result; varies by CAPTCHA type. For reCAPTCHA, the result is in the gRecaptchaResponse field within the object. |
| solution.gRecaptchaResponse | string   | Recognition result: the response value, to be POSTed or submitted to the target website. One-time use, valid for 120 seconds; use within 60 seconds is recommended. |

**Response Example**

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

**Response Description**

- Success: When `errorId` is `0` and `status` is `ready`, the result is in the `solution` field.
- In Progress: When `errorId` is `0` and `status` is `processing`, please retry after 3 seconds.
- Error: When `errorId` is greater than `0`, refer to `errorDescription` for details.