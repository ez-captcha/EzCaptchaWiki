## 如何获得 reCaptcha v3 高分?（0.7–0.9）

#### 1. 什么是 Google reCaptcha v3 ?

Google reCaptcha v3 是一款先进的风险分析工具，用于区分人类用户与机器人行为。它不会对用户提出可见的挑战，而是通过监测用户在页面上的行为，自动给出一个介于 0.0（疑似机器人）到 1.0（高度可信的人类）之间的评分。网站所有者可以依据该评分设定相应的防护阈值，在保障安全的同时兼顾用户体验，有效识别并拦截异常流量。

#### 2. EzCaptcha 使用流程

1. **注册账户**
   访问 [ezcaptcha](https://www.ez-captcha.com/) 官网，点击“注册”按钮，填写邮箱地址并设置密码，完成基本信息提交后即可创建账户。
2. **获取 API Key**
   注册成功后，系统将自动生成一个专属 API Key，用于后续调用验证服务。
3. **联系客服获取测试额度**
   通过在线客服申请约 0.5 美元的测试额度，便于体验服务效果。



使用 EzCapthca API，发送包含你的网站 URL 和 apikey 的请求，使用 EzCaptcha 专门提供的 **ReCaptchaV3TaskProxylessS9** 任务类型获得高分通过，示例：

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

API返回令牌和分数，帮助您确定交互的是人类还是机器。响应示例：

```
{
    "errorId": 0,
    "errorCode": "",
    "errorDescription": "",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006" // 请记录此ID
}
```

获取结果示例：

```
POST https://api.ez-captcha.com/getTaskResult

Content-Type: application/json

{
    "clientKey":"your clientkey",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}
```

获取结果响应：

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

**如何确保 ezcaptcha 获取高分的 reCaptcha v3 令牌？**

- **正确设置参数**：确保传入参数（如pageAction参数等）正确。
- **联系客服**：如果无法获取高分，请联系技术客服解决
