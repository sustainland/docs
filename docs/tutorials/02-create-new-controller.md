---
id: create-new-controller
title: 02-Create Controller
---

## Folder structure
in sustain application we group the controllers in a folder to prevent the noise in the root path.

## The Controller
let's imagine that we will start a project management project, after we created our application, now we will create the fisrt controller: ```ProjectController``

```typescript
import { Get } from "@sustain/common";
import { Controller } from "@sustain/core";

@Controller('/project')
export default class ProjectController {
    constructor() { }

    @Get('/list')
    listProject() { return 'OK' }

}
```
New folder structure

```ts
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo-text.png
│   └── logo.png
├── src
│   ├── app.ts
│   └── controllers <-- added folder
│       └── ProjectController.ts <-- added controller
└── tsconfig.json
```

## Import and usage
Now we have the controller, we need to inform the application about it.
What we need to is to import it in the main application file ``app.ts``

```typescript
import { App, bootstrap, Controller } from '@sustain/core';
import { Get } from '@sustain/common';
import ProjectController from './controllers/ProjectController'; // <-- import new controller

@Controller()
export default class HelloController {
    constructor() { }

    @Get()
    hello(): string {
        return `Hello Sustainers`;
    }
}
@App({
    controllers: [
        HelloController,
        ProjectController // <-- add it here
    ],
    port: process.env.PORT || 5002
})
class AppModule { }

module.exports = bootstrap(AppModule);

```
Now we need to create a service to manipulate data and access, will see this in the next section