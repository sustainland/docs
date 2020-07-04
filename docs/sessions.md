---
id: sessions
title: Sessions
---

Session are one of the web fundamentals and we use them to serve users.

## Manage the session

To run operations we need to get the object of the session by **@Session()**

### Read
Reading properties from the session is easy as accessing to attribute of an object
```typescript

@Controller('/users/profile')
export default class HttpController {
    constructor() { }

    @Get('/session')
    getSession(@Session() session: any) {
        return session.id;
    }

    // ...
}
```

### Write/Update

We use the attached **set** method to write keys values to the session or override them.

```typescript

@Controller('/users/profile')
export default class HttpController {
    constructor() { }
    
    //...

    @Post('/session')
    setSession(@Body('id') id: any, @Session() session: any) {
        session.id = id;
        return session;
    }
}
```



### Destroy

We need sometimes to clear the session for example when the user logged out we need to destroy the session to remove it from the server.

```typescript

@Controller('/users/profile')
export default class HttpController {
    constructor() { }
    
    //...

    @Post('/session')
    setSession(@Session() session: any) {
        session.destroy():
        return session;
    }
}


```