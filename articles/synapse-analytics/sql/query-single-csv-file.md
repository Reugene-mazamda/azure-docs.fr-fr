---
title: Interroger des fichiers CSV à l’aide de SQL à la demande (préversion)
description: Cet article explique comment interroger des fichiers CSV individuels de formats différents en utilisant SQL à la demande (préversion).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3d09692c06bcdffbb070f545950092592e417838
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427557"
---
# <a name="query-csv-files"></a>Interroger des fichiers CSV

Cet article explique comment interroger un fichier CSV à l’aide de SQL à la demande (préversion) dans Azure Synapse Analytics. Les fichiers CSV peuvent avoir des formats différents : 

- Avec ou sans ligne d’en-tête
- Valeurs délimitées par des virgules ou des tabulations
- Fins de ligne de style Windows ou Unix
- Valeurs sans guillemets ou entre guillemets, et caractères d’échappement

Toutes les variantes ci-dessus sont abordées ci-dessous.

## <a name="prerequisites"></a>Prérequis

Avant de lire le reste de cet article, consultez les articles suivants :

- [Première configuration](query-data-storage.md#first-time-setup)
- [Composants requis](query-data-storage.md#prerequisites)

## <a name="windows-style-new-line"></a>Nouvelle ligne de style Windows

La requête suivante montre comment lire un fichier CSV sans ligne d’en-tête qui comprend des caractères de nouvelle ligne de style Windows et des colonnes délimitées par des virgules.

Aperçu du fichier :

![10 premières lignes du fichier CSV sans en-tête, avec caractères de nouvelle ligne de style Windows.](./media/query-single-csv-file/population.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>Nouvelle ligne de style Unix

La requête suivante montre comment lire un fichier sans ligne d’en-tête, avec une nouvelle ligne de style Unix et des colonnes délimitées par des virgules. Notez l’emplacement différent du fichier par rapport aux autres exemples.

Aperçu du fichier :

![10 premières lignes du fichier CSV sans ligne d’en-tête, avec nouvelle ligne de style Unix.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="header-row"></a>Ligne d’en-tête

La requête suivante montre comment lire un fichier avec une ligne d’en-tête, une nouvelle ligne de style Unix et des colonnes délimitées par des virgules. Notez l’emplacement différent du fichier par rapport aux autres exemples.

Aperçu du fichier :

![10 premières lignes du fichier CSV avec ligne d’en-tête et nouvelle ligne de style Unix.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="custom-quote-character"></a>Guillemet personnalisé

La requête suivante montre comment lire un fichier avec une ligne d’en-tête, une nouvelle ligne de style Unix, des colonnes délimitées par des virgules et des valeurs entre guillemets. Notez l’emplacement différent du fichier par rapport aux autres exemples.

Aperçu du fichier :

![10 premières lignes du fichier CSV avec ligne d’en-tête, nouvelle ligne de style Unix et valeurs entre guillemets.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-quoted/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        FIELDQUOTE = '"'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

> [!NOTE]
> Cette requête retourne les mêmes résultats si vous omettez le paramètre FIELDQUOTE, car la valeur par défaut de FIELDQUOTE est un guillemet double.

## <a name="escaping-characters"></a>Caractères d’échappement

La requête suivante montre comment lire un fichier avec une ligne d’en-tête, une nouvelle ligne de style Unix, des colonnes délimitées par des virgules et un caractère d’échappement utilisé pour le délimiteur de champ (virgule) à l’intérieur des valeurs. Notez l’emplacement différent du fichier par rapport aux autres exemples.

Aperçu du fichier :

![10 premières lignes du fichier CSV avec ligne d’en-tête, nouvelle ligne de style Unix et caractère d’échappement utilisé pour le délimiteur de champ.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-escape/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        ESCAPECHAR = '\\'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slov,enia';
```

> [!NOTE]
> Cette requête échouerait si ESCAPECHAR n’était pas spécifié, car la virgule dans « Slov,enia » serait traitée comme un délimiteur de champ plutôt que comme une partie du nom de pays. « Slov, enia » serait alors traité comme deux colonnes. Par conséquent, cette ligne aurait une colonne plus que les autres lignes et que ce que vous avez défini dans la clause WITH.

## <a name="tab-delimited-files"></a>Fichiers délimités par des tabulations

La requête suivante montre comment lire un fichier avec une ligne d’en-tête, une nouvelle ligne de style Unix et des colonnes délimitées par des tabulations. Notez l’emplacement différent du fichier par rapport aux autres exemples.

Aperçu du fichier :

![10 premières lignes du fichier CSV avec ligne d’en-tête, nouvelle ligne de style Unix et délimiteur tabulation.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-tsv/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR ='\t',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017
```

## <a name="returning-subset-of-columns"></a>Retour d’un sous-ensemble de colonnes

Jusqu’à présent, vous avez spécifié le schéma de fichier CSV en utilisant la clause WITH et en répertoriant toutes les colonnes. Vous pouvez spécifier uniquement les colonnes dont vous avez réellement besoin dans votre requête en utilisant un nombre ordinal pour chaque colonne nécessaire. Vous allez également omettre les colonnes qui ne vous intéressent pas.

La requête suivante retourne le nombre de noms de pays distincts dans un fichier, en spécifiant uniquement les colonnes nécessaires :

> [!NOTE]
> Jetez un coup d’œil à la clause WITH dans la requête ci-dessous, et notez la présence du chiffre « 2 » (sans guillemets) à la fin de la ligne dans laquelle vous définissez la colonne *[country_name]* . Cela signifie que la colonne *[country_name]* est la deuxième dans le fichier. La requête ignore toutes les colonnes du fichier, à l’exception de la deuxième.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>Étapes suivantes

Les articles suivants décrivent les opérations ci-dessous :

- [Interrogation des fichiers Parquet](query-parquet-files.md)
- [Interrogation de plusieurs dossiers et fichiers](query-folders-multiple-csv-files.md)
