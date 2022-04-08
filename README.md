# semantic-release-template

## 用法

### 单包仓库

1. 配置 `package.json`

    ```json
    {
      "scripts": {
        "release": "semantic-release"
      },
      "devDependencies": {
        "@semantic-release/changelog": "^5.0.1",
        "@semantic-release/git": "^9.0.0",
        "semantic-release": "^17.4.1"
      }
    }
    ```

2. 配置 `release.config.js`

    ```js
    module.exports = {
      plugins: [
        '@semantic-release/commit-analyzer',
        '@semantic-release/release-notes-generator',
        [
          '@semantic-release/changelog',
          {
            changelogFile: 'CHANGELOG.md',
          },
        ],
        '@semantic-release/github',
        '@semantic-release/npm',
        [
          '@semantic-release/git',
          {
            assets: ['CHANGELOG.md', 'package.json'],
          },
        ],
      ],
    };
    ```

3. 配置 .npmrc

    ```
    registry=https://registry.npmjs.org/
    access=public
    ```

4. 配置 `.github/workflows`

    ```
    name: publish

    on:
      push:
        branches:    
          - master
          - beta
          - next

    jobs:
      publish_npm:
        name: publish npm
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - name: Use Node.js 16.x
            uses: actions/setup-node@v1
            with:
              node-version: 16.x
          - run: npm ci
          - env:
              GH_TOKEN: ${{ secrets.GH_TOKEN }}
              NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
            run: npm run release
    ```

5. 然后合并代码到 beta、master 等分支自动触发发布

### 多包仓库

1. 配置项目 `package.json`

    ```json
    {
      "name": "xxx",
      "workspaces": [
        "packages/*",
      ],
      "scripts": {
        "release": "multi-semantic-release"
      },
      "devDependencies": {
        "@qiwi/multi-semantic-release": "^3.14.2",
        "@semantic-release/changelog": "^5.0.1",
        "@semantic-release/git": "^9.0.0"
      }
    }
    ```

2. 配置 `release.config.js`

    ```js
    module.exports = {
      plugins: [
        '@semantic-release/commit-analyzer',
        '@semantic-release/release-notes-generator',
        [
          '@semantic-release/changelog',
          {
            changelogFile: 'CHANGELOG.md',
          },
        ],
        '@semantic-release/npm',
        [
          '@semantic-release/git',
          {
            assets: ['CHANGELOG.md', 'package.json'],
          },
        ],
      ],
    };
    ```

3. 配置 .npmrc

    ```
    registry=https://registry.npmjs.org/
    access=public
    ```

4. 配置 `.github/workflows`

    ```
    name: publish

    on:
      push:
        branches:    
          - master
          - beta
          - next

    jobs:
      publish_npm:
        name: publish npm
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - name: Use Node.js 16.x
            uses: actions/setup-node@v1
            with:
              node-version: 16.x
          - run: npm ci
          - env:
              GH_TOKEN: ${{ secrets.GH_TOKEN }}
              NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
            run: npm run release
    ```

5. 然后合并代码到 beta、master 等分支自动触发发布
