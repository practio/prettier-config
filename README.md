# @practio/prettier-config

Practio's global [Prettier](https://prettier.io/) config.

## Usage

In order to use this config in your project do the following:

Install this module:

```bash
$ npm i -D @practio/prettier-config
```

**Important!** After that you need to add the following line to the package.json file of your project:

```jsonc
{
  // ...
  "prettier": "@practio/prettier-config"
}
```

## Usage with your editor

If you'd like your editor to automatically run Prettier for you, then download the Prettier extenstion for your editor. It's recommended that you enable the "format on save" option in your editor (for vscode add `"editor.formatOnSave": true` to your settings.json file).

**Important!** Make sure to enable the eslint integration option (prettier-eslint) of the extension to make sure that our eslint rules are respected.

## Adding automatic formatting as part of deploying

The merge script of [ci-merge](https://github.com/practio/ci-merge) used by ready builds on [Teamcity](https://build.practio.com) tries to run the script `format` if one is defined in package.json. So in order to get auto formatting as part of deploys you must define a `format` script.

First start by ensuring you have completed the steps in the **Usage** section of this readme. After, install the modules needed for our format script:

```bash
$ npm i -D prettier prettier-eslint-cli
```

Then add the following script to the package.json file of your project:

```jsonc
{
  "scripts": {
    // ...
    "format": "prettier-eslint --write \"**/*.@(js|ts|mjs|json|css|scss|less|html|htm|md|yml|yaml)\""
  },
}
```

If you need the script to ignore some specific folders then add a `.prettierignore` and a `.eslintignore` file to the root of the repository and add the globs that needs to be ignored to both files (it uses [gitignore syntax](https://git-scm.com/docs/gitignore#_pattern_format)).

That's all. Next time you deploy, your code will be formatted as well.

NB! A limitation here is that any file statring with a dot will not be formatted by prettier here (hopefully in the future [prettier-eslint-cli](https://github.com/prettier/prettier-eslint-cli) will allow us to control the `dot` option of the [glob](https://github.com/isaacs/node-glob) module that it is using internally).

