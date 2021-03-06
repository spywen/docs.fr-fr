---
title: "Consid&#233;rations sur la s&#233;curit&#233; des m&#233;tadonn&#233;es | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e78ef8ab-4f63-4656-ab93-b1deab2666d5
caps.latest.revision: 10
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 10
---
# Consid&#233;rations sur la s&#233;curit&#233; des m&#233;tadonn&#233;es
Lorsque vous utilisez des fonctionnalités de métadonnées dans [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)], vous devez tenir compte des conséquences liées à la publication, la récupération et l'utilisation de métadonnées de service en termes de sécurité.  
  
## Quand publier des métadonnées  
 Les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] ne publient pas de métadonnées par défaut.  Pour publier des métadonnées pour un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], vous devez activer explicitement la publication de métadonnées en ajoutant des points de terminaison de métadonnées à votre service \(consultez [Publication de métadonnées](../../../../docs/framework/wcf/feature-details/publishing-metadata.md)\).  Désactiver la publication de métadonnées permet de réduire la surface d'attaque de votre service et limiter le risque de divulgation d'informations involontaire.  Tous les services ne sont pas tenus de publier des métadonnées.  Si vous n'avez pas besoin de publier de métadonnées, laissez cette fonctionnalité désactivée.  Notez que vous pouvez encore générer des métadonnées et le code client directement à partir des assemblys de votre service à l'aide de l'[Outil Service Model Metadata Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] l'utilisation de Svcutil.exe pour exporter des métadonnées, consultez [Comment : utiliser Svcutil.exe pour exporter des métadonnées à partir de code de service compilé](../../../../docs/framework/wcf/feature-details/how-to-use-svcutil-exe-to-export-metadata-from-compiled-service-code.md).  
  
## Publication de métadonnées à l'aide d'une liaison sécurisée  
 Les liaisons de métadonnées par défaut fournies par [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] ne sont pas sécurisées et elles autorisent l'accès anonyme aux métadonnées.  Les métadonnées de service publiées par un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] contiennent une description détaillée du service et peuvent, de manière intentionnelle ou non intentionnelle, contenir des informations sensibles.  Par exemple, des métadonnées de service peuvent contenir des informations sur des opérations d'infrastructure qui n'étaient pas destinées à être diffusées publiquement.  Pour protéger des métadonnées de service contre les accès non autorisés, vous pouvez recourir à une liaison sécurisée pour le point de terminaison des métadonnées.  Les points de terminaison de métadonnées répondent à des demandes HTTP\/GET qui peuvent utiliser le protocole SSL \(Secure Sockets Layer\) pour sécuriser les métadonnées.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Comment : sécuriser des points de terminaison de métadonnées](../../../../docs/framework/wcf/feature-details/how-to-secure-metadata-endpoints.md).  
  
 La sécurisation des points de terminaison de métadonnées permet aussi de fournir aux demandeurs un moyen de récupérer en toute sécurité les métadonnées de service sans risque de falsification ou d'usurpation.  
  
## Utilisation de métadonnées approuvées uniquement  
 Les métadonnées de service permettent de construire automatiquement les composants d'exécution requis pour appeler le service.  Vous pouvez également utiliser des métadonnées au moment du design, pour développer une application cliente, ou au moment de l'exécution, pour mettre à jour de manière dynamique la liaison qu'un client utilise pour appeler un service.  
  
 Les métadonnées de service peuvent être falsifiées ou usurpées lorsqu'elles sont récupérées de façon non sécurisée.  Des métadonnées falsifiées sont susceptibles de rediriger votre client vers un service malveillant ou de contenir des paramètres de sécurité compromis ou des structures XML nuisibles.  Les documents de métadonnées peuvent être volumineux et sont souvent enregistrés dans le système de fichiers.  Pour vous protéger contre la falsification et l'usurpation de données, utilisez une liaison sécurisée pour demander des métadonnées de service, lorsque ce type de liaison est disponible.  
  
## Utilisation de techniques éprouvées pour traiter les métadonnées  
 Les métadonnées de service sont souvent récupérées d'un service situé sur un réseau à l'aide de protocoles standardisés, tels que WS\-MetadataExchange \(MEX\).  De nombreux formats de métadonnées incluent des mécanismes de référencement pour pointer vers des métadonnées supplémentaires.  Le type <xref:System.ServiceModel.Description.MetadataExchangeClient> traite automatiquement les références, à votre place, dans des documents WSDL \(Web Services Description Language\), des schémas XML et des documents MEX.  La taille de l'objet <xref:System.ServiceModel.Description.MetadataSet> créé à partir des métadonnées récupérées est directement proportionnelle à la valeur <xref:System.ServiceModel.Description.MetadataExchangeClient.MaximumResolvedReferences%2A> de l'instance <xref:System.ServiceModel.Description.MetadataExchangeClient> utilisée et à la valeur `MaxReceivedMessageSize` de la liaison utilisée par cette instance <xref:System.ServiceModel.Description.MetadataExchangeClient>.  Affectez à ces quotas les valeurs appropriées définies par votre scénario.  
  
 Dans [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], les métadonnées de service sont traitées comme des données XML.  Lors du traitement de documents XML, les applications doivent se protéger contre des structures XML malveillantes.  Utilisez `XmlDictionaryReader` avec les quotas appropriés lors du traitement XML et affectez également à la propriété <xref:System.XML.XmlReaderSettings.ProhibitDtd%2A> sur l'objet <xref:System.Xml.XmlReaderSettings> de votre instance <xref:System.Xml.XmlReader> la valeur `true`.  
  
 Le système de métadonnées dans [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] est extensible et les extensions de métadonnées peuvent être inscrites dans le fichier de configuration de l'application \(consultez [Extension du système de métadonnées](../../../../docs/framework/wcf/extending/extending-the-metadata-system.md)\).  Les extensions de métadonnées peuvent exécuter du code arbitraire, vous devez donc protéger le fichier de configuration de l'application avec des listes de contrôle d'accès \(ACL\) appropriées et inscrire uniquement les implémentations d'extensions de métadonnées approuvées.  
  
## Validation des clients générés  
 Lorsque vous générez du code client à partir de métadonnées récupérées d'une source non approuvée, validez le code client généré pour vous assurer que le client généré est conforme aux stratégies de sécurité des applications clientes.  Vous pouvez utiliser un comportement de validation pour vérifier les paramètres sur votre liaison cliente ou inspecter visuellement le code généré par les outils.  Pour obtenir un exemple montrant comment implémenter un client qui valide des comportements, consultez [Client Validation](../../../../docs/framework/wcf/samples/client-validation.md).  
  
## Protection des fichiers de configuration d'application  
 Le fichier de configuration d'application d'un service peut spécifier si des métadonnées doivent être publiées et de quelle façon.  Il est judicieux de protéger le fichier de configuration d'application à l'aide des listes de contrôle d'accès \(ACL\) appropriées pour vous assurer qu'aucun intrus ne pourra modifier ces paramètres.  
  
## Voir aussi  
 [Comment : sécuriser des points de terminaison de métadonnées](../../../../docs/framework/wcf/feature-details/how-to-secure-metadata-endpoints.md)   
 [Sécurité](../../../../docs/framework/wcf/feature-details/security.md)