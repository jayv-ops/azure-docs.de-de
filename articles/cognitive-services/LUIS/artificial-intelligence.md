---
title: Künstliche Intelligenz (KI)
description: LUIS verwendet künstliche Intelligenz (KI) basierend auf dem definierten Schemafür das Sprachverständnis Ihrer Daten.
ms.topic: conceptual
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/07/2020
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 39f73dd002091451ae832516d525499eae98564d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "95021370"
---
# <a name="artificial-intelligence-in-language-understanding-luis"></a>Künstliche Intelligenz in Language Understanding (LUIS)

LUIS verwendet künstliche Intelligenz (KI) für das Verstehen natürlicher Sprache (Natural Language Understanding, NLU) Ihrer Daten basierend auf dem definierten Schema.

## <a name="natural-language-processing-nlp"></a>Verarbeitung natürlicher Sprache (Natural Language Processing, NLP)

Das Verstehen natürlicher Sprache (NLU) ist ein spezifisches Unterthema der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP).

Bei der Verarbeitung natürlicher Sprache handelt es sich um ein umfassenderes Konzept, das jede Form der Verarbeitung von Textdaten betrifft und beispielsweise Folgendes umfasst:

* Tokenisierung
* Satzteilmarkierung
* Segmentierung
* Morphologische Analyse
* Semantische Nähe
* Discourse
* Sprachübersetzung

## <a name="natural-language-processing-in-luis"></a>Verarbeitung natürlicher Sprache in LUIS

Die Verarbeitung natürlicher Sprache steht in der LUIS-App wie folgt zur Verfügung:
* [Verstehen natürlicher Sprache](#natural-language-processing-nlp) (LUIS)
* Konfigurierbare Aspekte der Verarbeitung natürlicher Sprache in LUIS:
    * [Tokenisierung](luis-language-support.md#tokenization)
    * Morphologie durch [API-Einstellungen](luis-reference-application-settings.md) für diakritische Zeichen, Interpunktion und Wortformen
* Vor- und Nachbearbeitung der Abfrageäußerung durch andere [Cognitive Services](../what-are-cognitive-services.md) wie beispielsweise:
    * [Übersetzung](../translator/translator-info-overview.md)

## <a name="natural-language-understanding-nlu"></a>Verstehen natürlicher Sprache (Natural Language Understanding, NLU)

Das Verstehen natürlicher Sprache stellt die Möglichkeit dar, eine sprachliche Aussage in eine Darstellung _umzuwandeln_, die es Ihnen ermöglicht, Benutzer auf natürliche Weise zu verstehen. Das Verstehen natürlicher Sprache ist immer noch eine große Herausforderung und ist als Problem des Typs _AI-hard_ definiert.

LUIS soll sich auf die Absicht und Extraktion konzentrieren, sodass Folgendes identifiziert werden kann:
* Was möchte der Benutzer?
* Worüber spricht er?

LUIS umfasst wenig oder gar kein Wissen der umfassenderen Aspekte der _Verarbeitung natürlicher Sprache_, z. B. semantische Nähe, und beinhaltet keine explizite Identifizierung in Beispielen. Beispielsweise unterscheiden sich die folgenden Token (Wörter), bis sie in Beispielen in einem ähnlichen Kontext verwendet werden:

* kaufen
* Kauf
* gekauft

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Entwicklungslebenszyklus](luis-concept-app-iteration.md) einer LUIS-App