---
title: "Größen virtueller Computer und Azure-Clouddienste | Microsoft-Dokumentation"
description: "Führt die verschiedenen VM-Größen (und IDs) für Web-und Workerrollen von Azure-Clouddiensten auf."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 1ba56eb9539a4295fdaaab523cfd2a7e1587ef54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="sizes-for-cloud-services"></a>Größen für Clouddienste
In diesem Thema werden die verfügbaren Größen und Optionen für Cloud Service-Rolleninstanzen (Web- und Workerrollen) beschrieben. Darüber hinaus werden Überlegungen zur Bereitstellung angestellt, die Sie berücksichtigen sollten, wenn Sie eine Verwendung dieser Ressourcen planen. Jede Größe besitzt eine ID, die Sie in Ihre [Dienstdefinitionsdatei](cloud-services-model-and-package.md#csdef) einfügen. Preise für jede Größe sind auf der Seite [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/) verfügbar.

> [!NOTE]
> Zugehörige Einschränkungen von Azure finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Größen für Web- und Workerrolleninstanzen
In Azure stehen mehrere Standardgrößen zur Auswahl zur Verfügung. Beachten Sie für einige dieser Größen die folgenden Punkte:

* VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)(Neue VM-Größen der D-Serie, in englischer Sprache).
* Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Der Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.
* Virtuelle Computer der G-Serie bieten den meisten Arbeitsspeicher und werden auf Hosts mit Prozessoren der Intel Xeon E5 V3-Familie ausgeführt.
* Die VMs der A-Reihe können auf vielen verschiedenen Hardwaretypen und Prozessoren bereitgestellt werden. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird.
* Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.

Die Größe des virtuellen Computers wirkt sich auf den Preis aus. Die Größe beeinflusst auch die Verarbeitung, den Arbeitsspeicher und die Speicherkapazität des virtuellen Computers. Speicherkosten werden separat basierend auf bereits verwendete Seiten im Speicherkonto berechnet. Weitere Informationen finden Sie unter [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/) und [Preisdetails zu Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Die folgenden Überlegungen können Ihnen bei der Entscheidung über die Größe behilflich sein:

* Die Größen A8 bis A11 und die Größen der H-Reihe werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Die Reihen A8 bis A11 nutzen Intel Xeon E5-2670 mit 2,6 GHZ und die H-Reihe Intel Xeon E5-2667 v3 mit 3,2 GHz. Ausführliche Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Größen für leistungsstarke Compute-VMs](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Die Serien Dv2, D und G eignen sich ideal für Anwendungen, die schnellere CPUs oder bessere lokale Datenträgerleistung erfordern oder höhere Speicheranforderungen haben. Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.
* Einige der physischen Hosts in Azure-Rechenzentren unterstützen möglicherweise keine der größeren VM-Größen, z.B. A5 bis A11. Daher wird beim Ändern der Größe eines vorhandenen virtuellen Computers, beim Erstellen eines neuen virtuellen Computers in einem virtuellen Netzwerk, das vor dem 16. April 2013 erstellt wurde, oder beim Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst möglicherweise die Fehlermeldung **Fehler beim Konfigurieren des virtuellen Computers "{Computername}"** oder **Fehler beim Erstellen des virtuellen Computers "{Computername}"** angezeigt. Problemumgehungen für die einzelnen Bereitstellungsszenarien finden Sie im Supportforum unter [Error: “Failed to configure virtual machine”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) (Fehler beim Konfigurieren des virtuellen Computers).
* Möglicherweise ist bei Ihrem Abonnement auch die Anzahl von Kernen beschränkt, die in bestimmten Größenkategorien bereitgestellt werden können. Wenn Sie Kontingent erhöhen möchten, wenden Sie sich an den Azure-Support.

## <a name="performance-considerations"></a>Überlegungen zur Leistung
Wir haben das Konzept der Azure-Berechnungseinheit ACU (Azure Compute Unit) geschaffen, um eine Möglichkeit zum Vergleichen der Rechenleistung (CPU) zwischen den Azure-SKUs zu erhalten und Ihnen die Ermittlung, welche SKU Ihren Leistungsanforderungen am ehesten entspricht, zu vereinfachen.  Zurzeit ist der ACU-Wert auf einem kleinen virtuellen Computer (Standard_A1) auf den Standardwert 100 festgelegt, und an den übrigen SKUs kann ungefähr abgelesen werden, wie viel schneller die jeweilige SKU einen Standard-Benchmarktest ausführen kann.

> [!IMPORTANT]
> Die ACU ist nur ein Richtwert. Die Ergebnisse für Ihre Workload können abweichen.
>
>

<br>

| SKU-Familie | ACU/Kern |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Small-ExtraLarge](#a-series) |100 |
| [A5–7](#a-series) |100 |
| [Standard_A1-8v2](#av2-series) |100 |
| [Standard_A2m-8mv2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-14](#d-series) |160 |
| [D1-15v2](#dv2-series) |210 - 250* |
| [G1-5](#g-series) |180 - 240* |
| [H](#h-series) |290 - 300* |

Mit * gekennzeichnete ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen eine Leistungssteigerung zu erzielen. Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.

## <a name="size-tables"></a>Größentabellen
In den folgenden Tabellen sind die Größe und die von den einzelnen Größen bereitgestellte Kapazität aufgeführt.

* Speicherkapazität wird in GiB-Einheiten oder 1.024^3 Bytes angezeigt. Beachten Sie beim Vergleich von in GB (1.000^3 Bytes) gemessenen Datenträgern mit in GiB (1.024^3) gemessenen Datenträgern, dass die in GiB angegebenen Kapazitätszahlen kleiner erscheinen können. Beispiel: 1.023 GiB = 1.098,4 GB.
* Der Datenträgerdurchsatz wird in E/A-Vorgängen pro Sekunde (Input/Output Operations Per Second, IOPS) und MB/s gemessen, wobei MB/s = 10^6 Bytes/Sekunde beträgt.
* Datenträger können mit oder ohne Cache betrieben werden. Beim Datenträgerbetrieb mit Cache ist der Hostcachemodus auf **ReadOnly** oder **ReadWrite** festgelegt. Beim Datenträgerbetrieb ohne Cache ist der Hostcachemodus auf **None**festgelegt.
* Bei der maximalen Netzwerkbandbreite handelt es sich um die maximale aggregierte Bandbreite, die pro VM-Typ zugewiesen wurde. Die maximale Bandbreite dient als Orientierungshilfe bei der Wahl des richtigen VM-Typs, um sicherzustellen, dass ausreichend Netzwerkkapazität verfügbar ist. Bei einem Wechsel zu „niedrig“, „mittel“, „hoch“ oder „sehr hoch“ ändert sich der Durchsatz entsprechend. Die tatsächliche Netzwerkleistung hängt von zahlreichen Faktoren ab. Hierzu zählen beispielsweise die Netzwerk- und Anwendungslast und die Netzwerkeinstellungen der Anwendung.

## <a name="a-series"></a>A-Serie
| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales HDD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Sehr klein      | 1         | 0,768        | 20                   | 1/niedrig |
| Klein           | 1         | 1,75         | 225                  | 1/moderat |
| Mittel          | 2         | 3,5 GB       | 490                  | 1/moderat |
| Groß           | 4         | 7            | 1000                 | 2/hoch |
| Extragroß      | 8         | 14           | 2040                 | 4/hoch |
| A5              | 2         | 14           | 490                  | 1/moderat |
| A6              | 4         | 28           | 1000                 | 2/hoch |
| A7              | 8         | 56           | 2040                 | 4/hoch |

## <a name="a-series---compute-intensive-instances"></a>A-Serie: Rechenintensive Instanzen
Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Größen für leistungsstarke Compute-VMs](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales HDD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8*             |8          | 56           | 1817                 | 2/hoch |
| A9*             |16         | 112          | 1817                 | 4/sehr hoch |
| A10             |8          | 56           | 1817                 | 2/hoch |
| A11             |16         | 112          | 1817                 | 4/sehr hoch |

\*RDMA-fähig

## <a name="av2-series"></a>Av2-Serie

| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales SSD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1/moderat                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2/moderat                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4/hoch                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8/hoch                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2/moderat                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4/hoch                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8/hoch                     |


## <a name="d-series"></a>D-Serie
| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales SSD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3,5          | 50                   | 1/moderat |
| Standard_D2     | 2         | 7            | 100                  | 2/hoch |
| Standard_D3     | 4         | 14           | 200                  | 4/hoch |
| Standard_D4     | 8         | 28           | 400                  | 8/hoch |
| Standard_D11    | 2         | 14           | 100                  | 2/hoch |
| Standard_D12    | 4         | 28           | 200                  | 4/hoch |
| Standard_D13    | 8         | 56           | 400                  | 8/hoch |
| Standard_D14    | 16        | 112          | 800                  | 8/sehr hoch |

## <a name="dv2-series"></a>Dv2-Serie
| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales SSD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3,5          | 50                   | 1/moderat |
| Standard_D2_v2  | 2         | 7            | 100                  | 2/hoch |
| Standard_D3_v2  | 4         | 14           | 200                  | 4/hoch |
| Standard_D4_v2  | 8         | 28           | 400                  | 8/hoch |
| Standard_D5_v2  | 16        | 56           | 800                  | 8/äußerst hoch |
| Standard_D11_v2 | 2         | 14           | 100                  | 2/hoch |
| Standard_D12_v2 | 4         | 28           | 200                  | 4/hoch |
| Standard_D13_v2 | 8         | 56           | 400                  | 8/hoch |
| Standard_D14_v2 | 16        | 112          | 800                  | 8/äußerst hoch |
| Standard_D15_v2 | 20        | 140          | 1.000                | 8/äußerst hoch |

## <a name="g-series"></a>G-Serie
| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales SSD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1/hoch |
| Standard_G2     | 4         | 56           | 768                  |2/hoch |
| Standard_G3     | 8         | 112          | 1.536                |4/sehr hoch |
| Standard_G4     | 16        | 224          | 3.072                |8/äußerst hoch |
| Standard_G5     | 32        | 448          | 6.144                |8/äußerst hoch |

## <a name="h-series"></a>H-Reihe
Virtuelle Azure-Computer der H-Reihe sind High Performing Computing-VMs der nächsten Generation für High-End-Berechnungsanforderungen, z.B. Molekülmodellierung und Strömungsdynamikberechnung. Diese virtuellen Computer mit 8 und 16 Kernen basieren auf der Intel Haswell E5-2667 V3-Prozessortechnologie mit DDR4-Speicher und lokalem SSD-Speicher.

Neben beträchtlicher CPU-Leistung bietet die H-Serie verschiedene Optionen für RDMA-Netzwerke mit niedriger Latenz unter Verwendung von FDR InfiniBand sowie verschiedene Speicherkonfigurationen für Berechnungsanforderungen mit hohem Speicherbedarf.

| Größe            | CPU-Kerne | Arbeitsspeicher: GiB  | Lokales SSD: GiB       | Maximale Anzahl NICs/Netzwerkbandbreite |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8/hoch |
| Standard_H16    | 16        | 112          | 2000                 | 8/sehr hoch |
| Standard_H8m    | 8         | 112          | 1000                 | 8/hoch |
| Standard_H16m   | 16        | 224          | 2000                 | 8/sehr hoch |
| Standard_H16r*  | 16        | 112          | 2000                 | 8/sehr hoch |
| Standard_H16mr* | 16        | 224          | 2000                 | 8/sehr hoch |

\*RDMA-fähig

## <a name="configure-sizes-for-cloud-services"></a>Konfigurieren von Größen für Cloud Services
Sie können die Größe des virtuellen Computers einer Rolleninstanz als Teil des Dienstmodells angeben, wie durch die [Dienstdefinitionsdatei](cloud-services-model-and-package.md#csdef)beschrieben. Die Größe einer Rolle bestimmt die Anzahl der CPU-Kerne, die Speicherkapazität und die lokale Dateisystemgröße, die einer aktiven Instanz zugeordnet werden. Wählen Sie die Rollengröße basierend auf der Ressourcenanforderung Ihrer Anwendung.

Hier sehen Sie ein Beispiel, bei dem die Rollengröße [Standard_D2](#general-purpose-d) für eine Webrolleninstanz festgelegt wird:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-the-size-of-an-existing-role"></a>Ändern der Größe einer vorhandenen Rolle

Wenn sich die Art Ihre Workload ändert oder neue VM-Größen verfügbar werden, kann es ratsam sein, die Größe Ihrer Rolle zu ändern. Hierzu müssen Sie die VM-Größe in Ihrer Dienstdefinitionsdatei (wie oben dargestellt) ändern, und Ihren Clouddienst neu packen und bereitstellen. Es ist nicht möglich, VM-Größen direkt über das Portal oder mit PowerShell zu ändern.

>[!TIP]
> Es kann hilfreich sein, unterschiedliche VM-Größen für Ihre Rolle zu verwenden, wenn Sie unterschiedliche Umgebungen nutzen (z.B. für Test und Produktion). Eine Möglichkeit ist die Erstellung von mehreren Dienstdefinitionsdateien (.csdef) in Ihrem Projekt und die anschließende Erstellung von unterschiedlichen Clouddienstpaketen pro Umgebung während des automatisierten Buildvorgangs mit dem CSPack-Tool. Weitere Informationen zu den Elementen eines Clouddienstpakets und zu deren Erstellung finden Sie unter [Was ist das Clouddienstmodell, und wie kann es gepackt werden?](cloud-services-model-and-package.md).
>
>

## <a name="get-a-list-of-sizes"></a>Abrufen einer Größenliste
Sie können PowerShell oder die REST-API zum Abrufen einer Größenliste verwenden. Eine Dokumentation zur REST-API finden Sie [hier](https://msdn.microsoft.com/library/azure/dn469422.aspx). Der folgende Code ist ein PowerShell-Befehl, der alle derzeit verfügbaren Größen für Ihren Clouddienst auflistet.

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
* Erfahren Sie mehr über [Größen für leistungsstarke Compute-VMs](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für HPC-Workloads.
