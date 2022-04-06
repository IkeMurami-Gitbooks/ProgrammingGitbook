# Simple page with GET param

```php
<? $page = $_GET['page'];?>

<script>
$(document).ready(function () {
    $('.waf_bypass').text("<?=$page?>");
});
</script>

<div class="waf_bypass">Not loaded yet</div>
```
