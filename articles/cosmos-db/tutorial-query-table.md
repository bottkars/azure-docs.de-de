---
title: "Wie werden Abfragen von Tabellendaten in Azure Cosmos DB durchgeführt? | Microsoft Docs"
description: Erfahren Sie, wie Sie Tabellendaten in Azure Cosmos DB abfragen.
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: 80fed91c45ae19193f6b8dfcaef747f8c4253dee
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api"></a>Azure Cosmos DB: Abfragen von Tabellendaten mithilfe der Table-API

Die [Table-API](table-introduction.md) von Azure Cosmos DB unterstützt OData- und [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service)-Abfragen von Schlüssel-/Wertdaten (Tabellendaten).  

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit der Tabellen-API

Die Abfragen in diesem Artikel verwenden die folgende Beispieltabelle `People`:

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Weitere Informationen zu Abfragen mit der Tabellen-API finden Sie unter [Abfragen von Tabellen und Entitäten](https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities). 

Weitere Informationen zu den Premiumfunktionen von Azure Cosmos DB finden Sie unter [Einführung in die Tabellen-API von Azure Cosmos DB](table-introduction.md) sowie unter [Azure Cosmos DB: Entwickeln mit der Tabellen-API in .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Voraussetzungen

Diese Abfragen können nur funktionieren, wenn Sie über ein Azure Cosmos DB-Konto verfügen und Entitätsdaten im Container vorliegen. Sie haben beides nicht? Absolvieren Sie den [5-Minuten-Schnellstart](create-table-dotnet.md) oder das [Entwicklertutorial](tutorial-develop-table-dotnet.md) zum Erstellen eines Kontos und Auffüllen Ihrer Datenbank.

## <a name="query-on-partitionkey-and-rowkey"></a>Abfragen nach PartitionKey und RowKey
Da die Eigenschaften PartitionKey und RowKey den Primärschlüssel einer Entität bilden, können Sie die folgende spezielle Syntax verwenden, um die Entität zu identifizieren: 

**Abfragen**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Ergebnisse**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

Alternativ können Sie diese Eigenschaften als Teil der `$filter`-Option angeben, wie im folgenden Abschnitt gezeigt. Beachten Sie, dass bei den Schlüsseleigenschaftsnamen und Konstantenwerten die Groß-/Kleinschreibung berücksichtigt wird. Die Eigenschaften PartitionKey und RowKey sind vom Typ Zeichenfolge. 

## <a name="query-by-using-an-odata-filter"></a>Abfragen mit einem OData-Filter
Wenn Sie eine Filterzeichenfolge erstellen, gelten diese Regeln: 

* Verwenden Sie die von der OData-Protokollspezifikation definierten logischen Operatoren, um eine Eigenschaft mit einem Wert zu vergleichen. Beachten Sie, dass Sie eine Eigenschaft nicht mit einem dynamischen Wert vergleichen können. Eine Seite des Ausdrucks muss eine Konstante sein. 
* Eigenschaftenname, Operator und Konstantenwert müssen durch URL-codierte Leerzeichen getrennt werden. Ein Leerzeichen wird als `%20` URL-codiert. 
* Bei allen Teilen der Filterzeichenfolge ist die Groß-/Kleinschreibung zu beachten. 
* Der konstante Wert muss den gleichen Datentyp besitzen wie die Eigenschaft, damit vom Filter gültige Ergebnisse zurückgegeben werden. Weitere Informationen zu unterstützten Eigenschaftentypen finden Sie unter [Grundlegendes zum Tabellenspeicherdienst-Datenmodell](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Diese Beispielabfrage zeigt das Filtern nach den Eigenschaften PartitionKey und Email unter Verwendung eines OData-`$filter`.

**Abfragen**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Weitere Informationen zum Erstellen von Filterausdrücken für verschiedene Datentypen finden Sie unter [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities) (Abfragen von Tabellen und Entitäten).

**Ergebnisse**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Abfragen mit LINQ 
Sie können auch Abfragen mit LINQ durchführen, wobei die Übersetzung in die entsprechenden OData-Abfrageausdrücke erfolgt. Dies ist ein Beispiel für das Erstellen von Abfragen mit dem .NET SDK.

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben erfahren, wie Sie Abfragen mit der Table-API erstellen.

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-table.md)
