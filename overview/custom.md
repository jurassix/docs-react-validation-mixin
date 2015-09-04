# Custom Validation Messages and I18N

Custom messages are dependent on the strategy chosen. The below example assumes _joi-validation-strategy_.

There are two ways to define custom messages; on the schema via options and globaly as joiOptions provided to the strategy.

#### [See Joi's`language.js` for a complete list of messages](https://github.com/hapijs/joi/blob/master/lib/language.js)

### 1) Options provided to specific schema

By providing the options to an individual schema, you can override messages when validating the single field.

```javascript
validatorTypes:  {
  firstName: Joi.string().required().label('First Name'),
  lastName: Joi.string().allow(null).label('Last Name'),
  email: Joi.string().email().min(3).max(10).required().label('Email').options({
    language: {
      any: {
        required: '{{key}} custom required message.',
      },
      string: {
        base: '{{key}} custom string message.',
        email: '{{key}} custom email message.',
      }
    }
  })
```

### 2) Global message override via strategy

By providing the options to the **strategy**, the custom messages will be applied to all schema validations. Using this pattern you can externalize the overrides and provide Internationalization or centrally manage all messages displayed to users.

```javascript
const options = {
  language: {
    any: {
      required: '{{key}} custom required message.',
    },
    string: {
      base: '{{key}} custom string message.',
      email: '{{key}} custom email message.',
    }
  }
};
export default validation(strategy(options))(component);
```
