---
layout: default
title: 設定
---
Node-REDを設定するために以下のプロパティが利用できます。

スタンドアローンのアプリケーションとして実行すると、これらのプロパティは`settings.js`ファイルから読み込まれます。このファイルの場所は以下の順に決定されます。

 - 起動時に`--settings|-s`オプションで指定した場所
 - 起動時に`--userDir|-u`オプションで指定されたディレクトリ
 - デフォルトのユーザディレクトリ`$HOME/.node-red/settings.js`
 - Node-REDがインストールされたディレクトリ

Node-REDにはユーザが指定する`settings.js`が存在しない場合に使用するデフォルトの`settings.js`ファイルが含まれています。また、独自の`settings.js`の雛形として使用できます。[GitHub上](https://github.com/node-red/node-red/blob/master/settings.js)でも確認できます。

[既存アプリケーションへの組込み](embedding.html)の場合、`settings`プロパティは`RED.init()`の呼び出しでNode-REDへ渡されます。ただ、このモードで実行すると特定のプロパティは無視されます。

### Node-RED実行時の設定

flowFile
: Flow設定をファイルに保存する場合のファイル名です。デフォルト: `flows_<hostname>.json`

userDir
: Flow設定をファイルに保存する場合のファイルへのPathです。デフォルト: `$HOME/.node-red`

nodesDir
: 自作のNodeなどを追加するディレクトリを指定できます。ここで指定したディレクトリ内にNodeの`.js`と`.html`ファイルを配置するとNode-REDのパレットに表示されるようになります。Node-REDの外部のPathも指定できます。デフォルト: `$HOME/.node-red/nodes`

uiHost
: ホストです。デフォルト: `0.0.0.0` -
  *all IPv4 interfaces*.

  *Standalone only*.

uiPort
: ポートです。デフォルト: `1880`.

  *Standalone only*.

httpAdminRoot
: Flow EditorのPathを指定できます。ちなみに`false`を指定するとEditorが無効になります。デフォルト: `/`

httpAdminAuth
: *Deprecated*: see `adminAuth`.

  Flow EditorにBasic認証を設けます。以下が設定例。

      httpAdminAuth: {user:"nol", pass:"5f4dcc3b5aa765d61d8327deb882cf99"}

  `pass`はMD5ハッシュを指定します。上記の例では実際にBasic認証ダイアログへ入力するパスワードは`password`という文字列になるということです。コンソールなどで以下のnodeコマンドを実行することでハッシュ値を得られます。

      node -e "console.log(require('crypto').createHash('md5').update('YOUR PASSWORD HERE','utf8').digest('hex'))"

  *Standalone only*.

httpNodeRoot
: HTTP In NodeのHTTPエンドポイントのPathを指定できます。ちなみに`false`Editorが無効になります。デフォルト: `/`

httpNodeAuth
: HTTP In NodeのHTTPエンドポイントにBasic認証を設けます。設定方法は`httpAdminAuth`と同様です。

httpRoot
: `httpRoot`を指定すると`httpAdminRoot`と`httpNodeRoot`を同じPathで上書きします。

https
: HTTPSを有効にします。以下が設定例。詳細は[here](http://nodejs.org/api/https.html#https_https_createserver_options_requestlistener)。

      https: {
        key: fs.readFileSync('privatekey.pem'),
        cert: fs.readFileSync('certificate.pem')
      },

  *Standalone only*.

disableEditor
: `true`を指定するとFlow Editorが無効になります。`httpAdminRoot`を`false`にした場合はUIもAPIも無効になりますが、こちらはUIのみ無効になります。デフォルト: `false`

httpStatic
: ここで指定したPathを静的なWebコンテンツとして表示できます。例えば`/home/username/.node-red/`と指定した場合、`/home/username/.node-red/index.html`を作成すると`/`へアクセスすると作成した`index.html`が表示されます。したがって`httpStatic`を指定する場合は`httpAdminRoot`は`/`より下層のPathにする必要があります。

  *Standalone only*.

httpStaticAuth
: `httpStatic`にBasic認証を設けます。設定方法は`httpAdminAuth`と同様です。

httpNodeCors
: HTTP In NodeのHTTPエンドポイントのCross-Origin Resource Sharing (CORS) を有効にします。以下が設定例。詳細は[here](https://github.com/troygoode/node-cors#configuration-options)

      httpNodeCors: {
        origin: "*",
        methods: "GET,PUT,POST,DELETE"
      },

httpNodeMiddleware
: HTTP In NodeのHTTPエンドポイントにHTTPミドルウェアを追加できます。HTTPエンドポイントでセッションとクッキーのハンドラを利用したい場合は以下のように設定します。ミドルウェア機能の形式は[こちら](http://expressjs.com/guide/using-middleware.html#middleware.application)で文書化されています。

      httpNodeMiddleware: function(req,res,next) {
        session({
          store: new MongoStore({
            url: process.env.MONGOLAB_URI
          })
        })(req, res, function() {
          cookieParser()(req, res, next);
        });
      }

logging
: コンソールロギングがサポートされています。以下のオプションでロギングの様々なレベルを指定することができます。

 - **fatal** - アプリケーションが使用できなくなるほどの致命的エラーのみを記録
 - **error** - エラー + 致命的エラーを記録
 - **warn** - 致命的でない問題の警告 + エラー + 致命的エラーを記録
 - **info** - アプリケーションの一般的な実行に関する情報 + 警告 + エラー + 致命的エラーを記録
 - **debug** - 更に詳細な情報 + 情報 + 警告 + エラー + 致命的エラーを記録
 - **trace** - 非常に詳細な情報 + 更に詳細な情報 + 情報 + 警告 + エラー + 致命的エラーを記録

デフォルトのレベルは `info` です。限られたフラッシュストレージの組み込みデバイスではディスクへの書き込みを最小限にするために `fatal` を設定することもできます。

### Flow Editorの設定

adminAuth
: Flow Editorにログインフォームを設けます。以下が設定例。

      adminAuth: {
        type: "credentials",
        users: [{
          username: "admin",
          password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
          permissions: "*"
        }]
      }

- `users`リストで複数のユーザを設定できます
- パスワードはbcryptアルゴリズムによるハッシュ値を指定します
- `permissions`でユーザ毎に`*`（フルアクセス）か`read`（read only）権限が付与できます

      node -e "console.log(require('bcryptjs').hashSync(process.argv[1], 8));" YOUR PASSWORD HERE

paletteCategories
: Flow Editorのパレットのカテゴリの順序を設定します。カテゴリがリストにない場合はパレットの最後に追加されます。デフォルトの順序は以下。

      ['subflows', 'input', 'output', 'function', 'social', 'storage', 'analysis', 'advanced'],

   _Note_: Sub Flowを作成するまでSub Flowカテゴリは空のままでパレットには表示されません。

### Editorテーマ

Editorのテーマは次の設定オブジェクトを使用して変更することができます。すべての項目はオプションです。

    editorTheme: {
        page: {
            title: "Node-RED",
            favicon: "/absolute/path/to/theme/icon",
            css: "/absolute/path/to/custom/css/file"
        },
        header: {
            title: "Node-RED",
            image: "/absolute/path/to/header/image", // or null to remove image
            url: "http://nodered.org" // optional url to make the header text/image a link to this url
        },
        deployButton: {
            type:"simple",
            label:"Save",
            icon: "/absolute/path/to/deploy/button/image" // or null to remove image
        },
        menu: { // Hide unwanted menu items by id. see editor/js/main.js:loadEditor for complete list
            "menu-item-import-library": false,
            "menu-item-export-library": false,
            "menu-item-keyboard-shortcuts": false,
            "menu-item-help": {
                label: "Alternative Help Link Text",
                url: "http://example.com"
            }
        },
        userMenu: false, // Hide the user-menu even if adminAuth is enabled
        login: {
            image: "/absolute/path/to/login/page/big/image" // a 256x256 image
        }
    },

### ダッシュボード

ui
: Node-RED-Dashboard のホームPathです。 これは **httpNodeRoot** の相対パスとなります。

    ui : { path: "mydashboard" },

### Nodeの設定

独自のNode Typeはファイルで提供される独自の設定を定義することができます。

functionGlobalContext
: Function Nodeの中では他のライブラリを`require`できませんが`functionGlobalContext`であらかじめ`require`しておくことで`context.global`からライブラリにアクセスできるようになります。以下が設定例。

      functionGlobalContext: { osModule:require('os') }

  上記のように設定してFunction Nodeで以下のようにしてDebug Nodeに吐くと`os`オブジェクトが格納されていることがわかります。ただしライブラリが`npm`でグローバルにインストールされているか、`settings.js`より下層にインストールされていなければいけません。

      var myos = global.get('osModule');

<div class="doc-callout"><em>Note</em>: Node-RED v0.13以前のドキュメントではグローバルコンテキストにアクセスする方法はサブプロパティを参照する方法でした。
<pre>
context.global.foo = "bar";
var osModule = context.global.osModule;
</pre>
この方法はまだサポートされていますが、<code> global.get </code> / <code> global.set </code>でのアクセスが推奨されます。これは将来のリリースでコンテキストデータが永続化できるようになるためです。</div>

debugMaxLength
: Debug NodeでDebugに表示する文字数を指定します。Debugに表示する文字が`...`で途切れてしまう場合はここを増やします。デフォルト: 1000

mqttReconnectTime
: MQTT Nodeの接続が切断された場合の再接続までの待機時間。デフォルト: 5000ミリ秒（5秒）

serialReconnectTime
: Serial Nodeの接続が切断された場合の再接続までの待機時間。デフォルト: 5000ミリ秒（5秒）

socketReconnectTime
: TCP Nodeの接続が切断された場合の再接続までの待機時間。
  デフォルト: 10000ミリ秒（10秒）

socketTimeout
: TCP Nodeのタイムアウト時間。デフォルト: 120000ミリ秒（120秒）
