# getValidatorData

### `getValidatorData`

This API defines the data to validate your schema against; props, state, refs, or a combination of any.

_Note: this should not be a static method, should return live data to validate_

```javascript
//props
getValidatorData: function() {
  return this.props;
}

//state
getValidatorData: function() {
  return this.state;
}

//refs
getValidatorData: function() {
  return {
    username: findDOMNode(this.refs.username).value,
    password: findDOMNode(this.refs.password).value
  };
}
```
