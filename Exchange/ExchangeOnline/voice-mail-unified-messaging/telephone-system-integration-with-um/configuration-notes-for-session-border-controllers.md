---
localization_priority: Normal
description: Session border controllers (SBCs) enable you to connect your on-premises telephony network to a Microsoft datacenter over a dedicated public WAN connection. An SBC sits on the edge of your on-premises IP network and connects to a second SBC in a Microsoft datacenter.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: d161f94a-a243-4294-93b3-2bf1dc17b59f
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configuration notes for supported session border controllers in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configuration notes for supported session border controllers in Exchange Online

Session border controllers (SBCs) enable you to connect your on-premises telephony network to a Microsoft datacenter over a dedicated public WAN connection. An SBC sits on the edge of your on-premises IP network and connects to a second SBC in a Microsoft datacenter.

SBCs require the use of digital certificates to encrypt all traffic between your on-premises organization and the Microsoft datacenter. You must obtain a digital certificate for the network border element, such as a session border controller, that you're using to communicate with Exchange hybrid and online deployments. Digital certificates establish trust between your on-premises organization and the Microsoft datacenter and enable mutual Transport Layer Security (mutual TLS). After this trust is established, the network border elements at your on-premises organization and at the Microsoft datacenter exchange session keys, and use these keys to encrypt the next data traffic.

In hybrid or online deployments, a UM IP gateway represents an SBC. The subject common name in the certificate must match the fully qualified domain name (FQDN) value in the Address box on the UM IP gateway that you create. For example, if you specify the FQDN address sbcexternal.contoso.com on your UM IP gateway, make sure that the subject name and subject alternative name in the certificate contain the same value: sbcexternal.contoso.com. The name that you use is case-sensitive, so make sure the case is the same on both the certificate and the UM IP gateway. If you're using an Acme Packet SBC and the common name doesn't match the UM IP gateway's FQDN, the call will be rejected with a 403 error.

> [!NOTE]
> Because SBCs are designed to sit on the network edge, they also function as a firewall. If you set up an SBC behind your organization's firewall, it can cause configuration problems and is unsupported for connecting to Microsoft 365 and Office 365.

## Supported session border controllers

The following SBCs have been successfully tested for interoperability with Exchange hybrid and online deployments. The capabilities and compatibilities of SBCs can vary, and the way you set them up can be different depending on other equipment on your network. Consult with the SBC manufacturer to see whether there are specific configuration notes for Unified Messaging in a hybrid or online deployment.

> [!NOTE]
> Exchange Online UM support for third-party PBX systems via direct connections from customer operated SBCs will end in December 2019. For more information, see the Exchange team blog [New date for discontinuation of support for Session Border Controllers in Exchange Online Unified Messaging](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/New-date-for-discontinuation-of-support-for-Session-Border/ba-p/607853).

|**Vendor**|**Model**|**Configuration notes**|**Comments**|
|:-----|:-----|:-----|:-----|
|[Acme Packet](http://www.acmepacket.com)|Net-Net 3820 or 4500|Contact the hardware vendor for up-to-date instructions on how to set up their device.|Dedicated SBC|
|[AudioCodes](https://www.audiocodes.com)|Mediant 1000B MSBG|Contact the hardware vendor for up-to-date instructions on how to set up their device.|Dedicated SBC|
|[AudioCodes](https://www.audiocodes.com)|Mediant 1000B MSBG|Contact the hardware vendor for up-to-date instructions on how to set up their device.|SBC and IP gateway|
|[Cisco](https://www.cisco.com/c/dam/en/us/solutions/collateral/enterprise-networks/unified-access/cube-asr-release-10-0.pdf)|ASR 1000  <br/> **Note**: Must have IOS 15.4(3)S3 or later installed.|Contact the hardware vendor for up-to-date instructions on how to set up their device.|Dedicated SBC|
|[Ingate](https://www.ingate.com/)|SIParator|Contact the hardware vendor for up-to-date instructions on how to set up their device.|Dedicated SBC|
|[NET](http://www.net.com)|VX1200 & VX1800|Contact the hardware vendor for up-to-date instructions on how to set up their device.|SBC option for a VoIP gateway product|
|[Sonus](http://www.sonus.net/)|SBC 1000/2000 2.2.1 or later|Contact the hardware vendor for up-to-date instructions on how to set up their device.|Dedicated SBC|
