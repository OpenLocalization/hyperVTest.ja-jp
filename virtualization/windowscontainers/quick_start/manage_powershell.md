ms。ContentId: d0a07897-5fd2-41a5-856d-dc8b499c6783
タイトル:PowerShell を使用した Windows Server のコンテナーを管理します。

#クイック スタート:Windows Server のコンテナーと PowerShell

この記事は、PowerShell を使用した Windows Server のコンテナーの管理の基礎について説明します。
Windows Server のコンテナーとサーバーのコンテナーの Windows イメージを作成する、Windows Server のコンテナーおよびコンテナーのイメージの削除、および最後に、アプリケーションを Windows Server のコンテナーに展開する対象となるアイテムが含まれます。
このチュートリアルで得られた教訓では、PowerShell を使用して Windows Server のコンテナーの展開と管理の調査を開始するを有効にする必要があります。

ご質問があるでしょうか。
依頼、 [Windows のコンテナーのフォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers)。

> **注: ** PowerShell で作成された Windows Server のコンテナーいない現在管理できます Docker と逆です。
> 代わりに、Docker とコンテナーを作成するを参照してください。 [クイック スタート:Windows Server のコンテナーと Docker](./manage_docker.md)。
> < br/>< br/> を詳細を知りたい場合 [読み取り、よく寄せられる質問](../about/faq.md#WhydoIhavetopickbetweenDockerandPowerShellforWindowsServerContainermanagement)。
> 

##前提条件

このチュートリアルを完了するのには、次の項目を適所に配置する必要があります。

*   Windows Server 2016 TP3 か、後で、Windows Server のコンテナーの機能を構成します。
    セットアップ ガイドを完了したら、これが Azure または HYPER-V で作成された VM です。
*   このシステムは、ネットワークに接続されていると、インターネットにアクセスすることである必要があります。

コンテナーの機能を構成する必要がある場合は、次のガイドを参照してください。 [Azure でのコンテナーのセットアップ](./azure_setup.md) または [HYPER-V のコンテナーのセットアップ](./container_setup.md)。

##PowerShell を使用した基本的なコンテナーの管理

この最初の例は、作成および Windows Server のコンテナーと PowerShell での Windows サーバー コンテナーのイメージを削除するための基本事項について説明します。

Windows Server のコンテナーのホスト システムにログを通じてウォークを開始するには、Windows のコマンド プロンプトが表示されます。

!(media/cmd.png)

」と入力して、PowerShell セッションを開始します。 `powershell`。
プロンプトを変更するときに、PowerShell セッションであることがわかります `C:\directory>` で仮想ネットワーク アダプター `PS C:\directory>`。


```
C:\> powershell
Windows PowerShell
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\>

```

vmmblue_2 `Get-Command` コンテナーのモジュールで利用可能なコマンドを表示するには


```
PS C:\> Get-Command -Module containers

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Install-ContainerOSImage                           1.0.0.0    Containers
Function        Uninstall-ContainerOSImage                         1.0.0.0    Containers
Cmdlet          Add-ContainerNetworkAdapter                        1.0.0.0    Containers
Cmdlet          Connect-ContainerNetworkAdapter                    1.0.0.0    Containers
Cmdlet          Disconnect-ContainerNetworkAdapter                 1.0.0.0    Containers
Cmdlet          Export-ContainerImage                              1.0.0.0    Containers
Cmdlet          Get-Container                                      1.0.0.0    Containers
Cmdlet          Get-ContainerHost                                  1.0.0.0    Containers
Cmdlet          Get-ContainerImage                                 1.0.0.0    Containers
Cmdlet          Get-ContainerNetworkAdapter                        1.0.0.0    Containers
Cmdlet          Import-ContainerImage                              1.0.0.0    Containers
Cmdlet          Move-ContainerImageRepository                      1.0.0.0    Containers
Cmdlet          New-Container                                      1.0.0.0    Containers
Cmdlet          New-ContainerImage                                 1.0.0.0    Containers
Cmdlet          Remove-Container                                   1.0.0.0    Containers
Cmdlet          Remove-ContainerImage                              1.0.0.0    Containers
Cmdlet          Remove-ContainerNetworkAdapter                     1.0.0.0    Containers
Cmdlet          Set-ContainerNetworkAdapter                        1.0.0.0    Containers
Cmdlet          Start-Container                                    1.0.0.0    Containers
Cmdlet          Stop-Container                                     1.0.0.0    Containers
Cmdlet          Test-ContainerImage                                1.0.0.0    Containers

```

次に、システムでは、有効な IP アドレスを使用して、持っていることを確認します。 `ipconfig` 後で使用するのには、このアドレスを書き留めます。


```
ipconfig

Ethernet adapter Ethernet 3:

   Connection-specific DNS Suffix  . :
   IPv6 Address. . . . . . . . . . . : 2601:600:8f01:84eb::e
   IPv6 Address. . . . . . . . . . . : 2601:600:8f01:84eb:a8c1:a3e:96b7:ffcb
   Link-local IPv6 Address . . . . . : fe80::a8c1:a3e:96b7:ffcb%5
   IPv4 Address. . . . . . . . . . . : 192.168.1.25

```

使用せずに Azure の仮想マシンで作業している場合 `ipconfig` Azure の仮想マシンのパブリック IP アドレスを取得する必要があります。

!(media/newazure9.png)

###手順 1 - 新しいコンテナーを作成します。

Windows Server のコンテナーを作成する前に、コンテナーのイメージの名前と、新しいコンテナーに追加される仮想スイッチの名前を必要があります。

「 `Get-ContainerImage` コンテナーのイメージのリストを返すためのコマンドは、ホストに読み込まれます。
コンテナーの作成に使用するイメージの名前を書き留めます。
'' PowerShell
Get ContainerImage

名前のパブリッシャー バージョン IsOSImage
----              ---------    -------      ---------
WindowsServerCore CN = Microsoft 10.0.10514.0 は True


```

Use the `Get-VMSwitch` command to return a list of switches available on the host. Take note of the switch name that will be used with the container.

``` PowerShell
Get-VMSwitch

Name           SwitchType NetAdapterInterfaceDescription
----           ---------- ------------------------------
Virtual Switch NAT

```

コンテナーを作成するには、次のコマンドを実行します。
実行している場合 `New-Container` コンテナーの名前をコンテナーのイメージを指定し、コンテナーで使用するネットワーク スイッチを選択します。
$Container の変数に出力を配置するこの例に注意してください。
この演習の後半に便利になります。

'' PowerShell
$container = 新しいコンテナーの名前"MyContainer"ContainerImageName WindowsServerCore SwitchName「仮想スイッチ」


```

To see a list of containers on the host and verify that the container was created, use the `Get-Container` command. Notice that a container has been created with the name of MyContainer, however it has not been started.

``` PowerShell
Get-Container

Name        State Uptime   ParentImageName
----        ----- ------   ---------------
MyContainer Off   00:00:00 WindowsServerCore

```

コンテナーを開始するには、次のように使用します。 `Start-Container` proivding コンテナーの名前。

'' PowerShell
開始コンテナーの名前を「MyContainer」


```

You can interact with containers using PowerShell remoting commands such as `Invoke-Command`, or `Enter-PSSession`. The example below creates a remote PowerShell session into the container using the `Enter-PSSession` command. This command needs the container id in order to create the remote session. The container id was stored in the `$container` variable when the container was created. 

Notice that once the remote session has been created the command prompt will change to include the first 11 characters of the container id `[2446380e-629]`.

``` PowerShell
Enter-PSSession -ContainerId $container.ContainerId -RunAsAdministrator

[2446380e-629]: PS C:\Windows\system32>

```

コンテナーは、物理マシンまたはバーチャル マシンのように非常によく管理できます。
などのコマンドします。 `ipconfig` コンテナー内の IP アドレスを返す `mkdir` コンテナーとのように、PowerShell コマンドでディレクトリを作成するには `Get-ChildItem` すべての作業です。
ファイルまたはフォルダーの作成などのコンテナーを変更してください。
たとえば、次のコマンドでは、コンテナーに関するネットワーク構成データを格納するファイルが作成されます。

'' PowerShell
ipconfig > c:\ipconfig.txt


```

You can read the contents of the file to ensure the command completed successfully. Notice that the IP address contained in the text file matches that of the container.

``` PowerShell
type c:\ipconfig.txt

Ethernet adapter vEthernet (Virtual Switch-E0D87408-325B-4818-ADB2-2EC7A2005739-0):

   Connection-specific DNS Suffix  . : corp.microsoft.com
   Link-local IPv6 Address . . . . . : fe80::400e:1e0e:591c:beef%18
   IPv4 Address. . . . . . . . . . . : 172.16.0.2
   Subnet Mask . . . . . . . . . . . : 255.240.0.0
   Default Gateway . . . . . . . . . : 172.16.0.1

```

これで、コンテナーが変更されている、リモートの PowerShell セッションを終了します。

'' PowerShell
exit


```

Stop the container by providing the container name to the `Stop-Container` command. When this command has completed, you will be back in control of the container host.

``` PowerShell
Stop-Container -Name "MyContainer"

```


###手順 2 - 新しいコンテナーのイメージを作成します。

このコンテナーから、イメージを行うようになりましたことができます。
このイメージは、コンテナーのスナップショットのように動作し、再展開できます何度もします。

名前付き 'newimage' を使用する新しいイメージを作成する、 `New-ContainerImage` コマンドなど) を指定します。
このコマンドを使用する場合は、新しいイメージは、次に見られるよう追加メタデータの名前をキャプチャするコンテナーを指定します。

'' PowerShell
$newimage = MyContainer を新規 ContainerImage ContainerName-パブリッシャーのデモ - 名前 newimage-バージョン 1.0


```

Use `Get-ContainerImage` to return a list of Container Images. Notice that a new image with the name 'newimage' has been created.

``` PowerShell
Get-ContainerImage

Name              Publisher    Version      IsOSImage
----              ---------    -------      ---------
newimage          CN=Demo      1.0.0.0      False
WindowsServerCore CN=Microsoft 10.0.10254.0 True

```


###ステップ 3 - イメージから新しいコンテナーを作成します。

コンテナーのカスタマイズされたイメージを作成したら、これでは、このイメージから新しいコンテナーを展開してください。

'新しいコンテナー' 'newimage' という名前のコンテナーのイメージからをという名前のコンテナーを作成、'$newcontainer' という名前の変数に結果を出力します。

'' PowerShell
$newcontainer = 新しいコンテナーの名前の新しい「コンテナー」- ContainerImageName newimage SwitchName「仮想スイッチ」


```

Start the new container.
``` PowerShell
Start-Container $newcontainer

```

により、コンテナーには、PowerShell のリモート セッションを作成します。
'' PowerShell
入力 PSSession ContainerId $newcontainer をダブルクリックします。ContainerId RunAsAdministrator


```

Finally notice that this new container contains the ipconfig.txt file created earlier in this exercise.

``` PowerShell
type c:\ipconfig.txt

Ethernet adapter vEthernet (Virtual Switch-E0D87408-325B-4818-ADB2-2EC7A2005739-0):

   Connection-specific DNS Suffix  . : corp.microsoft.com
   Link-local IPv6 Address . . . . . : fe80::400e:1e0e:591c:beef%18
   IPv4 Address. . . . . . . . . . . : 172.16.0.2
   Subnet Mask . . . . . . . . . . . : 255.240.0.0
   Default Gateway . . . . . . . . . : 172.16.0.1

```

 完了した時点リモート PowerShell セッションを終了するは、このコンテナーを使用します。

'' PowerShell
exit


```

This exercise has shown that an image taken from a modified container will include all modifications. While the example here was a simple file modification, the same would apply if you were to install software into the container such as a web server. Using these methods, custom images can be created that will deploy application ready containers.

### Step 4 - Remove Containers and Container Images

To stop all running containers run the command below. If any containers are in a stopped state when you run this command, you receive a warning, which is ok.

``` PowerShell
Get-Container | Stop-Container

```

すべてのコンテナーを削除するのには、次を実行します。

'' PowerShell
Get コンテナー |削除するコンテナーの強制


```
To remove the container image named 'newimage', run the following.

``` PowerShell
Get-ContainerImage -Name newimage | Remove-ContainerImage -Force

```


##コンテナー内の Web サーバーをホストします。

次の例では、Windows Server のコンテナーのより実用的な使用例を示します。
この演習では含まれている手順で Windows Server のコンテナー内でホストされる web アプリケーションを展開するために使用できる web サーバー コンテナーのイメージを作成する手順を説明します。

###手順 1 – Windows Server コア OS イメージからコンテナーの作成

Web サーバーのコンテナーのイメージを作成するには、まず、展開および Windows Server のコア OS イメージからコンテナーを開始する必要があります。
` PowerShell
$container = New-Container -Name webbase -ContainerImageName WindowsServerCore -SwitchName "Virtual Switch"
 `

コンテナーを開始します。
'' PowerShell
開始 $container をコンテナー


```

When the container is up, create a remote PowerShell session with the container.
``` PowerShell
Enter-PSSession -ContainerId $container.ContainerId -RunAsAdministrator

```


###手順 2 - Web サーバーのソフトウェアのインストール

次の手順では、web サーバーのソフトウェアをインストールします。
この例では、Windows の nginx が使用されます。
自動的にダウンロードして、c:\nginx-1.9.3 nginx ソフトウェアを展開するには、次のコマンドを使用します。
**注意** この手順には、インターネットに接続するコンテナーのホストが必要です。
この手順を生成した場合、接続、または名前の解決エラーは、コンテナーのホストのネットワーク構成を確認します。

Nginx のソフトウェアをダウンロードします。
'' PowerShell
wget 'http://nginx.org/download/nginx-1.9.3.zip' を uri の"c:\nginx-1.9.3.zip"を出力ファイル


```

Extract the nginx software.
``` PowerShell
Expand-Archive -Path C:\nginx-1.9.3.zip -DestinationPath c:\ -Force

```

これは、nginx ソフトウェアのインストールを完了する必要があることだけです。

リモートの PowerShell セッションを終了します。
'' PowerShell
exit


```

Stop the container using the following command. 
``` PowerShell
Stop-Container $container

```


###ステップ 3 - Web サーバーのコンテナーからイメージを作成

により、コンテナーを nginx web サーバーのソフトウェアが含まれるように変更すると、このコンテナーからイメージを作成できます。
これを行うには、次のコマンドを実行します。
'' PowerShell
$webserverimage = New ContainerImage-コンテナー $container-パブリッシャーは、次のデモ - nginxwindows という名前のバージョン 1.0


```
When completed, use the `Get-ContainerImage` command to validate that the image has been created.

``` PowerShell
Get-ContainerImage

Name              Publisher    Version      IsOSImage
----              ---------    -------      ---------
nginxwindows      CN=Demo      1.0.0.0      False
WindowsServerCore CN=Microsoft 10.0.10254.0 True

```


###手順 4 - Web サーバーの準備完了のコンテナーを展開します。

'Nginxwindows' イメージに基づく Windows Server のコンテナーを展開するには、次のように使用します、。 `New-Container` PowerShell コマンド。

'' PowerShell
$webservercontainer 新しいコンテナーを =-- ContainerImageName nginxwindows の webserver1 という名前を SwitchName「仮想スイッチ」


```

Start the container.
``` PowerShell
Start-Container $webservercontainer

```

新しいコンテナーとリモート PowerShell セッションを作成します。
'' PowerShell
入力 PSSession ContainerId $webservercontainer をダブルクリックします。ContainerId RunAsAdministrator


```

Once working inside the container, the nginx web server can be started and web content staged. To start the nginx web server, change to the nginx installation directory.
``` PowerShell
cd c:\nginx-1.9.3\

```

Nginx の web サーバーを起動します。
'' PowerShell
nginx を開始します。


```

And exit this PS-Session.  The web server will keep running.
``` PowerShell
exit

```


###ステップ 5 - ネットワークのコンテナーを構成します。

構成によっては、コンテナーのホストとネットワークのコンテナーはか、IP アドレスを受け取る、DHCP サーバーまたはコンテナーのホストからネットワーク アドレス変換 (NAT) を使用してそれ自体です。
このガイド付きひととおりが NAT を使用するように構成
この構成では、コンテナーからのポートは、コンテナーのホスト上のポートにマップされます。
コンテナーでホストされているアプリケーションは、IP アドレスを使用してアクセス/コンテナーのホストの名前。
たとえば、アプリケーションに一般的な http 要求をこの http://contianerhost:55534 のようになりますポート 80、コンテナーからが、コンテナーのホスト上のポート 55534 にマップされていた場合。
これにより、多くのコンテナーを実行し、同じポートを使用して要求に応答するは、これらのコンテナー内のアプリケーションでは、コンテナーのホストできます。

この実習では、このポートのマッピングを作成する必要があります。
実行するのには、コンテナーと内部の (アプリケーション)、および構成される外部 (コンテナーのホスト) ポートの IP アドレスを知っておく必要があります。
この例については、単純さの維持し、ポート、ホストのポート 80 に、コンテナーから 80 をマップしてみましょうします。
使用して、 `Add-NetNatStaticMapping` コマンドを `–InternalIPAddress` このチュートリアルでは '172.16.0.2' にする必要がありますコンテナーの IP アドレスになります。

'' PowerShell
"ContainerNat"を追加 NetNatStaticMapping NatName-TCP - ExternalIPAddress 0.0.0.0 InternalIPAddress 172.16.0.2 InternalPort 80 - ExternalPort 80 のプロトコル


```
When the port mapping has been created you will also need to configure an inbound firewall rule for the configured port. To do so for port 80 run the following command. This script can be copied into the VM. 

``` PowerShell
if (!(Get-NetFirewallRule | where {$_.Name -eq "TCP80"})) {
    New-NetFirewallRule -Name "TCP80" -DisplayName "HTTP on TCP/80" -Protocol tcp -LocalPort 80 -Action Allow -Enabled True
}

```

次に Azure から作業しているし、仮想マシンのエンドポイントにまだ作成していない場合には、ここで作成する必要があります。
Azure VM のエンドポイントの詳細については、この記事を参照してください。 [Azure の仮想マシンのエンドポイントをセットアップします。](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/)。

###ステップ 6: コンテナーのホストされる web サイトにアクセス

により作成された、web サーバー コンテナーでは、これで、コンテナーでホストされるアプリケーションは、チェック アウトすることができます。
これを行うには、別のコンピューターでのブラウザーを開きを入力します `http://containerhost-ipaddress`。
ここではするが、コンテナーのホストとコンテナー自体の IP アドレスを参照することに注意してください。
作業している場合は、Azure の仮想マシンからこのなります、パブリック IP アドレスまたはクラウド サービスの名前。

すべて正しく構成されている場合は、nginx の開始ページが表示されます。

!(media/nginx.png)

この時点では、自由に、web サイトを更新します。
独自のサンプルの web サイトにコピーするか、このデモをまだ作成して単純な"Hello World"サンプル サイトを使用します。
サンプルを使用するには、は、まず、コンテナーと PS のリモート セッションを再確立する必要があります。

まず、コンテナーとは、リモートの PS セッションを再作成する必要があります。
'' PowerShell
入力 PSSession ContainerId $webservercontainer をダブルクリックします。ContainerId RunAsAdministrator


```
Then run the following command to download and replace the index.html file.

``` powershell
wget -uri 'https://raw.githubusercontent.com/Microsoft/Virtualization-Documentation/master/doc-site/virtualization/windowscontainers/quick_start/SampleFiles/index.html' -OutFile "C:\nginx-1.9.3\html\index.html"

```

Web サイトが更新された後の移動します。 `http://containerhost-ipaddress`。

!(media/hello.png)

##ビデオ チュートリアル

<iframe src="https://channel9.msdn.com/Blogs/containers/Quick-Start-Deploying-and-Managing-Windows-Server-Containers-with-PowerShell/player" width="800" height="450" allowFullScreen="true" frameBorder="0" scrolling="no" caps_internal_Id="21ffd01f-b578-4fc7-85c8-a8e98efb718a" />

##次の手順

コンテナーを設定し、ツールの概要がある場合は、これでは、コンテナー化、独自のアプリの構築を参照してください。

ここより詳細です [PowerShell リファレンス](../reference/powershell_overview.md)。

ただし、これは、 **プレビュー** バグがあるし、多数の進行中の作業があります。
[このページ](../about/work_in_progress.md) 既知の問題の多くが含まれます。

監視も、 [フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowscontainers) 非常に密接にします。

上にあるも作成済みのサンプル [GitHub](https://github.com/Microsoft/Virtualization-Documentation/tree/master/windows-server-container-samples)。

-----------------------------------
[Back to Container Home](../containers_welcome.md)   
[Known Issues for Current Release](../about/work_in_progress.md)


