## A Mostly Reasonable Guide to MERN.

Hello! In these pages you will (hopefully) find, as the title implies, a mostly reasonable guide to creating
your own MERN-stack application. A bit of info for those not aware:

### MERN stands for:

MongoDB: a Javascript based, NoSQL database we will be using to store our data.
Express: A node framework we will use to set up our server, API endpoints, and all that good stuff.
React: Our front end!
Node: The underlying layer behind express, which is used to power our backend.

Ok!

### What this book expects of you:

You should be:
- __Comfortable with Javascript:__ while I will do my best to explain any new or interesting features, we will be using JS throughout the entire application, so at least a basic understanding is a must. If you don't feel comfortable with JS yet, I used [Free Code Camp](https://www.freecodecamp.com) and it was an amazing resource when I was getting started. Highly recommend.
- __Know React:__ This is a tutorial for putting all the pieces together to create an application, but not really a tutorial on how to use it. For that, I would recommend [the React Docs](https://reactjs.org/) as an excellent starting point.
- __Have npm or yarn installed__
- __Know the basics of GIT:__ if you want to use version control, you should know how it works. You can ignore these steps if you dont want to use a remote repository.
- __Ready to learn!__

Neat!

### The structure of this book:

We will walk through the process of creating an application from the beginning to end. The first chapter will be mostly getting set up. If you already have some sort of linter set up with rules you like, feel free to skip this one. We will also set up a git repo to hold your fancy new app. Second chapter will be setting up the architecture (folder structure, babel, node modules, etc) for our application. From there, chapters will be arbitrarily grouped by what I deem to be logical, but whose to say. I will keep seperate folders of stylesheets and assets for you to use, referance, or ignore.

_Note:_ this book may use some fancy new ES7/8/9/11/9999, but I will do my best to give you other options. Look for the __OPTIONAL SYNTAX__ tag for other ways to do things.

_Also note:_ while I will do my best to make this a perfect document, that is HIGHLY unlikely to happen. I welcome pull requests, comments, suggestions, etc. All I ask is we keep it polite and helpful!

__Also also note:__ I am writing this on, and pretty much exclusively use a Mac. That means, certain command line commands I use may be slightly different if you are running on Windows. Keep this in mind if you are getting weird errors when trying to run commands.

Ok cool! Now that thats out of the way, lets get down to business!

![business cat meme](http://www.businesscat.happyjar.com/wp-content/uploads/2014/01/2014-01-07-Coffee.png)

## Chapter One: Syntax, syntax, syntax (and version control)

The first thing we're going to do is set up a linter, which is something that will run continously, checking our code for errors and forcing us to stick to a pre-defined style guide. I use (a slighly modified version of) the Air-bnb style guide, so that is what we will be setting up. Feel free to skip this step, but if you don't currently use one, it will be EXTREMELY helpful going forward.

Before we install anything from npm, we should initialize our repo. I will be using yarn because the logo is cute, but feel free to use npm, if you prefer.

__Note:__ any code you see starting with `$` should be run in your command line.

First, make a new folder where all our code will live. Call it whatever you like. Im going to:

`$ mkdir mostly-reasonable-mern-app && cd mostly-reasonable-mern-app`.

__PROTIP:__ You can use the tab key to autocomplete folders and files in the terminal!

Now that we are here, lets first initialize our git repo:

`$ git init`

There are a few things we don't want to commit to our repo, so we can make a _.gitignore_ file to hide those files from being saved to our repo.

`$ touch .gitignore` will create the file. Open it in your text editor of choice. In it, we will add:

```
.gitignore
node_modules/
```

Great! Now we can create a repo on github, and follow the instructions there to add this repo there.

Now, lets initialize our yarn/npm config.

`$ npm init`

You can just keep hitting enter to accept the default values, when asked.

__PROTIP:__ Initializing this after we hook it up to our repo will fill in some of these questions for us.


[The npm site for Airbnb eslint config](https://www.npmjs.com/package/eslint-config-airbnb) is extremely helpful in getting this set up. following their lead, I'm going to run:
```
$ (
  export PKG=eslint-config-airbnb;
  npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
)
```
which will install all the dependencies we need for eslint.

From here, you should look into adding the linter to whatever editor you use. I am using VS Code, and installed the ESLint extension, but all modern editors should have this ability.

Next, we need to create a _.eslintrc_ file to keep our config in.

`$ touch .eslintrc` will create the file, then in your editor, lets add:
```
{
  "extends": "airbnb",
  "plugins": [
      "react",
      "jsx-a11y",
      "import"
  ],
  "env": {
      "browser": true,
      "node": true
  }
}
```

We will make some more changes to this going forward, but for now this should get us started. We can test it out by `$ touch TEST.js` and filling it with some bad js, like so:

```
cosnt h = 'test';

console.log(z);
```

Hopefully, you see 2/3 red squiggles in your editor.
- [eslint] 'h' is assigned a value but never used. (no-unused-vars)
- [eslint] 'z' is not defined. (no-undef)
and potentially:
- [eslint] Newline required at end of file but not found. (eol-last)

The end bit, `(no-unused-vars)` are the rule, so if there is anything you find too oppressive going forward, in the rules section of _.eslintrc_, you can add `"RULE_NAME": [0]` to turn it off. These will be very helpful to us, so we will leave them alone.

__EXTRA READING:__ New line required at end of file kinda a weird rule right? [This is where it originates](https://stackoverflow.com/questions/729692/why-should-text-files-end-with-a-newline).

We can delete this file `$ rm TEST.js`, since we won't need it.

Okay, we're done with this part! Have a treat, friend. You've earned it.

![Donut](https://www.donutbar.com/wp-content/uploads/2017/05/donut-bar-homers.png)


## Chapter Two: Configuring our dev environment

For this app, we will be using some features of JS that are not currently in the spec for the current version of JS. In order for this to work properly, we will be using Babel to transpile our code and make it run with the older syntax.

Let's start by adding:

`$ yarn add babel-cli babel-preset-es2015 babel-preset-stage-0`

We can make a few files to test this is working correctly. Lets `$ touch index.js && touch function.js` and update them as so:

```javascript
// index.js
import { helper } from './function';

helper();

// helper.js
export const helper = () => {
  console.log('YAY! it works!');
}

```

If we try to run these with `$ node index.js`, we should get an error that says something like
`SyntaxError: Unexpected token import`

In our _package.json_ file, lets add a new script to use babel-cli

```javascript
  devDepenencies: { //...
  },
  "scripts" {
    "runfile": "babel-cli index.js --presets es2015,stage-0"
  }
} // end of file
```

This now lets us use the command `$ yarn run runfile`, which will use babel-cli to interpret our `import/export`s. After running this, we should see a message
```
> babel-node index.js --presets es2015,stage-0

YAY! it works!
```

Great! we can `$ rm index.js helper.js` now that we know babel is working correctly.

__Sidenote:__ You may be wondering why we needed to make a start script instead of just running `$ babel-cli index.js --preset es2015,stage-0`. This is because your command line doesnt understand the command `babel-cli`, since it is specific to your node\_modules folder. The start script knows to look in the _node\_modules/_ folder for it.

### A bit about the structure of the app
By the end of this book, we should have something that resembles an application structure like this:

```
mostly-reasonable-mern-app
  - node_modules/
  - backend
    - node_modules/
    - models/
    - routes/
    - auth/
    - server.js
    - package.json
  - web
    - src/
    - public/
    - styles/
    - assets/
    - node_modules/
    - package.json
    - README.md
    - yarn.lock (if applicable)
  .eslintrc
  .gitignore
  .yarnlock (only if using yarn)
```

Setting it up like this, we can see there are two main points of entry, the front end (web), which is seperate from the back end. This makes its easy to continue on and say, add a Mobile folder as well, which could use the same backend. Keeping things as seperate and modular as possible makes working with the app easier in the future.

We can set up the skeleton of this now, so we can hop right in and get to work.

`$ mkdir backend && cd backend && yarn init -y` will create our backend folder and initialize the folder with a package.json, making it ready to add node_modules as necessary. The `-y` flag answers yes to all the questions it asked us the first time, getting us through the process faster.

Be sure to `$ cd ..` to get back to the top level folder.

To set up the front end, we will use `create-react-app`, an extremely helpful way to get started working with react without needing to spend a ton of time configuring webpack. If you don't have it installed already, you can `$ npm i -g create-react-app` to install it globally. This means we have access to the command in the terminal.

__Note:__ you may get an error saying you need to run the command as an admin. Use `$ sudo npm i -g create-react-app` and enter your password to solve this.

Now that we have access to the package, we can just run:

`create-react-app web`

which will set up our web folder almost exactly how we need it!

Nice! Pretty much all the setup is now complete. Now we get to have some fun and start working on the application! In the next chapter, we're going to start with the backend!

## Chapter Three: Express, The Server, and stuff.