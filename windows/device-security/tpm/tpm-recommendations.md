---
title: TPM recommendations (Windows 10)
description: This topic provides recommendations for Trusted Platform Module (TPM) technology for Windows 10.
ms.assetid: E85F11F5-4E6A-43E7-8205-672F77706561
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
localizationpriority: high
author: brianlic-msft
---

# TPM recommendations

**Applies to**

**Applies to**
-   Windows 10
-   Windows Server 2016

This topic provides recommendations for Trusted Platform Module (TPM) technology for Windows 10.

For a basic feature description of TPM, see the [Trusted Platform Module Technology Overview](trusted-platform-module-overview.md).

## TPM design and implementation

Traditionally, TPMs have been discrete chips soldered to a computer’s motherboard. Such implementations allow the computer’s original equipment manufacturer (OEM) to evaluate and certify the TPM separate from the rest of the system. Although discrete TPM implementations are still common, they can be problematic for integrated devices that are small or have low power consumption. Some newer TPM implementations integrate TPM functionality into the same chipset as other platform components while still providing logical separation similar to discrete TPM chips.

TPMs are passive: they receive commands and return responses. To realize the full benefit of a TPM, the OEM must carefully integrate system hardware and firmware with the TPM to send it commands and react to its responses. TPMs were originally designed to provide security and privacy benefits to a platform’s owner and users, but newer versions can provide security and privacy benefits to the system hardware itself. Before it can be used for advanced scenarios, however, a TPM must be provisioned. Windows 10 automatically provisions a TPM, but if the user is planning to reinstall the operating system, he or she may need to clear the TPM before reinstalling so that Windows can take full advantage of the TPM.

The Trusted Computing Group (TCG) is the nonprofit organization that publishes and maintains the TPM specification. The TCG exists to develop, define, and promote vendor-neutral, global industry standards that support a hardware-based root of trust for interoperable trusted computing platforms. The TCG also publishes the TPM specification as the international standard ISO/IEC 11889, using the Publicly Available Specification Submission Process that the Joint Technical Committee 1 defines between the International Organization for Standardization (ISO) and the International Electrotechnical Commission (IEC).

OEMs implement the TPM as a component in a trusted computing platform, such as a PC, tablet, or phone. Trusted computing platforms use the TPM to support privacy and security scenarios that software alone cannot achieve. For example, software alone cannot reliably report whether malware is present during the system startup process. The close integration between TPM and platform increases the transparency of the startup process and supports evaluating device health by enabling reliable measuring and reporting of the software that starts the device. Implementation of a TPM as part of a trusted computing platform provides a hardware root of trust—that is, it behaves in a trusted way. For example, if a key stored in a TPM has properties that disallow exporting the key, that key truly cannot leave the TPM.

The TCG designed the TPM as a low-cost, mass-market security solution that addresses the requirements of different customer segments. There are variations in the security properties of different TPM implementations just as there are variations in customer and regulatory requirements for different sectors. In public-sector procurement, for example, some governments have clearly defined security requirements for TPMs whereas others do not.

## TPM 1.2 vs. 2.0 comparison

From an industry standard, Microsoft has been an industry leader in moving and standardizing on TPM 2.0, which has many key realized benefits across algorithms, crypto, hierarchy, root keys, authorization and NV RAM.

## Why TPM 2.0?

TPM 2.0 products and systems have important security advantages over TPM 1.2, including:

-   The TPM 1.2 spec only allows for the use of RSA and the SHA-1 hashing algorithm.

-   For security reasons, some entities are moving away from SHA-1. Notably, NIST has required many federal agencies to move to SHA-256 as of 2014, and technology leaders, including Microsoft and Google have announced they will remove support for SHA-1 based signing or certificates in 2017.

-   TPM 2.0 **enables greater crypto agility** by being more flexible with respect to cryptographic algorithms.

    -   TPM 2.0 supports newer algorithms, which can improve drive signing and key generation performance. For the full list of supported algorithms, see the [TCG Algorithm Registry](http://www.trustedcomputinggroup.org/tcg-algorithm-registry/). Some TPMs do not support all algorithms.

    -   For the list of algorithms that Windows supports in the platform cryptographic storage provider, see [CNG Cryptographic Algorithm Providers](https://msdn.microsoft.com/library/windows/desktop/bb931354(v=vs.85).aspx).

    -   TPM 2.0 achieved ISO standardization ([ISO/IEC 11889:2015](http://blogs.microsoft.com/cybertrust/2015/06/29/governments-recognize-the-importance-of-tpm-2-0-through-iso-adoption/)).

    -   Use of TPM 2.0 may help eliminate the need for OEMs to make exception to standard configurations for certain countries and regions.

-   TPM 2.0 offers a more **consistent experience** across different implementations.

    -   TPM 1.2 implementations vary in policy settings. This may result in support issues as lockout policies vary.

    -   TPM 2.0 lockout policy is configured by Windows, ensuring a consistent dictionary attack protection guarantee.

-   While TPM 1.2 parts are discrete silicon components which are typically soldered on the motherboard, TPM 2.0 is available as a **discrete (dTPM)** silicon component in a single semiconductor package, an **integrated** component incorporated in one or more semiconductor packages - alongside other logic units in the same package(s) - and as a **firmware (fTPM)** based component running in a trusted execution environment (TEE) on a general purpose SoC.

## Discrete, Integrated or Firmware TPM?

There are three implementation options for TPMs:

-   Discrete TPM chip as a separate component in its own semiconductor package

-   Integrated TPM solution, using dedicated hardware integrated into one or more semiconductor packages alongside, but logically separate from, other components

-   Firmware TPM solution, running the TPM in firmware in a Trusted Execution mode of a general purpose computation unit

Windows uses any compatible TPM in the same way. Microsoft does not take a position on which way a TPM should be implemented and there is a wide ecosystem of available TPM solutions which should suit all needs.

## Is there any importance for TPM for consumers?

For end consumers, TPM is behind the scenes but is still very relevant. TPM is used for Windows Hello, Windows Hello for Business and in the future, will be a component of many other key security features in Windows. TPM secures the PIN, helps encrypt passwords, and builds on our overall Windows 10 experience story for security as a critical pillar. Using Windows on a system with a TPM enables a deeper and broader level of security coverage.

## TPM 2.0 Compliance for Windows 10

### Windows 10 for desktop editions (Home, Pro, Enterprise, and Education)

-   Since July 28, 2016, all new device models, lines or series (or if you are updating the hardware configuration of a existing model, line or series with a major update, such as CPU, graphic cards) must implement and enable by default TPM 2.0 (details in section 3.7 of the [Minimum hardware requirements](https://msdn.microsoft.com/library/windows/hardware/dn915086(v=vs.85).aspx) page).

### IoT Core

-   TPM is optional on IoT Core.

### Windows Server 2016

-   TPM is optional for Windows Server SKUs unless the SKU meets the additional qualification (AQ) criteria for the Host Guardian Services scenario in which case TPM 2.0 is required.

## TPM and Windows Features

The following table defines which Windows features require TPM support.

| Windows Features        | Windows 10 TPM 1.2   | Windows 10 TPM 2.0   | Details  |
|-------------------------|----------------------|----------------------|----------|
| Measured Boot                                | Required                 | Required                 | Measured boot requires TPM 1.2 or 2.0 and UEFI Secure Boot.   |
| Bitlocker                                    | Required                 | Required                 | TPM 1.2 or later required or a removable USB memory device such as a flash drive. Please note that TPM 2.0 requires UEFI Secure Boot in order for BitLocker to work properly.  |
| Passport: Domain AADJ Join                   | Required                 | Required                 | Supports both versions of TPM, but requires TPM with HMAC and EK certificate for key attestation support.  |
| Passport: MSA or Local Account               | Required                 | Required                 | TPM 2.0 is required with HMAC and EK certificate for key attestation support.      |
| Device Encryption                            | Not Applicable           | Required                 | TPM 2.0 is required for all InstantGo devices.                |
| Device Guard / Configurable Code Integrity   | See next column          | Recommended              |                    |
| Credential Guard                             | Required                 | Required                 | For Windows 10, version 1511, TPM 1.2 or 2.0 is highly recommended. If you don't have a TPM installed, Credential Guard will still be enabled, but the keys used to encrypt Credential Guard will not be protected by the TPM.   |
| Device Health Attestation                    | Required                 | Required                 |                    |
| Windows Hello                                | Not Required             | Recommended              |                    |
| UEFI Secure Boot                             | Not Required             | Recommended              |                    |
| Platform Key Storage provider                | Required                 | Required                 |                    |
| Virtual Smart Card                           | Required                 | Required                 |                    |
| Certificate storage (TPM bound)              | Required                 | Required                 |                    |
                                                                                  
## OEM Status on TPM 2.0 system availability and certified parts

Government customers and enterprise customers in regulated industries may have acquisition standards that require use of common certified TPM parts. As a result, OEMs, who provide the devices, may be required to use only certified TPM components on their commercial class systems. For more information, contact your OEM or hardware vendor.

## Related topics

- [Trusted Platform Module](trusted-platform-module-top-node.md) (list of topics)
