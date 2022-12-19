# Random Color для всех элементов

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js"></script>
</head>

<body>
   
    <script>
        var safeColors = ['00', '33', '66', '99', 'cc', 'ff'];
        var rand = function () {
            return Math.floor(Math.random() * 6);
        };
        var randomColor = function () {
            var r = safeColors[rand()];
            var g = safeColors[rand()];
            var b = safeColors[rand()];
            return "#" + r + g + b;
        };
        $(document).ready(function () {
            
            $('*').each(function () {
                $(this).css('background', randomColor());
            });
        
        });
    </script>
</body>

</html>
```
