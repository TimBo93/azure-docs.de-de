---
title: Verwenden von Windows-Clientimages in Azure | Microsoft Docs
description: "Verwenden von Visual Studio-Abonnementvorteilen zum Bereitstellen von Windows 7, Windows 8 oder Windows 10 in Azure für Entwicklungs-/Testszenarien"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 207a6562965b4913416bd4dbf3eb132b42938dc9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Verwenden des Windows-Clients in Azure für Entwicklungs-/Testszenarien
Sie können Windows 7, Windows 8 oder Windows 10 in Azure für Entwicklungs-/Testszenarien verwenden, sofern Sie über ein entsprechendes Visual Studio-Abonnement (früher MSDN) verfügen. Dieser Artikel beschreibt die erforderlichen Berechtigungen für die Ausführung des Windows-Clients in Azure und die Verwendung von Images aus dem Azure-Katalog.

## <a name="subscription-eligibility"></a>Abonnementberechtigung
Aktive Visual Studio-Abonnenten (Personen, die eine Visual Studio-Abonnementlizenz erworben haben) können den Windows-Client für Entwicklungs- und Testzwecke verwenden. Der Windows-Client kann auf Ihrer eigenen Hardware und virtuellen Azure-Computern, die in einem Azure-Abonnement beliebigen Typs ausgeführt werden, verwendet werden. Der Windows-Client darf nicht in Azure für die normale Produktion bereitgestellt oder verwendet werden, bzw. von Personen verwendet werden, die nicht aktive Abonnenten von Visual Studio sind.

Der Einfachheit halber haben wir Ihnen bestimmte Windows 10-Images aus dem Azure-Katalog in [Geeignete Angebote](#eligible-offers)zur Verfügung gestellt. Visual Studio-Abonnenten können auch in jedem Angebotstyp ein 64-Bit-Image für Windows 7, Windows 8 oder Windows 10 [angemessen vorbereiten und erstellen](prepare-for-upload-vhd-image.md) und dann [in Azure hochladen](upload-generalized-managed.md). Die Verwendung bleibt auf Entwicklung/Test durch aktive Visual Studio-Abonnenten beschränkt.

## <a name="eligible-offers"></a>Geeignete Angebote
In der folgenden Tabelle werden die Angebots-IDs aufgeführt, die über den Azure-Katalog in Windows 10 bereitgestellt werden können. Die Windows 10-Images sind nur für die folgenden Angebote sichtbar. Für Visual Studio-Abonnenten, die den Windows-Client in einem anderen Angebotstyp ausführen müssen, müssen Sie ein 64-Bit-Image für Windows 7, Windows 8 oder Windows 10 [angemessen vorbereiten und erstellen](prepare-for-upload-vhd-image.md) und [dann in Azure hochladen](upload-generalized-managed.md).

| Angebotsname | Angebotsnummer | Verfügbare Client-images |
|:--- |:---:|:---:|
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise-Abonnenten (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium mit MSDN (Vorteil)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise-Abonnenten (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise Dev/Test](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Überprüfen Ihres Azure-Abonnements
Wenn Sie Ihre Angebots-ID nicht kennen, können Sie sie auf eine der beiden folgenden Weisen beim Azure-Portal abrufen:  

- Auf dem Blatt „Abonnements“:

  ![Details der Angebots-ID aus dem Azure-Portal](./media/client-images/offer-id-azure-portal.png) 

- Klicken Sie alternativ auf **Abrechnung** und dann auf Ihre Abonnement-ID. Die Angebots-ID wird auf dem Blatt „Abrechnung“ angezeigt.

Sie können die Angebots-ID auch auf der [Registerkarte „Abonnements“](http://account.windowsazure.com/Subscriptions) des Azure-Kontoportals anzeigen:

![Details der Angebots-ID aus dem Azure-Kontoportal](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie Ihre VMs mit [PowerShell](quick-create-powershell.md), [Resource Manager-Vorlagen](ps-template.md) oder [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) bereitstellen.

