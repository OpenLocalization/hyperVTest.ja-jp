ms。ContentId:A6DD6776-614C-4D28-9B83-CB2EDFD263A3
タイトル:手順 2:10 の Windows での HYPER-V をインストールします。

#手順 2:10 の Windows での HYPER-V をインストールします。

使用して、HYPER-V をインストールすることができます。 [プログラムと機能](#UsingProgramsandFeatures) または [Windows PowerShell](#UsingPowerShell)。

##プログラムと機能を使用します。

1.  [AD トポロジ検索] を右クリックし、 **Windows** ボタンをクリック **プログラムと機能**。
    
    !(media\programs_and_features.png)
2.  [ **Windows の機能を有効または無効に**。
3.  選択 **Hyper-V**]、[ **OK**
    
    !(media\hyper-v_feature_selected.png)
4.  インストールが完了したら、コンピューターを再起動する必要があります。
    
    !(media\restart.png)

##PowerShell を使用します。

詳細については、PowerShell のヘルプを参照してください。 [Enable-windowsoptionalfeature](https://technet.microsoft.com/library/hh852172.aspx)。

1.  クリックして、 **Windows** ボタンを検索します **Windows PowerShell**。
2.  右クリックし **Windows PowerShell** クリックし、 **管理者として実行します。**。
3.  Windows Powerhshell プロンプトには、次を入力し、キーを押します、 **Enter** キー:  
    ` PowerShell
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
    `
4.  インストールが完了したら、コンピューターを再起動します。

##正しくインストールされている HYPER-V を知る方法をください。

した後に、コンピュータの起動、HYPER-V Manager ツールを再起動します。

1.  クリックして、 **Windows** 」と入力 **Hyper-V**。
2.  をクリックします。 **Hyper V マネージャー** リストします。
3.  HYPER-V マネージャーのアプリケーションを開始する必要があります。

##次の手順

[手順 3:仮想スイッチを作成する](walkthrough_virtual_switch.md)


