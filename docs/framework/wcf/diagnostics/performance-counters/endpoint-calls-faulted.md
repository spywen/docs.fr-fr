---
title: "Point de terminaison&#160;: appels ayant renvoy&#233; des erreurs | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 271e6284-9c4b-465f-b619-069e1555a5e4
caps.latest.revision: 4
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 4
---
# Point de terminaison&#160;: appels ayant renvoy&#233; des erreurs
Nom du compteur : appels ayant renvoyé des erreurs.  
  
## Description  
 Nombre d'appels à ce point de terminaison qui ont retourné des erreurs.Dans les applications [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)], les méthodes de service communiquent les informations sur l'erreur de traitement à l'aide de messages d'erreur SOAP.Les erreurs SOAP sont des types de message inclus dans les métadonnées d'une opération de service et créent, par conséquent, un contrat d'erreur permettant aux clients d'améliorer la fiabilité ou l'interactivité de leur exécution.Les erreurs SOAP étant exprimées aux clients dans un format XML, elles sont très interopérables.  
  
## Voir aussi  
 [Spécification et gestion des erreurs dans les contrats et les services](../../../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md)