# Strategy Pattern

__react-validation-mixin__ exposes a factory that accepts an external strategy to handle validation. The user can choose or write a custom validation strategy and have a common API within your React components.

A strategy has a simple Interface that requires a single method `validate(data, schema, key)` to be implemented.

Example: (from [_joi-validation-strategy_](https://github.com/jurassix/joi-validation-strategy))

```javascript
import Joi from 'joi';
import set from 'lodash.set';
import isEmpty from 'lodash.isEmpty';
import {hydrate} from './utils';
import invariant from 'invariant';

export default joiOptions => {
  return {
    validate: function(data = {}, joiSchema = {}, options = {}, callback) {
      invariant(typeof callback === 'function', 'joi-validation-strategy is asynchronous, a callback is expected: validate(data, schema, options, callback)');
      const {key, prevErrors = {}} = options;
      const validationOptions = {
        abortEarly: false,
        allowUnknown: true,
        ...joiOptions,
      };
      Joi.validate(data, joiSchema, validationOptions, (error) => {
        const errors = this.collectErrors(error);
        if (key === undefined || key === null || isEmpty(errors)) {
          return callback(hydrate(errors));
        }
        return callback(set(prevErrors, key, errors[key]));
      });
    },
    collectErrors: function(error) {
      if (error !== null) {
        return error.details.reduce((errors, {path, message}) => {
          errors[path] = message;
          return errors;
        }, {});
      }
      return {};
    },
  };
};
```
