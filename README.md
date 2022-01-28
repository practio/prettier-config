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

If you'd like your editor to automatically run Prettier for you, then download the Prettier extenstion for your editor. It's recommended that you enable the "format on save" option in your editor.

For vscode the plugin is found here https://marketplace.visualstudio.com/items?itemName=Prettier.prettier-vscode and you can add `"editor.formatOnSave": true` to your settings.json file to enable autosaving.

## Adding automatic formatting

First start by ensuring you have completed the steps in the **Usage** section of this readme before you continue.

Then install the modules needed for the formatting script and commit hook:

```bash
$ npm i -D prettier husky lint-staged
```

Then add the following scripts to the package.json file of your project (notice that the `format` script is also calling a `eslint:fix` script, see the [eslint-config repo](https://github.com/practio/eslint-config-practio) on how to add it):

```jsonc
{
  "scripts": {
    // ...
    "prettier:write": "prettier --loglevel warn --write \"**/*.@(js|jsx|ts|mjs|json|css|scss|less|html|htm|md|yml|yaml)\"",
    "format": "npm run eslint:fix && npm run prettier:write"
  }
}
```

Install [lint-staged](https://www.npmjs.com/package/lint-staged) as a dev dependency and add the following entry to the root of the package.json file:

```jsonc
{
  "lint-staged": {
    "*.@(js|jsx|ts|mjs|json|css|scss|less|html|htm|md|yml|yaml)": [
      "prettier --loglevel warn --write"
    ]
  }
}
```

Next install [husky](https://typicode.github.io/husky/#/?id=install) as a dev dependency and add a pre-commit hook by using the `npx husky add` command (see their docs). In the `pre-commit` file inside the `.husky` folder add the following line `npx --no-install lint-staged`.

You have now added a `format` script that can be executed in order to format the whole repository (for repositories that are merged with ready builds on [Circle-CI](https://app.circleci.com/projects/project-dashboard/github/practio/), the merge script of [ci-merge](https://github.com/practio/ci-merge) tries to run the script `format` if one is defined in package.json).

You have also added a commit hook that ensures that all files that you make changes to will be formatted when they are staged with git.

If you need the formatting to ignore some specific folders (for example coverage, build or dist folders) then add a `.prettierignore` and a `.eslintignore` file to the root of the repository and add the globs that needs to be ignored to both files (it uses [gitignore syntax](https://git-scm.com/docs/gitignore#_pattern_format)).

That's it. Next time you make changes to your code, it will be formatted automatically as well.
