---
title: Paramètres d’application - LUIS
description: Les paramètres d’application pour des applications Language Understanding Azure Cognitive Services sont stockés dans l’application et le portail.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 9e17736cd6ff5074a6eab76a6cf5bdb8acedc185
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382198"
---
# <a name="application-settings"></a>Paramètres de l’application

Ces paramètres d’application sont stockés dans l’application [exportée](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) et [mis à jour](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) avec les API REST. Le fait de modifier vos paramètres de version d’application réinitialise le statut d’entraînement de votre application sur « Non entraînée ».

Découvrez les [concepts](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) des signes diacritiques et de la ponctuation.

|Paramètre|Valeur par défaut|Notes|
|--|--|--|
|NormalizePunctuation|True|Supprime les signes de ponctuation.|
|NormalizeDiacritics|True|Supprime les signes diacritiques.|

## <a name="diacritics-normalization"></a>Normalisation des signes diacritiques

Activez la normalisation de l’énoncé pour les signes diacritiques dans votre fichier d’application LUIS JSON avec le paramètre `settings`.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

Les énoncés suivants montrent l’impact de la normalisation des signes diacritiques sur les énoncés :

|Avec les signes diacritiques définis sur false|Avec les signes diacritiques définis sur true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Prise en charge linguistique pour les signes diacritiques

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Signes diacritiques pour le Portugais (Brésil) `pt-br`

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`|
|||

#### <a name="dutch-nl-nl-diacritics"></a>Signes diacritiques pour le Néerlandais `nl-nl`

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Signes diacritiques pour le Français `fr-`

Cela inclut les sous-cultures Français et Canadien.

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`|
|`ê`|`e`|
|`î`|`i`|
|`ô`|`o`|
|`û`|`u`|
|`ç`|`c`|
|`ë`|`e`|
|`ï`|`i`|
|`ü`|`u`|
|`ÿ`|`y`|

#### <a name="german-de-de-diacritics"></a>Signes diacritiques pour l’Allemand `de-de`

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|--|--|
|`ä`|`a`|
|`ö`|`o`|
|`ü`|`u`|

#### <a name="italian-it-it-diacritics"></a>Signes diacritiques pour l’Italien `it-it`

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`|
|`í`|`i`|
|`î`|`i`|
|`ò`|`o`|
|`ó`|`o`|
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Signes diacritiques pour l’Espagnol `es-`

Cela inclut les sous-cultures Mexicain et Espagnol.

|Signes diacritiques définis sur false|Signes diacritiques définis sur true|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalisation des signes de ponctuation

Activez la normalisation de l’énoncé pour la ponctuation dans votre fichier d’application LUIS JSON avec le paramètre `settings`.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
]
```

Les énoncés suivants montrent l’impact de la ponctuation sur les énoncés :

|Avec la ponctuation définie sur False|Avec la ponctuation définie sur True|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Ponctuation supprimée

La ponctuation suivante est supprimée quand `NormalizePunctuation` a la valeur true.

|Ponctuation|
|--|
|`-`|
|`.`|
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|

## <a name="next-steps"></a>Étapes suivantes

* Découvrez les [concepts](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) des signes diacritiques et de la ponctuation.
