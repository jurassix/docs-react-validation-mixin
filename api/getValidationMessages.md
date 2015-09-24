# getValidationMessages

### `getValidationMessages([key])`

returns validation message(s) for this field.

This API is a wrapper around `this.props.errors`, that returns validations for a single key, or all validations.

```javascript
this.getValidationMessages('username'); // returns the current validation messages\ for this field or empty array if valid

this.getValidationMessages(); // returns array of messages for all fields or empty array if valid
```
