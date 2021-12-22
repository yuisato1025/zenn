---
title: "Dockerで原因不明のエラーが発生したら、ポートを確認してみよう"
emoji: "👻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["command", "port", "docker"]
published: true
---

## 概要

Docker で立ち上げた DB に接続できず、なんでか悩んでいたら、Docker が正常に終了されておらず、ポートの LISTEN が残っていたことが原因でした。

Docker を切断する際に割と残っている印象があるので、もし不明なエラーで Docker が上手く立ち上がらない場合はポートを確認してみましょう、という話です。

少しでも、どなたかのお役に立てれば幸いです。

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
