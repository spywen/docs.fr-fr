---
title: "H&#233;bergement dans une application de service Windows | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f4199998-27f3-4dd9-aee4-0a4addfa9f24
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# H&#233;bergement dans une application de service Windows
Les services Windows \(autrefois connus comme services Windows NT\) fournissent un modèle de processus particulièrement adapté aux applications qui doivent exister dans un exécutable à durée d'exécution longue et n'affichent aucune forme d'interface utilisateur.  La durée de vie de processus d'une application de service Windows est gérée par le gestionnaire de contrôle des services \(SCM\) qui vous autorise à démarrer, arrêter et suspendre les applications de service Windows.  Vous pouvez configurer un processus de service Windows pour qu'il démarre automatiquement lorsque l'ordinateur démarre, créant ainsi un environnement d'hébergement adapté pour les applications « toujours actives ».  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] les applications de service Windows, consultez [Applications de service Windows](http://go.microsoft.com/fwlink/?LinkId=89450).  
  
 Les applications qui hébergent des services [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] à durée d'exécution longue partagent de nombreuses caractéristiques avec les services Windows.  Notamment, les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] sont des exécutables du serveur à durée d'exécution longue qui n'interagissent pas directement avec l'utilisateur et, par conséquent, n'implémentent aucune forme d'interface utilisateur.  Par conséquent, l'hébergement de services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] dans une application de service Windows est l'une des solutions pour construire des applications [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] à durée d'exécution longue solides.  
  
 Souvent, les développeurs de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] doivent décider s'il faut héberger leur application [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] dans une application de service Windows, dans les services IIS \(Internet Information Services\) ou bien dans l'environnement d'hébergement du service d'activation des processus Windows \(WAS\).  Vous pouvez envisager d'utiliser des applications de service Windows dans les conditions suivantes :  
  
-   Votre application requiert l'activation explicite.  Par exemple, vous devez utiliser des services Windows lorsque votre application doit démarrer automatiquement quand le serveur démarre au lieu d'être démarrée dynamiquement en réponse au premier message entrant.  
  
-   Le processus qui héberge votre application doit rester actif une fois démarré.  Une fois démarré, un processus de service Windows reste actif à moins qu'il ne soit explicitement interrompu par l'administrateur du serveur via le gestionnaire de contrôle des services.  Les applications hébergées dans IIS ou WAS peuvent être démarrées et arrêtées dynamiquement pour une utilisation optimale des ressources système.  Les applications qui requièrent un contrôle explicite sur la durée de vie de leur processus d'hébergement doivent utiliser des services Windows au lieu d'IIS ou WAS.  
  
-   Votre service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] doit s'exécuter sous Windows Server 2003 et utiliser des transports autres que HTTP.  Sous Windows Server 2003, l'environnement d'hébergement [!INCLUDE[iis601](../../../../includes/iis601-md.md)] est restreint uniquement à la communication HTTP.  Les applications de service Windows ne sont pas soumises à cette restriction et peuvent utiliser tous les supports [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] de transport, y compris net.tcp, net.pipe et net.msmq.  
  
### Pour héberger WCF dans une application de service Windows  
  
1.  Créez une application de service Windows.  Vous pouvez écrire des applications de service Windows en code managé à l'aide des classes dans l'espace de noms <xref:System.ServiceProcess>.  Cette application doit inclure une classe qui hérite de <xref:System.ServiceProcess.ServiceBase>.  
  
2.  Liez la durée de vie des services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] à la durée de vie de l'application de service Windows.  En général, vous souhaitez que les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] hébergés dans une application de service Windows deviennent actifs lorsque le service d'hébergement démarre, cessent d'écouter les messages lorsque le service d'hébergement est arrêté et interrompent le processus d'hébergement lorsque le service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] rencontre une erreur.  Cela peut être accompli de la façon suivante :  
  
    -   Remplacez [OnStart\(String\<xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> pour ouvrir une ou plusieurs instances de <xref:System.ServiceModel.ServiceHost>.  Une application de service Windows unique peut héberger plusieurs services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] qui démarrent et s'arrêtent en tant que groupe.  
  
    -   Remplacez <xref:System.ServiceProcess.ServiceBase.OnStop%2A> pour appeler <xref:System.ServiceModel.Channels.CommunicationObject.Closed> sur <xref:System.ServiceModel.ServiceHost> tout service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] actif démarré pendant [OnStart\(String\<xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29>.  
  
    -   Abonnez\-vous à l'événement <xref:System.ServiceModel.Channels.CommunicationObject.Faulted> de <xref:System.ServiceModel.ServiceHost> et utilisez la classe <xref:System.ServiceProcess.ServiceController> pour interrompre l'application de service Windows en cas d'erreur.  
  
     Les applications de service Windows qui hébergent les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] sont déployées et gérées de la même façon que les applications de service Windows qui n'utilisent pas [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
## Voir aussi  
 <xref:System.ServiceProcess>   
 [Procédure pas à pas : création d'une application de service Windows dans le Concepteur de composants](http://go.microsoft.com/fwlink/?LinkId=94875)   
 [Comment : héberger un service WCF dans un service Windows managé](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md)   
 [Windows Service Host](../../../../docs/framework/wcf/samples/windows-service-host.md)   
 [Architecture de programmation d'une application de service](http://go.microsoft.com/fwlink/?LinkId=94876)   
 [Fonctionnalités d'hébergement de Windows Server AppFabric](http://go.microsoft.com/fwlink/?LinkId=201276)