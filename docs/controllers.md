---
id: controllers
title: Controllers
---

Controllers are a group related **HTTP** request handlers organized into classes, they are also the main responsible for returning **responses** to the client.

```typescript
import { Get, Post,  Request, Params, Response, Interceptors } from "@sustain/common";
import { Controller } from  "@sustain/core";
import { UserService } from "../services/user.service";
import { Auth } from "../auth";


@Controller()
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


A decorator is a new way to JavaScript developer to do things **[Thanks to TypeScript](https://www.typescriptlang.org/)**, get access to variable, manipulate and write data into them.

There's a bunch of operator in Sustain framework that let you access and manipulate the HTTP inctances.

**PS:** You can also use scoped route defined in the controller like:

```typescript
import { Get, Post,  Request, Params, Response, Interceptors } from "@sustain/common";
import { Controller } from  "@sustain/core";
import { UserService } from "../services/user.service";
import { Auth } from "../auth";


@Controller('users') // define route prefix
export default class UserController {
    constructor(private userService: UserService) { }

    @Get()
    users() {
        return this.userService.list();
    }

    @Get(':id')
    user(@Request() request: any) {
        const { id } = request.params
        return this.userService.get(id);

    }
}

```
This will generate

- **GET** /users

- **GET** /users/:id
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

With the @Post() decorator you can start accepting post request.
```typescript
@Controller()
export default class HomeController {
    @Post()
    PostBody(@Body() body: string) {
        return body;
    }
}

```
## @Body()

You can grap all the body by the  @Body() decorator or you ca pick a single element from the Body by passing the name of the attribute


```typescript
@Controller()
export default class HomeController {
    @Post()
    PostBody(@Body('name') name: string) {
        return name;
    }
}

```


## @Params()
  
Picking the url params is easy with the @Params() decorator.
  

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

## @Headers()

Grabing the headers from the request is easy by using **@Headers** decorator

We will take an example from the Interceptors


```typescript
import { Next, Response, Headers, Interceptor} from "@sustain/common";
import { Injectable } from "@sustain/core";

@Injectable()
export class Auth{

    static isAuthenticated(@Next() next: any, @Response() res : any, @Headers() header: any) {
        const { host } = header;
        console.log(`Enter in isAuthenticated from host ${host}`);
        next();
    }
    ...
}
```
In this example we will pick the host from the headers by passing the name **@Header('host')**


```typescript
import { Next, Response, Headers } from "@sustain/common";
import { Injectable } from "@sustain/core";

@Injectable()
export class Auth{

    static isAuthenticated(@Next() next: any, @Response() res : any, @Headers('host') host: any) {
        console.log(`Enter in isAuthenticated from host ${host}`);
        next();
    }
    ...
}
```

## Responses


The response is how to reply to the browser or the app that called the server, and we can grap the responce object with **@Response()** decorator.

There's more than one way  to send response.


### Using the response object

```typescript
// reply with string
greeting(@Response() response: HttpResponse) {
    const greetingMessage = "Hello Sustainer";
    response.end(greetingMessage);
}

//replay with json object
async getUserDetails(@Response() response: HttpResponse, @Params() params: HttpParams) {
    const { id } = params;
    const userDetails = await this.userService.syncGet(id);
    response.end(userDetails);
}

```
### Responce by returning value (string, json, ...)
```typescript
// reply by return a string
greeting() {
    return "Hello Sustainer";
}
```

### Responce by returning a promise  (async/await, Promise)

```typescript
// reply by return a promise
getUserDetails(@Params() params: HttpParams) {
    const { id } = params;
    return this.userService.syncGet(id);
}

```