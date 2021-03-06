---
title: Components
description: Définition des composants dans l’étendue d’Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.openlocfilehash: cb8b38addef736914a8627971e57ea2b173293d6
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679415"
---
# <a name="components"></a>Components

Azure Remote Rendering utilise le modèle de [système de composants d’entité](https://en.wikipedia.org/wiki/Entity_component_system). Alors que les [entités](entities.md) représentent la position et la composition hiérarchique des objets, les composants sont responsables de l’implémentation du comportement.

Les types de composants les plus fréquemment utilisés sont les [composants de maillage](meshes.md) qui ajoutent des mailles dans le pipeline de rendu. De même, des [composants légers](../overview/features/lights.md) sont utilisés pour ajouter de l’éclairage et des [composants de plan de coupe](../overview/features/cut-planes.md) pour couper les maillages ouverts.

Tous ces composants utilisent comme point de référence la transformation (position, rotation, échelle) de l’entité à laquelle ils sont attachés.

## <a name="working-with-components"></a>Utilisation des composants

Vous pouvez facilement ajouter, supprimer et manipuler des composants par programme :

```cs
// create a point light component
AzureSession session = GetCurrentlyConnectedSession();
PointLightComponent lightComponent = session.Actions.CreateComponent(ObjectType.PointLightComponent, ownerEntity) as PointLightComponent;

lightComponent.Color = new Color4Ub(255, 150, 20, 255);
lightComponent.Intensity = 11;

// ...

// destroy the component
lightComponent.Destroy();
lightComponent = null;
```

Un composant est attaché à une entité au moment de la création. Il n’est pas possible de le déplacer vers une autre entité par la suite. Les composants sont supprimés explicitement avec `Component.Destroy()` ou automatiquement lors de la destruction de l’entité propriétaire du composant.

Il n’est possible d’ajouter à une entité qu’une seule instance de chaque type de composant à la fois.

## <a name="unity-specific"></a>Spécificité d’Unity

L’intégration d’Unity a des fonctions d’extension supplémentaires pour interagir avec les composants. Consultez [Composants et objets de jeu Unity](../how-tos/unity/objects-components.md).

## <a name="next-steps"></a>Étapes suivantes

* [Limites des objets](object-bounds.md)
* [Maillages](meshes.md)
