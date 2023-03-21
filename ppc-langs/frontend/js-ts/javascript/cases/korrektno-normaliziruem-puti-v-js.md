# Корректно нормализируем пути в JS

```javascript
import * as path from 'path';

const normalize = (rootPath, childPath) => {
   const normalizedPath = path.normalize(`${rootPath}/${childPath}`); 
   
   if (normalizedPath.startsWith(`${rootPath}/`)) {
      return normalizedPath;
   }

   return null;  
}
```
