- 转promise化

```js
const {promisify}= require('util')
const fs = require('fs')
const readFile = promisify(fs.readFile)

```



