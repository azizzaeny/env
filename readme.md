## @zaeny/env 

[![npm version](https://img.shields.io/npm/v/@zaeny/env.svg)](https://www.npmjs.com/package/@zaeny/env)
![npm downloads](https://img.shields.io/npm/dm/@zaeny/env.svg)  

> Reading .env files

### Getting Started  
 ``` 
 npm install @zaeny/env
 ``` 

**Example**
create .env files `key=value` `="` and `=''` or `=`

```sh
touch .env
cat > .env <<'EOL'
FOO="Bar"
TEST=DATA
BAR='apapa'
EOL
```

usage:
```js

var {env} = require('@zaeny/env');
env()

/*
  { FOO: 'Bar', TEST: 'DATA', BAR: 'apapa'}  
  added to process.env[]
*/

// select different files
env('./environment'); 

```
### Implementation
this utility function free to copy 

```js path=index.js

/* author: https://github.com/azizzaeny/env */

function env(file=".env", state=process.env){
  var {readFileSync, existsSync} = require('fs');
  if(!existsSync(file)) return console.log('no file', file);
  let contents = readFileSync(file, 'utf8');  
  let params= contents.split('\n').map((line)=>line.split('=')).reduce((k, [key, value])=>{
    if(value && (value.startsWith('"') || value.startsWith("'"))) (value = value.slice(1, -1));
    return (value) ? ((state[key]=value), Object.assign(k,{[key] : value.trim()})) : k ;
  },{});
  return params
}

module.exports = { env }
```

### Development and building codes
```sh
npm i @zaeny/literate --save-dev
```

outputing files from readme.md  

start repl `node` and evaluating this block code  

```js
var {tangle} = require('@zaeny/literate/tangle');
tangle( require('fs').readFileSync('./readme.md', 'utf8'), (file) => file.path );

```

node evaluate  

```sh
node -e "var {tangle} = require('@zaeny/literate/tangle');console.log(tangle( require('fs').readFileSync('./readme.md', 'utf8'), (file) => file.path ));"

```
### Changes 
 - [1.0.0] initial release, add read double quote
 

