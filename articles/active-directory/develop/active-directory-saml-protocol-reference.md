---
title: Referenz zum Azure AD-SAML-Protokoll | Microsoft Docs
description: "Dieser Artikel enthält eine Übersicht über die SAML-Profile für das einmalige Anmelden und Abmelden in Azure Active Directory."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: d5ffba5d0c409fe9de7a9e82c6faa4ca2702ab95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# Verwendung des SAML-Protokolls durch Azure Active Directory
Azure Active Directory (Azure AD) verwendet das SAML 2.0-Protokoll, um es Anwendungen zu ermöglichen, für Benutzer das einmalige Anmelden bereitzustellen. In den SAML-Profilen zum [einmaligen Anmelden](active-directory-single-sign-on-protocol-reference.md) und [einmaligen Abmelden](active-directory-single-sign-out-protocol-reference.md) von Azure AD wird beschrieben, wie SAML-Assertionen, -Protokolle und -Bindungen im Identitätsanbieterdienst verwendet werden.

Für das SAML-Protokoll ist es erforderlich, dass der Identitätsanbieter (Azure AD) und der Dienstanbieter (die Anwendung) Informationen übereinander austauschen.

Wenn eine Anwendung unter Azure AD registriert ist, registriert der App-Entwickler verbundbezogene Informationen unter Azure AD. Dies gilt auch für den **Umleitungs-URI** und den **Metadaten-URI** der Anwendung.

Azure AD nutzt den **Metadaten-URI** des Clouddiensts, um den Signaturschlüssel und den Abmelde-URI des Clouddiensts abzurufen. Falls die Anwendung keinen Metadaten-URI unterstützt, muss sich der Entwickler an den Microsoft Support wenden, um den Abmelde-URI und den Signaturschlüssel anzugeben.

Azure Active Directory macht mandantenspezifische und allgemeine (mandantenunabhängige) Endpunkte für das einmalige Anmelden und das einmalige Abmelden verfügbar. Bei diesen URLs handelt es sich um adressierbare Speicherorte – nicht nur um Bezeichner –, damit Sie auf den Endpunkt zugreifen können, um die Metadaten zu lesen.

* Der mandantenspezifische Endpunkt befindet sich unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Der Platzhalter <TenantDomainName> steht für einen registrierten Domänennamen oder die TenantID-GUID eines Azure AD-Mandanten. Die Verbundmetadaten des Mandanten „contoso.com“ befinden sich beispielsweise hier: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml.

* Der mandantenunabhängige Endpunkt befindet sich unter `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Bei dieser Endpunktadresse wird anstelle eines Mandantendomänennamens oder einer ID das Wort **common** verwendet.

Informationen zu den Verbundmetadaten-Dokumenten, die von Azure AD veröffentlicht werden, finden Sie unter [Verbundmetadaten](active-directory-federation-metadata.md).
