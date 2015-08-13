When you install the mongoose module by npm, it does not have a built bson module in it's folder. In the file `node_modules/mongoose/node_modules/mongodb/node_modules/bson/ext/index.js`, change the line(s):

```javascript
bson = require('../build/Release/bson');
```

to:

```javascript
bson = require('bson');
```

And then install the bson module using npm.
