---
title: Connecteurs pour Azure Logic Apps
description: Automatiser les workflows avec des connecteurs pour Azure Logic Apps, tels que les connecteurs intégrés, managés et locaux, ou les connecteurs de compte d’intégration, ISE et d’entreprise
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/05/2020
ms.openlocfilehash: 3010f3c99a5b214c2503f890321cbb73427e3c20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225885"
---
# <a name="connectors-for-azure-logic-apps"></a>Connecteurs pour Azure Logic Apps

Les connecteurs offrent un accès rapide à partir d’Azure Logic Apps aux événements, aux données et aux actions de l’ensemble des autres applications, services, systèmes, protocoles et plateformes. En utilisant des connecteurs dans vos applications logiques, vous développez les fonctionnalités de vos applications cloud et locales pour exécuter des tâches avec les données que vous créez et dont vous disposez déjà.

Logic Apps propose [des centaines de connecteurs](https://docs.microsoft.com/connectors). Cet article aborde les connecteurs *les plus couramment utilisés* par des milliers d’applications et des millions d’exécutions pour le traitement des données et des informations. Pour rechercher la liste complète des connecteurs et les informations de référence sur chaque connecteur, telles que les déclencheurs, les actions et les limites, consultez les pages de référence de connecteur accessibles à partir de la [vue d’ensemble des connecteurs](https://docs.microsoft.com/connectors). En outre, apprenez-en davantage sur les [déclencheurs et actions](#triggers-actions), sur le [modèle de tarification de Logic Apps](../logic-apps/logic-apps-pricing.md) et sur les [détails de tarification de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!TIP]
> Pour assurer l’intégration à un service ou à une API ne disposant pas de connecteur, vous pouvez appeler directement le service par le biais d’un protocole tel que HTTP, ou créer un [connecteur personnalisé](#custom).

## <a name="connector-types"></a>Types de connecteurs

Les connecteurs sont disponibles sous forme de déclencheurs et d’actions intégrés ou de connecteurs managés.

<a name="built-in"></a>

* [**Intégrés**](#built-ins) : les déclencheurs et les actions intégrés sont « natifs » dans Azure Logic Apps. Ils vous aident à effectuer les tâches suivantes pour vos applications logiques :

  * Exécuter les applications selon des planifications personnalisées et avancées

  * Organiser et contrôler le workflow de votre application logique (par exemple, les boucles et les conditions), et utiliser des opérations de variables et de données

  * Communiquer avec d’autres points de terminaison

  * Recevoir des requêtes et y répondre

  * Appeler Azure Functions, Azure API Apps (Web Apps), vos propres API managées et publiées avec la Gestion des API Azure, ainsi que les applications logiques imbriquées qui peuvent recevoir des requêtes

<a name="managed-connectors"></a>

* [**Connecteurs managés**](#managed-api-connectors) : déployés et managés par Microsoft, ces connecteurs fournissent des déclencheurs et actions qui permettent d’accéder à des services cloud et/ou à des systèmes locaux, tels qu’Office 365, Stockage Blob Azure, SQL Server, Dynamics, Salesforce, SharePoint, etc. Certains connecteurs prennent spécifiquement en charge les scénarios de communication entreprise-entreprise (B2B) et nécessitent un [compte d’intégration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) lié à votre application logique. Avant d’utiliser certains connecteurs, vous devrez peut-être commencer par créer des connexions, qui sont managées par Azure Logic Apps.

  Par exemple, si vous utilisez Microsoft BizTalk Server, vos applications logiques peuvent se connecter à votre instance BizTalk Server et communiquer avec cette dernière à l’aide du [connecteur local BizTalk Server](#on-premises-connectors). Vous pouvez ensuite étendre ou exécuter des opérations de type BizTalk dans vos applications logiques à l’aide de [connecteurs de compte d’intégration](#integration-account-connectors).

  Les connecteurs sont classés en tant que connecteurs standard ou entreprise. Les [connecteurs entreprise](#enterprise-connectors) permettent d’accéder aux systèmes d’entreprise tels que SAP, IBM MQ et IBM 3270 moyennant un coût supplémentaire. Pour déterminer si un connecteur est de type standard ou entreprise, consultez les détails techniques sur la page de référence de chaque connecteur accessible à partir de la [vue d’ensemble des connecteurs](https://docs.microsoft.com/connectors).

  Vous pouvez également identifier les connecteurs à l’aide des catégories ci-dessous, même si certains connecteurs peuvent appartenir à plusieurs catégories. Par exemple, SAP est à la fois un connecteur entreprise et un connecteur local :

  |   |   |
  |---|---|
  | [**Connecteurs managés**](#managed-api-connectors) | Créent des applications logiques qui utilisent des services tels que Stockage Blob Azure, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online et bien d’autres encore. |
  | [**Connecteurs locaux**](#on-premises-connectors) | Une fois la [passerelle de données locale][gateway-doc] installée et configurée, ces connecteurs permettent à vos applications logiques d’accéder aux systèmes locaux comme SQL Server, SharePoint Server, Oracle DB, les partages de fichiers et autres. |
  | [**Connecteurs de compte d’intégration**](#integration-account-connectors) | Disponibles lorsque vous créez et payez un compte d’intégration, ces connecteurs transforment et valident le code XML, encodent et décodent les fichiers plats et traitent les messages entreprise-entreprise (B2B) avec les protocoles AS2, EDIFACT et X12. |
  |||

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment"></a>Se connecter à partir d’un environnement de service d’intégration

Pour les applications logiques qui ont besoin d’un accès direct aux ressources d’un réseau virtuel Azure, vous pouvez créer un [environnement d’intégration de service (ISE) ](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) isolé dans lequel vous pouvez générer, déployer et exécuter vos applications logiques sur des ressources dédiées. Dans le concepteur d’applications logiques, lorsque vous parcourez les connecteurs que vous souhaitez utiliser pour Logic Apps dans un environnement ISE, une étiquette **CORE** s’affiche en regard des déclencheurs et des actions intégrés, et l’étiquette **ISE** s’affiche en regard de certains connecteurs :

* **CORE** : les déclencheurs et les actions intégrés qui sont signalés par cette étiquette s’exécutent dans le même environnement ISE que vos applications logiques, par exemple :

  ![Exemple de connecteur ISE](./media/apis-list/example-core-connector.png)

* **ISE** : les connecteurs managés signalés par cette étiquette sont exécutés dans le même environnement ISE que vos applications logiques, par exemple :

  ![Exemple de connecteur ISE](./media/apis-list/example-ise-connector.png)

  Si vous disposez d’un système local connecté à un réseau virtuel Azure, l’environnement ISE permet à vos applications logiques d’accéder directement à ce système sans passer par la [passerelle de données locale](../logic-apps/logic-apps-gateway-connection.md). Au lieu de cela, vous pouvez utiliser le connecteur **ISE** du système s’il est disponible, une action HTTP ou un [connecteur personnalisé](#custom). Pour les systèmes locaux qui n’ont pas de connecteurs **ISE**, utilisez la passerelle de données locale. Pour consulter les connecteurs ISE disponibles, consultez [Connecteurs ISE](#ise-connectors).

* Les connecteurs qui ne sont signalés ni par l’étiquette **CORE** ni par l’étiquette **ISE** peuvent toujours être utilisés, et sont exécutés dans le service global et multilocataire de Logic Apps. Par exemple :

  ![Exemple de connecteur multilocataire](./media/apis-list/example-multi-tenant-connector.png)

Les applications logiques qui s’exécutent dans un environnement ISE, ainsi que leurs connecteurs, quel que soit l’endroit où ceux-ci s’exécutent, suivent un plan tarifaire fixe et non un plan tarifaire basé sur la consommation. Pour plus d’informations, consultez les pages suivantes :

* [Modèle de tarification de Logic Apps](../logic-apps/logic-apps-pricing.md)
* [Informations sur les tarifs Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Se connecter à des réseaux virtuels Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="built-ins"></a>

## <a name="built-in"></a>Intégré

Logic Apps fournit des déclencheurs et des actions intégrés qui vous permettent de créer des workflows basés sur une planification, d’aider vos applications logiques à communiquer avec d’autres applications et services, de contrôler le workflow via vos applications logiques, et de gérer ou de manipuler des données.

|   |   |   |   |
|---|---|---|---|
| [![Icône d’API][schedule-icon]<br>**Planification**][schedule-doc] | - Exécutez une application logique selon une récurrence spécifiée, allant des planifications de base aux planifications avancées, à l’aide du [déclencheur **Récurrence**][schedule-recurrence-doc]. <p>- Exécutez une application logique qui doit gérer les données en blocs contigus à l’aide du [déclencheur **Fenêtre glissante**][schedule-sliding-window-doc]. <p>- Suspendez votre application logique pour une durée spécifiée avec l’[action **Retarder**][schedule-delay-doc]. <p>- Suspendez votre application logique jusqu’à une date et une heure spécifiées avec l’[action **Retarder jusqu’à**][schedule-delay-until-doc]. | [![Icône d’API][batch-icon]<br>**Lot**][batch-doc] | - Traitez les messages par lot avec le déclencheur **Batch messages** (Messages par lot). <p>- Appelez les applications logiques comportant déjà des déclencheurs par lot avec l’action **Send messages to batch** (Envoyer les messages au lot). |
| [![Icône d’API][http-icon]<br>**HTTP**][http-doc] | Appelez des points de terminaison HTTP ou HTTPS avec des déclencheurs et des actions pour HTTP. Les autres déclencheurs et actions HTTP intégrés incluent notamment [HTTP + Swagger][http-swagger-doc] et [HTTP + Webhook][http-webhook-doc]. | [![Icône d’API][http-request-icon]<br>**Requête**][http-request-doc] | - Appelez votre application logique à partir d’autres applications ou services, déclenchez lors des événements liés aux ressources Event Grid ou déclenchez lors des réponses aux alertes Azure Security Center avec le déclencheur **Requête**. <p>- Envoyez des réponses à une application ou un service avec l’action **Réponse**. |
| [![Icône d’API][azure-api-management-icon]<br> **<br>Gestion** des API Azure][azure-api-management-doc] | Appelez les déclencheurs et les actions définis par vos propres API, gérées et publiées avec Gestion des API Azure. | [![Icône d’API][azure-app-services-icon]<br>**Azure App <br>Services**][azure-app-services-doc] | Appelez Azure API Apps ou Web Apps, hébergées sur Azure App Service. Les déclencheurs et les actions définies par ces applications apparaissent comme les autres déclencheurs et actions de première classe lorsque Swagger est inclus.|
| [![Icône d’API][azure-logic-apps-icon]<br>**Azure Logic <br>Apps**][nested-logic-app-doc] | Appelez d’autres applications logiques qui commencent par le déclencheur **Requête**. |
|||||

### <a name="run-code-from-logic-apps"></a>Exécuter du code à partir d’applications logiques

Logic Apps fournit des actions intégrées permettant d’exécuter votre propre code dans le workflow de votre application logique :

|   |   |   |   |
|---|---|---|---|
| [![Icône d’API][azure-functions-icon]<br>**Azure Functions**][azure-functions-doc] | Appelez les fonctions Azure qui exécutent des extraits de code personnalisés (C# ou Node.js) à partir de vos applications logiques. | [![Icône d’API][inline-code-icon]<br>**Code inline**][azure-functions-doc] | Ajoutez et exécutez des extraits de code JavaScript à partir de vos applications logiques. |
|||||

### <a name="control-workflow"></a>Contrôler le flux de travail

Logic Apps fournit des actions intégrées qui permettent de structurer et de contrôler les actions du flux de travail de votre application logique :

|   |   |   |   |
|---|---|---|---|
| [![Icône d’élément intégré][condition-icon]<br>**Condition**][condition-doc] | Évaluez une condition et exécutez différentes actions selon que la condition est true ou false. | [![Icône d’élément intégré][for-each-icon]<br>**Boucle Foreach**][for-each-doc] | Effectuez les mêmes actions sur chaque élément dans un tableau. |
| [![Icône d’élément intégré][scope-icon]<br>**Étendue**][scope-doc] | Regroupez les actions en *étendues*, qui obtiennent leur propre état à la fin de l’exécution des actions dans l’étendue. | [![Icône d’élément intégré][switch-icon]<br>**Commutateur**][switch-doc] | Regroupez les actions en *cas*, auxquels sont affectées des valeurs uniques à l’exception du cas par défaut. Exécutez uniquement le cas dont la valeur affectée correspond au résultat d’une expression, d’un objet ou d’un jeton. Si aucune correspondance n’existe, exécutez le cas par défaut. |
| [![Icône d’élément intégré][terminate-icon]<br>**Terminate**][terminate-doc] | Arrêtez un flux de travail d’application logique qui fonctionne activement. | [![Icône d’élément intégré][until-icon]<br>**Until**][until-doc] | Répétez les actions jusqu’à ce que la condition spécifiée ait la valeur true ou qu’un état ait changé. |
|||||

### <a name="manage-or-manipulate-data"></a>Gérer ou manipuler des données

Logic Apps fournit des actions intégrées qui permettent de manipuler les sorties de données et leur format :

|   |   |
|---|---|
| [![Icône intégrée][data-operations-icon]<br>**Opérations avec les données**][data-operations-doc] | Effectuez des opérations avec les données : <p>- **Composer** : créez une sortie unique à partir de plusieurs entrées avec différents types. <br>- **Créer un tableau CSV** : créez un tableau CSV (valeurs séparées par des virgules) à partir d’un tableau avec des objets JSON. <br>- **Créer un tableau HTML** : créez un tableau HTML à partir d’un tableau avec des objets JSON. <br>- **Filtrer un tableau** : créez un tableau à partir des éléments d’un autre tableau qui correspondent à vos critères. <br>- **Joindre** : créez une chaîne à partir de tous les éléments d’un tableau et séparez ces éléments à l’aide du séparateur spécifié. <br>- **Analyser JSON** : Créez des jetons conviviaux à partir des propriétés et de leurs valeurs dans le contenu JSON afin de pouvoir utiliser ces propriétés dans votre workflow. <br>- **Sélectionner** : créez un tableau avec des objets JSON en transformant les éléments ou les valeurs d’un autre tableau et en mappant ces éléments sur les propriétés spécifiées. |
| ![Icône d’élément intégré][date-time-icon]<br>**Date Heure** | Effectuez des opérations avec les horodatages : <p>- **Ajouter à l’heure** : ajoutez le nombre spécifié d’unités à un timestamp. <br>- **Convertir le fuseau horaire** : Convertit un horodatage du fuseau horaire source au fuseau horaire cible. <br>- **Heure actuelle** : Renvoyer le timestamp actuel sous forme de chaîne. <br>- **Obtenir l’heure future** : Retourne l’horodatage actuel plus les unités de temps spécifiées. <br>- **Obtenir l’heure passée** : Retourne l’horodatage actuel moins les unités de temps spécifiées. <br>- **Soustraire de l’heure** : Soustrait un nombre d’unités de temps d’un horodatage. |
| [![Icône d’élément intégré][variables-icon]<br>**Variables**][variables-doc] | Effectuez des opérations avec les variables : <p>- **Ajouter à la variable de tableau** : insérez une valeur en tant que dernier élément dans un tableau stocké par une variable. <br>- **Ajouter à la variable de chaîne** : insérez une valeur en tant que dernier caractère dans une chaîne stockée par une variable. <br>- **Décrémenter une variable** : diminuez une variable d’une valeur constante. <br>- **Incrémenter une variable** : augmentez une variable d’une valeur constante. <br>- **Initialiser la variable** : créez une variable et déclarez son type de données et sa valeur initiale. <br>- **Définir une variable** : attribuez une autre valeur à une variable existante. |
|  |  |

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Connecteurs gérés

Logic Apps fournit les connecteurs standard ci-dessous qui sont les plus couramment utilisés pour automatiser les tâches, les processus et les workflows avec ces services ou systèmes :

|   |   |   |   |
|---|---|---|---|
| [![Icône d’API][azure-service-bus-icon]<br>**Azure Service Bus**][azure-service-bus-doc] | Gérez les messages asynchrones, les sessions et les abonnements à une rubrique avec le connecteur le plus couramment utilisé dans Logic Apps. | [![Icône d’API][sql-server-icon]<br>**SQL Server**][sql-server-doc] | Connectez-vous à votre serveur SQL local ou à Azure SQL Database dans le cloud pour gérer les enregistrements, exécuter des procédures stockées ou exécuter des requêtes. |
| [![Icône d’API][azure-blob-storage-icon]<br>**Stockage Blob<br>Azure**][azure-blob-storage-doc] | Connectez-vous à votre compte de stockage pour créer et gérer du contenu d’objet blob. | [![Icône d’API][office-365-outlook-icon]<br>**Office 365<br>Outlook**][office-365-outlook-doc] | Connectez-vous à votre compte de messagerie Office 365 pour créer et gérer des e-mails, des tâches, des événements de calendrier, des réunions, des contacts, des requêtes et bien plus encore. |
| [![Icône d’API][sftp-ssh-icon]<br>**SFTP-SSH**][sftp-ssh-doc] | Connectez-vous aux serveurs SFTP auxquels vous avez accès à partir d’Internet via une connexion SSH afin de pouvoir utiliser vos fichiers et vos dossiers. | [![Icône d’API][sharepoint-online-icon]<br>**SharePoint<br>Online**][sharepoint-online-doc] | Connectez-vous à SharePoint Online pour gérer des fichiers, des pièces jointes, des dossiers et bien plus encore. | 
| [![Icône d’API][dynamics-365-icon]<br>**Dynamics 365<br>** ][dynamics-365-doc] | Connectez-vous à votre compte Dynamics 365 pour créer et gérer des enregistrements, des éléments et bien plus encore. | [![Icône d’API][azure-queues-icon]<br> **<br>Files d’attente** Azure][azure-queues-doc] | Connectez-vous à votre compte de stockage Azure afin de créer et de gérer des files d’attente et des messages. |
| [![Icône d’API][ftp-icon]<br>**FTP**][ftp-doc] | Connectez-vous aux serveurs FTP auxquels vous avez accès à partir d’Internet pour utiliser vos fichiers et vos dossiers. | [![Icône d’API][file-system-icon]<br> **<br>Système** de fichiers][file-system-doc] | Connectez-vous à votre partage de fichiers local afin de créer et de gérer des fichiers. |
| [![Icône d’API][azure-event-hubs-icon]<br>**Event Hubs**][azure-event-hubs-doc] | Consommez et publiez des événements via un Event Hub. Par exemple, obtenez une sortie à partir de votre application logique à l’aide des Event Hubs, puis envoyez-la à un fournisseur d’analyses en temps réel. | [![Icône d’API][azure-event-grid-icon]<br>**Azure Event**<br>**Grid**][azure-event-grid-doc] | Surveillez les événements publiés par une grille d’événements, par exemple, lorsque les ressources Azure ou les ressources tierces changent. |
| [![Icône d’API][salesforce-icon]<br>**Salesforce**][salesforce-doc] | Connectez-vous à votre compte Salesforce pour créer et gérer des éléments tels que des enregistrements, des travaux, des objets et bien plus encore. | [![Icône d’API][twitter-icon]<br>**Twitter**][twitter-doc] | Connectez-vous à votre compte Twitter pour gérer vos tweets, vos abonnés, votre timeline et bien plus encore. Enregistrez vos tweets dans SQL, Excel ou SharePoint. |
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Connecteurs locaux

Les connecteurs standard ci-dessous fournis par Logic Apps sont couramment utilisés pour offrir un accès aux données et aux ressources des systèmes locaux. Avant de créer une connexion à un système local, vous devez d’abord [télécharger, installer et configurer une passerelle de données locale][gateway-doc]. Cette passerelle fournit un canal de communication sécurisé sans avoir à configurer l’infrastructure réseau nécessaire.

|   |   |   |   |   |
|---|---|---|---|---|
| [![Icône d’API][biztalk-server-icon]<br>**BizTalk** <br>**Server**][biztalk-server-doc] | [![Icône d’API][file-system-icon]<br> **<br>Système** de fichiers][file-system-doc] | [![Icône d’API][ibm-db2-icon]<br>**IBM DB2**][ibm-db2-doc] | [![Icône d’API][ibm-informix-icon]<br>**IBM** <br>**Informix**][ibm-informix-doc] | [![Icône d’API][mysql-icon]<br>**MySQL**][mysql-doc] |
| [![Icône d’API][oracle-db-icon]<br>**Oracle DB**][oracle-db-doc] | [![Icône d’API][postgre-sql-icon]<br>**PostgreSQL**][postgre-sql-doc] | [![Icône d’API][sharepoint-server-icon]<br>**SharePoint <br>Server**][sharepoint-server-doc] | [![Icône d’API][sql-server-icon]<br>**SQL <br>Server**][sql-server-doc] | [![Icône d’API][teradata-icon]<br>**Teradata**][teradata-doc] |
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Connecteurs de compte d’intégration

Logic Apps fournit les connecteurs standard ci-dessous pour la création de solutions entreprise-entreprise (B2B) avec vos applications logiques lorsque vous créez et payez un [compte d’intégration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), qui est disponible par le biais d’Enterprise Integration Pack (EIP) dans Azure. Avec ce compte, vous pouvez créer et stocker des artefacts B2B tels que les partenaires commerciaux, les contrats, les mappages, les schémas, les certificats et ainsi de suite. Pour utiliser ces artefacts, associez vos applications logiques avec votre compte d’intégration. Si vous utilisez déjà BizTalk Server, ces connecteurs vous sont peut-être familiers.

|   |   |   |   |
|---|---|---|---|
| [![Icône d’API][as2-icon]<br> **<br>Décodage** AS2][as2-doc] | [![Icône d’API][as2-icon]<br> **<br>Codage** AS2][as2-doc] | [![Icône d’API][edifact-icon]<br> **<br>Décodage** EDIFACT][edifact-decode-doc] | [![Icône d’API][edifact-icon]<br> **<br>Codage** EDIFACT][edifact-encode-doc] |
| [![Icône d’API][flat-file-decode-icon]<br> **<br>Décodage** de fichiers plats][flat-file-decode-doc] | [![Icône d’API][flat-file-encode-icon]<br> **<br>Codage** de fichiers plats][flat-file-encode-doc] | [![Icône d’API][integration-account-icon]<br> **<br>Compte** d’intégration][integration-account-doc] | [![Icône d’API][liquid-icon]<br>**Transformations** <br>**Liquid**][json-liquid-transform-doc] |
| [![Icône d’API][x12-icon]<br>**Décodage<br> X12**][x12-decode-doc] | [![Icône d’API][x12-icon]<br>**Codage<br> X12**][x12-encode-doc] | [![Icône d’API][xml-transform-icon]<br>**Transformations** <br>**XML**][xml-transform-doc] | [![Icône d’API][xml-validate-icon]<br>**Validation <br>XML**][xml-validate-doc] |  
|||||

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Connecteurs d’entreprise

Logic Apps fournit les connecteur entreprise ci-dessous pour l’accès aux système d’entreprise, tels que SAP et IBM MQ :

|   |   |   |
|---|---|---|
| [![Icône d’API][ibm-3270-icon]<br>**IBM 3270**][ibm-3270-doc] | [![Icône d’API][ibm-mq-icon]<br>**IBM MQ**][ibm-mq-doc] | [![Icône d’API][sap-icon]<br>**SAP**][sap-connector-doc] |
||||

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>Connecteurs ISE

Pour les applications logiques que vous créez et exécutez dans un [environnement de service d’intégration (ISE)](#integration-service-environment) isolé, le concepteur d’applications logiques identifie les déclencheurs et les actions intégrés qui s’exécutent dans votre environnement ISE à l’aide de l’étiquette **CORE**. Les connecteurs managés qui s’exécutent dans un environnement ISE sont signalés par l’étiquette **ISE**. Les connecteurs qui s’exécutent dans le service global multilocataire Logic Apps ne sont signalés par aucune étiquette. Cette liste montre les connecteurs qui ont une version ISE :

|   |   |   |   |   |
|---|---|---|---|---|
[![Icône d’API][as2-icon]<br>**AS2**][as2-doc] | [![Icône d’API][azure-blob-storage-icon]<br>**Stockage Blob<br>Azure**][azure-blob-storage-doc] | [![Icône d’API][azure-cosmos-db-icon]<br>**Azure Cosmos <br> DB**][azure-cosmos-db-doc] | [![Icône d’API][azure-event-hubs-icon]<br>**Azure Event <br>Hubs**][azure-event-hubs-doc] | [![Icône d’API][azure-file-storage-icon]<br>**Stockage Fichier <br>Azure**][azure-file-storage-doc] |
| [![Icône d’API][azure-service-bus-icon]<br>**Azure Service <br>Bus**][azure-service-bus-doc] | [![Icône d’API][azure-sql-data-warehouse-icon]<br>**Azure SQL Data <br>Warehouse**][azure-sql-data-warehouse-doc] | [![API icon][azure-table-storage-icon]<br>**Stockage Table <br>Azure**][azure-table-storage-doc] | [![Icône d’API][azure-queues-icon]<br> **<br>Files d’attente** Azure][azure-queues-doc] | [![Icône d’API][edifact-icon]<br>**EDIFACT**][edifact-doc] |
| [![Icône d’API][file-system-icon]<br> **<br>Système** de fichiers][file-system-doc] | [![Icône d’API][ftp-icon]<br>**FTP**][ftp-doc] | [![Icône d’API][ibm-3270-icon]<br>**IBM 3270**][ibm-3270-doc] | [![Icône d’API][ibm-db2-icon]<br>**IBM DB2**][ibm-db2-doc] | [![Icône d’API][ibm-mq-icon]<br>**IBM MQ**][ibm-mq-doc] |
| [![Icône d’API][sap-icon]<br>**SAP**][sap-connector-doc] | [![Icône d’API][sftp-ssh-icon]<br>**SFTP-SSH**][sftp-ssh-doc] | [![Icône d’API][smtp-icon]<br>**SMTP**][smtp-doc] | [![Icône d’API][sql-server-icon]<br>**SQL<br> Server**][sql-server-doc] | [![Icône d’API][x12-icon]<br>**X12**][x12-doc] |
||||||

Pour plus d’informations, consultez les rubriques suivantes :

* [Accéder à des ressources de réseau virtuel Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Modèle de tarification de Logic Apps](../logic-apps/logic-apps-pricing.md)
* [Se connecter à des réseaux virtuels Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Types de déclencheurs et d’actions

Les connecteurs peuvent fournir des *déclencheurs* et/ou des *actions*. Un *déclencheur* représente la première étape de toute application logique, puisqu’il spécifie généralement l’événement qui l’active et commence à exécuter votre application logique. Par exemple, le connecteur FTP comporte un déclencheur qui démarre votre application logique « lorsqu’un fichier est ajouté ou modifié ». Certains déclencheurs recherchent régulièrement l’événement ou les données spécifiées, puis s’activent lorsqu’ils détectent ces derniers. D’autres déclencheurs attendent, mais se déclenchent instantanément quand un événement spécifique survient ou que de nouvelles données sont disponibles. Les déclencheurs transmettent également les données requises à votre application logique. Votre application logique peut lire et utiliser ces données tout au long du flux de travail. Par exemple, le connecteur Twitter comporte un déclencheur « Lors de la publication d’un nouveau tweet », qui transmet le contenu du tweet au flux de travail de votre application logique.

Après l’activation d’un déclencheur, Azure Logic Apps crée une instance de votre application logique et commence à exécuter les *actions* dans le flux de travail de votre application logique. Les actions désignent les étapes qui succèdent au déclencheur et exécutent des tâches dans le flux de travail de votre application logique. Par exemple, vous pouvez créer une application logique qui obtient des données client à partir d’une base de données SQL, puis traiter ces données à l’aide d’actions ultérieures.

Voici les principaux types de déclencheurs fournis par Azure Logic Apps :

* *Déclencheur de périodicité* : ce déclencheur s’exécute selon une planification spécifiée et n’est pas étroitement associé à un service ou système particulier.

* *Déclencheur d’interrogation* : ce déclencheur interroge régulièrement un service ou système spécifique sur la base de la planification spécifiée, pour y rechercher de nouvelles données ou vérifier si un événement précis s’est produit. Si de nouvelles données sont disponibles ou que l’événement spécifié s’est produit, le déclencheur crée et exécute une nouvelle instance de votre application logique, qui peut alors utiliser les données transmises sous forme d’entrée.

* *Déclencheur d’émission* : ce déclencheur attend et écoute l’apparition de nouvelles données ou la survenue d’un événement. Si de nouvelles données sont disponibles ou que l’événement se produit, le déclencheur crée et exécute une nouvelle instance de votre application logique, qui peut alors utiliser les données transmises sous forme d’entrée.

<a name="connections"></a>

## <a name="connector-configuration"></a>Configuration des connecteurs

Les déclencheurs et actions de chaque connecteur disposent de leurs propres propriétés, que vous devez configurer. Dans le cas de nombreux connecteurs, vous devez également commencer par créer une *connexion* au service ou système cible et fournir des informations d’identification d’authentification ou d’autres détails de configuration pour être en mesure d’utiliser un déclencheur ou une action dans votre application logique. Par exemple, vous devez autoriser une connexion à un compte Twitter pour accéder aux données ou publier des messages en votre nom.

Dans le cas des connecteurs qui utilisent l’authentification OAuth Azure AD (Azure Active Directory), créer une connexion signifie se connecter au service (tel qu’Office 365, Salesforce ou GitHub), où votre jeton d’accès est [chiffré](../security/fundamentals/encryption-overview.md) et stocké de manière sécurisée dans un magasin de secrets Azure. D’autres connecteurs, comme FTP et SQL, nécessitent une connexion comprenant des détails de configuration, tels que l’adresse du serveur, le nom d’utilisateur et le mot de passe. Ces informations de configuration de connexion sont également chiffrées et stockées de manière sécurisée. Apprenez-en davantage sur le [chiffrement dans Azure](../security/fundamentals/encryption-overview.md).

Les connexions peuvent accéder au service ou système cible aussi longtemps que ce dernier l’autorise. Pour les services qui utilisent des connexions OAuth Azure AD, tels qu’Office 365 et Dynamics, Azure Logic Apps actualise les jetons d’accès indéfiniment. D’autres services peuvent imposer des limites concernant la durée pendant laquelle Azure Logic Apps peut utiliser un jeton sans actualisation. En règle générale, certaines actions, telles que la modification de votre mot de passe, invalident tous les jetons d’accès.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Connecteurs et API personnalisés

Pour appeler des API qui exécutent du code personnalisé ou qui ne sont pas disponibles en tant que connecteurs, vous pouvez étendre la plateforme Logic Apps [en créant des applications API personnalisées](../logic-apps/logic-apps-create-api-app.md). Vous pouvez également [créer des connecteurs personnalisés](../logic-apps/custom-connector-overview.md) pour *n’importe quelle* API REST ou SOAP, ce qui rend ces API disponibles pour n’importe quelle application logique dans votre abonnement Azure. Pour rendre les applications API ou les connecteurs personnalisés publics afin que tout le monde puisse les utiliser dans Azure, vous pouvez [soumettre des connecteurs à la certification Microsoft](../logic-apps/custom-connector-submit-certification.md).

> [!NOTE]
> Les applications logiques que vous déployez et exécutez dans un [environnement de service d’intégration (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) peuvent accéder directement aux ressources d’un réseau virtuel Azure. Si vous disposez de connecteurs personnalisés qui ont besoin de la passerelle de données locale et que vous avez créé ces connecteurs hors d’un ISE, les applications logiques d’un ISE peuvent également utiliser ces connecteurs.
>
> Les connecteurs personnalisés créés au sein d’un ISE ne fonctionnent pas avec la passerelle de données locale. Toutefois, ces connecteurs peuvent accéder directement aux sources de données locales qui sont connectées à un réseau virtuel Azure hébergeant l’ISE. Par conséquent, les applications logiques d’un ISE n’ont généralement pas besoin de la passerelle de données lorsqu’elles communiquent avec ces ressources.
>
> Pour plus d’informations sur la création d’environnements ISE, consultez l’article [Se connecter à des réseaux virtuels Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

## <a name="next-steps"></a>Étapes suivantes

* Afficher la [liste complète des connecteurs](https://docs.microsoft.com/connectors)
* [Créez votre première application logique](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Créer des connecteurs personnalisés pour les applications logiques](https://docs.microsoft.com/connectors/custom-connectors/)
* [Créer des API personnalisées pour les applications logiques](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Connexion à des sources de données locales à partir d’applications logiques avec la passerelle de données locale"

<!--Built-in doc links-->
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Créer une instance du service Gestion des API Azure pour gérer et publier vos API"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Permet d’intégrer des applications logiques à App Service API Apps"
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Permet d’intégrer des applications logiques à Azure Functions"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Traiter les messages en groupes ou sous forme de lots"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Évaluer une condition et exécuter différentes actions selon que la condition est true ou false"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Effectuer les mêmes actions sur chaque élément dans un tableau"
[http-doc]: ./connectors-native-http.md "Appeler des points de terminaison HTTP ou HTTPS à partir de vos applications logiques"
[http-request-doc]: ./connectors-native-reqres.md "Recevoir des requêtes HTTP dans vos applications logiques"
[http-response-doc]: ./connectors-native-reqres.md "Répondre aux requêtes HTTP à partir de vos applications logiques"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Appeler des points de terminaison REST à partir de vos applications logiques"
[http-webhook-doc]: ./connectors-native-webhook.md "Attendre des événements à partir de points de terminaison HTTP ou HTTPS"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Intégrer des applications logiques à des flux de travail imbriqués"
[query-doc]: ../logic-apps/logic-apps-perform-data-operations.md#filter-array-action "Sélectionner et filtrer des tableaux avec l’action de requête"
[schedule-doc]: ../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md "Exécuter des applications logiques selon une planification"
[schedule-delay-doc]: ./connectors-native-delay.md "Retarder l’exécution de l’action suivante"
[schedule-delay-until-doc]: ./connectors-native-delay.md "Retarder l’exécution de l’action suivante"
[schedule-recurrence-doc]:  ./connectors-native-recurrence.md "Exécuter des applications logiques selon une planification périodique"
[schedule-sliding-window-doc]: ./connectors-native-sliding-window.md "Exécuter des applications logiques qui doivent gérer des données en blocs contigus"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Organiser les actions en groupes, qui obtiennent leur propre état à la fin de l’exécution des actions dans le groupe"
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organiser les actions en cas, auxquels sont affectées des valeurs uniques. Exécuter uniquement le cas dont la valeur correspond au résultat d’une expression, d’un objet ou d’un jeton. Si aucune correspondance n’existe, exécuter le cas par défaut"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Arrêter ou annuler un flux de travail qu fonctionne activement pour votre application logique"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Répéter les actions jusqu’à ce que la condition spécifiée ait la valeur true ou qu’un état ait changé"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Effectuer des opérations avec les données telles que le filtrage de tableaux ou la création de tableaux CSV et HTML"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Effectuer des opérations avec des variables, comme initialiser, définir, incrémenter, décrémenter et ajouter à une variable de chaîne ou de tableau"

<!--Managed connector doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Gérer les fichiers de votre conteneur d’objets blob avec le connecteur Azure Blob Storage"
[azure-cosmos-db-doc]: https://docs.microsoft.com/connectors/documentdb/ "Se connecter à Azure Cosmos DB pour accéder à des documents et à des procédures stockées"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md " Superviser les événements publiés par Event Grid, par exemple, lorsque les ressources Azure ou les ressources tierces changent"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Se connecter à Azure Event Hubs pour l’envoi et la réception d’événements entre vos applications logiques et Event Hubs"
[azure-file-storage-doc]: https://docs.microsoft.com/connectors/azurefile/ "Se connecter à votre compte de stockage Azure pour créer, mettre à jour, récupérer et supprimer des fichiers"
[azure-queues-doc]: https://docs.microsoft.com/connectors/azurequeues/ "Se connecter à votre compte de stockage Azure pour créer et gérer des files d’attente et des messages"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Envoyer des messages à partir de files d’attente et de rubriques Service Bus, et recevoir des messages de files d’attente et d’abonnements ServiceBus"
[azure-sql-data-warehouse-doc]: https://docs.microsoft.com/connectors/sqldw/ "Se connecter à Azure SQL Data Warehouse pour voir vos données"
[azure-table-storage-doc]: https://docs.microsoft.com/connectors/azuretables/ "Se connecter à votre compte de stockage Azure pour créer, mettre à jour et interroger des tables"
[biztalk-server-doc]: https://docs.microsoft.com/connectors/biztalk/ "Se connecter à BizTalk Server pour exécuter des applications BizTalk en même temps qu’Azure Logic Apps"
[box-doc]: ./connectors-create-api-box.md "Se connecter à Box. Télécharger, obtenir, supprimer, répertorier vos fichiers et bien plus encore"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Se connecter à Dropbox. Télécharger, obtenir, supprimer, répertorier vos fichiers et bien plus encore"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Se connecter à Dynamics CRM Online pour utiliser les données CRM Online"
[facebook-doc]: ./connectors-create-api-facebook.md "Se connecter à Facebook. Publier sur une timeline, obtenir un flux de page et bien plus encore."
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Se connecter à un système de fichiers local"
[ftp-doc]: ./connectors-create-api-ftp.md "Se connecter à un serveur FTP/FTPS pour les tâches FTP (notamment télécharger, obtenir et supprimer des fichiers)."
[github-doc]: ./connectors-create-api-github.md "Se connecter à GitHub et suivre les problèmes"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Se connecter à Google Agenda et gérer un agenda"
[google-drive-doc]: ./connectors-create-api-googledrive.md "Se connecter à Google Drive pour utiliser vos données"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Se connecter à Google Sheets pour modifier vos feuilles"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Se connecter à Google Tasks pour gérer vos tâches"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Se connecter à des applications 3270 sur des mainframes IBM"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Se connecter à IBM DB2 dans le nuage ou sur site. Mettre à jour une ligne, obtenir une table et bien plus encore"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Se connecter à Informix dans le nuage ou sur site. Lire une ligne, répertorier les tables et bien plus encore"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Se connecter à IBM MQ en local ou dans Azure pour envoyer et recevoir des messages"
[instagram-doc]: ./connectors-create-api-instagram.md "Se connecter à Instagram. Déclencher ou agir sur les événements"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Se connecter à votre compte MailChimp. Gérer et automatiser les courriers électroniques"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Se connecter à Mandrill pour la communication"
[mysql-doc]: https://docs.microsoft.com/connectors/mysql/ "Se connecter à votre base de données MySQL locale pour lire et écrire des données"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Se connecter à votre compte Office 365 pour envoyer et recevoir des e-mails, gérer votre calendrier et vos contacts, etc."
[office-365-users-doc]: ./connectors-create-api-office365-users.md
[onedrive-doc]: ./connectors-create-api-onedrive.md "Se connecter à votre Microsoft OneDrive personnel pour charger, supprimer, lister des fichiers, etc."
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Se connecter à votre Microsoft OneDrive professionnel pour charger, supprimer, lister des fichiers, etc."
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Se connecter à une base de données Oracle pour ajouter, insérer, supprimer des lignes, etc."
[outlook.com-doc]: ./connectors-create-api-outlook.md "Se connecter à votre boîte aux lettres Outlook pour gérer vos e-mails, vos calendriers, vos contacts, etc."
[postgre-sql-doc]: https://docs.microsoft.com/connectors/postgresql/ "Se connecter à votre base de données PostgreSQL pour lire des données à partir des tables"
[project-online-doc]: ./connectors-create-api-projectonline.md "Se connecter à Microsoft Project Online pour gérer vos projets, vos tâches, vos ressources, etc."
[rss-doc]: ./connectors-create-api-rss.md "Publier et récupérer des éléments de flux, déclencher des opérations lorsqu’un nouvel élément est publié sur un flux RSS"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Se connecter à votre compte Salesforce. Gérer les comptes, les prospects, les opportunités et bien plus encore"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Se connecter à un système SAP local"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Se connecter à SendGrid. Envoyer un courrier électronique et gérer les listes des destinataires"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "Se connecter à votre compte SFTP via SSH. Télécharger, obtenir, supprimer des fichiers et bien plus encore"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Se connecter au serveur local SharePoint. Gérer des documents, des éléments de liste et bien plus encore"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Se connecter à SharePoint Online. Gérer des documents, des éléments de liste et bien plus encore"
[slack-doc]: ./connectors-create-api-slack.md "Se connecter à Slack et publier des messages sur les canaux Slack"
[smtp-doc]: ./connectors-create-api-smtp.md "Se connecter à un serveur SMTP et envoyer du courrier électronique avec des pièces jointes"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Se connecter à SparkPost pour la communication"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Se connecter à Azure SQL Database ou SQL Server. Créer, mettre à jour, obtenir et supprimer des entrées dans une table de base de données SQL"
[teradata-doc]: https://docs.microsoft.com/connectors/teradata/ "Se connecter à votre base de données Teradata pour lire des données à partir des tables"
[trello-doc]: ./connectors-create-api-trello.md "Se connecter à Trello. Gérer vos projets et organiser ce que vous voulez avec qui vous voulez"
[twilio-doc]: ./connectors-create-api-twilio.md "Se connecter à Twilio. Envoyer et obtenir des messages, obtenir des numéros disponibles, gérer des numéros de téléphone entrants et bien plus encore"
[twitter-doc]: ./connectors-create-api-twitter.md "Se connecter à Twitter. Consulter les fils d’actualité, publier des tweets et bien plus encore"
[yammer-doc]: ./connectors-create-api-yammer.md "Se connecter à Yammer. Publier des messages, obtenir de nouveaux messages et bien plus encore"
[youtube-doc]: ./connectors-create-api-youtube.md "Se connecter à YouTube. Gérer vos vidéos et vos canaux"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Encoder et décoder des messages qui utilisent le protocole AS2"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Encoder et décoder des messages qui utilisent le protocole EDIFACT"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Décoder les messages qui utilisent le protocole EDIFACT"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Encoder les messages qui utilisent le protocole EDIFACT"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Découvrir le fichier plat d’intégration d’entreprise"
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Découvrir le fichier plat d’intégration d’entreprise"
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Gérer les métadonnées des artefacts de compte d’intégration"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Transformer du code JSON avec des modèles Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Encoder et décoder des messages qui utilisent le protocole X12"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Décoder les messages qui utilisent le protocole X12"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Encoder les messages qui utilisent le protocole X12"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Transformer des messages XML"
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Valider des messages XML"

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[inline-code-icon]: ./media/apis-list/inline-code.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-file-storage-icon]: ./media/apis-list/azure-file-storage.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[azure-sql-data-warehouse-icon]: ./media/apis-list/azure-sql-data-warehouse.png
[azure-table-storage-icon]: ./media/apis-list/azure-table-storage.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-ssh-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png
