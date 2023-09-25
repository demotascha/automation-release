# automation-release
111
222
333

## Semantic Release


```javascript
//release.config.js
module.exports = {
    branches: ['main'],
    plugins: [
      '@semantic-release/commit-analyzer',
      '@semantic-release/release-notes-generator',
      ['@semantic-release/npm',{npmPublish: false}],
      [
        '@semantic-release/changelog',
        {
          mangle: false,
          headerIds: false,
          changelogFile: 'CHANGELOG.md',
        },
      ],
      [
        '@semantic-release/git',
        {
          assets: ['CHANGELOG.md', 'package.json'],
          message:
            'chore(release): ${nextRelease.version} [skip ci]',
        },
      ],
    ],
  };
```


```javascript
// release.config.js
module.exports = {
    branches: ['main'],
    plugins: [
      '@semantic-release/commit-analyzer',
      '@semantic-release/release-notes-generator',
      ['@semantic-release/npm',{npmPublish: false}],
      [
        '@semantic-release/changelog',
        {
          mangle: false,
          headerIds: false,
          changelogFile: 'CHANGELOG.md',
        },
      ],
      [
        '@semantic-release/git',
        {
          assets: ['CHANGELOG.md', 'package.json'],
          message:
            'chore(release): ${nextRelease.version} [skip ci]',
        },
      ],
        '@semantic-release/github',
    ],
  };
  
```



```yaml
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs

name: Release workflow

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
permissions:
  contents: read # for checkout

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write # to be able to publish a GitHub release
      issues: write # to be able to comment on released issues
      pull-requests: write # to be able to comment on released pull requests
      id-token: write # to enable use of OIDC for npm provenance
    strategy:
      matrix:
        node-version: [18.x]
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm install
      env:
        HUSKY: 0
    - run: npx semantic-release
      env:
        GH_TOKEN: ${{ secrets.GH_TOKEN }}
      name: Release

```
