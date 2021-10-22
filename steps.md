## steps to configure a React development environment

If you would like to skip this part just use the command
` npx create-react-app [your cool directory name]`
just replace '[your cool directory name]' with whatever you want to call your app.

### Install Node and intialize an npm directory.

1. install node/npm.
2. create a folder called src in your project root.
3. run `npm init -y` in the root of your project (one level above your src folder).

### Install and configure prettier

4. run `npm install -D prettier` to install prettier (-D marks it as a dev dependency).
5. create a file in the root of your project called `.prettierrc`.
6. inside `.prettierrc` place an empty object `{}`. (this will tell prettier we just want to use the default setting)
7. inside your `package.json` delete the test script and create a script called

```json
"format": "prettier --write \"src/**/*.{js,jsx}\""
```

    - if you're using VS code you can skip this step if you want we will configure VS Code to auto format using prettier next.

8. install the prettier code formatter extention for VS Code.
9. go to your vs code settings search "format on save" and make sure the checkbox is checked.
10. while in settings search "prettier config" and check the box that enabales "require a prettier configuration file to format. (this will make sure prettier doesn't run in projects where you arn't using it).

### Install and Configure ESLint

11. run `npm install -D eslint@7.18.0 eslint-config-prettier@8.1.0`
12. create a file next to `.prettierrc` called `.eslintrc.json`. (this will be the config file for eslint).
13. enter the following in your .eslintrc.json

```json
{
  //the order of the "extends" array matters.
  "extends": [
    "eslint:recommended", //loads all of the eslint:recommended rules including white space rules.
    "prettier" //turns off the eslint white space rules and other rules it knows how to handle specifically.
  ],
  "plugins": [], //no plugins for now so leave a blank array.
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
  }
}
```

14. add the following script to your package.json

```json
"lint": "eslint \"src/**/*.{js,jsx}\" --quiet"
```

15. install the eslint VS code extention.

### Configure git

16. in the root of your project run `git init`.
17. in the root of your project create a file called `.gitignore`.
18. inside the .gitignore file type

```
node_modules/
.DS_Store
.cache/
dist/
.env
coverage/
.vscode/
.parcel-cache
```

save .gitignore.

### Install Parcel

19. run ``npm install -D parcel`
20. add the following line to your "scripts" in package.json

```json
"dev": "parcel src/index.html",
```

21. run `npm install react@17.0.1 react-dom@17.0.1`

### Install babel and configure with eslint

22. create a file in the root of your project called `.babelrc`.
23. Inside the .babelrc enter

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

24. Run `npm install -D @babel/core@7.12.16 @babel/preset-react@7.12.13` to install babel and the preset we just set in the .babelrc
25. In package.json add

```json
{
  "browserslist": [
    "last 2 chrome versions"
    //for more more info about this go to https://browserslist.dev/?q=bGFzdCAyIHZlcnNpb25z
  ]
}
```

## Configure ESLint to understand React

26. Out of the box eslint doesn't understand react so we need to add a few configurations to fix that.
27. run `npm install -D eslint-plugin-import@2.22.1 eslint-plugin-jsx-a11y@6.4.1 eslint-plugin-react@7.22.0`

28. Add the following to your "extends" array in .eslintrc.json

```json
{
  //the order of the "extends" array matters.
  "extends": [
    "eslint:recommended", //loads all of the eslint:recommended rules including white space rules.

    //NEW THING
    "plugin:import/errors", //https://www.npmjs.com/package/eslint-plugin-import
    "plugin:react/recommended", //https://www.npmjs.com/package/eslint-plugin-react
    "plugin:jsx-a11y/recommended", //https://www.npmjs.com/package/eslint-plugin-jsx-a11y
    //END NEW THING

    "prettier" //turns off the eslint white space rules and other rules it knows how to handle specifically.
  ],
  "plugins": [], //no plugins for now so leave a blank array.
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
  }
}
```

29. Now we are going to add a "rules" key to our .eslintrc.json to tell eslint what rules we want to turn OFF.

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

  //NEW THING************************************************************
  "rules": {
    "react/prop-types": 0, //turns off props types '0' means turn off
    "react/react-in-jsx-scope": 0 //turns off requiring to import react in every file
  },
  //END NEW THING************************************************************

  "plugins": [], //no plugins for now so leave a blank array.
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
  }
}
```

30. Now we are going to add a plugins to our .eslintrc.json configuration

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

  //NEW THING************************************
  "plugins": ["react", "jsx-a11y", "import"],
  //END NEW THING********************************

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
  }
}
```

31. Lastly, we are going to add a settings key to our .eslintrc.json

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

  //NEW THING************************************
  "settings": {
    "react": {
      "version": "detect" //look at the package.json and figure it out
    }
  }
  //END NEW THING********************************
}
```

32. everything should work now...hopefully, If not check for spelling errors :)
