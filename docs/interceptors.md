---
id: interceptors
title: Interceptors
---

It intercepts your request, executes some treatment and let it go, that's it.

## Use cases

The use case the most interesting for using Interceptors is with controllers. Let’s take an example of a token validation

It implements `` SustainInterceptor `` interface from `` @sustain/core `` module

``` typescript
import { Response, Next, Headers } from '@sustain/common';
import { Injectable, SustainInterceptor } from "@sustain/core";

@Injectable()
export class CanLoginInterceptor implements SustainInterceptor {
    constructor() { }
    /**
     * @description using decorators in interceptors, passing to next interceptor or route handler function with @Next
     * we can use all params route decorators like : @Response, @Request, @Session, @Params, @Files ....
     */
    intercept(@Next() next: any, @Response() response: any, @Headers() header: any) {
        const { host } = header;
        /*

        * ... token validation ...

        */
        response.end( `You are not authorized to execute this request, ${host}` );
        next();
    }

}
```

As shown above, an interceptor is an `` @Injectable`` class so it should be imported in the `` app module `` .

``` typescript
@App({
    controllers: [
        BaseController,
        DatabaseProvider,
        PlayerController,

    ],
    providers: [
        LoggerService,
        UserService,
        CanLoginInterceptor,  // <-- addded here
    ],
    port: process.env.PORT || 5002,
   ...
})
```

And to use this Interceptor in the controller, we should use a method decorator ` ` @Interceptors ` `

``` typescript
...
@Controller('/player')
@CrudModel(PlayerDto)
export default class PlayerController extends TypeORMCrudController<PlayerDto> {
    constructor() {
        super(PlayerDto)
    }

    @Interceptors([CanLoginInterceptor]) // <-- used here.
    @Get('/:id')
    overridedDelete(@Param('id') id: string) {
        // A validation process here.
        return 'You are not authorized to run delete operation';
    }
}

....
```

We can also have an Interceptor in the controller. 
Let's look at the example here.

``` typescript
import { Response, Next, Headers } from '@sustain/common';
import { Injectable, SustainInterceptor } from "@sustain/core";

@Injectable()
export class PlayerInterceptor implements SustainInterceptor {
    constructor() { }

    intercept(@Next() next: any, @Response() response: any, @Headers() header: any) {
        const { host } = header;
        response.end( `Enter in PlayerInterceptors ${host}` );
        next();
    }
}
```

Don't forget to import it in the main app module

``` typescript
@App({
    controllers: [
        BaseController,
        DatabaseProvider,
        PlayerController,

    ],
    providers: [
        LoggerService,
        UserService,
        CanLoginInterceptor,  
        PlayerInterceptor, // <-- added here
    ],
   ...
})
```

And in the controller 

``` typescript
...

@Controller('/player', {
    interceptors: [
        PlayerInterceptors //  added here
    ]
})
@CrudModel(PlayerDto)
export default class PlayerController extends TypeORMCrudController<PlayerDto> {
    constructor() {
        super(PlayerDto)
    }
...
```
