---
id: responses
title: @Response()
---

The response is how to reply to the browser or the app that called the server, and we can grap the responce object with @Response decorator.

There's more than one way  to return data.


## 1) Using the response object

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
## 2) Responce by returning value (string, json, ...)
```typescript
// reply by return a string
greeting() {
    return "Hello Sustainer";
}
```

## 3) Responce by returning a promise  (async/await, Promise)

```typescript
// reply by return a promise
getUserDetails(@Params() params: HttpParams) {
    const { id } = params;
    return this.userService.syncGet(id);
}

```

