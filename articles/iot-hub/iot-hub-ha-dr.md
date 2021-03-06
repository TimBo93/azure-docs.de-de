---
title: "Hochverfügbarkeit und Notfallwiederherstellung für Azure IoT Hub | Microsoft-Dokumentation"
description: "Informationen zu den Azure- und IoT Hub-Features zum Erstellen von Azure IoT-Lösungen mit hoher Verfügbarkeit und Notfallwiederherstellung."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: elioda
ms.openlocfilehash: b3ca2ed90dd14350d3962a558aaac41f2e007bbd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Hohe Verfügbarkeit und Notfallwiederherstellung von IoT Hub
Als Azure-Dienst sorgt IoT Hub für hohe Verfügbarkeit, indem Redundanzen auf Azure-Regionsebene bereitgestellt werden, ohne dass zusätzlicher Aufwand für die Lösung anfällt. Die Microsoft Azure-Plattform bietet auch Features zum Erstellen von Lösungen mit Notfallwiederherstellungsfunktionen (Disaster Recovery, DR) oder regionsübergreifender Verfügbarkeit. Wenn Sie global und regionsübergreifend eine hohe Verfügbarkeit für Geräte oder Benutzer erreichen möchten, müssen Sie Ihre Lösungen für die Nutzung der Notfallwiederherstellungsfeatures von Azure gestalten und vorbereiten. Im Artikel [Azure-Geschäftskontinuität – Technische Anleitung](../resiliency/resiliency-technical-guidance.md) werden die integrierten Features von Azure für Geschäftskontinuität und Notfallwiederherstellung beschrieben. Das Dokument [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen][Disaster recovery and high availability for Azure applications] enthält Architekturinformationen zu Strategien für Azure-Anwendungen in Bezug auf Notfallwiederherstellung und hohe Verfügbarkeit.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub – Notfallwiederherstellung
Zusätzlich zur Hochverfügbarkeit zwischen Regionen implementiert IoT Hub Failovermechanismen für die Notfallwiederherstellung, die keine Benutzereingriffe erfordern. Die IoT Hub-Notfallwiederherstellung löst sich selbst aus und hat ein Recovery Time Objective (RTO) von 2-26 Stunden und die folgenden Recovery Point Objectives (RPOs).

| Funktionalität | RPO |
| --- | --- |
| Dienstverfügbarkeit für Registrierungs- und Kommunikationsvorgänge |Möglicher CNAME-Verlust |
| Identitätsdaten in der Identitätsregistrierung |0-5 Minuten Datenverlust |
| D2C-Nachrichten |Alle ungelesenen Nachrichten gehen verloren |
| Vorgangsüberwachungsnachrichten |Alle ungelesenen Nachrichten gehen verloren |
| C2D-Nachrichten |0-5 Minuten Datenverlust |
| C2D-Feedbackwarteschlange |Alle ungelesenen Nachrichten gehen verloren |

## <a name="regional-failover-with-iot-hub"></a>Regionales Failover mit IoT Hub
Eine ausführliche Erläuterung von Bereitstellungstopologien in IoT-Lösungen würde den Rahmen dieses Artikels sprengen. In diesem Artikel wird das Bereitstellungsmodell für das *regionale Failover* zur Erzielung von Notfallwiederherstellung und hoher Verfügbarkeit behandelt.

Bei einem Modell für regionales Failover wird das Back-End der Lösung hauptsächlich an einem einzelnen Datencenterstandort ausgeführt, und ein sekundärer IoT-Hub und ein Back-End werden in einer anderen Datencenterregion bereitgestellt. Wenn die IoT Hub-Einheit im primären Datencenter ausfällt oder die Netzwerkverbindung des Geräts mit dem primären Datencenter unterbrochen wird, nutzen Geräte jeweils einen sekundären Dienstendpunkt, wenn das primäre Gateway nicht erreichbar ist. Mit einer regionsübergreifenden Failoverfunktion kann die Lösungsverfügbarkeit über die hohe Verfügbarkeit einer einzelnen Region hinweg verbessert werden.

Sie benötigen im Allgemeinen Folgendes, um ein Modell für regionales Failover mit IoT Hub zu implementieren:

* **Sekundäre IoT Hub-Einheit und Logik für Geräterouting:** Bei einer Dienstunterbrechung in der primären Region müssen Geräte eine Verbindung mit der sekundären Region herstellen. Da die meisten beteiligten Dienste zustandsorientiert sind, wird der Failoverprozess zwischen Regionen häufig von Lösungsadministratoren ausgelöst. Die beste Möglichkeit, Geräte über den neuen Endpunkt zu informieren und gleichzeitig die Kontrolle über den Prozess zu behalten, besteht darin, für die Geräte eine regelmäßige Prüfung eines *Concierge*-Diensts auf den derzeit aktiven Endpunkt durchführen zu lassen. Der Concierge-Dienst kann eine Webanwendung sein, die repliziert und deren Erreichbarkeit mithilfe von DNS-Umleitungsverfahren (etwa per [Azure Traffic Manager][Azure Traffic Manager]) gewährleistet wird.
* **Identitätsregistrierungsreplikation:** Um verwendet werden zu können, muss der sekundäre IoT-Hub alle Geräteidentitäten enthalten, die eine Verbindung mit der Lösung herstellen können. Für die Lösung sollten georeplizierte Backups von Geräteidentitäten vorgehalten und auf die sekundäre IoT Hub-Einheit hochgeladen werden, bevor der aktive Endpunkt für die Geräte gewechselt wird. Die Funktionen zum Exportieren der Geräteidentität von IoT Hub sind in diesem Zusammenhang praktisch. Weitere Informationen finden Sie unter [IoT Hub-Entwicklerhandbuch – Identitätsregistrierung][IoT Hub developer guide - identity registry].
* **Zusammenführungslogik:** Wenn die primäre Region wieder verfügbar ist, müssen die Status und Daten, die am sekundären Standort erstellt wurden, zurück zur primären Region migriert werden. Dieser Status und die Daten beziehen sich hauptsächlich auf Geräteidentitäten und Anwendungsmetadaten, die mit dem primären IoT-Hub und etwaigen anderen anwendungsspezifischen Datenspeichern in der primären Region zusammengeführt werden müssen. Zur Vereinfachung dieses Schritts empfiehlt sich die Verwendung idempotenter Vorgänge. Idempotente Vorgänge verringern die Nebeneffekte für die letztendliche konsistente Verteilung von Ereignissen sowie für Duplikate oder die außerordentliche Bereitstellung von Ereignissen. Außerdem sollte die Anwendungslogik so entworfen werden, dass potenzielle Inkonsistenzen oder ein „geringfügig“ veralteter Zustand toleriert werden. Dieser Fall kann aufgrund der zusätzlichen Zeit eintreten, die das System für die „Heilung“ basierend auf Recovery Point Objectives (RPO) benötigt.

## <a name="next-steps"></a>Nächste Schritte
Folgen Sie diesen Links, um mehr über Azure IoT Hub zu erfahren:

* [Erste Schritte mit IoT-Hubs (Tutorial)][lnk-get-started]
* [Was ist Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
