---
title: Optimieren des VM-Netzwerkdurchsatzes | Microsoft-Dokumentation
description: Informationen zum Optimieren des Netzwerkdurchsatzes virtueller Azure-Computer
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer

Virtuelle Azure-Computer weisen standardmäßige Netzwerkeinstellungen auf, die zur Steigerung des Netzwerkdurchsatzes weiter optimiert werden können. Dieser Artikel beschreibt die Optimierung des Netzwerkdurchsatzes für virtuelle Microsoft Azure-Computer unter Windows und Linux, einschließlich der wichtigsten Versionen, wie etwa Ubuntu, CentOS und Red Hat.

## <a name="windows-vm"></a>Windows-VM

Wenn Ihr virtueller Windows-Computer [beschleunigten Netzwerkbetrieb](virtual-network-create-vm-accelerated-networking.md) unterstützt, ist die Aktivierung dieser Funktion im Hinblick auf den Durchsatz die optimale Konfiguration. Alle anderen virtuellen Windows-Computer, die die empfangsseitige Skalierung (Receive Side Scaling; RSS) verwenden, können einen höheren maximalen Durchsatz als ein virtueller Computer ohne RSS erreichen. RSS kann auf virtuellen Windows-Computern standardmäßig deaktiviert sein. Führen Sie die folgenden Schritte aus, um zu bestimmen, ob RSS aktiviert ist und um sie ggf. zu aktivieren.

1. Geben Sie den PowerShell-Befehl `Get-NetAdapterRss` ein, um anzuzeigen, ob RSS für ein Netzwerk aktiviert ist. In der folgenden Beispielausgabe aus `Get-NetAdapterRss` ist RSS nicht aktiviert.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Geben Sie den folgenden Befehl zum Aktivieren von RSS ein:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Der vorherige Befehl weist keine Ausgabe auf. Mit dem Befehl wurden die NIC-Einstellungen geändert und dadurch etwa eine Minute lang vorübergehende Konnektivitätsverluste verursacht. Während des Konnektivitätsverlusts wird das Dialogfeld „Verbindung wird wiederhergestellt“ angezeigt. In der Regel ist die Konnektivität nach dem dritten Versuch wiederhergestellt.
3. Bestätigen Sie, dass RSS auf dem virtuellen Computer aktiviert ist, indem Sie den Befehl `Get-NetAdapterRss` erneut eingeben. Bei Erfolg wird die folgende Beispielausgabe zurückgegeben:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux-VM

RSS ist auf einem virtuellen Azure Linux-Computer immer standardmäßig aktiviert. Linux-Kernel, die seit Januar 2017 veröffentlicht wurden, enthalten neue Netzwerkoptimierungsoptionen, die einem virtuellen Linux-Computer einen höheren Netzwerkdurchsatz ermöglichen.

### <a name="ubuntu"></a>Ubuntu

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Juni 2017):
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Nach dem Update geben Sie die folgenden Befehle zum Abrufen des neuesten Kernels ein:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Optionaler Befehl:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Kernel der Azure-Preview für Ubuntu
> [!WARNING]
> Dieser Linux Preview-Kernel von Azure ist unter Umständen nicht so verfügbar und zuverlässig wie Marketplace-Images und -Kernel in Versionen mit allgemeiner Verfügbarkeit. Das Feature wird nicht unterstützt, bietet möglicherweise eingeschränkte Funktionen und ist vielleicht nicht so zuverlässig wie der Standardkernel. Verwenden Sie dieses Kernel nicht für Produktionsworkloads.

Signifikante Durchsatzleistung kann durch die Installation der vorgeschlagenen Linux-Kernel von Azure erreicht werden. Um diesen Kernel auszuprobieren, fügen Sie diese Zeile zu /etc/apt/sources.list hinzu.

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Führen Sie anschließend diese Befehle als Stamm aus.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Juli 2017):
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Nach dem Update installieren Sie die neuesten Linux Integration Services (LIS).
Die Optimierung des Durchsatzes erfolgt in LIS, beginnend mit 4.2.2-2. Geben Sie die folgenden Befehle zum Installieren von LIS ein:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Juli 2017):
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Nach dem Update installieren Sie die neuesten Linux Integration Services (LIS).
Die Optimierung des Durchsatzes erfolgt in LIS, beginnend mit 4.2. Geben Sie die folgenden Befehle ein, um LIS herunterzuladen und zu installieren:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Weitere Informationen zu Linux Integration Services Version 4.2 für Hyper-V erhalten Sie auf der [Downloadseite](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie nun die VM optimiert haben, sehen Sie sich das Ergebnis beim [Testen der Bandbreite bzw. des Durchsatzes der Azure-VM](virtual-network-bandwidth-testing.md) für Ihr Szenario an.
* Weitere Informationen finden Sie unter [Azure Virtual Network – häufig gestellte Fragen (FAQs)](virtual-networks-faq.md).
