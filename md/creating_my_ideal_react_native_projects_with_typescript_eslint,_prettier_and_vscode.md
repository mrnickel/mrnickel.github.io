---
date: 2019-05-30T20:38:34-04:00
draft: true
title: Creating My Ideal React Native Projects With TypeScript ESLint, Prettier and VSCode
---

With the [announcement of TSLint being deprecated in favour of using ESLint](https://medium.com/palantir/tslint-in-2019-1a144c2317a9) 
I thought it was a good opportunity to set up a template project with my five required features on any React Native project I start. Namely:

1. Writing in TypeScript with proper linting
1. Utilize Prettier to automatically fix code styles
1. Set up VSCode for live linting
1. Set up VSCode to properly format using Prettier config on save
1. Resolve imports from the root instead of relatively (i.e. no `import x from '../../../x';`)

This post is really more for myself as a reminder on what is required to meet the above conditions. As I learn more I'll update this post.

This took me _far_ longer than I had originally anticipated. Although I'm not new to React Native, I am new to some of the more advanced
JavaScript tooling.

I won't go through all of the things that I tried and _didn't_ work, as there were far too many of them and I can't fully recall the number of hoops
I tried to jump through before realizing they were the wrong hoop to begin with. (note to self: I should document my processes _as_ they're happening, 
instead of after the fact).

Let's walk through the process of creating an entirely new React Native TypeScript project, then set up the project to address the 5 requirements outlined
above.

Thankfully the process of adding TypeScript to the RN projects has been greatly simplified. Let's stick with the React Native convention and create a new
project **MyAwesomeProject**

```
$ react-native init MyAwesomeProject --template typescript
```

Done. We're running on TypeScript. Huzzah!

Now let's set up the recommended VSCode extensions so we can verify the linting rules as we do everything:

```
$ cd MyAwesomeProject
$ mkdir .vscode && cd .vscode
$ touch extensions.json
$ touch settings.json
$ cd ..
$ mkdir src
$ code .
```

This will create the .vscode project folder and open up the project.

I prefer to keep my folder structure as follows:

```
MyAwesomeProject
|____tests__/
|__.vscode/
|__android/
|__ios/
|__src/
|  |__components/ // general components shared throughout the codebase
|  |__scenes/ // full screen sections
|  |  App.tsx
|
|  .bablerc.js
|  ...
|  etc
|  ...
```

This will become important when specifying the root paths in `tsconfig.json` and `.babelrc.js`.


Let's update the `extensions.json` file as below:

## ./vscode/extensions.json
```
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode"
  ]
}
```

Now we can make sure that everyone opening this project will use the same default extensions and have the same consistent tooling.

Now it's time to start adding the various dependencies:

Open up the VSCode terminal by pressing `CTRL + ``.

Let's start adding some dependencies:

```
$ yarn add --dev     @typescript-eslint/eslint-plugin \
  @typescript-eslint/parser \
  babel-eslint \
  babel-plugin-module-resolver \
  eslint \
  eslint-config-airbnb \
  eslint-config-prettier \
  eslint-config-react \
  eslint-import-resolver-babel-module \
  eslint-import-resolver-typescript \
  eslint-plugin-import \
  eslint-plugin-jsx-a11y \
  eslint-plugin-module-resolver \
  eslint-plugin-prettier \
  eslint-plugin-react \
  prettier \
  react-native-typescript-transformer \
```

Phew! That's a lot of dependencies. Welcome to JavaScript I guess!

Note: I prefer to use the default Airbnb rules. (side note: Want to learn more about JavaScript eccentricities? Read the [Airbnb JavaScript style guide](https://github.com/airbnb/javascript))

Finally, let's modify a few config files.

## ./vscode/settings.json
```
{
  "search.exclude": {
    "ios/build": true,
    "android/app/build": true,
  },
  "editor.formatOnSave": false,
  "editor.trimAutoWhitespace": true,
  "editor.tabSize": 2,
  "editor.rulers": [
    150
  ],
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    {
      "language": "javascript",
      "autoFix": true
    },
    {
      "language": "javascriptreact",
      "autoFix": true
    },
    {
      "language": "typescript",
      "autoFix": true
    },
    {
      "language": "typescriptreact",
      "autoFix": true
    }
  ],
  "files.exclude": {
    "**/node_modules": true,
    "**/android/app/build": true,
    "**/ios/build": true
  },
  "files.insertFinalNewline": true,
}
```

## .eslintignore
```
src/registerServiceWorker.js
src/**/__tests__/**
babel.config.js
index.js
node_modules/**
ios/**
android/**
```

## .eslint.js
```
module.exports = {
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint",
    "prettier",
    "import",
    "module-resolver"
  ],
  "extends": [
    "airbnb",
    "plugin:@typescript-eslint/recommended",
    "plugin:import/typescript",
    "plugin:react/recommended",
    "plugin:import/recommended",
    "prettier",
    "prettier/@typescript-eslint",
    "prettier/react"
  ],
  "env": {
    "browser": true,
    "jasmine": true,
    "jest": true
  },
  "rules": {
    "prettier/prettier": ["error"],
    "@typescript-eslint/explicit-member-accessibility": 0,
    "@typescript-eslint/no-empty-interface": 0,
    "@typescript-eslint/explicit-function-return-type": 0,
    "@typescript-eslint/no-non-null-assertion": 0,
    "@typescript-eslint/no-use-before-define": 0,
    "no-use-before-define": 0,
    "@typescript-eslint/indent": ["error", 2],
    "max-len": ["error", 150],
    "react/jsx-filename-extension": [1, { "extensions": [".jsx", ".tsx"] }],
    "object-curly-newline": ["error", { "ImportDeclaration": "never" }],
    "@typescript-eslint/interface-name-prefix": 0,
    "class-methods-use-this": 0,
    "no-unused-vars": 0,
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_[0-9]?" }],
    "react/display-name": 0,
    "react/prop-types": 0, // not necessary as we use typescript
    "object-curly-newline": 0,

  },
  "settings": {
    "import/resolver": {
      "babel-module": {},
      "typescript": {}
    }
  },
  "globals": {
    "__DEV__": true
  }
}
```

Note I prefer the js extension so I can add inline comments.

## .prettierrc.js
```
module.exports = {
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 150,
  "bracketSpacing": true
};
```

## .babelrc.js
```
module.exports = {
  presets: [
    'module:metro-react-native-babel-preset'
  ],
  plugins: [
    ['module-resolver',
      {
        extensions: [
          '.js',
          '.jsx',
          '.ts',
          '.tsx'
        ],
        root: ["./src"],
        alias: {
          "resources": "./resources"
        }
      }
    ]
  ]
};
```

Note: I prefer to use the name `.bablerc.js` to stick with the other config file naming conventions.

## tsconfig.json
```
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react",
    "lib": [
      "es6"
    ],
    "module": "es2015",
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext",
    "baseUrl": "./src",
    "rootDir": "./src",
    "resolveJsonModule": true
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

That's about it. Now get started writing your next world-changing app with sane defaults.

I've created a [demo project on GitHub](https://github.com/mrnickel/ReactNativeTypeScriptConfigDemo). If you have any recommendations on how I can make this better please feel free to submit a PR!
