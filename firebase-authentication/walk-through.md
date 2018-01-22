# Firebase Authentication: Walk-through

Welcome to Full-Stack Firebase with Chris Esplin and Juarez Filho

The app you see before you is publicly accessible at fogo, that's spelled F-O-G-O, dot how to firebase dot com

So let's see Firebase Authentication in action in the wild.

We'll start off by logging into Google with a Provider.

Don't worry... providers are trivial to create, and we'll cover them elsewhere

Just note that this provider is specific to Google. 

You'll have different providers for Facebook, Twitter or GitHub.

Now simply pass the provider into auth.signInWithRedirect and we're off and running

Notice how our browser automatically redirects to Google's auth page

When authentication is complete we're redirected back to our app

Our auth state has changed, so our onAuthStateChanged listener fires with our new currentUser

The currentUser is our J-W-T or JOT, and it represents our authentication session.

Notice the currentUser's displayName, email and emailVerified attributes

There's some extra provider data available as well.

Now we'll let our application-specific logic respond to the new currentUser object and log us into the app.

And now that we've logged in, let's log out!

auth.signOut() will get the job done for us.

Notice how our onAuthStateChanged listener fires again, this time with a null currentUser

... and that's it!

You can log in with different OAuth providers, an email and password or even a mobile phone.

Firebase Authentication is quick and easy to implement, and it provides the backbone for
all of Firebase's security.