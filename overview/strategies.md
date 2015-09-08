# Strategy Pattern

__react-validation-mixin__ exposes a factory that accepts an external strategy to handle validation. The user can choose or write a custom validation strategy and have a common API within your React components.

A strategy has a simple Interface that requires a single method `validate(data, schema, key)` to be implemented.

Example: (from _joi-validation-strategy_)

```javascript
import Joi from 'joi';
import union from 'lodash.union';

export default joiOptions => {
  return {
    validate: function(data = {}, joiSchema = {}, key) {
      const options = {
        abortEarly: false,
        allowUnknown: true,
        ...joiOptions,
      };
      const errors = this._format(Joi.validate(data, joiSchema, options));
      if (key === undefined) {
        union(Object.keys(joiSchema), Object.keys(data)).forEach(function(error) {
          errors[error] = errors[error] || [];
        });
        return errors;
      }
      const result = {};
      result[key] = errors[key];
      return result;
    },
    _format: function(joiResult) {
      if (joiResult.error !== null) {
        return joiResult.error.details.reduce(function(memo, detail) {
          if (!Array.isArray(memo[detail.path])) {
            memo[detail.path] = [];
          }
          memo[detail.path].push(detail.message);
          return memo;
        }, {});
      }
      return {};
    },
  };
};
```
