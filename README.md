# Repro for PR 6783

# v12.9.1

```shell
$ firebase --version
12.9.1
$ firebase functions:shell --project demo-project
✔ functions: Using node@18 from host.
Serving at port 8922

i functions: Loaded functions: myCallableFunction
⚠ functions: The following emulators are not running, calls to these services will affect production: firestore, database, pubsub, storage, eventarc
firebase > myCallableFunction({'foo': 'bar'})
Sent request to function.
firebase > > {"verifications":{"app":"MISSING","auth":"MISSING"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

> { foo: 'bar' }

RESPONSE RECEIVED FROM FUNCTION: 200, {
    "result": {
        "success": true
    }
}
```

# v13.3.0

Note: invoked function 2 times here <br>
Fist Invoke - `myCallableFunction({'foo': 'bar'})` <br>
Second Invoke - `myCallableFunction({ data: {'foo': 'bar'}})` <br>

```shell
$ firebase --version
13.3.0
$ firebase functions:shell --project demo-project
✔  functions: Using node@18 from host.
Serving at port 8861

i  functions: Loaded functions: myCallableFunction
⚠  functions: The following emulators are not running, calls to these services will affect production: firestore, database, pubsub, storage, eventarc
firebase > myCallableFunction({'foo': 'bar'})
undefined
firebase > >  {"foo":"bar","severity":"WARNING","message":"Request body is missing data."}
>  {"severity":"ERROR","message":"Error: Invalid request, unable to process.\n    at entryFromArgs (/<PATH>/functions/node_modules/firebase-functions/lib/logger/index.js:130:19)\n    at Object.error (/<PATH>/functions/node_modules/firebase-functions/lib/logger/index.js:116:11)\n    at /<PATH>/functions/node_modules/firebase-functions/lib/common/providers/https.js:405:24\n    at /<PATH>/functions/node_modules/firebase-functions/lib/common/providers/https.js:394:25\n    at cors (/<PATH>/functions/node_modules/cors/lib/index.js:188:7)\n    at /<PATH>/functions/node_modules/cors/lib/index.js:224:17\n    at originCallback (/<PATH>/functions/node_modules/cors/lib/index.js:214:15)\n    at /<PATH>/functions/node_modules/cors/lib/index.js:219:13\n    at optionsCallback (/<PATH>/functions/node_modules/cors/lib/index.js:199:9)\n    at corsMiddleware (/<PATH>/functions/node_modules/cors/lib/index.js:204:7)"}

ERROR SENDING REQUEST: FirebaseError: HTTP Error: 400, Bad Request
firebase > myCallableFunction({ data: {'foo': 'bar'}})
undefined
firebase > >  {"verifications":{"app":"MISSING","auth":"MISSING"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}
>  { foo: 'bar' }

RESPONSE RECEIVED FROM FUNCTION: 200, {
  "result": {
    "success": true
  }
}
```
