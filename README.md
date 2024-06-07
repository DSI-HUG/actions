<h1 align="center">
    dsi-hug/action
</h1>

<p align="center">
    <br/>
    <a href="https://www.hug.ch">
        <img src="https://cdn.hug.ch/svgs/hug/hug-logo-horizontal.svg" alt="hug-logo" height="54px" />
    </a>
    <br/><br/>
    <i>Reusable Github workflow mainly designed to be used by the <a href="https://github.com/dsi-hug">HUG organization's team</a></i>
    <br/><br/>
</p>

<p align="center">
    <a href="https://github.com/dsi-hug/action/blob/main/CONTRIBUTING.md#-submitting-a-pull-request-pr">
        <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome" /></a>
    <a href="https://github.com/dsi-hug/action/blob/main/LICENSE">
        <img src="https://img.shields.io/badge/license-GPLv3-ff69b4.svg" alt="license GPLv3" /></a>
</p>

<hr/>

This github action will run the following steps in order:
1. `Checkout sources`
2. `Setup node`
3. `Install latest npm`
4. `Cache .angular and node_modules`
5. `Install dependencies`
6. `Lint` *(optional)*
7. `Test` *(optional)*
8. `Build` *(optional)*
9. `Release dry-run` *(optional)*
10. `Release` *(optional)*

It will also apply a matrix strategy to run them on different types of machine and node versions.

## Usage

See [action.yml](action.yml)
```yaml
- uses: dsi-hug/action/.github/workflows/action.yml@v1
  with:
    #
    # The working directory of where to run the commands.
    #
    # @required
    #
    working-directory: ''

    #
    # Type(s) of machine to run the job on.
    #
    # @see: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#choosing-github-hosted-runners
    # @default: '["ubuntu-latest"]'
    #
    runs-on: '["ubuntu-latest"]'

    #
    # Node version(s) to be used.
    #
    # @examples: '[18, 20]', '[12.x]', '[10.15.1]', '[>=10.15.0]', '[lts/Hydrogen]', '[16-nightly]', '[latest]', '[node]'
    # @default: '[20]'
    #
    node-versions: '[20]'

    #
    # Whether to run the command `npm run lint`.
    #
    # @default: true
    #
    lint: true

    #
    # Whether to run the command `npm run test:ci`.
    #
    # @default: true
    #
    test: true

    #
    # Whether to run the command `npm run build`.
    #
    # @default: false
    #
    build: false

    #
    # Whether to run the command `npm run release`.
    # If required, tokens such as NPM_TOKEN and GITHUB_TOKEN can be passed as secrets.
    #
    # @default: false
    #
    release: false

    #
    # Whether to run the command `npm run release:dry-run`.
    # If required, tokens such as NPM_TOKEN and GITHUB_TOKEN can be passed as secrets.
    #
    # @default: false
    #
    dry-run: false
```

## Examples
1. Runs `lint` and `test` jobs on a desired project, with specific `platforms` and `node versions`.

   ```yaml
   jobs:
     ci_test:
       uses: dsi-hug/action/.github/workflows/action.yml@v1
       with:
         working-directory: projects/package-a
         runs-on: '["ubuntu-latest", "macos-latest", "windows-latest"]'
         node-versions: '[18, 20]'
   ```
2. Runs `lint`, `test`, `build` and `release` jobs on a desired project.

   ```yaml
   jobs:
     ci_release:
       uses: dsi-hug/action/.github/workflows/action.yml@v1
       secrets:
         NPM_TOKEN: ${{ secrets.YOUR_NPM_TOKEN }}
         GITHUB_TOKEN: ${{ secrets.YOUR_GITHUB_TOKEN }}
       with:
         working-directory: projects/package-a
         release: true
   ```

## Credits

Copyright (C) 2024 [HUG - Hôpitaux Universitaires Genève][dsi-hug]

[![love@hug](https://img.shields.io/badge/@hug-%E2%9D%A4%EF%B8%8Flove-magenta)][dsi-hug]



[dsi-hug]: https://github.com/dsi-hug
