---
title: "【redux toolkit】 createAsyncThunk内で異なるactionをdispatchする方法"
emoji: "🔖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["redux", "reduxtoolkit"]
published: true
---

# はじめに

redux toolkit を使って非同期処理をしたい場合に createAsyncThunk が使えますが、他の action を dispatch したい時にどうするか悩んだので書き記しておきます。

https://redux-toolkit.js.org/api/createAsyncThunk

:::message
公式では、非同期処理には createAsyncThunk ではなく RTK Query というツールを使うことが推奨されています。まだ Redux toolkit を導入していない場合はそちらを積極的に使いましょう！
:::

https://redux-toolkit.js.org/rtk-query/overview

# 結論

createAsyncThunk 関数の第二引数に thunkAPI を定義し、`thunkAPI.dispatch(ACTION)`とすることで他アクションを dispatch できます。

```react
export const updateItem = createAsyncThunk(
  'item/update',
  async (
    item: Item,
    thunkAPI,   // 👈 here
  ) => {
    try {
      await update(item)
      thunkAPI.dispatch(otherActions.notify("updated"))
    } catch (e) {
      throw e
    }
  },
)
```

# thunk API とは

> an object containing all of the parameters that are normally passed to a Redux thunk > function, as well as additional options

と公式には書いてあり、Redux の基本的なフローである`dispatch`・`getState`の他に、`signal`・`rejectWithValue`・`fulfillWithValue`といった redux toolkit 特有のフロー制御をメソッドとして呼ぶことができるようです。

詳しくは[こちら](https://redux-toolkit.js.org/api/createAsyncThunk#payloadcreator)をご覧ください。

## まとめ

以上、createAsyncThunk 内で他の action を dispatch する方法でした！

どなたかのお役に立てれば幸いです 🙌
