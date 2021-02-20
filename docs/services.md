---
id: services
title: Services
---

Let's talk about Dependency injection before we move to services
## Dependency injection
Dependency injection is basically providing the objects that an object needs (its dependencies) instead of having it construct them itself. It's a very useful technique for testing, since it allows dependencies to be mocked or stubbed out.

And this take us to services Sections

## Services 
A service is an instance of a TypeScript/JavaScript class decorated with ``@Injectable()`` decorator.

```typescript
import { Injectable } from "@sustain/core";
import { User } from "../models/user.model.ts";

@Injectable()
export class UserService {
    private users = [ ... ] // bunch of fata
    constructor() { }

    getById(id: string): User {
        return this.users.find((user: any) => user._id === id);
    }
    allUsers(): User[] {
        return this.users;
    }
}

```
**📄 app.ts** 

The service can be imported in the main application or in one of the application modules, that can be injected using the Injector whenever we need it.

```typescript
...
@App({
    modules: [
        UserModule
    ],
    controllers: [
        HelloController,
        BaseController
    ],
    providers: [
       UserService, // <---- imported here
    ]
})
class AppModule { }
...
```

Then our UserService can be Injected in any application controllers

Let's create our controller ``UserController``

```typescript
import { UserService } from './UserService';
import { Controller } from '@sustain/core';
import { Get, Response, Param } from '@sustain/common';

@Controller('/user')
export default class UserController {
    constructor(
        private userService: UserService // <-- Injected here
    ) { }

    @Get()
    allUsers(): any[] {
        return this.userService.list();
    }

    @Get('/:id')
    singleUser(
        @Response() response: any,
        @Param('id') id: string,
    ): any {
        response.setHeader('Content-Type', 'application/json');
        return this.userService.get(id);
    }
}
```

Passing the ``UserService`` in the constructor the ``Injector`` will grap an Instance of the service and it will be accessible in the controller.