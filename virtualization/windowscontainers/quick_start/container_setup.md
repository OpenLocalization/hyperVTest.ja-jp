ms.ContentId: 71a03c62-50fd-48dc-9296-4d285027a96a
タイトル:ローカルの VM で Windows のコンテナーをセットアップします。

#Windows Server のコンテナーのための Windows サーバーに関するテクニカル プレビューの準備

作成して、Windows Server のコンテナーの管理をするためには、Windows Server 2016 に関するテクニカル プレビュー環境を準備する必要があります。
このガイドは、HYPER-V 仮想マシンで Windows Server のコンテナーの構成方法を説明します。

> その他のファースト ステップ ガイド。
> 

*   Windows Server のコンテナーを実行します。 [Azure](./azure_setup.md)。
*   Windows Server のコンテナーを実行します。 [既存のバーチャル マシン](./inplace_setup.md)。
*   Windows Server のコンテナーをセットアップします。 [物理的な Windows Server Core インストール上](./inplace_setup.md)。
    
    **コンテナーの OS イメージをインストールする前に」を参照してください。**  Microsoft Windows Server のプレリリース版ソフトウェア (以下、「ライセンス条項」) のライセンス条項は、Microsoft Windows のコンテナーの OS イメージは、(以下「補足的なソフトウェア) 本追加ソフトウェア使用に適用されます。
    をダウンロードして、追加のソフトウェアを使用して、ライセンス条項に同意して、ライセンス条項に同意していない場合に使用できません。
    Microsoft Corporation によっては、Windows Server のプレリリース版のソフトウェアと追加のソフトウェアの両方がライセンス供与します。
    
    *   Windows の 10 を実行しているシステムと Windows Server 技術のプレビュー 2 またはそれ以降。
    *   HYPER-V ロールが有効になっている ([手順を参照してください。](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install#UsingPowerShell)).
    *   コンテナーのホスト イメージ、OS ベース イメージおよびセットアップ スクリプトの使用可能記憶域を 20 GB です。
    *   HYPER-V ホスト上の管理者のアクセス許可。

> Windows Server のコンテナーでは、このガイドわかりやすくには、Windows Server のコンテナーのホストを実行する HYPER-V 環境が使用されていると仮定していますが、HYPER-V は必要ありません。
> 

##新しいバーチャル マシン上の新しいコンテナー ホストのセットアップ

Windows Server のコンテナーは、Windows Server のコンテナーのホスト OS ベース イメージのコンテナーなどのいくつかのコンポーネントで構成されます。
まとめたものをダウンロードおよびをこれらの項目を構成するスクリプトです。
新しい HYPER-V バーチャル マシンを展開し、Windows Server のコンテナーのホストとしてこのシステムを構成するには、この手順に従います。

管理者として PowerShell セッションを開始します。
これは、PowerShell アイコンを右クリックして [管理者として実行] を選択すると、または任意の PowerShell セッションから、次のコマンドを実行して行うことができます。

'' powershell
powershell の起動プロセスの動詞 runAs


```

Use the following command to download the configuration script. The script can also be manually downloaded from this location - [Configuration Script](http://aka.ms/newcontainerhost).

``` PowerShell
wget -uri https://aka.ms/newcontainerhost -OutFile New-ContainerHost.ps1

```

作成し、コンテナーのホストを構成するには、次のコマンドを実行する場所 `<containerhost>` 仮想マシンの名前になり、 `<password>` パスワードに割り当てる管理者のアカウント。

``` powershell
.\New-ContainerHost.ps1 – VmName <containerhost> -Password < パスワード >


```

When the script begins you will be asked to read and accept licensing terms.


```

インストールとコンテナーの仮想マシンと Windows Server テクニカル プレビュー 3 を使用する前に次の操作を行う必要があります。
   1.
このリンクに移動して、ライセンス条項を確認します http://aka.ms/WindowsServerTP3ContainerVHDEula。
   2.
記録用にライセンス条項を印刷し、保持します。
このようなに同意するをダウンロードし、コンテナーの仮想マシンでの Windows Server Technical Preview 3 の使用
ライセンス条項。
ライセンス条項に同意して受け入れましたであることを確認してください。
[N] [Y] はい [?]ヘルプ (既定値は"N")。Y


```

The script will then begin to download and configure the Windows Server Container components. This process may take quite some time due to the large download. When finished your Virtual Machine will be configured and ready for you to create and manage Windows Server Containers and Windows Server Container Images with both PowerShell and Docker.  

You may receive the following message during the Window Server Container host deployment process. 

```

この VM は、ネットワークに接続されていません。 それを接続するには次の手順を実行します。
Get VM |Get VMNetworkAdapter |接続 VMNetworkAdapter Switchname の <switchname>


```
If you do, check the properties of the virtual machine and connect the virtual machine to a virtual switch. You can also run the following PowerShell command where `<switchname>` is the name of the Hyper-V virtual switch that you would like to connect to the virtual machine.

``` powershell 
Get-VM | Get-VMNetworkAdapter | Connect-VMNetworkAdapter -Switchname <switchname>

```

構成スクリプトの実行が完了すると、仮想マシンを起動します。
VM では、Windows Server 2016 core が構成されは、次のようになります。

<center>!(./media/containerhost2.png)</center><br />

最後に、構成プロセス中に指定したパスワードを使用して仮想マシンにログインし、バーチャル マシンが有効な IP アドレスを持つかどうかを確認します。
完了した次の項目を含む、システムは、Windows Server のコンテナーの準備ができてする必要があります。

##ビデオ チュートリアル

<iframe src="https://channel9.msdn.com/Blogs/containers/Quick-Start-Configure-Windows-Server-Containers-on-a-Local-System/player" width="800" height="450" allowFullScreen="true" frameBorder="0" scrolling="no" caps_internal_Id="0f9e3ac7-296f-45ec-9be4-6e903d571ccf" />

##次の手順 - は、コンテナーの使用を開始します。

Windows Server のコンテナーの機能を実行しているシステムがイメージを Windows のサーバーのコンテナーおよび Windows Server のコンテナーを作業を開始する、次のガイドにジャンプ、Windows Server 2016 をしたとします。

[クイック スタート:Windows Server のコンテナーと Docker](./manage_docker.md)

[クイック スタート:Windows Server のコンテナーと PowerShell](./manage_powershell.md)

-------------------

[コンテナーのホームに戻る](../containers_welcome.md)[現在のリリースに関する既知の問題](../about/work_in_progress.md)




