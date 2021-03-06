---
title: Supprimer Azure Active Directory Domain Services | Microsoft Docs
description: Découvrez comment désactiver ou supprimer un domaine managé Azure Active Directory Domain Services à partir du portail Azure
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 595436daa2efbd8e706a539d0a89c3ea98be31ff
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80655466"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Désactiver ou supprimer un domaine managé Azure Active Directory Domain Services à partir du portail Azure

Si vous n’avez plus besoin d’un domaine managé, vous pouvez supprimer une instance Azure Active Directory Domain Services (Azure AD DS). Il n’existe aucune option permettant de désactiver de façon provisoire ou définitive un domaine managé Azure AD DS. La suppression du domaine managé Azure AD DS ne supprime ni n’impacte défavorablement le locataire Azure AD. Cet article explique comment supprimer un domaine managé Azure AD DS à partir du portail Azure.

> [!WARNING]
> **La suppression est définitive et ne peut pas être annulée.**
> Quand vous supprimez un domaine managé Azure AD DS, voici ce qu’il se passe :
>   * Les contrôleurs de domaine pour le domaine managé sont déprovisionnés et supprimés du réseau virtuel.
>   * Les données sur le domaine managé sont supprimées définitivement. Ces données incluent les unités d’organisation personnalisées, les stratégies de groupe, les enregistrements DNS personnalisés, les principaux de service, les GMSA, etc. que vous avez créés.
>   * Les machines jointes au domaine managé perdent leur relation d’approbation avec ce domaine et doivent être disjointes de celui-ci.
>       * Vous ne pouvez pas vous connecter à ces machines en utilisant des informations d’identification AD. Pour cela, vous devez utiliser les informations d’identification de l’administrateur local de la machine.

## <a name="delete-the-managed-domain"></a>Supprimer le domaine managé

Pour supprimer un domaine managé Azure AD DS, effectuez les étapes suivantes :

1. Sur le portail Azure, recherchez et sélectionnez **Azure AD Domain Services**.
1. Sélectionnez le nom de votre domaine managé Azure AD DS, par exemple *aaddscontoso.com*.
1. Dans la page **Vue d’ensemble**, sélectionnez **Supprimer**. Pour confirmer la suppression, tapez à nouveau le nom du domaine managé, puis sélectionnez **Supprimer**.

La suppression du domaine managé Azure AD DS peut prendre entre 15 et 20 minutes, voire plus.

## <a name="next-steps"></a>Étapes suivantes

N’hésitez pas à [faire part de vos commentaires][feedback] concernant les fonctionnalités que vous aimeriez voir dans Azure AD DS.

Si vous voulez redémarrer avec Azure AD DS, consultez [Créer et configurer une instance Azure Active Directory Domain Services][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
