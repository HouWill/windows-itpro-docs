---
title: Configure endpoints using Mobile Device Management tools
description: Use Mobile Device Management tools to deploy the configuration package on endpoints so that they are onboarded to the service.
keywords: configure endpoints using mdm, endpoint management, configure Windows ATP endpoints, configure Windows Defender Advanced Threat Protection endpoints, mdm
search.product: eADQiWindows 10XVcnh
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: mjcaparas
localizationpriority: high
---

# Configure endpoints using Mobile Device Management tools

**Applies to:**

- Windows 10 Enterprise
- Windows 10 Education
- Windows 10 Pro
- Windows 10 Pro Education
- Windows Defender Advanced Threat Protection (Windows Defender ATP)

You can use mobile device management (MDM) solutions to configure endpoints. Windows Defender ATP supports MDMs by providing OMA-URIs to create policies to manage endpoints.

For more information on using Windows Defender ATP CSP see, [WindowsAdvancedThreatProtection CSP](https://msdn.microsoft.com/library/windows/hardware/mt723296(v=vs.85).aspx) and [WindowsAdvancedThreatProtection DDF file](https://msdn.microsoft.com/library/windows/hardware/mt723297(v=vs.85).aspx).

## Configure endpoints using Microsoft Intune

For more information on using Windows Defender ATP CSP see, [WindowsAdvancedThreatProtection CSP](https://msdn.microsoft.com/library/windows/hardware/mt723296(v=vs.85).aspx) and [WindowsAdvancedThreatProtection DDF file](https://msdn.microsoft.com/library/windows/hardware/mt723297(v=vs.85).aspx).

### Onboard and monitor endpoints using the classic Intune console

1. Open the Microsoft Intune configuration package .zip file (*WindowsDefenderATPOnboardingPackage.zip*) that you downloaded from the service onboarding wizard. You can also get the package from the [Windows Defender ATP portal](https://securitycenter.windows.com/):

    a.  Select **Endpoint management** on the **Navigation pane**.

    b.  Select **Mobile Device Management/Microsoft Intune** > **Download package** and save the .zip file.

      ![Endpoint onboarding](images/atp-mdm-onboarding-package.png)

2. Extract the contents of the .zip file to a shared, read-only location that can be accessed by the network administrators who will deploy the package. You should have a file named  *WindowsDefenderATP.onboarding*.

3. Use the Microsoft Intune custom configuration policy to deploy the following supported OMA-URI settings. For more information on Microsoft Intune policy settings see, [Windows 10 policy settings in Microsoft Intune](https://docs.microsoft.com/en-us/intune/deploy-use/windows-10-policy-settings-in-microsoft-intune).

  a. Select **Policy** > **Configuration Policies** > **Add**.
  ![Microsoft Intune Configuration Policies](images/atp-add-intune-policy.png)

  b. Under **Windows**, select **Custom Configuration (Windows 10 Desktop and Mobile and later)** > **Create and Deploy a Custom Policy** > **Create Policy**. <br>
  ![Microsoft Intune Configuration Policies](images/atp-intune-new-policy.png)

  c. Type a name and description for the policy. <br>

  ![Microsoft Intune Create Policy](images/atp-intune-policy-name.png)

  d. Under OMA-URI settings, select **Add...**. <br>

  ![Microsoft Intune add OMC-URI](images/atp-intune-add-oma.png)

  e. Type the following values then select **OK**:

  ![Microsoft Intune save policy](images/atp-intune-oma-uri-setting.png)

  - **Setting name**: Type a name for the setting.
  - **Setting description**: Type a description for the setting.
  - **Data type**: Select **String**.
  - **OMA-URI**:  *./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Onboarding*
  - **Value**: Copy and paste the contents of the *WindowsDefenderATP.onboarding* file you downloaded.


  f. Save the policy.

  ![Microsoft Intune save policy](images/atp-intune-save-policy.png)

  g. Deploy the policy.

  ![Microsoft Intune deploy policy](images/atp-intune-deploy-policy.png)

  h. Select the device group to deploy the policy to:

  ![Microsoft Intune manage deployment](images/atp-intune-manage-deployment.png)

When the policy is deployed and is propagated, endpoints will be shown in the **Machines list**.

You can use the following onboarding policies to deploy configuration settings on endpoints. These policies can be sub-categorized to:
- Onboarding
- Health Status for onboarded machines
- Configuration for onboarded machines

Policy | OMA-URI | Type | Value | Description
:---|:---|:---|:---|:---
Onboarding | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Onboarding | String | Copy content from onboarding MDM file |  Onboarding
Health Status for onboarded machines: Sense Is Running | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/SenseIsRunning | Boolean | TRUE |  Windows Defender ATP service is running
Health Status for onboarded machines: Onboarding State | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OnBoardingState | Integer | 1 | Onboarded to Windows Defender ATP
Health Status for onboarded machines: Organization ID | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OrgId | String | Use OrgID from onboarding file | Onboarded to Organization ID
Configuration for onboarded machines  | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Configuration/SampleSharing | Integer | 0 or 1 <br> Default value: 1 | Windows Defender ATP Sample sharing is enabled
Configuration for onboarded machines: telemetry reporting frequency | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Configuration/TelemetryReportingFrequency | Integer | 1 or 2 <br> 1: Normal (default)<br><br> 2: Expedite | Windows Defender ATP telemetry reporting

> [!NOTE]
> - The **Health Status for onboarded machines** policy uses read-only properties and can't be remediated.
> - Configuration of telemetry reporting frequency is only available for machines on Windows 10, version 1703.
> - Using the Expedite mode might have an impact on the machine's battery usage and actual bandwidth used for sensor data. You should consider this when these measures are critical.

### Using the Azure Intune Portal to deploy Windows Defender Advanced Threat Protection policies on Windows 10 1607 and higher

1. Open the Microsoft Intune configuration package .zip file (*WindowsDefenderATPOnboardingPackage.zip*) that you downloaded from the service onboarding wizard. You can also get the package from the [Windows Defender ATP portal](https://securitycenter.windows.com/):

    a.  Select **Endpoint management** on the **Navigation pane**.

    b.  Select **Mobile Device Management/Microsoft Intune** > **Download package** and save the .zip file.

      ![Endpoint onboarding](images/atp-mdm-onboarding-package.png)

2. Extract the contents of the .zip file to a shared, read-only location that can be accessed by the network administrators who will deploy the package. You should have a file named *WindowsDefenderATP.onboarding*.

3. Login to the [Microsoft Azure portal](https://portal.azure.com).

4. From the Intune blade, choose **Device configuration**.

  ![Image of device configuration menu in Microsoft Azure](images/atp-azure-intune-device-config.png)

5. Under **Manage**, choose **Profiles** and click **Create Profile**.

  ![Image of policy creation in Azure](images/atp-azure-intune-create-profile.png)

4. Type a name, description and choose **Windows 10 and later** as the Platform and **Windows Defender ATP (Windows 10 Desktop)** as the Profile type.

  ![Image of naming a policy](images/atp-azure-intune-create-policy-configure.png)

7. Click **Settings** > **Configure**.

  ![Image of settings](images/atp-azure-intune-settings-configure.png)

8. Click the folder icon and select the WindowsDefenderATP.onboarding file you extracted earlier. Configure whether you want to allow sample collection from endpoints for [Deep Analysis](investigate-files-windows-defender-advanced-threat-protection.md) by choosing **All**, or disable this feature by choosing **None**. When complete, click **OK**.

  ![Image of configuration settings](images/atp-azure-intune-configure.png)

9. Click **Create**.

  ![Image of profile creation](images/atp-azure-intune-create.png)

10. Search for and select the Group you want to apply the Configuration Policy to, then click **Select**.

  ![Image of select groups to apply configuration policy](images/atp-azure-intune-select-group.png)

11. Click **Save** to finish deploying the Configuration Policy.

  ![Image of the policy being saved](images/atp-azure-intune-save-policy.png)


### Offboard and monitor endpoints

For security reasons, the package used to offboard endpoints will expire 30 days after the date it was downloaded. Expired offboarding packages sent to an endpoint will be rejected. When downloading an offboarding package you will be notified of the packages expiry date and it will also be included in the package name.

> [!NOTE]
> Onboarding and offboarding policies must not be deployed on the same endpoint at the same time, otherwise this will cause unpredictable collisions.

1.	Get the offboarding package from the [Windows Defender ATP portal](https://securitycenter.windows.com/):

    a. Click **Endpoint management** on the **Navigation pane**.

    b. Click the **Endpoint offboarding** section.

    c. Select **Mobile Device Management /Microsoft Intune**, click **Download package** and save the .zip file.

2.	Extract the contents of the .zip file to a shared, read-only location that can be accessed by the network administrators who will deploy the package. You should have a file named *WindowsDefenderATP_valid_until_YYYY-MM-DD.offboarding*.

3. Use the Microsoft Intune custom configuration policy to deploy the following supported OMA-URI settings. For more information on Microsoft Intune policy settings see, [Windows 10 policy settings in Microsoft Intune](https://docs.microsoft.com/en-us/intune/deploy-use/windows-10-policy-settings-in-microsoft-intune).

Offboarding - Use the offboarding policies to remove configuration settings on endpoints. These policies can be sub-categorized to:
- Offboarding
- Health Status for offboarded machines
- Configuration for offboarded machines

Policy | OMA-URI | Type | Value | Description
:---|:---|:---|:---|:---
Offboarding | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Offboarding | String | Copy content from offboarding MDM file | Offboarding
 Health Status for offboarded machines: Sense Is Running | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/SenseIsRunning | Boolean | FALSE |Windows Defender ATP service is not running
Health Status for offboarded machines: Onboarding State | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OnBoardingState | Integer | 0 | Offboarded from Windows Defender ATP

> [!NOTE]
> The **Health Status for offboarded machines** policy uses read-only properties and can't be remediated.

> [!IMPORTANT]
> Offboarding causes the machine to stop sending sensor data to the portal but data from the machine, including reference to any alerts it has had will be retained for up to 6 months.

## Related topics
- [Configure endpoints using Group Policy](configure-endpoints-gp-windows-defender-advanced-threat-protection.md)
- [Configure endpoints using System Center Configuration Manager](configure-endpoints-sccm-windows-defender-advanced-threat-protection.md)
- [Configure endpoints using a local script](configure-endpoints-script-windows-defender-advanced-threat-protection.md)
- [Troubleshoot Windows Defender Advanced Threat Protection onboarding issues](troubleshoot-onboarding-windows-defender-advanced-threat-protection.md)
