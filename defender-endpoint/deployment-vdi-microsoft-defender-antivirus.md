---
title: Configure Microsoft Defender Antivirus on a remote desktop or virtual desktop infrastructure environment
description: Get an overview of how to configure Microsoft Defender Antivirus in a remote desktop or non-persistent virtual desktop environment.
ms.localizationpriority: medium
ms.date: 10/28/2024
ms.topic: conceptual
author: denisebmsft
ms.author: deniseb
ms.custom: nextgen
ms.reviewer: jesquive
manager: deniseb
ms.subservice: ngp
ms.service: defender-endpoint
ms.collection:
- m365-security
- tier2
- ContentEngagementFY23
- mde-ngp
search.appverid: met150
---

# Configure Microsoft Defender Antivirus on a remote desktop or virtual desktop infrastructure environment

**Applies to:**

- Microsoft Defender Antivirus
- [Defender for Endpoint Plan 1](microsoft-defender-endpoint.md)
- [Defender for Endpoint Plan 2](microsoft-defender-endpoint.md)

**Platforms**

- Windows

This article is designed for customers who are using Microsoft Defender Antivirus capabilities only. If you have Microsoft Defender for Endpoint (which includes Microsoft Defender Antivirus alongside other device protection capabilities), also go through [Onboard non-persistent virtual desktop infrastructure (VDI) devices in Microsoft Defender XDR](configure-endpoints-vdi.md).

You can use Microsoft Defender Antivirus in a remote desktop (RDS) or non-persistent virtual desktop infrastructure (VDI) environment. Following the guidance in this article, you can configure updates to download directly to your RDS or VDI environments when a user signs in.

This guide describes how to configure Microsoft Defender Antivirus on your VMs for optimal protection and performance, including how to:

- [Set up a dedicated VDI file share for security intelligence updates](#set-up-a-dedicated-vdi-file-share-for-security-intelligence)
- [Randomize scheduled scans](#randomize-scheduled-scans)
- [Use quick scans](#use-quick-scans)
- [Prevent notifications](#prevent-notifications)
- [Disable scans from occurring after every update](#disable-scans-after-an-update)
- [Scan out-of-date machines or machines that were offline for a while](#scan-vms-that-have-been-offline)
- [Apply exclusions](#exclusions)

> [!IMPORTANT]
> Although a VDI can be hosted on Windows Server 2012 or Windows Server 2016, virtual machines (VMs) should be running Windows 10, version 1607 at a minimum, due to increased protection technologies and features that are unavailable in earlier versions of Windows.

## Set up a dedicated VDI file share for security intelligence

In Windows 10, version 1903, Microsoft introduced the shared security intelligence feature, which offloads the unpackaging of downloaded security intelligence updates onto a host machine. This method reduces the usage of CPU, disk, and memory resources on individual machines. Shared security intelligence now works on Windows 10, version 1703 and later. You can set up this capability by using Group Policy or PowerShell.

### Group Policy

1. On your Group Policy management computer, open the Group Policy Management Console, right-click the Group Policy Object you want to configure, and then select **Edit**.

2. In the Group Policy Management Editor, go to **Computer configuration**.

3. Select **Administrative templates**. Expand the tree to **Windows components** > **Microsoft Defender Antivirus** > **Security Intelligence Updates**.

4. Double-click **Define security intelligence location for VDI clients**, and then set the option to **Enabled**. 

   A field automatically appears.

5. Enter `\\<Windows File Server shared location\>\wdav-update` (for help with this value, see [Download and unpackage](#download-and-unpackage-the-latest-updates)).

6. Select **OK**, and then deploy the Group Policy Object to the VMs you want to test.

### PowerShell

1. On each RDS or VDI device, use the following cmdlet to enable the feature: 

   `Set-MpPreference -SharedSignaturesPath \\<Windows File Server shared location>\wdav-update`
   
2. Push the update as you normally would push PowerShell-based configuration policies onto your VMs. (See the [Download and unpackage](#download-and-unpackage-the-latest-updates) section in this article. Look for the *shared location* entry.) 

## Download and unpackage the latest updates

Now you can get started on downloading and installing new updates. We've created a sample PowerShell script for you below. This script is the easiest way to download new updates and get them ready for your VMs. You should then set the script to run at a certain time on the management machine by using a scheduled task (or, if you're familiar with using PowerShell scripts in Azure, Intune, or SCCM, you could also use those scripts).

```PowerShell
$vdmpathbase = "$env:systemdrive\wdav-update\{00000000-0000-0000-0000-"
$vdmpathtime = Get-Date -format "yMMddHHmmss"
$vdmpath = $vdmpathbase + $vdmpathtime + '}'
$vdmpackage = $vdmpath + '\mpam-fe.exe'

New-Item -ItemType Directory -Force -Path $vdmpath | Out-Null

Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' -OutFile $vdmpackage

Start-Process -FilePath $vdmpackage -WorkingDirectory $vdmpath -ArgumentList "/x"
```

You can set a scheduled task to run once a day so that whenever the package is downloaded and unpacked then the VMs receive the new update. We suggest starting with once a day, but you should experiment with increasing or decreasing the frequency to understand the impact.

Security intelligence packages are typically published once every three to four hours. Setting a frequency shorter than four hours isn't advisable because it increases the network overhead on your management machine for no benefit.

You can also set up your single server or machine to fetch the updates on behalf of the VMs at an interval and place them in the file share for consumption. This configuration is possible when the devices have share and read access (NTFS permissions) to the share so they can grab the updates. To set up this configuration, follow these steps:

1. Create an SMB/CIFS file share.

2. Use the following example to create a file share with the following share permissions.

   ```PowerShell
   
   PS c:\> Get-SmbShareAccess -Name mdatp$

   Name   ScopeName AccountName AccessControlType AccessRight
   ----   --------- ----------- ----------------- -----------
   mdatp$ *         Everyone    Allow             Read
   
   ```

   > [!NOTE]
   > An NTFS permission is added for **Authenticated Users:Read:**.

   For this example, the file share is `\\WindowsFileServer.fqdn\mdatp$\wdav-update`.
   
### Set a scheduled task to run the PowerShell script

1. On the management machine, open the Start menu and type `Task Scheduler`. From the results, select Task Scheduler and then select **Create task...** in the side panel.

2. Specify the name as `Security intelligence unpacker`. 

3. On the **Trigger** tab, select **New...** > **Daily**, and select **OK**.

4. On the **Actions** tab, select **New...**.

5. Specify `PowerShell` in the **Program/Script** field. 

6. In the **Add arguments**  field, type `-ExecutionPolicy Bypass c:\wdav-update\vdmdlunpack.ps1`, and then select **OK**.

7. Configure any other settings as appropriate.

8. Select **OK** to save the scheduled task.

To initiate the update manually, right-click on the task, and then select **Run**.

### Download and unpackage manually

If you would prefer to do everything manually, here's what to do to replicate the script's behavior:

1. Create a new folder on the system root called `wdav_update` to store intelligence updates. For example, create the folder `c:\wdav_update`.

2. Create a subfolder under `wdav_update` with a GUID name, such as `{00000000-0000-0000-0000-000000000000}`

   Here's an example: `c:\wdav_update\{00000000-0000-0000-0000-000000000000}`

   > [!NOTE]
   > We set the script so that the last 12 digits of the GUID are the year, month, day, and time when the file was downloaded so that a new folder is created each time. You can change this so that the file is downloaded to the same folder each time.

3. Download a security intelligence package from [https://www.microsoft.com/wdsi/definitions](https://www.microsoft.com/wdsi/definitions)  into the GUID folder. The file should be named `mpam-fe.exe`.

4. Open a Command Prompt window and navigate to the GUID folder you created. Use the `/X` extraction command to extract the files. For example `mpam-fe.exe /X`.

   > [!NOTE]
   > The VMs will pick up the updated package whenever a new GUID folder is created with an extracted update package or whenever an existing folder is updated with a new extracted package.

## Randomize scheduled scans

Scheduled scans run in addition to [real-time protection and scanning](configure-real-time-protection-microsoft-defender-antivirus.md).

The start time of the scan itself is still based on the scheduled scan policy (**ScheduleDay**, **ScheduleTime**, and **ScheduleQuickScanTime**). Randomization causes Microsoft Defender Antivirus to start a scan on each machine within a four-hour window from the time set for the scheduled scan.

See [Schedule scans](schedule-antivirus-scans.md) for other configuration options available for scheduled scans.

## Use quick scans

You can specify the type of scan that should be performed during a scheduled scan. Quick scans are the preferred approach as they're designed to look in all places where malware needs to reside to be active. The following procedure describes how to set up quick scans using Group Policy.

1. In your Group Policy Editor, go to **Administrative templates** \> **Windows components** \> **Microsoft Defender Antivirus** \> **Scan**.

2. Select **Specify the scan type to use for a scheduled scan** and then edit the policy setting.

3. Set the policy to **Enabled**, and then under **Options**, select  **Quick scan**.

4. Select **OK**.

5. Deploy your Group Policy object as you usually do.

## Prevent notifications

Sometimes, Microsoft Defender Antivirus notifications are sent to or persist across multiple sessions. To help avoid user confusion, you can lock down the Microsoft Defender Antivirus user interface. The following procedure describes how to suppress notifications using Group Policy.

1. In your Group Policy Editor, go to **Windows components** \> **Microsoft Defender Antivirus** \> **Client Interface**.

2. Select **Suppress all notifications** and then edit the policy settings.

3. Set the policy to **Enabled**, and then select **OK**.

4. Deploy your Group Policy object as you usually do.

Suppressing notifications prevents notifications from Microsoft Defender Antivirus from showing up when scans are done or remediation actions are taken. However, your security operations team sees the results of a scan if an attack is detected and stopped. Alerts, such as an initial access alert, are generated, and appear in the [Microsoft Defender portal](https://security.microsoft.com).

## Disable scans after an update

Disabling a scan after an update prevents a scan from occurring after receiving an update. You can apply this setting when creating the base image if you have also run a quick scan. This way, you can prevent the newly updated VM from performing a scan again (as you've already scanned it when you created the base image).

> [!IMPORTANT]
> Running scans after an update helps ensure your VMs are protected with the latest security intelligence updates. Disabling this option reduces the protection level of your VMs and should only be used when first creating or deploying the base image.

1. In your Group Policy Editor, go to **Windows components** \> **Microsoft Defender Antivirus** \> **Security Intelligence Updates**.

2. Select **Turn on scan after security intelligence update** and then edit the policy setting.

3. Set the policy to **Disabled**.

4. Select **OK**.

5. Deploy your Group Policy object as you usually do.

This policy prevents a scan from running immediately after an update.

## Disable the `ScanOnlyIfIdle` option

Use the following cmdlet, to stop a quick or scheduled scan whenever the device goes idle if it is in passive mode.

```PowerShell
Set-MpPreference -ScanOnlyIfIdleEnabled $false
```

You can also disable the `ScanOnlyIfIdle` option in Microsoft Defender Antivirus by configuration via local or domain group policy. This setting prevents significant CPU contention in high density environments.

For more information, see [Start the scheduled scan only when computer is on but not in use](https://admx.help/?Category=SystemCenterEndpointProtection&Policy=Microsoft.Policies.Antimalware::scan_scanonlyifidle).

## Scan VMs that have been offline

1. In your Group Policy Editor, go to **Windows components** \> **Microsoft Defender Antivirus** \> **Scan**.

2. Select **Turn on catch-up quick scan** and then edit the policy setting.

3. Set the policy to **Enabled**.

4. Select **OK**.

5. Deploy your Group Policy Object as you usually do.

This policy forces a scan if the VM missed two or more consecutive scheduled scans.

## Enable headless UI mode

1. In your Group Policy Editor, go to **Windows components** \> **Microsoft Defender Antivirus** \> **Client Interface**.

2. Select **Enable headless UI mode** and edit the policy.

3. Set the policy to **Enabled**.

4. Select **OK**.

5. Deploy your Group Policy Object as you usually do.

This policy hides the entire Microsoft Defender Antivirus user interface from end users in your organization.

## Run the "Windows Defender Cache Maintenance" scheduled task

Optimize the "Windows Defender Cache Maintenance" scheduled task for non-persistent and/or persistent VDI environments. Run this task on the main image before sealing.

1. Open up the **Task Scheduler** mmc (`taskschd.msc`).

2. Expand **Task Scheduler Library** > **Microsoft** > **Windows** > **Windows Defender**, and then right-click on **Windows Defender Cache Maintenance**.

3. Select **Run**, and let the scheduled task finish.

## Exclusions

If you think you need to add exclusions, see [Manage exclusions for Microsoft Defender for Endpoint and Microsoft Defender Antivirus](defender-endpoint-antivirus-exclusions.md).

## See also

- [Tech Community Blog: Configuring Microsoft Defender Antivirus for non-persistent VDI machines](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/configuring-microsoft-defender-antivirus-for-non-persistent-vdi/ba-p/1489633)
- [TechNet forums on Remote Desktop Services and VDI](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverTS)
- [SignatureDownloadCustomTask PowerShell script](https://www.powershellgallery.com/packages/SignatureDownloadCustomTask/1.4)

If you're looking for information about Defender for Endpoint on non-Windows platforms, see the following resources:

- [Microsoft Defender for Endpoint on Mac](microsoft-defender-endpoint-mac.md)
- [Microsoft Defender for Endpoint on Linux](microsoft-defender-endpoint-linux.md)
- [Configure Defender for Endpoint on Android features](android-configure.md)
- [Configure Microsoft Defender for Endpoint on iOS features](ios-configure-features.md)

[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../includes/defender-mde-techcommunity.md)]
