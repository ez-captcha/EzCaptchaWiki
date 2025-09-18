# How to judge the reCAPTCHA version

### Method 1：Determine the version by style

The version can be judged by the style, but it cannot be judged whether it is the enterprise version



**ReCaptcha V2**

If the following needs to be checked, it can be judged as `reCaptcha v2`

![img](imgs\recaptchav2-en.png)

But reCaptcha V2 normal version and enterprise version look the same in appearance, this method can't see the difference

`reCaptcha v2 normal` example: [ReCAPTCHA demo](https://www.google.com/recaptcha/api2/demo)



**ReCaptcha V2 invisible**

The invisible version will hide the check box, you may need to submit the form to trigger the page that requires you to select a picture

`reCaptcha v2 invisible` example: [ReCAPTCHA demo](https://www.google.com/recaptcha/api2/demo?invisible=true)



**ReCaptcha V3**

`reCaptcha v3` is a sensorless form, it does not require the user to click on the verification, it will be verified automatically. This type does not have any style display, and it is easy to confuse with reCaptcha V2 invisible, and it is impossible to judge intuitively whether the normal version or the enterprise version is used in this way. For the v3 version, Google scores users based on various environmental factors. The scores range from 0.1 to 0.9. The higher the score, the closer to human beings. The website can judge whether to pass according to the user's score.

### Method 2: Use the browser console network 

Open the webpage, press F12->Network,



**ReCaptcha V2**

Search `api.js`, if the request link does not contain a render parameter, or the render parameter is `explicit`, for example:

```
https://www.google.com/recaptcha/api.js https://www.google.com/recaptcha/api.js?onload=onloadcallback&render=explicit
```

You can also search for `anchor`, for example:

```
https://www.google.com/recaptcha/api2/anchor?ar=1&k=6LcbPQsTAAAAAB7gt1_a0tDBPojRuzgfe_Z_wW_f&co=aHR0cHM6Ly93d3cuc2VycHJvYm90LmNvbTo0NDM.&hl=zh-CN&v=3kTz7WGoZLQTivI-amNftGZO&size=normal&cb=d2rs6bua44wr 
```

As long as the url prefix is `https://www.google.com/recaptcha`, it indicates that it is the normal version, not the enterprise version. For v2 normal version, the `size` parameter in url is `normal`



**ReCaptcha V2 invisible**

Same as the V2 normal version, the request link does not contain a render parameter, or the render parameter is `explicit`

```
https://www.google.com/recaptcha/api.js https://www.google.com/recaptcha/api.js?onload=onloadcallback&render=explicit
```

If the above conditions are met, and there is a `size=invisible` parameter in the url of the search `anchor`, it may be an invisible version, for example

```
https://www.google.com/recaptcha/api2/anchor?ar=1&k=6LdDCdYcAAAAANPaWKlIKYBRPNQirZFckBZKgZzj&co=aHR0cHM6Ly91bnVzdWFsd2hhbGVzLmNvbTo0NDM.&hl=en-US&type=image&v=5qcenVbrhOy8zihcc2aHOWD4&theme=light&size=invisible&badge=bottomright&cb=sym595bmbzux
```



**ReCaptcha V2 Enterprise**

Same as the above conditions, the only difference is that `https://www.google.com/recaptcha/api.js` url becomes `https://recaptcha.net/recaptcha/enterprise.js` 

Other urls such as `https://www.google.com/recaptcha/api2/anchor` become `https://www.google.com/recaptcha/enterprise/anchor`

 

**ReCaptcha V3**

The request link contains a render parameter, and the render parameter is not equal to `explicit`, for example:

```
https://www.google.com/recaptcha/api.js?render=6LdyC2cUAAAAACGuDKpXeDorzUDWXmdqeg-xy696
```

In the `anchor` interface url, the `size` parameter will only be `size=invisible`



**ReCaptcha V3 Enterprise**

Same as reCaptcha v3 condition, the difference is that `https://www.google.com/recaptcha/api.js` url becomes `https://recaptcha.net/recaptcha/enterprise.js`



Note, if `api.js` or `enterprise.js` is not found in the browser network request, please clear the browser cache and refresh again