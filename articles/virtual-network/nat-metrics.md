---
title: Métriques et alertes pour le service NAT du réseau virtuel Azure
titleSuffix: Azure Virtual Network
description: Prenez connaissance des métriques et des alertes Azure Monitor disponibles pour le service NAT du réseau virtuel Azure.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to understand available Azure Monitor metrics and alerts for Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: 1c3a73cba6a2ece0c9c7459b3515f053a648d683
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81408606"
---
# <a name="azure-virtual-network-nat-metrics"></a>Métriques du service NAT du réseau virtuel Azure

Les ressources de la passerelle NAT du réseau virtuel Azure fournissent des métriques multidimensionnelles. Vous pouvez utiliser ces métriques pour observer l’opération et pour la [résolution des problèmes](troubleshoot-nat.md).  Il est possible de configurer des alertes pour des problèmes critiques comme l’épuisement des ressources SNAT.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Service NAT de Réseau virtuel pour le trafic sortant vers Internet">
</p>

*Figure : Service NAT de Réseau virtuel pour le trafic sortant vers Internet*

## <a name="metrics"></a>Mesures

Les ressources de passerelle NAT fournissent les métriques multidimensionnelles suivantes dans Azure Monitor :

| Métrique | Description | Agrégation recommandée | Dimensions |
|---|---|---|---|
| Octets | Octets entrants et sortants traités | SUM | Direction (In ; Out), Protocole (6 TCP ; 17 UDP) |
| Paquets | Paquets entrants et sortants traités | SUM | Direction (In ; Out), Protocole (6 TCP ; 17 UDP) |
| Paquets ignorés | Paquets ignorés par la passerelle NAT | SUM | / |
| Nombre de connexions SNAT | Transitions d’état par intervalle | SUM | État de la connexion, Protocole (6 TCP; 17 UDP) |
| Nombre total de connexions SNAT | Connexions SNAT actives actuelles (~ ports SNAT utilisés) | SUM | Protocole (6 TCP ; 17 UDP) |


## <a name="alerts"></a>Alertes

Les alertes relatives aux métriques peuvent être configurées dans Azure Monitor pour chacune des [métriques](#metrics) précédentes.

## <a name="limitations"></a>Limites

Resource Health n’est pas pris en charge.

## <a name="next-steps"></a>Étapes suivantes

* Apprenez-en davantage sur le service [Nat de Réseau virtuel](nat-overview.md).
* Découvrir la [ressource de passerelle NAT](nat-gateway-resource.md)
* Découvrir [Azure Monitor](../azure-monitor/overview.md)
* Découvrez la [résolution des problèmes liés aux ressources de passerelle NAT](troubleshoot-nat.md).
* [Utilisez UserVoice pour nous faire part des prochains développements dont vous aimeriez bénéficier concernant le service NAT de réseau virtuel](https://aka.ms/natuservoice).


