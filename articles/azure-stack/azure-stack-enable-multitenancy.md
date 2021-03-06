---
title: "Aktivieren der Mehrinstanzenfähigkeit in Azure Stack | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mehrere Azure Active Directory-Verzeichnisse in Azure Stack unterstützen."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: bdf92b8b73dca55e819545913931c0a79a366486
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="enable-multi-tenancy-in-azure-stack"></a>Aktivieren der Mehrinstanzenfähigkeit in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können Azure Stack so konfigurieren, dass Benutzer aus mehreren Azure Active Directory-Mandanten (Azure AD) Dienste in Azure Stack verwenden können. Betrachten Sie beispielsweise das folgende Szenario:

 - Sie sind Dienstadministrator von contoso.onmicrosoft.com, wo Azure Stack installiert ist.
 - Mary ist Verzeichnisadministrator von fabrikam.onmicrosoft.com, wo sich Gastbenutzer befinden. 
 - Marys Unternehmen bezieht IaaS- und PaaS-Dienste von Ihrem Unternehmen und muss es Benutzern aus dem Gastverzeichnis (fabrikam.onmicrosoft.com) ermöglichen, sich bei Azure Stack-Ressourcen in contoso.onmicrosoft.com anzumelden und diese zu verwenden.

Der vorliegende Leitfaden beschreibt die erforderlichen Schritte – im Kontext dieses Szenarios –, um die Mehrinstanzenfähigkeit in Azure Stack zu konfigurieren.  In diesem Szenario müssen sowohl Sie als auch Mary einige Schritte ausführen, um es Benutzern von Fabrikam zu ermöglichen, sich bei der Azure Stack-Bereitstellung in Contoso anzumelden und dort Dienste zu nutzen.  

## <a name="before-you-begin"></a>Voraussetzungen
Bevor Sie die Mehrinstanzenfähigkeit in Azure Stack konfigurieren können, müssen einige Voraussetzungen erfüllt werden:
  
 - Mary und Sie müssen Verwaltungsschritte sowohl in dem Verzeichnis, in dem Azure Stack installiert ist (Contoso), als auch im Gastverzeichnis (Fabrikam) koordinieren.  
 - Stellen Sie sicher, dass Sie die PowerShell für Azure Stack [installiert](azure-stack-powershell-install.md) und [konfiguriert](azure-stack-powershell-configure-admin.md) haben.
 - [Laden Sie die Azure Stack-Tools](azure-stack-powershell-download.md) herunter, und importieren Sie die Module „Connect“ und „Identity“:

    ````PowerShell
        Import-Module .\Connect\AzureStack.Connect.psm1
        Import-Module .\Identity\AzureStack.Identity.psm1
    ```` 
 - Mary benötigt [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)-Zugriff auf Azure Stack. 

## <a name="configure-azure-stack-directory"></a>Konfigurieren des Azure Stack-Verzeichnisses
In diesem Abschnitt konfigurieren Sie Azure Stack so, dass Anmeldungen aus Fabrikam-Azure AD-Verzeichnismandanten zugelassen werden.

### <a name="onboard-guest-directory-tenant"></a>Integrieren des Gastverzeichnismandanten
Als nächstes integrieren Sie den Gastverzeichnismandanten (Fabrikam) in Azure Stack.  Dieser Schritt konfiguriert Azure Resource Manager so, dass Benutzer und Dienstprinzipale aus dem Gastverzeichnismandanten akzeptiert werden.

````PowerShell
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local" `
 -ResourceGroupName $ResourceGroupName
````



## <a name="configure-guest-directory"></a>Konfigurieren des Gastverzeichnisses
Nachdem Sie die Schritte im Azure Stack-Verzeichnis abgeschlossen haben, muss Mary zustimmen, dass Azure Stack auf das Gastverzeichnis zugreift, und Azure Stack beim Gastverzeichnis registrieren. 

### <a name="registering-azure-stack-with-the-guest-directory"></a>Registrieren von Azure Stack beim Gastverzeichnis
Sobald Mary als Administrator des Gastverzeichnisses dem Zugriff auf das Verzeichnis von Fabrikam durch Azure Stack zugestimmt hat, muss sie Azure Stack beim Fabrikam-Verzeichnismandanten registrieren.

````PowerShell
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````
## <a name="direct-users-to-sign-in"></a>Weiterleiten von Benutzern zur Anmeldung
Nachdem Mary und Sie die Schritte zum Integrieren von Marys Verzeichnis abgeschlossen haben, kann Mary Fabrikam-Benutzer zur Anmeldung weiterleiten.  Fabrikam-Benutzer (also Benutzer mit dem Suffix „fabrikam.onmicrosoft.com“) melden sich hier an: https://portal.local.azurestack.external.  

Mary leitet alle [fremden Prinzipale](../active-directory/active-directory-understanding-resource-access.md) im Fabrikam-Verzeichnis (also Benutzer im Fabrikam-Verzeichnis ohne das Suffix „fabrikam.onmicrosoft.com“) zur Anmeldung bei dieser Adresse weiter: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Wenn diese Benutzer diese URL nicht verwenden, werden sie an ihr Standardverzeichnis (Fabrikam) weitergeleitet und erhalten eine Fehlermeldung, die besagt, dass ihr Administrator nicht zugestimmt hat.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten delegierter Anbieter](azure-stack-delegated-provider.md)
- [Schlüsselkonzepte für Azure Stack](azure-stack-key-features.md)
