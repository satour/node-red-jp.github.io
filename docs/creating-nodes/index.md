---
layout: default
title: Nodeの作成
---

Node-REDを拡張する主な方法は、パレットに新しいNodeを追加することです。

*このガイドはまだ記載途中です - フィードバックは[メーリングリスト](https://groups.google.com/forum/#!forum/node-red)までおねがいします。*

以下のセクションについては、だいたい完成しています:

 - [はじめてのNode作成](first-node)
 - [JavaScriptファイル](node-js)
 - [HTMLファイル](node-html)
 - [Nodeコンテキスト](context)
 - [Nodeプロパティ](properties)
 - [Nodeの見た目](appearance)
 - [Nodeステータス](status)
 - [設定を設定](config-nodes)
 - [パッケージング](packaging)
 
これからすべきこと(To Do):

1. ライブラリ
2. カスタムHTTPエンドポイント

### 全体のガイダンス

新しいNodeを作成する時には、いくつかの一般的な原則に従う必要があります。これらは標準Nodeで採用したアプローチを反映していて、一貫したユーザー体験を提供するのに役立ちます。

すべきこと:

- **目的が明確に定義されていること**

   APIに含まれるすべての項目を設定できるようにしたNodeは、単一目的のために分割した複数のNodeよりも使いやすくないことが多いです。

- **元の機能に関係なく簡単に使えること**

   複雑さを隠蔽して専門用語やドメイン固有の知識の使用を避けます。

- **多様なメッセージ型が渡されても正常に処理できること**

   メッセージは文字列、数値、Boolean、Buffer、オブジェクト、配列、nullなどの様々な型で渡される可能性があります。いずれの型であっても正しく処理される必要があります。

- **送信される内容に一貫性を持つこと**

   Nodeはメッセージにどのようなプロパティを追加するのかを文書化する必要があり、動作において一貫性があり、予測可能でなければなりません。

- **Flowの先頭、中間、または末尾に位置し - すべてを一度にしようとしないこと**

- **エラーをキャッチすること**

   Nodeがキャッチできないエラーをスローすると、Node-REDはシステム全体の状態がわからなくなるため停止します。
   Nodeはできるかぎり、エラーをキャッチするか非同期呼び出しのためのエラーハンドラを登録しなければなりません。
