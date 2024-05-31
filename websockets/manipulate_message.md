## Lab: Manipulating WebSocket messages to exploit vulnerabilities

Defence
- encode chat message to HTML code, to defence XSS attacks, but on client side

Bypass
- Intercept chat message from client, change the encoded message to a raw html node: `<img src=1 onerror="alert(1)" />`


Question:
why `<script>setTimeout(() => {alert(1)}, 1000)</script>` does not work?

![wef](./settimtout.png)