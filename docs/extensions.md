---
id: extensions
title: Extensions
---

Sustain extension is different from middlewares, with the extension we can touch the core of the application from a piece of code outside it  


## Request Logger
Let's take down one of the core extensions the  ``request-logger.extension.ts``

```typescript
...
@Injectable()
export class RequestLoggerExtension implements SustainExtension {
  onResquestStartHook(request: SustainRequest) {
    request.startAt = new Date();
  }
  onResponseEndHook(request: SustainRequest, response: SustainResponse) {
    const endTime: any = new Date();
    const timeDiff: any = endTime - request.startAt;
    let color;
    if (response.statusCode < 400) {
      color = `\x1b[32m$$\x1b[0m `;
    } else {
      color = `\x1B[31m$$\x1b[0m`;
    }
    console.log(
      color.replace('$$', `${request.method} ${request.url}`),
      `${response.statusCode}`,
      'in ',
      timeDiff,
      'ms'
    );
  }
}

```

An extension is simply a class that implements ``SustainExtension`` with hooks that will be executed when the event occurs

## Hooks
Sustain hooks are a lifecycle event for the application and the user requests
|  Hook name |  params  |  Description  | 
|---|---|---|
|  onResquestStartHook |  ```request: SustainRequest ``` | execute when start treating a request 
|  onResponseEndHook |  ```request: SustainRequest   response: SustainResponse``` | execute when treating a request is done 
