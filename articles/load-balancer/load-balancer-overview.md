---
title: "Übersicht über Azure Load Balancer | Microsoft-Dokumentation"
description: "Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Erfahren Sie, wie der Load Balancer funktioniert, und nutzen Sie ihn in der Cloud."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: ecf1fc38d2b9fd54fe5b00db616224a0848179fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-load-balancer-overview"></a>Übersicht über Azure Load Balancer

Der Azure Load Balancer bietet hohe Verfügbarkeit und Netzwerkleistung für Ihre Anwendungen. Es ist ein Layer-4-Lastenausgleichsmodul (TCP, UDP), das eingehenden Datenverkehr auf funktionierende Dienstinstanzen verteilt, die in einer Lastenausgleichsgruppe definiert sind.

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer kann für Folgendes konfiguriert werden:

* Lastenausgleich des eingehenden Internetdatenverkehrs für virtuelle Computer. Diese Konfiguration wird als [Lastenausgleich für Internetzugriff](load-balancer-internet-overview.md)bezeichnet.
* Lastenausgleich für Datenverkehr zwischen virtuellen Computern in einem virtuellen Netzwerk, zwischen virtuellen Computern in Clouddiensten oder zwischen lokalen und virtuellen Computern in einem standortübergreifenden virtuellen Netzwerk. Diese Konfiguration wird als [interner Lastenausgleich](load-balancer-internal-overview.md)bezeichnet.
* Weiterleiten von externem Datenverkehr an eine bestimmte Instanz eines virtuellen Computers.

Alle Ressourcen in der Cloud benötigen eine öffentliche IP-Adresse, damit sie im Internet erreichbar sind. Die Cloudinfrastruktur in Azure verwendet für ihre Ressourcen nicht routingfähige IP-Adressen. Für die Kommunikation mit dem Internet verwendet Azure die Netzwerkadressübersetzung (Network Address Translation, NAT) mit öffentlichen IP-Adressen.

## <a name="load-balancer-features"></a>Lastenausgleichsfunktionen

* Hash-basierte Verteilung

    Der Azure Load Balancer verwendet einen Verteilungsalgorithmus auf Hashbasis. Standardmäßig wird ein Fünf-Tupel-Hash (Quell-IP, Quellport, IP-Zieladresse, Zielport und Protokolltyp) verwendet, um den Datenverkehr verfügbaren Servern zuzuordnen. Dabei wird Stickiness nur *innerhalb* einer Transportsitzung angeboten. Pakete in derselben TCP- oder UDP-Sitzung werden an die gleiche Instanz hinter dem Lastenausgleichs-Endpunkt geleitet. Wenn der Client die Verbindung schließt und erneut öffnet oder eine neue Sitzung über die gleiche Quell-IP startet, wird der Quellport geändert. Dadurch wird der Datenverkehr möglicherweise an einen anderen Endpunkt in einem anderen Rechenzentrum geleitet.

    Weitere Informationen finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md). Die folgende Grafik zeigt die hashbasierte Verteilung:

    ![Hashbasierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

    Abbildung: Hashbasierte Verteilung

* Portweiterleitung

    Mit dem Azure Load Balancer steuern Sie, wie die eingehende Kommunikation verwaltet wird. Diese Kommunikation beinhaltet Datenverkehr, der von Internethosts, von virtuellen Computern in anderen Clouddiensten oder von virtuellen Netzwerken initiiert wird. Diese Steuerung wird durch einen Endpunkt (auch als Eingabeendpunkt bezeichnet) abgebildet.

    Ein Eingabeendpunkt lauscht an einem öffentlichen Port und leitet Datenverkehr an einen internen Port weiter. Sie können dieselben Ports einem internen oder externen Endpunkt zuordnen oder einen anderen Port verwenden. Beispiel: Sie können einen Webserver konfigurieren, der an Port 81 lauscht, während Port 80 dem öffentlichen Endpunkt zugeordnet ist. Die Erstellung eines öffentlichen Endpunkts löst die Erstellung einer Load Balancer-Instanz aus.

    Beim Erstellen über das Azure-Portal erstellt das Portal für den virtuellen Computer automatisch Endpunkte für den Datenverkehr von Remotedesktopprotokoll (RDP) und Windows PowerShell-Remotesitzungen. Über diese Endpunkte können Sie den virtuellen Computer remote über das Internet verwalten.

* Automatische Neukonfiguration

    Der Azure Load Balancer konfiguriert sich selbst sofort, wenn Sie Instanzen nach oben oder unten skalieren. Diese Neukonfiguration kann z.B. erfolgen, wenn Sie die Anzahl der Instanzen für Web-/Workerrollen in einem Clouddienst erhöhen oder wenn Sie weitere virtuelle Computer in die gleiche Gruppe mit Lastenausgleich aufnehmen.

* Dienstüberwachung

    Azure Load Balancer kann die Integrität der verschiedenen Serverinstanzen testen. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Vorhandene Verbindungen sind nicht betroffen.

    Drei Testarten werden unterstützt:

    + **Gast-Agent-Test (nur PaaS-VMs):** Der Lastenausgleich nutzt den Gast-Agent im virtuellen Computer. Der Gast-Agent lauscht und antwortet nur dann mit dem HTTP-Code „200 OK“, wenn die Instanz sich im Zustand „Bereit“ befindet (d.h. nicht beschäftigt ist bzw. recycelt oder angehalten wird). Wenn der Agent nicht mit dem HTTP-Code „200 OK“ antwortet, kennzeichnet das Lastenausgleichsmodul die Instanz als nicht reagierend und sendet keinen Datenverkehr mehr an diese Instanz. Das Lastenausgleichsmodul pingt die Instanz weiterhin. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Load Balancer wieder Datenverkehr an diese Instanz. Wenn Sie eine Webrolle verwenden, wird Ihr Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Das bedeutet, dass Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) nicht an den Gast-Agent gemeldet werden und der Load Balancer nicht erkennen kann, dass diese Instanz aus der Rotation entfernt werden soll.
    + **Benutzerdefinierter HTTP-Test:** Dieser Test setzt den (Gast-Agent-)Standardtest außer Kraft. Sie können damit Ihre eigene Logik zum Ermitteln der Integrität der Rolleninstanz erstellen. Der Load Balancer testet regelmäßig Ihren Endpunkt (standardmäßig alle 15 Sekunden). Die Instanz wird als rotierend betrachtet, wenn sie innerhalb des Zeitlimits (standardmäßig 31 Sekunden) mit einem TCP-ACK oder HTTP-Code 200 reagiert. Dies ist hilfreich für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Rotation des Lastenausgleichs. Beispielsweise können Sie die Instanz so konfigurieren, dass sie einen anderen Status als 200 zurückgibt, wenn die Instanz bei über 90 % CPU-Auslastung liegt. Für Webrollen, die „w3wp.exe“ verwenden, wird die Website außerdem automatisch überwacht, da Fehler im Websitecode einen anderen Status als 200 an den Test zurückgeben.
    + **Benutzerdefinierter TCP-Test:** TCP-Tests basieren auf der erfolgreichen Einrichtung von TCP-Sitzungen an einem definierten Testport.

    Weitere Informationen finden Sie unter [LoadBalancerProbe-Schema](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Quell-NAT

    Der gesamte von Ihrem Dienst gesendete Internetdatenverkehr unterliegt einer Quell-NAT (SNAT, Source Network Address Translation) mit der gleichen VIP-Adresse wie für eingehenden Datenverkehr. SNAT bietet wichtige Vorteile:

    + Sie ermöglicht einfache Upgrades und Notfallwiederherstellung von Diensten, da die VIP-Adresse dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
    + Sie vereinfacht die Verwaltung von Zugriffssteuerungslisten. Zugriffssteuerungslisten, die als VIPs ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden.

    Die Load Balancer-Konfiguration unterstützt vollständige Cone-NAT für UDP. Vollständige Cone-NAT ist ein NAT-Typ, bei dem der Port eingehende Verbindungen von jedem externen Host (als Reaktion auf eine externe Anforderung) erlaubt.

    Für jede neue ausgehende Verbindung, die von einem virtuellen Computer initiiert wird, wird vom Lastenausgleich auch ein ausgehender Port zugeordnet. Dem externen Host wird Datenverkehr von einem Port angezeigt, der einer virtuellen IP (VIP) zugewiesen ist. In Szenarien, die eine große Anzahl von ausgehenden Verbindungen erfordern, empfiehlt sich die Verwendung [öffentlicher IP-Adressen auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md) , damit die virtuellen Computer eine dedizierte ausgehende IP-Adresse für SNAT besitzen. Dies reduziert das Risiko einer Portauslastung.

    Im Artikel zu [ausgehenden Verbindungen](load-balancer-outbound-connections.md) finden Sie weitere Informationen zu diesem Thema.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Unterstützung für mehrere IP-Adressen mit Lastenausgleich für virtuelle Computer
Sie können einer Gruppe von virtuellen Computern mehr als eine öffentliche IP-Adresse mit Lastenausgleich zuweisen. Mit dieser Funktion können Sie mehrere SSL-Websites und/oder mehrere Listener für SQL Server-AlwaysOn-Verfügbarkeitsgruppen in der gleichen Gruppe virtueller Computer hosten. Weitere Informationen finden Sie unter [Mehrere VIPs pro Clouddienst](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Einschränkungen

Back-End-Pools des Load Balancer können mit Ausnahme des Basic-Tarifs beliebige VM-SKUs enthalten.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lastenausgleich für Internetzugriff](load-balancer-internet-overview.md).

- Erfahren Sie mehr über den [internen Lastenausgleich](load-balancer-internal-overview.md).

- Erstellen eines [Lastenausgleichs mit Internetzugriff](load-balancer-get-started-internet-portal.md)

- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.

