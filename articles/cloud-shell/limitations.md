---
title: "Einschränkungen von Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Übersicht über die Einschränkungen von Azure Cloud Shell"
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: fe325cf5fa5e3d4b0a188599de7308cf2008b458
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Einschränkungen von Azure Cloud Shell
Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

## <a name="general-limitations"></a>Allgemeine Einschränkungen

### <a name="system-state-and-persistence"></a>Systemstatus und Persistenz
Der Computer mit Ihrer Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung 20 Minuten lang inaktiv ist. Für Cloud Shell muss eine Dateifreigabe bereitgestellt werden. Daher muss Ihr Abonnement Speicherressourcen für den Zugriff auf Cloud Shell einrichten können. Weitere Überlegungen:
* Mit eingebundenem Speicher werden nur Änderungen im `clouddrive`-Verzeichnis gespeichert. In Bash bleibt Ihr `$Home`-Verzeichnis ebenfalls erhalten.
* Dateifreigaben können nur innerhalb Ihrer [zugewiesenen Region](persisting-shell-storage.md#mount-a-new-clouddrive) eingebunden werden.
  * Führen Sie in Bash `env` aus, um für Ihre Region die Einstellung `ACC_LOCATION` zu ermitteln.
* Azure Files unterstützt nur Konten mit lokal redundantem Speicher und georedundantem Speicher.

### <a name="browser-support"></a>Browserunterstützung
Cloud Shell unterstützt die aktuellen Versionen von Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox und Apple Safari. Safari im privaten Modus wird nicht unterstützt.

### <a name="copy-and-paste"></a>Kopieren und Einfügen

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Für einen bestimmten Benutzer kann nur eine Shell aktiv sein.
Benutzer können jeweils nur eine Art von Shell zu einem bestimmten Zeitpunkt starten, entweder **Bash** oder **PowerShell**. Möglicherweise führen Sie jedoch mehrere Instanzen von Bash oder PowerShell gleichzeitig aus. Der Wechsel zwischen Bash oder PowerShell führt dazu, dass Cloud Shell neu gestartet wird, wodurch bestehende Sitzungen beendet werden.

### <a name="usage-limits"></a>Usage limits (Nutzungseinschränkungen)
Cloud Shell ist für interaktive Anwendungsfälle konzipiert. Daher werden lange Sitzungen ohne Interaktion ohne Vorwarnung beendet.

## <a name="bash-limitations"></a>Bash-Einschränkungen

### <a name="user-permissions"></a>Benutzerberechtigungen
Berechtigungen werden als reguläre Benutzer ohne sudo-Zugriff festgelegt. Installationen außerhalb des Verzeichnisses `$Home` werden nicht gespeichert.
Bestimmte Befehle im Verzeichnis `git clone` (beispielsweise `clouddrive`) verfügen zwar nicht über die entsprechenden Berechtigungen, das Verzeichnis `$Home` hingegen schon.

### <a name="editing-bashrc"></a>Bearbeiten von „.bashrc“
Gehen Sie bei der Bearbeitung von „.bashrc“ vorsichtig vor, da sonst unerwartete Fehler in Cloud Shell auftreten können. 

### <a name="bashhistory"></a>.bash_history
Der Verlauf der Bashbefehle ist möglicherweise aufgrund einer Unterbrechung der Cloud Shell-Sitzung oder gleichzeitiger Sitzungen inkonsistent.

## <a name="powershell-limitations"></a>PowerShell-Einschränkungen

### <a name="slow-startup-time"></a>Langsame Startzeit
Die Initialisierung von PowerShell in Azure Cloud Shell kann während der Vorschau bis zu 60 Sekunden dauern.

### <a name="no-home-directory-persistence"></a>Keine $Home-Verzeichnispersistenz
Von beliebigen Anwendungen (z. B.: git, vim usw.) in „$Home“ geschriebene Daten bleiben zwischen PowerShell-Sitzungen nicht erhalten.  Informationen zu einer Problemumgehung [finden Sie hier](troubleshooting.md#powershell-resolutions).

### <a name="error-if-azurerm-module-is-updated-from-powershellgallery"></a>Fehler, wenn das AzureRM-Modul über den PowerShell-Katalog aktualisiert wird
Wenn Sie die AzureRM-Module auf die aktuelle Version (4.4.0) aktualisieren, wird beim Start von Cloud Shell unter Umständen der folgende Fehler angezeigt:
``` powershell
VERBOSE: Authenticating to Azure ...
Import-Module : Method 'RemoveUser' in type 'Microsoft.Azure.PSCloudConsole.ADAuth.ADAuthFactory' from assembly 'Microsoft.Azure.PSCloudConsole.ADAuth, Version=0.0.0.0,
Culture=neutral, PublicKeyToken=null' does not have an implementation.
At C:\Program Files\WindowsPowerShell\Modules\PSCloudShellADAuth\PSCloudShellADAuth.psm1:12 char:1
+ Import-Module -Name $AdAuthModulePath -PassThru -Force
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Import-Module], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.PowerShell.Commands.ImportModuleCommand

Unable to find type [Microsoft.Azure.PSCloudConsole.ADAuth.ADAuthFactory].
At C:\Users\ContainerAdministrator\PSCloudShellStartup.ps1:94 char:9
+         [Microsoft.Azure.PSCloudConsole.ADAuth.ADAuthFactory]::Update ...
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (Microsoft.Azure...h.ADAuthFactory:TypeName) [], RuntimeException
    + FullyQualifiedErrorId : TypeNotFound
```

## <a name="next-steps"></a>Nächste Schritte
[Problembehandlung für Cloud Shell](troubleshooting.md) <br>
[Schnellstart für Bash](quickstart.md) <br>
[Schnellstart für PowerShell](quickstart-powershell.md)
