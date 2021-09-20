---
title: "@nuxt/axiosを使ったAPI呼び出し共通化について"
date: "2021-02-05 00:00:00+0900"
tags: ["axios", "Nuxt"]
showToc: true
TocOpen: false
draft: true
---

## はじめに
Nuxt.jsで@nuxt/axios(axios-module)を使いSSR・Store・コンポーネントなどの
様々なコンテキストに対応するaxiosの共通化に少し癖があったので備忘録として残します。

## なぜ共通化するのか？
- エンドポイントの修正による変更が大変  
あちこちで↓のようなコードが呼ばれていると検索・置換を使って修正するしかない。
```js
this.$axios.$get('https://example.com/api/users')
```
- ヘッダー等に付与する情報を適宜変更したい  
- 可読性が上がる  
axiosの処理を↓のように呼び出せるようになると見やすい
```js
this.$axios.$get('https://example.com/api/users')
// ↓ 変更
api.getUsers()
```
etc...

## 共通化の方法について
調査してみるとプラグイン初期化のタイミングでアクセスできる`$axios`をexportしている例をよく見かけました。  
> Nuxt.jsでaxiosの共通処理を作成し、API呼び出し処理をラップして使用する  
> https://qiita.com/itouuuuuuuuu/items/4132e3b7ddf2cbf02442

axios-moduleのissueにも共通化に関するものがあったので参考に実装してみました。
> Can I use axios-module in non-vue files? #28  
> https://github.com/nuxt-community/axios-module/issues/28#issuecomment-340743659

実際にうまく動いているように思えましたが一つ問題がありました。  
  
リクエストヘッダーにはセッションごとに違うトークンを付与して、
サーバ側で実行されるフック(fetch/asyncData)でリクエストを行う実装を行いました。  
複数のブラウザ（セッションがことなる）で同時にリクエストをしたところ
後からリクエストしたレスポンスで返されるべき情報が返されてしまいました。

この現象について調べてみたところ以下のissueが参考なりました。  
> axios module: axios.setToken where to run? #298  
> https://github.com/nuxt-community/axios-module/issues/298#issuecomment-318165164

どうやら$axiosはグローバルとして定義され同時リクエスト時に共有されるため、
import/exportすることは推奨されていないようでした。

さらに調査して以下の方法にたどり着きました。
> How to import module to external file #101  
> https://github.com/nuxt-community/axios-module/issues/101#issuecomment-365909923

> Organize and decouple your API calls in Nuxt.js  
> https://blog.lichter.io/posts/nuxt-api-call-organization-and-decoupling/

api用の別プラグインを定義してそこに$axiosインスタンスを注入する方法でした。

```js
// api/apiFactory.js
export default $axios => ({
  getUsers() {
    return $axios.$get('https://example.com/api/users')
  },
})
```

```js
// plugins/api.js
import { apiFactory } from '~/api/apiFactory'

export default ({ $axios }, inject) => {
  const api = apiFactory($axios)
  inject("api", api)
}
```

## さいごに
axios-moduleは普通axiosと違いNuxtに統合されたaxiosのため、
import/exportして使うやり方が推奨されないなどの拡張/共通化することに癖がありました。
また、共通化する場合はどのコンテキスト（SSR/CSR/Store/Component）でも呼ばれることを考慮しなければならないので少し大変でした。

