---
id: create-new-app
title: 01-Create Application
---

## CLI (Command Line Interface)
Creating sustain application is easy using the CLI

First we need to install the CLI

```haskell
npm install -g @sustain/cli
```

Then we can generate the application based on a blank template or we can choose one of the available templates:

## Templates


```haskell
// blank template

sustain new sustain-project

```
Generated files from basic template
```haskell
|   package.json
|   tsconfig.json
|
+---public
|       favicon.ico
|       index.html
|       logo-text.png
|       logo.png
|
\---src
    |   app.ts
    |   constants.ts
    |   index.ts
    |
    +---controllers
    |       BaseController.ts
    |       HelloController.ts
    |
    +---modules
    |   \---users
    |           UserController.ts
    |           UserModule.ts
    |           UserService.ts
    |
    \---services
            HelloService.ts
```

Minimal template (one file application)

```haskell
// minimal template

sustain new sustain-project -t minimal

```
Generated files from minimal template
```haskell
|   package.json
|   tsconfig.json
|
+---public
|       favicon.ico
|       index.html
|       logo-text.png
|       logo.png
|
\---src
        app.ts

```

You can check all the available templates list at : https://github.com/sustainland/sustain/tree/master/samples