---
title: "Reactでブラウザバックを無効化する方法"
emoji: "✍️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "reactrouterdom", "history"]
published: true
---

# 概要

React でブラウザバックを無効化する方法を紹介します。

# 方針

- 直前の履歴に現在の URL を追加する
- popstate イベントでブラウザバックを検知する
- 検知した際、ブラウザバックの実行と同時にページを前に進める処理をしてループさせる

イメージ

![image](/images/browser-back.png)

# 実装

```react
const blockBrowserBack = useCallback(() => {
    window.history.go(1)
}, [])

useEffect(() => {
    // 直前の履歴に現在のページを追加
    window.history.pushState(null, '', window.location.href)

    // 直前の履歴と現在のページのループ
    window.addEventListener('popstate', blockBrowserBack)

    // クリーンアップは忘れない
    return () => {
        window.removeEventListener('popstate', blockBrowserBack)
    }
}, [blockBrowserBack])
```

# 参考記事

https://developer.mozilla.org/ja/docs/Web/API/Window/popstate_event

https://developer.mozilla.org/ja/docs/Web/API/History/pushState
