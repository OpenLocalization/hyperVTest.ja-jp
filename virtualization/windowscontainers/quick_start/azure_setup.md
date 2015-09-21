ms。ContentId:1ab7bfe1-da35-4ff1-916f-936fedf536a0
タイトル:Azure での Windows のコンテナーをセットアップします。

#Microsoft Azure の Windows Server のコンテナーの準備

作成と Azure での Windows Server のコンテナーを管理する前に、Windows Server のコンテナーの機能が、あらかじめ設定されている Windows Server 2016 に関するテクニカル プレビュー イメージを展開する必要があります。
このガイドでは、このプロセスを説明します。

> その他のファースト ステップ ガイド。
> 

*   Windows Server のコンテナーを実行します。 [新しい HYPER-V 仮想マシン](./container_setup.md)。
*   Windows Server のコンテナーを実行します。 [既存のバーチャル マシン](./inplace_setup.md)
*   Windows Server のコンテナーをセットアップします。 [物理的な Windows Server Core インストール上](./inplace_setup.md)

##Azure ポータルの使用を開始します。

Azure アカウントがある場合は場合、は、すぐに省略します。 [コンテナーのホスト仮想マシンを作成します。](#CreateacontainerhostVM)。

1.  参照してください。 [azure.com](https://azure.com) 手順を実行して、 [Azure 無料評価版](https://azure.microsoft.com/en-us/pricing/free-trial/)。
2.  Microsoft アカウントでサインインします。
3.  サインイン アカウントは、準備ができましたが、ときに、 [Azure 管理ポータル](https://portal.azure.com)。

##コンテナーのホスト仮想マシンを作成します。

VM の作成プロセスを開始するには、次のリンクをクリックします。 [Azure での新しい Windows Server コンテナー ホスト](https://portal.azure.com/#gallery/Microsoft.WindowsServer2016TechnicalPreviewwithContainers)。

Azure のギャラリーのイメージを検索することもできます。

クリックして、 `create` ボタンをクリックします。

!(./media/newazure1.png)

により、仮想マシンの名前は、ユーザー名とパスワードを選択します。

!(media/newazure2.png)

オプションの構成の選択 > エンドポイント > 以下に示すように、プライベートおよびパブリック ポートは 80 の HTTP エンドポイントを入力します。
ときに完了した] をクリックには、2 回が [ok] です。

!(./media/newazure3.png)

選択します `create` 仮想マシンの展開プロセスを開始するボタンをクリックします。

!(media/newazure2.png)

VM の展開が完了すると、Windows Server のコンテナーのホストで RDP セッションを開始する [接続] ボタンを選択します。

!(media/newazure6.png)

ユーザー名と、VM の作成ウィザードの中に指定されたパスワードを使用して VM にログインします。
Windows のコマンド プロンプトで、そのでする検索は 1 回ログに記録します。

!(media/newazure7.png)

##ビデオ チュートリアル

<iframe src="https://channel9.msdn.com/Blogs/containers/Quick-Start-Configure-Windows-Server-Containers-in-Microsoft-Azure/player" width="800" height="450" allowFullScreen="true" frameBorder="0" scrolling="no" caps_internal_Id="5162a6c0-597a-4873-ab64-1e8374efa46c" />

##次の手順 - は、コンテナーの使用を開始します。

Windows Server のコンテナーの機能を実行しているシステムがイメージを Windows のサーバーのコンテナーおよび Windows Server のコンテナーを作業を開始する、次のガイドにジャンプ、Windows Server 2016 をしたとします。

[クイック スタート:Windows Server のコンテナーと Docker](./manage_docker.md)[クイック スタート:Windows Server のコンテナーと PowerShell](./manage_powershell.md)

-------------------
[Back to Container Home](../containers_welcome.md)  
[Known Issues for Current Release](../about/work_in_progress.md)


