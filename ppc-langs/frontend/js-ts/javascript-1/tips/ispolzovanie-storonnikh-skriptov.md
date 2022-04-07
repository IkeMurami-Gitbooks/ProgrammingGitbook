# Использование сторонних скриптов

### На примере socket.io

```markup
<html>
    <head>
        <title>Test Client.io</title>
    </head>
    <body>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.3.0/socket.io.js"></script>
        <script>
            var socket = io.connect('https://127.0.0.1:8080/', { transports: ['websocket'] });
            var uiXSLData = base64EncodedPayload;
            var prepareXSLData = base64EncodedPayload;
            
            socket.on('connect', (data) => {
                // ...
            });
            
            socket.emit('signdata', {
                someParam: "TestParam"
            });
        </script>
    </body>
</html>
```
