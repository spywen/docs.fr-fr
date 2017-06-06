---
title: "Emprunt de l&#39;identit&#233; du client | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Emprunt de l'identité du client (exemple) (Windows Communication Foundation)"
  - "emprunt d'identité, exemple Windows Communication Foundation"
  - "comportements de service, emprunt d'identité (exemple)"
ms.assetid: 8bd974e1-90db-4152-95a3-1d4b1a7734f8
caps.latest.revision: 25
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 25
---
# Emprunt de l&#39;identit&#233; du client
Cet exemple montre comment emprunter l'identité de l'application de l'appelant au niveau du service afin que ce dernier puisse accéder aux ressources système pour le compte de l'appelant.  
  
 Il est basé sur l'exemple [Self\-Host](../../../../docs/framework/wcf/samples/self-host.md).Les fichiers de configuration du service et du client sont le mêmes que ceux de l'exemple [Self\-Host](../../../../docs/framework/wcf/samples/self-host.md).  
  
> [!NOTE]
>  La procédure d'installation ainsi que les instructions de génération relatives à cet exemple figurent à la fin de cette rubrique.  
  
 Le code de service a été modifié afin que la méthode `Add` sur le service emprunte l'identité de l'appelant à l'aide de <xref:System.ServiceModel.OperationBehaviorAttribute>, tel qu'indiqué dans l'exemple de code suivant.  
  
```  
[OperationBehavior(Impersonation = ImpersonationOption.Required)]  
public double Add(double n1, double n2)  
{  
    double result = n1 + n2;  
    Console.WriteLine("Received Add({0},{1})", n1, n2);  
    Console.WriteLine("Return: {0}", result);  
    DisplayIdentityInformation();  
    return result;  
}  
  
```  
  
 En conséquence, le contexte de sécurité du thread en cours d'exécution est basculé pour emprunter l'identité de l'appelant avant d'entrer dans la méthode `Add` et est rétabli lorsqu'il quitte la méthode.  
  
 La méthode `DisplayIdentityInformation` indiquée dans l'exemple de code suivant est une fonction utilitaire qui affiche l'identité de l'appelant.  
  
```  
static void DisplayIdentityInformation()  
{  
    Console.WriteLine("\t\tThread Identity            :{0}",  
         WindowsIdentity.GetCurrent().Name);  
    Console.WriteLine("\t\tThread Identity level  :{0}",   
         WindowsIdentity.GetCurrent().ImpersonationLevel);  
    Console.WriteLine("\t\thToken                     :{0}",  
         WindowsIdentity.GetCurrent().Token.ToString());  
    return;  
}  
  
```  
  
 La méthode `Subtract` sur le service emprunte l'identité de l'appelant à l'aide d'appels impératifs, tel qu'indiqué dans l'exemple de code suivant.  
  
```  
public double Subtract(double n1, double n2)  
{  
    double result = n1 - n2;  
    Console.WriteLine("Received Subtract({0},{1})", n1, n2);  
    Console.WriteLine("Return: {0}", result);  
Console.WriteLine("Before impersonating");  
DisplayIdentityInformation();  
  
    if (ServiceSecurityContext.Current.WindowsIdentity.ImpersonationLevel == TokenImpersonationLevel.Impersonation ||  
        ServiceSecurityContext.Current.WindowsIdentity.ImpersonationLevel == TokenImpersonationLevel.Delegation)  
    {  
        // Impersonate.  
        using (ServiceSecurityContext.Current.WindowsIdentity.Impersonate())  
        {  
            // Make a system call in the caller's context and ACLs   
            // on the system resource are enforced in the caller's context.   
            Console.WriteLine("Impersonating the caller imperatively");  
            DisplayIdentityInformation();  
        }  
    }  
    else  
    {  
        Console.WriteLine("ImpersonationLevel is not high enough to perform this operation.");  
    }  
  
Console.WriteLine("After reverting");  
DisplayIdentityInformation();  
    return result;  
}  
```  
  
 Notez que dans ce cas, l'identité de l'appelant n'est pas empruntée pour l'ensemble de l'appel, mais uniquement pour une partie de celui\-ci.En général, l'emprunt d'identité pour la plus petite étendue est préférable à l'emprunt d'identité pour l'ensemble de l'opération.  
  
 Les autres méthodes n'empruntent pas l'identité de l'appelant.  
  
 Le code client a été modifié pour affecter <xref:System.Security.Principal.TokenImpersonationLevel> au niveau d'emprunt d'identité.Le client spécifie le niveau d'emprunt d'identité à utiliser par le service, en utilisant l'énumération <xref:System.Security.Principal.TokenImpersonationLevel>.L'énumération prend en charge les valeurs suivantes : <xref:System.Security.Principal.TokenImpersonationLevel>, <xref:System.Security.Principal.TokenImpersonationLevel>, <xref:System.Security.Principal.TokenImpersonationLevel>, <xref:System.Security.Principal.TokenImpersonationLevel> et <xref:System.Security.Principal.TokenImpersonationLevel>.Pour effectuer un contrôle lors de l'accès à une ressource système sur l'ordinateur local protégé à l'aide des listes de contrôle d'accès Windows, le niveau d'emprunt d'identité doit avoir la valeur <xref:System.Security.Principal.TokenImpersonationLevel>, tel qu'indiqué dans l'exemple de code suivant.  
  
```  
// Create a client with given client endpoint configuration  
CalculatorClient client = new CalculatorClient();  
  
client.ClientCredentials.Windows.AllowedImpersonationLevel = TokenImpersonationLevel.Impersonation;  
```  
  
 Lorsque vous exécutez l'exemple, les demandes et réponses de l'opération s'affichent dans les fenêtres de console du service et du client.Appuyez sur ENTER dans chaque fenêtre de console pour arrêter le service et le client.  
  
> [!NOTE]
>  Le service doit s'exécuter sous un compte d'administrateur ou le compte sous lequel il s'exécute doit disposer des droits pour enregistrer l'URI http:\/\/localhost:8000\/ServiceModelSamples avec la couche HTTP.Des droits de ce type peuvent être accordés en définissant une [Réservation d'espace de noms](http://go.microsoft.com/fwlink/?LinkId=95012) \(page pouvant être en anglais\) à l'aide de l'[Outil Httpcfg.exe](http://go.microsoft.com/fwlink/?LinkId=95010) \(page pouvant être en anglais\).  
  
> [!NOTE]
>  Sur les ordinateurs qui exécutent [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)], l'emprunt d'identité est uniquement pris en charge si l'application Host.exe dispose du privilège d'emprunt d'identité.\(Par défaut, seuls les administrateurs disposent de cette autorisation.\) Pour ajouter ce privilège à un compte sous lequel le service s'exécute, accédez à **Administrative Tools**, puis ouvrez **Local Security Policy**, **Local Policies**, cliquez sur **User Rights Assignment**, sélectionnez **Impersonate a Client after Authentication** et double\-cliquez sur **Properties** pour ajouter un utilisateur ou un groupe.  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure indiquée dans la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer l'édition C\# ou Visual Basic .NET de la solution, suivez les instructions indiquées dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, suivez les instructions indiquées dans [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
4.  Pour montrer que le service emprunte l'identité de l'appelant, exécutez le client sous un autre compte que celui sous lequel le service s'exécute.Pour ce faire, à l'invite de commandes tapez :  
  
    ```  
    runas /user:<machine-name>\<user-name> client.exe  
    ```  
  
     Vous êtes ensuite invité à entrer un mot de passe.Entrez le mot de passe du compte précédemment spécifié.  
  
5.  Lorsque vous exécutez le client, notez l'identité avant et après l'avoir exécuté avec des informations d'identification différentes.  
  
## Voir aussi