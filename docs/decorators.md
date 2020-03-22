---
id: decorators
title: Decorators
---

A decorator is a new way to JavaScript developer to do things **[Thanks to TypeScript](https://www.typescriptlang.org/)**, get access to variable, manipulate and write data into them.

There's a bunch of operator in Sustain framework that let you access and manipulate the HTTP inctances.


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
  

- Using **@Response** decorator to get the res object sending response directely from the res object

  

```typescript

@Get('/user/:id/details')

userDetails(@Response() response: any, @Params() params: any)  {

const  {  id  }  = params

const  userDetails  =  this.userService.get(id);

response.end(JSON.stringify(userDetails));

}

```

- Using the **@Request** decorator to get the request object

```typescript

@Get('/user/:id/details')

userDetails(@Request() request: any)  {

const  {  id  }  = request.params;

return  this.userService.syncGet(id);

}

```