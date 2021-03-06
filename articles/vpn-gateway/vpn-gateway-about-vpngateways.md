---
title: "VPN Gateway-Übersicht: Erstellen von standortübergreifenden VPN-Verbindungen mit virtuellen Azure-Netzwerken | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, was ein VPN Gateway ist und wie Sie über das Internet eine VPN-Verbindung mit virtuellen Azure-Netzwerken herstellen. Der Artikel enthält Diagramme mit grundlegenden Verbindungskonfigurationen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: cherylmc
ms.openlocfilehash: ebecbfa3279a71cda005f60c32247e9e95dd6646
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="about-vpn-gateway"></a>Informationen zu VPN Gateway

Ein VPN-Gateway ist eine Art von Gateway für virtuelle Netzwerke, mit dem verschlüsselter Datenverkehr über eine öffentliche Verbindung an einen lokalen Standort gesendet wird. Sie können VPN-Gateways auch verwenden, um verschlüsselten Datenverkehr zwischen virtuellen Azure-Netzwerken über das Microsoft-Netzwerk zu senden. Wenn Sie verschlüsselten Netzwerkdatenverkehr zwischen Ihrem virtuellen Azure-Netzwerk und Ihrem lokalen Standort senden möchten, müssen Sie für Ihr virtuelles Netzwerk ein VPN-Gateway erstellen.

Jedes virtuelle Netzwerk kann nur ein VPN-Gateway aufweisen. Sie können aber mehrere Verbindungen mit dem gleichen VPN-Gateway erstellen. Ein Beispiel hierfür ist eine Verbindungskonfiguration mit mehreren Standorten. Wenn Sie mehrere Verbindungen mit dem gleichen VPN-Gateway erstellen, teilen sich alle VPN-Tunnel (einschließlich Point-to-Site-VPNs) die für das Gateway zur Verfügung stehende Bandbreite.

## <a name="whatis"></a>Was ist ein Gateway für virtuelle Netzwerke?

Ein Gateway für virtuelle Netzwerke besteht aus mindestens zwei virtuellen Computern, die in einem speziellen Subnetz namens „GatewaySubnet“ bereitgestellt werden. Die virtuellen Computer in „GatewaySubnet“ werden beim Erstellen des Gateways für virtuelle Netzwerke erstellt. Virtuelle Computer im Gateway für virtuelle Netzwerke werden so konfiguriert, dass sie spezifische Routingtabellen und Gatewaydienste für das Gateway enthalten. Sie können die virtuellen Computer, die Teil des Gateways für virtuelle Netzwerke sind, nicht direkt konfigurieren, und Sie sollten nie zusätzliche Ressourcen in „GatewaySubnet“ bereitstellen.

Beim Erstellen des Gateways für virtuelle Netzwerke mit dem Gatewaytyp „Vpn“ wird ein bestimmter Gatewaytyp für virtuelle Netzwerke erstellt, mit dem Datenverkehr verschlüsselt wird: ein VPN-Gateway. Die Erstellung eines VPN-Gateways kann bis zu 45 Minuten dauern. Dies liegt daran, dass die virtuellen Computer für das VPN-Gateway im Gatewaysubnetz bereitgestellt und mit den angegebenen Einstellungen konfiguriert werden. Die von Ihnen ausgewählte Gateway-SKU bestimmt, wie leistungsfähig die virtuellen Computer sind.

## <a name="configuring"></a>Konfigurieren von VPN Gateway

Eine VPN Gateway-Verbindung basiert auf mehreren, mit spezifischen Einstellungen konfigurierten Ressourcen. Die meisten der Ressourcen können separat konfiguriert werden. In manchen Fällen ist allerdings eine bestimmte Reihenfolge einzuhalten.

### <a name="settings"></a>Einstellungen

Die Einstellungen, die Sie für die einzelnen Ressourcen auswählen, sind für eine erfolgreiche Verbindungserstellung entscheidend. Informationen zu einzelnen Ressourcen und Einstellungen für VPN Gateway finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md). Dieser Artikel enthält Informationen zu Gatewaytypen, VPN-Typen, Verbindungstypen, Gatewaysubnetzen, lokalen Netzwerkgateways und verschiedenen anderen Ressourceneinstellungen, die Sie ggf. berücksichtigen sollten.

### <a name="tools"></a>Bereitstellungstools

Sie können zunächst mit einem Konfigurationstool wie dem Azure-Portal Ressourcen erstellen und konfigurieren. Später können Sie mit einem anderen Tool (beispielsweise PowerShell) zusätzliche Ressourcen konfigurieren oder ggf. vorhandene Ressourcen ändern. Derzeit können nicht alle Ressourcen und Ressourceneinstellungen über das Azure-Portal konfiguriert werden. Sollte ein bestimmtes Konfigurationstool benötigt werden, ist dies in den Anleitungen der Artikel zu den einzelnen Verbindungstopologien angegeben. 

### <a name="models"></a>Bereitstellungsmodell

Die Konfigurationsschritte für ein VPN-Gateway hängen davon ab, mit welchem Bereitstellungsmodell Sie das virtuelle Netzwerk erstellt haben. Wenn Sie Ihr VNET beispielsweise mit dem klassischen Bereitstellungsmodell erstellt haben, verwenden Sie die Richtlinien und Anleitungen für das klassische Bereitstellungsmodell, um die Einstellungen für das VPN Gateway zu erstellen und zu konfigurieren. Weitere Informationen zu Bereitstellungsmodellen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="gwsku"></a>Gateway-SKUs

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

## <a name="diagrams"></a>Diagramme zur Verbindungstopologie

Wichtig: Für VPN-Gateway-Verbindungen sind verschiedene Konfigurationen verfügbar. Sie müssen ermitteln, welche Konfiguration am besten zu Ihren Anforderungen passt. Die folgenden Abschnitte enthalten Informationen und Topologiediagramme zu den folgenden VPN-Gatewayverbindungen: Die folgenden Abschnitte enthalten Tabellen mit Folgendem:

* Verfügbares Bereitstellungsmodell
* Verfügbare Konfigurationstools
* Direkte Links zu Artikeln, sofern verfügbar

Orientieren Sie sich bei der Wahl einer geeigneten Verbindungstopologie an den Diagrammen und Beschreibungen. Die Diagramme zeigen die grundlegenden Topologien, aber Sie können auch komplexere Topologien erstellen, indem Sie die Diagramme als Anhaltspunkte verwenden.

## <a name="s2smulti"></a>Site-to-Site und Multi-Site (IPsec-/IKE-VPN-Tunnel)

### <a name="S2S"></a>Site-to-Site

Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für S2S-Verbindungen wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das sich nicht hinter einer NAT-Einheit befindet. S2S-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.   

![Beispiel für Site-to-Site-Verbindung per Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Multi-Site

Bei dieser Art von Verbindung handelt es sich um eine Abwandlung der Site-to-Site-Verbindung. Sie erstellen mehrere VPN-Verbindung über Ihr Gateway für virtuelle Netzwerke, durch die in der Regel mehrere lokale Standorte verbunden werden. Bei Verwendung mehrerer Verbindungen müssen Sie den VPN-Typ „RouteBased“ verwenden (wird bei Verwendung klassischer VNets als dynamisches Gateway bezeichnet). Da jedes virtuelle Netzwerk nur über ein einzelnes VPN-Gateway verfügen kann, wird die verfügbare Bandbreite von allen über das Gateway laufenden Verbindungen gemeinsam genutzt. Dies wird häufig als Multi-Site-Verbindung bezeichnet.

![Beispiel für Multi-Site-Verbindung per Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Bereitstellungsmodelle und -methoden für Site-to-Site und Multi-Site

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Point-to-Site (VPN über IKEv2 oder SSTP)

Mit einer P2S-VPN-Gatewayverbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung mit Azure-VNETs herstellen möchten. Wenn nur einige wenige Clients eine Verbindung mit einem VNET herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung.

Im Gegensatz zu S2S-Verbindungen ist für P2S-Verbindungen keine lokale öffentliche IP-Adresse bzw. kein VPN-Gerät erforderlich. P2S-Verbindungen können mit S2S-Verbindungen über das gleiche VPN-Gateway verwendet werden – vorausgesetzt, alle Konfigurationsanforderungen beider Verbindungen sind kompatibel. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [About Point-to-Site VPN](point-to-site-about.md) (Informationen zu P2S-VPN).


![Beispiel für Point-to-Site-Verbindung per Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/point-to-site.png)

### <a name="deployment-models-and-methods-for-p2s"></a>Bereitstellungsmodelle und -methoden für P2S

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>VNet-zu-VNet-Verbindungen (IPsec-/IKE-VPN-Tunnel)

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines VNet mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPsec/IKE bereitzustellen. Die VNet-zu-VNet-Kommunikation kann sogar mit Multi-Site-Verbindungskonfigurationen kombiniert werden. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren.

Die verbundenen VNets können wie folgt angeordnet sein:

* In derselben Region oder in verschiedenen Regionen
* In demselben Abonnement oder in verschiedenen Abonnements 
* In demselben Bereitstellungsmodell oder in verschiedenen Bereitstellungsmodellen

![Beispiel für VNet-zu-VNet-Verbindung per Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Verbindungen zwischen Bereitstellungsmodellen

In Azure sind zurzeit zwei Bereitstellungsmodelle verfügbar: klassisches Modell und Resource Manager-Modell. Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. Sie können eine Verbindung zwischen den VNets erstellen, damit die Ressourcen in einem VNet direkt mit Ressourcen im anderen VNet kommunizieren können.

### <a name="vnet-peering"></a>VNet-Peering

Sofern Ihr virtuelles Netzwerk bestimmte Anforderungen erfüllt, können Sie Ihre Verbindung ggf. mithilfe von VNet-Peering erstellen. Beim VNet-Peering wird kein Gateway für das virtuelle Netzwerk verwendet. Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Bereitstellungsmodelle und -methoden für VNet-zu-VNet

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (private Verbindung)

Mit Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Mit ExpressRoute können Sie Verbindungen mit Microsoft-Clouddiensten herstellen, z. B. Microsoft Azure, Office 365 und CRM Online. Die Konnektivität kann über ein Any-to-Any-Netzwerk (IP VPN), ein Point-to-Point-Ethernet-Netzwerk oder eine virtuelle Querverbindung über einen Konnektivitätsanbieter in einer Co-Location-Einrichtung bereitgestellt werden.

ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Auf diese Weise können ExpressRoute-Verbindungen eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet bieten.

Bei ExpressRoute-Verbindungen kommt zwar kein VPN-Gateway zur Anwendung, als Teil der erforderlichen Konfiguration wird jedoch ein Gateway für virtuelle Netzwerke verwendet. Bei einer ExpressRoute-Verbindung wird das Gateway für virtuelle Netzwerke nicht mit dem Gatewaytyp „Vpn“, sondern mit „ExpressRoute“ konfiguriert. Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Parallel bestehende Site-to-Site- und ExpressRoute-Verbindungen

ExpressRoute ist eine direkte, private Verbindung aus Ihrem WAN mit Microsoft Services (einschließlich Azure), die nicht über das öffentliche Internet verläuft. Site-to-Site-VPN-Datenverkehr wird verschlüsselt über das öffentliche Internet gesendet. Die Möglichkeit, Site-to-Site-VPN- und ExpressRoute-Verbindungen für dasselbe virtuelle Netzwerk zu konfigurieren, hat mehrere Vorteile.

Sie können eine Site-to-Site-VPN-Verbindung als sicheren Failoverpfad für ExpressRoute konfigurieren oder für die Verbindung mit Websites nutzen, die nicht Teil Ihres Netzwerks, aber über ExpressRoute verbunden sind. Diese Konfiguration erfordert zwei Gateways für das gleiche virtuelle Netzwerk: eins mit dem Gatewaytyp „Vpn“ und eins mit dem Gatewaytyp „ExpressRoute“.

![Beispiel für gleichzeitig bestehende ExpressRoute- und VPN Gateway-Verbindungen](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute-coexist"></a>Koexistenz von Bereitstellungsmodellen und -methoden für S2S und ExpressRoute

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Preise

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

Weitere Informationen zu Gateway-SKUs für VPN Gateway finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="faq"></a>Häufig gestellte Fragen

Häufig gestellte Fragen zu VPN-Gateways finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md).

## <a name="next-steps"></a>Nächste Schritte

- Planen Sie Ihre VPN Gateway-Konfiguration. Siehe [Planung und Entwurf für VPN Gateway](vpn-gateway-plan-design.md).
- Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md).
- Sehen Sie sich die [Abonnements und Diensteinschränkungen](../azure-subscription-service-limits.md#networking-limits) an.
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.
