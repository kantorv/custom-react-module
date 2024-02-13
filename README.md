# custom-react-module



#### package init (reproducing steps, no needed when cloned)

```bash

$ sudo apt install node-typescript                                              # make sure tcs installed
$ yarn init -y                                                                  # create package.json
$ yarn add --dev @types/node  @types/react tsup typescript                      # install dev dependencies
$ yarn add --peer react react-dom                                               # install peep dependencies
$ tsc --init                                                                    # create tsconfig.json
$ # uncomment '"jsx": "react"'  in tsconfig.json

```


#### build config (update/replace in package.json)

```json  
  "scripts": {
    "build": "tsup src/",
    "start": "npm run build -- --watch",
    "type-check": "tsc"
  },
  "tsup": {
    "entry": [
      "src/index.ts"
    ],
    "treeshake": true,
    "sourcemap": "inline",
    "minify": true,
    "clean": true,
    "dts": true,
    "splitting": false,
    "format": [
      "cjs",
      "esm"
    ],
    "external": [
      "react"
    ],
    "injectStyle": false
  },
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "require": "./dist/index.js",
      "import": "./dist/index.mjs"
    }
  },
  "files": [
    "dist"
  ],
```


#### react component init

```bash
$ mkdir -p src/component                                          
$ touch src/index.ts                                                               # create package.json
$ touch src/component/Main.tsx 
```


```tsx
// add to src/component/Main.tsx

import React from 'react';

const Main = ()=>{
    return (
        <div>custom-react-component</div>
    )
}
export { Main }
```

```ts
// add to src/index.ts

export { Main as CustomComponent } from './component/Main'
```



#### local testing

```bash
$ yarn build # test integrity
$ yarn link  # create global yarn link                          
$ yarn start # watch updates in external app
# ...
$ yarn unlink # after component testing
```


```bash
# in external react app
$ yarn link "custom-react-module"
```


```tsx
//import custom component
import {CustomComponent} from "custom-react-module";

// ...

// use custom component 
// ...
    <CustomComponent />
// ...


// make an updates to 'custom-react-module' and see changes realtime
```

```bash
# after local testing
$ yarn unlink "custom-react-module" # in external app
```