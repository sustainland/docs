---
id: requests
title: @Request()
---

With @Request() decoratorr you can get the object of the HTTP request in the HTTP controllers, [@Interceptors](hello-link) and Guards

for demo purpuse in this example we will extract the id from the params object from the request other than using @Params or @Param decorators
```typescript
userDetails(@Request() request: HttpRequest) {
    const { id } = request.params;
    return this.userService.syncGet(id);
}
```

