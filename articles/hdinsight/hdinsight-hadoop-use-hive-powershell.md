---
title: "Verwenden von Hadoop Hive mit PowerShell in HDInsight – Azure | Microsoft-Dokumentation"
description: "Ausführen von Hive-Abfragen mit Hadoop in HDInsight mithilfe von PowerShell"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: larryfr
ms.openlocfilehash: 74571fc6e1a0b2d6a903cdd992a247f4d5dfa700
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="run-hive-queries-using-powershell"></a>Ausführen von Hive-Abfragen mit PowerShell
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dieses Dokument enthält ein Beispiel für die Verwendung von Azure PowerShell im Azure-Ressourcengruppenmodus zum Ausführen von Hive-Abfragen in einem Hadoop-Cluster in HDInsight.

> [!NOTE]
> Dieses Dokument bietet keine detaillierte Beschreibung dazu, wie die in diesem Beispiel verwendeten HiveQL-Anweisungen vorgehen. Informationen zu der HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md).

**Voraussetzungen**

* Einen **Azure HDInsight-Cluster**: Es spielt keine Rolle, ob der Cluster auf Windows oder auf Linux basiert.

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Eine Arbeitsstation mit Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Ausführen von Hive-Abfragen mit Azure PowerShell

Azure PowerShell stellt *cmdlets* bereit, mit denen Sie Hive-Abfragen in HDInsight remote ausführen können. Intern führen die Cmdlets REST-Aufrufe von [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) im HDInsight-Cluster durch.

Die folgenden Cmdlets werden zum Ausführen der Hive-Abfragen auf einem HDInsight-Remotecluster verwendet:

* **Add-AzureRmAccount**: Authentifiziert Azure PowerShell für Ihr Azure-Abonnement.
* **New-AzureRmHDInsightHiveJobDefinition**: Erstellt mithilfe der angegebenen HiveQL-Anweisungen eine *Auftragsdefinition*.
* **Start-AzureRmHDInsightJob**: Sendet die Auftragsdefinition an HDInsight und startet den Auftrag. Ein *Auftragsobjekt* wird zurückgegeben.
* **Wait-AzureRmHDInsightJob**: verwendet das Auftragsobjekt, um den Status des Auftrags zu prüfen. Es wird gewartet, bis der Auftrag abgeschlossen oder die Wartezeit überschritten ist.
* **Get-AzureRmHDInsightJobOutput**: Wird zum Abrufen der Ausgabe des Auftrags verwendet.
* **Invoke-AzureRmHDInsightHiveJob**: wird zum Ausführen von HiveQL-Anweisungen verwendet. Mit diesem Cmdlet wird die Abfrage vollständig blockiert, und anschließend werden die Ergebnisse zurückgegeben.
* **Use-AzureRmHDInsightCluster**: Legt den aktuellen Cluster für die Verwendung mit dem Befehl **Invoke-AzureRmHDInsightHiveJob** fest.

Die folgenden Schritte veranschaulichen, wie diese Cmdlets zum Ausführen eines Auftrags auf dem HDInsight-Cluster verwendet werden:

1. Speichern Sie den folgenden Code mithilfe eines Editors als **hivejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Öffnen Sie eine neue **Azure PowerShell** -Befehlsaufforderung. Navigieren Sie zum Speicherort der Datei **hivejob.ps1** , und verwenden Sie folgenden Befehl zum Ausführen des Skripts:

        .\hivejob.ps1

    Beim Ausführen des Skripts werden Sie aufgefordert, den Clusternamen und die Anmeldeinformationen für das HTTPS-/Administratorkonto für den Cluster einzugeben. Sie werden außerdem möglicherweise dazu aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden.

3. Nachdem der Auftrag abgeschlossen ist, werden Informationen zurückgegeben, die folgendem Text ähneln:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Wie bereits erwähnt, kann **Invoke-Hive** dazu verwendet werden, um eine Abfrage auszuführen und auf die Antwort zu warten. Mit dem folgenden Skript können Sie veranschaulichen, wie „Invoke-Hive“ funktioniert:

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Die Ausgabe sieht in etwa wie folgt aus:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Für längere HiveQL-Abfragen können Sie Azure PowerShell **Here-Strings** -Cmdlet oder HiveQL-Skriptdateien verwenden. Der folgende Ausschnitt zeigt, wie Sie eine HiveQL-Skriptdatei mit dem **Invoke-Hive** -Cmdlet ausführen können. Die HiveQL-Skriptdatei muss auf wasb:// hochgeladen werden.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Weitere Informationen zu **Here-Strings** erhalten Sie unter <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell „Here-Strings“</a> (Verwenden von Windows PowerShell-Here-Strings).

## <a name="troubleshooting"></a>Problembehandlung

Wenn keine Informationen zurückgegeben werden, wenn der Auftrag abgeschlossen ist, zeigen Sie die Fehlerprotokolle an. Um Fehlerinformationen für diesen Auftrag anzuzeigen, fügen Sie Folgendes am Ende der Datei **hivejob.ps1** hinzu. Speichern Sie die Datei anschließend, und führen Sie sie erneut aus.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Mit diesem Cmdlet werden die Informationen zurückgegeben, die bei der Auftragsverarbeitung in STDERR geschrieben wurden.

## <a name="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, um Hive-Abfragen auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)
