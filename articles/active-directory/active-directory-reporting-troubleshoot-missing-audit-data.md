---
title: "Problembehandlung: Fehlende Daten im Azure Active Directory-Aktivitätsprotokoll | Microsoft-Dokumentation"
description: "Enthält eine Liste mit den verschiedenen verfügbaren Berichten für Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 55587fb4ff6d8a2130eb838782a65788fb2dd2b3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>Ich kann einige Aktionen nicht finden, die ich im Aktivitätsprotokoll für Azure Active Directory ausgeführt habe.


## <a name="symptoms"></a>Symptome

Ich habe einige Aktionen im Azure-Portal ausgeführt und erwartet, die Überwachungsprotokolle für diese Aktionen auf dem Blatt `Activity logs > Audit Logs` zu finden. Ich kann sie jedoch nicht finden.

 ![Berichterstellung](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Ursache

Aktionen werden nicht sofort im Aktivitätsüberwachungsprotokoll angezeigt. Ab der Ausführung des Vorgangs kann es zwischen 15 Minuten und eine Stunde dauern, bis die Überwachungsprotokolle im Portal angezeigt werden.

## <a name="resolution"></a>Lösung

Warten Sie 15 Minuten bis eine Stunde, und überprüfen Sie dann, ob die Aktionen im Protokoll angezeigt werden. Wenn sie weiterhin nicht vorhanden sind, erstellen Sie ein Supportticket, und wir nehmen uns der Sache an.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Active Directory-Berichten](active-directory-reporting-faq.md).

