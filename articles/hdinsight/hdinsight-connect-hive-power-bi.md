---
title: Visualisieren von Big Data mit Power BI in Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Microsoft Power BI verwenden, um von Azure HDInsight verarbeitete Hive-Daten zu visualisieren.
keywords: hdinsight, hadoop, hive, interactive query, interactive hive, LLAP, odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jgao
ms.openlocfilehash: 9a17c984f5ca801dfa017f1d6fc8fa5ccdeaf39b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-hive-data-with-microsoft-power-bi-in-azure-hdinsight"></a>Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight

Erfahren Sie, wie Sie Microsoft Power BI mit Azure HDInsight verbinden und die Hive-Daten visualisieren. Derzeit unterstützt Power BI nur die ODBC-Verbindung mit HDInsight. In diesem Tutorial laden Sie die Daten aus einer „hivesampletable“-Hive-Tabelle in Power BI. Die Hive-Tabelle enthält einige Mobiltelefon-Nutzungsdaten. Anschließend zeichnen Sie die Nutzungsdaten auf eine Weltkarte:

![HDInsight Power BI Kartenbericht](./media/hdinsight-connect-power-bi/hdinsight-power-bi-visualization.png)

Die Informationen gelten auch für den neuen Clustertyp [Interactive Query](./hdinsight-hadoop-use-interactive-hive.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diesen Artikel durchgehen können, benötigen Sie Folgendes:

* **HDInsight-Cluster**. Bei dem Cluster kann es sich entweder um einen HDInsight-Cluster mit Hive oder um einen neu veröffentlichten Interactive Query-Cluster handeln. Informationen zum Erstellen von Clustern finden Sie unter [Cluster erstellen](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Sie können eine Kopie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331) herunterladen.

## <a name="create-hive-odbc-data-source"></a>Erstellen einer Hive ODBC-Datenquelle

Informationen finden Sie unter [Erstellen einer Hive ODBC-Datenquelle](hdinsight-connect-excel-hive-odbc-driver.md#create-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Laden von Daten aus HDInsight

Die Hive-Tabelle „hivesampletable“ ist in allen HDInsight-Clustern enthalten.

1. Melden Sie sich bei Power BI Desktop an.
2. Klicken Sie auf die Registerkarte **Start**, klicken Sie im Menüband **Externe Daten** auf **Daten abrufen**, und wählen Sie dann **Mehr** aus.

    ![HDInsight Power BI Daten öffnen](./media/hdinsight-connect-power-bi/hdinsight-power-bi-open-odbc.png)
3. Klicken Sie im Bereich **Daten abrufen** links auf **Other** (Sonstige), klicken Sie rechts auf **ODBC**, und klicken Sie dann unten auf **Verbinden**.
4. Wählen Sie im Bereich **Aus ODBC** den im letzten Abschnitt erstellten Datenquellennamen aus, und klicken Sie dann auf **OK**.
5. Erweitern Sie im Bereich **Navigator** **ODBC > HIVE > Standard**, wählen Sie **hivesampletable** aus, und klicken Sie auf **Laden**.

## <a name="visualize-date"></a>Visualisieren von Daten

Fahren Sie ab der vorherigen Prozedur fort.

1. Wählen Sie im Bereich „Visualisierungen“ **Karte** aus.  Dabei handelt es sich um ein Globussymbol.

    ![HDInsight Power BI passt Bericht an](./media/hdinsight-connect-power-bi/hdinsight-power-bi-customize.png)
2. Wählen Sie im Bereich „Felder“ **Land** und **devicemake** aus. Ihnen werden die auf die Karte gezeichneten Daten angezeigt.
3. Erweitern Sie die Karte.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power BI visualisieren.  Weitere Informationen finden Sie in den folgenden Artikeln:

* [Use Zeppelin to run Hive queries in Azure HDInsight (Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight)](./hdinsight-connect-hive-zeppelin.md).
* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](./hdinsight-connect-excel-hive-odbc-driver.md).
* [Verbinden von Excel mit Hadoop mithilfe von Power Query](./hdinsight-connect-excel-power-query.md).
* [Connect to Azure HDInsight and run Hive queries using Data Lake Tools for Visual Studio (Verbinden mit Azure HDInsight und Ausführen von Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio)](./hdinsight-hadoop-visual-studio-tools-get-started.md).
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](hdinsight-for-vscode.md)
* [Upload data to HDInsight (Hochladen von Daten in HDInsight)](./hdinsight-upload-data.md).
