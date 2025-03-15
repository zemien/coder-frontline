---
title: Delete user workspace from TFS server
date: 2021-02-28T09:00:00+13:00
featuredImg: /delete-user-workspace-from-tfs-server/close-up-of-pencils-and-notebook.webp
categories:
  - Programming
---

I'm here to share a useful technique to delete a TFS [^1] workspace that no longer exists locally but is still registered on the server. This was an extremely frustrating endeavour due to the lack of clear documentation, so I hope this helps someone! [Jump to solution](#delete-tfs-workspace-of-ex-employee)

Git may be the source versioning control of choice for most developers today, but centralized source control systems like TFS and Subversion are still used by many older repositories. That is the case in my company, where we have 15 year-old source code in TFS while newer repositories are in Azure DevOps (Git).

I was doing a repository clean-up and wanted to remove NuGet packages that were downloaded and checked-in. This was common practice back in the day of unreliable/slow Internet but is generally frowned upon these days because it bloats up the repository over time. Old packages are not automatically deleted by TFS when they're upgraded so they stay in the repo. Have you met many devs who lovingly curate their packages or node_modules folder by hand?

Sorry, digression over! Anyway, when I tried to check in my Delete operation it failed with the dreaded error message:

```terminal
Unable to perform operation on $/repo/Service/Packages. The item $/repo/Service/Packages/Utility 1.09/utility.dll is locked in workspace DEMO206;Admin - Carol Danvers
```

{{< 
  figure src="locked-workspace-error.webp" 
  title="Locked workspace error"
  alt="Visual Studio output window displaying a locked workspace error when deleting files from TFS" 
>}}
 
There are different solutions to this issue depending on your specific situation:

1. The user is still in the organisation and have their local environment: This is the easiest scenario to be in. Just ask the user to unlock the folder in Source Control Explorer.
2. The user is no longer in the organisation but their local environment still exists (i.e. their work laptop has not been wiped): You can ask IT to re-activate their account, log in as them, and unlock the folder. But it's easier to ask the TFS administrator to follow my steps below.
3. The user is no longer around and their local environment no longer exists (i.e. their work laptop has been wiped): My suggestion is to delete their entire workspace, which will also remove all locks that they owned. Ask the TFS administrator to follow my steps below.

## Delete TFS workspace of ex-employee
### Pre-requisites 
1. TFS administrator permissions
2. Windows environment with Visual Studio installed, in order to get TF.exe command line tools. A recent version of Visual Studio like 2017 or 2019 will do.

### Get list of TFS workspaces on the server
First, we want to get the list of TFS workspaces on the server so we have the exact names that we will need for the next step.

1. Launch Developer Command Line or Developer PowerShell for Visual Studio. This makes sure TF.exe is in the PATH.
2. Modify and run the following command, which outputs details of all workspaces registered on the TFS server into an XML file.
    ```terminal
    TF workspaces /collection:http://tfsvc:8080/tfs/DemoTeamProject /Owner:* /Computer:* /format:XML > c:\temp\workspaces.xml
    ```

This will result in an output like this in workspaces.xml:

```XML
  <Workspace computer="DEMO206" islocal="true" name="DEMO206" ownerdisp="Admin - Carol Danvers" 
  ownerid="S-1-5-22-2456812397-2423433832-269712057-11353" ownertype="System.Security.Principal.WindowsIdentity" 
  owner="4d74d864-53dc-4d11-8095-7ec8bb9121f6" owneruniq="4d74d864-53dc-4d11-8095-7ec8bb9121f6">
    <Comment />
    <Folders>
      <WorkingFolder local="C:\src\DemoProject" item="$/" />
    </Folders>
    <LastAccessDate>2019-03-28T08:42:13.39+13:00</LastAccessDate>
    <OwnerAliases>
      <string>DEMODOMAIN\a-cdanvers</string>
      <string>a-cdanvers</string>
      <string>Admin - Carol Danvers</string>
    </OwnerAliases>
  </Workspace>
```

### Delete workspace
Next, we will delete the user workspace with a simple command, but we may need to try various permutations of the username.

1. Launch Developer Command Line or Developer PowerShell again if you closed it.
2. Determine workspace name possibilities, which is usually a concatenation of the `name` element and one of the user identifiers. Referring to the sample XML above, the full workspace name could be one of the following:
    1. DEMO206;Admin - Carol Danvers `name;ownerdisp`
    2. DEMO206;S-1-5-22-2456812397-2423433832-269712057-11353 `name;ownerid`
    3. DEMO206;DEMODOMAIN\a-cdanvers `name;OwnerAliases`
    4. DEMO206;a-cdanvers `name;OwnerAliases`
3. Modify and run the command below, trying out one workspace name combination at a time.
    ```terminal
    tf workspace /server:http://tfsvc:8080/tfs/DemoTeamProject /delete " DEMO206;a- cdanvers"
    ```

If it's the wrong combination you will see error messages like this:
```terminal
TF14061: The workspace DEMO206;S-1-5-22-2456812397-2423433832-269712057-11353 does not exist.
TF14061: The workspace DEMO206;Admin - Carol Danvers does not exist.
```

If it's the right combination you will be prompted to confirm the deletion (and please do double-check!)

**Success!**

### References
I figured out the different workspace name combinations on my own. If your situation is unique you can refer to the following references or ask a question on StackOverflow.

* [tf workspaces command (Microsoft Docs)](https://docs.microsoft.com/en-us/azure/devops/repos/tfvc/workspaces-command?view=azure-devops-2020)
* [tf workspace command (Microsoft Docs)](https://docs.microsoft.com/en-us/azure/devops/repos/tfvc/workspace-command?view=azure-devops-2020)
* [Cleanup workspaces](https://almguide.net/2019/01/10/cleanup-workspaces/)

[^1]: I used "TFS" to refer to "TFVC" because TFS is commonly associated with TFVC. Actually, they are two separate things: TFS = Team Foundation Server is the server-based application that manages source control, work items, and pipelines. It is now known as Azure DevOps Server. TFVC = Team Foundation Version Control is the centralized source control system bundled with TFS. 