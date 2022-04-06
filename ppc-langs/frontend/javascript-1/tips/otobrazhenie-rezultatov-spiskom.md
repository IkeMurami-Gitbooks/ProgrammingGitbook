# Отображение результатов списком

```markup
<html>
    <head>
        <title>Test</title>
    </head>
    <body>
    <ul id="events"></ul>
    <script>
        // UI
        const $events = document.getElementById('events');

        const newItem = (content) => {
            const item = document.createElement('li');
            item.innerText = content;
            return item;
        };
        
        // Output to list
        $events.appendChild(newItem('connect'));
    </script>
    </body>
</html>
```
