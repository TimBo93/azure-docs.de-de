---
title: "Migrieren von vorhandenen Datenbanken für die horizontale Skalierung | Microsoft Docs"
description: "Erstellen eines Shardzuordnungs-Managers, um Sharddatenbanken für die Verwendung der Tools für elastische Datenbanken umzuwandeln"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 099f40d00753b7c86ba726a818f17d440a125221
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-existing-databases-to-scale-out"></a>Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung
Verwalten Sie Ihre vorhandenen horizontal skalierten Sharddatenbanken mithilfe von Azure SQL-Datenbanktools (wie z.B. der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)). Konvertieren Sie zunächst eine vorhandene Gruppe von Datenbanken für die Verwendung des [Shardzuordnungs-Managers](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Übersicht
So migrieren Sie eine vorhandene Sharddatenbank: 

1. Bereiten Sie die [Shardzuordnungs-Manager-Datenbank](sql-database-elastic-scale-shard-map-management.md)vor.
2. Erstellen Sie die Shardzuordnung.
3. Bereiten Sie die einzelnen Shards vor.  
4. Fügen Sie Zuordnungen zur Shardzuordnung hinzu.

Diese Schritte können entweder unter Verwendung der [.NET Framework-Clientbibliothek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) oder der PowerShell-Skripts ausgeführt werden, die unter [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db) (Azure SQL-Datenbank – Skripts für Tools für elastische Datenbanken) verfügbar sind. Für die hier beschriebenen Beispiele werden die PowerShell-Skripts verwendet.

Weitere Informationen zum Shardzuordnungs-Manager finden Sie unter [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md). Eine Übersicht der Tools für elastische Datenbanken finden Sie unter [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Vorbereiten der Shardzuordnungs-Manager-Datenbank
Der Shardzuordnungs-Manager ist eine spezielle Datenbank, die Daten zur Verwaltung von horizontal skalierten Datenbanken enthält. Sie können eine vorhandene Datenbank verwenden oder eine neue Datenbank erstellen. Beachten Sie, dass es sich bei einer als Shardzuordnungs-Manager eingesetzten Datenbank nicht gleichzeitig um eine Datenbank handeln sollte, die als Shard definiert ist. Beachten Sie außerdem, dass das PowerShell-Skript die Datenbank nicht für Sie erstellt. 

## <a name="step-1-create-a-shard-map-manager"></a>Schritt 1: Erstellen eines Shardzuordnungs-Managers
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>So rufen Sie den Shardzuordnungs-Manager ab
Nach dem Erstellen können Sie den Shardzuordnungs-Manager mit diesem Cmdlet abrufen. Dieser Schritt muss jedes Mal ausgeführt werden, wenn Sie das ShardMapManager-Objekt verwenden müssen.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>Schritt 2: Erstellen der Shardzuordnung
Sie müssen den Typ der zu erstellenden Shardzuordnung auswählen. Die Auswahl hängt von der Architektur der Datenbank ab: 

1. Ein Mandant pro Datenbank (Informationen finden Sie im [Glossar](sql-database-elastic-scale-glossary.md)). 
2. Mehrere Mandanten pro Datenbank (zwei Typen):
   1. Listenzuordnung
   2. Bereichszuordnung

Erstellen Sie für ein Modell mit einem einzelnen Mandanten eine **Listenzuordnungs** -Shardzuordnung. Beim Modell mit einem Mandanten wird eine Datenbank pro Mandant zugewiesen. Dies ist ein effektives Modell für SaaS-Entwickler, da es die Verwaltung vereinfacht.

![Listenzuordnung][1]

Beim Modell mit mehreren Mandanten werden einer Datenbank mehrere Mandanten zugewiesen. Außerdem können Sie Mandantengruppen auf verschiedene Datenbanken verteilen. Verwenden Sie dieses Modell, wenn Sie erwarten, dass die einzelnen Mandanten geringe Datenanforderungen haben. In diesem Modell wird einer Datenbank mithilfe der **Bereichszuordnung** ein Bereich von Mandanten zugewiesen. 

![Bereichszuordnung][2]

Wenn Sie einer Einzeldatenbank mehrere Mandanten zuweisen möchten, kann das Datenbankmodell mit mehreren Mandanten auch unter Verwendung einer *Listenzuordnung* implementiert werden. Beispiel: DB1 wird zum Speichern von Informationen zu Mandanten 1 und 5, DB2 zum Speichern von Daten für Mandant 7 und 10 verwendet. 

![Einzeldatenbank mit mehreren Mandanten][3] 

**Basierend auf Ihrer Entscheidung wählen Sie eine der folgenden Optionen aus:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Option 1: Erstellen einer Shardzuordnung für eine Listenzuordnung
Erstellen Sie mithilfe des ShardMapManager-Objekts eine Shardzuordnung. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Option 2: Erstellen einer Shardzuordnung für eine Bereichszuordnung
Beachten Sie, dass die Mandanten-IDs bei Verwendung dieses Zuordnungsmusters fortlaufenden Bereichen entsprechen müssen. Dabei können Bereiche unterbrochen werden, indem Sie einen Bereich beim Erstellen der Datenbanken überspringen.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Option 3: Listenzuordnungen bei Einzeldatenbanken
Für die Einrichtung dieses Musters muss auch eine Listenzuordnung erstellt werden, wie in Schritt 2, Option 1 gezeigt.

## <a name="step-3-prepare-individual-shards"></a>Schritt 3: Vorbereiten der einzelnen Shards
Fügen Sie die einzelnen Shards (Datenbanken) dem Shardzuordnungs-Manager hinzu. Dadurch werden die einzelnen Datenbanken für das Speichern von Zuordnungsinformationen vorbereitet. Führen Sie diese Schritte für jeden Shard aus.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>Schritt 4: Hinzufügen von Zuordnungen
Die Schritte zum Hinzufügen von Zuordnungen hängen von der Art der Shardzuordnung ab, die Sie erstellt haben. Wenn Sie eine Listenzuordnung erstellt haben, fügen Sie Listenzuordnungen hinzu. Wenn Sie eine Bereichszuordnung erstellt haben, fügen Sie Bereichszuordnungen hinzu.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Option 1: Zuordnen der Daten für eine Listenzuordnung
Ordnen Sie die Daten zu, indem Sie eine Listenzuordnung für jeden Mandanten hinzufügen.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Option 2: Zuordnen der Daten für eine Bereichszuordnung
Fügen Sie die Bereichszuordnungen für alle Zuordnungen zwischen Mandanten-ID-Bereichen und der Datenbank hinzu.

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Schritt 4, Option 3: Zuordnen der Daten für mehrere Mandanten zu einer Einzeldatenbank
Führen Sie für jeden Mandanten „Add-ListMapping“ aus (Option 1 oben). 

## <a name="checking-the-mappings"></a>Überprüfen der Zuordnungen
Informationen zu vorhandenen Shards und den zugehörigen Zuordnungen können über die folgenden Befehle abgefragt werden:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Zusammenfassung
Nach der Einrichtung können Sie die Clientbibliothek für elastische Datenbanken verwenden. Außerdem können Sie das [datenabhängige Routing](sql-database-elastic-scale-data-dependent-routing.md) und das [Abfragen mehrerer Shards](sql-database-elastic-scale-multishard-querying.md) nutzen.

## <a name="next-steps"></a>Nächste Schritte
Laden Sie die PowerShell-Skripts auf der Seite [Azure SQL DB - Elastic Database tools scripts (Azure SQL-Datenbank – Skripts für Tools für elastische Datenbanken)](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)herunter.

Sie finden diese Tools auch auf GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).

Verschieben Sie Daten mithilfe des Split-Merge-Tools in das Modell mit mehreren Mandanten bzw. aus diesem Modell in ein Modell mit einem einzelnen Mandanten. Siehe [Split-Merge-Tool](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Informationen zu gängigen Datenarchitekturmustern von mehrinstanzenfähigen SaaS-Datenbankanwendungen (Software as a Service) finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Fragen und Funktionswünsche
Bei Fragen erreichen Sie uns im [Forum für SQL-Datenbank](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted), Featureanforderungen können Sie im [Feedbackforum für SQL-Datenbank](https://feedback.azure.com/forums/217321-sql-database/) einreichen.

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

