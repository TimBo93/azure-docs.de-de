---
title: 'Transformieren von XML mit XSLT-Zuordnungen: Azure-Logik-Apps | Microsoft-Dokumentation'
description: "Informationen zum Hinzufügen von XSLT-Zuordnungen zum Transformieren von XML-Daten mit Azure Logic Apps und dem Enterprise Integration Pack"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a>Hinzufügen von Zuordnungen für die XML-Datentransformation

Für die Unternehmensintegration werden Zuordnungen verwendet, um XML-Daten in andere Formate zu transformieren. Eine Zuordnung ist ein XML-Dokument, das definiert, welche Daten in einem Dokument in ein anderes Format transformiert werden sollen. 

## <a name="why-use-maps"></a>Gründe für Zuordnungen

Angenommen, Sie erhalten regelmäßig B2B-Aufträge oder -Rechnungen von einem Kunden, der das Format JJMMTT für Datumsangaben verwendet. In Ihrer Organisation wird das Datum allerdings im Format TTMMJJJJ gespeichert. Mithilfe einer Zuordnung können das Datumsformat JJMMTT in das Format TTMMJJJJ *transformieren* , ehe die Auftrags- oder Rechnungsdetails in Ihrer Kundendatenbank gespeichert werden.

## <a name="how-do-i-create-a-map"></a>Wie erstelle ich eine Zuordnung?

Das [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") für Visual Studio 2015 ermöglicht Ihnen die Erstellung von BizTalk Integration-Projekten. Sie können eine Integration Map-Datei erstellen, mit deren Hilfe Sie Elemente visuell zwischen zwei XML-Schemadateien zuordnen können. Nachdem Sie dieses Projekt erstellt haben, verfügen Sie über ein XSLT-Dokument.

## <a name="how-do-i-add-a-map"></a>Wie füge ich eine Zuordnung hinzu?

1. Wählen Sie im Microsoft Azure-Portal **Durchsuchen** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Geben Sie im Filtersuchfeld das Wort **Integration** ein, und wählen Sie in der Liste mit den Ergebnissen den Eintrag **Integrationskonten** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wählen Sie die Kachel **Zuordnungen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ **Hinzufügen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Geben Sie einen **Namen** für Ihre App ein. Wählen Sie dann zum Hochladen der Zuordnungsdatei das Ordnersymbol rechts neben dem Textfeld **Zuordnung** aus. Klicken Sie nach dem Hochladevorgang auf **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Nachdem Azure die Zuordnung Ihrem Integrationskonto hinzugefügt hat, erhalten Sie auf dem Bildschirm die Meldung, dass Ihre Zuordnungsdatei hinzugefügt wurde oder nicht. Nachdem Sie diese Meldung erhalten haben, wählen Sie die Kachel **Zuordnungen** aus, damit Sie die neu hinzugefügte Zuordnung anzeigen können.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Wie bearbeite ich eine Zuordnung?

Sie müssen eine neue Zuordnungsdatei mit den gewünschten Änderungen hochladen. Zunächst können Sie die Zuordnung zum Bearbeiten herunterladen.

Gehen Sie wie folgt vor, um eine neue Zuordnung hochzuladen, die eine vorhandene Zuordnung ersetzt.

1. Wählen Sie die Kachel **Zuordnungen** aus.

2. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ die Zuordnung aus, die Sie bearbeiten möchten.

3. Wählen Sie auf dem Blatt **Zuordnungen** den Befehl **Aktualisieren** aus.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Wählen Sie in der Dateiauswahl die Zuordnungsdatei, die Sie hochladen möchten, und dann **Öffnen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Löschen einer Zuordnung

1. Wählen Sie die Kachel **Zuordnungen** aus.

2. Wählen Sie auf dem geöffneten Blatt „Zuordnungen“ die Zuordnung aus, die Sie löschen möchten.

3. Wählen Sie **Löschen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Bestätigen Sie, dass Sie die Zuordnung löschen möchten.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  
* [Weitere Informationen zu Transformationen](logic-apps-enterprise-integration-transform.md "Informationen zu Unternehmensintegrationstransformationen")  

