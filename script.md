## script for config section

- In this video I'm going to show you how to configure a modern react development environment.

- If you just want to start building you can use the command `npx create-react-app` followed by your project name.

- I wanted to make this video for people who really want to understand how everything is connected.

- I'll try to make this quick because configuring your environment might be the most boring part of modern web development.

- I've included a Steps.md file in the github repository if you need to reference specific commands.

- Everything should work together with the most modern package versions but if you run into errors or issues you can use the version numbers for packages specified in the Steps.md file.

- The only prerequizite for this video is to install node and npm.

- you can test to see if you have these install by typing `node -v` and `npm -v` in the terminal.

- our first step will be to initialize our directory as an npm directory you can do this by typing `npm init -y`.
- the `-y` just tells npm to skip the inititialization questions.
- now we have a `package.json` file in our root directory.
- you can think of `package.json` as a manifest for your project.
- if you want to share your project configuration with anyone all they need is your `package.json` file and to run `npm install` and npm will install everything specified in the package.json.

- next we are going to create a src directory in our project root.
- this will be where we keep the source code for our project.

- It's a good idea to form the habit of using version control even if you arn't planning to upload to github.
- initialize your root directory as a `git` repository by typing `git init` while in the root of your project.
- create a file called `.gitignore` and add the following strings of text to it.

```
# node_modules
node_modules/
.DS_Store
.cache/
dist/
.env
coverage/
.vscode/
.parcel-cache
```

- everything in your `.gitignore` will not be tracked by `git`.

- You almost always want to have `node_modules` in your `.gitignore` it's where npm installs packages but as stated earlier if anyone has your package.json they can install the packages from that, so omitting `node_modules` will reduce your project size considerably when trying to share it.

- .DS_Store is a file hidden in macOS directories. it's not necessary to share and would just clutter up our project root.

- .cache/, dist, and .parcel-cache are all files that will be created by our task runner `parcel` and don't need to be included.

- finally .env will ensure any environment variables we create won't acedentally get shared and .vscode/ will remove any editor specific settings we have enabled locally on our machine.

- now that we have git set up we can install some npm packages that will help with code formatting and linting.

- First we will install prettier with the command `npm install -D prettier`.
- If using VSCode we can install the pritter extention and add some extra settings to make prettier format on save and run only in directories that we want it to.

- search `prettier` in the vsCode extention store it should be the first option.
- go to your vs code settings and search 'prettier config' check the box that will require a config file for prettier to run. This will ensure prettier wont run in projects where we dont want it to.

- while still in settings search 'format on save' check the box that tells vs code to use a file formatter when saving a file.

- since we just told VSCode not to run prettier unless there is a config file lets create one really quick.

- create a file named `.prettierrc` in your project root.

- place an empty javascript object in it. `{}`.

- that's it, since we are just going to be using the default prettier settings there is nothing to put in this file but we still need it because we just told prettier that it needs a config file to run in a directory.

- next let's install eslint and some eslint plugins run the following command

```
npm install -D eslint eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react
```

- eslint-plugin-import enables eslint to check your import statements for errors
- eslint-plugin-jsx-a11y will help you write accesible jsx for screen readers
- finally eslint-plugin-react gives eslint the ability to understand react syntax.

- now lets create a `.eslintrc.json` file in our root directory to connect everything we just installed.

- inside `.eslintrc.json` add the following.

```json
{
  //the order of the "extends" array matters.
  "extends": [
    "eslint:recommended", //loads all of the eslint:recommended rules including white space rules.
    "plugin:import/errors", //https://www.npmjs.com/package/eslint-plugin-import
    "plugin:react/recommended", //https://www.npmjs.com/package/eslint-plugin-react
    "plugin:jsx-a11y/recommended", //https://www.npmjs.com/package/eslint-plugin-jsx-a11y
    "prettier" //turns off the eslint white space rules and other rules it knows how to handle specifically.
  ],
  "rules": {
    "react/prop-types": 0, //turns off props types '0' means turn off
    "react/react-in-jsx-scope": 0 //turns off requiring to import react in every file
  },
  "plugins": ["react", "jsx-a11y", "import"],
  "parserOptions": {
    "ecmaVersion": 2021, //use the most modern version.
    "sourceType": "module", //we will be using ES Modules
    "ecmaFeatures": {
      "jsx": true //we will be using JSX
    }
  },
  "env": {
    //describe the environment this code will run in
    "es6": true, //eslint won't throw error if we try to use es6 methods like map() or reduce().
    "browser": true, //eslint won't throw errors if we use fetch() or the window object
    "node": true //eslint won't throw erroes if we try to access things in a node environement like 'golbal' or 'process'.
  },
  "settings": {
    "react": {
      "version": "detect" //look at the package.json and figure it out
    }
  }
}
```

- the extends array is a list of the rules we would like to apply to our project.
- the order of this array matters so be sure to write it exactly as I have it here.
- after the extends array add a key called "plugins" this array works hand in hand with the extends array and specifies what plugins we are going to use.

- think of the plugins array as three restaurant menus, and whatever we put in the extends array as us ordering certain dishes off of those menus for use in our project.


- next we are going to turn off some of the default rules for react projects

- we won't be using React proptypes and the new version of babel doesn't require we import React in every file so we will turn that off too.

```json
{
  "rules": {
    "react/prop-types": 0, //turns off props types '0' means turn off
    "react/react-in-jsx-scope": 0 //turns off requiring to import react in every file
  }
}
```

- next we need to give eslint some information about our code and the environment it will be running in


```json
  "parserOptions": {
    "ecmaVersion": 2021, //use the most modern version.
    "sourceType": "module", //we will be using ES Modules
    "ecmaFeatures": {
      "jsx": true //we will be using JSX
    }
  },
  "env": {
    //describe the environment this code will run in
    "es6": true, //eslint won't throw error if we try to use es6 methods like map() or reduce().
    "browser": true, //eslint won't throw errors if we use fetch() or the window object
    "node": true //eslint won't throw erroes if we try to access things in a node environement like 'golbal' or 'process'.
  },
```

- lastly, we will add a settings key that will tell eslint to refer to the package.json for our version of react.

- after all of this let's finally install react reactDOM
- run `npm install react react-dom`;

- then lets install parcel `npm install -D parcel`

- we're getting close to the end I promise.

- add a script to your package.json that points parcel to your project index file
  `"dev": "parcel src/index.html"`

- because we are using parcel there is no need to configure babel, but if you arn't using parcel you can install babel with `npm install -D @babel/core a nd @babel/preset-react`

- then add a .babelrc file to the root of your project with the following inside

```json
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

- everything should be configured at this point let's create a hello world react project to test everything.

- inside the `src` directory create 3 files index.html index.js and App.js
- inside index.html just create a div with the id of root and add a script tag pointing to index.js with a type="module" attribute.
- inside index.js

  - import ReactDOM
  - import App
  - attach app to the root element by calling
    `ReactDOM.render(<App />, document.getElementById("root"));`

- inside our App.js we will create a function called App that reutrn a jsx h1 that says `hello react`

- You'll notice here I get an error when I try to run our parcel server with `npm run dev`

- because parcel configures babel for use it's just telling me what's in the .babelrc is redundant. 

- if you're not using parcel having a babelrc can be useful.

- if ythere arn't any typos and you go to localhost:1234 you should see your react app working.

- congratulations you've just configured a modern react develeopment environment from scrach.

- because we are using a live development server the browser will automatically refresh whenever we save our project.

- if you enjoyed this video please give it a thumbs up and for more content like this hit the subscribe button.

- thanks for watching.
