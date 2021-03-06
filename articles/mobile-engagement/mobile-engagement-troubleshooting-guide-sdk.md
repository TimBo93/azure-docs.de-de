---
title: "Azure Mobile Engagement – Handbuch zur Problembehandlung – SDK"
description: Behandlung von Problemen der SDK-Integration in Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4d9d6165deb4bd0c65f1841aa7c457363a1f2865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Handbuch zur Problembehandlung bei der SDK-Integration
Im Folgenden finden Sie mögliche Probleme, die bei der Integration von Azure Mobile Engagement in Ihre Anwendung auftreten können.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Probleme mit SDK, die sich durch einen Fehler in einem anderen Bereich der Anwendung äußern
### <a name="issue"></a>Problem
* Es treten Fehler bei der Erfassung von Benutzeroberflächendaten auf (in Analyse, Überwachung, Segmentierung oder Dashboards).
* Pushvorgänge funktionieren nicht (App-intern, App-extern oder beides).
* Es treten Fehler bei erweiterten Funktionen auf (Nachverfolgung, Geolocation oder plattformspezifische Pushes funktionieren nicht).
* API-Fehler (bei APIs treten oft "stille" Fehler ohne Fehlermeldung auf).
* Dienstfehler (Azure Mobile Engagement funktioniert nicht für Ihre Anwendung).

### <a name="causes"></a>Ursachen
* Die meisten Probleme in Bezug auf das Azure Mobile Engagement-SDK äußern sich durch einen Fehler in Ihrer Anwendung (beispielsweise Fehler bei der Erfassung von Benutzeroberflächendaten, Pushfehler, Fehler mit erweiterten Funktionen, API-Fehler, Anwendungsabstürze oder scheinbare Dienstausfälle).  
* Wenn eine bestimmte Funktion von Azure Mobile Engagement noch nie in Ihrer App funktioniert hat, müssen Sie eine Integration durchführen. 
* Wenn eine bestimmte Funktion von Azure Mobile Engagement funktioniert hat, aber jetzt Fehler auftreten, müssen Sie möglicherweise mit dem Azure Mobile Engagement-SDK ein Upgrade auf die neueste Version durchführen. Denken Sie daran, dass es unterschiedliche Versionen des Azure Mobile Engagement-SDK für jede von Azure Mobile Engagement unterstützte Plattform gibt (Android, iOS, Windows und Windows Phone).

#### <a name="sdk-integration"></a>SDK-Integration
* Azure Mobile Engagement ist nicht ordnungsgemäß in das SDK integriert (Analyse).
* Reach ist nicht ordnungsgemäß in das SDK integriert (App-interne und App-externe Pushes).
* Zertifikate abgelaufen oder falsch konfiguriert, PROD bzw.  DEV (nur iOS).
* GCM oder ADM sind nicht ordnungsgemäß in das SDK integriert (nur Android - dienstspezifische Pushes).
* Die Nachverfolgung ist nicht ordnungsgemäß in das SDK integriert (installieren Sie die Speicherverfolgung).
* Lazy Location oder GPS-Ortung sind nicht ordnungsgemäß in das SDK integriert (Zielgruppenadressierung durch Geolocation).

**Weitere Informationen:**

* [SDK-Dokumentation – Handbücher zur Integration][Link 5] 
* [Handbuch zur Problembehandlung – Push][Link 23]

#### <a name="sdk-upgrade"></a>SDK-Upgrade
* Es muss ein SDK-Upgrade durchgeführt werden, um Probleme mit älteren SDK-Versionen zu lösen (stehen häufig in Zusammenhang mit neueren Versionen des Gerätebetriebssystems).
* Deinstallieren Sie alle vorherigen Versionen Ihrer App vom Gerät, und installieren Sie die neueste Version Ihrer App. Registrieren Sie Ihre Geräte-ID anschließend erneut über die Azure Mobile Engagement-Benutzeroberfläche um sicherzustellen, dass Ihr Gerät die neueste Version der App verwendet.

**Weitere Informationen:**

* [SDK-Dokumentation – Versionshinweise](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [SDK-Dokumentation – Handbücher zu Upgrades](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Weitere SDK-Probleme
* Fehler im Anwendungsmanifest "AndroidManifest.xml" kann dazu führen, dass Azure Mobile Engagement nicht funktioniert (nur Android).
* Ein häufiger Fehler bei der SDK-Integration und der API-Verwendung ist der, dass SDK- und API-Schlüssel verwechselt werden.

**Weitere Informationen:**

* [Konzepte – Glossar][Link 6]

## <a name="advanced-coding-issues"></a>Fehler bei der erweiterten Codierung
### <a name="issue"></a>Problem
* Plattformspezifischer Code, der nicht in direktem Zusammenhang mit Azure Mobile Engagement steht, kann zu Problemen auf iOS, Android und Windows Phone führen.

### <a name="causes"></a>Ursachen
* Viele Probleme mit Azure Mobile Engagement, die bei der erweiterten Codierung auftreten, werden durch fehlerhaften plattformspezifischen Code verursacht, der nicht in direktem Zusammenhang mit Azure Mobile Engagement steht. Neben der Azure Mobile Engagement-Dokumentation müssen Sie auch die Dokumentation für die Plattform zu Rate ziehen, für die Sie entwickeln (Android, iOS, Web, Windows und Windows Phone).
* Eine nicht ordnungsgemäße Konfiguration von "categories" verhindert das Verlinken aus einer Benachrichtigung mit einem anderen Speicherort, entweder App-intern oder App-extern (nur Android). 
* Wenn Sie "UIKit.framework" in Ihrem iOS-Code auf "optional" setzen, wird ein "Symbol nicht gefunden"-Fehler angezeigt und/oder es kommt auf älteren iOS-Geräten zu einem Absturz (nur iOS).
* Abgelaufene Zertifikate oder Zertifikate, die nicht die richtige DEV- oder Prod-Version des Zertifikats verwenden, verursachen Pushfehler (nur iOS).
* Es gelten einige Plattformeinschränkungen, die Azure Mobile Engagement nicht steuern kann (beispielsweise die Funktionsweise von System Center für App-externe Pushes in Android und iOS).
* Azure Mobile Engagement veröffentlicht als Referenz eine vollständige Liste der internen Pakete, die von Azure Mobile Engagement für iOS und Android verwendet werden. Beachten Sie, dass einige Funktionen von Azure Mobile Engagement plattformspezifisch sind (Android, iOS, Web, Windows und Windows Phone).

### <a name="see-also"></a>Weitere Informationen
* [Handbuch zur Problembehandlung – Push][Link 23] 
* [SDK-Dokumentation – Versionshinweise][Link 5]
* [SDK-Dokumentation – Handbücher zu Upgrades][Link 5]

## <a name="application-crashes"></a>Abstürze von Anwendungen
### <a name="issue"></a>Problem
* Die Anwendung stürzt auf dem Endbenutzergerät ab.

### <a name="causes"></a>Ursachen
* Auf der *Benutzeroberfläche* oder in der *API für die Analyse* können Sie Absturzinformationen anzeigen.
* Sie können die Geräte-ID Ihres Testgeräts ermitteln und dieselbe Aktion ausführen, die bei einem Endbenutzer zum Absturz Ihrer Anwendung führt, um die Ursache des Absturzes zu identifizieren.
* Bekannte Probleme mit dem Azure Mobile Engagement-SDK, die zu Abstürzen der Anwendungen führen, können manchmal durch ein Upgrade auf die neueste SDK-Version behoben werden. Lesen Sie daher die Versionshinweise zu Ihrer Plattform, wenn Sie Abstürze untersuchen.

### <a name="see-also"></a>Weitere Informationen
* [SDK-Dokumentation – Versionshinweise][Link 5]
* [SDK-Dokumentation – Handbücher zu Upgrades][Link 5]

## <a name="app-store-upload-failures"></a>Fehler bei App Store-Uploads
### <a name="issue"></a>Problem
* Es treten Fehler auf, wenn Sie die neueste Version Ihrer App in Apple, Google oder den Windows App Store hochladen.

### <a name="causes"></a>Ursachen
* App Stores blockieren gelegentlich Apps, bei denen bestimmte Funktionen aktiviert sind (Apple Store verhindert die Verwendung von IDFV in Apps im Store, und GooglePlay Store verhindert die Freigabe von Anwendungsinformationen zwischen Apps). 
* Prüfen Sie die Versionshinweise zu Ihrer Plattform und dem aktuellen SDK, wenn Sie Probleme beim Hochladen einer App in den Store haben.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

