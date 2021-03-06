---
title: Verwalten von Serveradministratoren in Azure Analysis Services | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in Azure Serveradministratoren für einen Analysis Services-Server verwalten."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-server-administrators"></a>Verwalten von Serveradministratoren
„Serveradministratoren“ muss eine gültige Benutzergruppe in Azure Active Directory (Azure AD) für den Mandanten sein, in dem sich der Server befindet. Zum Verwalten von Serveradministratoren können Sie die Option **Analysis Services-Administratoren** auf dem Blatt für die Steuerung des Servers im Azure-Portal oder die Servereigenschaften in SSMS verwenden. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>So fügen Sie Serveradministratoren über das Azure-Portal hinzu
1. Klicken Sie auf dem Steuerungsblatt für den Server auf **Analysis Services-Administratoren**.
2. Klicken Sie auf dem Blatt **\<Servername >-Analysis Services-Administratoren** auf **Hinzufügen**.
3. Wählen Sie auf dem Blatt **Serveradministratoren hinzufügen** Benutzerkonten in Ihrem Azure AD aus, oder laden Sie externe Benutzer per E-Mail ein.

    ![Serveradministratoren im Azure-Portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>So fügen Sie Serveradministratoren über SSMS hinzu
1. Klicken Sie mit der rechten Maustaste auf den Server, und wählen Sie dann **Eigenschaften** aus.
2. Klicken Sie in **Eigenschaften für Analysis-Server** auf **Sicherheit**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie dann die E-Mail-Adresse eines Benutzers oder einer Gruppe in Ihrem Azure AD ein.
   
    ![Hinzufügen von Serveradministratoren in SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Nächste Schritte 
[Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md)  
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)  
[Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md)  

