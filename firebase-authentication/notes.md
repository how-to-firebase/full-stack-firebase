# Firebase Authentication: Notes

See the [Firebase Authentication docs for web](https://firebase.google.com/docs/auth/web/manage-users).

### onAuthStateChanged
```javascript
firebase.auth().onAuthStateChanged(currentUser => {
  if (currentUser) {
    // User is signed in.
  } else {
    // No user is signed in.
  }
});
```

### Register Email/Password
```javascript
firebase.auth().createUserWithEmailAndPassword(email, password).catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  // ...
});
```

### Sign In Email/Password

```javascript
firebase.auth().signInWithEmailAndPassword(email, password).catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  // ...
});
```

### Create Provider

Google
```javascript
var provider = new firebase.auth.GoogleAuthProvider();
```

Facebook
```javascript
var provider = new firebase.auth.FacebookAuthProvider();
```

Twitter
```javascript
var provider = new firebase.auth.TwitterAuthProvider();
```

GitHub
```javascript
var provider = new firebase.auth.GithubAuthProvider();
```

### OAuth sign in with a provider

Popup
```javascript
firebase.auth().signInWithPopup(provider);
```

Redirect
```javascript
firebase.auth().signInWithRedirect(provider);
```

### Phone Auth

First attach a recaptcha using an element ID...
```javascript
window.recaptchaVerifier = new firebase.auth.RecaptchaVerifier('sign-in-button', {
  'size': 'invisible',
  'callback': function(response) {
    // reCAPTCHA solved, allow signInWithPhoneNumber.
    onSignInSubmit();
  }
});
```

... then capture a phone number from user input and send the sms...
```javascript
var phoneNumber = getPhoneNumberFromUserInput();
var appVerifier = window.recaptchaVerifier;
firebase.auth().signInWithPhoneNumber(phoneNumber, appVerifier)
  .then(function (confirmationResult) {
    // SMS sent. Prompt user to type the code from the message, then sign the
    // user in with confirmationResult.confirm(code).
    window.confirmationResult = confirmationResult;
  }).catch(function (error) {
    // Error; SMS not sent
    // ...
  });
```

...and finally authenticate with the code from user input.
```javascript
var code = getCodeFromUserInput();
confirmationResult.confirm(code).then(function (result) {
  // User signed in successfully.
  var user = result.user;
  // ...
}).catch(function (error) {
  // User couldn't sign in (bad verification code?)
  // ...
});
```