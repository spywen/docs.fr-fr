---
title: "Résoudre les problèmes liés aux installations et désinstallations bloquées du .NET Framework"
ms.custom: 
ms.date: 05/26/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- .NET Framework, troubleshooting blocked installations
- blocked .NET Framework installations, troubleshooting
ms.assetid: c3fdfbc1-ed99-4202-a2b0-8c4f1646385d
caps.latest.revision: 57
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: c0d2d657a44c813fc94f8342ba1e6c33b330dfdb
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---

# <a name="troubleshoot-blocked-net-framework-installations-and-uninstallations"></a>Résoudre les problèmes liés aux installations et désinstallations bloquées du .NET Framework

Quand vous exécutez le [programme d’installation web ou hors connexion](../../../docs/framework/install/guide-for-developers.md) pour .NET Framework 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2 ou 4.7, vous pouvez rencontrer un problème qui empêche ou bloque l’installation du .NET Framework. Le tableau suivant répertorie les problèmes possibles et fournit des liens vers des informations de dépannage.

Dans Windows 8 et ultérieur, le .NET Framework est un composant du système d’exploitation qui ne peut pas être désinstallé séparément. Les mises à jour du .NET Framework s’affichent sous l’onglet **Mises à jour installées** de l’application **Programmes et fonctionnalités** du Panneau de configuration. Pour les systèmes d’exploitation sur lesquels il n’est pas préinstallé, le .NET Framework s’affiche sous l’onglet **Désinstaller ou modifier un programme** (ou l’onglet **Ajout/Suppression de programmes**) de l’application **Programmes et fonctionnalités** du Panneau de configuration. Pour obtenir des informations sur les versions Windows sur lesquelles le .NET Framework est préinstallé, consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md).

> [!IMPORTANT]
> Étant donné que les versions 4.x du .NET Framework sont mises à jour sur place, vous ne pouvez pas installer une version antérieure du .NET Framework 4.x sur un système qui a déjà une version ultérieure. Par exemple, sur un système Windows 10 Creators Update, vous ne pouvez pas installer le .NET Framework 4.6.2, puisque le .NET Framework 4.7 est préinstallé avec le système d’exploitation.

Vous pouvez déterminer quelles versions du .NET Framework sont installées sur un système. Pour plus d’informations, consultez [Guide pratique pour déterminer les versions .NET Framework installées](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).

Dans ce tableau, 4.5.*x* fait référence à .NET Framework 4.5 et ses versions intermédiaires 4.5.1 et 4.5.2 ; 4.6.*x* fait référence à .NET Framework 4.6 et ses versions intermédiaires 4.6.1 et 4.6.2 ; 4.7 fait référence à .NET Framework 4.7.

|Message bloquant|Pour plus d'informations ou pour résoudre le problème|  
|----------------------|--------------------------------------------------|  
|La désinstallation du Microsoft .NET Framework peut interrompre le fonctionnement de certaines applications.|En général, vous ne devez pas désinstaller les versions du .NET Framework installées sur votre ordinateur, car une application que vous utilisez peut dépendre d'une version spécifique du .NET Framework. Pour plus d’informations, consultez [.NET Framework pour les utilisateurs](../../../docs/framework/get-started/index.md#ForUsers) dans le guide *Prise en main*.|  
|Le .NET Framework 4.5*.x*/4.6*.x*/4.7 (FRA) ou une version ultérieure est déjà installé sur cet ordinateur.|Aucune action n'est nécessaire.<br /><br /> Pour déterminer quelles versions du .NET Framework sont installées sur un système, consultez [Guide pratique pour déterminer les versions .NET Framework installées](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).|  
|Le .NET Framework 4.5*.x*/4.6*.x*/4.7 (*langue*) nécessite .NET Framework 4.5*.x*/4.6*.x*. Installez .NET Framework 4.5*.x*/4.6*.x* à partir du Centre de téléchargement, puis réexécutez le programme d’installation.|Vous devez installer la version anglaise du .NET Framework spécifié avant d’installer un module linguistique. Pour plus d’informations, consultez la section [Pour installer les modules linguistiques](../../../docs/framework/install/guide-for-developers.md#to-install-language-packs) dans le guide d’installation.|  
|Impossible d’installer le .NET Framework 4.5*.x*/4.6*.x*/4.7. D'autres applications installées sur cet ordinateur ne sont pas compatibles avec ce programme.<br /><br /> ou<br /><br /> D'autres applications installées sur cet ordinateur ne sont pas compatibles avec ce programme.|La cause la plus probable de ce message est qu'une version d'évaluation ou RC du .NET Framework a été installée. Désinstallez la version d'évaluation ou RC et réexécutez le programme d'installation.|  
|Impossible de désinstaller le .NET Framework 4.5*.x*/4.6*.x*/4.7 en utilisant ce package. Pour désinstaller .NET Framework 4.5*.x*/4.6*.x* de votre ordinateur, ouvrez le **Panneau de configuration**, choisissez **Programmes et fonctionnalités**, **Afficher les mises à jour installées**, sélectionnez Mise à jour pour Microsoft Windows (KB2828152), puis choisissez **Désinstaller**.|Le package que vous installez ne désinstalle pas les versions d’évaluation ou RC du .NET Framework.<br /><br /> Désinstallez la version d’évaluation ou RC à partir du Panneau de configuration.|  
|Impossible de désinstaller .NET Framework 4.5*.x*/4.6*.x*/4.7. D'autres applications installées sur votre ordinateur dépendent de ce programme.|En général, vous ne devez pas désinstaller les versions du .NET Framework de votre ordinateur car une application que vous utilisez peut dépendre d'une version spécifique du .NET Framework. Pour plus d’informations, consultez [.NET Framework pour les utilisateurs](../../../docs/framework/get-started/index.md#ForUsers) dans le guide *Prise en main*.|  
|Le redistribuable du .NET Framework 4.5*.x*/4.6*.x*/4.7 ne s’applique pas à ce système d’exploitation. Téléchargez .NET Framework 4.5*.x*/4.6*.x* pour votre système d’exploitation à partir du Centre de téléchargement Microsoft.|Vous essayez peut-être d’installer [!INCLUDE[net_v451](../../../includes/net-v451-md.md)], 4.5.2, 4.6, 4.6.1, 4.6.2 ou 4.7 sur une plateforme qui n’est pas prise en charge, ou vous avez choisi le package d’installation qui n’inclut pas les composants pour tous les systèmes d’exploitation pris en charge. Réexécutez l’installation en utilisant le programme d’installation hors connexion ([pour 4.5.1](http://go.microsoft.com/fwlink/p/?LinkId=309493), [4.5.2](http://go.microsoft.com/fwlink/p/?LinkId=397706), [4.6](http://go.microsoft.com/fwlink/p/?LinkId=528233), [4.6.1](http://go.microsoft.com/fwlink/p/?LinkId=671744), [4.6.2](http://go.microsoft.com/fwlink/p/?LinkId=780604) ou pour [4.7](http://go.microsoft.com/fwlink/p/?LinkId=825306)). Pour plus d’informations, consultez le [guide d’installation](../../../docs/framework/install/guide-for-developers.md) et la [configuration requise](../../../docs/framework/get-started/system-requirements.md) pour connaître les systèmes d’exploitation pris en charge.|  
|La mise à jour correspondant à l’article de la Base de connaissances \<*numéro*> doit être installée avant de pouvoir installer ce produit.|L’installation du .NET Framework nécessite qu’une mise à jour de la Base de connaissances soit installée avant d’installer le .NET Framework. Installez la mise à jour et lancez de nouveau l’installation du .NET Framework.<br /><br /> Par exemple, l’installation de versions mises à jour du .NET Framework sur Windows 8.1, Windows RT 8.1 et Windows Server 2012 R2 nécessite que la mise à jour correspondant à [l’article de la Base de connaissances 2919355](https://support.microsoft.com/kb/2919355) soit installé.|  
|Votre ordinateur exécute actuellement une installation Server Core du système d'exploitation Windows Server 2008. Le .NET Framework 4.5.*x* nécessite une version plus récente du système d’exploitation. Installez Windows Server 2008 R2 SP1 ou ultérieur, puis réexécutez le programme d’installation du .NET Framework 4.5.*x*.|Les [!INCLUDE[net_v451](../../../includes/net-v451-md.md)] et 4.5.2 sont pris en charge dans le rôle principal du serveur avec Windows Server 2008 R2 SP1 ou une version ultérieure. Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md).|  
|Vous ne disposez pas de privilèges suffisants pour effectuer cette opération pour l'ensemble des utilisateurs de cet ordinateur. Ouvrez une session en tant qu’administrateur et réexécutez le **programme d’installation**.|Vous devez être administrateur de l'ordinateur pour installer le .NET Framework.|  
|Impossible de poursuivre l'installation, car une installation antérieure requiert le redémarrage de l'ordinateur. Redémarrez l'ordinateur et réexécutez le programme d'installation.|Un redémarrage est parfois nécessaire pour terminer complètement une installation. Suivez les instructions pour redémarrer votre ordinateur et réexécutez le programme d'installation.<br /><br /> Dans de rares cas, vous pouvez être invité à redémarrer plusieurs fois votre système si Windows a détecté un nombre de mises à jour manquantes et qu’un redémarrage est nécessaire pour installer la mise à jour suivante dans la file d’attente.|  
|Le programme d'installation du .NET Framework ne peut pas être exécuté en mode de compatibilité des programmes.|Consultez la section [Problèmes de compatibilité entre les programmes](#compat) plus loin dans cet article.|  
|Le .NET Framework 4.5*.x*/4.6*.x*/4.7 n’a pas été installé, car le magasin de composants a été endommagé.|Pour plus d’informations, consultez [Corriger les erreurs Windows Update à l’aide de DISM ou de l’outil d’analyse de l’installation conforme des mises à jour du système](https://support.microsoft.com/en-us/kb/947821).|  
|Impossible d'exécuter le programme d'installation car le service Windows Installer n'est pas disponible sur cet ordinateur.|Consultez [Erreur du service Windows Installer lorsque vous essayez d’installer ou de mettre à jour des programmes](http://go.microsoft.com/fwlink/p/?LinkId=248684) sur le site web du support technique de Microsoft.|  
|Le programme d'installation peut ne pas s'exécuter correctement car le service Windows Update n'est pas disponible sur cet ordinateur.|L'ordinateur peut être configuré pour utiliser Windows Server Update Services (WSUS) au lieu de Microsoft Windows Update. Pour plus d’informations, consultez la section relative au code d’erreur 0x800F0906 dans [Codes d’erreur lorsque vous essayez d’installer .NET Framework 3.5 dans Windows 8 ou Windows Server 2012](http://support.microsoft.com/kb/2734782).<br /><br /> Consultez également [Comment obtenir la version la plus récente de l’agent Windows Update de gestion des mises à jour](http://go.microsoft.com/fwlink/p/?LinkId=248437), disponible sur le site web du support technique de Microsoft.|  
|Le programme d'installation peut ne pas s'exécuter correctement car le service de transfert intelligent en arrière-plan n'est pas disponible sur cet ordinateur.|Consultez [Mise à jour visant à empêcher une panne du service de transfert intelligent en arrière-plan (BITS) sur un ordinateur Windows Vista](http://go.microsoft.com/fwlink/p/?LinkId=248680), disponible sur le site web du support technique de Microsoft.|  
|Le programme d’installation peut ne pas fonctionner correctement, car Windows Update a rencontré une erreur et affiche le code d’erreur 0x80070643 ou 0x643.|Consultez [Erreur d’installation de la mise à jour de .NET Framework : « 0x80070643 » ou « 0x643 »](https://support.microsoft.com/kb/976982) sur le site web du support technique de Microsoft.|  
|Le .NET Framework 4.5*.x*/4.6*.x*/4.7 fait déjà partie de ce système d’exploitation. Il n’est pas nécessaire d’installer le redistribuable .NET Framework 4.5*.x*/4.6*.x*/4.7.|Aucune action.<br /><br /> Pour déterminer quelles versions du .NET Framework sont installées sur un système, consultez [Guide pratique pour déterminer les versions .NET Framework installées](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md). Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md) pour connaître les systèmes d’exploitation pris en charge.|  
|Le .NET Framework 4.5*.x*/4.6*.x*/4.7 n’est pas pris en charge sur ce système d’exploitation.|Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md) pour connaître les systèmes d’exploitation pris en charge.<br /><br /> Pour les installations du .NET Framework sur Windows 7 qui ont échoué, ce message indique généralement que Windows 7 SP1 n’est pas installé. Sur les systèmes Windows 7, le .NET Framework nécessite Windows 7 SP1. Si vous êtes sur Windows 7 et que vous n’avez pas encore installé le Service Pack 1, vous devez le faire avant d’installer le .NET Framework. Pour plus d’informations sur l’installation de Windows 7 SP1, consultez [Installer Windows 7 Service Pack 1 (SP1)](http://windows.microsoft.com/en-us/windows7/install-windows-7-service-pack-1).|  
|Votre ordinateur exécute actuellement une installation minimale du système d'exploitation Windows Server 2008. Le .NET Framework 4.5.*x* nécessite une version complète du système d’exploitation ou Server Core 2008 R2 SP1. Installez la version complète de Windows Server 2008 SP2, Windows Server 2008 R2 SP1 ou Server Core 2008 R2 SP1 et réexécutez le programme d’installation du .NET Framework 4.5.*x*.|Le .NET Framework est pris en charge dans le rôle principal du serveur avec Windows Server 2008 R2 SP1 ou une version ultérieure. Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md).|  
|Le .NET Framework 4.5.*x* fait déjà partie de ce système d’exploitation, mais il est actuellement désactivé ([!INCLUDE[winserver8](../../../includes/winserver8-md.md)] uniquement).|Consultez la page [Activer ou désactiver des fonctionnalités Windows](http://go.microsoft.com/fwlink/p/?LinkId=248438) sur le site web de Windows.|  
|Impossible d'exécuter ce programme d'installation sur un ordinateur x64 ou IA64. Le programme d'installation requiert un ordinateur x86.|Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md) dans MSDN Library.|  
|Impossible d'exécuter ce programme d'installation sur un ordinateur IA64. Il ne peut pas être installé sur les ordinateurs IA64.|Consultez [Configuration requise](../../../docs/framework/get-started/system-requirements.md) dans MSDN Library.|  

<a name="compat"></a>
### <a name="program-compatibility-issues"></a>Problèmes de compatibilité entre les programmes

L’installation du .NET Framework 4.5 ou de ses versions intermédiaires échoue avec un code d’erreur 1603 ou se bloque quand elle est exécutée en mode de compatibilité des programmes Windows. L’**Assistant Compatibilité des programmes** signale que l’installation du .NET Framework semble ne pas s’être déroulée correctement. Il vous invite à le réinstaller en utilisant le paramètre recommandé (mode de compatibilité des programmes). Le mode de compatibilité des programmes peut également avoir été défini par l'Assistant Compatibilité des programmes lors de tentatives défectueuses ou annulées précédentes d'exécution du programme d'installation .NET Framework.

Le programme d'installation .NET Framework ne peut pas être exécuté en mode de compatibilité des programmes. Pour résoudre cette erreur majeure, vous devez vérifier que le paramètre de mode de compatibilité n'est pas activé au niveau du système dans l'Éditeur du Registre :

1. Choisissez le bouton **Démarrer**, puis sélectionnez **Exécuter**.

1. Dans la boîte de dialogue **Exécuter**, tapez « regedit » puis cliquez sur **OK**.

1. Dans l'Éditeur du Registre, parcourez les sous-clés suivantes :

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Persisted

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers

1. Dans la colonne Nom, recherchez les noms de téléchargement [!INCLUDE[net_v45](../../../includes/net-v45-md.md)], 4.5.1, 4.5.2, 4.6, 4.6.1 ou 4.6.2, selon la version que vous installez, puis supprimez ces entrées. Pour les noms des téléchargements, consultez l’article [Installer le .NET Framework pour les développeurs](../../../docs/framework/install/guide-for-developers.md).

1. Réexécutez le programme d’installation du .NET Framework pour la version 4.5, 4.5.1, 4.5.2, ou 4.6, .4.6.1, 4.6.2 ou 4.7.

## <a name="see-also"></a>Voir aussi

[Installer le .NET Framework pour les développeurs](../../../docs/framework/install/guide-for-developers.md)   
[Guide pratique pour déterminer les versions .NET Framework installées](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md)   
[Versions et dépendances](../../../docs/framework/migration-guide/versions-and-dependencies.md)

