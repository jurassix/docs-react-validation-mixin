# Checkbox

Simple component with checkbox input validation.

```javascript
import React from 'react';
import classnames from 'classnames';
import Joi from 'joi';
import strategy from 'joi-validation-strategy';
import validation from 'react-validation-mixin';

var Demo = React.createClass({
  displayName: 'Demo',
  validatorTypes:  {
    rememberMe: Joi.boolean(),
  },
  getValidatorData: function() {
    return this.state;
  },
  getInitialState: function() {
    return {
      rememberMe: 'off',
    };
  },
  render: function() {
    return (
      <div className='form-group'>
        <label>
          Remember me{' '}
          <input
            type='checkbox'
            ref='rememberMe'
            value='on'
            checked={this.state.rememberMe === 'on'}
            onChange={this.onCheckboxChange('rememberMe')}/>
        </label>
        {this.props.getValidationMessages('rememberMe').map(this.renderHelpText)}
      </div>
    )
  },
  renderHelpText: function(message) {
    return (
      <span className="help-block">{message}</span>
    );
  },
  onCheckboxChange: function(field) {
    return event => {
      let state = {};
      state[field] = this.state[field] === 'on' ? 'off' : 'on';
      this.setState(state, this.props.handleValidation(field));
    };
  }
});

module.exports = validation(strategy)(Demo);
```
