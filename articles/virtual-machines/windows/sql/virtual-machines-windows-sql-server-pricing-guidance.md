---
title: "Effektives Verwalten der Kosten für SQL Server auf virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Enthält bewährte Methoden zum Auswählen des richtigen Preismodells für virtuelle SQL Server-Computer."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/17/2017
ms.author: jroth
ms.openlocfilehash: fe04aa382a3e829b201dbe92442f841fb29fff15
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Preisinformationen für virtuelle Azure-Computer mit SQL Server

Dieser Artikel enthält Preisinformationen zu virtuellen SQL Server-Computern in Azure. Verschiedene Optionen wirken sich auf die Kosten aus, und es ist wichtig, das richtige Image auszuwählen, damit Kosten und Geschäftsanforderungen im Gleichgewicht sind.

## <a name="free-licensed-sql-server-editions"></a>SQL Server-Editionen mit kostenlosen Lizenzen

Wenn Sie Funktionen entwickeln oder testen oder eine Machbarkeitsstudie erstellen möchten, verwenden Sie die **SQL Server Developer Edition** mit kostenloser Lizenz. Diese Edition bietet den vollständigen Funktionsumfang der SQL Server Enterprise Edition, daher können Sie jede beliebige Anwendung damit erstellen. Sie darf allerdings nicht in der Produktion ausgeführt werden. Bei einem virtuellen SQL Server Developer-Computer werden nur die Kosten des virtuellen Computers berechnet, nicht die SQL Server-Lizenzierung.

Wenn Sie eine kleine Arbeitsauslastung in der Produktion ausführen möchten (<4 Kerne, <1 GB Arbeitsspeicher, <10 GB/Datenbank), verwenden Sie die **SQL Server Express Edition** mit kostenloser Lizenz. Bei einem virtuellen SQL Server Express-Computer werden nur die Kosten des virtuellen Computers berechnet, nicht die SQL-Lizenzierung.

Für solche Entwicklungs- oder Testarbeitsauslastungen oder Arbeitsauslastungen mit geringem Umfang können Sie auch Geld sparen, indem Sie eine kleinere VM-Größe auswählen, die für diese Arbeitsauslastungen ausreichend ist. Die Größe DS1v2 beispielsweise eignet sich für diese Art Arbeitsauslastungen.

Informationen zum Erstellen eines virtuellen Azure-Computers mit SQL Server 2017 mit einem dieser Images finden Sie unter den folgenden Links:

| Plattform | Kostenlos lizenzierte Images |
|---|---|
| Windows Server 2016 | [Virtueller Azure-Computer mit SQL Server 2017 Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [Virtueller Azure-Computer mit SQL Server 2017 Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [Virtueller Azure-Computer mit SQL Server 2017 Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [Virtueller Azure-Computer mit SQL Server 2017 Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Kostenpflichtige SQL Server-Editionen

Für umfangreichere Arbeitsauslastungen in der Produktion verwenden Sie eine der folgenden SQL Server-Editionen:

| SQL Server-Edition | Workload |
|-----|-----|
| Web | Kleine Websites |
| Standard | Kleine bis mittelgroße Arbeitsauslastungen |
| Enterprise | Umfangreiche oder geschäftskritische Arbeitsauslastungen|

Es gibt zwei Möglichkeiten, für die Lizenzierung dieser SQL Server-Editionen zu zahlen: *Nutzungsbasierte Bezahlung* oder *BYOL (Bring Your Own License)*.

### <a name="pay-per-usage"></a>Nutzungsbasierte Bezahlung

Die **Bezahlung für SQL Server-Lizenzen je nach Nutzung** bedeutet, dass die minutenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz einschließen. Die Preise für die verschiedenen SQL-Server-Editionen (Web, Standard, Enterprise) finden Sie auf der [Preisseite für virtuelle Azure-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). Die Kosten sind für alle SQL Server-Versionen (2012 SP3 bis 2017) gleich. Ebenso wie bei der SQL Server-Lizenzierung im Allgemeinen richten sich die Lizenzierungskosten pro Minute nach der Anzahl der VM-Kerne.

Die nutzungsbasierte Bezahlung der SQL Server-Lizenzierung empfiehlt sich in folgenden Fällen:

- Temporär oder regelmäßig auftretende Arbeitsauslastungen. Beispiele: eine App, die jedes Jahr einige Monate lang ein Ereignis unterstützen muss, oder Geschäftsanalysen, die jeden Montag ausgeführt werden.
- Arbeitsauslastungen mit unbekannter Lebensdauer oder unbekanntem Umfang. Beispiele: eine App, die einige Monate lang nicht benötigt wird, oder eine App, die je nach Bedarf mehr oder weniger Computeleistung benötigt.

Informationen zum Erstellen eines virtuellen Azure-Computers mit SQL Server 2017 mit einem dieser Images mit nutzungsbasierter Bezahlung finden Sie unter den folgenden Links:

| Plattform | Lizenzierte Images |
|---|---|
| Windows Server 2016 | [Virtueller Azure-Computer mit SQL Server 2017 Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [Virtueller Azure-Computer mit SQL Server 2017 Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [Virtueller Azure-Computer mit SQL Server 2017 Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [Virtueller Azure-Computer mit SQL Server 2017 Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[Virtueller Azure-Computer mit SQL Server 2017 Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Wenn Sie einen virtuellen SQL Server-Computer im Portal erstellen, zeigt das Fenster **Größe auswählen** geschätzte Kosten an. Es ist wichtig zu beachten, dass diese Schätzung nur die Kosten für die Ausführung des virtuellen Computers zusammen mit den Windows-Lizenzierungskosten für virtuelle Windows-Computer berücksichtigt. Sie umfasst nicht die zusätzlichen SQL Server-Lizenzierungskosten für eine Web, Standard oder Enterprise Edition. Sie enthält auch keine zusätzlichen Lizenzierungskosten für Linux-Betriebssysteme von Drittanbietern für virtuelle Linux-Computer. Um die genaueste Preisschätzung zu erhalten, wählen Sie Ihr Betriebssystem und die SQL Server-Edition auf der Seite zur Preisfindung für [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) und [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) aus.
>
> ![Blatt zum Auswählen der VM-Größe](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)

### <a name="bring-your-own-license-byol"></a>Bring Your Own License (BYOL)

Das Konzept der **Verwendung eigener SQL-Lizenzen über die „Lizenzmobilität“**, auch als **BYOL** bezeichnet, bedeutet, dass Sie eine vorhandene SQL Server-Volumenlizenz mit Software Assurance auf einem virtuellen Azure-Computer nutzen. Bei einem virtuellen SQL Server-Computer mit BYOL-Modell werden nur die Kosten für die Ausführung des Computers berechnet, nicht für die SQL Server-Lizenzierung. Dies setzt voraus, dass Sie bereits über ein Volumenlizenzprogramm Lizenzen und Software Assurance erworben haben.

> [!NOTE]
> BYOL-Images sind zurzeit nur für virtuelle Windows-Computer verfügbar. Sie können SQL Server jedoch manuell auf einem virtuellen Nur-Linux-Computer installieren. Weitere Informationen finden Sie in den Richtlinien in den [Häufig gestellten Fragen (FAQ) zu virtuellen Linux-Computern](../../linux/sql/sql-server-linux-faq.md).

Die Verwendung eigener SQL-Lizenzen über die „Lizenzmobilität“ empfiehlt sich in folgenden Fällen:

- Kontinuierliche Arbeitsauslastungen. Beispiel: eine App, die rund um die Uhr Geschäftsabläufe unterstützen muss.
- Arbeitsauslastungen mit bekannter Lebensdauer und bekanntem Umfang. Beispiel: eine App mit prognostizierten Anforderungen, die das ganze Jahr über benötigt wird.

Um BYOL mit einem virtuellen SQL Server-Computer zu verwenden, müssen Sie über eine Lizenz für SQL Server Standard oder Enterprise sowie über [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1) verfügen. Software Assurance ist in einigen Programmen zur [Volumenlizenzierung](https://www.microsoft.com/en-us/download/details.aspx?id=10585) erforderlich und in anderen Programmen optional.  Die Preisstufen, die in den verschiedenen Volumenlizenzprogrammen angeboten werden, unterscheiden sich je nach Art der Vereinbarung sowie nach Umfang und Nutzungsdauer von SQL Server. Generell bietet das BYOL-Modell für kontinuierliche Arbeitsauslastungen in der Produktion folgende Vorteile:

| BYOL-Vorteil | BESCHREIBUNG |
|-----|-----|
| **Kostenersparnis** | Die Nutzung eigener SQL Server-Lizenzen ist kostengünstiger als die nutzungsbasierte Bezahlung, wenn eine Arbeitsauslastung SQL Server Standard oder Enterprise *länger als 10 Monate* kontinuierlich ausführt. |
| **Langfristige Einsparungen** | Im Durchschnitt ist es in den ersten drei Jahren *pro Jahr um 30%* günstiger, eine SQL Server-Lizenz zu erwerben oder zu verlängern. Nach drei Jahren müssen Sie die Lizenz nicht mehr verlängern, sondern zahlen nur noch für Software Assurance. Ab diesem Punkt ist BYOL *200% günstiger*. |
| **Kostenloses passives sekundäres Replikat** | Ein weiterer Vorteil der Nutzung eigener Lizenzen ist die [kostenlose Lizenz für ein passives sekundäres Replikat](https://azure.microsoft.com/pricing/licensing-faq/) pro SQL Server-Instanz, um Hochverfügbarkeit sicherzustellen. Damit halbieren sich die Lizenzierungskosten einer hochverfügbaren SQL Server-Bereitstellung (etwa mit AlwaysOn-Verfügbarkeitsgruppen). Die Berechtigungen zum Ausführen des passiven sekundären Replikats werden über den Software Assurance-Vorteil für Failoverserver bereitgestellt. |

Um einen virtuellen Azure-Computer mit SQL Server 2016 mit einem dieser BYOL-Images zu erstellen, verwenden Sie die VMs mit dem Präfix „{BYOL}“:

- [Virtueller Azure-Computer mit SQL Server 2016 Enterprise](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [Virtueller Azure-Computer mit SQL Server 2016 Standard](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!IMPORTANT]
> Informieren Sie uns innerhalb von 10 Tagen, wie viele SQL Server-Lizenzen Sie in Azure verwenden werden. Unter den Links zu den oben genannten Images finden Sie Anweisungen dazu.

> [!NOTE]
> Es ist nicht möglich, das Lizenzierungsmodell einer SQL Server-VM mit minutenbasierter Bezahlung zu ändern und eine eigene Lizenz zu verwenden. In diesem Fall müssen Sie einen neuen virtuellen BYOL-Computer erstellen und die Datenbanken zu diesem neuen virtuellen Computer migrieren. 

## <a name="avoid-unnecessary-costs"></a>Vermeiden unnötiger Kosten

Um unnötige Kosten zu vermeiden, wählen Sie eine optimale Größe für virtuelle Computer aus, und ziehen Sie vorübergehendes Herunterfahren für nicht kontinuierlich ausgeführte Arbeitsauslastungen in Betracht.

### <a id="machinesize"></a>Richtige Größe Ihres virtuellen Computers

Die Kosten der Lizenzierung für SQL Server beziehen sich direkt auf die Anzahl der Kerne. Wählen Sie eine Größe der virtuellen Computer aus, die mit den erwarteten Anforderungen für CPU, Arbeitsspeicher, Speicher und E/A-Bandbreite übereinstimmt. Eine vollständige Liste der Computergrößenoptionen finden Sie unter [Größen virtueller Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) und [Größen virtueller Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Es sind neue Größen für Computer verfügbar, die für bestimmte Arten von SQL Server-Arbeitsauslastungen gut geeignet sind. Dieser Computergrößen verwalten einen hohen Grad an Arbeitsspeicher, Speicher und E/A-Bandbreite, verfügen jedoch über eine niedrigere Anzahl virtualisierter Kerne. Betrachten Sie etwa das folgende Beispiel:

| Größe des virtuellen Computers | vCPUs | Arbeitsspeicher | Max. Anzahl Datenträger | Maximaler E/A-Durchsatz | SQL-Lizenzierungskosten | Gesamtkosten (Compute und Lizenzierung) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOPS oder 768 MB/s | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOPS oder 768 MB/s | 75 % niedriger | 57 % niedriger |

> [!IMPORTANT]
> Dies ist ein Point-in-Time-Beispiel. Die neuesten Spezifikationen finden Sie in den Artikeln zu den Computergrößen und auf der Azure-Preisseite für [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) und [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Im Beispiel oben können Sie sehen, dass die Spezifikationen für **Standard_DS14v2** und **Standard_DS14-4v2** bis auf die vCPUs identisch sind. Das Suffix **-4v2** am Ende der Computergröße **Standard_DS14-4v2** gibt die Anzahl der aktiven vCPUs an. Da die Lizenzierungskosten für SQL Server an die Anzahl der Kerne gebunden sind, reduziert dies die Kosten des virtuellen Computers in Szenarien erheblich, in denen die zusätzlichen vCPUs nicht benötigt werden. Dies ist ein Beispiel, und es gibt viele Computergrößen mit eingeschränkten vCPUs, die mit diesem Suffixmuster identifiziert werden. Weitere Informationen finden Sie im Blogbeitrag [Ankündigung neuer Größen für virtuelle Azure-Computer für kostengünstigeres Arbeiten mit der Datenbank](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Herunterfahren Ihres virtuellen Computers, wenn möglich

Wenn Sie Arbeitsauslastungen verwenden, die nicht kontinuierlich ausgeführt werden, sollten Sie erwägen, den virtuellen Computer während der inaktiven Zeiträume herunterzufahren. Sie bezahlen nur für die tatsächliche Nutzung.

Wenn Sie SQL Server z.B. auf einem virtuellen Azure-Computer nur einmal ausprobieren möchten, liegt es sicher nicht in Ihrem Interesse, Gebühren zu verursachen, indem der Computer versehentlich wochenlang ausgeführt wird. Eine Lösung für dieses Problem ist die [Feature zum automatischen Herunterfahren](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Automatisches Herunterfahren von virtuellen SQL-Computern](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Das automatische Herunterfahren gehört zu einem größeren Satz ähnlicher Features, die von [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab) bereitgestellt wurden.

Bei anderen Workflows sollten Sie eine Skriptlösung wie z.B. [Azure Automation](https://azure.microsoft.com/services/automation/) in Betracht ziehen, um virtuelle Azure-Computer automatisch herunterzufahren und zu starten.

> [!IMPORTANT]
> Das Herunterfahren und Freigeben des virtuellen Computers ist die einzige Möglichkeit, Gebühren zu vermeiden. Wenn Sie einen virtuellen Computer einfach nur beenden oder in einen Energiesparmodus versetzen, fallen weiterhin Nutzungsgebühren an.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zu den Preisen in Azure finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](../../../billing/billing-getting-started.md).

Die neuesten Preisinformationen für virtuelle Computer, einschließlich SQL Server, finden Sie auf der [Preisseite für virtuelle Azure-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Erfahren Sie mehr über virtuelle SQL Server-Computer: [virtuelle SQL Server Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md) und [virtuelle SQL Server Linux-Computer](../../linux/sql/sql-server-linux-virtual-machines-overview.md).
