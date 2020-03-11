---
id: controllers
title: Controllers
---

Controllers are a group related **HTTP** request handlers organized into classes, they are also the main responsible for returning **responses** to the client.

```typescript
import { Get, Post,  Request, Params, Response, Interceptors } from "@sustain/http";
import { Injectable } from  "@sustain/core";
import { UserService } from "../services/user.service";
import { Auth } from "../auth";


@Injectable()
export default class UserController {
    constructor(private userService: UserService) { }

    @Get('/users')
    users() {
        return this.userService.list();

    }

    @Get('/user/:id')
    user(@Request() request: any) {
        const { id } = request.params
        return this.userService.get(id);

    }
}

```

## @decorators

A decorator is a new way to JavaScript developer to do things **[Thanks to TypeScript](https://www.typescriptlang.org/)**, get access to variable, manipulate and write data into them.

There's a bunch of operator in Sustain framework that let you access and manipulate the HTTP inctances.



## Controller decorators


## @Get()
You can listining to the GET Http methods in your controller by using the **@Get('url segment or regex')** 

```typescript
@Get('/')
home() {
    return "Hello Sustain";
}
```

Or you can extract params from url

```typescript
@Get('/:name')
welcome(@Param('name') name: string) {
    return `Welcome ${name}`;
}
```



Here's a full example for the **HomeController**

```typescript

@Controller()
export default class HomeController {
    @Get('/')
    welcome(@Request() request: HttpRequest) {
        return "Hello Sustain";
    }
}

```




## @Post()


# Need content




## @Params(), @Param()
  

The **@Params** decorator let you get all the route params.
  

```typescript

@Get('/user/:id/details')
userDetails(@Params() params: any)  {
    const  {  id  }  = params;
    return  this.userService.syncGet(id);
}

```

You can also get a *single* param by passing the name to the @Param() decorator
**Example**

```typescript

@Get('/user/:id/details')
userDetails(@Param('id') id: string)  {
    return  this.userService.syncGet(id);
}

```