---
title: "Probl&#232;mes de s&#233;curit&#233; relatifs &#224; la journalisation des messages | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 21f513f2-815b-47f3-85a6-03c008510038
caps.latest.revision: 17
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 17
---
# Probl&#232;mes de s&#233;curit&#233; relatifs &#224; la journalisation des messages
Cette rubrique décrit comment empêcher l'exposition des données sensibles dans les journaux de message, ainsi que des événements générés par la journalisation des messages.  
  
## Problèmes de sécurité  
  
### Enregistrement des informations sensibles  
 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] ne modifie pas les données contenues dans le corps et les en\-têtes spécifiques aux applications.[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] n'effectue pas non plus le suivi des informations personnelles présentes dans les en\-têtes spécifiques à l'application ou les données relatives au corps.  
  
 Lorsque la journalisation des messages est activée, les informations personnelles contenues dans les en\-têtes spécifiques aux applications \(par exemple, une chaîne de requête\) et les données relatives au corps \(par exemple, un numéro de carte de crédit\) peuvent être visibles dans les journaux.Le responsable du déploiement d'applications est chargé d'appliquer le contrôle d'accès sur les fichiers journaux et de configuration.Si vous ne souhaitez pas que ce type d'informations soit visible, vous devez désactiver la journalisation, ou filtrer une partie des données en cas de partage des journaux.  
  
 Les conseils suivants vous permettent d'éviter l'exposition involontaire du contenu d'un fichier journal :  
  
-   Assurez\-vous que les fichiers journaux sont protégés par des listes de contrôle d'accès \(ACL, Access Control Lists\), à la fois dans les scénarios auto\-hébergés et hébergés sur le Web.  
  
-   Choisissez une extension de fichier qui ne peut pas être facilement fournie à l'aide d'une demande Web.Par exemple, l'extension de fichier .xml n'est pas un choix judicieux.Consultez le guide d'administration IIS \(Internet Information Services\) pour obtenir la liste des extensions qui peuvent être fournies.  
  
-   Spécifiez un chemin d'accès absolu pour l'emplacement de fichier journal, qui doit se trouver hors du répertoire public de la racine virtuelle de l'hôte Web afin d'empêcher tout intervenant externe d'y accéder à l'aide d'un navigateur Web.  
  
 Par défaut, les clés et informations personnelles, telles que le nom d'utilisateur et le mot de passe, ne sont pas enregistrées dans les traces et les messages consignés.Toutefois, un administrateur d'ordinateur peut utiliser l'attribut `enableLoggingKnownPII` dans l'élément `machineSettings` du fichier Machine.config pour autoriser les applications s'exécutant sur l'ordinateur à enregistrer les informations personnelles connues.La configuration suivante montre comment procéder :  
  
```  
<configuration>  
   <system.serviceModel>  
      <machineSettings enableLoggingKnownPii="true"/>  
   </system.serviceModel>  
</configuration>   
```  
  
 Un responsable du déploiement d'applications peut ensuite utiliser l'attribut `logKnownPii` dans le fichier App.config ou Web.config pour activer la journalisation PII comme suit :  
  
```  
<system.diagnostics>  
  <sources>  
      <source name="System.ServiceModel.MessageLogging"  
        logKnownPii="true">  
        <listeners>  
                 <add name="messages"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="c:\logs\messages.svclog" />  
          </listeners>  
      </source>  
    </sources>  
</system.diagnostics>  
```  
  
 La journalisation PII est uniquement activée lorsque les deux paramètres ont la valeur `true`.La combinaison de deux commutateurs offre la souplesse nécessaire pour enregistrer les informations personnelles connues pour chaque application.  
  
> [!IMPORTANT]
>  Dans [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)], les indicateurs `logEntireMessage` et `logKnownPii` doivent également être définis avec la valeur `true` dans le fichier Web.config ou App.config pour activer la journalisation PII, comme indiqué dans l'exemple suivant `<system.serviceModel><messageLogging logEntireMessage="true" logKnownPii="true" …`.  
  
 Sachez que si vous spécifiez plusieurs sources personnalisées dans un fichier de configuration, seuls les attributs de la première sont lus.Les autres sont ignorés.Cela signifie que, pour le fichier App.config suivant, les informations personnelles ne sont pas enregistrées pour les deux sources, même si leur journalisation est explicitement activée pour la deuxième source.  
  
```  
<system.diagnostics>  
   <sources>  
      <source name="System.ServiceModel.MessageLogging"  
              logKnownPii="false">  
              <listeners>  
                 <add name="messages"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\messages.svclog" />  
              </listeners>  
            </source>  
      <source name="System.ServiceModel"   
              logKnownPii="true">  
              <listeners>  
                 <add name="traces"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\traces.svclog" />  
              </listeners>  
      </source>  
   </sources>  
</system.diagnostics>  
```  
  
 Si l'élément `<machineSettings enableLoggingKnownPii="Boolean"/>` existe en dehors du fichier Machine.config, le système lève un <xref:System.Configuration.ConfigurationErrorsException>.  
  
 Les modifications ne sont effectives qu'au démarrage ou redémarrage de l'application.Un événement est enregistré au démarrage lorsque les deux attributs ont la valeur `true`.Un événement est également enregistré si `logKnownPii` a la valeur `true` mais que `enableLoggingKnownPii` a la valeur `false`.  
  
 L'administrateur d'ordinateur et le responsable du déploiement d'applications doivent observer la plus grande prudence lorsqu'ils utilisent ces deux commutateurs.Si la journalisation PII est activée, les clés de sécurité et les informations personnelles sont enregistrées.Si elle est désactivée, les données sensibles et spécifiques aux applications sont toujours enregistrées dans les corps et en\-têtes des messages.Pour plus d'informations sur la confidentialité et les mesures à observer pour empêcher l'exposition des données personnelles, consultez [Confidentialité de l'utilisateur](http://go.microsoft.com/fwlink/?LinkID=94647) \(page éventuellement en anglais\).  
  
> [!CAUTION]
>  Les informations personnelles ne sont pas masquées dans les messages malformés.Les messages de ce type sont consignés en l'état sans aucune modification.Les attributs précédemment mentionnés n'ont aucun effet sur cela.  
  
### Écouteur de suivi personnalisé  
 L'ajout d'un écouteur de suivi personnalisé sur la source de suivi de journalisation des messages est un privilège qui doit être réservé à l'administrateur.En effet, des écouteurs personnalisés malveillants peuvent être configurés pour envoyer des messages à distance, ce qui se traduit par la divulgation d'informations sensibles.De plus, si vous configurez un écouteur personnalisé pour envoyer des messages sur le câble, par exemple vers une base de données distante, vous devez appliquer le contrôle d'accès approprié sur les journaux de messages de l'ordinateur distant.  
  
## Événements déclenchés par la journalisation des messages  
 La section suivante répertorie tous les événements émis par la journalisation des messages.  
  
-   Journalisation des messages activée : cet événement est émis lorsque la journalisation des messages est activée dans la configuration ou via WMI.Le contenu de l'événement est "La journalisation des messages a été activée.Des informations sensibles peuvent être enregistrées en clair, même si elles ont été chiffrées sur le câble, par exemple, corps de message."  
  
-   Journalisation des messages désactivée : cet événement est émis lorsque la journalisation des messages est désactivée via WMI.Le contenu de l'événement est "La journalisation des messages a été activée."  
  
-   Enregistrement des informations personnelles connues activé : cet événement est émis lorsque l'enregistrement des données personnelles connues est activé.Cet événement se produit lorsque l'attribut `enableLoggingKnownPii`  de l'élément `machineSettings` du fichier Machine.config a la valeur `true` et que l'attribut `logKnownPii` de l'élément `source` du fichier App.config ou Web.config à la valeur `true`.  
  
-   Enregistrement des informations personnelles connues activé non autorisé : cet événement est émis lorsque l'enregistrement des informations personnelles connues n'est pas autorisé.Cet événement se produit lorsque l'attribut `logKnownPii` de l'élément `source` du fichier App.config ou Web.config à la valeur `true`, mais que l'attribut `enableLoggingKnownPii`  de l'élément `machineSettings` du fichier Machine.config à la valeur `false`.Aucune exception n'est levée.  
  
 Ces événements peuvent être affichés dans l'outil Observateur d'événements fourni avec Windows.Pour plus d'informations, consultez [Journalisation des événements](../../../../docs/framework/wcf/diagnostics/event-logging/index.md).  
  
## Voir aussi  
 [Enregistrement des messages](../../../../docs/framework/wcf/diagnostics/message-logging.md)   
 [Problèmes de sécurité et conseils utiles pour le suivi](../../../../docs/framework/wcf/diagnostics/tracing/security-concerns-and-useful-tips-for-tracing.md)