---
title: "【React】余白はmarginではなくSpacerコンポーネントを使おう"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "css"]
published: false
---

# 概要

React で余白を実装する際に、 CSS の margin か Spacer コンポーネントのどちらを適用すべきかで悩んだので調査しました。

# 結論から

**余白は margin ではなく、Spacer コンポーネントで実装する。**

これからその理由を説明していきます。

# 余白を margin でスタイリングすることの問題点

**コンポーネントの独立性を損ない、再利用性が低下してしまうため**です。

例えば、以下のようなコンポーネントがあったとします。

```react
const ComponentA = () => {
  return <div style="margin: 10px">...</div>;
};
```

このコンポーネントには、以下の２つの問題があると思っています。

問題 ①

- 余白がコンポーネントに依存しているため、余白の幅を変更できない(拡張性 ↘︎)。

問題 ②

- コンポーネントに余白が内包されていると、余白の有無をコンポーネント内部まで見る必要があり、デバッグしづらくなる(可読性 ↘︎)。

問題 ① は再利用される汎用コンポーネントで顕著になります。

```react: 問題 ①
// PageAではComponentAのmarginを15pxにしたい...
const PageA = () => {
  return (
    <>
      <OtherComponent />
      <ComponentA />
      <OtherComponent />
    </>
  )
}

// PageBではComponentAのmarginを20pxにしたい...
const PageB = () => {
  return (
    <>
      <OtherComponent />
      <ComponentA />
      <OtherComponent />
    </>
  )
}
```

問題 ② はネストされたコンポーネントなどで発生しやすくなります。

```react:問題 ②
const ComponentB = () => {
  return (
    <>    　
      {/* 何らかのWrapepr component */}
      <Hoge>
        {/* 何らかのWrapepr component */}
        <Fuga>
          <ComponentA /> {/* 余白があるかを知るにはCard内部まで見る必要がある */}
        </Fuga>
      </Hoge>
    </>
  )
}
```

解決策としては、余白とコンポーネントの実体を分離し、それぞれ独立したものとして実装できればよさそうです。

そこで、Spacer というコンポーネントの出番です。

# Spacer 実装

余白をコンポーネントの一部ではなく、**コンポーネントそのもの**として扱います。

```react:Spacer.tsx

import React, { FC } from 'react'

type SpacerProps = {
  size: number;
  horizontal?: boolean;
}

export const Spacer: FC<SpacerProps> = ({ size, horizontal }) => {
  return (
    <div
      style={
        horizontal
          ? { width: size, height: 'auto', display: 'inline-block', flexShrink: 0 }
          : { width: 'auto', height: size, flexShrink: 0  }
      }
    />
  )
}

```

Spacer を使うことで、例えば上記のネストされたコンポーネントは以下のようになります。

```react
const ComponentA = () => {
  return <div>...</div>;
};
```

```react
const ComponentB = () => {
  return (
    <>    　
      <Hoge>
        <Fuga>
          <ComponentA />
          <Spacer size={10}/>
          <ComponentA />
          <Spacer size={10}/>
          <ComponentA />
        </Fuga>
      </Hoge>
    </>
  )
}
```

結果、余白とコンポーネントの実体が明確に分離され、コンポーネントが再利用しやすくなりました。

# まとめ

スタイリングは色々な方法があって、どれが最適か悩ましいところだと思いますが、個人的には Spacer を使うことでより明確に余白とコンポーネントを分離することができて良いなと思いました。まだ Spacer を使ったことがない方は是非検討してみてください！

# 参考記事

https://javascript.plainenglish.io/stop-using-margin-use-spacer-component-instead-953d9b2dbacc

https://zenn.dev/seya/articles/09545c7503baa4
