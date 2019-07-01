# Nuxt on Azure AppService for Windows

> This is a sample application that you can deploy to Azure AppService for Windows

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start
```

## Deploying To Azure AppService for Windows

[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

Windows leverages Kudu to do the build and IISNode to run the project. This means that some additional files/changes are required.

1. A `.deployment` file must be present in the project to instruct Kudu to run `npm install`.

   ```
   [config]
   SCM_DO_BUILD_DURING_DEPLOYMENT=true
   ```

2. Include a `postinstall` hook in the "scripts" section of the `package.json` to execute `npm run build`

   ```
   "postinstall": "npm run build"
   ```

3. Paths will be broken by default with the standard Nuxt setup because Azure creates a `web.config` based on the startup point of `server/index.js`. This is correct, but sets the base path to `/server` which breaks the app. To fix this, add a `server.js` file to the root of the project and just require in the `server/index.js`. This will force Azure to run this app from the root and not from the `server/index.js` file.

   ```
   require("./server/index.js")
   ```

---

For detailed explanation on how Nuxt works, checkout [Nuxt.js docs](https://nuxtjs.org).
