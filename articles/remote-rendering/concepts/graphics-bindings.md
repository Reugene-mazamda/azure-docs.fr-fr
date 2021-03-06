---
title: Liaison graphique
description: Configuration de liaisons graphiques et cas d’utilisation
author: florianborn71
manager: jlyons
services: azure-remote-rendering
titleSuffix: Azure Remote Rendering
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.service: azure-remote-rendering
ms.openlocfilehash: 8b5db0532f3dcc8b6dfb024238d0cacff2e6d2a1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679411"
---
# <a name="graphics-binding"></a>Liaison graphique

Pour pouvoir utiliser Azure Remote Rendering dans une application personnalisée, vous devez l’intégrer dans le pipeline de rendu de l’application. Cette intégration est la responsabilité de la liaison graphique.

Une fois configurée, la liaison graphique donne accès à différentes fonctions qui agissent sur l’image rendue. Ces fonctions peuvent être divisées en deux catégories : des fonctions générales qui sont toujours disponibles et des fonctions spécifiques qui ne sont pertinentes que pour le `Microsoft.Azure.RemoteRendering.GraphicsApiType` sélectionné.

## <a name="graphics-binding-in-unity"></a>Liaison graphique dans Unity

Dans Unity, la liaison entière est gérée par le struct `RemoteUnityClientInit` passé dans `RemoteManagerUnity.InitializeManager`. Pour définir le mode graphique, le champ `GraphicsApiType` doit être défini sur la liaison choisie. Le champ est renseigné automatiquement selon qu’un XRDevice est présent ou non. Le comportement peut être substitué manuellement par les comportements suivants :

* **HoloLens 2** : la liaison graphique [Windows Mixed Reality](#windows-mixed-reality) est toujours utilisée.
* **Application de bureau UWP plate** : la [simulation](#simulation) est toujours utilisée. Pour utiliser ce mode, veillez à suivre les étapes décrites dans [Didacticiel : Configuration d’un projet Unity en partant de zéro](../tutorials/unity/project-setup.md).
* **Éditeur Unity** : la [simulation](#simulation) est toujours utilisée à moins qu’un casque de réalité virtuelle WMR soit connecté, auquel cas ARR est désactivé pour permettre le débogage des parties non-ARR de l’application. Voir aussi la [communication à distance holographique](../how-tos/unity/holographic-remoting.md).

La seule autre partie pertinente pour Unity est l’accès à la [liaison de base](#access), toutes les autres sections ci-dessous peuvent être ignorées.

## <a name="graphics-binding-setup-in-custom-applications"></a>Configuration de la liaison graphique dans des applications personnalisées

Pour sélectionner une liaison graphique, effectuez les deux étapes suivantes : Tout d’abord, la liaison graphique doit être initialisée de manière statique lorsque le programme est initialisé :

``` cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

L’appel ci-dessus est nécessaire pour initialiser Azure Remote Rendering dans les API holographiques. Cette fonction doit être appelée avant l’appel d’une API holographique et avant d’accéder à d’autres API Remote Rendering. De même, la fonction de-init correspondante `RemoteManagerStatic.ShutdownRemoteRendering();` doit être appelée lorsqu’aucune API holographique n’est plus appelée.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Accès aux liaisons graphiques

Une fois qu’un client est configuré, la liaison graphique de base est accessible à l’aide du getter `AzureSession.GraphicsBinding`. Par exemple, les statistiques des dernières images peuvent être récupérées comme suit :

``` cs
AzureSession currentSesson = ...;
if (currentSesson.GraphicsBinding)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        ...
    }
}
```

## <a name="graphic-apis"></a>API graphiques

Il existe actuellement deux API graphiques qui peuvent être sélectionnées, `WmrD3D11` et `SimD3D11`. Une troisième (`Headless`) existe, mais n’est pas encore pris en charge côté client.

### <a name="windows-mixed-reality"></a>Windows Mixed Reality

`GraphicsApiType.WmrD3D11` est la liaison par défaut à exécuter sur HoloLens 2. Elle crée la liaison `GraphicsBindingWmrD3d11`. Dans ce mode, Azure Remote Rendering est directement branché aux API holographiques.

Pour accéder aux liaisons graphiques dérivées, la `GraphicsBinding` de base doit être castée.
Vous devez effectuer deux opérations pour utiliser la liaison WMR :

#### <a name="inform-remote-rendering-of-the-used-coordinate-system"></a>Informer Remote Rendering du système de coordonnées utilisé

``` cs
AzureSession currentSesson = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (binding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

Où le `ptr` ci-dessus doit être un pointeur vers un objet `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem` natif qui définit le système de coordonnées de l’espace universel dans lequel les coordonnées de l’API sont exprimées.

#### <a name="render-remote-image"></a>Afficher l’image distante

Au début de chaque image, l’image distante doit être affichée dans la mémoire tampon d’arrière-plan. Pour ce faire, appelez `BlitRemoteFrame`, qui remplit à la fois les informations de couleur et de profondeur dans la cible de rendu actuellement liée. Par conséquent, il est important de le faire après avoir lié la mémoire tampon d’arrière-plan en tant que cible de rendu.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
binding.BlitRemoteFrame();
```

### <a name="simulation"></a>Simulation

`GraphicsApiType.SimD3D11` est la liaison de simulation et, si elle est sélectionnée, elle crée la liaison graphique `GraphicsBindingSimD3d11`. Cette interface est utilisée pour simuler le mouvement de la tête, par exemple dans une application de bureau, et affiche une image monoscopique.
La configuration est un peu plus complexe et fonctionne comme suit :

#### <a name="create-proxy-render-target"></a>Créer un proxy de cible de rendu

Le contenu local et distant doit être affiché sur une cible de rendu des couleurs et de la profondeur hors écran appelée « proxy » à l’aide des données de la caméra proxy fournies par la fonction `GraphicsBindingSimD3d11.Update`. Le proxy doit correspondre à la résolution de la mémoire tampon d’arrière-plan. Une fois qu’une session est prête, `GraphicsBindingSimD3d11.InitSimulation` doit être appelée avant de l’y connecter :

``` cs
AzureSession currentSesson = ...;
IntPtr d3dDevice = ...; // native pointer to ID3D11Device
IntPtr color = ...; // native pointer to ID3D11Texture2D
IntPtr depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
simBinding.InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

La fonction init doit être fournie avec des pointeurs vers l’appareil D3D natif, ainsi que vers la texture de couleur et de profondeur du proxy de la cible de rendu. Une fois initialisées, `AzureSession.ConnectToRuntime` et `DisconnectFromRuntime` peuvent être appelées plusieurs fois, mais, lors du basculement vers une autre session, `GraphicsBindingSimD3d11.DeinitSimulation` doit d’abord être appelée sur l’ancienne session avant que `GraphicsBindingSimD3d11.InitSimulation` puisse être appelée sur une autre session.

#### <a name="render-loop-update"></a>Mise à jour de la boucle de rendu

La mise à jour de la boucle de rendu se compose de plusieurs étapes :

1. Pour chaque image et avant tout rendu, `GraphicsBindingSimD3d11.Update` est appelée avec la transformation de caméra actuelle qui est envoyée au serveur pour être affichée. En même temps, la transformation du proxy retourné doit être appliquée à la caméra proxy pour s’afficher dans le proxy de la cible de rendu.
Si la mise à jour du proxy retournée `SimulationUpdate.frameId` a la valeur Null, il n’y a pas encore de données distantes. Dans ce cas, au lieu d’être affiché dans le proxy de la cible de rendu, tout contenu local doit être affiché directement dans la mémoire tampon d’arrière-plan à l’aide des données de caméra actuelles et les deux étapes suivantes sont ignorées.
1. L’application doit maintenant lier le proxy de la cible de rendu et appeler `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy`. Cela remplit les informations distantes de couleur et de profondeur dans le proxy de la cible de rendu. Tout contenu local peut maintenant être affiché sur le proxy à l’aide de la transformation de caméra proxy.
1. Ensuite, la mémoire tampon d’arrière-plan doit être liée en tant que cible de rendu et `GraphicsBindingSimD3d11.ReprojectProxy` appelée, ce qui permet de présenter la mémoire tampon d’arrière-plan.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
SimulationUpdate update = new SimulationUpdate();
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate = new SimulationUpdate();
simBinding.Update(update, out proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding.BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding.ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

## <a name="next-steps"></a>Étapes suivantes

* [Tutoriel : Configuration d’un projet Unity en partant de zéro](../tutorials/unity/project-setup.md)
