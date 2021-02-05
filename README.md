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

[Get an Azure Account](https://azure.microsoft.com/free/?WT.mc_id=m365-0000-buhollan)

> Note that this Nuxt project uses Express. If you picked a different server, you may have a slightly different configuration.

Windows leverages Kudu to do the build and IISNode to run the project. This means that some additional files/changes are required. These changes have already been made to this project (except for number 1) - for your deployment pleasure. 

1. **CHANGE YOU NEED TO MAKE:** You need to set the `WEBSITE_NODE_DEFAULT_VERSION` Application Setting to one that is greater than or equal to 8. Nuxt requires this. Otherwise you will see an error that the keyword `function` is unexpected. I use version  10.15.2. 

2. A `.deployment` file must be present in the project to instruct Kudu to run `npm install`.

   ```
   [config]
   SCM_DO_BUILD_DURING_DEPLOYMENT=true
   ```

3. Include a `postinstall` hook in the "scripts" section of the `package.json` to execute `npm run build`

   ```
   "postinstall": "npm run build"
   ```

4. Paths will be broken by default with the standard Nuxt setup because Azure creates a `web.config` based on the startup point of `server/index.js`. This is correct, but sets the base path to `/server` which breaks the app. To fix this, add a `server.js` file to the root of the project and just require in the `server/index.js`. This will force Azure to run this app from the root and not from the `server/index.js` file.

   ```
   require("./server/index.js")
   ```

---

For detailed explanation on how Nuxt works, checkout [Nuxt.js docs](https://nuxtjs.org).
