---
id: configs
title: Configuration
---

From the start of the application age, the configuration is essential to make the software working in different environment.

## Sustain.yaml file

Here an example the yaml file use in the `jwt-authentication` template
```yaml
# Sustain config file
# documentation : https://sustainland.github.io/docs

domain: localhost
development:
  port: 5003
  core:
  extensions:
    swagger: true
production: 
  port: 6993
swagger:
  securityDefinitions:
    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key
  security:
    - ApiKeyAuth: []
  info:
    title: Sustain REST API
    version: '1.0.0'
    description: Generated with `Sustain`
  swagger: '2.0.0'
jwt_config:
....



```

**``port``** : here you can define port for your application, you can also define it in the application source
```typescript
....
@App({
  modules: [SwaggerModule, AuthModule, UserModule],
  controllers: [BaseController],
  middleswares: [bodyParser.json()],
  providers: [HelloService],
  extensions: [RequestLoggerExtension],
  staticFolders: [{path: 'public'}],
  port : 5330 // <--
})
class AppModule {}
....
```
**``module_name {core}``** : You can pass params to modules through the config file.

For example, here we want to enable the log extension
```yaml
...
core:
  extensions:
    swagger: true
...
```  

## Envirement

In the previous example, we can see that we have two environments ``production`` and ``development``
To run the application with the production mode you can execute

```shell
$ sustain start --env production
```

Running the application without passing the environment will fallback to  ``development``

We can share a variable between all environments and to do that you just need to define it in the root 

Example

```yaml
# Sustain config file
# documentation : https://sustainland.github.io/docs

domain: localhost
port: 5005
development:
  core:
  extensions:
    swagger: true
production: 
  core:
  extensions:
    swagger: false
...

```