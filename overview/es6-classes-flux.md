# ES6 Classes + FLUX Example

```javascript
import {Component, PropTypes} from 'react';
import Joi from 'joi';
import validation from 'react-validation-mixin';
import strategy from 'joi-validation-strategy';
import classnames from 'classnames';

class UserLogin extends Component {

  constructor(props) {
    super(props);
    this.validatorTypes = {
      username: Joi.string().alphanum().min(3).max(30).required().label('Username'),
      password: Joi.string().regex(/[a-zA-Z0-9]{3,30}/).label('Password')
    };
    this.getValidatorData = this.getValidatorData.bind(this);
    this.renderHelpText = this.renderHelpText.bind(this);
    this.getClasses = this.getClasses.bind(this);
    this.onChange = this.onChange.bind(this);
    this.onSubmit = this.onSubmit.bind(this);
  }

  getValidatorData() {
    return this.props;
  }

  render() {
    return (
      <form onSubmit={this.onSubmit}>
        <div className={this.getClasses('username')}>
          <label htmlFor='username'>Username</label>
          <input
            type='text'
            className='form-control'
            placeholder='Username'
            value={this.props.username}
            onChange={this.onChange('username')}
            onBlur={this.props.handleValidation('username')}
          />
          {this.renderHelpText(this.props.getValidationMessages('username'))}
        </div>
        <div className={this.getClasses('password')}>
          <label htmlFor='password'>Password</label>
          <input
            type='password'
            className='form-control'
            placeholder='Password'
            value={this.props.password}
            onChange={this.onChange('password')}
            onBlur={this.props.handleValidation('password')}
          />
          {this.renderHelpText(this.props.getValidationMessages('password'))}
        </div>
      </form>
    );
  }

  renderHelpText(message) {
    return (
     <span className='help-block'>{message}</span>
    );
  }

  getClasses(field) {
    return classnames({
      'form-group': true,
      'has-error': !this.props.isValid(field)
    });
  }

  onChange(field) {
    return event => {
      const value = event.target.value;
      this.props.updateField(field, value);
    };
  }

  onSubmit(event) {
    event.preventDefault();
    const onValidate = error => {
      if (error) {
        //form has errors; do not submit
      } else {
        this.props.submitForm();
      }
    };
    this.props.validate(onValidate);
  }
}

UserLogin.propTypes = {
  username: PropTypes.string,
  password: PropTypes.string,
  updateField: PropTypes.func,
  submitForm: PropTypes.func,
  errors: PropTypes.object,
  validate: PropTypes.func,
  isValid: PropTypes.func,
  getValidationMessages: PropTypes.func,
  clearValidations: PropTypes.func,
};

export default validation(strategy)(UserLogin);
```
