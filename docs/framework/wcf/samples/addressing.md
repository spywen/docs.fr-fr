---
title: "Addressing | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d438e6f2-d0f3-43aa-b259-b51b5bda2e64
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# Addressing
Cet exemple illustre les divers aspects et fonctionnalités des adresses de point de terminaison.  Il est basé sur [Getting Started](../../../../docs/framework/wcf/samples/getting-started-sample.md).  Dans cet exemple, le service est auto\-hébergé.  Le client et le service sont tous les deux des applications console.  Le service définit plusieurs points de terminaison en utilisant à la fois des adresses de point de terminaison absolues et relatives.  
  
> [!NOTE]
>  La procédure d'installation ainsi que les instructions de génération relatives à cet exemple figurent à la fin de cette rubrique.  
  
 Le fichier de configuration du service spécifie une adresse de base et quatre points de terminaison.  L'adresse de base est spécifiée à l'aide de l'élément Add, sous service\/host\/baseAddresses, tel qu'illustré dans l'exemple de configuration suivant.  
  
```  
<service   
    name="Microsoft.ServiceModel.Samples.CalculatorService"  
    behaviorConfiguration="CalculatorServiceBehavior">  
  <host>  
    <baseAddresses>  
      <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
    </baseAddresses>  
  </host>  
  ...  
</service>  
```  
  
 La première définition de point de terminaison contenue dans l'exemple de configuration suivant spécifie une adresse relative, ce qui signifie que l'adresse de point de terminaison est une combinaison entre l'adresse de base et l'adresse relative conformément aux règles de composition d'URI.  
  
```  
<!-- Empty relative address specified:   
     use the base address provided by the host. -->  
<!-- The endpoint address is  
     http://localhost:8000/ServiceModelSamples/service. -->  
<endpoint address=""  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 Dans ce cas de figure, l'adresse relative est vide \(""\). Par conséquent, l'adresse de point de terminaison correspond à l'adresse de base.  La véritable adresse de point de terminaison est http:\/\/localhost:8000\/servicemodelsamples\/service.  
  
 La deuxième définition de point de terminaison spécifie également une adresse relative, comme affiché dans l'exemple de configuration suivant.  
  
```  
<!-- The relative address specified: use the base address -->  
<!-- provided by the host + path. The endpoint address is -->  
<!-- http://localhost:8000/servicemodelsamples/service/test. -->  
<endpoint address="/test"  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 L'adresse relative, "test", est ajoutée à l'adresse de base.  La véritable adresse de point de terminaison est http:\/\/localhost:8000\/servicemodelsamples\/service\/test.  
  
 La troisième définition de point de terminaison spécifie une adresse absolue, comme affiché dans l'exemple de configuration suivant.  
  
```  
<endpoint address="http://localhost:8001/hello/servicemodelsamples"  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 L'adresse de base ne joue aucun rôle dans l'adresse.  La véritable adresse de point de terminaison est http:\/\/localhost:8001\/hello\/servicemodelsamples.  
  
 La quatrième adresse de point de terminaison spécifie une adresse absolue et un transport différent \- TCP.  L'adresse de base ne joue aucun rôle dans l'adresse.  La véritable adresse de point de terminaison est net.tcp:\/\/localhost:9000\/servicemodelsamples\/service.  
  
```  
<!-- The absolute address specified, different transport: -->  
<!-- use the specified address, and ignore the base address. -->  
<!-- The endpoint address is -->  
<!-- net.tcp://localhost:9000/servicemodelsamples/service. -->  
<endpoint address=  
          "net.tcp://localhost:9000/servicemodelsamples/service"  
          binding="netTcpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
</service>  
```  
  
 Le client accède uniquement à un seul des quatre points de terminaison du service, mais les quatre sont définis dans son fichier de configuration.  Le client sélectionne un point de terminaison lorsqu'il crée l'objet `CalculatorProxy`.  En modifiant le nom de configuration de `CalculatorEndpoint1` jusqu'à `CalculatorEndpoint4`, vous pouvez exécuter chacun des points de terminaison.  
  
 Lorsque vous exécutez l'exemple, le service énumère l'adresse, le nom de la liaison et le nom du contrat pour chacun de ses points de terminaison.  Le point de terminaison d'échange de métadonnées \(MEX\) est considéré comme étant un autre point de terminaison par ServiceHost, c'est pourquoi il apparaît dans la liste.  
  
```  
Service endpoints:  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service/test  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8001/hello/servicemodelsamples  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  net.tcp://localhost:9000/servicemodelsamples/service  
           binding:  NetTcpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service/mex  
           binding:  MetadataExchangeHttpBinding  
           contract: IMetadataExchange  
  
The service is ready.  
Press <ENTER> to terminate service.  
  
```  
  
 Lorsque vous exécutez le client, les demandes et réponses d'opération s'affichent dans les fenêtres de console du service et du client.  Appuyez sur ENTER dans chaque fenêtre de console pour arrêter le service et le client.  
  
```  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure indiquée à la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer l'édition C\# ou Visual Basic .NET de la solution, conformez\-vous aux instructions figurant dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, conformez\-vous aux instructions figurant dans [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
    > [!NOTE]
    >  Si vous utilisez Svcutil.exe pour régénérer la configuration pour cet exemple, assurez\-vous de modifier le nom du point de terminaison dans la configuration client afin qu'il corresponde au code client.  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.  Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].  Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Basic\Services\Addressing`  
  
## Voir aussi