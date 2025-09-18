# reCAPTCHA中sa参数的作用

在Google recaptcha 中，sa参数有什么作用？为什么我们在解决reCaptcha验证码时，需要传入正确的sa参数呢

### sa 参数的作用

在一些集成 Google reCAPTCHA v2 的站点中，请求anchor端点时可能会携带sa参数，例如 reCaptcha Demo: https://www.google.com/recaptcha/api2/demo。`sa` 参数一般代表**site action**，用于描述“当前页面/行为的动作”，比如 `"login"`、`"signup"`、`"homepage"` 等。

- 这个“动作”值通常**由开发者在调用 reCAPTCHA 时指定**。
- 作用和 v3 里的 `pageAction` 字段类似，用来指明本次验证码/风险评估所处的业务场景。

### sa 参数的常见实例

在 anchor 地址中，`sa=login`、`sa=register`、`sa=resetPassword` 等都很常见。

**例子：**

```
https://www.google.com/recaptcha/api2/anchor?ar=1&k=xxxx&co=xxx&sa=login  
```

### 为什么 sa 必须正确？

- **sa 用于声明业务场景**，Google 校验此参数是为了和你前端请求以及服务端期望的动作一致，保障风控一致性。

- 若 sa 不正确，Google 可能识别为伪造的页面行为或拒绝响应，无法返回合理评估分数，或直接报错

  

### 如何使用EzCaptcha解决带有 sa 参数的 reCaptcha 网站？

请参考我们的使用教程，其中通过在任务参数对象结构中添加 "sa" 参数来传递网站真实的sa参数，我们将会用您提供的sa解决验证码。

**任务类型**

对于ReCaptcha V2的解决方案，我们提供的任务类型如下：

| **任务类型**                        | **描述**                                                     | **价格(美元)** |
| :---------------------------------- | :----------------------------------------------------------- | :------------- |
| ReCaptchaV2TaskProxyless            | reCaptcha V2解决方案，使用服务器内置代理                     | $0.6/1k        |
| ReCaptchaV2TaskProxylessS9          | reCaptcha V2高分值解决方案，使用服务器内置代理并使得返回的token分数至少为0.9 | $1.2/1k        |
| ReCaptchaV2STaskProxyless           | reCaptcha V2带s参数解决方案，使用服务器内置代理              | $0.6/1k        |
| ReCaptchaV2EnterpriseTaskProxyless  | reCaptcha V2Enterprise解决方案，使用服务器内置代理           | $1.2/1k        |
| ReCaptchaV2SEnterpriseTaskProxyless | reCaptcha V2Enterprise带s参数解决方案，使用服务器内置代理    | $1.2/1k        |

**创建任务**

通过 createTask方法 创建识别任务

请求节点： `https://api.ez-captcha.com `

请求地址： `https://api.ez-captcha.com/createTask`

中国优化地址：`http://47.115.166.118:15000/createTask`

请求格式：`POST` `application/json`

**对象结构**

| **属性**     | **类型** | **必须** | **说明**                                                     |
| :----------- | :------- | :------- | :----------------------------------------------------------- |
| clientKey    | string   | 是       | 您账号的用户密钥                                             |
| type         | string   | 是       | 任务类型，例如 ReCaptchaV2TaskProxyless                      |
| websiteURL   | string   | 是       | ReCaptcha 网页地址，一般固定值。                             |
| websiteKey   | string   | 是       | ReCaptcha 网站密钥，固定值。                                 |
| isInvisible  | Bool     | 否       | 遇到invisible类型的reCaptchaV2需要添加此参数并设置为true，该参数不填时默认为false |
| sa           | string   | 否       | 如果**sa**出现在**anchor**请求的查询参数中，您将需要设置该值为sa查询参数的值，它通常是一个字符串。注意：**有部分网站可能会严格校验该值** |
| s            | string   | 否       | 用于解决带s参数的reCaptcha V2，包括V2和V2 Enterprise，需要将类型指定为ReCaptchaV2STaskProxyless或RecaptchaV2SEnterpriseTaskProxyless，从你所使用的网站中返回的数据包中找到该参数并设置 |
| websiteTitle | string   | 否       | 加载recaptcha的网站页面，使用js控制台运行“document.title”获取title |
| proxy        | string   | 否       | 设置自定义代理，格式：protocol:username:password@ip(domain):port例如：`http://username:password@test.com:5555` |

**请求示例**

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
        "isInvisible": false // invisable类型的reCaptchaV2设置添加true
    }
}
```

**响应示例**

```
{
    "errorId": 0,
    "errorCode": "",
    "errorDescription": "",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006" // 请记录此ID
}
```

**获取结果**

使用 getTaskResult方法获取识别结果

请求节点： `https://api.ez-captcha.com `

请求地址： `https://api.ez-captcha.com/getTaskResult`

中国优化地址：`http://47.115.166.118:15000/getTaskResult`

请求格式：`POST` `application/json`

根据系统负载，您将在大约 1s 到 60s 的时间间隔内得到结果，120秒超时

**请求示例**

```
POST https://api.ez-captcha.com/getTaskResult

Content-Type: application/json
 
{
    "clientKey":"YOUR_API_KEY",
    "taskId": "TASKID OF CREATETASK" //由createTask方法创建的ID
}
```

**响应结果**

| **参数**                    | **类型** | **说明**                                                     |
| :-------------------------- | :------- | :----------------------------------------------------------- |
| errorId                     | Integer  | 错误提示： 0 - 没有错误，1 - 有错误                          |
| errorCode                   | string   | 错误代码                                                     |
| errorDescription            | string   | 错误详细描述                                                 |
| status                      | String   | **processing** - 正在识别中，请3秒后重试 **ready** - 识别完成，在*solution*参数中找到结果 |
| solution                    | Object   | 识别结果，不同类型的captcha的结果会有所区别，对于reCaptcha而言，结果为对象中的gRecaptchaResponse。 |
| solution.gRecaptchaResponse | string   | 识别结果：response值，用于POST或模拟提交给目标网站。一次性使用，有效期120s，建议在60s内使用。 |

**响应示例**

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

**响应说明**

- 识别成功：当`errorId`等于`0` 并且`status`等于 `ready`，结果在`solution`里面。
- 正在识别中：当`errorId`等于`0` 并且`status`等于 `processing`，请3秒后重试。
- 出错了：当`errorId` 大于`0`，请根据`errorDescription`了解出错误信息 