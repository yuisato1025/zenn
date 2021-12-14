---
title: "Github Actions 'Not compatible with your version of node/npm'を解決する"
emoji: "✅"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["github", "githubactions"]
published: true
---

# 概要

Github Actions で`npm install`を実行すると、以下のようなエラーが発生しました。

![npm install error image](/images/npm-install-error.png)

# 原因

Github Actions 上で動く node のバージョンが `package.json`のバージョンと異なるためです。

node の LTS 版(安定板)が 12 月 1 日にマイナーアップデートされていたため、以下のような表記だと自動で最新のバージョンが取得され、package.json のバージョンと異なってしまうことが原因でした。
https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V16.md

**package.json**

```json
{
	"engines": {
		"node": "16.13.0",
		"npm": "8.1.0"
	}
}
```

**Github Actions**

```yml:github-actions.yml
- uses: actions/setup-node@v2
        with:
          node-version: '16.13' # 16.13.xの最新版16.13.1がインストールされる
          registry-url: 'https://npm.pkg.github.com'v
```

# 解決策

node-version をマイナーバージョンまでバージョン指定することで解決できます。

```yml:github-actions.yml
- uses: actions/setup-node@v2
        with:
          node-version: '16.13.0' # マイナーバージョンまで指定
          registry-url: 'https://npm.pkg.github.com'
```
