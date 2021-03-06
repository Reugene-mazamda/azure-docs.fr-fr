---
title: Azure Front Door - Méthodes de routage du trafic | Microsoft Docs
description: Cet article vous aide à comprendre les différentes méthodes de routage du trafic utilisées par Front Door
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: b7dd00d28ecfe844094677e0ae19f4fd359d97d0
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687804"
---
# <a name="front-door-routing-methods"></a>Méthodes de routage Front Door

Azure Front Door prend en charge différentes méthodes de routage du trafic pour déterminer comment router votre trafic HTTP/HTTPS vers les différents points de terminaison de service. Quand chacune des requêtes de vos clients atteint Front Door, la méthode de routage configurée est appliquée pour garantir que les requêtes sont transmises à la meilleure instance de backend. 

Quatre concepts principaux pour le routage de trafic sont disponibles dans Front Door :

* **[Latence](#latency) :** Le routage basé sur la latence garantit que les requêtes sont envoyées aux backends ayant la latence la plus faible acceptables dans une plage de sensibilité. Essentiellement, vos requêtes utilisateur sont envoyées à l’ensemble de backends le plus « proche » en ce qui concerne la latence du réseau.
* **[Priority](#priority):** Vous pouvez attribuer des priorités à vos différents backends si vous voulez utiliser un backend de service principal pour l’ensemble du trafic et fournir une solution de secours en cas d’indisponibilité du backend principal ou des backends de secours.
* **[Weighted](#weighted):** Vous pouvez attribuer des poids à vos différents backends si vous voulez répartir le trafic entre un ensemble de backends, que ce soit de façon équitable ou selon des coefficients de pondération.
* **[Affinité de session](#affinity) :** vous pouvez configurer l’affinité de session pour vos hôtes frontend ou domaines quand vous voulez que les requêtes suivantes d’un utilisateur soient envoyées au même backend tant que la session utilisateur est toujours active et que les sondes d’intégrité indiquent que l’instance de backend est toujours intègre. 

Toutes les configurations Front Door incluent la supervision de l’intégrité des backends et le basculement global instantané automatisé. Pour plus d’informations, consultez [Supervision des backends Front Door](front-door-health-probes.md). Votre porte d’entrée peut être configurée pour fonctionner en se basant sur une méthode de routage unique, ou bien vous pouvez, en fonction des besoins de votre application, utiliser plusieurs ou toutes ces méthodes de routage conjointement afin de créer une topologie de routage optimale.

## <a name="lowest-latencies-based-traffic-routing"></a><a name = "latency"></a>Routage du trafic basé sur les latences les plus faibles

Le déploiement de backends dans deux emplacements ou plus dans le monde peut améliorer la réactivité de nombreuses applications en routant le trafic vers l’emplacement « le plus proche » de vos utilisateurs finaux. La méthode de routage du trafic par défaut pour votre configuration Front Door transfère les requêtes de vos utilisateurs finaux vers le backend le plus proche de l’environnement Front Door qui a reçu la requête. Associée à l’architecture Anycast d’Azure Front Door, cette approche garantit que chacun de vos utilisateurs finaux obtient des performances optimales personnalisées en fonction de son emplacement.

Le backend « le plus proche » n’est pas nécessairement le plus proche en termes de distance géographique. Front Door détermine plutôt les backends les plus proches en mesurant la latence du réseau. Découvrez-en plus sur [l’architecture de routage de Front Door](front-door-routing-architecture.md). 

Vous trouverez ci-dessous le flux décisionnel global :

| Backends disponibles | Priority | Signal de latence (en fonction de sonde d’intégrité) | Poids |
|-------------| ----------- | ----------- | ----------- |
| Tout d’abord, sélectionnez tous les backends qui sont signalés intègres (200 OK) par la sonde d’intégrité. Imaginons qu’il y a six backends A, B, C, D, E et F, parmi lesquels C est non intègre et E est désactivé. Par conséquent, la liste des backends disponibles est A, B, D et F.  | Ensuite, les backends de priorité supérieure parmi ceux disponibles sont sélectionnés. Imaginons que les backends A, B et D ont une priorité 1 et que le backend F a une priorité 2. Les backends sélectionnés seront donc A, B et D.| Sélectionnez les backends ayant une plage de latence (latence minimale et sensibilité de latence spécifiées en millisecondes). Imaginons, si A a la valeur 15 ms, B la valeur 30 ms et D la valeur 60 ms en dehors de l’environnement Front Door où la requête est arrivée et que la sensibilité de latence est de 30 ms, le pool de latences les plus faibles se compose des backends A et B, D étant éloigné de 30 ms du backend le plus proche qui est A. | Enfin, Front Door répartira le trafic par tourniquet (Round Robin) entre le pool de backends sélectionné final dans les proportions définies par les pondérations spécifiées. Par exemple, si le backend A a une pondération de 5 et que le backend B a une pondération de 8, le trafic sera distribué dans les proportions 5:8 entre les backends A et B. |

>[!NOTE]
> Par défaut, la propriété de sensibilité de latence est définie sur 0 ms, ce qui correspond au transfert systématique de la requête au backend disponible le plus rapide.


## <a name="priority-based-traffic-routing"></a><a name = "priority"></a>Routage du trafic basé sur les priorités

Souvent, une organisation souhaite assurer la fiabilité de ses services en déployant un ou plusieurs services de sauvegarde utilisables en cas de défaillance du service principal. Dans l’ensemble du secteur, cette topologie est également appelée « topologie de déploiement Active/En veille ou Active/passive ». La méthode de routage du trafic « Priorité » permet aux clients Azure d’implémenter facilement ce modèle de basculement.

Votre porte d’entrée par défaut contient une liste de backends de priorité égale. Par défaut, Front Door envoie le trafic uniquement aux backends de priorité supérieure (valeur de priorité la plus faible) ; en d’autres termes, l’ensemble de backends principal. Si les backends principaux ne sont pas disponibles, Front Door route le trafic vers l’ensemble de backends secondaire (deuxième valeur de priorité la plus faible). Si les backends principaux et secondaires ne sont pas disponibles, le trafic est envoyé aux backends de troisième rang, et ainsi de suite. La disponibilité du backend est basée sur l’état configuré (activé ou désactivé) et sur l’état d’intégrité du backend en cours comme déterminé par les sondes d’intégrité.

### <a name="configuring-priority-for-backends"></a>Configuration de la priorité des backends

Dans la configuration Front Door, chacun des backends de votre pool de backends a une propriété appelée « Priorité », qui peut être un nombre compris entre 1 et 5. Avec Azure Front Door, vous configurez la priorité des back-ends explicitement à l’aide de cette propriété pour chaque back-end. Cette propriété est une valeur comprise entre 1 et 5. Plus la valeur est basse, plus la priorité est élevée. Des backends peuvent partager des valeurs de priorité.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Méthode de routage du trafic basé sur la pondération
La méthode de routage du trafic « Pondéré » vous permet de répartir le trafic uniformément ou d’utiliser une pondération prédéfinie.

Dans la méthode de routage du trafic basé sur la pondération, vous affectez une pondération à chaque backend dans la configuration Front Door de votre pool de backends. La pondération est un entier compris entre 1 et 1000. Ce paramètre utilise la pondération par défaut « 50 ».

Dans la liste des backends disponibles dans la sensibilité de latence acceptée (tel que spécifié), le trafic est distribué dans un mécanisme de tourniquet (round-robin) dans les proportions définies par les pondérations spécifiées. Si la sensibilité de latence est définie sur 0 milliseconde, cette propriété n’a pas d’effet sauf si deux backends ont la même latence de réseau. 

La méthode pondérée permet des scénarios utiles :

* **Mise à niveau progressive d’une application** : Allouez un pourcentage de trafic à router vers un nouveau backend, puis augmentez progressivement le trafic au fil du temps jusqu’à ce qu’il soit au même niveau que celui des autres backends.
* **Migration d’application vers Azure** : Créez un pool de backends avec à la fois des backends Azure et des backends externes. Ajustez la pondération des backends pour privilégier les nouveaux backends. Pour effectuer progressivement ce paramétrage, commencez par désactiver les nouveaux backends, puis affectez-leur les pondérations les plus faibles, et augmentez-les lentement à des niveaux où ils prennent le plus de trafic. Enfin, désactivez les backends les moins prisés, puis supprimez-les du pool.  
* **Extension du cloud pour une capacité supplémentaire** : Étendez rapidement un déploiement local dans le cloud en le plaçant derrière Front Door. Quand vous avez besoin d’une capacité supplémentaire dans le cloud, vous pouvez ajouter ou activer des backends supplémentaires, et spécifier quelle partie du trafic est destinée à chaque backend.

## <a name="session-affinity"></a><a name = "affinity"></a>Affinité de session
Par défaut, sans affinité de session, Front Door transfère les requêtes provenant du même client à différents back-ends en fonction de la configuration d’équilibrage de charge en particulier quand les latences relatives aux différents back-ends changent ou si différentes requêtes émanant du même utilisateur arrivent sur un environnement Front Door différent. Toutefois, pour certaines applications avec état ou dans certains autres scénarios, il est préférable que les requêtes suivantes du même utilisateur arrivent sur le même backend que celui qui a traité la requête initiale. La fonctionnalité d’affinité de session basée sur les cookies est utile quand vous souhaitez conserver une session utilisateur sur le même backend. En utilisant des cookies gérés de Front Door, Azure Front Door peut diriger le trafic suivant provenant d’une session utilisateur vers le même back-end pour qu’il soit traité, tant que le back-end est sain et que la session utilisateur n’a pas expiré. 

L’affinité de session peut être activée au niveau de l’hôte frontend pour chacun des domaines (ou sous-domaines) que vous avez configurés. Une fois qu’elle est activée, Front Door ajoute un cookie à la session de l’utilisateur. L’affinité de session basée sur les cookies permet à Front Door d’identifier différents utilisateurs même s’ils utilisent la même adresse IP, ce qui permet une répartition plus équitable du trafic entre vos différents backends.

La durée de vie du cookie est identique à celle de la session de l’utilisateur, car Front Door ne prend actuellement en charge que le cookie de session. 

> [!NOTE]
> Les proxys publics peuvent interférer avec l’affinité de session. En effet, l’établissement d’une session nécessite que Front Door ajoute un cookie d’affinité de session à la réponse. Cette opération ne peut pas être effectuée si la réponse peut être mise en cache, car cela perturberait les cookies d’autres clients demandant la même ressource. Pour vous protéger contre cela, l’affinité de session n’est **pas** établie si le backend envoie une réponse pouvant être mise en cache quand vous tentez cette opération. Si la session a déjà été établie, le fait que la réponse du backend puisse être mise en cache n’a pas d’importance.
> L’affinité de session est établie dans les circonstances suivantes, **sauf si** la réponse a un code d’état HTTP 304 :
> - La réponse a des valeurs spécifiques définies pour l’en-tête ```Cache-Control``` qui empêche la mise en cache, comme « private » ou « non-store ».
> - La réponse contient un en-tête ```Authorization``` qui n’a pas expiré.
> - La réponse a un code d’état HTTP 302.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [créer une porte d’entrée](quickstart-create-front-door.md).
- Découvrez [comment fonctionne Front Door](front-door-routing-architecture.md).
