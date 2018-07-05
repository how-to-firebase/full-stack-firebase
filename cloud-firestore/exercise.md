# Challenge: Cloud Firestore

Firestore is the heart of this application.

It's going to be the longest, most complex module, so be patient with yourself.

Firestore is where we store our data. We're going to learn to save data to Firestore. We'll learn a few ways to query that data back into the application. 

And we're going to write some Firestore security rules to lock down our database.

Firestore is a deep topic and we can't cover every aspect of it;

however, we'll learn more than enough to get started.

Completing this series of challenges will give you a sense of what you can do with Firestore...

...and it will put you in a great place to start integrating Firestore into your own apps.

So let's get started!

First, run `git status` to see what changes you've made to your branch.

You'll likely want to run `git stash` to stash your changes.

Then run `git checkout challenge dash firestore` to get to the new branch.

The `challenge dash firestore` branch has the completed code from `challenge dash authentication`...

...but it doesn't have the changes that you made to your environment files.

So either edit your environment files manually, or use `git stash pop` to recover your changes.

If you're happy with your authentication code, go ahead and leave it.

If you'd like to continue with the solution's auth code, type `git checkout src`

This will remove changes to all files within the `src` folder...

...but it won't touch the changes to your `public slash environments` files.

Or type `git checkout dot` to start from scratch...

...and then copy/paste your own project environment values back into your environment files.

Type `git diff` to see all of your changes.

Use the arrow keys to scroll up and down through your changes...

...and type `q` for 'quit' once you're done reviewing your 'git diff'.

So now that we're ready to continue...

Search your codebase for the text `challenge firestore`.

You should see a bunch of results, each of which is a task that you'll complete.

The very first task to complete is in `public slash index dot html`.

You'll notice that it's labelled as `challenge firestore zero one`

At the time of this writing, Firestore is still in beta...

...so you may need to include Firestore with its own script tag.

If Firestore is out of beta by the time you're completing this task...

...it's very possible that you merely need to update `firebase dot js` to the latest version.

It's very likely that future versions of `firebase dot js` will include Firestore by default.

You can confirm that you have Firestore on the page by starting up your app with `yarn start`

Then open `https colon slash slash localhost colon 8080` and then open the DevTools console.

If you type `firebase dot firestore` and it returns `undefined`, then you don't have Firestore.

If you see text that looks like a function... then you have Firestore on your page.

Once Firestore is on your page, keep your dev server running and start completing each challenge.

As you complete a challenge, you'll enable a new feature of the application.

You'll be able to go to your demo app on localhost to test each feature out as you add it.

Start with `src slash database slash add dash note dot js`

You'll see challenges two and three in this file.

Completing the 'add note' challenges will let you begin adding notes in the app.

Once that's done, continue with challenges four, five, six and so on.

There are a ton of numbered Firestore challenges.

Once you've completed the first few challenges, the rest will be much, much easier.

You'll be writing a lot of Firestore functionality... but you'll get the hang of it quickly.

And now for two words of warning!

First, these challenges use a code pattern called "reactive programming".

Reactive programming is fantastic for handling streams of data, which fits Firestore perfectly.

Reactive programming patterns aren't native to JavaScript, so we'll use the RxJs library.

RxJs is the leading reactive programming library for JavaScript, and it's incredible;

...however; RxJs is a tough library to learn.

So we will only be using RxJs observables. 

Observables are the best part of RxJs, and they will make our lives much, much easier.

So you're going to need to learn the basics of creating an RxJs observable.

RxJs is not required to use Firestore.

But Firestore's onSnapshot function creates data streams...

...and handling data streams is very tricky without a strong programming pattern...

...and so we'll use RxJs, because it's the best way we've found to handle Firestore data streams.

And for our second word of warning:

These challenges will take at least an hour to complete... and maybe much longer.

Don't let yourself get hung up on any one challenge.

Use the solutions to get unstuck and keep moving.

If you feel like you used the solutions too much and didn't learn enough...

...reset the branch with `git checkout src` and run through the challenges again.

Firestore is a complicated topic, and while we'd love to pretend that we can make it easy...

...it's not going to be easy unless you already have deep knowledge of JavaScript.

So forge ahead. 

Be patient with yourself.

Use the solutions liberally...

...and you'll soon be on your way to becoming a Firestore expert!






