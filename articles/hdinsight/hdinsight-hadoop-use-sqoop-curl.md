---
title: "Verwenden von Hadoop Sqoop mit Curl in HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Sqoop-Aufträge mithilfe von Curl remote an HDInsight übermitteln."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: 5aa47b4b12dc136a3f6ba66688804859f9eb5446
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Ausführen von Sqoop-Aufträgen mit Hadoop in HDInsight mit Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie Sie mit Sqoop-Aufträge in einem Hadoop-Cluster in HDInsight ausführen können.

Curl wird verwendet, um zu veranschaulichen, wie Sie über unformatierte HTTP-Anforderungen zum Ausführen, Überwachen und Abrufen der Ergebnisse der Sqoop-Aufträge mit HDInsight interagieren können. Dies funktioniert mithilfe der WebHCat REST-API (ehemals Templeton), die von Ihrem HDInsight-Cluster bereitgestellt wird.

## <a name="prerequisites"></a>Voraussetzungen
Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie Folgendes:

* Schließen Sie [Verwenden von Sqoop mit Hadoop in HDInsight](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) ab, um eine Umgebung mit einem HDInsight-Cluster und einer Azure SQL-Datenbank zu konfigurieren.
* [Curl](http://curl.haxx.se/). Curl ist ein Tool zum Übertragen von Daten aus einem oder in einen HDInsight-Cluster.
* [jq](http://stedolan.github.io/jq/). Das Hilfsprogramm jq wird verwendet, um die von REST-Anforderungen zurückgegebenen JSON-Daten zu verarbeiten.

## <a name="submit-sqoop-jobs-by-using-curl"></a>Übermitteln von Sqoop-Aufträgen mithilfe von Curl
> [!NOTE]
> Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen auch den Clusternamen als Teil des URIs (Uniform Resource Identifier) verwenden, um die Anforderungen an den Server zu senden.
> 
> Ersetzen Sie für die Befehle in diesem Abschnitt die Option **BENUTZERNAME** zur Authentifizierung im Cluster durch den Benutzer und die Option **KENNWORT** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.
> 
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Sie sollten Anforderungen immer über HTTPS (Secure HTTP) stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.
> 
> 

1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash   
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Sie sollten eine Antwort empfangen, die in etwa der im Folgenden aufgeführten entspricht:

    ```json   
    {"status":"ok","version":"v1"}
    ```
   
    Folgende Parameter werden in diesem Befehl verwendet:
   
   * **-u** – Der Benutzername und das Kennwort für die Authentifizierung der Anforderung
   * **-G** – Gibt an, dass dies eine GET-Anforderung ist
     
     Der Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch. Der Pfad, **/status**, gibt an, dass die Anforderung den Status von WebHCat (auch bekannt als Templeton) für den Server zurückgibt. 
2. Geben Sie den folgenden Befehl an, um einen Sqoop-Auftrag zu übermitteln:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    Folgende Parameter werden in diesem Befehl verwendet:

    * **-d**: Da `-G` nicht verwendet wird, verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.

        * **user.name** – Der Benutzer, der den Befehl ausführt

        * **command** – Der auszuführende Sqoop-Befehl.

        * **statusdir** – Das Verzeichnis, in das die Statusangaben für diesen Auftrag geschrieben werden.

    Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann.

        ```json
        {"id":"job_1415651640909_0026"}
        ```

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen. Ersetzen Sie **JOBID** durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z. B. `{"id":"job_1415651640909_0026"}` lautet, ist die **JOBID** `job_1415651640909_0026`.

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Wenn der Auftrag abgeschlossen ist, wird der Status **ERFOLGREICH**angezeigt.
   
   > [!NOTE]
   > Diese Curl-Anforderung gibt ein JSON-Dokument (JavaScript Object Notation) mit Informationen zum Auftrag zurück. Mithilfe von "jq" wird nur der Statuswert abgerufen.
   > 
   > 
4. Sobald der Status des Auftrags zu **ERFOLGREICH** wechselt, können Sie die Ergebnisse des Auftrags aus dem Azure Blob-Speicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Fall ist dieser **wasb:///example/data/sqoop/curl**. Diese Adresse speichert die Ausgabe des Auftrags im Verzeichnis **example/data/sqoop/curl** des Standardspeichercontainers, der von Ihrem HDInsight-Cluster verwendet wird.
   
    Sie können über das Azure-Portal auf die Blobs „stderr“ und „stdout“ zugreifen.  Sie können auch mithilfe von Microsoft SQL Server Management Studio die Daten überprüfen, die in die log4jlogs-Tabelle hochgeladen werden.

## <a name="limitations"></a>Einschränkungen
* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector derzeit keine Masseneinfügungen.
* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="summary"></a>Zusammenfassung
Wie in diesem Dokument veranschaulicht, können Sie unformatierte HTTP-Anforderungen dazu verwenden, um Sqoop-Aufträge auf dem HDInsight-Cluster auszuführen, zu überwachen und deren Ergebnisse anzuzeigen.

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie unter <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a> (Leitfaden zur Sqoop-REST-API).

## <a name="next-steps"></a>Nächste Schritte
Allgemeine Informationen zu Hive mit HDInsight:

* [Verwenden von Sqoop mit Hadoop in HDInsight](hdinsight-use-sqoop.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Weitere HDInsight-Artikel zu curl:
 
* [Erstellen von Hadoop-Clustern mithilfe der Azure-REST-API](hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [Ausführen von Hive-Abfragen mit Hadoop in HDInsight mit REST](hdinsight-hadoop-use-hive-curl.md)
* [Ausführen von MapReduce-Aufträgen mit Hadoop in HDInsight mithilfe von REST](hdinsight-hadoop-use-mapreduce-curl.md)
* [Ausführen von Pig-Aufträgen mit Hadoop in HDInsight mithilfe von Curl](hdinsight-hadoop-use-pig-curl.md)



