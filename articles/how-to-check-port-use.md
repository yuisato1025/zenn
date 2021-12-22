---
title: "ポートが使われているか確認する方法"
emoji: "👻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["command", "port", "docker"]
published: true
---

## 概要

Docker で立ち上げた DB に接続できず、なんでか悩んでいたら、Docker が正常に終了されておらず、ポートの LISTEN が残っていたことが原因でした。

備忘録として残しておきます。

## ポートの使用状況を確認する

以下のコマンドで確認ができます。

```shell
$ lsof -i:[ポート番号]
```

他のサービスに使われている又は残っている場合 👇

![lsof image](/images/lsof_port.png)
_以前の docker で立ち上げた DB(postgres)が残っている_

## 対処法

残っているプロセスを Kill すればよいです。

```shell
Kill [プロセスID]
```
