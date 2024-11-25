---
title: What's new in Microsoft Defender for IoT in the Defender portal
description: This article describes new features available in Microsoft Defender for IoT in the Defender portal, including both OT and Enterprise IoT networks.
ms.topic: whats-new
ms.service: defender-for-iot
author: lwainstein
ms.author: lwainstein
ms.localizationpriority: medium
ms.date: 03/07/2024
ms.custom: enterprise-iot
---

# What's new in Microsoft Defender for IoT?

This article describes features available in Microsoft Defender for IoT in the Defender portal, across both OT and Enterprise IoT networks.

[!INCLUDE [defender-iot-preview](../includes//defender-for-iot-defender-public-preview.md)]

## November 2024

|Service area  |Updates  |
|---------|---------|
| **OT networks** | - [Secure site-linked devices in Microsoft Security Exposure Management Initiatives page](#secure-site-linked-devices-in-microsoft-security-exposure-management-initiatives-page) |

### Secure site-linked devices in Microsoft Security Exposure Management Initiatives page

You can now review the new **OT Security** initiative in the Microsoft Security Exposure Management **Initiatives** page. This new initiative provides a metric-driven way of tracking exposure about unmanaged OT devices.

:::image type="content" source="media/review-security-initiatives/ot-security-initiative.png" alt-text="Screenshot showing the OT Security initiative in Microsoft Defender for IoT in the Microsoft Defender portal." lightbox="media/review-security-initiatives/ot-security-initiative.png":::

This new initiative serves as a powerful tool to improve your OT site security posture. The initiative aims to monitor and safeguard OT environments within the organization by employing network layer monitoring. This initiative identifies devices and ensures that systems are working correctly, and data is protected.

For more information, see: 

- [Review security initiatives](review-security-initiatives.md)
- [Microsoft Security Exposure Management release notes](/security-exposure-management/whats-new#ot-security-initiative).

## September 2024

|Service area  |Updates  |
|---------|---------|
| **OT networks** | - [Review unmanaged enterprise IoT devices in Microsoft Security Exposure Management Initiatives page](#review-unmanaged-enterprise-iot-devices-in-microsoft-security-exposure-management-initiatives-page)<br>- [New Building Management Systems (BMS) device category](#new-building-management-systems-bms-device-category)|

### Review unmanaged enterprise IoT devices in Microsoft Security Exposure Management Initiatives page

You can now review the new **Enterprise IoT Security** initiative in the Microsoft Security Exposure Management **Initiatives** page. This new initiative provides a metric-driven way of tracking exposure about unmanaged enterprise IoT devices.

For more information, see the [Microsoft Security Exposure Management release notes](/security-exposure-management/whats-new#new-enterprise-iot-security-initiative).

### New Building Management Systems (BMS) device category

We now support the new BMS device category in Defender for IoT that improves BMS device discovery and security. The BMS category includes a subset of Smart Facility and Surveillance devices (previously under the IoT category) such as fire alarms, humidity sensors, security radars, etc. Camera devices remain under the IoT category.

For more information, see [overview of device discovery](device-discovery.md).

## July 2024

|Service area  |Updates  |
|---------|---------|
| **OT networks** | - [Site property added DeviceInfo schema](#new-site-property-added-deviceinfo-schema) |

### New Site property added DeviceInfo schema

In the advanced hunting tables, the **Site** property is added to the **DeviceInfo** schema. For more information, see [investigate threats](investigate-threats.md#advanced-hunting).

## Next steps

[Get started with Defender for IoT](get-started.md)
