---
title: What's new in Microsoft Defender for Endpoint on Android
description: Learn about the major changes for previous versions of Microsoft Defender for Endpoint on Android.
ms.service: defender-endpoint
ms.author: siosulli
author: siosulli
ms.localizationpriority: medium
manager: deniseb
audience: ITPro
ms.collection:
- m365-security
- tier3
- mde-android
ms.topic: reference
ms.subservice: android
search.appverid: met150
ms.date: 03/04/2024
---

# What's new in Microsoft Defender for Endpoint on Android

[!INCLUDE [Microsoft Defender XDR rebranding](../includes/microsoft-defender.md)]

**Applies to:**
- [Microsoft Defender for Endpoint](microsoft-defender-endpoint.md)
- [Microsoft Defender XDR](/defender-xdr)

Want to experience Microsoft Defender for Endpoint? [Sign up for a free trial.](https://signup.microsoft.com/create-account/signup?products=7f379fee-c4f9-4278-b0a1-e4c8c2fcdf7e&ru=https://aka.ms/MDEp2OpenTrial?ocid=docs-wdatp-exposedapis-abovefoldlink)


## Device Tagging

Mobile Device Tagging is now generally available. This feature enables bulk tagging the mobile devices by allowing the admins to set up tags via Intune. Admin can configure the device tags through Intune via configuration policies and push them to user's devices. Once the user installs and activates Defender, the client app passes the device tags to the Security Portal. The Device tags appear against the devices in the Device Inventory.

This configuration is available for both the enrolled (MDM) devices and unenrolled (MAM) devices. For more information, see [Device Tagging (MDM)](android-configure.md#device-tagging) and [Device Tagging (MAM)](android-configure-mam.md#device-tagging).

## Microsoft Defender for Endpoint on Company-owned personally enabled devices

MDE is now generally available on AE COPE devices. Enterprises can onboard devices on COPE mode and push MDE to user's devices through the [Microsoft Intune admin center](https://go.microsoft.com/fwlink/?linkid=2109431). With this support, Android Enterprise COPE devices get the full capabilities of our offering on Android, including:

- Phishing and web protection.
- Malware scanning.
- Network protection (preview).
- Additional breach prevention through integration with Microsoft Intune and Conditional Access.

Read the announcement [here](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/microsoft-defender-for-endpoint-is-now-available-on-android/ba-p/3626100).

## Privacy Controls

Microsoft Defender for Endpoint on Android enables Privacy Controls for both the Admins and the End Users. This includes the controls for enrolled (MDM) and unenrolled (MAM) devices. Admins can configure the privacy in the alert report while End Users can configure the information shared to their organization. For more information, see [privacy controls(MDM)](android-configure.md#privacy-controls) and [privacy controls (MAM)](android-configure-mam.md#configure-privacy-controls).

## Optional Permissions and Disable Web Protection

Microsoft Defender for Endpoint on Android enables **Optional Permissions** in the onboarding flow. Currently the permissions required by MDE are mandatory in the onboarding flow. With this feature, admin can deploy MDE on devices without enforcing the mandatory **VPN** and **Accessibility** permissions during onboarding. End Users can onboard the app without the mandatory permissions and can later review these permissions. This feature is currently present only for unenrolled devices (MAM). For more information, see [optional permissions](android-configure-mam.md#optional-permissions).

## Microsoft Defender on Android enterprise BYOD personal profile

Microsoft Defender for Endpoint is now supported on Android Enterprise personal profile (BYOD only) with all the key features including malware scanning, protection from phishing links, network protection and vulnerability management. This support is coupled with [privacy controls](android-configure.md#privacy-controls) to ensure user privacy on personal profile. For more information, read the [announcement](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/announcing-the-public-preview-of-defender-for-endpoint-personal/ba-p/3370979) and the [deployment guide](android-intune.md#set-up-microsoft-defender-in-personal-profile-on-android-enterprise-in-byod-mode).

## Network protection

Network Protection on Microsoft Defender for Endpoint is now available. Network protection provides protection against rogue Wi-Fi related threats, rogue hardware like pineapple devices and notifies the user if a related threat is detected. Users also see a guided experience to connect to secure networks and change networks when they're connected to an unsecure connection.
> [!IMPORTANT]
> Network protection feature will soon be enabled by default for all users. The update will be rolled out in a phased manner. As a result, users will be able to see Network Protection Card in the Defender for Endpoint iOS app along with App Protection and Web Protection. Users are also required to provide Location permission to complete the set up. Admins can change the default value for the Network Protection feature if they decide not to use it via the Intune App Configuration policies. There are also several admin controls to offer flexibility, including privacy controls to configure the data that's sent by Defender for Endpoint from Android devices. For more information, see [network protection](android-configure.md).


> [!NOTE]
> Microsoft Defender is no longer supported for versions 1.0.3011.0302 or earlier. Users are requested to upgrade to latest versions to keep their devices secure.

To update, users can use the following steps:

> 1. On your work profile, go to Managed Play Store.
> 2. Tap on the profile icon on the top right corner and select "Manage apps and device".
> 3. Locate MDE under updates available and select update.
> If you encounter any issues, [submit in-app feedback](android-support-signin.md#send-in-app-feedback).

## Microsoft Defender for Endpoint is now Microsoft Defender in the Play store

Microsoft Defender for Endpoint is now available as **Microsoft Defender** in the play store. With this update, the app is available as preview for **Consumers in the US region**. Based on how you log into the app with your work or personal account, you have access to features for Microsoft Defender for Endpoint or for Microsoft Defender for individuals. For more information, see [this blog](https://www.microsoft.com/microsoft-365/microsoft-defender-for-individuals).

## Vulnerability Management

On January 25, 2022, we announced the general availability of Vulnerability management on Android and iOS. For more information, see [the techcommunity post here](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/announcing-general-availability-of-vulnerability-management/ba-p/3071663).

## Upcoming permission changes for Microsoft Defender for Endpoint running Android 11 or later (Nov 2021)

Release Build: 1.0.3501.0301
Release month: Nov 2021
Microsoft Defender for Endpoint has released this update required by [Google](https://developer.android.com/distribute/play-policies#APILevel30) to upgrade to Android API 30. This change prompts users seeking access to [new storage permission](https://developer.android.com/training/data-storage/manage-all-files#all-files-access-google-play), for devices running Android 11 or later. Users need to accept this new storage permission once they update Defender app with the release build 1.0.3501.0301 or later. This update ensures that Defender for Endpoint's app security feature to function without any disruption. For more information, review the following sections.

**How will this affect your organization:** These changes take effect if you're using Microsoft Defender for Endpoint on devices running Android 11 or later and updated Defender for Endpoint to release build 1.0.3501.0301 or later.

> [!NOTE]
> The new storage permissions cannot be configured by admin to 'Auto Approve' through Microsoft Intune. User will need to take action to provide access to this permission.

- **User experience:** Users receive a notification indicating a missing permission for app security. If the user denies this permission, the 'App security' functionality is turned off on the device. If user doesn't accept or deny permission, they'll continue to receive the prompt when unlocking their device or opening the app, until it has been approved.

> [!NOTE]
> If your organization is previewing 'Tamper protection' feature and if the new storage permissions are not granted by the user within 7 days of updating to the latest version, the user might lose access to corporate resources.

**What you need to do to prepare:**

Notify your users and helpdesk (as applicable) that users will need to accept the new permissions when prompted after they have updated Defender for Endpoint to build 1.0.3501.0301 or later version. To accept the permissions, users should:

1. Tap on the Defender for Endpoint in-app notification or open the Defender for Endpoint app. Users see a screen that lists the permissions needed. A green check mark is missing next to the Storage permission.

2. Tap **Begin**.

3. Tap the toggle for **Allow access to manage all files.**

4. The device is now protected.

  > [!NOTE]
  > This permission allows Microsoft Defender for Endpoint to access storage on user's device, which helps detect and remove malicious and unwanted apps. Microsoft Defender for Endpoint accesses/scans Android app package file (.apk) only. On devices with a Work Profile, Defender for Endpoint only scans work-related files.

[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../includes/defender-mde-techcommunity.md)]
