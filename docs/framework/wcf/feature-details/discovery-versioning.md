---
title: "Contr&#244;le des versions de d&#233;couverte | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f91c6d0a-3af2-45c5-9a5c-e75390619836
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# Contr&#244;le des versions de d&#233;couverte
Cette rubrique donne une rapide vue d'ensemble de l'implémentation de nouvelles fonctionnalités de découverte.  Elle donne également une vue d'ensemble de la manière de sélectionner la version de découverte à utiliser.  
  
## Contrôle des versions de découverte  
 La fonctionnalité de découverte inclut la prise en charge de trois versions du protocole WS\_Discovery.  Les API de découverte vous permettent de sélectionner la version du protocole que vous souhaitez utiliser.  Ce document décrit brièvement les paramètres liés au contrôle des versions.  
  
 Les classes de découverte suivantes ont désormais une propriété <xref:System.ServiceModel.Discovery.DiscoveryVersion> et prennent un argument <xref:System.ServiceModel.Discovery.DiscoveryVersion> dans leurs constructeurs :  
  
-   <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>  
  
-   <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>  
  
-   <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>  
  
-   <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>  
  
### DiscoveryVersion.WSDiscoveryApril2005  
 Si vous spécifiez <xref:System.ServiceModel.Discovery.DiscoveryVersion.WSDiscoveryApril2005> comme paramètre de constructeur, l'implémentation utilise la version d'avril 2005 du protocole WS\-Discovery.  Cette version correspond à la version publiée de la spécification du protocole WS\-Discovery.  Cette version doit être utilisée pour interopérer avec une application héritée utilisant la version de WS\-Discovery d'avril 2005.  
  
### DiscoveryVersion.WSDiscovery11  
 La version de découverte par défaut utilisée par les API est <xref:System.ServiceModel.Discovery.DiscoveryVersion.WSDiscovery11>.  C'est la version standardisée actuelle du protocole WS\-Discovery.  
  
## DiscoveryVersion.WSDiscoveryCD1  
 Si vous spécifiez <xref:System.ServiceModel.Discovery.DiscoveryVersion.WSDiscoveryCD1> comme paramètre de constructeur, l'implémentation utilise la version Committee Draft 1 du protocole WS\-Discovery.  Cette version du protocole doit être utilisée pour interopérer avec les implémentations exécutant la version CD1 du protocole WS\-Discovery.  
  
## Prise en charge de plusieurs points de terminaison de découverte UDP pour différentes versions de découverte sur un hôte de service unique  
 Vous pouvez exposer plusieurs points de terminaison de découverte UDP pour différentes versions de découverte sur un hôte de service unique.  Pour ce faire, vous devez spécifier une adresse unique pour chaque point de terminaison de découverte UDP.  L'exemple suivant montre comment effectuer cette opération.  
  
```  
UdpDiscoveryEndpoint newVersionUdpEndpoint = new UdpDiscoveryEndpoint(DiscoveryVersion.WSDiscovery11);  
UdpDiscoveryEndpoint oldVersionUdpEndpoint = new UdpDiscoveryEndpoint(DiscoveryVersion.WSDiscoveryApril2005);  
  
newVersionUdpEndpoint.Address = new EndpointAddress(newVersionUdpEndpoint.Address.Uri.ToString() + "/version11");  
oldVersionUdpEndpoint.Address = new EndpointAddress(oldVersionUdpEndpoint.Address.Uri.ToString() + "/versionAril2005");  
  
serviceHost.AddServiceEndpoint(newVersionUdpEndpoint);  
serviceHost.AddServiceEndpoint(oldVersionUdpEndpoint);  
  
```