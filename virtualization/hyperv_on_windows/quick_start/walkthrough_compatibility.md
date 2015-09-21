ms。ContentId:C2593EA1-B182-4C71-8504-49691F619158
タイトル:手順 1:コンピューターが互換性のあることを確認します。

#手順 1:コンピューターは、HYPER-V を実行できるかどうかを確認します。

のみ Windows 10 Pro および Windows 10 Enterprise は、HYPER-V をサポートします。

> Windows 10 Home--を実行している場合は、セキュリティ設定にあるライセンス認証] ページで 10 Pro の獲得にアップグレードすることができます。
> 「を格納する参照してください」リンクは、アップグレードするためのページにを見ていきます。
> 

HYPER-V では使用できません Windows 10 Mobile または Windows 10 モバイル Enterprise

HYPER-V には、少なくとも 4 GB の RAM が必要ですが、同時に複数の仮想マシンを実行する場合の詳細に必要な。

以降では、Windows 10 は、HYPER-V には、64 ビット プロセッサ Second Level Address Translation (SLAT) とが必要です。

##ハードウェアの互換性を確認します。

互換性を確認するには、PowerShell を開くまたは Windows コマンド プロンプト (cmd.exe) と種類。 `systeminfo.exe`。
お客様のコンピューターに関する情報が提供されます。

すべての下にある項目 **Hyper-V の要件** 場合に、値が必要 **はい**。

!(media\systeminfo.png)

関連するセクション:

*   `OS Name` --Windows 8 以上と職業または Enterprise のいずれかである必要があります。
*   `Hyper-V Requirements` -すべての必要があります ("Yes"の値) は true ですが、一部は、BIOS で構成できます。
    
    *   `VM Monitor Mode Extensions` -ハードウェアのプロパティです。
        このコンピューターで HYPER-V を実行できません。
    *   `Virtualization Enabled in Firmware` -BIOS で有効にすることができます。
    *   `Second Level Address Translation` -ハードウェアのプロパティです。
        このコンピューターで HYPER-V を実行できません。
    *   `Data Execution Prevention Available` -BIOS で有効にすることができます。

HYPER-V が既に有効なかどうか、HYPER-V の要件セクションが表示されます。  


```
Hyper-V Requirements: A hypervisor has been detected. Features required for Hyper-V will not be displayed.

```


##次の手順:

[手順 2:Hyper-V をインストールする](walkthrough_install.md)


