
## ELI5

**Babel** -  If you like to use a new feature of javascript that some browsers may not support, this is where babel comes in. Babel transpiles js code into browser(or for other envs) compatible code. For example, arrow functions are neat and a treat for devs to use, so to get solid browser support, we can use babel and a plugin called `plugin-transform-arrow-functions` (really long name). That brings us to an important concept in babel, it works with plugins all. the. time.

Install the babel cli dev dependency and try the following:

```arrow.js
let foo = () => {
  console.log("Hello World!")
}
```

```bash
$ npm install --save-dev @babel/core @babel/cli
$ npm install --save-dev @babel/plugin-transform-arrow-functions
$ ./node_modules/.bin/babel --plugins @babel/plugin-transform-arrow-functions arrow.js 
```

outputs:

```
var foo = function foo() {
  console.log("Hello World!");
};
```

Features like promises, react jsx,  typescript, even spread operator each has its own plugins. Babel also has the concept of `presets`, since plugins get outdated quickly, presets enable us to target conditions instead of features. For example, we can broading specify the year of features like `babel-preset-es2015` or `babel-preset-react`

`babel-preset-env`  is a smart preset that use latest js without needing to micromanage which syntax transforms are needed by the target environment. As browser support becomes better, and features get supported natively, those transformations are omitted

`@babel/preset-env` takes any [target environments you've specified](https://babeljs.io/docs/en/babel-preset-env#targets) and checks them against its mappings to compile a list of plugins and passes it to Babel.

All babel configuration can be added to `.babelrc` or `package.json` file. For example, 

```json
#package.json

...
"babel" : {
  "presets": ["env"]
}
...

```

When using vue, the `build` command mapped to  `vue-cli-service build` will take care of applying babel presets and plugin transformations. In other cases like when using webpack, the bundler would generally pick up the `.babelrc` config to apply the transformations.

**Lint** - A linter is a tool that takes a look at your code and looks for problems. Things like confirming to style guides or referencing variables that dont exist. 

**ESLint** - static analysis tool that parses your code and looks for issues based on a set of **rules**. We can decide upon the rules by adding them to `.eslintrc` file in our project and we can then run:

```bash
$ npm install --save-dev eslint
$ node_modules/.bin/eslint --init # initializes a .eslintrc file in project
# answer few questions and add rules
# add "lint" script to package.json


```

Any rule violation will be reported in a list.

ESLint can be triggered by the following ways:

1. IDE Plugin. There is ESLint plugins for VSCode for example
2. CI/CD systems and test it against pull requests to make sure code is consistent
3. GitHooks: like pre-commit, do linting and commit only if all pass?

Rules are listed in ESLint rules websites and guard against common programmer errors, like not allowing unrechable code, make sure the team adheres to a style guide, tabs or spaces.

ESLint can also fix some of the problems it finds. This can be done using `--fix` 

With a decent sized codebase the linter would find many issues, and it might overwhelm the developer or make them not care about fixing the issues. To avoid this, we can use `lint-staged` package as dev dependency, which will run your linter only on files that changes after the last commit. This will help you make your package into ship shape gradually.

Other linting tools:
1. JSHint - not actively maintainer
2. Prettier - focused more on styling than code quality
3. StandardJS - more generic and is about all JS. developer with a take them or leave them attitude, since there is no configuration (like no .eslintrc)
4. Clinton - similar in concept to eslint
5. TSLint - ESLint like for someone working in TS


webpack - builder and packager

**Polyfills** - (or PolyFiller) is a piece of code (or plugin) that provides the technology that the developer expects the browser to provide natively.  

**SASS** - CSS preprocessor that has many features not available in CSS. Some of the features are:

1. split styles across files and use `@import`. For example, in a file `style.scss` we can do `@import ./variables` to import a file `_variables.scss`. Note the underscore
2. use variables, say `$primaryColor: rgb(100,100,100);` and use it anywhere, like `color: $primaryColor`
3. We can define a mixin and insert the mixin code anywhere in the stylesheet. 
```
@mixin mixinName {
  display: flex;
  justify-content: center;
}

header {
  @include mixinName();
  color: $primaryColor
}
```

Note the paranthesis when calling mixin. This is because a mixin works like a function and infact we can even pass variables into it

```
@mixin mixinName($direction) {
  display: flex;
  justify-content: center;
  flex-direction: $direction;
}
```

4. We also have `@if $variable {}, @else {}, @else if $variable {}` if you are into that stuff
5. We can use `@extend` to copy the contents of a block in another block

```
.content {
  @extend header;
  color: $secondaryColor;
}
```
6. block inside block. In CSS to style a item inside a container we will do something like
```
# CSS
header {...}
header .box {...}

# SASS
header {
...
.box {
  ...
}
}
```
7. We can use + - * / operator in our sheets

`height: 100% - 20%;` is valid

8. We even have loops. Take a look at `@each` 

```
$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}

```

that takes a list of items and processes them.