# Strapi 3 CMS

Quickstart: https://www.youtube.com/watch?v=6FnwAbd2SDY

https://docs-v3.strapi.io/developer-docs/latest/getting-started/introduction.html

- https://www.youtube.com/c/AlextheEntreprenerd
    - https://www.youtube.com/playlist?list=PLjhq46XB5LWukdEt29lTaJl9XFG9CBMOp

## Plugin

- https://docs-v3.strapi.io/developer-docs/latest/development/local-plugins-customization.html
- https://strapi.io/blog/how-to-create-an-import-content-plugin-part-1-4 (read later)
- https://www.youtube.com/watch?v=kIZHzbmnhnU
    - https://www.youtube.com/watch?v=kIZHzbmnhnU&t=960s

## Auth

- https://www.youtube.com/watch?v=TIK9CYDgs5k&list=PL2dKqfImstaROBMu304aaEfIVTGodkdHh&index=3 
- https://www.youtube.com/watch?v=EjBpJuqqi3U 
- https://www.youtube.com/watch?v=xv5TWP3tCKs 
- https://forum.strapi.io/t/retrieve-user-details/5082/2
- https://strapi.io/blog/a-beginners-guide-to-authentication-and-authorization-in-strapi (v4, but relatable to v3)
    - https://www.youtube.com/watch?v=vcopLqUq594&t=4336s

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

Using MySQL instead of SQLite: https://www.youtube.com/watch?v=PaNSN_h1_JA

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
