ms。ContentId:7561B149-A147-4F71-9840-6AE149B9DED5
タイトル:サポートされている Windows ゲスト オペレーティング システム

#サポートされている Windows ゲスト

この記事では、Windows では、HYPER-V でゲストとしてサポートされている Windows オペレーティング システムの一覧も統合サービスに関する情報を提供します。

> Windows 10 は、Windows 8.1 と Windows Server 2012 R2 HYPER-V ホストでは、ゲスト オペレーティング システムとして実行されます。
> 

##平均値がサポートする新機能

サポートするには、マイクロソフトがこれらのホストとゲストの組み合わせをテストしたことを意味します。
これらの組み合わせに関する問題は、製品サポート サービスからの注意を受信する可能性があります。

マイクロソフトでは、次のように、ゲスト オペレーティング システムのサポートを提供します。

*   Microsoft のオペレーティング システムと統合サービスの問題は、Microsoft サポートによってサポートされます。
*   オペレーティング システム ベンダーによって Hyper-V の実行が認定されている、他のオペレーティング システムで検出された問題については、ベンダーによってサポートが提供されます。
*   他のオペレーティング システムで見つかった問題については、Microsoft には、マルチ ベンダー サポート コミュニティに問題が送信します。 [製造元](http://www.tsanet.org/)。

##統合サービスは、理由は、重要なのでしょうか。

HYPER-V には、サポートされているゲスト オペレーティング システム用の統合サービスが含まれています。
Integration services には、ホスト システムと仮想マシン間の相互作用が向上します。

(異なるバージョンの Windows を含む)、一部のオペレーティング システムに組み込まれており、統合サービスがあるは、Windows Update を通じて統合サービスを提供します。

##サポートされている Windows Server のゲスト オペレーティング システム

10 の Windows 対応の HYPER-V ホスト用。

| ゲスト オペレーティング システム| 仮想プロセッサの最大数| Integration Services| メモ| |
| -----                                | -----                                     | -----                     | -----     | ----- |
| Windows Server Technical Preview| 64| 組み込み| | | |
| Windows Server 2012 R2| 64| 組み込み| | | |
| Windows Server 2012| 64| 組み込み| | | |
| Windows Server 2008 R2 Service Pack 1 (SP 1)| 64| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをインストールします。| Datacenter、Enterprise、Standard、および Web Edition。| |
| Windows Server 2008 Service Pack 2 (SP 2)| 4| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをインストールします。| Datacenter、Enterprise、Standard、および Web Edition (32 ビットおよび 64 ビット)。| |
| Windows Home Server 2011| 4| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをインストールします。| |
| Windows Small Business Server 2011| Essentials edition - 2, Standard edition - 4| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをインストールします。| Essentials および Standard Edition。| |

##サポートされている Windows ゲスト オペレーティング システム

10 の Windows 対応の HYPER-V ホスト用。

| ゲスト オペレーティング システム| 仮想プロセッサの最大数| 統合サービス| メモ| |
| ----- | ----- | ----- | ----- | ----- |
| Windows 10| 32| 組み込み| | |
| Windows 8.1| 32| 組み込み| | |
| Windows 8| 32| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをアップグレードします。| | |
| Windows 7 Service Pack 1 (SP 1)| 4| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをアップグレードします。| Ultimate、Enterprise、および Professional Edition (32 ビットおよび 64 ビット)。| |
| Windows 7| 4| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをアップグレードします。| Ultimate、Enterprise、および Professional Edition (32 ビットおよび 64 ビット)。| |
| Windows Vista Service Pack 2 (SP2)| 2 で保護されたプロセスとして起動されました| 仮想マシンでオペレーティング システムをセットアップした後で、統合サービスをインストールします。| Business、Enterprise、および Ultimate (N および KN エディションを含む)。| |
 Windows 10 では、Windows 8.1 と Windows Server 2012 R2 HYPER-V ホスト上のゲスト オペレーティング システムです。

##Linux および無料の BSD

10 の Windows 対応の HYPER-V ホスト用。

| ゲスト オペレーティング システム| |
| -----|------|
| [CentOS、Red Hat Enterprise Linux ](https://technet.microsoft.com/library/dn531026.aspx)| |
| [Debian の仮想マシンを HYPER-V で](https://technet.microsoft.com/library/dn614985.aspx)| |
| [SUSE](https://technet.microsoft.com/en-us/library/dn531027.aspx)| |
| [Oracle Linux](https://technet.microsoft.com/en-us/library/dn609828.aspx)| |
| [Ubuntu](https://technet.microsoft.com/en-us/library/dn531029.aspx)| |
| [FreeBSD](https://technet.microsoft.com/library/dn848318.aspx)| |
これは、特定のカーネルのバージョンに関する注意事項があります。
詳細については、HYPER-V の過去のバージョンのサポートの情報も含めて、次を参照してください。 [Linux および Hyper V 上のバーチャル マシンの FreeBSD](https://technet.microsoft.com/library/dn531030.aspx)。


