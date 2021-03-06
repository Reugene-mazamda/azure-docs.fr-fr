---
title: 'Démarrage rapide : Clusters Apache Spark avec Azure CLI - Azure HDInsight'
description: Ce guide de démarrage rapide explique comment utiliser l'interface de ligne de commande Azure (Azure CLI) pour créer un cluster Apache Spark dans Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 02/03/2020
ms.openlocfilehash: e4679d5a04be7b8c0145fd93818e4187170b4194
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77049683"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-cli"></a>Démarrage rapide : créer un cluster Apache Spark dans Azure HDInsight à l’aide d’Azure CLI

Dans ce guide de démarrage rapide, vous allez apprendre à créer un cluster Apache Spark dans Azure HDInsight à l’aide de l’interface de ligne de commande (CLI) Azure. Azure HDInsight est un service d’analytique open source managé et complet pour les entreprises. Le framework Apache Spark pour HDInsight permet une analytique données et des calculs sur cluster rapides à l’aide du traitement en mémoire. Azure CLI est l’interface de ligne de commande multiplateforme de Microsoft pour la gestion de ressources Azure.

## <a name="prerequisites"></a>Prérequis

- Compte Azure avec un abonnement actif. [Créez un compte gratuitement](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) si vous ne souhaitez pas utiliser Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-an-apache-spark-cluster"></a>Créer un cluster Apache Spark

1. Connectez-vous à votre abonnement Azure. Si vous prévoyez d’utiliser Azure Cloud Shell, sélectionnez **Essayer** en haut à droite du bloc de code suivant. Sinon, entrez la commande suivante :

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Définir des variables d’environnement Dans ce guide de démarrage rapide, l'utilisation des variables repose sur Bash. De petites modifications seront nécessaires si vous utilisez d'autres environnements. Dans l'extrait de code ci-dessous, remplacez RESSOURCEGROUPNAME, LOCATION, CLUSTERNAME, STORAGEACCOUNTNAME et PASSWORD par les valeurs de votre choix. Entrez ensuite les commandes CLI pour définir les variables d'environnement.

    ```azurecli-interactive
    export resourceGroupName=RESOURCEGROUPNAME
    export location=LOCATION
    export clusterName=CLUSTERNAME
    export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
    export httpCredential='PASSWORD'
    export sshCredentials='PASSWORD'
    
    export AZURE_STORAGE_CONTAINER=$clusterName
    export clusterSizeInNodes=1
    export clusterVersion=3.6
    export clusterType=spark
    export componentVersion=Spark=2.3
    ```

3. Créez le groupe de ressources en entrant la commande ci-dessous :

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

4. Créez un compte de stockage Azure en entrant la commande ci-dessous :

    ```azurecli-interactive
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. Extrayez la clé primaire du compte de stockage Azure et stockez-la dans une variable en entrant la commande ci-dessous :

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. Créez un conteneur de stockage Azure en entrant la commande ci-dessous :

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. Créez le cluster Apache Spark en entrant la commande suivante :

    ```azurecli-interactive
    az hdinsight create \
        --name $clusterName \
        --resource-group $resourceGroupName \
        --type $clusterType \
        --component-version $componentVersion \
        --http-password $httpCredential \
        --http-user admin \
        --location $location \
        --workernode-count $clusterSizeInNodes \
        --ssh-password $sshCredentials \
        --ssh-user sshuser \
        --storage-account $AZURE_STORAGE_ACCOUNT \
        --storage-account-key $AZURE_STORAGE_KEY \
        --storage-container $AZURE_STORAGE_CONTAINER \
        --version $clusterVersion
    ```

## <a name="clean-up-resources"></a>Nettoyer les ressources

Après avoir suivi ce guide de démarrage rapide, vous souhaiterez peut-être supprimer le cluster. Avec HDInsight, vos données sont stockées dans le stockage Azure. Vous pouvez ainsi supprimer un cluster en toute sécurité s’il n’est pas en cours d’utilisation. Vous devez également payer pour un cluster HDInsight, même quand vous ne l’utilisez pas. Étant donné que les frais pour le cluster sont bien plus élevés que les frais de stockage, mieux vaut supprimer les clusters quand ils ne sont pas utilisés.

Entrez tout ou partie des commandes suivantes pour supprimer des ressources :

```azurecli-interactive
# Remove cluster
az hdinsight delete \
    --name $clusterName \
    --resource-group $resourceGroupName

# Remove storage container
az storage container delete \
    --account-name $AZURE_STORAGE_ACCOUNT \
    --name $AZURE_STORAGE_CONTAINER

# Remove storage account
az storage account delete \
    --name $AZURE_STORAGE_ACCOUNT \
    --resource-group $resourceGroupName

# Remove resource group
az group delete \
    --name $resourceGroupName
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez appris à créer un cluster Apache Spark dans Azure HDInsight à l'aide d'Azure CLI.  Passez au tutoriel suivant pour apprendre à utiliser un cluster HDInsight pour exécuter des requêtes interactives sur des exemples de données.

> [!div class="nextstepaction"]
> [Exécuter des requêtes interactives sur Apache Spark](./apache-spark-load-data-run-query.md)
