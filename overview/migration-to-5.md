# Migrating to 5.0

Follow these steps to have a seemless migration from 4.x to 5.0

### install a strategy

_react-validation-mixin_ is now extendable via strategies. You will need to install the _joi-validation-strategy_ separately.

```javascript
> npm install --save joi-validation-strategy
```

### export your component

Wrap the component with the validation mixin and supporting strategy on export:

```javascript
export default validation(strategy)(Component);
```

### this.validatorTypes and this.getValidatorData

Implement both `validatoryTypes` and `getValidatorData`: _(Invariants will be thrown when these methods are not implemented)_

```javascript
validatorTypes = {
  username: Joi.string().alphanum().min(3).max(30).required().label('Username'),
  password: Joi.string().regex(/[a-zA-Z0-9]{3,30}/).label('Password')
}

getValidatorData() {
  return this.props;
}
```

### this.props

Your component will now receive the validation API as props.

```javascript
propTypes = {
  errors: PropTypes.object,
  validate: PropTypes.func,
  isValid: PropTypes.func,
  handleValidation: PropTypes.func,
  getValidationMessages: PropTypes.func,
  clearValidations: PropTypes.func,
};
```
