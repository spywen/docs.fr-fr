---
title: "H&#233;bergement de services de workflow | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 2d55217e-8697-4113-94ce-10b60863342e
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# H&#233;bergement de services de workflow
Un service de workflow doit être hébergé pour pouvoir répondre aux messages entrants.Les services de workflow utilisent l'infrastructure de messagerie [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] et sont donc hébergés de manière similaire.Comme les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], les services de workflow peuvent être hébergés dans n'importe quelle application managée, dans les services IIS \(Internet Information Services\) ou WAS \(Windows Process Activation Services\).De plus, les services de workflow peuvent être hébergés sous Windows Server App Fabric.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] Windows Server App Fabric, consultez la [documentation de Windows Server App Fabric](http://go.microsoft.com/fwlink/?LinkId=193037), les [fonctionnalités d'hébergement d'AppFabric](http://go.microsoft.com/fwlink/?LinkId=196494) et les [concepts d'hébergement AppFabric](http://go.microsoft.com/fwlink/?LinkId=196495).[!INCLUDE[crabout](../../../../includes/crabout-md.md)] les différentes manières d'héberger les services [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], consultez [Hébergement de services](../../../../docs/framework/wcf/hosting-services.md).  
  
## Hébergement dans une application managée  
 Pour héberger un service de workflow dans une application managée, utilisez la classe <xref:System.ServiceModel.Activities.WorkflowServiceHost>.Le constructeur <xref:System.ServiceModel.Activities.WorkflowServiceHost> vous permet de spécifier une instance singleton du service de workflow, une définition du service de workflow ou une activité qui utilise les activités de messagerie du workflow.L'appel à la méthode <xref:System.ServiceModel.Activities.WorkflowServiceHost.Open%2A> oblige le service à se mettre à l'écoute des messages entrants.  
  
## Hébergement dans les services IIS ou WAS  
 Héberger un service de workflow dans les services IIS ou WAS implique de créer un répertoire virtuel et d'y placer les fichiers qui définissent le service et son comportement.Lors de l'hébergement d'un service de workflow dans les services IIS ou WAS, il existe plusieurs possibilités :  
  
-   Placez un fichier .xamlx définissant le service de workflow dans un répertoire virtuel IIS\/WAS, ainsi qu'un fichier Web.config qui spécifie les comportements, les points de terminaison et d'autres éléments de configuration du service.  
  
-   Placez un fichier .xamlx définissant le service de workflow dans un répertoire virtuel IIS\/WAS.Le fichier .xamlx spécifie les points de terminaison à exposer.Les points de terminaison sont spécifiés dans un élément `WorkflowService.Endpoints`, comme le montre l'exemple suivant.  
  
    ```  
    <WorkflowService xmlns="http://schemas.microsoft.com/netfx/2009/xaml/servicemodel"  xmlns:p1="http://schemas.microsoft.com/netfx/2009/xaml/activities" xmlns:sad="clr-namespace:System.Activities.Debugger;assembly=System.Activities" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
      <WorkflowService.Endpoints>  
        <Endpoint ServiceContractName="IWorkFlowEchoService" AddressUri="">  
          <Endpoint.Binding>  
            <BasicHttpBinding />  
          </Endpoint.Binding>  
        </Endpoint>  
      </WorkflowService.Endpoints>  
    <!-- ... -->  
    </WorkflowService>  
  
    ```  
  
    > [!NOTE]
    >  Les comportements ne peuvent pas être spécifiés dans un fichier .xamlx ; vous devez donc utiliser un fichier Web.config si vous devez spécifier des paramètres de comportement.  
  
-   Placez un fichier .xamlx définissant le service de workflow dans un répertoire virtuel IIS\/WAS.Placez également un fichier .svc dans le répertoire virtuel.Ce fichier .svc vous permet de spécifier une fabrique hôte de service Web personnalisée, d'appliquer le comportement personnalisé ou de charger la configuration à partir d'un emplacement personnalisé.  
  
-   Placez un assembly dans le répertoire virtuel IIS\/WAS qui contient une activité utilisant les activités de messagerie WCF.  
  
 Un fichier .xamlx définissant un service de workflow doit contenir un élément racine \<`Service`\> ou un élément racine contenant un type dérivé de l'objet <xref:System.Workflow.ComponentModel.Activity>.Lors de l'utilisation du modèle Activité de [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)], un fichier .xamlx est créé.Lors de l'utilisation du modèle Service de workflow WCF, un fichier .xamlx est créé.  
  
## Hébergement des services de workflow sous Windows Server App Fabric  
 L'hébergement d'un service de workflow sous Windows Server App Fabric est similaire à l'hébergement sous IIS\/WAS.La seule différence est que Windows Server App Fabric est installé.Windows Server App Fabric fournit des outils qui sont ajoutés au Gestionnaire des services IIS, ainsi que des applets de commande powershell.Ces outils simplifient le déploiement, la gestion et le suivi des services de workflow et des services WCF..[!INCLUDE[crabout](../../../../includes/crabout-md.md)] Windows Server App Fabric, consultez [Windows Server App Fabric](http://go.microsoft.com/fwlink/?LinkId=193037).  
  
## Référencement d'activités personnalisées  
 Les références aux activités personnalisées doivent être ajoutées à la section \<`Assemblies`\> sous \<`System.Web.Compilation`\>, afin qu'elles soient chargées dans le domaine d'application et que le désérialiseur XAML soit en mesure de localiser les types.Ces paramètres peuvent être effectués au niveau de l'application ou dans le Web.config racine si les paramètres doivent s'appliquer à toutes les applications sur l'ordinateur.  
  
## Déploiement  
 L'outil de déploiement Web a été créé pour simplifier le travail de déploiement.Cet outil vous permet de migrer des applications entre IIS 6.0 et IIS 7.0, de synchroniser des batteries de serveurs et d'empaqueter, d'archiver et de déployer des applications Web.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][l'outil de déploiement MS](http://go.microsoft.com/fwlink/?LinkId=178690)  
  
## Voir aussi  
 [Éléments internes de l'hôte du service de workflow](../../../../docs/framework/wcf/feature-details/workflow-service-host-internals.md)   
 [Configuration de WorkflowServiceHost](../../../../docs/framework/wcf/feature-details/configuring-workflowservicehost.md)