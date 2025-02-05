---
title: Verwenden von SCP mit Apache Hadoop in Azure HDInsight
description: In diesem Artikel finden Sie Informationen zum Herstellen einer Verbindung mit HDInsight mithilfe des Befehle „ssh“ und „scp“.
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 927b8c55008c3e01d8ff1dd09c46cfa3c6618026
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98946268"
---
# <a name="use-scp-with-apache-hadoop-in-azure-hdinsight"></a>Verwenden von SCP mit Apache Hadoop in Azure HDInsight

In diesem Artikel finden Sie Informationen über die sichere Übertragung von Dateien mit Ihrem HDInsight-Cluster.

## <a name="copy-files"></a>Kopieren von Dateien

Mit dem Hilfsprogramm `scp` können Sie Dateien aus einzelnen Knoten im Cluster und an einzelne Knoten im Cluster kopieren. Der folgende Befehl kopiert beispielsweise das Verzeichnis `test.txt` aus dem lokalen System in den primären Hauptknoten:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Da nach `:` kein Pfad angegeben ist, wird die Datei im Stammverzeichnis `sshuser` abgelegt.

Das folgende Beispiel kopiert die Datei `test.txt` aus dem Stammverzeichnis `sshuser` auf dem primären Hauptknoten in das lokale System:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

Das Hilfsprogramm `scp` kann nur auf das Dateisystem einzelner Knoten innerhalb des Clusters zugreifen. Es kann nicht für den Zugriff auf Daten im HDFS-kompatiblen Speicher für den Cluster verwendet werden.

Verwenden Sie `scp`, wenn Sie eine Ressource für die Verwendung in einer SSH-Sitzung hochladen möchten. So können Sie beispielsweise ein Python-Skript hochladen und anschließend in einer SSH-Sitzung ausführen.

Informationen zum direkten Laden von Daten in den HDFS-kompatiblen-Speicher finden Sie in den folgenden Dokumenten:

* [Verwenden von Azure Storage mit Azure HDInsight-Clustern](hdinsight-hadoop-use-blob-storage.md)
* [Verwenden von Azure Data Lake Storage Gen1 mit HDInsight](../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen1.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von SSH mit HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwenden von Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)
