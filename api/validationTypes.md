# validationTypes

### `validatorTypes`

validatorTypes is the schema defining the validity of a component.

validatorTypes can be defined as an **object or function**, as long as a valid schema is returned.

---
### _joi-validation-strategy_


*See [Joi](https://github.com/hapijs/joi) for a full list of api validation strategies available.*

```javascript
// defined as object
validatorTypes: {
  username: Joi.string().alphanum().min(3).max(30).required(),
  password: Joi.string().regex(/[a-zA-Z0-9]{3,30}/)
}

// defined as function
validatorTypes: function() {
  return Joi.object().keys({
    username: Joi.string().alphanum().min(3).max(30).required(),
    password: Joi.string().regex(/[a-zA-Z0-9]{3,30}/)
  });
}

// defined as a function with conditional component state
validatorTypes: function() {
  var base = Joi.object().keys({
    firstName: Joi.string().required(),
    lastName: Joi.string().allow(null),
    email: Joi.string().email(),
    username:  Joi.string().alphanum().min(3).max(30).required()
  });
  if (this.props.user.anonymous) {
    return base.keys({
      newPassword: Joi.string().regex(/[a-zA-Z0-9]{3,30}/),
      verifyPassword: Joi.ref('newPassword')
    });
  } else {
    return base;
  }
}
```

## Human readable errors

Messages like "serialNumber must be a number" are not very friendly.
Joi provides an option to override key name in message with [label](https://github.com/hapijs/joi#anylabelname) method.

```javascript
validatorTypes: function() {
  return {
    serialNumber: Joi.number().required().label("Serial Number"),
    assemblyDate: Joi.date().required().label("Assembly Date"),
    manufacturer: Joi.string().required().label("Manufacturer"),
  };
}
```

You can also use `this.refs` to pull the labes from the form itself:

```javascript
validatorTypes: function() {
  return {
    serialNumber: Joi.number().required().label(this.refs.serialNumber.props.label),
    assemblyDate: Joi.date().required().label(this.refs.assemblyDate.props.label),
    manufacturer: Joi.string().required().label(this.refs.manufacturer.props.label),
  };
},
```
