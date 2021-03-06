---
title: "Flux de transactions vers et depuis des services de workflow | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 03ced70e-b540-4dd9-86c8-87f7bd61f609
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# Flux de transactions vers et depuis des services de workflow
Les services et clients de workflow peuvent participer aux transactions.Pour qu'une opération de service fasse partie d'une transaction ambiante, placez une activité <xref:System.ServiceModel.Activities.Receive> dans une activité <xref:System.ServiceModel.Activities.TransactedReceiveScope>.Tous les appels effectués par une activité <xref:System.ServiceModel.Activities.Send> ou <xref:System.ServiceModel.Activities.SendReply> au sein de l'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope> seront également effectués dans la transaction ambiante.Une application cliente de workflow peut créer une transaction ambiante en utilisant l'activité <xref:System.Activities.Statements.TransactionScope> et appeler des opérations de service à l'aide de la transaction ambiante.Cette rubrique vous guide dans la création d'un service de workflow et d'un client de workflow qui participent à des transactions.  
  
> [!WARNING]
>  Si une instance de service de workflow est chargée dans une transaction et le workflow contient une activité <xref:System.Activities.Statements.Persist>, l'instance de workflow va être bloquée le temps que la transaction expire.  
  
> [!IMPORTANT]
>  Lorsque vous utilisez une activité <xref:System.ServiceModel.Activities.TransactedReceiveScope>, il est recommandé de placer toutes les réceptions dans le workflow dans les activités <xref:System.ServiceModel.Activities.TransactedReceiveScope>.  
  
> [!IMPORTANT]
>  Lorsque vous utilisez <xref:System.ServiceModel.Activities.TransactedReceiveScope> et les messages arrivent dans le mauvais ordre incorrect, le workflow est abandonné lorsque vous tentez de livrer le premier message dans le désordre.Vous devez vous assurer que votre workflow est toujours à un point d'arrêt cohérent lorsqu'il est inactif.Cela vous permet de redémarrer le workflow à partir d'un point de persistance précédent s'il est abandonné.  
  
### Créer une bibliothèque partagée  
  
1.  Créez une solution Visual Studio vide.  
  
2.  Ajoutez un nouveau projet de bibliothèque de classes nommé `Common`.Ajoutez des références aux assemblys suivants :  
  
    -   System.Activities.dll  
  
    -   System.ServiceModel.dll  
  
    -   System.ServiceModel.Activities.dll  
  
    -   System.Transactions.dll  
  
3.  Ajoutez une nouvelle classe nommée `PrintTransactionInfo` au projet `Common`.Cette classe est dérivée de <xref:System.Activities.NativeActivity> et surcharge la méthode <xref:System.Activities.NativeActivity.Execute%2A>.  
  
    ```  
    using System;  
    using System;  
    using System.Activities;  
    using System.Transactions;  
  
    namespace Common  
    {  
        public class PrintTransactionInfo : NativeActivity  
        {  
            protected override void Execute(NativeActivityContext context)  
            {  
                RuntimeTransactionHandle rth = context.Properties.Find(typeof(RuntimeTransactionHandle).FullName) as RuntimeTransactionHandle;  
  
                if (rth == null)  
                {  
                    Console.WriteLine("There is no ambient RuntimeTransactionHandle");  
                }  
  
                Transaction t = rth.GetCurrentTransaction(context);  
  
                if (t == null)  
                {  
                    Console.WriteLine("There is no ambient transaction");  
                }  
                else  
                {  
                    Console.WriteLine("Transaction: {0} is {1}", t.TransactionInformation.DistributedIdentifier, t.TransactionInformation.Status);  
                }  
            }  
        }  
  
    }  
  
    ```  
  
     Il s'agit d'une activité native qui affiche des informations sur la transaction ambiante et est utilisée dans les workflows du service et du client utilisés dans cette rubrique.Générez la solution pour rendre cette activité disponible dans la section **Commun** de la **boîte à outils**.  
  
### Implémenter le service de workflow  
  
1.  Ajoutez un nouveau service de workflow [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], nommé `WorkflowService`, au projet `Common`.Pour ce faire, cliquez avec le bouton droit sur le projet `Common`, sélectionnez **Ajouter**, **Nouvel élément**, puis **Workflow** sous **Modèles installés** et enfin **Application de service de workflow WCF**.  
  
     ![Ajout d'un service de flux de travail](../../../../docs/framework/wcf/feature-details/media/addwfservice.JPG "AddWFService")  
  
2.  Supprimez les activités par défaut `ReceiveRequest` et `SendResponse`.  
  
3.  Faites glisser une activité <xref:System.Activities.Statements.WriteLine> dans l'activité `Sequential Service`.Affectez à la propriété Text la valeur `"Workflow Service starting ..."`, comme le montre l'exemple suivant.  
  
     ![Ajout d'une activité WriteLine](../../../../docs/framework/wcf/feature-details/media/addwriteline.JPG "AddWriteLine")  
  
4.  Faites glisser une activité <xref:System.ServiceModel.Activities.TransactedReceiveScope> après l'activité <xref:System.Activities.Statements.WriteLine>.L'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope> se trouve dans la section **Messagerie** de la **boîte à outils**.L'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope> est composée de deux sections : **Requête** et **Corps**.La section **Requête** contient l'activité <xref:System.ServiceModel.Activities.Receive>.La section **Corps** contient les activités à exécuter dans une transaction après réception d'un message.  
  
     ![Ajout d'une activité TransactedReceiveScope](../../../../docs/framework/wcf/feature-details/media/trs.JPG "TRS")  
  
5.  Sélectionnez l'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope> et cliquez sur le bouton **Variables**.Ajoutez les variables suivantes.  
  
     ![Ajout de variables à TransactedReceiveScope](../../../../docs/framework/wcf/feature-details/media/trsvariables.JPG "TRSVariables")  
  
    > [!NOTE]
    >  Vous pouvez supprimer la variable de données proposée à cet endroit par défaut.Vous pouvez également utiliser la variable de handle existante.  
  
6.  Faites glisser une activité <xref:System.ServiceModel.Activities.Receive> dans la section **Requête** de l'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope>.Définissez les propriétés suivantes :  
  
    |Propriété|Valeur|  
    |---------------|------------|  
    |CanCreateInstance|True \(activez la case à cocher\)|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     Le workflow doit ressembler à ceci :  
  
     ![Ajout d'une activité Receive](../../../../docs/framework/wcf/feature-details/media/serviceaddreceive.JPG "ServiceAddReceive")  
  
7.  Cliquez sur le lien **Définir** de l'activité <xref:System.ServiceModel.Activities.Receive> et effectuez les paramétrages suivants :  
  
     ![Définition des paramètres des messages pour l'activité Receive](../../../../docs/framework/wcf/feature-details/media/receivemessagesettings.JPG "ReceiveMessageSettings")  
  
8.  Faites glisser une activité <xref:System.Activities.Statements.Sequence> dans la section de corps du <xref:System.ServiceModel.Activities.TransactedReceiveScope>.Dans l'activité <xref:System.Activities.Statements.Sequence>, faites glisser deux activités <xref:System.Activities.Statements.WriteLine> et définissez les propriétés <xref:System.Activities.Statements.WriteLine.Text%2A> conformément aux indications du tableau suivant.  
  
    |Activité|Valeur|  
    |--------------|------------|  
    |1re WriteLine|“Service: Receive Completed”|  
    |2e WriteLine|"Service: Received \= " \+ requestMessage|  
  
     Le workflow doit maintenant ressembler à ceci :  
  
     ![Ajout d'activités WriteLine](../../../../docs/framework/wcf/feature-details/media/afteraddingwritelines.JPG "AfterAddingWriteLines")  
  
9. Faites glisser l'activité `PrintTransactionInfo` après la deuxième activité <xref:System.Activities.Statements.WriteLine> du **Corps** de l'activité <xref:System.ServiceModel.Activities.TransactedReceiveScope>.  
  
     ![Après l'ajout de PrintTransactionInfo](../../../../docs/framework/wcf/feature-details/media/afteraddingprinttransactioninfo.JPG "AfterAddingPrintTransactionInfo")  
  
10. Faites glisser une activité <xref:System.Activities.Statements.Assign> après l'activité `PrintTransactionInfo` et définissez ses propriétés conformément aux indications du tableau suivant.  
  
    |Propriété|Valeur|  
    |---------------|------------|  
    |To|replyMessage|  
    |Value|"Service: Sending reply."|  
  
11. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> après l'activité <xref:System.Activities.Statements.Assign> et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Service: Begin reply."  
  
     Le workflow doit maintenant ressembler à ceci :  
  
     ![Après l'ajout de Assign et de WriteLine](../../../../docs/framework/wcf/feature-details/media/afteraddingsbrwriteline.JPG "AfterAddingSBRWriteLine")  
  
12. Cliquez avec le bouton droit sur l'activité <xref:System.ServiceModel.Activities.Receive>, sélectionnez **Create SendReply** et collez\-la après la dernière activité <xref:System.Activities.Statements.WriteLine>.Cliquez sur le lien **Définir** de l'activité `SendReplyToReceive` et effectuez les paramétrages suivants.  
  
     ![Paramètres des messages Reply](../../../../docs/framework/wcf/feature-details/media/replymessagesettings.JPG "ReplyMessageSettings")  
  
13. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> après l'activité `SendReplyToReceive` et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Service: Reply sent."  
  
14. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> en bas du workflow et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Service: Workflow ends, press ENTER to exit."  
  
     Le workflow de service terminé doit ressembler à ceci :  
  
     ![Flux de travail du service complet](../../../../docs/framework/wcf/feature-details/media/servicecomplete.jpg "ServiceComplete")  
  
### Implémenter le client de workflow  
  
1.  Ajoutez une nouvelle application de workflow WCF nommée `WorkflowClient` au projet `Common`.Pour ce faire, cliquez avec le bouton droit sur le projet `Common`, sélectionnez **Ajouter**, **Nouvel élément**, puis **Workflow** sous **Modèles installés** et enfin **Activité**.  
  
     ![Ajouter un projet d'activité](../../../../docs/framework/wcf/feature-details/media/addactivity.JPG "AddActivity")  
  
2.  Faites glisser une activité <xref:System.Activities.Statements.Sequence> sur l'aire de conception.  
  
3.  Dans l'activité <xref:System.Activities.Statements.Sequence>, faites glisser une activité <xref:System.Activities.Statements.WriteLine> et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur `"Client: Workflow starting"`.Le workflow doit maintenant ressembler à ceci :  
  
     ![Ajouter une activité WriteLine](../../../../docs/framework/wcf/feature-details/media/clientaddwriteline.JPG "ClientAddWriteLine")  
  
4.  Faites glisser une activité <xref:System.Activities.Statements.TransactionScope> après l'activité <xref:System.Activities.Statements.WriteLine>.Sélectionnez l'activité <xref:System.Activities.Statements.TransactionScope>, cliquez sur le bouton Variables et ajoutez les variables suivantes.  
  
     ![Ajouter des variables à TransactionScope](../../../../docs/framework/wcf/feature-details/media/tsvariables.JPG "TSVariables")  
  
5.  Faites glisser une activité <xref:System.Activities.Statements.Sequence> dans le corps de l'activité <xref:System.Activities.Statements.TransactionScope>.  
  
6.  Faites glisser une activité `PrintTransactionInfo` dans <xref:System.Activities.Statements.Sequence>.  
  
7.  Faites glisser une activité <xref:System.Activities.Statements.WriteLine> après l'activité `PrintTransactionInfo` et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Client: Beginning Send".Le workflow doit maintenant ressembler à ceci :  
  
     ![Ajout d'activités](../../../../docs/framework/wcf/feature-details/media/clientaddcbswriteline.JPG "ClientAddCBSWriteLine")  
  
8.  Faites glisser une activité <xref:System.ServiceModel.Activities.Send> après l'activité <xref:System.Activities.Statements.Assign> et définissez les propriétés suivantes :  
  
    |Propriété|Valeur|  
    |---------------|------------|  
    |EndpointConfigurationName|workflowServiceEndpoint|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     Le workflow doit maintenant ressembler à ceci :  
  
     ![Définition des propriétés de l'activité Send](../../../../docs/framework/wcf/feature-details/media/clientsendsettings.JPG "ClientSendSettings")  
  
9. Cliquez sur le lien **Définir** et effectuez les paramétrages suivants :  
  
     ![Paramètres des messages de l'activité Send](../../../../docs/framework/wcf/feature-details/media/sendmessagesettings.JPG "SendMessageSettings")  
  
10. Cliquez avec le bouton droit sur l'activité <xref:System.ServiceModel.Activities.Send> et sélectionnez **Create ReceiveReply**.L'activité <xref:System.ServiceModel.Activities.ReceiveReply> sera automatiquement placée après l'activité <xref:System.ServiceModel.Activities.Send>.  
  
11. Cliquez sur le lien Définir…de l'activité ReceiveReplyForSend et effectuez les paramétrages suivants :  
  
     ![Définition des paramètres de message ReceiveForSend](../../../../docs/framework/wcf/feature-details/media/clientreplymessagesettings.JPG "ClientReplyMessageSettings")  
  
12. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> entre les activités <xref:System.ServiceModel.Activities.Send> et <xref:System.ServiceModel.Activities.ReceiveReply> et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Client: Send complete."  
  
13. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> après l'activité <xref:System.ServiceModel.Activities.ReceiveReply> et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Client side: Reply received \= " \+ replyMessage  
  
14. Faites glisser une activité `PrintTransactionInfo` après l'activité <xref:System.Activities.Statements.WriteLine>.  
  
15. Faites glisser une activité <xref:System.Activities.Statements.WriteLine> à la fin du workflow et affectez à sa propriété <xref:System.Activities.Statements.WriteLine.Text%2A> la valeur "Client workflow ends." Le workflow du client terminé doit ressembler au diagramme suivant.  
  
     ![Flux de travail client complet](../../../../docs/framework/wcf/feature-details/media/clientcompleteworkflow.jpg "ClientCompleteWorkflow")  
  
16. Générez la solution.  
  
### Créer l'application Service  
  
1.  Ajoutez à la solution un nouveau projet d'application console nommé `Service`.Ajoutez des références aux assemblys suivants :  
  
    1.  System.Activities.dll  
  
    2.  System.ServiceModel.dll  
  
    3.  System.ServiceModel.Activities.dll  
  
2.  Ouvrez le fichier Program.cs généré et le code suivant :  
  
    ```  
    static void Main()  
          {  
              Console.WriteLine("Building the server.");  
              using (WorkflowServiceHost host = new WorkflowServiceHost(new DeclarativeServiceWorkflow(), new Uri("net.tcp://localhost:8000/TransactedReceiveService/Declarative")))  
              {                
                  //Start the server  
                  host.Open();  
                  Console.WriteLine("Service started.");  
  
                  Console.WriteLine();  
                  Console.ReadLine();  
                  //Shutdown  
                  host.Close();  
              };         
          }  
  
    ```  
  
3.  Ajoutez le fichier app.config suivant au projet.  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <!-- Copyright © Microsoft Corporation.  All rights reserved. -->  
    <configuration>  
        <system.serviceModel>  
            <bindings>  
                <netTcpBinding>  
                    <binding transactionFlow="true" />  
                </netTcpBinding>  
            </bindings>  
        </system.serviceModel>  
    </configuration>  
  
    ```  
  
### Créer l'application Client  
  
1.  Ajoutez à la solution un nouveau projet d'application console nommé `Client`.Ajoutez une référence à System.Activities.dll.  
  
2.  Ouvrez le fichier program.cs et ajoutez le code suivant.  
  
    ```  
    class Program  
        {  
  
            private static AutoResetEvent syncEvent = new AutoResetEvent(false);  
  
            static void Main(string[] args)  
            {  
                //Build client  
                Console.WriteLine("Building the client.");  
                WorkflowApplication client = new WorkflowApplication(new DeclarativeClientWorkflow());  
                client.Completed = Program.Completed;  
                client.Aborted = Program.Aborted;  
                client.OnUnhandledException = Program.OnUnhandledException;  
  
                //Wait for service to start  
                Console.WriteLine("Press ENTER once service is started.");  
                Console.ReadLine();  
  
                //Start the client              
                Console.WriteLine("Starting the client.");  
                client.Run();  
                syncEvent.WaitOne();  
  
                //Sample complete  
                Console.WriteLine();  
                Console.WriteLine("Client complete. Press ENTER to exit.");  
                Console.ReadLine();  
            }  
  
            private static void Completed(WorkflowApplicationCompletedEventArgs e)  
            {  
                Program.syncEvent.Set();  
            }  
  
            private static void Aborted(WorkflowApplicationAbortedEventArgs e)  
            {  
                Console.WriteLine("Client Aborted: {0}", e.Reason);  
                Program.syncEvent.Set();  
            }  
  
            private static UnhandledExceptionAction OnUnhandledException(WorkflowApplicationUnhandledExceptionEventArgs e)  
            {  
                Console.WriteLine("Client had an unhandled exception: {0}", e.UnhandledException);  
                return UnhandledExceptionAction.Cancel;  
            }  
        }  
  
    ```  
  
## Voir aussi  
 [Services de workflow](../../../../docs/framework/wcf/feature-details/workflow-services.md)   
 [Vue d'ensemble des transactions Windows Communication Foundation](../../../../docs/framework/wcf/feature-details/transactions-overview.md)   
 [Utilisation de TransactedReceiveScope](../../../../docs/framework/windows-workflow-foundation/samples/use-of-transactedreceivescope.md)