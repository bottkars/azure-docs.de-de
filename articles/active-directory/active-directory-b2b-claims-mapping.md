---
title: "Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit in Azure Active Directory | Microsoft-Dokumentation"
description: "Referenz für die Zuordnung von Ansprüchen für die Azure Active Directory B2B-Zusammenarbeit"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 439219087d0a5027c729e9d0e0ecb90bd2a9de3c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt die Anpassung der Ansprüche, die im SAML-Token für B2B-Zusammenarbeitsbenutzer ausgegeben werden. Wenn sich ein Benutzer bei der Anwendung authentifiziert, stellt Azure AD ein SAML-Token für die App aus, das Informationen (so genannte „Ansprüche“) über den Benutzer enthält, die ihn eindeutig identifizieren. Dazu gehören normalerweise der Benutzername, die E-Mail-Adresse, der Vorname und der Nachname des Benutzers. Sie können die im SAML-Token an die Anwendung übermittelten Ansprüche auf der Registerkarte „Attribute“ anzeigen oder bearbeiten.

Es gibt zwei Gründe dafür, dass Sie die im SAML-Token ausgegebenen Ansprüche möglicherweise bearbeiten müssen.

1. Die Anwendung wurde so geschrieben, dass sie einen anderen Satz an Anspruchs-URIs oder Anspruchswerten erfordert.

2. Die Anwendung erfordert, dass es sich beim NameIdentifier-Anspruch nicht um den in Azure Active Directory gespeicherten Benutzerprinzipalnamen handelt.

  ![Anzeigen von Ansprüchen im SAML-Token](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Informationen zum Hinzufügen und Bearbeiten von Ansprüchen finden Sie in diesem Artikel zur Anpassung von Ansprüchen: [Anpassen ausgestellter Ansprüche im SAML-Token für bereits in Azure Active Directory integrierte Apps](develop/active-directory-saml-claims-customization.md). Aus Sicherheitsgründen ist eine mandantenübergreifende Zuordnung von NameID und UPN für Benutzer der B2B-Zusammenarbeit nicht möglich.


## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Externe Office 365-Freigaben](active-directory-b2b-o365-external-user.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)
