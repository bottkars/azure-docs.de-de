---
title: "Transformation „JSON erweitern“ mit Azure Machine Learning Workbench"
description: "Dies ist das Referenzdokument für die Transformation „JSON erweitern“."
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 614f4422aa987fc32dcce62826bb2477473fdc32
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2017
---
# <a name="expand-json-transformation"></a>Transformation „JSON erweitern“
Durch die Transformation **JSON erweitern** können Benutzer eine bestehende Spalte erweitern, die gültigen JSON-Text in mehreren Spalten enthält.

## <a name="how-to-perform-this-transformation"></a>Vorgehensweise zum Durchführen dieser Transformation

Führen Sie die folgenden Schritte für diese Transformation durch:
1. Wählen Sie die Quellspalte, die den JSON-Text enthält.
2. Wählen Sie im Menü **Transformationen** die Option **JSON erweitern** aus. Klicken Sie alternativ mit der rechten Maustaste auf die Überschrift der Quellspalte, und wählen Sie im Kontextmenü **JSON erweitern** aus. 
3. Klicken Sie auf **OK**. 
 
Neue Spalten werden neben der Quellspalte hinzugefügt. Diese Spalten enthalten die Eigenschaften der nächsten Hierarchieebene im JSON-Text. Falls ein Eigenschaftsschlüssel vorhanden ist, wird dieser zum Erstellen des Namens der entsprechenden Spalte verwendet. Geschachtelte Eigenschaften werden als JSON-Text beibehalten, die der Benutzer nach Bedarf iterativ erweitern oder verwerfen kann.

## <a name="examples"></a>Beispiele

Die Quellspalte *Kunde* ist in zwei Spalten unterteilt – in *Customer.Name* und *Customer.Phone*.

| Kunde                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| { "Name" : "Carrie Dodson", "Phone" : "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| { "Name" : "Leonard Robledo", "Phone" : "456-7890-123"} | Leonard Robledo | 456-7890-123   |

