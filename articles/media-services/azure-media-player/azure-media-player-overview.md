---
title: Présentation du Lecteur multimédia Azure
description: Apprenez-en davantage sur le Lecteur multimédia Azure.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: b6d30aebd4de272ba98fce87f23701b129eacb02
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727556"
---
# <a name="azure-media-player-overview"></a>Présentation du lecteur multimédia Azure #

Le Lecteur multimédia Azure est un lecteur vidéo web, conçu pour lire du contenu multimédia à partir de [Microsoft Azure Media Services](https://azure.microsoft.com/services/media-services/), sur un grand nombre de navigateurs et d’appareils. Azure Media Player exploite des normes du secteur, telles que HTML5, Media Source Extensions (MSE) et Encrypted Media Extensions (EME), pour offrir une expérience enrichie de diffusion en continu adaptative.  Lorsque ces normes ne sont pas disponibles sur un périphérique ou dans un navigateur, Azure Media Player utilise Flash et Silverlight comme technologies de secours. Quelle que soit la technologie de lecture utilisée, les développeurs bénéficient d’une interface JavaScript unifiée pour accéder aux API.  Le contenu fourni par Azure Media Services peut ainsi être lu sur un large éventail de périphériques et de navigateurs sans effort supplémentaire.

Microsoft Azure Media Services permet la distribution de contenu dans les formats de streaming DASH, Smooth Streaming et HLS pour la lecture. Azure Media Player prend en compte des différents formats et lit automatiquement le lien le mieux adapté aux capacités de la plateforme/du navigateur. Microsoft Azure Media Services assure également le chiffrement dynamique des ressources avec le chiffrement commun (PlayReady ou Widevine) ou le chiffrement d’enveloppe AES 128 bits. Lorsqu’il est configuré de manière appropriée, Azure Media Player permet le déchiffrement du contenu chiffré avec PlayReady et AES 128 bits.  Consultez la section [Contenu protégé](azure-media-player-protected-content.md) pour savoir comment configurer cela.

Pour demander de nouvelles fonctionnalités ou nous faire part de vos idées ou commentaires, écrivez-nous sur [UserVoice pour le Lecteur multimédia Azure](https://aka.ms/ampuservoice). Si vous avez des problèmes ou des questions spécifiques, ou si vous détectez des bogues, contactez-nous à l’adresse ampinfo@microsoft.com.

[Inscrivez-vous](https://aka.ms/ampsignup) pour ne jamais manquer une version et vous tenir informé des nouveautés que le Lecteur multimédia Azure a à offrir.

> [!NOTE]
> Notez que Lecteur multimédia Azure ne prend en charge que les flux multimédias provenant d’Azure Media Services.

## <a name="license"></a>Licence ##

Le Lecteur multimédia Azure est concédé sous licence, et soumis aux conditions énoncées dans les termes du contrat de licence logiciel Microsoft du Lecteur multimédia Azure. Consultez le [fichier de licence](azure-media-player-license.md) pour connaître l’intégralité des conditions. Pour plus d’informations, consultez la [Déclaration de confidentialité](https://www.microsoft.com/en-us/privacystatement/default.aspx).

Copyright 2015, Microsoft Corporation.

## <a name="next-steps"></a>Étapes suivantes ##

- [Démarrage rapide du Lecteur multimédia Azure](azure-media-player-quickstart.md)