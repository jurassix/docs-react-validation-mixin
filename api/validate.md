# validate

### `validate([key,] callback)`

Asynchronous validation of entire form or specifc key if provided. Error-first callback will return an error if form or key is invalid.

This API allows developers to validate a single field or the entire form if no key is provided.

```javascript
this.validate(function(error) {
  if(error) {
    // form contains errors
    return;
  }
  // form is valid, fire action
});

```
