---
title: "Aufrufen eines Azure Automation-Runbooks über eine Log Analytics-Warnung | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie ein Automation-Runbook über eine Microsoft OMS Log Analytics-Warnung aufrufen."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 3370efe7696a6ffe9013886e07392806e008993b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Aufrufen eines Azure Automation-Runbooks über eine OMS Log Analytics-Warnung

Wenn in Log Analytics eine Warnung mit Erstellung eines Warnungsdatensatzes konfiguriert ist, falls Ergebnisse bestimmte Kriterien erfüllen (also beispielsweise über einen längeren Zeitraum eine Spitze bei der Prozessorauslastung vorliegt oder bei einem für eine Unternehmensanwendung kritischen Anwendungsprozess ein Fehler auftritt und daraufhin ein entsprechendes Ereignis in das Windows-Ereignisprotokoll geschrieben wird), kann die Warnung automatisch ein Automation-Runbook ausführen, um das Problem ggf. automatisch zu beheben.  

Es gibt zwei Optionen zum Aufrufen eines Runbooks in der Warnungskonfiguration, z.B.:

1. Verwenden eines Webhooks
   * Falls Ihr OMS-Arbeitsbereich nicht mit einem Automation-Konto verknüpft ist, ist dies die einzige verfügbare Option.
   * Falls Sie bereits ein Automation-Konto mit einem OMS-Arbeitsbereich verknüpft haben, steht diese Option auch weiterhin zur Verfügung.  

2. Direktes Auswählen eines Runbooks
   * Diese Option ist nur verfügbar, 
   *  wenn der OMS-Arbeitsbereich mit einem Automation-Konto verknüpft ist.  

## <a name="calling-a-runbook-using-a-webhook"></a>Aufrufen eines Runbooks unter Verwendung eines Webhooks

Mit einem Webhook können Sie ein bestimmtes Runbook in Azure Automation über eine einfache HTTP-Anforderung starten.  Bevor Sie die [Log Analytics-Warnung](../log-analytics/log-analytics-alerts.md#alert-rules) so konfigurieren, dass sie das Runbook unter Verwendung eines Webhooks aufruft, müssen Sie zunächst einen Webhook für das Runbook erstellen, das mit dieser Methode aufgerufen werden soll.  Führen Sie die unter [Erstellen eines Webhooks](automation-webhooks.md#creating-a-webhook) angegebenen Schritte aus, und notieren Sie sich die Webhook-URL, damit Sie sie später beim Konfigurieren der Warnungsregel zur Hand haben.   

## <a name="calling-a-runbook-directly"></a>Direktes Aufrufen eines Runbooks

Falls Sie das Angebot „Automation & Control“ installiert und in Ihrem OMS-Arbeitsbereich konfiguriert haben, können Sie beim Konfigurieren der Runbook-Aktionen für die Warnung in der Dropdownliste **Runbook auswählen** alle Runbooks anzeigen und das spezifische Runbook auswählen, das als Reaktion auf die Warnung ausgeführt werden soll.  Das ausgewählte Runbook kann in einem Arbeitsbereich in der Azure-Cloud oder für einen Hybrid Runbook Worker ausgeführt werden.  Nachdem die Warnung mit der Runbookoption erstellt wurde, wird ein Webhook für das Runbook erstellt.  Den Webhook können Sie anzeigen, indem Sie unter dem Automation-Konto zum Webhook-Bereich des ausgewählten Runbooks navigieren.  Wenn Sie die Warnung löschen, bleibt der Webhook bestehen, kann aber vom Benutzer manuell gelöscht werden.  Es ist kein Problem, wenn der Webhook nicht gelöscht wird. Er ist lediglich ein verwaistes Element, das Sie irgendwann einmal löschen sollten, damit Sie über ein aufgeräumtes Automation-Konto verfügen.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Merkmale eines Runbooks (für beide Optionen)

Beide Methoden, mit denen das Runbook über die Log Analytics-Warnung aufgerufen werden kann, haben unterschiedliche Verhaltensmerkmale, mit denen Sie vertraut sein müssen, bevor Sie Ihre Warnungsregeln konfigurieren.  

* Sie benötigen einen Runbook-Eingabeparameter namens **WebhookData** vom Typ **Objekt**.  Dieser kann obligatorisch oder optional sein.  Mithilfe dieses Eingabeparameters übergibt die Warnung die Suchergebnisse an das Runbook.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )

*  Sie benötigen Code, der die Webhookdaten (WebhookData) in ein PowerShell-Objekt konvertiert.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* ist ein Array mit Objekten, wobei jedes Objekt die Felder mit Werten eines einzelnen Suchergebnisses enthält.

### <a name="webhookdata-inconsistencies-between-the-webhook-option-and-runbook-option"></a>WebhookData-Inkonsistenzen zwischen der Webhook- und der Runbookoption

* Wenn Sie eine Warnung zum Aufrufen eines Webhooks konfigurieren, geben Sie eine Webhook-URL ein, die Sie für ein Runbook erstellt haben, und klicken Sie anschließend auf die Schaltfläche **Test Webhook** (Webhook testen).  Die Webhookdaten, die daraufhin an das Runbook gesendet werden, enthalten weder *.SearchResult* noch *.SearchResults*.

*  Nachdem die Warnung gespeichert wurde, wird die Warnung ausgelöst und der Webhook aufgerufen, und die an das Runbook gesendeten Webhookdaten enthalten das Element *.SearchResult*.
* Wenn Sie eine Warnung erstellen und so konfigurieren, dass sie ein Runbook aufruft (wobei auch ein Webhook erstellt wird), werden beim Auslösen der Warnung Webhookdaten mit *.SearchResults* an das Runbook gesendet.

Daher müssen Sie im obigen Codebeispiel *.SearchResult* abrufen, wenn die Warnung einen Webhook aufruft, und *.SearchResults*, wenn die Warnung ein Runbook direkt aufruft.

## <a name="example-walkthrough"></a>Exemplarische Vorgehensweise

Wie dies funktioniert, zeigen wir anhand des folgenden grafischen Beispielrunbooks, mit dem ein Windows-Dienst gestartet wird.<br><br> ![Grafisches Runbook zum Starten eines Windows-Diensts](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Das Runbook besitzt einen einzelnen Eingabeparameter vom Typ **Objekt** namens **WebhookData**, der die Webhookdaten enthält, die von der Warnung übergeben wurden (einschließlich *.SearchResults*).<br><br> ![Runbookeingabeparameter](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Für dieses Beispiel haben wir in Log Analytics zwei benutzerdefinierte Felder (*SvcDisplayName_CF* und *SvcState_CF*) erstellt, um aus dem Ereignis, das in das Systemereignisprotokoll geschrieben wurde, den Anzeigenamen und den Zustand des Diensts (ausgeführt oder beendet) zu extrahieren.  Anschließend erstellen wir eine Warnungsregel mit der Suchabfrage `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"`, um ermitteln zu können, wann der Druckspoolerdienst für das Windows-System beendet wird.  Sie können jeden beliebigen Dienst verwenden, der für Sie von Interesse ist. In diesem Beispiel verweisen wir jedoch auf einen der Dienste, die bereits im Windows-Betriebssystem enthalten sind.  Die Warnungsaktion ist so konfiguriert, dass das in diesem Beispiel verwendete Runbook für den Hybrid Runbook Worker ausgeführt wird, der auf dem Zielsystem aktiviert ist.   

Die Runbook-Codeaktivität **Get Service Name from LA** (Dienstname aus LA abrufen) konvertiert die JSON-Zeichenfolge in einen Objekttyp und filtert nach dem Element *SvcDisplayName_CF*, um den Anzeigenamen des Windows-Diensts zu extrahieren und an die nächste Aktivität zu übergeben. Diese Aktivität überprüft, ob der Dienst beendet wurde, bevor sie versucht, ihn neu zu starten.  *SvcDisplayName_CF* ist ein [benutzerdefiniertes Feld](../log-analytics/log-analytics-custom-fields.md), das in Log Analytics für dieses Beispiel erstellt wurde.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Wenn der Dienst beendet wird, erkennt die Warnungsregel in Log Analytics eine Übereinstimmung, löst das Runbook aus und sendet den Warnungskontext an das Runbook. Das Runbook überprüft, ob der Dienst beendet wurde. Wenn ja, wird versucht, den Dienst zu starten, und es wird überprüft, ob er richtig gestartet wurde. Danach gibt das Runbook die Ergebnisse aus.     

Falls Ihr Automation-Konto nicht mit Ihrem OMS-Arbeitsbereich verknüpft ist, können Sie die Warnungsregel alternativ mit einer Webhookaktion konfigurieren, um das Runbook auszulösen, und das Runbook so konfigurieren, dass es die JSON-Zeichenfolge konvertiert und nach *.SearchResult* filtert (siehe Anleitung weiter oben).    

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Warnungen in Log Analytics und zu deren Erstellung finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md).

* Informationen zum Auslösen von Runbooks mithilfe eines Webhooks finden Sie unter [Azure Automation-Webhooks](automation-webhooks.md).
