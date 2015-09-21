ms。ContentId: e586a11a-870f-403b-8af8-4c2931589d26
タイトル:Windows PowerShell のダイレクトとの管理します。 

#Windows PowerShell のダイレクトとの管理します。

PowerShell のダイレクトを使用すると、Windows 10 または Windows Server テクニカル プレビュー HYPER-V ホストから 10 の Windows または Windows Server に関するテクニカル プレビューの仮想マシンをリモートで管理します。
PowerShell のダイレクトでは、ネットワークの構成またはいずれかの HYPER-V ホスト上のリモート管理の設定に関係なく、virtual machine または仮想マシン内の PowerShell の管理ができます。
これにより、HYPER-V の管理者を自動化し、仮想マシン管理と構成スクリプトを作成しやすくなります。

直接の PowerShell を実行する 2 つの方法はあります。  

*   作成し、PSSession コマンドレットを使用して、直接の PowerShell セッションを終了します。
*   Invoke-command コマンドレットにスクリプトまたはコマンドを実行します。

古い仮想マシンを管理している場合は、仮想マシン接続 (VMConnect) を使用しますまたは。 [仮想マシンの仮想ネットワークを構成します。](http://technet.microsoft.com/library/cc816585.aspx)。

##作成し、PSSession コマンドレットを使用して、直接の PowerShell セッションを終了します。

1.  HYPER-V ホストでは、Windows PowerShell を管理者として開きます。
2.  バーチャル マシンの名前または GUID を使用してセッションを作成するには、次のコマンドのいずれかを実行します。
    '' PowerShell
    入力 PSSession-VMName < VMName >
    < VMGUID > を入力して PSSession VMGUID


```

4. Run whatever commands you need to. These commands run on the virtual machine that you created the session with.
5. When you're done, run the following command to close the session:  
``` PowerShell
Exit-PSSession 

```


> 注: ならセッションは接続しないを確認してください - HYPER-V ホストではなくに接続する仮想マシンの資格情報を使用しています。
> 

これらのコマンドレットの詳細については、次を参照してください。 [Enter-PSSession](http://technet.microsoft.com/library/hh849707.aspx) 」と「 [Exit-PSSession](http://technet.microsoft.com/library/hh849743.aspx)。

##Invoke-command コマンドレットにスクリプトまたはコマンドを実行します。

このデータを取得するには、 [Invoke-Command](http://technet.microsoft.com/library/hh849719.aspx) 仮想マシンで事前に決められた一連のコマンドを実行するコマンドレットを使用します。
Invoke-command コマンドレットを使用する方法の例を次に示します PSTest が仮想マシンの名前であり、(foo.ps1) を実行するスクリプトが、スクリプトのフォルダー、c:/ドライブします。

 '' PowerShell
Invoke-command VMName PSTest - FilePath C:\script\foo.ps1 


 ```

To run a single command, use the **-ScriptBlock** parameter:

 ``` PowerShell
 Invoke-Command -VMName PSTest -ScriptBlock { cmdlet } 

 ```


##PowerShell を使用して何が必要なでしょうか。

バーチャル マシンの場合は、直接の PowerShell セッションを作成するには

*   仮想マシンでは、ホスト上でローカルに実行する必要があり、起動します。
*   HYPER-V の管理者として、ホスト コンピューターには、ログインする必要があります。
*   仮想マシンの有効なユーザーの資格情報を指定する必要があります。
*   ホストのオペレーティング システムには、10 の Windows、Windows サーバーに関するテクニカル プレビュー、または以降のバージョンを実行する必要があります。
*   仮想マシンには、10 の Windows、Windows サーバーに関するテクニカル プレビュー、または以降のバージョンを実行する必要があります。

このデータを取得するには、 [Get-VM](http://technet.microsoft.com/library/hh848479.aspx) コマンドレットを使用する、資格情報を使用しているが、HYPER-V の管理者ロールにあることを確認して確認すると、Vm がホストにローカルで実行しているし、起動、します。

##PowerShell のダイレクトでは、何をことができますか。

記憶域スペース構成からすべてを完全に消去するときに役立つスクリプトについては、「 [PowerShell の直接のスニペット](../develop/powershell_snippets.md) PowerShell を使用した HYPER-V のスクリプトを記述するため、環境だけでなくヒントとテクニックで PowerShell ダイレクトを使用する方法のさまざまな例についてはします。


