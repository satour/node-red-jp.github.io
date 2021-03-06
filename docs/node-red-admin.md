---
layout: default
title: Adminツール
---

[Adminツール](http://npmjs.org/package/node-red-admin)を使用するとリモートのNode-REDインスタンスを管理することができます。

### インストール

Adminツールをグローバルnpmインストールする場合。

    npm install -g node-red-admin

<div class="doc-callout">
<em>Note</em>: <code>sudo</code> はLinux/OS Xでルートユーザ以外のユーザとして実行するときに必要となるコマンドです。Windowsで実行するときは、<a href="https://technet.microsoft.com/en-gb/library/cc947813%28v=ws.10%29.aspx">Administratorとしてコマンドプロンプトから</a><code>sudo</code>コマンドを使わずに実行してください。
</div>

### ターゲットとログイン

Adminツールを使うには、まずアクセスしたいNode-REDインスタンスを指定しなければいけません。デフォルトでは `http://localhost:1880` が指定されています。変更するには `target` コマンドを使用します。

    node-red-admin target http://node-red.example.com/admin

[認証](security.html)が有効な場合は `login` します。

    node-red-admin login

これらのコマンドはターゲットとアクセストークンの情報を格納する `~/.node-red/cli-config.json` というファイルを作成します。

### その他のコマンド

このツールは以下のコマンドを提供します。

 - `list` - インストール済みNodeのリストを表示する
 - `info` - モジュールまたはNodeセットの詳細情報を表示する
 - `enable` - 指定したモジュールまたはNodeセットを有効にする
 - `disable` - 指定したモジュールまたはNodeセットを無効にする
 - `search` - npm公開リポジトリで指定したキーワードに関連するNode-REDモジュールを検索する
 - `install` - npm公開リポジトリからNode-REDモジュールをインストールする
 - `remove` - npm公開リポジトリからインストールしたNode-REDモジュールを削除する
