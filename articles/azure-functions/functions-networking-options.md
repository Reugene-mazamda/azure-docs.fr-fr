---
title: Options de mise en réseau d’Azure Functions
description: Vue d’ensemble de toutes les options de mise en réseau disponibles dans Azure Functions.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 6637627d48df8f9b6126debc215aac9bceb76f6b
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419534"
---
# <a name="azure-functions-networking-options"></a>Options de mise en réseau d’Azure Functions

Cet article décrit les fonctionnalités de mise en réseau disponibles dans les options d’hébergement pour Azure Functions. Toutes les options de mise en réseau suivantes vous permettent d’accéder à des ressources sans utiliser d’adresses routables sur Internet, ou de restreindre l’accès à Internet à une application de fonction.

Les modèles d’hébergement offrent différents niveaux d’isolement réseau. Le choix de l’option appropriée vous aide à répondre à vos besoins d’isolement réseau.

Vous pouvez héberger des applications de fonction de deux façons :

* Vous avez le choix entre des plans qui s’exécutent sur une infrastructure mutualisée, avec divers niveaux de connectivité au réseau virtuel et diverses options de mise à l’échelle :
    * Le [plan Consommation](functions-scale.md#consumption-plan), qui s’adapte de façon dynamique en réponse à la charge et offre des options d’isolement réseau minimal.
    * Le [plan Premium](functions-scale.md#premium-plan), qui s’adapte de façon dynamique tout en offrant un isolement réseau plus complet.
    * Le plan [Azure App Service](functions-scale.md#app-service-plan) opère à une échelle fixe et offre un offre isolement réseau similaire au plan Premium.
* Vous pouvez exécuter des fonctions dans un [Azure App Service Environment](../app-service/environment/intro.md). Cette méthode déploie votre fonction dans votre réseau virtuel et offre un contrôle et un isolement réseau complets.

## <a name="matrix-of-networking-features"></a>Matrice de fonctionnalités de mise en réseau

|                |[Plan Consommation](functions-scale.md#consumption-plan)|[Plan Premium](functions-scale.md#premium-plan)|[Plan App Service](functions-scale.md#app-service-plan)|[Environnement App Service](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Restrictions d’adresses IP entrantes et accès aux sites privés](#inbound-ip-restrictions)|✅Oui|✅Oui|✅Oui|✅Oui|
|[Intégration du réseau virtuel](#virtual-network-integration)|❌Non|✅Oui (Zones géographiques)|✅Oui (Zones géographiques et Passerelle)|✅Oui|
|[Déclencheurs de réseau virtuel (non HTTP)](#virtual-network-triggers-non-http)|❌Non| ✅Oui |✅Oui|✅Oui|
|[Connexions hybrides](#hybrid-connections) (Windows uniquement)|❌Non|✅Oui|✅Oui|✅Oui|
|[Restrictions d’adresse IP sortantes](#outbound-ip-restrictions)|❌Non| ✅Oui|✅Oui|✅Oui|

## <a name="inbound-ip-restrictions"></a>Restrictions d’adresse IP entrantes

Vous pouvez utiliser des restrictions d’adresse IP pour définir la liste des adresses IP classées par ordre de priorité qui sont autorisées ou non à accéder à votre application. La liste peut inclure des adresses IPv4 et IPv6. Lorsqu’il y a une ou plusieurs entrées, une règle implicite « Tout refuser » se trouve à la fin de la liste. Les restrictions d’adresse IP fonctionnent avec toutes les options d’hébergement de fonction.

> [!NOTE]
> Une fois les restrictions réseau en place, vous ne pouvez utiliser l'éditeur du portail qu'à partir de votre réseau virtuel ou après avoir ajouté l'adresse IP de la machine que vous utilisez pour accéder au Portail Azure sur la liste des destinataires approuvés. Néanmoins, vous pouvez aussi accéder aux fonctionnalités à partir de l'onglet **Fonctionnalités de la plateforme** de n'importe quel ordinateur.

Pour en savoir plus, consultez [Restrictions d’accès statique Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Accès aux sites privés

L’accès aux sites privés fait référence au fait de rendre votre application accessible uniquement à partir d’un réseau privé, par exemple, à partir d’un réseau virtuel Azure.

* L’accès aux sites privés est disponible dans les plans [Premium](./functions-premium-plan.md), [Consommation](functions-scale.md#consumption-plan) et [App Service](functions-scale.md#app-service-plan) quand des points de terminaison de service sont configurés.
    * Les points de terminaison de service peuvent être configurés pour chaque application, sous **Fonctionnalités de la plateforme** > **Mise en réseau** > **Configurer des restrictions d’accès** > **Ajouter une règle**. Les réseaux virtuels peuvent maintenant être sélectionnés comme un type de règle.
    * Pour plus d’informations, consultez [Points de terminaison de service de réseau virtuel](../virtual-network/virtual-network-service-endpoints-overview.md).
    * N'oubliez pas qu'avec les points de terminaison de service, votre fonction dispose toujours d'un accès sortant complet à Internet, même si l'intégration au réseau virtuel est configurée.
* L'accès aux sites privés est également disponible via une instance d'Azure App Service Environment configurée avec un équilibreur de charge interne (ILB). Pour plus d’informations, consultez [Créer et utiliser un équilibreur de charge interne avec un Azure App Service Environment](../app-service/environment/create-ilb-ase.md).

Pour apprendre à configurer l’accès privé aux site, consultez [Établir l’accès privé aux sites avec Azure Functions](functions-create-private-site-access.md).

## <a name="virtual-network-integration"></a>Intégration du réseau virtuel

L’intégration de réseau virtuel permet à votre Function App d’accéder aux ressources au sein d’un réseau virtuel.
Azure Functions prend en charge deux types d’intégration de réseau virtuel :

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Dans Azure Functions, l'intégration au réseau virtuel utilise une infrastructure partagée avec les applications Web App Service. Pour en savoir plus sur les deux types d'intégration au réseau virtuel, consultez :

* [Intégration au réseau virtuel régional](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Intégration au réseau virtuel avec passerelle obligatoire](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Pour savoir comment configurer l’intégration au réseau virtuel, consultez [Intégrer une application de fonction à un réseau virtuel Azure](functions-create-vnet.md).

## <a name="regional-virtual-network-integration"></a>Intégration du réseau virtuel régional

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connect-to-service-endpoint-secured-resources"></a>Se connecter à des ressources sécurisées de point de terminaison de service

Afin d’offrir un niveau de sécurité supérieur, vous pouvez restreindre un nombre de services Azure sur un réseau virtuel en utilisant des points de terminaison de service. Vous devez ensuite intégrer votre application de fonction à ce réseau virtuel pour accéder à la ressource. Cette configuration est prise en charge sur tous les plans qui prennent en charge l’intégration du réseau virtuel.

Pour plus d’informations, consultez [Points de terminaison de service de réseau virtuel](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>Restreindre votre compte de stockage à un réseau virtuel

Quand vous créez une application de fonction, vous devez créer un compte de stockage Azure à usage général qui prend en charge le stockage Blob, File d’attente et Table, ou établir un lien vers un compte de ce type. Actuellement, vous ne pouvez pas utiliser de restrictions de réseau virtuel sur ce compte. La configuration d’un point de terminaison de service de réseau virtuel sur le compte de stockage que vous utilisez pour votre application de fonction entraîne l’arrêt de votre application.

Pour plus d’informations, consultez [Exigences pour le compte de stockage](./functions-create-function-app-portal.md#storage-account-requirements).

## <a name="use-key-vault-references"></a>Utiliser des références Key Vault

Vous pouvez utiliser des références Azure Key Vault pour utiliser des secrets d’Azure Key Vault dans votre application Azure Functions, sans avoir à modifier le code. Azure Key Vault est un service qui fournit une gestion centralisée des secrets, avec un contrôle total sur les stratégies d’accès et l’historique d’audit.

Actuellement, les [références Key Vault](../app-service/app-service-key-vault-references.md) ne fonctionnent pas si votre coffre de clés est sécurisé à l’aide de points de terminaison de service. Pour vous connecter à un coffre de clés en utilisant l’intégration de réseau virtuel, vous devez appeler le coffre de clés dans le code de votre application.

## <a name="virtual-network-triggers-non-http"></a>Déclencheurs de réseau virtuel (non HTTP)

Vous pouvez utiliser des fonctions de déclencheur non-HTTP à partir d’un réseau virtuel de deux manières :

+ Exécutez votre application de fonction dans un plan Premium, et activez la prise en charge des déclencheurs de réseau virtuel.
+ Exécutez votre application de fonction dans un plan ou environnement App Service.

### <a name="premium-plan-with-virtual-network-triggers"></a>Plan Premium avec déclencheurs de réseau virtuel

En cas d’exécution dans un plan Premium, vous pouvez connecter des fonctions de déclencheur non-HTTP à des services s’exécutant au sein d’un réseau virtuel. Pour ce faire, vous devez activer la prise en charge des déclencheurs de réseau virtuel pour votre application de fonction. Le paramètre de **prise en charge des déclencheurs de réseau virtuel** se trouve dans le [portail Azure](https://portal.azure.com), sous **Paramètres de l’application de fonction**.

![Bascule de réseau virtuel](media/functions-networking-options/virtual-network-trigger-toggle.png)

Vous pouvez également activer les déclencheurs de réseau virtuel en utilisant la commande Azure CLI suivante :

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Les déclencheurs de réseau virtuel sont pris en charge dans la version 2.x et les versions ultérieures du runtime Functions. Les types de déclencheurs non-HTTP suivants sont pris en charge.

| Extension | Version minimale |
|-----------|---------| 
|[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 ou ultérieure |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 ou ultérieure|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 ou ultérieure|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 ou ultérieure|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 ou ultérieure|

> [!IMPORTANT]
> Lorsque vous activez la prise en charge des déclencheurs de réseau virtuel, seuls les types de déclencheurs présentés dans le tableau précédent sont mis à l’échelle de façon dynamique avec votre application. Vous pouvez toujours utiliser les déclencheurs qui ne figurent pas dans le tableau, mais ils ne sont pas mis à l’échelle au-delà de leur nombre d’instances préchauffées. Pour obtenir la liste complète des déclencheurs, consultez [Déclencheurs et liaisons](./functions-triggers-bindings.md#supported-bindings).

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>Plan App Service et App Service Environment avec déclencheurs de réseau virtuel

Lorsque votre application de fonction s’exécute dans un plan App Service ou un App Service Environment, vous pouvez utiliser des fonctions de déclencheur non-HTTP. Pour le bon déclenchement de vos fonctions, vous devez être connecté à un réseau virtuel, avec accès à la ressource définie dans la connexion de déclenchement.

Supposons, par exemple, que vous souhaitiez configurer Azure Cosmos DB pour accepter le trafic uniquement à partir d’un réseau virtuel. Dans ce cas, vous devez déployer votre application de fonction dans un plan App Service fournissant une intégration de réseau virtuel à ce réseau virtuel. L’intégration permet donc qu’une fonction soit déclenchée par cette ressource Azure Cosmos DB.

## <a name="hybrid-connections"></a>les connexions hybrides

[Connexions hybrides](../service-bus-relay/relay-hybrid-connections-protocol.md) est une fonctionnalité d’Azure Relay que vous pouvez utiliser pour accéder aux ressources d’application dans d’autres réseaux. Elles permettent d’accéder depuis votre application à un point de terminaison d’application. Vous ne pouvez pas l’utiliser pour accéder à votre application. La fonctionnalité Connexions hybrides est disponible pour les fonctions exécutées sur Windows dans tous les plans, sauf le plan Consommation.

Utilisée dans Azure Functions, chaque connexion hybride correspond à une combinaison d’hôte et de port TCP unique. Cela signifie que le point de terminaison de connexion hybride peut se trouver sur un quelconque système d’exploitation et toute application tant que vous accédez à un port d’écoute TCP. La fonctionnalité Connexions hybrides ne détecte pas et ne prend pas en compte le protocole d’application ou les ressources auxquels vous accédez. Elle fournit simplement un accès réseau.

Pour plus d’informations, consultez la [documentation App Service pour les connexions hybrides](../app-service/app-service-hybrid-connections.md). Ces mêmes étapes de configuration prennent en charge Azure Functions.

>[!IMPORTANT]
> Les connexions hybrides sont uniquement prises en charge dans les plans Windows. Linux n’est pas pris en charge.

## <a name="outbound-ip-restrictions"></a>Restrictions d’adresse IP sortantes

Les restrictions d’adresse IP sortante sont disponibles dans un plan Premium, un plan App Service ou un App Service Environment. Vous pouvez configurer des restrictions sortantes pour le réseau virtuel sur lequel votre Azure App Service Environment est déployé.

Lorsque vous intégrez une application de fonction à un réseau virtuel dans le cadre d'un plan Premium ou App Service, l’application est toujours en mesure de passer des appels sortants vers Internet par défaut. En ajoutant le paramètre d’application `WEBSITE_VNET_ROUTE_ALL=1`, vous forcez l’envoi de tout le trafic sortant vers votre réseau virtuel, où des règles de groupe de sécurité réseau peuvent être utilisées pour restreindre le trafic.

## <a name="troubleshooting"></a>Dépannage

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la mise en réseau et Azure Functions :

* [Tutoriel pour bien démarrer avec l’intégration de réseau virtuel](./functions-create-vnet.md)
* [Lire les questions fréquentes (FAQ) sur la mise en réseau de Functions](./functions-networking-faq.md)
* [En savoir plus sur l’intégration de réseau virtuel avec App Service/Functions](../app-service/web-sites-integrate-with-vnet.md)
* [En savoir plus sur les réseaux virtuels dans Azure](../virtual-network/virtual-networks-overview.md)
* [Activer davantage de fonctionnalités de réseau et plus de contrôle avec Azure App Service Environment](../app-service/environment/intro.md)
* [Se connecter à des ressources locales sans modifier le pare-feu à l’aide des connexions hybrides](../app-service/app-service-hybrid-connections.md)
