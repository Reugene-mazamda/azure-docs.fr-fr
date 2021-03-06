---
title: 'Langage de requête Azure Cosmos DB : SUBSTRING'
description: Découvrez la fonction système SQL SUBSTRING dans Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d4462fc407093b23510bddfae4d9f55d68f8c0fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303696"
---
# <a name="substring-azure-cosmos-db"></a>SUBSTRING (Azure Cosmos DB)
 Renvoie une partie d’une expression de chaîne commençant à la position de caractère spécifiée (avec base zéro) et se poursuit jusqu'à la longueur spécifiée ou à la fin de la chaîne.  
  
## <a name="syntax"></a>Syntaxe
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Arguments
  
*str_expr*  
   Est une expression de chaîne.
  
*num_expr1*  
   Est une expression numérique indiquant le caractère de début. La valeur 0 est le premier caractère de *str_expr*.
  
*num_expr2*  
   Est une expression numérique indiquant le nombre maximal de caractères de *str_expr* à retourner. La valeur 0 ou une valeur inférieure génère une chaîne vide.

## <a name="return-types"></a>Types de retour
  
  Retourne une expression de chaîne.  
  
## <a name="examples"></a>Exemples
  
  L’exemple suivant retourne la sous-chaîne de « abc » commençant à 1 pour une longueur de 1 caractère.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Voici le jeu de résultats obtenu.  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Notes

Cette fonction système bénéficiera d’un [index de plage](index-policy.md#includeexclude-strategy) si sa position de départ est `0`.

## <a name="next-steps"></a>Étapes suivantes

- [Fonctions de chaîne Azure Cosmos DB](sql-query-string-functions.md)
- [Fonctions système Azure Cosmos DB](sql-query-system-functions.md)
- [Présentation d’Azure Cosmos DB](introduction.md)
