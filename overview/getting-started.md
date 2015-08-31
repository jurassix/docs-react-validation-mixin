# Getting Started

Import the following dependencies:

```javascript
import validation from 'react-validation-mixin'; //import the mixin
import strategy from 'joi-validation-strategy'; //choose a validation strategy
```

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

Wrap the component with the validation mixin and supporting strategy on export:

```javascript
export default validation(strategy)(Component);
```

Wrapped components will receive the following props that serve as the API when validating:

```javascript
propTypes = {
  errors: PropTypes.object,
  validate: PropTypes.func,
  isValid: PropTypes.func,
  handleValidation: PropTypes.func,
  getValidationMessages: PropTypes.func,
  clearValidations: PropTypes.func,
}
```
