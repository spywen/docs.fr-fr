---
title: "Types de contraintes | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b6b246e6-1130-4698-9625-c5c42abcbfed
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Types de contraintes
Cet exemple illustre deux façons différentes d'appliquer des contraintes à un workflow, une à partir de l'intérieur de l'activité \(génération\) et l'autre à partir de l'extérieur de l'activité \(stratégie\).Dans ce scénario, un auteur d'activité \(d'un éditeur de logiciels tiers\) souhaite valider la relation entre deux arguments.Dans ce cas, le coût doit être inférieur ou égal au prix.Il s'agit d'une contrainte de génération de validation générale.  
  
 Un propriétaire de magasin souhaite ensuite ajouter une validation à cette activité générique.Dans son cas, il souhaite que le prix de la majorité de ses produits soit égal ou inférieur à 9,99 $.Il utilise ainsi une contrainte de stratégie qui est au\-dessus de l'activité `CreateProduct`.  
  
 Dans l'exemple :  
  
 L'auteur d'activité \(génération\) doit :  
  
-   Créer une contrainte \(`PriceGreaterThanCost`\).Il s'agit de l'emplacement de toute la logique de validation.  
  
-   Substituer <xref:System.Activities.CodeActivity%601.OnGetConstraints%2A> et ajouter la contrainte \(`PriceGreaterThanCost`\) aux contraintes <xref:System.Collections.IList>.  
  
 L'auteur de workflow \(stratégie\) doit effectuer les opérations suivantes :  
  
-   Créer un workflow avec une instance de l'activité à valider \(`CreateProduct`\).  
  
-   Créer une contrainte \(`MaxPrice`\).  
  
-   Créer une instance <xref:System.Activities.Validation.ValidatorSettings> \(`validatorSettings`\) et ajouter la contrainte \(`MaxPrice`\) à la collection `AdditionalConstraints`.Ici, l'auteur de workflow peut ajouter des contraintes de stratégie à toute activité, telle qu'une séquence ou un parallèle.  
  
-   Appeler <xref:System.Activities.Validation.ActivityValidationServices.Validate%2A>, qui retourne une collection <xref:System.Activities.Validation.ValidationResults> d'objets <xref:System.Activities.Validation.ConstraintViolation>.  
  
-   \(Facultatif\) Imprimer les objets <xref:System.Activities.Validation.ConstraintViolation>.  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Ouvrez l'exemple de solution ConstraintTypes.sln dans [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Générez et exécutez la solution.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Scenario\Validation\ConstraintLibrary`  
  
## Voir aussi