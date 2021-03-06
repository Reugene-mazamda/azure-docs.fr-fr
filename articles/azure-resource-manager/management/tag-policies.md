---
title: Stratégies pour l’étiquetage des ressources
description: Décrit les stratégies Azure que vous pouvez attribuer pour garantir la conformité des étiquettes.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e3eeb28ea23b18c3492f68d2fac294fc014420c5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147869"
---
# <a name="assign-policies-for-tag-compliance"></a>Attribuer des stratégies pour la conformité des étiquettes

Vous pouvez utiliser [Azure Policy](../../governance/policy/overview.md) pour appliquer des règles et conventions d’étiquetage. En créant une stratégie, vous pouvez éviter de déployer dans votre abonnement des ressources n’ayant pas les étiquettes attendues pour votre organisation. Au lieu d’appliquer des étiquettes ou de rechercher des ressources non conformes manuellement, vous pouvez créer une stratégie qui applique automatiquement les étiquettes nécessaires pendant le déploiement. Les étiquettes peuvent également être appliquées aux ressources existantes avec le nouvel effet [Modifier](../../governance/policy/concepts/effects.md#modify) et une [tâche de correction](../../governance/policy/how-to/remediate-resources.md). La section suivante présente des exemples de stratégies pour les étiquettes.

## <a name="policies"></a>Stratégies

[!INCLUDE [Tag policies](../../../includes/policy/samples/bycat/policies-tags.md)]

## <a name="next-steps"></a>Étapes suivantes

* Pour en savoir plus sur la catégorisation des ressources, consultez [Organisation des ressources Azure à l’aide d’étiquettes](tag-resources.md).
* Les types de ressources ne prennent pas tous en charge les étiquettes. Pour déterminer si vous pouvez appliquer une étiquette à un type de ressource, consultez [Prise en charge des étiquettes pour les ressources Azure](tag-support.md).
