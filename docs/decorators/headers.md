---
id: headers
title: @Headers()
---

## The decorator @Headers() return all the header attached to the HTTP request.

Here's an example for using the header in the @Interceptors() 

```typescript
@Injectable()
export class Auth{
    /**
     * @descripttion: check if the user is authenticated
     */
    static isAuthenticated(@Next() next: any, @Headers() header: HttpHeaders) {
        const { host } = header;
        console.log(`Enter in isAuthenticated from host ${host}`);
        next();
    }
}
```

We see here that we're using a new decorator **@Next**, more details at the @Interceptors section.

