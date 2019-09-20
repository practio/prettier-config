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

## Adding automatic formatting

First start by ensuring you have completed the steps in the **Usage** section of this readme before you continue.

Then install the modules needed for the formatting script and commit hook:

```bash
$ npm i -D prettier prettier-eslint-cli husky lint-staged
```

Then add the following script to the package.json file of your project:

```jsonc
{
  "scripts": {
    // ...
    "format": "prettier-eslint --write \"**/*.@(js|ts|mjs|json|css|scss|less|html|htm|md|yml|yaml)\""
  }
}
```

and add the following two entries to the root of the package.json file:

```jsonc
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.@(js|ts|mjs|json|css|scss|less|html|htm|md|yml|yaml)": ["prettier-eslint --write", "git add"]
  }
}
```

You have now added a `format` script that can be executed in order to format the whole repository (for repositories that are merged with ready builds on [Teamcity](https://build.practio.com), the merge script of [ci-merge](https://github.com/practio/ci-merge) tries to run the script `format` if one is defined in package.json).

You have also added a commit hook that ensures that all files that you make changes to will be formatted when they are staged with git.

NB! A limitation here is that any file statring with a dot will **not** be formatted by prettier (hopefully in the future [prettier-eslint-cli](https://github.com/prettier/prettier-eslint-cli) will allow us to control the `dot` option of the [glob](https://github.com/isaacs/node-glob) module that it is using internally).

If you need the formatting to ignore some specific folders (for example coverage, build or dist folders) then add a `.prettierignore` and a `.eslintignore` file to the root of the repository and add the globs that needs to be ignored to both files (it uses [gitignore syntax](https://git-scm.com/docs/gitignore#_pattern_format)).

That's it. Next time you make changes to your code, it will be formatted automatically as well.
