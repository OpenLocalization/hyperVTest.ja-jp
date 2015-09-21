ms。ContentId:5bbac9eb-c31e-40db-97b1-f33ea59ac3a3
タイトル:進行中の作業

#進行中の作業

ここで、問題を参照してくださいまたは、ご質問はありません、に掲載したり、。 [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

-----------------------

##一般的な機能

###コンテナーの Windows イメージは、コンテナーのホストに正確に一致する必要があります。

Windows Server のコンテナーには、オペレーティング システム イメージを構築して、レベルの修正プログラムを適用する点ではコンテナーのホストに一致する必要があります。
不一致は、コンテナーや、ホストの不安定性や予期しない動作につながります。

Windows のコンテナーに対して更新プログラムをインストールする場合は、OS が一致する更新プログラムをダウンロードするコンテナーの基本 OS イメージを更新する必要がありますをホストします。

**回避をするには。**   
ダウンロードし、コンテナーのホストの OS バージョンとパッチ レベルが一致するコンテナーの基本イメージをインストールします。

###コマンドの散発的失敗--、もう一度やり直してください

テストでコマンドしなければならないを複数回実行します。
その他の操作に、同じ原則が適用されます。
たとえば、新しいファイルを作成することが表示されない場合は、ファイルに手を加えるしてみてください。

これを行う場合は、お知らせを使用して [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

** 回避する: **  
コマンドを何度も試されるように、スクリプトを作成します。
コマンドが失敗した場合、再度実行してください。

###すべて非-c: ドライブが自動的に新しいコンテナーに割り当てられている/

すべて非-c:/、コンテナーのホストで利用できるドライブは、新しい Windows Server コンテナーの実行中に自動的にマップします。

この時点で方法はありませんのコンテナーにフォルダーを選択的にマップするように、中間の解決方法は、ドライブが自動的にマップします。

** 回避する: **  
それに取り組んでいます。
将来のあるフォルダーを共有する場合。

###Windows Server のコンテナーは非常に時間がかかる作業を開始します。

場合は、コンテナーには、30 秒以上を開始するがかかり、多くの重複するウイルス スキャンが実行することがあります。

Windows Defender は、おそらくファイル コンテナー内のイメージが、OS のバイナリ ファイルとファイルのすべてをコンテナーの OS イメージに含めてを不必要にスキャンなどの多くのマルウェア対策ソリューションです。
これは、新しいコンテナーを作成して、以前スキャンされていない新しいファイルのようになります「コンテナーのファイル」のすべてのマルウェアの観点からずっとときに発生します。
したがって、コンテナー内のプロセスが、これらのファイルを読み取るしようとしています。 マルウェア対策コンポーネント回目のスキャンにファイルへのアクセスを許可する前にします。
実際にはこれらのファイルは、コンテナーのイメージは、インポートまたはサーバーに取得したときに既にスキャンされていました。
後で新しいインフラストラクチャのプレビューでは Windows Defender をなど、マルウェア対策ソリューションは、このような状況を認識してを複数のスキャンを回避するのにはそれに応じて動作できるようにの場所になります。

--------------------------

##ネットワーク

###コンテナーごとにネットワーク コンパートメントの数

このリリースでは、コンテナーごとに 1 つのネットワーク コンパートメントをサポートします。
したがって、複数のネットワーク アダプターにコンテナーがある場合は、それぞれのアダプターで、同じネットワーク ポートをアクセスすることはできません (使用や 192.168.0.2:80 など、同じコンテナーに属する)。

** 回避する: **  
複数のエンドポイントは、コンテナーによって公開される必要がある場合、は、NAT のポートのマッピングを使用します。

###Windows のコンテナーには、ip アドレスがされません。

DHCP の VM のスイッチを使用して、windows server のコンテナーに接続している場合は、コンテナーのホストをコンテナー、IP wwhile を受信する可能性があります。

コンテナーは、169.254.*.* を取得します。***.*** APIPA IP アドレスです。

**回避をするには。**
これは、共有、カーネルの副作用です。
すべてのコンテナーには、同じ mac アドレスが実質的があります。

MAC アドレスのコンテナーのホスト上のスプーフィングを有効にします。

これは PowerShell を使用して実現できます。


```
Get-VMNetworkAdapter -VMName "[YourVMNameHere]"  | Set-VMNetworkAdapter -MacAddressSpoofing On

```


###ファイル共有の作成は、コンテナーでは機能しません

現在、コンテナー内のファイル共有を作成することはできません。
実行する場合 `net share` 次のように、エラーが表示されます。


```
The Server service is not started.

Is it OK to start it? (Y/N) [Y]: y
The Server service is starting.
The Server service could not be started.

A service specific error occurred: 2182.

More help is available by typing NET HELPMSG 3547.

```

** 回避する: **
実行して、円形の他の方法を使用するには、コンテナーにファイルをコピーする場合 `net use` 内のコンテナー。
たとえば、次のように入力します。


```
net use S: \\your\sources\here /User:shareuser [yourpassword]

```

--------------------------

##アプリケーションの互換性

あるため、アプリケーションの互換性のある情報を分割することにしました mnay の質問については、アプリケーションが動作し、Windows Server のコンテナーでは機能しません。 [今回は、](../reference/app_compat.md)。

いくつかの最も一般的な問題はこちらからもします。

###WinRM が Windows Server のコンテナー内で起動しません。

WinRM は、開始はのエラーをスローし、もう一度停止します。
エラーは、イベント ログに記録されません。

**回避をするには。**
WMI を使用します。 [RDP](#RemoteDesktopAccessOfContainers)、または入力 PSSession ContainerID

###DISM を使用してコンテナー内の IIS と ASP.NET 4.5 または ASP.NET 3.5 をインストールことはできません。

Windows Server のコンテナー内のコンテナーでの IIS ASPNET45 のインストールは機能しません。
インストールの進行状況スティック 95.5% 程度です。

'' PowerShell
Enable-windowsoptionalfeature-IIS-ASPNET45 のオンライン機能名


```

This fails because ASP.NET 4.5 doesn't run in a container.

**Work Around:**  
Instead, install the Web-Server role to use IIS. ASP 5.0 does work. 

``` PowerShell
Enable-WindowsOptionalFeature -Online -FeatureName Web-Server

```

参照してください、 [アプリケーションの互換性の記事](../reference/app_compat.md) 詳細については、どのようなアプリケーション コンテナー詰めすることができます。

--------------------------

##Docker 管理

###既定でセキュリティ保護されていない docker クライアント

このプレリリース版では、docker 通信は検索する場所がわかっている場合はパブリックです。

###Windows Server のコンテナーでは動作しない docker コマンド

コマンドが失敗します。

| **Docker コマンド**| **これを実行しています。**| **エラー**| **メモ**|
|:-----|:-----|:-----|:-----|
| **docker コミット**| image| Docker がコンテナーの実行を停止し、適切なエラー メッセージが表示されません。| A が停止しているコンテナーをコミットしています。コンテナーを実行するため。:) それを処理します。|
| **docker の相違点**| デーモン| エラー:Windows graphdriver が Changes() をサポートしていません| |
| **docker kill**| コンテナー| エラー:無効な信号。KILL エラー: コンテナーを削除できませんでした:| |
| **docker 負荷**| image| 警告なしで失敗します。| エラーはありません。 が、イメージはいずれかで読み込みではありません。|
| **docker の一時停止**| コンテナー| エラー:Windows のコンテナーを一時停止することはできません。サポートされていません| |
| **docker ポート**| コンテナー| | ポートを取得するが記載されていないもここには、RDP にできます。
| **docker プル**| デーモン| エラー:システムでは、ファイルのパスを見つけられません。私たちを実行できません。 コンテナーがこのイメージを使用します。| イメージの追加を取得するために使用することはできません。:) それを処理します。|
| **docker の再起動**| コンテナー| エラー:システムのシャット ダウンは、実行中です。| |
| **docker を再開します。**| コンテナー| | 一時停止がまだ動作しないためにをテストすることはできません。|
この一覧に表示されていないものが失敗した (場合、または場合よりも異なる方法でコマンドが失敗した場合は期待) をお知らせを使用していただいて [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

###Windows Server のコンテナーを部分的に使用する docker コマンド

一部の機能とコマンド。

| **Docker コマンド**| **上で実行しています.**| **パラメーター**| **メモ**|
|:-----|:-----|:-----|:-----|
| **docker をアタッチします。**| コンテナー| -いいえ stdin = false| コマンドには、ときに終了しない ctrl キーを押し P と ctrl キーを押し Q が押されました。|
| | | -sig プロキシ = true| 動作します。|
| **docker ビルド**| イメージ| -f, - ファイル| エラー:コンテキストを準備することができません。Synlinks を取得できません。|
| | | -force rm = false| 動作します。|
| | | -キャッシュなし = false| 動作します。|
| | | -q, - quiet = false| |
| | | -rm = true| 動作します。|
| | | -t, - タグ =""| 動作します。|
| **docker ログイン**| デーモン| -e, -p、-u| sporratic 動作|
| **docker プッシュ**| デーモン| | たまにを取得するエラーの「リポジトリは終了しない」です。|
| **docker rm**| コンテナー| -f| エラー:システムのシャット ダウンは、実行中です。|
この一覧に表示されていないものが失敗した場合、コマンドが、想定よりも、異なる方法で失敗した場合、または回避策が見つかった場合は、お知らせを使用して [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

###50 文字までに制限は対話型の Docker セッションにコマンドを貼り付ける

対話型の Docker セッションにコマンド ラインをコピーする場合に、50 文字に現在に制限されます。
貼り付けられた文字列は単純に切り捨てられます。

仕様ではこれで、制限の変換を処理します。

###net を使用するには、ユーザー名またはパスワードを要求する代わりにシステム エラー 1223年が返されます。

回避策: では、ユーザー名とパスワードの両方、net を使用してを実行するときに指定します。
たとえば、次のように入力します。


```
net use S: \\your\sources\here /User:shareuser [yourpassword]

```


###新しいコンテナーのイメージを作成するときに、HCS Shim エラー

次のようなエラー メッセージが表示されます。 場合、


```
hcsshim::ExportLayer - Win32 API call returned error r1=2147942523 err=The filename, directory name, or volume label syntax is incorrect. layerId=606a2c430fccd1091b9ad2f930bae009956856cf4e6c66062b188aac48aa2e34 flavour=1 folder=C:\ProgramData\docker\windowsfilter\606a2c430fccd1091b9ad2f930bae009956856cf4e6c66062b188aac48aa2e34-1868857733

```

0 日の修正プログラムの Windows Server 2016 TP3 によってアドレス指定された問題がヒットしています。
このエラーは、コンテナーでの Python 3.4.3.msi インストーラーまたはノード v0.12.7.msi を実行する場合にも発生します。

その他の hcsshim エラーをヒットした知らせを使用して [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

##リモート デスクトップでの windows server のコンテナーへのアクセス

Windows Server のコンテナーは、管理と対話して RDP セッションを使用できます。

RDP を使用して、Windows Server コンテナーへのリモート接続するには、次の手順が必要です。
コンテナーが NAT スイッチを介してネットワークに接続されていることが前提とします。
これは、インストール スクリプトまたは Azure で新しい VM の作成を通じて、コンテナーのホストを設定するときに、既定値です。

** 接続すると、コンテナーに **

次の手順は Docker を使用して、または PowerShell を使用する場合に指定するコンテナーを管理するいずれかを必要としています、。 `-RunAsAdministrator` コンテナーに接続するときに切り替えます。
コンテナーに接続するのには、次の手順を参照してください。

1.  コンテナーの IP アドレスを取得します。


  ```
  ipconfig

  ```

  次のようなものを返します


  ```
  Windows IP Configuration

  Ethernet adapter vEthernet (Virtual Switch-f758a5a9519e1956cc3bef06eb03e5728d3fb61cf6d310246185587be490210a-0):

  Connection-specific DNS Suffix  . :
  Link-local IPv6 Address . . . . . : fe80::91cd:fb4c:4ea5:51df%17
  IPv4 Address. . . . . . . . . . . : 172.16.0.2
  Subnet Mask . . . . . . . . . . . : 255.240.0.0
  Default Gateway . . . . . . . . . : 172.16.0.1

  ```

  一般的にこれは形式 172.16.x.x で IPv4 アドレスに注意してください。

1.  コンテナーの組み込みの管理者のユーザーのパスワードを設定します。


  ```
  net user administrator [yourpassword]

  ```


1.  コンテナーの組み込みの管理者のユーザーを有効にします。


  ```
  net user administrator /active:yes

  ```

** には、コンテナーのホスト **

Windows サーバーに既定で有効になっているセキュリティが強化された Windows ファイアウォールが設定されているのでを RDP を操作するためにいくつかの通信のポートを開く必要があります。
さらに、コンテナーは、コンテナーのホスト上のポート経由で到達可能であるために、ポートのマッピングが作成されます。

次の手順では、コンテナーのホスト上で管理者として起動 PowerShell が必要です。

1.  既定の Windows の高度なファイアウォール経由の RDP ポートを許可します。


  ```
  New-NetFirewallRule -Name "RDP" -DisplayName "Remote Desktop Protocol" -Protocol TCP -LocalPort @(3389) -Action Allow

  ```


1.  コンテナーへの RDP 接続の追加のポートを許可します。


  ```
  New-NetFirewallRule -Name "ContainerRDP" -DisplayName "RDP Port for connecting to Container" -Protocol TCP -LocalPort @(3390) -Action Allow

  ```

この手順では、コンテナーのホスト上のポート 3390 を開きます。
これについては、コンテナーに RDP セッションを使用します。
複数のコンテナーに接続する場合は、中に、追加のポート番号を提供するこの手順を繰り返すことができます。

1.  既存の NAT のポートのマッピングを追加します。
    
    ここでは、コンテナー内では、手順 1. から IP アドレスが必要があります。


  ```
  Add-NetNatStaticMapping -NatName ContainerNAT -Protocol TCP -ExternalPort 3390 -ExternalIPAddress 0.0.0.0 -InternalPort 3389 -InternalIPAddress [your container IP]

  ```

  ここでを確認するコンテナーでは、ポート 3389 を 3390 のポート上取得されるコンテナーのホストへの通信がリダイレクトされることを指定する IP アドレスでを実行しています。

** RDP を使用してコンテナーに接続します *。*

最後には、RDP を使用して、コンテナーに接続できます。
リモート デスクトップ クライアントを持つシステムに次のコマンドを実行してください。 そのためには (コンテナーのホスト仮想マシンを実行しているシステムなど) をインストールします。


```
mstsc /v:[ContainerHostIP]:3390 /prompt

```

指定してください。 `administrator` ユーザー名とパスワードとして選択したパスワードです。

リモート デスクトップ接続するように求められます証明書のエラーに関係なく、システムに接続するかどうか。
[はい] を選択する場合は、RDP 接続が開かれます。

**注: ** ログオフせず、コンテナーの RDP セッションを終了すると、シャット ダウンしてから、コンテナーができない場合があります。
コンテナーをシャット ダウンする前に、("exit"または RDP ウィンドウを閉じるだけ) せずに"にある [ログオフを」と入力して RDP セッションを終了することを確認してください。

--------------------------

##PowerShell の管理

###入力 PSSession containerid 引数を持つ、New-pssession しません。

これは、正しいです。
後で完全 cimsession サポートを計画しています。

音声機能の要望を [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

--------------------------
[Back to Container Home](../containers_welcome.md)


