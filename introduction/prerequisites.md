# Prerequisites

There are always prerequisites.

### JavaScript all the way down

We'll be doing the bulk of our work in JavaScript. Firebase supports JavaScript for the web, Java for Android and whatever language iOS uses these days. It also has quite a bit of support for C++, Go and Python.

We focus entirely on JavaScript. We only write JavaScript. We're web guys, and this is a web course, so we won't get distracted with any non-JavaScript languages.

### HTML & CSS

Unfortunately for us all, the web browser is still an under-developed platform. It needs some sugar to make it digestible, and by sugar we're referring to frameworks. 

We're huge fans of native web components and hope that they bring an end to the JavaScript framework wars; however, we live in a world with Internet Explorer 11 and old Firefox browsers, so first-class web component support is still a few years away for most of us.

Therefore we're using frameworks instead of developing in native web components. We'll stick to the two most popular and universal frameworks, Angular 2+ and React. Angular 2+ has massive adoption, and so does React. Additionally, React has spawned an entire ecosystem of spinoff frameworks such as Preact and Inferno which use JSX for rendering. If you're comfortable with JSX, you'll have no trouble following our React code.

### Firebase is vanilla

The bright side is that after agonizing over framework decisions and just how we plan to render our browser pages, the Firebase SDK is entirely vanilla.

And by vanilla we mean vanilla JavaScript. It's just JavaScript. You can copy/paste our code samples into Vue.js or Stenciljs or jQuery for that matter, and they will just work.

The Firebase SDKs also aim for parity between Node.js and the browser. Don't get us wrong... the underlying code is quite different in Node.js and the browser environment; however, the Firebase SDK provides a very similar ***external*** API in both environments, so your life just got much simpler. The Firebase calls that you make in the browser will look nearly identical to the calls that you'll make in Node.js. 

### And speaking of Node.js...

Cloud Functions, Google's functions-as-a-service or FaaS platform, uses Node.js. It also uses the command line.

If you're still not comfortable with the command line, don't worry. We'll take it slowly. However! Node.js is executed on a server, not in a browser. So we'll be executing Node.js locally in our test environment before shipping it off to Cloud Functions to be run in Cloud Functions' functions-as-a-service environment.

Also, some interactions with Firebase services require the *Firebase Tools*ah command-line interface or CLI. Firebase Tools is not particularly complicated... but it does use the command line. 

Every developer needs to get comfortable with the command line at some point. Today is a golden opportunity!
