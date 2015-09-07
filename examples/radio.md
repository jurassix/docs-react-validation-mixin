# Radio

Simple component with radio input validation.

```javascript
import React from 'react';
import classnames from 'classnames';
import Joi from 'joi';
import strategy from 'joi-validation-strategy';
import validation from 'react-validation-mixin';

var Demo = React.createClass({
  displayName: 'Demo',
  validatorTypes:  {
    referral: Joi.any().valid('tv', 'radio'),
  },
  getValidatorData: function() {
    return this.state;
  },
  getInitialState: function() {
    return {
      referral: null,
    };
  },
  render: function() {
    return (
      <div className='form-group'>
        <label>How did you hear about us?</label>
        <label htmlFor='tv' className='radio-inline'>
          <input
            type='checkbox'
            id='tv'
            name='referral'
            value='tv'
            checked={this.state.referral === 'tv'}
            onChange={this.onRadioChange('referral')}/>
          {' '}tv
        </label>
        <label htmlFor='radio' className='radio-inline'>
          <input
            type='checkbox'
            id='radio'
            name='referral'
            value='radio'
            checked={this.state.referral === 'radio'}
            onChange={this.onRadioChange('referral')}/>
          {' '}radio
        </label>
        {this.props.getValidationMessages('referral').map(this.renderHelpText)}
      </div>
    )
  },
  renderHelpText: function(message) {
    return (
      <span className='help-block'>{message}</span>
    );
  },
  onChange: function(field) {
    return event => {
      let state = {};
      state[field] = event.target.value;
      this.setState(state, this.props.handleValidation(field));
    };
  }
});

module.exports = validation(strategy)(Demo);
```
