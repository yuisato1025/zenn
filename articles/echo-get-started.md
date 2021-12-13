---
title: "【Go】echoを使ってAPIサーバーを3分で構築してみる"
emoji: "🎉"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go", "echo", "API"]
published: true
---

# 概要

Go の WEB フレームワーク echo を使って API サーバーを 3 分で構築します。

![echo logo image](https://cdn.labstack.com/images/echo-logo.svg =300x)
_High performance, extensible, minimalist Go web framework_

# 構築してみる

## 環境構築

```shell
$ mkdir echo-get-started && cd echo-get-started
$ go mod init echo-get-started                  # モジュール初期化
$ go get github.com/labstack/echo/v4
$ go get github.com/labstack/echo/v4/middleware # middlewareも必要なのでインストール
```

## 実装

```go:server.go
package main

import (
	"net/http"

	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
)

func main() {
  // インスタンスを作成
  e := echo.New()

  // ミドルウェアを設定
  e.Use(middleware.Logger())
  e.Use(middleware.Recover())

  // ルートを設定
  e.GET("/", hello) // ローカル環境の場合、http://localhost:1323/ にGETアクセスされるとhelloハンドラーを実行する

  // サーバーをポート番号1323で起動
  e.Logger.Fatal(e.Start(":1323"))
}

// ハンドラーを定義
func hello(c echo.Context) error {
  return c.String(http.StatusOK, "Hello, World!")
}
```

## 実行

```shell:/echo-get-started
$ go run server.go
```

以下のような画面がターミナルで表示されたら、サーバーは起動しています。
![running echo server image](/images/running-echo-server.png)

ブラウザで http://localhost:1323/ にアクセスすると、
![echo return hello world image](/images/echo-return-helloworld.png)

「Hello, World!」が表示され、API サーバーとの通信が確認できました！

## まとめ

初めて Go のフレームワークを使ってみましたが、echo は最低限の実装で動き、とてもシンプルで直感的に理解しやすいフレームワークだなと思いました。
次回は Context や CRUD な API など、今回触れられなかった部分を深掘りしていきたいと思います。

### 参考

[echo 公式](https://echo.labstack.com/)
[echo Github](https://github.com/labstack/echo)
