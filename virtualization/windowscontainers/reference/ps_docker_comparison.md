ms。ContentId:4981828d-1a08-4d8c-a99d-874a926a153f
タイトル:PowerShell Docker 比較する

#PowerShell と Windows Server のコンテナーを管理するための Docker 比較

インボックス Windows ツール (このプレビューでは、PowerShell) と Docker などのオープン ソースの管理ツールの両方を使用して Windows Server のコンテナーの管理に多くの方法はあります。
ここではアウトラインの両方を個別に使用可能なガイド:

*   [Docker と Windows Server のコンテナーを管理します。](../quick_start/manage_docker.md)
*   [PowerShell を使用した Windows Server のコンテナーを管理します。](../quick_start/manage_powershell.md)

このページでは、Docker ツールと PowerShell 管理ツールの比較の深さの参照になります。

##HYPER-V Vm とコンテナー用 PowerShell

作成し、実行、および PowerShell コマンドレットを使用して Windows Server のコンテナーと対話できます。
開始するに必要なすべては、ボックス内の利用可能なです。

PowerShell の HYPER-V を使用した場合、コマンドレットの設計がよく知っている場合があります。
ワークフローの多くは、HYPER-V モジュールを使用して、仮想マシンを管理する方法に似ています。
代わりに `New-VM`, `Get-VM`, `Start-VM`, `Stop-VM`、があります。 `New-Container`, `Get-Container`, `Start-Container`, `Stop-Container`。
多くのコンテナーに固有のコマンドレットとパラメーターが、全般的なライフ サイクルがあるし、Windows コンテナーの管理は、HYPER-V の仮想マシンの場合と同様にほぼ探します。

##PowerShell 管理は Docker にどのような比較をするでしょうか。

Docker の; とまったく同じではない API を公開する、コンテナーの PowerShell コマンドレット一般的な規則として、コマンドレットは、さらに詳細に操作します。
Docker のいくつかのコマンドでは、PowerShell で緯線の非常に簡単ですがあります。

| Docker コマンド| PowerShell コマンドレット|
|----|----|
| `docker ps -a`| `Get-Container`|
| `docker images`| `Get-ContainerImage`|
| `docker rm`| `Remove-Container`|
| `docker rmi`| `Remove-ContainerImage`|
| `docker create`| `New-Container`|
| `docker commit <container ID>`| `New-ContainerImage -Container <container>`|
| `docker load <tarball>`| `Import-ContainerImage <AppX package>`|
| `docker save`| `Export-ContainerImage`|
| `docker start`| `Start-Container`|
| `docker stop`| `Stop-Container`|
PowerShell コマンドレットは、正確な完璧なパリティの場合ではないとは、かなり多くのコマンド用の PowerShell の置換が提供されていないことを * (特に `docker build` 」と「 `docker cp`).
1 つの代わりに 1 行がないことにはどのような場合がありますに留まりますが、 `docker run`。

\ * 変更される可能性があります。

###Docker 必要がありますが、実行します。 何が起こっているでしょうか。

PowerShell には、その方法を既に知っているユーザーに若干の他の一般的な対話モデルを提供する次の点をここで行ってきました。
もちろん、docker の動作に慣れて、これは少し観念の移行のなります。

1.  PowerShell のモデルのコンテナーのライフ サイクルは若干異なります。
    コンテナーの PowerShell モジュールでは、公開のより詳細な操作 `New-Container` (が停止するための新しいコンテナーを作成する) と `Start-Container`。
    
    作成して、コンテナーの開始、間に、コンテナーの設定を構成することもTP3、コンテナーのネットワーク接続を設定する機能は、その他の構成のみを公開することを計画してです。
    (追加/削除/接続/切断/取得/設定) を使用して-ContainerNetworkAdapter コマンドレットです。
2.  現在、コンテナー内の起動時に実行するコマンドを渡すことはできません。 ただし、実行されているコンテナーを使用して、対話型の PowerShell セッションを取得できますも `Enter-PSSession -ContainerId <ID of a running container>`を使用して実行されているコンテナー内のコマンドを実行して `Invoke-Command -ContainerId <container id> -ScriptBlock { code to run inside the container }` または `Invoke-Command -ContainerId <container id> -FilePath <path to script>`。  
    これらのコマンドのどちらも省略可能なします。 `-RunAsAdministrator` 高 privilige アクションのフラグです。  

##注意事項と既知の問題

1.  現在、コンテナーのコマンドレットを使用しないため、コンテナーまたは Docker、によって作成されたイメージに関するに関する知識を持って Docker が認識していないすべてのコンテナーとイメージ、PowerShell を使用して作成します。
    Docker 内で作成した場合は Docker; で管理します。powershell で作成した場合は、PowerShell を使って管理します。
2.  多くの作業をエラー メッセージの向上より優れた進行状況レポート、無効なイベント文字列、およびなど--、エンド ユーザー エクスペリエンスを向上させるために必要があります。
    状況の詳細を受け取っている場合に実行した場合や、情報の向上、ご自由にフォーラムをご提案を送信します。

##クイック runthrough

ここでは、いくつかの一般的なワークフローの説明です。

これには、"ServerDatacenterCore"という名前の OS コンテナー イメージをインストールし、「仮想スイッチ」(New-vmswitch を使用) という名前の仮想スイッチを作成したと仮定しています。

'' PowerShell

###1.イメージを列挙します。

#この時点で、システム上のイメージを列挙できます。

Get ContainerImage

#Get ContainerImage では、フィルターも受け入れます。

#たとえば、S が小文字で始まる名前を持つすべてのコンテナーのイメージを返します。

Get ContainerImage-名前 S *

#この結果は、変数に保存できます。

#(PowerShell に慣れていない場合は、「$」変数を表します。)

$baseImage = get ContainerImage-名前 ServerDatacenterCore
$baseImage

###2.作成して、コンテナーの列挙

#ここで、このイメージを使用して、新しいコンテナーを作成できます。

新しいコンテナーの名前"MyContainer"ContainerImage $baseImage SwitchName「仮想スイッチ」

#これですべてのコンテナーを列挙することができます。

Get コンテナー

#同様に、このコンテナーを変数に保存します。

$container1 = Get コンテナー-"MyContainer"という名前を

###3.コンテナーでは、コンテナーを実行していると、コンテナーの停止との対話を開始します。

#これで、コンテナーを開始してみましょう。

開始コンテナーの名前を「MyContainer」

#(私たちでしたも開始したこのコンテナーを使用して"開始コンテナーのコンテナー ドル container1"です)。

#現在実行中、コンテナーでは、先に進んでし、対話型の PowerShell セッションを入力します。

入力 PSSession-ContainerId $ container1 します。Id

#これが、コンテナーの内部からの PowerShell プロンプトを最終的に表示されます。

#指定された対話型のコマンド プロンプトで行ったすべての処理を試すことができます"の実行 - docker こと"です。

#ここでは、経験を証明するためにここでは、新しいファイルを作成できます。

cd \
mkdir テスト
cd のテスト
エコー"hello world"> hello.txt
exit

#これで、外部の世界に戻っておにする必要があります。 にもかかわらず終了後に、PowerShell セッションでは、

#コンテナー自体がまだ実行中、ように、コンテナーにもう一度出力を確認できます。

container1 ドル

#新しいイメージには、このコンテナーをコミットすることが、前に、コンテナーを停止する必要があります。

#今すぐに操作してみましょう。

コンテナーに停止コンテナー container1 ドル

###4.新しいコンテナーのイメージを作成します。

#新しいイメージにコミットしてみましょう。

$image1 = ContainerImage で新しい-コンテナー ドル container1-パブリッシャーは、次のテスト: Image1 という名前を-バージョン 1.0

#正当性のためにここでも、すべてのイメージを列挙します。

Get ContainerImage

#例を繰り返します。 新しいイメージに基づいて別のコンテナーを確認します。

ドル container2 = 新しいコンテナーの名前を「仮想スイッチ」を"MySecondContainer"- ContainerImage ドル image1 SwitchName

#(必要な場合は、2 つ目のコンテナーを開始していることを確認、新しいファイル

#"\Test\hello.txt"は予期したとおり) です。

###5.コンテナーの削除

#作成した最初のコンテナーが現在停止しています。 それではこれを取り除きます。

削除するコンテナーのコンテナー container1 ドル

#それになったことを確認します。

Get コンテナー

###6.エクスポート、削除、および画像の読み込み

#イメージ ベースの OS イメージではないに、.appx パッケージ ファイルにエクスポートおできます。

エクスポート ContainerImage-ドル image1 をイメージのパス"C:\exports"

#これにより、C:\exports フォルダーに .appx ファイルが作成する必要があります。

#以前では、使用して同じのパブリッシャー、名、およびバージョン、イメージを指定した場合

#名前が返される結果の .appx お察し"CN = Test_Image1_1.0.0.0.appx"です。

#イメージを再度インポートしよう、前に、イメージを削除する必要があります。

#(このイメージに依存する任意の実行中のコンテナーがある場合をする場合に最初に停止します。)

削除 ContainerImage-image1 ドルの画像

#これで、イメージをもう一度インポートしてみましょう。

インポート ContainerImage-パス C:\exports\CN=Test_Image1_1.0.0.0.appx

#お以前に作成したコンテナーこのイメージに依存します。 開始することができます。

開始コンテナーのコンテナー container2 ドル 


```

### Build your own sample
You can see all the Containers cmdlets using `Get-Command -Module Containers`.  There are several other cmdlets that are not described here, which we'll leave to you to learn about on your own.    
**Note** This won't return the `Enter-PSSession` and `Invoke-Command` cmdlets, which are part of core PowerShell.

You can also get help about any cmdlet using `Get-Help [cmdlet name]`, or equivalently `[cmdlet name] -?`.  Today, the help output is auto-generated and just tells you the syntax for commands; we will be adding further documentation as we get closer to finalizing the cmdlet design.

A nicer way to discover the syntax is the PowerShell ISE, which you may not have looked at before if you haven't used PowerShell very much. If you're running on a SKU that permits it, try starting the ISE, opening the Commands pane, and choosing the "Containers" module, which will show you a graphical representation of the cmdlets and their parameter sets.

PS: Just to prove it can be done, here's a PowerShell function that composes some of the cmdlets we've seen already into an ersatz `docker run`. (To be clear, this is a proof of concept, not under active development.)

``` PowerShell
function Run-Container ([string]$ContainerImageName, [string]$Name="fancy_name", [switch]$Remove, [switch]$Interactive, [scriptblock]$Command) {
    $image = Get-ContainerImage -Name $ContainerImageName
    $container = New-Container -Name $Name -ContainerImage $image
    Start-Container $container

    if ($Interactive) {
         Start-Process powershell ("-NoExit", "-c", "Enter-PSSession -ContainerId $($container.Id)") -Wait
    } else {
        Invoke-Command -ContainerId $container.Id -ScriptBlock $Command
    }

    Stop-Container $container

    if ($Remove) {
        Remove-Container $container -Force
    }
} 

```


##Docker

Docker コマンドでは、Windows Server のコンテナーを管理できます。
Docker を通じて Windows コンテナーが Linux 対応する比較を実行できるし、同じ管理する必要がありますが発生する、中には、意味のある単にない Windows コンテナーといくつかの Docker コマンド。
他のユーザーだけではまだされてテスト (取得があります)。

いない Docker で利用可能な API のドキュメントを複製するために、次に、管理 Api へのリンクを示します。
そのチュートリアルではすばらしいです。

いるものと、進行中の作業の文書の Docker Api では機能しない管理するいるとします。


