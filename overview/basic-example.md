# Basic Example

ES6 Classes (_uncontrolled form_)
```javascript
import {Component, PropTypes, findDOMNode} from 'react';
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
    this.onSubmit = this.onSubmit.bind(this);
  }

  getValidatorData() {
    return {
      username: findDOMNode(this.refs.username).value,
      password: findDOMNode(this.refs.password).value
    };
  }

  render() {
    return (
      <form onSubmit={this.onSubmit}>
        <div className={this.getClasses('username')}>
          <label htmlFor='username'>Username</label>
          <input
            ref='username'
            type='text'
            className='form-control'
            placeholder='Username'
            onBlur={this.props.handleValidation('username')}
          />
          {this.renderHelpText(this.props.getValidationMessages('username'))}
        </div>
        <div className={this.getClasses('password')}>
          <label htmlFor='password'>Password</label>
          <input
            ref='password'
            type='password'
            className='form-control'
            placeholder='Password'
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

  onSubmit(event) {
    event.preventDefault();
    const onValidate = (error) => {
      if (error) {
        //form has errors; do not submit
      } else {
        //no errors; submit form
      }
    };
    this.props.validate(onValidate);
  }
}

UserLogin.propTypes = {
  errors: PropTypes.object,
  validate: PropTypes.func,
  isValid: PropTypes.func,
  handleValidation: PropTypes.func,
  getValidationMessages: PropTypes.func,
  clearValidations: PropTypes.func,
};

module.exports = validation(strategy)(UserLogin);
```
