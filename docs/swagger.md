---
id: swagger
title: Swagger
---
The [OpenAPI](https://swagger.io/) Specification (formerlthy Swagger Specification) is an API description format for REST APIs. 
Sustain provides out-of-the-box a dedicated extension which allows generating the specification by leveraging decorators.

## @SwaggerAPI()

The @SwaggerAPI() helps define the Swagger properties such as the infos (title, description), the schemes (HTTP, HHTPS) ...

### Example
``` typescript
...

@SwaggerAPI({
    info: {
        title: "Sustain API",
        version: "1.0.0",
        description: "Generated with `Sustain`"
    },
    swagger: "2.0",
    schemes: [
        "http"
    ],
})
@App({
    controllers: [
        HelloController,
    ],
    providers: [
        LoggerService,
        UserService,
    ],
    port: process.env.PORT
})

...
```

### Swagger UI
Navigate to http://localhost:5002/swagger-ui to consult the Swagger UI.


![alt text](../img/swagger-ui.PNG "Swagger Ui")


### Models