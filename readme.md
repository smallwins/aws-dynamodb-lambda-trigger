# aws-dynamodb-lambda-trigger

Cleans up the code you need to run a DynamoDB trigger on AWS Lambda. 

```javascript
var trigger = require('aws-dynamodb-lambda-trigger')

// regular lambda fn business here
exports.handler = function myLambdaTriggerForTableName(event, context) {

  // register a handler for inserts
  trigger.insert(event, context, function handleInsert(record, callback) {
    console.log(record)
    callback()
  })
}
```

## api

- `trigger.insert(event, context, handler)` register a handler for `INSERT`
- `trigger.modify(event, context, handler)` register a handler for `MODIFY`
- `trigger.remove(event, context, handler)` register a handler for `REMOVE`
- `trigger.all(event, context, handler)` register a handler for `INSERT`, `MODIFY` and `REMOVE`
- `trigger.save(event, context, handler)` register a handler for `INSERT`, `MODIFY`
- `trigger.change(event, context, handler)` register a handler for `INSERT` and `REMOVE`

In all cases the `handler` is a function with the following signature:

```javascript
function handleSave(record, callback) {
  // do something with record and then callback node-style
  // if an err is passed to the callback it will be passed to context.fail
  callback(null, record)
}
```

### even CLEANER api

```
var trigger = require('aws-dynamodb-lambda-trigger/lambda')

// runs on every record inserted
function handler(record, callback) {
  callback()
}

exports.handler = trigger.insert(handler)
```
