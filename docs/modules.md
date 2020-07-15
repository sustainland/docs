---
id: modules
title: Modules
---
A sustain module is a portion of the application that can included in other sustain applications.

It behave like the main appliation by importing, injecting prodivers and declaring controllers.

It also can be expoterted and shared across the Internet.

### Structure
A module is a folder that mainly contain a module declaration file and a controller.
It also can hold an entire application that can be imported is the main application

Here's a basic example ``UserModule``

```typescript
+---modules
|   \---users
|           UserController.ts
|           UserModule.ts
|           UserService.ts
```


The module file will be close to the app.ts file.

```typescript
import { UserService } from './UserService';
import { Module } from '@sustain/core';
import UserController from './UserController';
@Module({
    controllers: [
        UserController
    ],
    providers: [UserService]
})
export class UserModule { } 
```

And simply imported in the app.ts file 
```typescript
...
@App({
    modules: [
        UserModule // <-- imported here
    ],
    controllers: [
        HelloController,
        BaseController
    ],
    ....
})
```
