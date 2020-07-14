---
id: interceptors
title: Interceptors
---

@Interceptors can be a middleware or a guard, a class of interceptors is a collections of related method that can recieve all http related object, like request, response, header and so on.
Theses methods will be used in the controllers and called using @Interceptors() decorator.

Here's and example, will start by creating the class 

```typescript
import { Next, Response, Headers } from "@sustain/common";
import { Injectable } from "@sustain/core";

@Injectable()
export class Auth{

    /**
     * @description using decorators in interceptors, passing to next interceptor or route handler function with @Next
     * we can use all params route decorators like : @Response, @Request, @Session, @Params, @Files ....
     */
    static isAuthenticated(@Next() next: any, @Response() res : any, @Headers() header: any) {
        const { host } = header;
        console.log(`Enter in isAuthenticated from host ${host}`);
        next();
    }
    
      validateParams(req: any, res: any, next: any) {
        const { id } = req.params;
        // throw  new Error('Hey, handling errors');
        if (isNaN(+id)) {
            res.end('Not a number')
        }
        next();
    }
}
```

And using theses methods in the ``UserController``


```typescript
import { Get, Post, Request, Params, Response } from "@sustain/htpp";
import { Controller, Interceptors } from "@sustain/core";
import { UserService } from "../services/user.service";
import { Auth } from "../auth";


@Controller('/users')
export default class UserController {
    constructor(private userService: UserService) { }

    @Get()
    users() {
        return this.userService.list();

    }

    @Interceptors([
        Auth.isAuthenticated, // this interceptor will be called before executing the userDetails method
        Auth.validateParams, // we can also chain a list of function and they will be called in sequance
    ])
    @Get('/:id/details')
    userDetails(@Request() request: HttpRequest , @Params() params: HttpParams) {
        const { id } = params;
        return this.userService.syncGet(id);
    }
}

```