# Challenge

## Cloud Messaging

## Find the repo

We'll be working on a branch of our [firelist-react](https://github.com/how-to-firebase/firelist-react) repo named [firebase-messaging](https://github.com/how-to-firebase/firelist-react/tree/master).

## Codesandbox

Simply open up the [CodeSandbox.io](https://codesandbox.io/s/github/how-to-firebase/firelist-react/tree/firebase-messaging) project :\)

## Localhost installation

Pull [the repo](https://github.com/how-to-firebase/firelist-react) directly from GitHub...

```text
git clone https://github.com/how-to-firebase/firelist-react.git
cd firelist-react
git checkout firebase-messaging
```

Once you're on the branch, make sure to run `yarn` or `npm install` to get your Node.js dependencies.

Then run `yarn start` or `npm run start` to spin up the development server.

```text
yarn
yarn start
```

## Complete the challenge

We'll be editing `./src/components/messaging.js` and `./public/sw.js`.

Read the comments and complete the steps in those two files.

## Notes on sw.js vs firebase-messaging-sw.js

Firebase Messaging recommends calling your file [firebase-messaging-sw.js](https://firebase.google.com/docs/cloud-messaging/js/receive).

This is fine as long as you don't care about running your app as a [Progressive Web App](https://developers.google.com/web/progressive-web-apps/) or PWA.

However! We believe that all apps should be PWAs, so we've configured this project to use the standard `sw.js` filename.

You'll notice a function in `./src/components/messaging.js` named `registerServiceWorker`

```text
async function registerServiceWorker(messaging) {
  if ('serviceWorker' in navigator) {
    const registration = await navigator.serviceWorker.register('/sw.js');
    messaging.useServiceWorker(registration);
  }
}
```

This function is will register `/sw.js` and then, once that's done, tell messaging to use `/sw.js` instead of `firebase-messaging-sw.js`.

You can only have one service worker per page. This doesn't work any other way. Trust us. We've tried.

Yes, you can add all of the service worker functionality that you like into `firebase-messaging-sw.js` and let Firebase Messaging register that file automatically.

However, this automatic registration will not allow your users to install your app to their homescreen as a PWA... which totally defeats the purpose of a PWA.

Hence our insistence on using `/sw.js`.

