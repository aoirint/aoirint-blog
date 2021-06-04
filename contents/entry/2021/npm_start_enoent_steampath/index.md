---
canonical_url: ./
title: npm startでError: ENOENT: no such file or directory ... .steampathで起動しないときの対処
date: '2021-06-05 01:30:00'
draft: false
category: Node.js
tags:
  - Node.js
  - npm
---

# npm startでError: ENOENT: no such file or directory ... .steampathで起動しないときの対処

gitでブランチを切り替えて依存関係に差分があるとき、
node_modulesに正しい依存関係がインストールされていないことが
原因らしい。

```
Error: ENOENT: no such file or directory, stat '/home/USER/.steampath'
```

package-lock.jsonがあれば、
package-lock.jsonに基づいて依存関係をインストールする`npm ci`を実行する。

実行時に`node_modules`ディレクトリの中身は削除され、再構築される。

```shell
npm ci
```

- https://docs.npmjs.com/cli/v7/commands/npm-ci
- https://stackoverflow.com/questions/64962960/error-enoent-no-such-file-or-directory-stat-home-dylan-steampath