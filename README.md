# @practio/prettier-config

Practio's global Prettier config.

## Usage

In order to use this config in your project do the following:

Install this module:

```bash
$ npm i --dev @practio/prettier-config
```

After that you need to add the following config to the package.json file of your project:

```json
{
  // ...
  "prettier": "@practio/prettier-config"
}
```

## Usage with your editor

If you'd like your editor to automatically run Prettier for you, then download the Prettier extenstion for your editor. 

**Important!** Make sure to enable the eslint integration option (prettier-eslint) of the extension to make sure that our eslint rules are respected.