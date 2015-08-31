# isValid

### `isValid([key])`

returns true|false depending on the validity of the current state.

This API is a wrapper around `this.props.errors`, that allows developers to check for the validity of a single field or entire form.

```javascript
this.isValid('username'); // returns boolean for validity of only this field

this.isValid(); // returns boolean for validity of all fields in schema
```
