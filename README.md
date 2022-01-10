# action-setup-node

Composite action to setup a specific version of node for a repository that has an `.npmrc` file that will require 
authentication to the `croesusfin` private node package hosted by Github.

This repository makes use of multiple github action in order to reduce boilerplate code across workflows.

## Why use an .npmrc file instead of simply using the `NODE_AUTH_TOKEN` env variable ?

The `.npmrc` files allow us to specify a registry to a scope. This allows us to only retrieve the `@croesusfin` scoped 
packages from github, and the rest can simply be retrieved from https://registry.npmjs.org/

## Why is this action needed ?

There are certain limitations with private packages hosted on Github with regards to authentication:

- [GITHUB_TOKEN does not have access to other private packages](https://github.com/actions/setup-node/issues/49)

## Examples

### Using the default options will setup node version 14, will update the root .npmrc file found in the repository, and will run "npm install"

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Setup Node.js
        uses: croesusfin/action-setup-node@v1
        with:
          token: ${{secrets.NODE_API_KEY}}
```
---
### Specify another install command

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Setup Node.js
        uses: croesusfin/action-setup-node@v1
        with:
          token: ${{secrets.NODE_API_KEY}}
          install-command: 'npm install --ignore-scripts' # or 'npm ci'
```
---
### Skip the install step, only setup node and update the .npmrc

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Setup Node.js
        uses: croesusfin/action-setup-node@v1
        with:
          token: ${{secrets.NODE_API_KEY}}
          skip-install: true
```
---
### Specify another path for the .npmrc file

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Setup Node.js
        uses: croesusfin/action-setup-node@v1
        with:
          token: ${{secrets.NODE_API_KEY}}
          npmrc-path: src/.npmrc
```
---
### Enable caching

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Setup Node.js
        uses: croesusfin/action-setup-node@v1
        with:
          token: ${{secrets.NODE_API_KEY}}
          cache: true
```
Caching makes use of and configures the `actions/cache@v2` github action. [More info on that can be found here](https://github.com/actions/cache)