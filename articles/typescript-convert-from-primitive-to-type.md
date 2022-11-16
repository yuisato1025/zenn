---
title: "Typescriptでプリミティブ型を指定の型に変換する方法"
emoji: "✍️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["typescript"]
published: true
---

# 概要

Typescript でプリミティブ型を指定の型に変換する方法を紹介します。

# 具体例

以下のような オブジェクトリテラルがあったとき、

```react
export const Animal = {
  Dog: 1,
  Cat: 2,
  Fish: 3,
  Dragon: 4,
} as const

export type AnimalType = typeof Animal[keyof typeof Animal]
```

型である AnimalType はプリミティブなユニオン型になります。

```react
type AnimalType = 1 | 2 | 3 | 4
```

上記のような オブジェクトリテラルを引数や props の型として取る場合、クエリパラメータや自動生成した型のように明示的に決められない場面において、しばし型エラーになることがあります。

```react
// type t = number
const t = Number("1")

const isMammals = (type: AnimalType) => {
    return type === Animal.Dog || type === Animal.Cat
}

// 「型 'number' を型 'AnimalType' に割り当てることはできません」という型エラーになる。
isMammals(t)
          -
```

理由は単純で、AnimalType が number の部分集合になってしまうからです。

![image](/images/numset.png)

範囲外の値の可能性があるためエラーになる、ということです。

# 対応

## 対応 1） find メソッドで型を絞る

find メソッドで値の範囲を型に絞ることができます。存在しない場合は undefined が返るため、制御が必要です。

確実にその型の範囲であるなら!で型を絞る、またはデフォルト値を設定する対応をします。

```react
// type t = number
const t = Number("1")

// type animalType = AnimalType
const animalType = Object.values(Animal).find((v) => v === t)!
// または const animalType = Object.values(Animal).find((v) => v === t) ?? Animal.Dog

const isMammals = (type: AnimalType) => {
    return type === Animal.Dog || type === Animal.Cat
}

// [ok] 型エラーにならない
isMammals(animalType)
```

## 対応 2） as でキャストする

as による明示的なキャストで対応します。

確実に値が型の範囲内である場合に有効ですが、対応 1 に比べてより強制的であるため、使う際は例外が起きないか慎重に検討しましょう。

```react
// type t = number
const t = Number("1")

// type animalType = AnimalType
const animalType = t as AnimalType

const isMammals = (type: AnimalType) => {
    return type === Animal.Dog || type === Animal.Cat
}

// [ok] 型エラーにならない
isMammals(animalType)
```

・
・
・

以上になります、ではまた 👋
