ms。ContentId:3fdd690d-4259-4066-8781-360bb0554512
タイトル:Windows のコンテナーでのアプリの実行

#Windows Server のコンテナー内のアプリケーションとの互換性

ホー HB プロセスをテストするためには、この文を追加します。
何かこの一覧にでしょうか。
お知らせが失敗し、環境内で成功すると新機能 [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

##Windows Server のコンテナー内で実行するアプリケーション

Windows Server のコンテナー内で、次のアプリケーションを実行しているようにしました。
これらの結果では、アプリケーションが動作することは保証されません。
唯一の目的では、経験を共有します。

| **名前**| **バージョン**| **動作はしますか。**| **コメント**|
|:-----|:-----|:-----|:-----|
| .NET| 3.5| いいえ| 正常にインストールに失敗します。|
| .NET| 4.6| はい| 基本イメージに含まれる|
| .NET CLR| 5 beta 6| はい| X64 および x86 の両方、|
| アクティブな Python| 3.4.1| はい| |
| Apache CouchDB| 1.6.1| いいえ| |
| Apache HTTPD| 2.4| はい| |
| Apache Tomcat| 8.0.24 x64| はい| |
| ASP.NET| 3.5| いいえ| |
| ASP.NET| 4.5| いいえ| |
| ASP.NET| 5 beta 6| はい| X64 および x86 の両方、|
| アーラン/OTP| 18.0| いいえ| |
| FileZilla FTP サーバー| 0.9| はい| RDP セッションを経由して、コンテナー内にインストールするには|
| プログラミング言語を参照してください。| 1.4.2| はい| |
| インターネット インフォメーション サービス| 10.0| はい| |
| Java| 1.8.0_51| はい| サーバーのバージョンを使用します。クライアントのバージョンが正しくインストールされません。|
| Jetty| 9.3| 部分的に| デモ ベースが失敗した場合に実行しています。|
| MineCraft サーバー| 1.8.5| はい| |
| MongoDB| 3.0.4| はい| |
| MySQL| 5.6.26| はい| |
| NGinx| 1.9.3| はい| |
| Node.js| 0.12.6| 部分的に| 使用 (js) ファイルによる、node を実行します。NPM では、パッケージをダウンロードに失敗します。ノードを対話的に実行については、正しく機能しません。|
| PHP| 5.6.11| 部分的に| Apache では、現在の FastCGI 経由での IIS は機能しません。|
| PostgreSQL| 9.4.4| はい| |
| Python| 3.4.3| はい| |
| R| 3.2.1| いいえ| |
| RabbitMQ| 3.5.x| はい| RDP セッションを経由して、コンテナー内にインストールするには|
| Redis| 2.8.2101| はい| |
| Ruby| 2.2.2| はい| X64 および x86 の両方、|
| Ruby on Rails| 4.2.3| はい| |
| SQLite| 3.8.11.1| はい| |
| SQL Server Express| 2014 LocalDB| いいえ| |
| Sysinternals ツール| *| はい| のみ、GUI を必要としないものをしようとしました。PsExec は、現在のデザインでは機能しません|

##どのような省略可能な Windows の機能をインストールできますか。

次の Windows の省略可能な機能は、インストールすることと確認されていたことです。
この時点ではインストール後の多くは機能しません。

*   証明書の AD
*   ADCS-証明書の機関
*   ファイル サービス
    
    *   FS FileServer
    *   FS VSS-エージェント
*   DirectAccess VPN
*   ルーティング
*   リモート デスクトップのサービス
*   VolumeActivation
*   Web サーバー
*   Web サーバーの web
*   一般的な Http-web
*   Web の既定のドキュメント
*   Web ディレクトリ参照
*   Web Http-エラー
*   Web 静的コンテンツ
*   Web Http のリダイレクト
*   Web DAV 発行
*   Web ヘルス
*   Web Http ログ記録
*   Web のカスタム ログの記録
*   Web ログ-ライブラリ
*   Web の ODBC ログの記録
*   Web 要求のモニター
*   Web パフォーマンス
*   Web の圧縮状態
*   Web の圧縮 Dyn
*   Web セキュリティ
*   Web フィルター処理
*   Web Basic-認証
*   Web CertProvider
*   Web クライアントの認証
*   Web ダイジェストの認証
*   Web 証明書の認証
*   Web IP セキュリティ
*   Web Url の認証
*   Windows-認証 web
*   Web アプリの開発
*   Web AppInit
*   Web CGI
*   Web-ISAPI の拡張
*   ISAPI-フィルターが web
*   Web が含まれています
*   Web Websocket
*   Web の互換性の管理
*   Web メタベース
*   BitLocker
*   EnhancedStorage
*   GPMC
*   分離モード
*   サーバーのメディアの基盤
*   MSMQ DCOM
*   MultiPoint コネクタの機能
*   qWave
*   RDC
*   RSAT 機能ツール BitLocker
*   RSAT クラスタ リング PowerShell
*   RSAT クラスタ リング AutomationServer
*   RSAT クラスタ リング CmdInterface
*   RSAT の影響を受けません-VM-ツール
*   RSAT AD-ツール
*   RSAT AD-PowerShell
*   RSAT 追加
*   RSAT-AD-AdminCenter
*   RSAT 追加ツール
*   RSAT ADLDS
*   Hyper V の PowerShell
*   UpdateServices API
*   RSAT NetworkController
*   Windows Fabric のツール
*   RSAT HostGuardianService
*   FS SMBBW
*   記憶域レプリカ
*   Telnet クライアント
*   が
    
    *   プロセス モデル
    *   構成 Api
*   Windows Server のバックアップ
*   移行

何かこの一覧にでしょうか。
お知らせが失敗し、環境内で成功すると新機能 [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。


