ms。ContentId:52DAFFBE-40F5-46D2-96F3-FB8659581594
タイトル:Windows の 10 の HYPER-V の新機能

#10 の Windows で HYPER-V の新機能します。

このトピックでは、10 の Windows ® で HYPER-V の追加または変更の機能について説明します。

##Windows PowerShell のダイレクト

ここで、簡単かつ信頼性の高い方法は、ホスト オペレーティング システムから仮想マシン内の Windows PowerShell コマンドを実行します。
ネットワークまたはファイアウォールの要件や特別な構成はありません。
これは、リモート管理の構成に関係なく動作します。
これを使用するには、ホストとバーチャル マシンのゲスト オペレーティング システムで Windows 10 または Windows Server に関するテクニカル プレビューを実行する必要があります。

ダイレクトの PowerShell セッションを作成するには、次のコマンドのいずれかを使用します。

'' PowerShell
入力 PSSession-VMName VMName
VMName を Invoke-command VMName ScriptBlock {コマンド。


```

Today, Hyper-V administrators rely on two categories of tools for connecting to a virtual machine on their Hyper-V host:
- Remote management tools such as PowerShell or Remote Desktop
- Hyper-V Virtual Machine Connection (VM Connect)

Both of these technologies work well, but each have trade-offs as your Hyper-V deployment grows. VMConnect is reliable, but it can be hard to automate. Remote PowerShell is powerful, but can be difficult to setup and maintain. 

Windows PowerShell Direct provides a powerful scripting and automation experience with the simplicity of VMConnect. Because Windows PowerShell Direct runs between the host and virtual machine, there is no need for a network connection or to enable remote management. You do need guest credentials to log into the virtual machine.

#### Requirements
- You must be connected to a Windows 10 or Windows Server Technical Preview host with virtual machines that run Windows 10 or Windows Server Technical Preview as guests.
- You need to be logged in with Hyper-V Administrator credentials on the host.
- You need User credentials for the virtual machine.
- The virtual machine you want to connect to must be running and booted.


## Hot add and remove for network adapters and memory

You can now add or remove a Network Adapter while the virtual machine is running, without downtime. This works for generation 2 virtual machines running both Windows and Linux operating systems. 

You can also adjust the amount of memory assigned to a virtual machine while it's running, even if you haven’t enabled Dynamic Memory. This works for both generation 1 and generation 2 virtual machines.

## Production checkpoints

Production checkpoints allow you to easily create “point in time” images of a virtual machine, which can be restored later on in a way that is completely supported for all production workloads. This is achieved by using backup technology inside the guest to create the checkpoint, instead of using saved state technology. For production checkpoints, the Volume Snapshot Service (VSS) is used inside Windows virtual machines. Linux virtual machines flush their file system buffers to create a file system consistent checkpoint. If you want to create checkpoints using saved state technology, you can still choose to use standard checkpoints for your virtual machine. 


> **Important:** The default for new virtual machines will be to create production checkpoints with a fallback to standard checkpoints. 


## Hyper-V Manager improvements

- **Alternate credentials support** – You can now use a different set of credentials in Hyper-V manager when you connect to another Windows 10 Technical Preview remote host. You can also save these credentials so it's easier to log on later. 

- **Down-level management** - You can now use Hyper-V manager to manage more versions of Hyper-V. With Hyper-V manager in Windows 10 Technical Preview, you can manage computers running Hyper-V on Windows Server 2012, Windows 8, Windows Server 2012 R2 and Windows 8.1.

- **Updated management protocol** - Hyper-V manager has been updated to communicate with remote Hyper-V hosts using the WS-MAN protocol, which permits CredSSP, Kerberos or NTLM authentication. When you use CredSSP to connect to a remote Hyper-V host, it allows you to perform a live migration without first enabling constrained delegation in Active Directory. WS-MAN-based infrastructure also simplifies the configuration necessary to enable a host for remote management. WS-MAN connects over port 80, which is open by default.


## Connected Standby works 

When Hyper-V is enabled on a computer that uses the Always On/Always Connected (AOAC) power model, the Connected Standby power state is now available.

In Windows 8 and 8.1, Hyper-V caused computers that used the Always On/Always Connected (AOAC) power model (also known as InstantON) to never sleep. See this [KB article](
https://support.microsoft.com/en-us/kb/2973536) for a full discription.


## Linux secure boot 

More Linux operating systems, running on generation 2 virtual machines, can now boot with the secure boot option enabled.  Ubuntu 14.04 and later, and SUSE Linux Enterprise Server 12, are enabled for secure boot on hosts that run the Technical Preview. Before you boot the virtual machine for the first time, you must specify that the virtual machine should use the Microsoft UEFI Certificate Authority.  At an elevated Windows Powershell prompt, type:

    Set-VMFirmware vmname -SecureBootTemplate MicrosoftUEFICertificateAuthority

For more information on running Linux virtual machines on Hyper-V, see [Linux and FreeBSD Virtual Machines on Hyper-V](http://technet.microsoft.com/library/dn531030.aspx).


## Virtual Machine Configuration Version 

When you move or import a virtual machine to a host running Hyper-V on Windows 10 from host running Windows 8.1, the virtual machine’s configuration file isn't automatically upgraded. This allows the virtual machine to be moved back to a host running Windows 8.1. You won't have access to new virtual machine features until you manually update the virtual machine configuration version. 

The virtual machine configuration version represents what version of Hyper-V the virtual machine’s configuration, saved state, and snapshot files it's compatible with. Virtual machines with configuration version 5 are compatible with Windows 8.1 and can run on both Windows 8.1 and Windows 10. Virtual machines with configuration version 6 are compatible with Windows 10 and won't run on Windows 8.1.

#### How do I check the configuration version of the virtual machines running on Hyper-V? 

From an elevated command prompt, run the following command:

``` PowerShell
Get-VM * | Format-Table Name, Version

```


####仮想マシンの構成バージョンをアップグレードする方法

管理者特権での Windows PowerShell コマンド プロンプトから次のコマンドのいずれかを実行します。

'' PowerShell
更新プログラム-VmConfigurationVersion < vmname >


```

Or

``` PowerShell
Update-VmConfigurationVersion <vmobject>

```

** 重要: **

*   仮想マシンの構成バージョンをアップグレードした後は、Windows 8.1 を実行しているホストにバーチャル マシンを移動できません。
*   バージョン 5 に 6 のバージョンから仮想マシンの構成のバージョンにダウン グレードすることはできません。
*   仮想マシンの構成をアップグレードするバーチャル マシンをオフにする必要があります。
*   アップグレード後、仮想マシンは、新しい構成ファイルの形式を使用します。
    詳細については、新しいバーチャル マシン構成ファイルの形式を参照してください。

##新しいバーチャル マシン構成ファイルの形式

仮想マシンでは、仮想マシンの構成データの読み書きの効率を上げるために設計された新しい構成ファイルの形式があるようになりました。
記憶域の障害が発生した場合は、データの破損が発生する可能性を減らすために設計もいます。
新しい構成ファイルを使用します。仮想マシンの構成データの拡張機能を VMCX とします。実行時の状態データの VMRS 拡張機能。

> **重要:** 」を参照します。VMCX ファイルは、バイナリ形式です。
> 直接編集します。VMCX またはします。VMRS ファイルはサポートされていません。
> 

##Integration Services は、Windows Update を通じて配信

Windows ゲストの integration services への更新プログラムは Windows Update を通じて分散ようになりました。

(Integration services とも呼ばれます) の統合コンポーネントは、これにより、ホスト オペレーティング システムとの通信にバーチャル マシン代理のドライバーのセットです。
これらは、サービスのゲストのファイル コピー ～ 同期時間の範囲を制御します。
統合コンポーネントのインストールおよびアップグレードの処理中に大きな問題点をいることを検出する過去の年の顧客に説明してきました。

従来は、HYPER-V のすべての新しいバージョンでは、新しい統合コンポーネントに付属します。
HYPER-V ホストのアップグレードもの仮想マシンでの統合コンポーネントのアップグレードが必要です。
新しい統合コンポーネントが HYPER-V ホストに付属し、vmguest.iso を使用して仮想マシンにインストールされているします。
このプロセスでは、仮想マシンを再起動するために必要な他の Windows の更新とバッチ処理できることができませんでした。
Vmguest.iso と仮想マシンの管理者は、それらをインストールする必要があるを提供する HYPER-V の管理者があるため、HYPER-V の管理者はこれは必ずしもそういうの仮想マシンで管理者資格情報を必要統合コンポーネントのアップグレードが必要です。

Windows の 10 および今後、すべての統合コンポーネントは、仮想に配信されますには、その他の重要な更新プログラムと Windows Update を通じて加工します。

更新プログラムを実行する仮想マシンの現在利用可能ながあります。

*   Windows Server 2012
*   Windows Server 2008 R2
*   Windows 8
*   Windows 7

仮想マシンは、Windows Update または WSUS サーバーに接続する必要があります。
統合コンポーネントの更新がカテゴリ ID の場合が今後、サポート技術情報として示されているこのリリースでは、します。

詳細は、［ 適用性を確認して方法には、これを表示 [ブログの投稿](http://blogs.technet.com/b/virtualization/archive/2014/11/24/integration-components-how-we-determine-windows-update-applicability.aspx)。

記憶域スペース構成からすべてを完全に消去するときに役立つスクリプトについては、「 [このブログ](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/11/12/updating-integration-components-over-windows-update.aspx) integration services のインストールの詳細なチュートリアルについて投稿できます。

> **重要:** ISO イメージ ファイルの vmguest.iso は統合コンポーネントを更新する必要ありません。
> これが 10 の Windows では、HYPER-V で含まれていません。
> 

##次の手順

[10 の Windows での HYPER-V を紹介します。](..\quick_start\walkthrough.md)


