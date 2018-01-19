# Demo Install

You can take this course without installing a local copy of the demo.

You can always come back to this page later if you change your mind :)

## Check out the code

The code can be found on the [How To Firebase GitHub repo](https://github.com/how-to-firebase/fogo).


Clone the project to your local filesystem with

```bash
git clone https://github.com/how-to-firebase/fogo.git
```

You'll find all of the relevant installation instructions inside `fogo/README.md`.

If you're already a Node.js pro, here's the tldr;

```bash
git clone https://github.com/how-to-firebase/fogo.git
cd fogo
yarn
yarn start
```

## Local development

The startup commands for local development are all combined under `yarn start`.

`yarn start` will use the Preact CLI to spin up a WebPack compiler and a local development
server with [hot module replacement](https://webpack.js.org/concepts/hot-module-replacement/).

It should just work... but with all things WebPack, failures can be baffling, so try Googling your
error messages and scan the course comments for hints.

You should have a working demo in your browser at this point. If not, go through the steps again
and maybe phone a friend.
