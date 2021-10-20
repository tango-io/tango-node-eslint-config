# tango node eslint config

Tango ESLint rules

## Installation

You'll first need to install ESLint:

```bash
npm install eslint --save-dev
```

Next, install eslint-plugin-json-files:

```bash
npm install eslint-config-tango-node --save-dev
```

*NOTE:* If you installed ESLint globally (using the -g flag) then you must also install eslint-plugin-json-files globally.

## Usage

You'll see several dependencies were installed. Now, create (or update) a `.eslintrc` file with the following content:

```json
{
  'extends': [
    'tango-node'
  ]
}
```
