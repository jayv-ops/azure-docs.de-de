---
title: Benennungskonventionen für Azure-VM-Größen
description: Erläutert die für Azure-VM-Größen verwendeten Namenskonventionen
ms.service: virtual-machines
subservice: sizes
author: mimckitt
ms.topic: conceptual
ms.date: 7/22/2020
ms.author: mimckitt
ms.custom: sttsinar
ms.openlocfilehash: 98d35821c884ba25a109f929556087ff6ecba0ca
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102551013"
---
# <a name="azure-virtual-machine-sizes-naming-conventions"></a>Namenskonventionen für Azure-VM-Größen

Diese Seite gibt einen Überblick über die Namenskonventionen, die für Azure VMs verwendet werden.Diese Seite bietet eine Übersicht über die Namenskonventionen, die für Azure-VMs verwendet werden. VMs verwenden diese Namenskonventionen, um variierende Features und Spezifikationen zu bezeichnen.

## <a name="naming-convention-explanation"></a>Erläuterung der Namenskonvention

**[Familie]**  +  **[Unterfamilie*]**  +  **[Anzahl der vCPUs]**  +  **[Eingeschränkte vCPUs*]**  +  **[Zusätzliche Features]**  +  **[Beschleunigungstyp*]**  +  **[Version]**

|Wert | Erklärung|
|---|---|
| Familie | Gibt die Serie der VM-Familie an| 
| *Unterfamilie | Wird nur für spezialisierte VM-Differenzierungen verwendet|
| Anzahl der vCPUs| Gibt die Anzahl der vCPUs des virtuellen Computers an |
| * Eingeschränkte vCPUs| Werden nur für bestimmte VM-Größen verwendet Gibt die Anzahl der vCPUs für die [eingeschränkte vCPU-fähige Größe](https://docs.microsoft.com/azure/virtual-machines/constrained-vcpu) an |
| Zusätzliche Features | Mindestens ein Kleinbuchstabe bezeichnet zusätzliche Features. Beispiele: <br> a = AMD-basierter Prozessor <br> d = Datenträger (lokaler temporärer Datenträger ist vorhanden). Dies gilt für neuere Azure-VMS (siehe [Ddv4- und Ddsv4-Serie](./ddv4-ddsv4-series.md)) <br> h = Ruhezustand-fähig <br> i = isolierte Größe <br> l = geringer Arbeitsspeicher; eine niedrigere Arbeitsspeichermenge als die arbeitsspeicherintensive Größe <br> m = arbeitsspeicherintensiv; die höchste Arbeitsspeichermenge einer bestimmten Größe <br> t = sehr geringer Arbeitsspeicher; die kleinste Arbeitsspeichermenge einer bestimmten Größe <br> r = RDMA-fähig <br> s = Storage Premium-fähig, einschließlich der möglichen Verwendung von [SSD Ultra](./disks-types.md#ultra-disk) (Hinweis: einige neuere Größen ohne das Attribut „s“ können noch Storage Premium unterstützen, z. B. M128, M64 usw.)<br> |
| *Beschleunigertyp | Gibt den Typ des Hardwarebeschleunigers in den spezialisierten/GPU-SKUs an. Nur die neuen spezialisierten/GPU-SKUs, die ab dem 3. Quartal 2020 auf den Markt kommen, werden den Hardwarebeschleuniger im Namen tragen. |
| Version | Bezeichnet die Version der Serie der VM-Familie |

## <a name="example-breakdown"></a>Beispielaufschlüsselung

**[Familie]**  +  **[Unterfamilie*]**  +  **[Anzahl der vCPUs]**  +  **[Zusätzliche Features]**  +  **[Beschleunigertyp*]**  +  **[Version]**

### <a name="example-1-m416ms_v2"></a>Beispiel 1: M416ms_v2

|Wert | Erklärung|
|---|---|
| Familie | M | 
| Anzahl der vCPUs | 416 |
| Zusätzliche Features | m = arbeitsspeicherintensiv <br> s = Storage Premium-fähig |
| Version | V2 |

### <a name="example-2-nv16as_v4"></a>Beispiel 2: NV16as_v4

|Wert | Erklärung|
|---|---|
| Familie | N | 
| Unterfamilie | V |
| Anzahl der vCPUs | 16 |
| Zusätzliche Features | a = AMD-basierter Prozessor <br> s = Storage Premium-fähig |
| Version | v4 |

### <a name="example-3-nc4as_t4_v3"></a>Beispiel 3: NC4as_T4_v3

|Wert | Erklärung|
|---|---|
| Familie | N | 
| Unterfamilie | C |
| Anzahl der vCPUs | 4 |
| Zusätzliche Features | a = AMD-basierter Prozessor <br> s = Storage Premium-fähig |
| Beschleunigertyp | T4 |
| Version | V3 |

### <a name="example-4-m8-2ms_v2-constrained-vcpu"></a>Beispiel 4: M8-2ms_v2 (eingeschränkte vCPU)

|Wert | Erklärung|
|---|---|
| Familie | M | 
| Anzahl der vCPUs | 8 |
| Anzahl der eingeschränkten (tatsächlichen) vCPUs | 2 |
| Zusätzliche Features | m = arbeitsspeicherintensiv <br> s = Storage Premium-fähig |
| Version | V2 |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die verfügbaren [VM-Größen](./sizes.md) in Azure. 
