---
title: Abfragesprache
titleSuffix: Azure Digital Twins
description: Grundlagen der Azure Digital Twins-Abfragesprache
author: baanders
ms.author: baanders
ms.date: 11/19/2020
ms.topic: conceptual
ms.service: digital-twins
ms.custom: contperf-fy21q2
ms.openlocfilehash: 9549e6ea30be0cd9eb1a8c200a5af4a4721793a6
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102034675"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Abfragesprache für Azure Digital Twins

Beachten Sie, dass im Mittelpunkt von Azure Digital Twins der [Zwillingsgraph](concepts-twins-graph.md) steht, der sich aus digitalen Zwillingen und Beziehungen zusammensetzt. 

Dieser Graph kann abgefragt werden, um Informationen zu den darin enthaltenen digitalen Zwillingen und Beziehungen abzurufen. Diese Abfragen werden in einer benutzerdefinierten SQL-ähnlichen Abfragesprache mit der Bezeichnung **Azure Digital Twins-Abfragesprache** geschrieben. Diese ähnelt der [IoT Hub-Abfragesprache](../iot-hub/iot-hub-devguide-query-language.md) mit vielen vergleichbaren Features.

In diesem Artikel werden die Grundlagen der Abfragesprache und ihrer Funktionen beschrieben. Ausführlichere Beispiele für die Abfragesyntax und das Ausführen von Abfrageanforderungen finden Sie unter [*Vorgehensweise: Abfragen des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).

## <a name="about-the-queries"></a>Informationen zu Abfragen

Sie können die Abfragesprache von Azure Digital Twins verwenden, um digitale Zwillinge gemäß folgender Aspekte abzurufen:
* ihren Eigenschaften (einschließlich [Tageigenschaften](how-to-use-tags.md))
* Modelle
* relationships
  - Eigenschaften der Beziehungen

Wenn Sie eine Abfrage aus einer Client-App an den Dienst senden möchten, verwenden Sie die Azure Digital Twins-[**Abfrage-API**](/rest/api/digital-twins/dataplane/query). Eine Möglichkeit, die API zu verwenden, ist durch eins der [SDKs](how-to-use-apis-sdks.md#overview-data-plane-apis) für Azure Digital Twins.

### <a name="considerations-for-querying"></a>Hinweise zur Abfrage

Beachten Sie beim Schreiben von Abfragen für Azure Digital Twins Folgendes:
* **Denken Sie an die Beachtung der Groß-/Kleinschreibung:** Bei allen Azure Digital Twins-Abfragevorgängen wird die Groß-/Kleinschreibung beachtet. Verwenden Sie daher genau die in den Modellen definierten Namen. Wenn Eigenschaftennamen falsch oder in falscher Groß-/Kleinschreibung geschrieben werden, ist das Resultset leer, und es werden keine Fehler zurückgegeben.
* **Versehen Sie einfache Anführungszeichen mit Escapezeichen:** Wenn die Daten Ihres Abfragetexts ein einfaches Anführungszeichen enthalten, muss dieses mit dem Escapezeichen `\` versehen werden. Im folgenden Beispiel wird der Umgang mit dem Eigenschaftswert *D'Souza* gezeigt:

  :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="EscapedSingleQuote":::

## <a name="reference-expressions-and-conditions"></a>Referenz: Ausdrücke und Bedingungen

In diesem Abschnitt werden die Operatoren und Funktionen beschrieben, die beim Schreiben von Azure Digital Twins-Abfragen verfügbar sind. Beispielabfragen, die die Verwendung dieser Features veranschaulichen, finden Sie unter [*Vorgehensweise: Abfragen des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).

### <a name="operators"></a>Operatoren

Die folgenden Operatoren werden unterstützt:

| Familie | Operatoren |
| --- | --- |
| Logisch |`AND`, `OR`, `NOT` |
| Vergleich | `=`, `!=`, `<`, `>`, `<=`, `>=` |
| Enthält | `IN`, `NIN` |

### <a name="functions"></a>Funktionen

Folgende Typüberprüfungs- und Umwandlungsfunktionen werden unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| `IS_DEFINED` | Gibt einen booleschen Wert zurück, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde. Dies wird nur unterstützt, wenn es sich bei dem Wert um einen primitiven Typ handelt. Primitive Typen umfassen Zeichenfolgen, boolesche Werte, numerische Werte und `null`. `DateTime`, Objekttypen und Arrays werden nicht unterstützt. |
| `IS_OF_MODEL` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Zwilling dem angegebenen Modelltyp entspricht. |
| `IS_BOOL` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „boolesch“ ist. |
| `IS_NUMBER` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zahl“ ist. |
| `IS_STRING` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zeichenfolge“ ist. |
| `IS_NULL` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „NULL“ ist. |
| `IS_PRIMITIVE` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck ein Grundtyp ist (Zeichenfolge, boolesch, numerisch oder `null`). |
| `IS_OBJECT` | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „JSON-Objekt“ ist. |

Folgende Zeichenfolgenfunktionen werden unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| `STARTSWITH(x, y)` | Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt. |
| `ENDSWITH(x, y)` | Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten endet. |

## <a name="query-limitations"></a>Abfrageeinschränkungen

Dieser Abschnitt beschreibt Einschränkungen der Abfragesprache.

* Timing: Es kann bis zu 10 Sekunden dauern, bis Änderungen in Ihrer Instanz in Abfragen berücksichtigt werden. Wenn Sie beispielsweise einen Vorgang wie das Erstellen oder Löschen von Zwillingen mit der Digital Twins-API durchführen, wird das Ergebnis möglicherweise in Anforderungen an die Abfrage-API nicht sofort reflektiert. Zum Beheben dieser Einschränkung ist es normalerweise ausreichend, eine kurze Zeit zu warten.
* In der `FROM`-Anweisung werden keine Unterabfragen unterstützt.
* `OUTER JOIN`-Semantik wird nicht unterstützt. Das bedeutet, dass die gesamte „Zeile“ aus dem Ausgaberesultset entfernt wird, wenn die Beziehung den Rang null aufweist.
* Die Durchlauftiefe für den Graphen ist auf fünf `JOIN`-Ebenen pro Abfrage beschränkt.
* Die Quelle für `JOIN`-Vorgänge ist eingeschränkt: Die Abfrage muss die Zwillinge deklarieren, in denen die Abfrage beginnt.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Schreiben von Abfragen, und untersuchen Sie Clientcodebeispiele unter [*Abfragen des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).