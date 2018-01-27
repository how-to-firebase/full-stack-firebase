# Firebase Authentication: Notes

See the [Firebase Authentiction docs for web](https://firebase.google.com/docs/auth/web/manage-users).

### onAuthStateChanged
```
firebase.auth().onAuthStateChanged(currentUser => {
  if (currentUser) {
    // User is signed in.
  } else {
    // No user is signed in.
  }
});
```

### createUserWithEmailAndPassword
```
firebase.auth().createUserWithEmailAndPassword(email, password).catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  // ...
});
```

