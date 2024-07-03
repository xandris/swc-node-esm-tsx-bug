# TSX bug demo

Output from 1.8.x:

```shell
$ npm i -D @swc-node/register@1.8.x
$ npm start
> test-swc-register@1.0.0 start
> export SWCRC=true NODE_OPTIONS=--import=@swc-node/register/esm-register; node b.ts; node c.tsx

hi

node:internal/modules/run_main:129
    triggerUncaughtException(
    ^
Error [ERR_MODULE_NOT_FOUND]: Cannot find package 'react' imported from /Users/aparker/src/test-swc-register/c.tsx
    at packageResolve (node:internal/modules/esm/resolve:854:9)
    at moduleResolve (node:internal/modules/esm/resolve:927:18)
    at defaultResolve (node:internal/modules/esm/resolve:1157:11)
    at nextResolve (node:internal/modules/esm/hooks:866:28)
    at resolve (file:///Users/aparker/src/test-swc-register/node_modules/@swc-node/register/esm/esm.mjs:44:12)
    at nextResolve (node:internal/modules/esm/hooks:866:28)
    at Hooks.resolve (node:internal/modules/esm/hooks:304:30)
    at MessagePort.handleMessage (node:internal/modules/esm/worker:196:24)
    at [nodejs.internal.kHybridDispatch] (node:internal/event_target:820:20)
    at MessagePort.<anonymous> (node:internal/per_context/messageport:23:28) {
  code: 'ERR_MODULE_NOT_FOUND'
}
```

This shows the file `c.tsx` was transpiled and run. React isn't available; that's fine.

Output from 1.10.0:

```shell
$ npm i -D @swc-node/register@1.10.0
$ npm start
> test-swc-register@1.0.0 start
> export SWCRC=true NODE_OPTIONS=--import=@swc-node/register/esm-register; node b.ts; node c.tsx

hi

node:internal/modules/run_main:129
    triggerUncaughtException(
    ^
TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".tsx" for /Users/aparker/src/test-swc-register/c.tsx
    at Object.getFileProtocolModuleFormat [as file:] (node:internal/modules/esm/get_format:160:9)
    at defaultGetFormat (node:internal/modules/esm/get_format:203:36)
    at defaultLoad (node:internal/modules/esm/load:143:22)
    at async nextLoad (node:internal/modules/esm/hooks:866:22)
    at async load (file:///Users/aparker/src/test-swc-register/node_modules/@swc-node/register/esm/esm.mjs:178:48)
    at async nextLoad (node:internal/modules/esm/hooks:866:22)
    at async Hooks.load (node:internal/modules/esm/hooks:449:20)
    at async MessagePort.handleMessage (node:internal/modules/esm/worker:196:18) {
  code: 'ERR_UNKNOWN_FILE_EXTENSION'
}
```

This shows `c.tsx` isn't run at all.
