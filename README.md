<h1 align="center">
    dsi-hug/actions
</h1>

<p align="center">
    <br/>
    <a href="https://www.hug.ch">
        <img src="https://cdn.hug.ch/svgs/hug/hug-logo-horizontal.svg" alt="hug-logo" height="54px" />
    </a>
    <br/><br/>
    <i>Reusable Github workflows mainly designed to be used by the <a href="https://github.com/dsi-hug">HUG organization's team</a></i>
    <br/><br/>
</p>

<p align="center">
    <a href="https://github.com/dsi-hug/action/blob/main/CONTRIBUTING.md#-submitting-a-pull-request-pr">
        <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome" /></a>
    <a href="https://github.com/dsi-hug/action/blob/main/LICENSE">
        <img src="https://img.shields.io/badge/license-GPLv3-ff69b4.svg" alt="license GPLv3" /></a>
</p>

<hr/>

The following workflows are available:

#### - Setup
Runs the following steps in order:
```
1. Checkout sources
2. Setup chrome (optional)
3. Setup git
4. Setup node
5. Setup npm
6. Cache .angular and node_modules
7. Install npm dependencies
```

#### - Action
Runs the following steps in order and also apply a matrix strategy to run them on different types of machine and node versions:
```md
1. Run the setup workflow
2. Lint (optional)
3. Test (optional)
4. Build (optional)
5. Release dry-run (optional)
6. Release (optional)
```

## Usage

### [dsi-hug/actions/setup](./setup/action.yml)
```yaml
jobs.<job_id>.steps[*]:
  - name: Setup
    uses: dsi-hug/actions/setup@v2
    with:
      #
      # Node version to be used.
      #
      # @examples: 18, '12.x', '10.15.1', '>=10.15.0', 'lts/Hydrogen', '16-nightly', 'latest', 'node'
      # @default: 20
      #
      node-version: 20

      #
      # Whether to install the latest Chrome version.
      #
      # @default: false
      #
      setup-chrome: false
```

### [dsi-hug/actions/.github/workflows/action.yml](./.github/workflows/action.yml)
```yaml
jobs.<job_id>:
  uses: dsi-hug/actions/.github/workflows/action.yml@v2
  with:
    #
    # The working directory of where to run the commands.
    #
    # @default: '.'
    #
    working-directory: '.'

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
    # Whether to install the latest Chrome version.
    #
    # @default: false
    #
    setup-chrome: false

    #
    # Whether to run the command `npm run lint`.
    #
    # @default: false
    #
    lint: false

    #
    # Whether to run the command `npm run test:ci`.
    #
    # @default: false
    #
    test: false

    #
    # Whether to run the command `npm run build`.
    #
    # @default: false
    #
    build: false

    #
    # Whether to run the command `npm run release`.
    # If required, tokens such as NPM_TOKEN and GH_TOKEN can be passed as secrets.
    #
    # @default: false
    #
    release: false

    #
    # Whether to run the command `npm run release:dry-run`.
    # If required, tokens such as NPM_TOKEN and GH_TOKEN can be passed as secrets.
    #
    # @default: false
    #
    dry-run: false
```

## Examples
1. Setup everything on a project and runs e2e tests.

   ```yaml
   jobs:
     ci_e2e:
       runs-on: ubuntu-latest
       steps:
         - name: Setup
           uses: dsi-hug/actions/setup@v2
           with:
             - setup-chrome: true
         - name: Run e2e tests
           run: ...
   ```

2. Runs `lint` and `test` jobs on a desired project, with specific `platforms` and `node versions`.

   ```yaml
   jobs:
     ci_tests:
       uses: dsi-hug/actions/.github/workflows/action.yml@v2
       with:
         working-directory: projects/package-a
         runs-on: '["ubuntu-latest", "macos-latest", "windows-latest"]'
         node-versions: '[18, 20]',
         lint: true,
         test: true
   ```

3. Runs `lint`, `test`, `build` and `release` jobs on a desired project.

   ```yaml
   jobs:
     ci_release:
       uses: dsi-hug/actions/.github/workflows/action.yml@v2
       secrets:
         GIT_USER_NAME: 'dsi-hug-bot'
         GIT_USER_EMAIL: 'dsi-hug-bot@users.noreply.github.com'
         GH_TOKEN: ${{ secrets.YOUR_GITHUB_TOKEN }}
         NPM_TOKEN: ${{ secrets.YOUR_NPM_TOKEN }}
       with:
         working-directory: projects/package-a
         lint: true,
         test: true,
         release: true
   ```

## Credits

Copyright (C) 2024 [HUG - Hôpitaux Universitaires Genève][dsi-hug]

[![love@hug](https://img.shields.io/badge/@hug-%E2%9D%A4%EF%B8%8Flove-magenta)][dsi-hug]



[dsi-hug]: https://github.com/dsi-hug
