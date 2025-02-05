---
title: 'Tutorial: Erstellen von Machine Learning-Anwendungen mithilfe von Microsoft Machine Learning für Apache Spark (Vorschau)'
description: Hier erfahren Sie, wie Sie mithilfe von Microsoft Machine Learning für Apache Spark Machine Learning-Anwendungen in Azure Synapse Analytics erstellen.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: ''
ms.date: 03/08/2021
author: ruxu
ms.author: ruxu
ms.openlocfilehash: ad6f0d5ad55716e19e4e0c571056d18641e23d21
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102619896"
---
# <a name="tutorial-build-machine-learning-applications-using-microsoft-machine-learning-for-apache-spark-preview"></a>Tutorial: Erstellen von Machine Learning-Anwendungen mithilfe von Microsoft Machine Learning für Apache Spark (Vorschau)

In diesem Artikel erfahren Sie, wie Sie mithilfe von Microsoft Machine Learning für Apache Spark ([MMLSpark](https://github.com/Azure/mmlspark)) Machine Learning-Anwendungen erstellen. Mit MMLSpark wird die Lösung für verteiltes maschinelles Lernen von Apache Spark erweitert, indem zahlreiche Deep Learning- und Data Science-Tools wie [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/big-data/cognitive-services-for-big-data), [OpenCV](https://opencv.org/), [LightGBM](https://github.com/Microsoft/LightGBM) usw. hinzugefügt werden.  MMLSpark ermöglicht die Erstellung leistungsstarker und hochgradig skalierbarer Vorhersage- und Analysemodelle auf der Grundlage verschiedener Spark-Datenquellen.
Synapse Spark bietet integrierte MMLSpark-Bibliotheken, u. a.:

- [Vowpal Wabbit:](https://github.com/VowpalWabbit/vowpal_wabbit) Bibliotheksdienste für maschinelles Lernen, um die Textanalyse (etwa Stimmungsanalyse) in Tweets zu ermöglichen
- [Cognitive Services in Spark:](https://docs.microsoft.com/azure/cognitive-services/big-data/cognitive-services-for-big-data) Dient zum Kombinieren der Azure Cognitive Services-Features in SparkML-Pipelines, um einen Lösungsentwurf für kognitive Datenmodellierungsdienste wie Anomalieerkennung abzuleiten.
- [LightBGM:](https://github.com/Azure/mmlspark/blob/master/docs/lightgbm.md) Machine Learning-Modell, um das Trainieren des Modells für Vorhersageanalysen wie Gesichtserkennung zu ermöglichen
- Bedingtes KNN-Modell: Skalierbare KNN-Modelle mit bedingten Abfragen
- [HTTP in Spark:](https://github.com/Azure/mmlspark/blob/master/docs/http.md) Ermöglicht die Orchestrierung verteilter Microservices bei der Integration von Spark und HTTP-basiertem Zugriff.

In diesem Tutorial werden Beispiele für die Verwendung von Azure Cognitive Services in MMLSpark für Folgendes behandelt: 

- Textanalyse: Ermitteln der Stimmung (oder Laune) in einer Reihe von Sätzen
- Maschinelles Sehen: Abrufen der Tags (Einwortbeschreibungen) die einem Satz von Bildern zugeordnet sind
- Bing-Bildersuche: Suchen im Web nach Bildern im Zusammenhang mit einer Abfrage in natürlicher Sprache
- Anomalieerkennung: Erkennen von Anomalien innerhalb von Zeitreihendaten

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen 

- [Azure Synapse Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/synapse-analytics/get-started-create-workspace) mit einem als Standardspeicher konfigurierten Azure Data Lake Storage Gen2-Speicherkonto. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.
- Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Ausführliche Informationen finden Sie unter [Erstellen eines Spark-Pools in Azure Synapse](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-sql-pool-studio).
- Vorkonfigurationsschritte im Tutorial [Konfigurieren von Cognitive Services in Azure Synapse](https://docs.microsoft.com/azure/synapse-analytics/machine-learning/tutorial-configure-cognitive-services-synapse)


## <a name="get-started"></a>Erste Schritte
Importieren Sie zunächst MMLSpark, und konfigurieren Sie Dienstschlüssel.

```python
import mmlspark
mmlspark.__spark_package_version__ # current version: 1.0.0-rc3-6-a862d6b1-SNAPSHOT

from mmlspark.cognitive import *
from notebookutils import mssparkutils

# A general Cognitive Services key for Text Analytics and Computer Vision (or use separate keys that belong to each service)
service_key =  "ADD_YOUR_SUBSCRIPION_KEY" 
# A Bing Search v7 subscription key
bing_search_key = "ADD_YOUR_SUBSCRIPION_KEY" 
# An Anomaly Dectector subscription key
anomaly_key =  "ADD_YOUR_SUBSCRIPION_KEY" 


cognitive_service_key = mssparkutils.credentials.getSecret("keyvaultForSynapse", service_key)
bingsearch_service_key = mssparkutils.credentials.getSecret("keyvaultForSynapse", bing_search_key)
anomalydetector_key = mssparkutils.credentials.getSecret("keyvaultForSynapse", anomaly_key)

```

## <a name="text-analytics-sample"></a>Beispiel für Textanalyse

Der [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/)-Dienst bietet mehrere Algorithmen zum Extrahieren intelligenter Erkenntnisse aus Text. Beispielsweise können wir die Stimmung eines Eingabetexts ermitteln. Der Dienst gibt eine Bewertung zwischen 0,0 und 1,0 zurück, wobei niedrige Scores auf eine negative Stimmung und hohe Scores auf eine positive Stimmung hindeuten. In diesem Beispiel werden drei einfache Sätze verwendet, und es wird jeweils die Stimmung zurückgegeben.

```python
from pyspark.sql.functions import col

# Create a dataframe that's tied to it's column names
df_sentences = spark.createDataFrame([
  ("I am so happy today, its sunny!", "en-US"), 
  ("this is a dog", "en-US"), 
  ("I am frustrated by this rush hour traffic!", "en-US") 
], ["text", "language"])

# Run the Text Analytics service with options
sentiment = (TextSentiment()
    .setTextCol("text")
    .setLocation("eastasia")
    .setSubscriptionKey(cognitive_service_key)
    .setOutputCol("sentiment")
    .setErrorCol("error")
    .setLanguageCol("language"))

# Show the results of your text query in a table format

display(sentiment.transform(df_sentences).select("text", col("sentiment")[0].getItem("sentiment").alias("sentiment")))
```

### <a name="expected-results"></a>Erwartete Ergebnisse

|text | Stimmung|
|--|--|
| Ich bin frustriert über diesen Berufsverkehr. | Negativ |
| Dies ist ein Hund. | Neutral |
| Ich bin heute so glücklich, die Sonne scheint! | Positiv |

## <a name="computer-vision-sample"></a>Beispiel für maschinelles Sehen
[Maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) analysiert Bilder, um die Struktur zu identifizieren, wie z. B. Gesichter, Objekte und Beschreibungen in natürlicher Sprache. In diesem Beispiel markieren wir das folgende Bild. Tags sind Einwortbeschreibungen der Dinge im Bild, wie z. B. erkennbare Objekte, Personen, Szenen und Aktionen.


![image](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg)

```python
# Create a dataframe with the image URL
df_images = spark.createDataFrame([
        ("https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg", )
    ], ["image", ])

# Run the Computer Vision service. Analyze Image extracts information from/about the images.
analysis = (AnalyzeImage()
    .setLocation("eastasia")
    .setSubscriptionKey(cognitive_service_key)
    .setVisualFeatures(["Categories","Color","Description","Faces","Objects","Tags"])
    .setOutputCol("analysis_results")
    .setImageUrlCol("image")
    .setErrorCol("error"))

# Show the results of what you wanted to pull out of the images.
display(analysis.transform(df_images).select("image", "analysis_results.description.tags"))
```
### <a name="expected-results"></a>Erwartete Ergebnisse

|image | tags|
|--|--|
| `https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/objects.jpg` | [Skaten, Person, Mann, draußen, Reiten, Sport, Skateboard, jung, Brett, Hemd, Luft, Park, Junge, Seite, Springen, Rampe, Trick, tun, Fliegen] |

## <a name="bing-image-search-sample"></a>Beispiel für Bing-Bildersuche
Die [Bing-Bildersuche](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview) durchsucht das Web nach Bildern im Zusammenhang mit einer Abfrage in natürlicher Sprache eines Benutzers. In diesem Beispiel verwenden wir eine Textabfrage, die nach Bildern mit Zitaten sucht. Sie gibt eine Liste mit Bild-URLs zurück, die Fotos im Zusammenhang mit unserer Abfrage enthalten.


```python
from pyspark.ml import PipelineModel

# Number of images Bing will return per query
imgsPerBatch = 2
# A list of offsets, used to page into the search results
offsets = [(i*imgsPerBatch,) for i in range(10)]
# Since web content is our data, we create a dataframe with options on that data: offsets
bingParameters = spark.createDataFrame(offsets, ["offset"])

# Run the Bing Image Search service with our text query
bingSearch = (BingImageSearch()
    .setSubscriptionKey(bingsearch_service_key)
    .setOffsetCol("offset")
    .setQuery("Martin Luther King Jr. quotes")
    .setCount(imgsPerBatch)
    .setOutputCol("images"))

# Transformer that extracts and flattens the richly structured output of Bing Image Search into a simple URL column
getUrls = BingImageSearch.getUrlTransformer("images", "url")
pipeline_bingsearch = PipelineModel(stages=[bingSearch, getUrls])

# Show the results of your search: image URLs
res_bingsearch = pipeline_bingsearch.transform(bingParameters)
display(res_bingsearch.dropDuplicates())
```

### <a name="expected-results"></a>Erwartete Ergebnisse

|image | 
|--|
|`http://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes-16.jpg` |
|`http://www.scrolldroll.com/wp-content/uploads/2017/06/6-25.png` |
| `http://abettertodaymedia.com/wp-content/uploads/2017/01/86783bd7a92960aedd058c91a1d10253.jpg`|
| `https://weneedfun.com/wp-content/uploads/2016/05/martin-luther-king-jr-quotes-11.jpg` |
| `http://www.sofreshandsogreen.com/wp-content/uploads/2012/01/martin-luther-king-jr-quote-sofreshandsogreendotcom.jpg` |
| `https://cdn.quotesgram.com/img/72/57/1104209728-martin_luther_king_jr_quotes_16.jpg` |
| `http://comicbookandbeyond.com/wp-content/uploads/2019/05/Martin-Luther-King-Jr.-Quotes.jpg` |
| `https://exposingthepain.files.wordpress.com/2015/01/martin-luther-king-jr-quotes-08.png` |
| `https://topmemes.me/wp-content/uploads/2020/01/Top-10-Martin-Luther-King-jr.-Quotes2-1024x538.jpg` |
| `http://img.picturequotes.com/2/581/580286/dr-martin-luther-king-jr-quote-1-picture-quote-1.jpg` |
| `http://parryz.com/wp-content/uploads/2017/06/Amazing-Martin-Luther-King-Jr-Quotes.jpg` |
| `http://everydaypowerblog.com/wp-content/uploads/2014/01/Martin-Luther-King-Jr.-Quotes1.jpg` |
| `https://lessonslearnedinlife.net/wp-content/uploads/2020/05/Martin-Luther-King-Jr.-Quotes-2020.jpg` |
| `https://quotesblog.net/wp-content/uploads/2015/10/Martin-Luther-King-Jr-Quotes-Wallpaper.jpg` |

## <a name="anomaly-detector-sample"></a>Beispiel für Anomalieerkennung

Die [Anomalieerkennung](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/) eignet sich hervorragend zum Erkennen von Unregelmäßigkeiten in ihren Zeitreihendaten In diesem Beispiel verwenden wir den Dienst, um Anomalien in der gesamten Zeitreihe zu suchen.

```python
from pyspark.sql.functions import lit

# Create a dataframe with the point data that Anomaly Detector requires
df_timeseriesdata = spark.createDataFrame([
    ("1972-01-01T00:00:00Z", 826.0),
    ("1972-02-01T00:00:00Z", 799.0),
    ("1972-03-01T00:00:00Z", 890.0),
    ("1972-04-01T00:00:00Z", 900.0),
    ("1972-05-01T00:00:00Z", 766.0),
    ("1972-06-01T00:00:00Z", 805.0),
    ("1972-07-01T00:00:00Z", 821.0),
    ("1972-08-01T00:00:00Z", 20000.0), # anomaly
    ("1972-09-01T00:00:00Z", 883.0),
    ("1972-10-01T00:00:00Z", 898.0),
    ("1972-11-01T00:00:00Z", 957.0),
    ("1972-12-01T00:00:00Z", 924.0),
    ("1973-01-01T00:00:00Z", 881.0),
    ("1973-02-01T00:00:00Z", 837.0),
    ("1973-03-01T00:00:00Z", 9000.0) # anomaly
], ["timestamp", "value"]).withColumn("group", lit("series1"))

# Run the Anomaly Detector service to look for irregular data
anamoly_detector = (SimpleDetectAnomalies()
  .setSubscriptionKey(anomalydetector_key)
  .setLocation("eastasia")
  .setTimestampCol("timestamp")
  .setValueCol("value")
  .setOutputCol("anomalies")
  .setGroupbyCol("group")
  .setGranularity("monthly"))

# Show the full results of the analysis with the anomalies marked as "True"
display(anamoly_detector.transform(df_timeseriesdata).select("timestamp", "value", "anomalies.isAnomaly"))

```

### <a name="expected-results"></a>Erwartete Ergebnisse

|timestamp | value | isAnomaly |
|--|--|--|
| 1972-01-01T00:00:00Z|826.0|false|
|1972-02-01T00:00:00Z|799.0|false|
|1972-03-01T00:00:00Z|890.0|false|
|1972-04-01T00:00:00Z|900.0|false|
|1972-05-01T00:00:00Z|766.0|false|
|1972-06-01T00:00:00Z|805.0|false|
|1972-07-01T00:00:00Z|821.0|false|
|1972-08-01T00:00:00Z|20000.0|true|
|1972-09-01T00:00:00Z|883.0|false|
|1972-10-01T00:00:00Z|898.0|false|
|1972-11-01T00:00:00Z|957.0|false|
|1972-12-01T00:00:00Z|924.0|false|
|1973-01-01T00:00:00Z|881.0|false|
|1973-02-01T00:00:00Z|837.0|false|
|1973-03-01T00:00:00Z|9000.0|true|

## <a name="next-steps"></a>Nächste Schritte

* [Beispiele für Synapse-Notebooks](https://github.com/Azure-Samples/Synapse/tree/main/Notebooks) 
* [MMLSpark-GitHub-Repository](https://github.com/Azure/mmlspark)