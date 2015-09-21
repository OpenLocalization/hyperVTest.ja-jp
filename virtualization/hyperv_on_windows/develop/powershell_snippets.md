ms。ContentId:8DE9250B-556B-47BC-AD9A-8992B3D3D0F9
タイトル:PowerShell のスニペット

#PowerShell のスニペット

ホー HB プロセスをテストするためには、この文を追加します。
PowerShell は、素晴らしいスクリプティング、オートメーション、および管理ツール HYPER-V 用です。ここでは、いくつかのすばらしいものを実行できる検証用のツールボックスです。

すべての HYPER-V 管理では、すべてのスクリプトと管理者とスニペットは、HYPER-V の管理者アカウントから管理者として実行する必要があります、実行が必要です。

適切なアクセス許可があるかどうか不明でない場合は、入力します。 `Get-VM` 移動する準備ができたら、エラーなしで実行している場合。

##PowerShell の直接のツール

すべてのスクリプトとスニペットでは、このセクションでは、次の基本原則に依頼します。

**要件** :  

*   PowerShell のダイレクトします。
    Windows の 10 のゲストとホスト OS です。

**共通変数** :  
`$VMName` -VMName での文字列です。
使用可能な Vm の一覧を参照してください。 `Get-VM``$cred` -ゲスト OS の資格情報です。
使用して設定できます。 `$cred = Get-Credential`

###ゲストが起動されているかどうかの確認します。

HYPER-V マネージャーでは、多くの場合、ゲスト OS が起動されているかどうかがわかりにくく、ゲスト オペレーティング システムに可視性を提供します。

ゲストが起動されているかどうかを確認するのにには、このコマンドを使用します。

'' PowerShell
場合 ((Invoke-command-VMName $VMName-資格情報の $cred {"Test"}) ne"Test") {「起動されていない」Write-host} else {Write-host"Booted"}


```

**Outcome**  
Prints a friendly message declaring the state of the guest OS.


### Script locking until the guest has booted

The following function waits uses the same principle to wait until PowerShell is available in the guest (meaning the OS has booted and most services are running) then returns.

``` PowerShell
function waitForPSDirect([string]$VMName, $cred){
   Write-Output "[$($VMName)]:: Waiting for PowerShell Direct (using $($cred.username))"
   while ((Invoke-Command -VMName $VMName -Credential $cred {"Test"} -ea SilentlyContinue) -ne "Test") {Sleep -Seconds 1}}

```

**結果**  
VM への接続が成功するまでは、わかりやすいメッセージとロックを出力します。
サイレント モードでは成功します。

###スクリプトは、ゲストには、ネットワーク時までロック

PowerShell の直接仮想マシンを前に仮想マシン内の PowerShell セッションに接続することは、IP アドレスを受け取りました。

'' PowerShell

#DHCP を待機します。

中に ((Get NetIPAddress | でしょうか。
アドレスファ ・ リ-eq IPv4 か?
IPAddress - ne 127.0.0.1)。「Dhcp」ne SuffixOrigin) {スリープ-(ミリ秒) の 10}
```

** 結果の **
DHCP リースを受信するまでロックされます。
このスクリプトはしない検索、特定のサブネットまたは IP アドレスの場合は、どのようなネットワークの構成に関係なく、機能しています。
サイレント モードでは成功します。

##PowerShell を使用した資格情報の管理

HYPER-V のスクリプトでは、1 つまたは複数の仮想マシン、HYPER-V ホスト、またはその両方の資格情報の処理を頻繁に必要です。

これを実現する PowerShell の直接接続または標準の PowerShell リモート処理を使用するときに複数の方法はあります。

1.  最初の (そして最も単純な) 方法は、ホストとゲスト、またはローカルおよびリモート ホストで有効であるのと同じユーザー資格情報です。
    これはやドメイン環境である場合は、Microsoft アカウントでログインしている場合に非常に簡単です。
    このシナリオでのみ実行できます。 `Invoke-Command -VMName "test" {get-process}`。
2.  資格情報を要求する PowerShell を使用します。
    資格情報が一致しない場合は、資格情報プロンプトが、仮想マシンの適切な資格情報を提供することができますを自動的に表示されます。
3.  再利用するための変数には、資格情報を格納します。
    次のような単純なコマンドを実行しています。  
    ` PowerShell
    $localCred = Get-Credential
    `
    次のように何かを実行しています。
    '' PowerShell
    Invoke-command VMName"test"- 資格情報 $localCred {get プロセスしました


  ```
  Will mean that you only get prompted once per script/PowerShell session for your credentials.

4. Code your credentials into your scripts.  **Don't do this for any real workload or system**
 > Warning:  _Do not do this in a production system.  Do not do this with real passwords._

  You can hand craft a PSCredential object with some code like this:  
  ``` PowerShell
  $localCred = New-Object -typename System.Management.Automation.PSCredential -argumentlist "Administrator", (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) 

  ```

大きな安全でないがテストに役立ちます。
ようになりましたが表示ないこのセッションですべての画面の指示します。


