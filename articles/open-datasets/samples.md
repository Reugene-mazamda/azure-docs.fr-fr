---
title: Exemples de notebooks Jupyter utilisant des données NOAA
titleSuffix: Azure Open Datasets
description: Utilisez les exemples de notebooks Jupyter pour Azure Open Datasets afin de savoir comment charger des jeux de données ouverts et les utiliser pour enrichir les données de démonstration. Les techniques utilisent Spark et Pandas à des fins de traitement des données.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 11/04/2019
ms.openlocfilehash: 8b96a35db91a282be1fb5e4c6143e6bd0a0203f2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73606142"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Exemples de notebooks Jupyter montrant comment enrichir les données avec Open Datasets 
Les exemples de notebooks Jupyter pour Azure Open Datasets montrent comment charger des jeux de données ouverts et les utiliser pour enrichir les données de démonstration. Les techniques utilisent Apache Spark et Pandas à des fins de traitement des données.

>[!IMPORTANT]
>Dans un environnement autre que Spark, Azure Open Datasets permet de télécharger un seul mois de données à la fois, avec certaines classes, afin d’éviter l'erreur MemoryError liée aux jeux de données volumineux.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>Charger des données ISD (Integrated Surface Database) NOAA 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[Charger un mois récent de données météorologiques dans un dataframe Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Découvrez comment charger des données météorologiques historiques dans votre dataframe Pandas favori. |
|[Charger un mois récent de données météorologiques dans un dataframe Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Découvrez comment charger des données météorologiques historiques dans votre dataframe Spark favori.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Joindre des données de démonstration avec des données ISD NOAA 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[Joindre des données de démonstration avec des données météorologiques - Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Joignez un jeu de données de démonstration d'un mois d'emplacements de capteur avec relevés météorologiques dans un dataframe Pandas.  |
|[Joindre des données de démonstration avec des données météorologiques - Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Joignez un jeu de données de démonstration d'emplacements de capteur avec relevés météorologiques dans un dataframe Spark. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>Joindre des données de taxi de New York avec des données ISD NOAA 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[Données de trajet des taxis enrichies avec des données météorologiques - Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Chargez les données de trajet des taxis de New York (sur un mois) et enrichissez-les avec des données météorologiques dans un dataframe Pandas. Cet exemple remplace la méthode `get_pandas_limit` et équilibre les performances de chargement des données avec la quantité de données.|
|[Données de trajet des taxis enrichies avec des données météorologiques - Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | Chargez les données de trajet des taxis de New York et enrichissez-les avec des données météorologiques dans un dataframe Spark.  |

## <a name="next-steps"></a>Étapes suivantes

* [Tutoriel : Modélisation de régression avec Machine Learning automatisé et jeu de données ouvert](/azure/machine-learning/service/tutorial-auto-train-models?context=azure/open-datasets/context/open-datasets-context)
* [Kit de développement logiciel (SDK) Python pour Open Datasets](/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py)
* [Catalogue Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/catalog/)
