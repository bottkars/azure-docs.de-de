---
title: "Azure PowerShell-Skriptbeispiel – Skalieren der Dienstinstanz | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Skalieren der Dienstinstanz"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: d412676e54cf23436607951cbc0367774676c9a6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="scale-the-service-instance"></a>Skalieren der Dienstinstanz

Dieses Beispielskript skaliert die API Management-Dienstinstanz und fügt ihr eine Region hinzu.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 3.6 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/api-management/scale-and-addregion-apim-service/scale_and_addregion_apim_service.ps1 "Scale the APIM service instance")]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/overview).

Weitere Azure PowerShell-Beispiele für Azure API Management finden Sie unter [Azure PowerShell samples for API Management](../powershell-samples.md) (Azure PowerShell-Beispiele für API Management).
