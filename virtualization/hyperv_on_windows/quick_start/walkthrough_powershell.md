ms。ContentId:B9414110-BEFD-423F-9AD8-AFD5EE612CDA
タイトル:手順 8:Windows PowerShell を使用したテストします。

#手順 8:Windows PowerShell を使用したテストします。

これで、HYPER-V を展開し、バーチャル マシンの作成および、これらの仮想マシンを管理するための基本事項を確認、PowerShell でこれらのアクティビティの多くを自動化する方法について説明します。

###HYPER-V のコマンドの一覧を返します

1.  [Windows の [スタート] ボタンの種類] をクリックします。 **PowerShell**。
2.  PowerShell コマンドと、HYPER-V の PowerShell モジュールで利用可能な検索可能な一覧を表示するには、次のコマンドを実行します。
    
    '' powershell
    get コマンドの場合 – モジュール hyper-v の概要 |アウト gridview


```
  You get something like this:

  ![](media\command_grid.png)

3. To learn more about a particular PowerShell command use `get-help`. For instance running the following command will return information about the `get-vm` Hyper-V command.

  ```powershell
get-help get-vm

```

 出力では、コマンド、必須およびオプションのパラメーターとは何か、および別名を使用することができますを構成する方法を示します。

!(media\get_help.png)

###仮想マシンの一覧を返します

「 `get-vm` 仮想マシンの一覧を取得するコマンドです。

1.  PowerShell では、次のコマンドを実行します。
    
    `powershell
    get-vm
    `
    これは、次のように何かが表示されます。
    
    !(media\get_vm.png)
2.  フィルターを追加するだけの電源投入時に仮想マシンの一覧を返すには `get-vm` コマンドなど) を指定します。
    Where オブジェクトのコマンドを使用して、フィルターを追加できます。
    フィルターの詳細については、次を参照してください、。 [は、Where-object を使用します。](https://technet.microsoft.com/en-us/library/ee177028.aspx) ドキュメントです。
    
    '' powershell
    get vm |ここで {$_ です。状態の – eq 'Running'}


 ```
3.  To list all virtual machines in a powered off state, run the following command. This command is a copy of the command from step 2 with the filter changed from ‘Running’ to ‘Off’.

 ```powershell
 get-vm | where {$_.State –eq ‘Off’}

 ```


###起動し、仮想マシンをシャット ダウン

1.  特定のバーチャル マシンを開始するには、仮想マシンの名前で、次のコマンドを実行します。
    
    '' powershell
    仮想マシンの開始-名前 < 仮想マシン名 >


 ```

2. To start all currently powered off virtual machines, get a list of those machines and pipe the list to the 'start-vm' command:

  ```powershell
 get-vm | where {$_.State –eq ‘Off’} | start-vm

 ```

1.  実行中のすべてのバーチャル マシンを停止するには、これを実行します。
    
    '' powershell
    get vm |ここで {$_ です。状態の – eq 'Running'}。仮想マシンの停止


 ```

### Create a VM checkpoint

To create a checkpoint using PowerShell, select the virtual machine using the `get-vm` command and pipe this to the `checkpoint-vm` command. Finally give the checkpoint a name using `-snapshotname`. The complete command will look like the following:

 ```powershell
 get-vm -Name <VM Name> | checkpoint-vm -snapshotname <name for snapshot>

 ```

たとえば、チェックポイント DEMOCP という名前の次に示します。

!(media\POSH_CP2.png)

###新しい仮想マシンを作成する

次の例では、PowerShell Integrated Scripting Environment (ISE) で、新しいバーチャル マシンを作成する方法を示します。
これは、単純な例であり、PowerShell の機能を追加しより高度な VM の配置を対象に拡張することです。

1.  起動時に PowerShell ISE のクリックを開くには、次のように入力します。 **PowerShell ISE**。
2.  仮想マシンを作成するには、次のコードを実行します。
    参照してください、 [New-VM](https://technet.microsoft.com/en-us/library/hh848537.aspx) 詳細については、新規 VM のコマンドは、ドキュメントです。
    
    '' powershell
    $VMName ="VMNAME"
    
    $VM = @{
    名前 = $VMName
    MemoryStartupBytes 2147483648 を =
    生成 = 2
    NewVHDPath ="C:\Virtual Machines\$VMName\$VMName.vhdx"
    NewVHDSizeBytes 53687091200 を =
    一 ="VHD"
    パス ="C:\Virtual Machines\$ VMName"
    SwitchName = (get vmswitch)。名前 [0]
    }
    
    新しい VM @VM
    ```

##ラップし、参照

このドキュメントはいくつかのサンプル シナリオと同様に、HYPER-V の PowerShell モジュールに、エクスプ ローラーにいくつかの簡単な手順を説明しました。
PowerShell HYPER-V モジュールの詳細については、次を参照してください、。 [Windows PowerShell での HYPER-V コマンドレットを参照します。](https://technet.microsoft.com/%5Clibrary/Hh848559.aspx)。


