# Strapi 3 CMS

Quickstart: https://www.youtube.com/watch?v=6FnwAbd2SDY

https://docs-v3.strapi.io/developer-docs/latest/getting-started/introduction.html

- https://www.youtube.com/c/AlextheEntreprenerd
    - https://www.youtube.com/playlist?list=PLjhq46XB5LWukdEt29lTaJl9XFG9CBMOp

## Plugin

- https://docs-v3.strapi.io/developer-docs/latest/development/local-plugins-customization.html
- https://www.youtube.com/watch?v=kIZHzbmnhnU
    - https://www.youtube.com/watch?v=kIZHzbmnhnU&t=960s

### Appearing in `/admin`

If you don't want your plugin to appear in the `/admin` left-side-bar, then comment out the entire `menu` section in `{root}/plugins/{plugin name}/admin/src/index.js`.

If you don't want your plugin to be accessable by URL the `/admin`, then make `mainComponent: null,` instead of `mainComponent: App,` in `{root}/plugins/{plugin name}/admin/src/index.js`.

```js
'someplugin': {
  enabled: true,
  resolve: './src/plugins/someplugin'
},
```

## Reset password

https://forum.strapi.io/t/didnt-get-the-password-recovery-link/1533

`yarn strapi admin:reset-user-password --email=chef@strapi.io --password=Gourmet1234`

## Auth

- https://www.youtube.com/watch?v=TIK9CYDgs5k&list=PL2dKqfImstaROBMu304aaEfIVTGodkdHh&index=3 
- https://www.youtube.com/watch?v=EjBpJuqqi3U 
- https://www.youtube.com/watch?v=xv5TWP3tCKs 
- https://forum.strapi.io/t/retrieve-user-details/5082/2
- https://strapi.io/blog/a-beginners-guide-to-authentication-and-authorization-in-strapi (v4, but relatable to v3)
    - https://www.youtube.com/watch?v=vcopLqUq594&t=4336s

```
curl --location --request POST '{domain}/auth/local' 
--form 'identifier="user@auth.com"' 
--form 'password="auth_user_password"'
```

Potential Problem: If you get
```
error /var/www/v3/node_modules/redis/dist/index.js:42
            ...options?.modules
                       ^
SyntaxError: Unexpected token '.'
```
you need to install a lower version of Redis https://forum.strapi.io/t/problem-with-a-new-instance/18246

## Backend

- https://docs-v3.strapi.io/developer-docs/latest/development/backend-customization.html
    - Querying DB: 
        - https://docs-v3.strapi.io/developer-docs/latest/development/backend-customization.html#queries
        - https://strapi.gitee.io/documentation/v3.x/concepts/queries.html#api-reference
        - Eg: https://forum.strapi.io/t/how-to-query-for-administrators/4045
    - Hooks:
        - https://docs-v3.strapi.io/developer-docs/latest/development/backend-customization.html#models
        - https://docs-v3.strapi.io/developer-docs/latest/development/backend-customization.html#webhooks

## DB

### Querying

**In Strapi v4, it's like:**

Example 1: Reading from `files` table:
```js
await strapi.entityService.findMany('plugin::upload.file', {
  filters: {
    $not: {
      name: 'exclude.png',
    },
  },
  populate: { category: true },
});
```

Example 2: Reading from `admin_users` table:
```js
await strapi.entityService.findMany('admin::user');
```

(Entity Service API)   
https://github.com/atabegruslan/Strapi4#db-interaction 

**But in Strapi v3, it's like:**

Example 1: Reading from `upload_file` table:
```js
await strapi.query('file', 'upload').find({name_ne: 'exclude.png'});
```

Example 2: Reading from `strapi_administrator` table:
```js
await strapi.query('user', 'admin').find({});
```

https://strapi.gitee.io/documentation/v3.x/concepts/queries.html

### Changing DB

Using MySQL instead of SQLite: https://www.youtube.com/watch?v=PaNSN_h1_JA

## MVC

**In Strapi v4, a plugin's MVC flows like:**

`src/plugins/plugin-name/admin/src/pages/HomePage/index.js`

```js
import { useEffect, useState } from 'react';
import { request } from "@strapi/helper-plugin";

const HomePage = () => {
  const [config, setConfig] = useState({
    key1: '',
    key2: ''
  });

  useEffect(() => {
    request('/plugin-name/config', {method: 'GET'}).then(setConfig);
  }, []);
```

`src/plugins/plugin-name/server/routes/index.js`

```js
module.exports = [
  {
    method: 'GET',
    path: '/config',
    handler: 'pluginName.getConfig',
    config: {
      policies: [],
      auth: false,
    },
  },
```

`src/plugins/plugin-name/server/controllers/index.js`

```js
const pluginName = require('./plugin-name');

module.exports = {
  pluginName,
};
```

`src/plugins/plugin-name/server/controllers/plugin-name.js`

```js
module.exports = {
  async getConfig(ctx) {
    ctx.body = await strapi
      .plugin('plugin-name')
      .service('pluginName')
      .getConfig();
  },
```

`src/plugins/plugin-name/server/services/index.js`

```js
const pluginName = require('./plugin-name');

module.exports = {
  pluginName,
};
```

`src/plugins/plugin-name/server/services/plugin-name.js`

```js
const fs = require('fs');
const path = require('path');
const fetch = require("node-fetch");

module.exports = ({ strapi }) => ({
  ownFunction() {
    return {key1:'val1', key2:'val2'};
  },
  async getConfig() {
    var config = this.ownFunction();

    // var queryParams = ctx.request.query;
    // var payload = ctx.request.body;

    // strapi.store... # Use Strapi store
    // strapi.config... # Get Strapi info
    // strapi.entityService... # Use Entity Service API
    // strapi.dirs.public # Get public directory

    return config;
  },
```

**But in Strapi v3, a plugin's MVC flows like:**

`plugins/plugin-name/admin/src/containers/HomePage/index.js`

```js
import { useEffect, useState } from 'react';
import { request } from "strapi-helper-plugin";

const HomePage = () => {
  const [config, setConfig] = useState({
    key1: '',
    key2: ''
  });

  useEffect(() => {
    request('/plugin-name/config', {method: 'GET'}).then(setConfig);
  }, []);
```

`plugins/plugin-name/config/routes.json`

```js
{
  "routes": [
    {
      "method": "GET",
      "path": "/config",
      "handler": "plugin-name.getConfig",
      "config": {
        "policies": []
      }
    },
```

`plugins/plugin-name/controllers/index.js`

```js
const fs = require('fs');
const path = require('path');
const fetch = require("node-fetch");

module.exports = {
  ownFunction: () => {
    return {key1:'val1', key2:'val2'};
  },
  getConfig: async (ctx) => {
    var config = module.exports.ownFunction();

    // var queryParams = ctx.request.query;
    // var payload = ctx.request.body;

    // strapi.store... # Use Strapi store
    // strapi.config... # Get Strapi info
    // strapi.query... # Query DB
    // path.join(strapi.dir, 'public') # Get public directory

    ctx.send(config);
  },
```

## Providers

V4 Providers: https://github.com/atabegruslan/Strapi4#providers

Take Cloudinary upload provider for example:

V4: 
- https://github.com/strapi/strapi/tree/master/packages/providers/upload-cloudinary
- https://www.npmjs.com/package/@strapi/provider-upload-cloudinary

V3: 
- https://github.com/strapi/strapi/tree/v3/3.6.11/packages/strapi-provider-upload-cloudinary
- https://www.npmjs.com/package/strapi-provider-upload-cloudinary

For local development setup:

v4: 
- Put the local module here: `local_modules/@strapi/provider-upload-filerobot`
- In `package.json`, name it `@strapi/provider-upload-cloudinary`
- Include it by running from root CMS folder: `npm i -S ./local_modules/@strapi/provider-upload-filerobot`

`config/plugins.js`
```js
module.exports = {
  ...
  'upload': {
    config: {
      provider: 'cloudinary',
      providerOptions: { /* ... */ },
    },
  },
};
```

`config/middlewares.js`: Replace `'strapi::security'` with:
```js
{
  name: 'strapi::security',
  config: {
    contentSecurityPolicy: {
      useDefaults: true,
      directives: {
        'connect-src': ["'self'", 'https:'],
        'img-src': ["'self'", 'data:', 'blob:', 'res.cloudinary.com'],
        'media-src': ["'self'", 'data:', 'blob:', 'res.cloudinary.com'],
        upgradeInsecureRequests: null,
      },
    },
  },
},
```

V3: 
- Put the local module here: `local_modules/strapi-provider-upload-filerobot`
- In `package.json`, name it `strapi-provider-upload-cloudinary`
- Include it by running from root CMS folder: `npm i -S ./local_modules/strapi-provider-upload-filerobot`

`extensions/upload/config/settings.json`
```js
{
  "provider": "cloudinary",
  "providerOptions": { /* ... */ }
}
```

## Update Strapi from v3 to v4

- CMS: 
    - https://docs.strapi.io/developer-docs/latest/update-migration-guides/migration-guides.html#v3-to-v4-migration-guide
- Plugin: 
    - https://strapi.io/blog/v4-plugin-migration-guide
        - New: https://docs.strapi.io/developer-docs/latest/update-migration-guides/migration-guides/v4/plugin-migration.html

## Lang

- https://strapi.io/blog/i18n-implementation-best-practices-in-strapi
- https://www.youtube.com/watch?v=bWyP1piDEcg
- https://docs-v3.strapi.io/developer-docs/latest/development/local-plugins-customization.html#i18n
- https://stackoverflow.com/questions/58154297/showing-custom-plugins-pages-in-the-left-menu-in-strapi-3/58490999#58490999 (partly related)

### Implementation

Just like in v4 https://github.com/atabegruslan/Strapi4/tree/master/src/plugins/language-strings   
but with 1 difference: `intl.formatMessage({id:'pluginName.some.identifier'})` instead of `intl.formatMessage({id:'plugin-name.some.identifier'})`

## Install and Run

node v14.0.0
npm v6.14.4

`npx create-strapi-app@3.6.1 plugin-name --quickstart`

After you install and run for the first time, you will be prompted to make a superadmin account.

`yarn strapi generate:plugin camelCase`

`yarn build`

Run: `yarn start`

Hot reload: `yarn develop`

Hot reload (Backend included): `yarn develop --watch-admin`

## Logging

https://strapi.gitee.io/documentation/3.0.0-beta.x/concepts/logging.html#global-logger-configuration

## REST API

Permissions: Create an user, give it Authenticated role. Settings > Roles > Authenticated user role > Give permissions

Auth: https://github.com/atabegruslan/Strapi3#auth

GET all media: 

```
curl --location --request GET '{domain}/upload/files' \
--header 'Authorization: Bearer {token}'
```
https://docs.strapi.io/developer-docs/latest/plugins/upload.html#endpoints

GET all contents of custom content-type: 

```
curl --location --request GET '{domain}/tests' \
--header 'Authorization: Bearer {token}'
```

## Publishing to NPMJS

- Providers: The `name` in `package.json` must have this format: `strapi-provider-upload-{whatever}`
- Plugins: The `name` in `package.json` must have this format: `strapi-plugin-{whatever}` . Use only hyphenated-case; There shouldn't be any camelCasing involved.
  - The prefixes for your requests and language strings should also be `whatever` your plugin module is named.
    - EG: `request('/whatever/rest/of/path', {method: 'GET'})`
    - EG: `intl.formatMessage({id: 'whatever.rest.of.id'})`

## Ref

https://github.com/strapi/strapi/releases

https://discord.com/invite/strapi

## Other tutorials beyond

- Fullstack App with Strapi and Next.js: https://www.youtube.com/watch?v=WrmndNpWSJw
- React.js and GraphQL: https://www.youtube.com/playlist?list=PL4cUxeGkcC9h6OY8_8Oq6JerWqsKdAPxn
    - https://github.com/iamshaunjp/Strapi-Crash-Course/tree/lesson-5
- Instant Messenger with React.js and MongoDB: https://strapi.io/blog/how-to-build-a-real-time-chat-forum-using-strapi-socket-io-react-and-mongo-db
- `strapi-plugin-magic`: https://www.youtube.com/watch?v=dcRupfuPa1U

## My personal projects and notes

- https://github.com/atabegruslan/Strapi3/tree/master/plugins/contact-form (Not yet finished)
- https://github.com/atabegruslan/Strapi3/tree/master/plugins/import-content (Not yet finished)
- https://github.com/atabegruslan/Strapi3/tree/master/plugins/wysiwyg
