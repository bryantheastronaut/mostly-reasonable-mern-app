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
- __You have npm or yarn installed__
- __Ready to learn!__

Neat!

### The structure of this book:

We will walk through the process of creating an application from the beginning to end. The first chapter will be mostly getting set up. If you already have some sort of linter set up with rules you like, feel free to skip this one. We will also set up a git repo to hold your fancy new app. Second chapter will be setting up the architecture (folder structure, babel, node modules, etc) for our application. From there, chapters will be arbitrarily grouped by what I deem to be logical, but whose to say. I will keep seperate folders of stylesheets and assets for you to use, referance, or ignore.

_Note:_ this book may use some fancy new ES7/8/9/11/9999, but I will do my best to give you other options. Look for the [TODO: FIGURE OUT WHAT IM GOING TO USE FOR THE OPTIONAL NEW/OLD JS SYNTAX]

_Also note:_ while I will do my best to make this a perfect document, that is HIGHLY unlikely to happen. I welcome pull requests, comments, suggestions, etc. All I ask is we keep it polite and helpful!

__Also also note:__ I am writing this on, and pretty much exclusively use a Mac. That means, certain command line commands I use may be slightly different if you are running on Windows. Keep this in mind if you are getting weird errors when trying to run commands.

Ok cool! Now that thats out of the way, lets get down to business!

![business cat meme][/README_ASSETS/business_cat.png]

## Chapter One: The boring stuff

The first thing we're going to do is set up a linter, which is something that will run continously, checking our code for errors and forcing us to stick to a pre-defined style guide. I use (a slighly modified version of) the Air-bnb style guide, so that is what we will be setting up. Feel free to skip this step, but if you don't currently use one, it will be EXTREMELY helpful going forward.

Before we install anything from npm, we should initialize our repo. I will be using yarn because the logo is cute, but feel free to use npm, if you prefer.

__Note:__ any code you see starting with `$` should be run in your command line.

First, make a new folder where all our code will live. Call it whatever you like. Im going to `$ mkdir mostly-reasonable-mern-app && cd mostly-reasonable-mern-app`.

__PROTIP:__ You can use the tab key to autocomplete folders and files in the terminal!

Now that we are here, lets initialize our yarn/npm config.

`$ yarn

[The npm site for Airbnb eslint config](https://www.npmjs.com/package/eslint-config-airbnb) is extremely helpful in getting this set up. following their lead, I'm going to run:
```
(
  export PKG=eslint-config-airbnb;
  npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
)
```
which will install all my depend