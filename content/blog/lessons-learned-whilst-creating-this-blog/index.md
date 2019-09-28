---
title: Testing Express with Jest.
date: "2019-09-28T11:00:00.001Z"
description: "A how to guide using SuperTest"
---

I'm going to assume a little knowledge straight of the bat and that is that you know what [Express](https://expressjs.com/) and [Jest](https://jestjs.io/) are.

**Goal:** I want to write a test which will assert that when I call `/example` I get back a `200` status code with the payload of `{ok: true}`.

To do this we can use a pacakge called [SuperTest](https://www.npmjs.com/package/supertest), this package allows us to "glue" together the Express application
and the Jest tests.

One thing we need to make sure we do though is not run the server when we run our tests, we can achieve this quite easly by exporting the `app` variable and having another file for starting the server which would look something like this.

```javascript
// app.js
import express from 'express';
const app = express();

app.get(
    '/example', 
    (request, response) => {
        response.json({ok: true});
    }
);

export default app;
```
```javascript
// server.js
import app from './app';
app.listen(3000); 
```

### So how do we use super test with Jest?

```javascript
import { request } from 'supertest';
import app from '../whereever/youveplaced/your/app';

describe('/example', () => {
    it('should successfully return with ok true', () => {
        return request(app)
            .expect(200)
            .then(response => {
                expect(response.body.ok, true);
            });
    });
});
```
This post doens't explain how to handle state that might, however this just requires stubbing the module to return what you want before the tests run.

### Pros
- We're testing our applicaiton code as a whole (an integration test) without running a server, which means it is very fast.

### Cons
- This isn't a comprehensive end to end test, because we wont be able to test client side events with this approach.