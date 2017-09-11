---
title: Deploy Windows Defender Device Guard - enable virtualization-based security (Windows 10)
description: This article describes how to enable virtualization-based security, one of the main features that are part of Windows Defender Device Guard in Windows 10. 
keywords: virtualization, security, malware
ms.prod: w10
ms.mktglfcycl: deploy
ms.localizationpriority: high
author: brianlic-msft
---

# Deploy Windows Defender Device Guard: enable virtualization-based security

**Applies to**
-   Windows 10
-   Windows Server 2016

Hardware-based security features, also called virtualization-based security or VBS, make up a large part of Windows Defender Device Guard security offerings. VBS reinforces the most important feature of Windows Defender Device Guard: configurable code integrity. There are a few steps to configure hardware-based security features in Windows Defender Device Guard:

1.  **Decide whether to use the procedures in this topic, or to use the Windows Defender Device Guard readiness tool**. To enable VBS, you can download and use [the hardware readiness tool on the Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=53337), or follow the procedures in this topic.

2.  **Verify that hardware and firmware requirements are met**. Verify that your client computers possess the necessary hardware and firmware to run these features. A list of requirements for hardware-based security features is available in [Hardware, firmware, and software requirements for Windows Defender Device Guard](requirements-and-deployment-planning-guidelines-for-device-guard.md#hardware-firmware-and-software-requirements-for-windows-defender-device-guard).

3.  **Enable the necessary Windows features**. There are several ways to enable the Windows features required for hardware-based security. You can use the [Windows Defender Device Guard and Windows Defender Credential Guard hardware readiness tool](https://www.microsoft.com/en-us/download/details.aspx?id=53337), or see the following section, [Windows feature requirements for virtualization-based security](#windows-feature-requirements-for-virtualization-based-security-and-device-guard).

4.  **Enable additional features as desired**. When the necessary Windows features have been enabled, you can enable additional hardware-based security features as desired. You can use the [Windows Defender Device Guard and Windows Defender Credential Guard hardware readiness tool](https://www.microsoft.com/en-us/download/details.aspx?id=53337), or see [Enable virtualization-based security (VBS)](#enable-virtualization-based-security-vbs-and-device-guard), later in this topic. 

For information about enabling Windows Defender Credential Guard, see [Protect derived domain credentials with Windows Defender Credential Guard](/windows/access-protection/credential-guard/credential-guard).

## Windows feature requirements for virtualization-based security and Windows Defender Device Guard

In addition to the hardware requirements found in [Hardware, firmware, and software requirements for Windows Defender Device Guard](requirements-and-deployment-planning-guidelines-for-device-guard.md#hardware-firmware-and-software-requirements-for-windows-defender-device-guard), you must confirm that certain operating system features are enabled before you can enable VBS:

- Beginning with Windows 10, version 1607 or Windows Server 2016:<br> 
Hyper-V Hypervisor, which is enabled automatically. No further action is needed.

- With an earlier version of Windows 10:<br> 
Hyper-V Hypervisor and Isolated User Mode (shown in Figure 1).

> **Note**&nbsp;&nbsp;You can configure these features by using Group Policy or Deployment Image Servicing and Management, or manually by using Windows PowerShell or the Windows Features dialog box.
 
![Turn Windows features on or off](images/dg-fig1-enableos.png)

**Figure 1. Enable operating system features for VBS, Windows 10, version 1511**

## Enable Virtualization Based Security (VBS) and Windows Defender Device Guard

There are multiple ways to configure VBS features for Windows Defender Device Guard: 

- You can use the [readiness tool](https://www.microsoft.com/en-us/download/details.aspx?id=53337) rather than the procedures in this topic.
- You can use Group Policy, as described in the procedure that follows.
- You can configure VBS manually, as described in [Use registry keys to enable VBS and Windows Defender Device Guard](#use-registry-keys-to-enable-vbs-and-device-guard), later in this topic.

> **Note**&nbsp;&nbsp;We recommend that you test-enable these features on a group of test computers before you enable them on users' computers. If untested, there is a possibility that this feature can cause system instability and ultimately cause the client operating system to fail.

### Use Group Policy to enable VBS and Windows Defender Device Guard

1.  To create a new GPO, right-click the OU to which you want to link the GPO, and then click **Create a GPO in this domain, and Link it here**.

    ![Group Policy Management, create a GPO](images/dg-fig2-createou.png)

    Figure 2. Create a new OU-linked GPO

2.  Give the new GPO a name, for example, **Contoso VBS settings GPO Test**, or any name you prefer. Ideally, the name will align with your existing GPO naming convention.

3.  Open the Group Policy Management Editor: right-click the new GPO, and then click **Edit**.

4.  Within the selected GPO, navigate to Computer Configuration\\Policies\\Administrative Templates\\System\\Windows Defender Device Guard. Right-click **Turn On Virtualization Based Security**, and then click **Edit**.

    ![Edit the group policy for Virtualization Based Security](images/dg-fig3-enablevbs.png)

    Figure 3. Enable VBS

5.  Select the **Enabled** button, and then choose a secure boot option, such as **Secure Boot**, from the **Select Platform Security Level** list.

    ![Group Policy, Turn On Virtualization Based Security](images/device-guard-gp.png)

    Figure 4. Configure VBS, Secure Boot setting (in Windows 10, version 1607)

    > **Important**&nbsp;&nbsp;These settings include **Secure Boot** and **Secure Boot with DMA**. In most situations we recommend that you choose **Secure Boot**. This option provides secure boot with as much protection as is supported by a given computer’s hardware. A computer with input/output memory management units (IOMMUs) will have secure boot with DMA protection. A computer without IOMMUs will simply have secure boot enabled.<br>In contrast, with **Secure Boot with DMA**, the setting will enable secure boot—and VBS itself—only on a computer that supports DMA, that is, a computer with IOMMUs. With this setting, any computer without IOMMUs will not have VBS (hardware-based) protection, although it can have code integrity policies enabled.<br>For information about how VBS uses the hypervisor to strengthen protections provided by a code integrity policy, see [How Windows Defender Device Guard features help protect against threats](introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies.md#how-windows-defender-device-guard-features-help-protect-against-threats).

6. For **Virtualization Based Protection of Code Integrity**, select the appropriate option.

    > [!WARNING]
    >  Virtualization-based protection of code integrity may be incompatible with some devices and applications. We strongly recommend testing this configuration in your lab before enabling virtualization-based protection of code integrity on production systems. Failure to do so may result in unexpected failures up to and including data loss or a blue screen error (also called a stop error).
    
    Select an option as follows:

    - With Windows 10, version 1607 or Windows Server 2016, choose an appropriate option:<br>For an initial deployment or test deployment, we recommend **Enabled without lock**.<br>When your deployment is stable in your environment, we recommend changing to **Enabled with lock**. This option helps protect the registry from tampering, either through malware or by an unauthorized person.

    - With earlier versions of Windows 10:<br>Select the **Enable Virtualization Based Protection of Code Integrity** check box.

    ![Group Policy, Turn On Virtualization Based Security](images/dg-fig7-enablevbsofkmci.png)

    Figure 5. Configure VBS, Lock setting (in Windows 10, version 1607)

7.  Close the Group Policy Management Editor, and then restart the Windows 10 test computer. The settings will take effect upon restart.

8.  Check the test computer’s event log for Windows Defender Device Guard GPOs.

    Processed Windows Defender Device Guard policies are logged in event viewer at **Applications and Services Logs\\Microsoft\\Windows\\DeviceGuard-GPEXT\\Operational**. When the **Turn On Virtualization Based Security** policy is successfully processed, event ID 7000 is logged, which contains the selected settings within the policy.

>**Note**&nbsp;&nbsp;Events will be logged in this event channel only when Group Policy is used to enable Windows Defender Device Guard features, not through other methods. If other methods such as registry keys are used, Windows Defender Device Guard features will be enabled but the events won’t be logged in this event channel.

### Use registry keys to enable VBS and Windows Defender Device Guard

Set the following registry keys to enable VBS and Windows Defender Device Guard. This provides exactly the same set of configuration options provided by Group Policy.

> [!WARNING]
>  Virtualization-based protection of code integrity (controlled through the registry key **HypervisorEnforcedCodeIntegrity**) may be incompatible with some devices and applications. We strongly recommend testing this configuration in your lab before enabling virtualization-based protection of code integrity on production systems. Failure to do so may result in unexpected failures up to and including data loss or a blue screen error (also called a stop error).

<!--This comment ensures that the Important above and the Warning below don't merge together. -->

> **Important**&nbsp;&nbsp;
> - Among the commands that follow, you can choose settings for **Secure Boot** and **Secure Boot with DMA**. In most situations we recommend that you simply choose **Secure Boot**. This option provides secure boot with as much protection as is supported by a given computer’s hardware. A computer with input/output memory management units (IOMMUs) will have secure boot with DMA protection. A computer without IOMMUs will simply have secure boot enabled.<br>In contrast, with **Secure Boot with DMA**, the setting will enable secure boot—and VBS itself—only on a computer that supports DMA, that is, a computer with IOMMUs. With this setting, any computer without IOMMUs will not have VBS (hardware-based) protection, although it can still have code integrity policies enabled.<br>For information about how VBS uses the hypervisor to strengthen protections provided by a code integrity policy, see [How Windows Defender Device Guard features help protect against threats](introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies.md#how-windows-defender-device-guard-features-help-protect-against-threats).<br>
> - All drivers on the system must be compatible with virtualization-based protection of code integrity; otherwise, your system may fail. We recommend that you enable these features on a group of test computers before you enable them on users' computers.

#### For Windows 1607 and above

Recommended settings (to enable virtualization-based protection of Code Integrity policies, without UEFI Lock):

``` commands
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "EnableVirtualizationBasedSecurity" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "RequirePlatformSecurityFeatures" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "Locked" /t REG_DWORD /d 0 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v "Enabled" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v "Locked" /t REG_DWORD /d 0 /f
```

If you want to customize the preceding recommended settings, use the following settings.

**To enable VBS**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "EnableVirtualizationBasedSecurity" /t REG_DWORD /d 1 /f
```

**To enable VBS and require Secure boot only (value 1)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "RequirePlatformSecurityFeatures" /t REG_DWORD /d 1 /f
```

> To enable **VBS with Secure Boot and DMA (value 3)**, in the preceding command, change **/d 1** to **/d 3**.

**To enable VBS without UEFI lock (value 0)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "Locked" /t REG_DWORD /d 0 /f
```

> To enable **VBS with UEFI lock (value 1)**, in the preceding command, change **/d 0** to **/d 1**.

**To enable virtualization-based protection of Code Integrity policies**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v "Enabled" /t REG_DWORD /d 1 /f
```

**To enable virtualization-based protection of Code Integrity policies without UEFI lock (value 0)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity" /v "Locked" /t REG_DWORD /d 0 /f
```

> To enable **virtualization-based protection of Code Integrity policies with UEFI lock (value 1)**, in the preceding command, change **/d 0** to **/d 1**.

#### For Windows 1511 and below

Recommended settings (to enable virtualization-based protection of Code Integrity policies, without UEFI Lock):

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "EnableVirtualizationBasedSecurity" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "RequirePlatformSecurityFeatures" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "HypervisorEnforcedCodeIntegrity" /t REG_DWORD /d 1 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "Unlocked" /t REG_DWORD /d 1 /f
```

If you want to customize the preceding recommended settings, use the following settings.

**To enable VBS (it is always locked to UEFI)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "EnableVirtualizationBasedSecurity" /t REG_DWORD /d 1 /f
```

**To enable VBS and require Secure boot only (value 1)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "RequirePlatformSecurityFeatures" /t REG_DWORD /d 1 /f
```

> To enable **VBS with Secure Boot and DMA (value 3)**, in the preceding command, change **/d 1** to **/d 3**.

**To enable virtualization-based protection of Code Integrity policies (with the default, UEFI lock)**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "HypervisorEnforcedCodeIntegrity" /t REG_DWORD /d 1 /f
```

**To enable virtualization-based protection of Code Integrity policies without UEFI lock**

``` command
reg add "HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard" /v "Unlocked" /t REG_DWORD /d 1 /f
```

### Validate enabled Windows Defender Device Guard hardware-based security features

Windows 10 and Windows Server 2016 and later have a WMI class for Windows Defender Device Guard–related properties and features: *Win32\_DeviceGuard*. This class can be queried from an elevated Windows PowerShell session by using the following command:

` Get-CimInstance –ClassName Win32_DeviceGuard –Namespace root\Microsoft\Windows\DeviceGuard`

> **Note**&nbsp;&nbsp;The *Win32\_DeviceGuard* WMI class is only available on the Enterprise edition of Windows 10.

The output of this command provides details of the available hardware-based security features as well as those features that are currently enabled. For detailed information about what each property means, refer to Table 1.

Table 1. Win32\_DeviceGuard properties

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Properties</strong></th>
<th align="left"><strong>Description</strong></th>
<th align="left"><strong>Valid values</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><strong>AvailableSecurityProperties</strong></td>
<td align="left">This field helps to enumerate and report state on the relevant security properties for Windows Defender Device Guard.</td>
<td align="left"><ul>
<li><p><strong>0.</strong> If present, no relevant properties exist on the device.</p></li>
<li><p><strong>1.</strong> If present, hypervisor support is available.</p></li>
<li><p><strong>2.</strong> If present, Secure Boot is available.</p></li>
<li><p><strong>3.</strong> If present, DMA protection is available.</p></li>
<li><p><strong>4.</strong> If present, Secure Memory Overwrite is available.</p></li>
<li><p><strong>5.</strong> If present, NX protections are available.</p></li>
<li><p><strong>6.</strong> If present, SMM mitigations are available.</p></li>
</ul>
<p><strong>Note</strong>: 4, 5, and 6 were added as of Windows 10, version 1607.</p>
</td>
</tr>
<tr class="even">
<td align="left"><strong>InstanceIdentifier</strong></td>
<td align="left">A string that is unique to a particular device.</td>
<td align="left">Determined by WMI.</td>
</tr>
<tr class="odd">
<td align="left"><strong>RequiredSecurityProperties</strong></td>
<td align="left">This field describes the required security properties to enable virtualization-based security.</td>
<td align="left"><ul>
<li><p><strong>0.</strong> Nothing is required.</p></li>
<li><p><strong>1.</strong> If present, hypervisor support is needed.</p></li>
<li><p><strong>2.</strong> If present, Secure Boot is needed.</p></li>
<li><p><strong>3.</strong> If present, DMA protection is needed.</p></li>
<li><p><strong>4.</strong> If present, Secure Memory Overwrite is needed.</p></li>
<li><p><strong>5.</strong> If present, NX protections are needed.</p></li>
<li><p><strong>6.</strong> If present, SMM mitigations are needed.</p></li>
</ul>
<p><strong>Note</strong>: 4, 5, and 6 were added as of Windows 10, version 1607.</p>
</td>
</tr>
<tr class="even">
<td align="left"><strong>SecurityServicesConfigured</strong></td>
<td align="left">This field indicates whether the Windows Defender Credential Guard or HVCI service has been configured.</td>
<td align="left"><ul>
<li><p><strong>0.</strong> No services configured.</p></li>
<li><p><strong>1.</strong> If present, Windows Defender Credential Guard is configured.</p></li>
<li><p><strong>2.</strong> If present, HVCI is configured.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><strong>SecurityServicesRunning</strong></td>
<td align="left">This field indicates whether the Windows Defender Credential Guard or HVCI service is running.</td>
<td align="left"><ul>
<li><p><strong>0.</strong> No services running.</p></li>
<li><p><strong>1.</strong> If present, Windows Defender Credential Guard is running.</p></li>
<li><p><strong>2.</strong> If present, HVCI is running.</p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><strong>Version</strong></td>
<td align="left">This field lists the version of this WMI class.</td>
<td align="left">The only valid value now is <strong>1.0</strong>.</td>
</tr>
<tr class="odd">
<td align="left"><strong>VirtualizationBasedSecurityStatus</strong></td>
<td align="left">This field indicates whether VBS is enabled and running.</td>
<td align="left"><ul>
<li><p><strong>0.</strong> VBS is not enabled.</p></li>
<li><p><strong>1.</strong> VBS is enabled but not running.</p></li>
<li><p><strong>2.</strong> VBS is enabled and running.</p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><strong>PSComputerName</strong></td>
<td align="left">This field lists the computer name.</td>
<td align="left">All valid values for computer name.</td>
</tr>
</tbody>
</table>

Another method to determine the available and enabled Windows Defender Device Guard features is to run msinfo32.exe from an elevated PowerShell session. When you run this program, the Windows Defender Device Guard properties are displayed at the bottom of the **System Summary** section, as shown in Figure 6.

![Windows Defender Device Guard properties in the System Summary](images/dg-fig11-dgproperties.png)

Figure 6. Windows Defender Device Guard properties in the System Summary

## Related topics

- [Introduction to Windows Defender Device Guard: virtualization-based security and code integrity policies](introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies.md)

- [Deploy Windows Defender Device Guard: deploy code integrity policies](deploy-device-guard-deploy-code-integrity-policies.md)
