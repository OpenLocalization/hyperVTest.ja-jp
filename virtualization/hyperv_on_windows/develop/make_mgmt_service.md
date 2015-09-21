ms。ContentId:6C7EB25D-66FB-4B6F-AB4A-79D6BB424637
タイトル:新しい管理サービスを作成します。

#新しい管理サービスを作成します。

このドキュメントには、HYPER-V のソケットとそれを使用して開始する方法で構築された VM のサービスが導入されています。

##仮想マシン サービスとは何ですか。

ホー HB プロセスをテストするためには、この文を追加します。
VM のサービスは、HYPER-V ホストおよびホスト上で実行する仮想マシンにまたがるサービスです。

HYPER-V ようになりました (Windows 10 およびサーバー 2016年 +) を使用すると、サービスとホスト側のテナントの分離、コントロールに関連する HYPER-V の基本的な要件を維持しながら、ホストと仮想マシンの境界のまたがりメモリ割り当てを作成すると、ネットワークへの接続を提供すると共にとします。

HYPER-V では、(時間の同期) などの基本と、共通の要求は、ボックス内のサービス (integration services) の基本セットを提供する続行されますが、記述し、必要に応じて、仮想マシン サービスを展開だれでもようになりました。

PowerShell のダイレクトでは、仮想マシンのサービスのボックスの例を示します。

##HYPER-V のソケットとは何ですか。

HYPER-V ソケットは、ネットワークへの依存しない付きソケットの TCP と同様です。
HYPER-V のソケットを使用するには、サービスは、ネットワーク スタックとは別に実行でき、すべてのデータ フローは、ホストのメモリに保持します。

##システム要件

**サポートされているホスト OS**

*   Windows 10
*   Windows Server に関するテクニカル プレビュー 3
*   今後のリリース (サーバー 2016年 +)

**サポートされているゲスト OS**

*   Windows 10
*   Windows Server に関するテクニカル プレビュー 3
*   今後のリリース (サーバー 2016年 +)
*   Linux

##機能と制限事項

カーネル モードまたはユーザー モード
データのストリームのみ  
ブロックのメモリのバックアップ/ビデオに最適ではありません。 がありません。  

##はじめに

このガイドでは、ソケットの C と C++ でプログラミングに慣れていると仮定します。

###手順 1 - HYPER-V ホストにサービスを登録

HYPER-V と統合されて、カスタム サービスを使用するのには、HYPER-V ホストのレジストリと、新しいサービスを登録する必要があります。

によって、サービス レジストリに登録すると、次の操作を取得します。

*   WMI 管理を有効にする、無効化、および利用可能なサービスを一覧表示します。
*   サービスの一覧を使用して仮想マシンと直接通信できます。

** は、レジストリの場所と情報 **


```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\VirtualDevices\6C09BB55-D683-4DA0-8931-C9BF705F6480\GuestCommunicationServices\

```

このレジストリの場所では、いくつかの GUID が表示されます。
これらは、マイクロソフトのボックスでのサービスです。

サービスごとのレジストリ内の情報。

*   `Service GUID`
    
    *   `ElementName (REG_SZ)` -これは、サービスのフレンドリ名

独自のサービスを登録するには、独自の GUID と表示名を使用して新しいレジストリ キーを作成します。

レジストリ エントリは、次のようになります。


```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\GuestCommunicationServices\
    999E53D4-3D5C-4C3E-8779-BED06EC056E1\
        ElementName REG_SZ  VM Session Service
    YourGUID\
        ElementName REG_SZ  Your Service Friendly Name

```


> ** ヒント:: **  PowerShell で GUID の生成をクリップボードにコピーするには、次のコマンドを実行します。
> '' PowerShell
> [System.Guid]::NewGuid() です。ToString() |clip.exe
> 


```

<!-- How do customers know this worked -->

### Step 2 - Create a simple host-side service



### Step 3 - Create a simple guest-side service

## More information about AF_HYPERV
Since Hyper-V sockets do not depend on a networking stack, TCP/IP, DNS, etc. the socket end point needed a non-IP, not hostname, format that still describes the connection.  In lieu of an IP or hostname, AF_HYPERV endpoints rely heavily on two GUIDS:  
* VM ID – this is the unique ID assigned per VM.  A VM’s ID can be found using the following PowerShell snippet.
```PowerShell
(Get-VM -Name vmname).Id

```

*   サービス ID]: GUID をサービスが HYPER-V ホストのレジストリに登録されています。
    記憶域スペース構成からすべてを完全に消去するときに役立つスクリプトについては、「 [新しいサービスを登録します。](#GettingStarted)。

仮想マシン上のサービスをホスト上のサービスからの接続。
VMID とサービスの ID
ホスト上のサービスにバーチャル マシン上のサービスからの接続。
0 個の GUID とサービスの ID

##サポートされているソケット コマンド

Socket()
Bind()
() を接続します。
Send()
Listen()
Accept()


