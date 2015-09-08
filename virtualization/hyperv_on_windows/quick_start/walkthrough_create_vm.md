ms.ContentId: 3C63F9A8-30E4-40F4-BC7B-A001C1E90779
title: Step 4: Create a Windows virtual machine from an .iso file

#Step 4: Create a Windows virtual machine from an .iso file

For this step, if you already have a .iso file for a supported 64-bit operating system, you can use that.
If not, you can download the .iso for [Windows 8.1 Enterprise](http://www.microsoft.com/en-us/evalcenter/evaluate-windows-8-1-enterprise) and choose the 64-bit edition.

1.  In Hyper-V Manager, click on the **Action** menu > **New** > **Virtual machine**.
2.  In the virtual machine wizard, make the following choices:
    
    <table>
      <tr>
        <th caps_internal_Id="99b6633b-89f4-46d4-8a41-2734f536d6aa">Page</th>
        <th caps_internal_Id="55dea354-4a48-4dc4-b6c7-d40c78ef9441">Entry</th>
      </tr>
      <tr>
        <td caps_internal_Id="10cbd3ca-8a75-4e90-888a-8087b009e8e7">Name:</td>
        <td>Type in <b caps_internal_Id="c0fb2cae-ffe5-4cc0-a9e6-c88d89d78a23">Windows Walkthrough VM</b></td>
      </tr>
      <tr>
        <td caps_internal_Id="73dace68-68bf-42ca-abd3-d9b1190ca218">Generation:</td>
        <td>
          <b caps_internal_Id="20012cfe-9c65-49d4-bbdb-5b15964f8d56">Generation 2</b>
        </td>
      </tr>
      <tr>
        <td caps_internal_Id="9f18c2b9-2122-4a43-80ab-5d5756add59b">Startup Memory:</td>
        <td>
          <b caps_internal_Id="a1bec76b-f4e2-4fc9-b8c4-219c292bda9a">1024</b> and leave dynamic memory selected</td>
      </tr>
      <tr>
        <td caps_internal_Id="2612821d-9bdc-4def-b3a6-69dba580b8d4">Configure Networking:</td>
        <td>
          <b caps_internal_Id="14b516d9-6516-44e9-994b-adff36c2b13a">External</b> (this is the virtual switch you created in Step 3)</td>
      </tr>
      <tr>
        <td caps_internal_Id="8917d87e-f580-4f51-86e0-b0882e2bd49a">Connect virtual hard disk:</td>
        <td>
          <b caps_internal_Id="f719f1ab-0555-4155-bbd8-c29b701debb9">Create a virtual hard disk</b> (keep the other default values) </td>
      </tr>
      <tr>
        <td caps_internal_Id="b35d4bf6-cce9-473c-836d-94df289d2e01">Installation Options:</td>
        <td>
          <b caps_internal_Id="eec0fd47-b85d-4a17-bbcc-2f510dc17128">Install an operating system from a bootable CD/DVD-ROM</b>. Under <b caps_internal_Id="5b94d02f-0cbf-4a32-8d9c-1d48fe09027d">Media</b>, select <b caps_internal_Id="3f9d1060-ed99-4f7b-9cc8-a636c94a6730">Image file (iso)</b> and then click <b caps_internal_Id="54c5a109-9ed5-44f7-810d-52a143d62577">Browse</b> to point to the .iso file.</td>
      </tr>
    </table>
3.  When everything looks right, click **Finish**.

> **Note:** If you only have 32-bit version of Windows, you need to choose Generation 1 in the **Generation** section of the wizard.
> Generation 2 VMs only support 64-bit operating systems.
> 

##Next Step:

[Step 5: Connect to the virtual machine and finish the installation](walkthrough_vmconnect.md)


