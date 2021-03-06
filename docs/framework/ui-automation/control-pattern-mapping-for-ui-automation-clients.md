---
title: "Control Pattern Mapping for UI Automation Clients | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-bcl"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "control patterns, for UI Automation clients"
  - "UI Automation, clients, control patterns for"
ms.assetid: 8b81645b-8be3-4e26-9c98-4fb0fceca06b
caps.latest.revision: 18
author: "Xansky"
ms.author: "mhopkins"
manager: "markl"
caps.handback.revision: 18
---
# Control Pattern Mapping for UI Automation Clients
> [!NOTE]
>  Cette documentation s'adresse aux développeurs .NET Framework qui souhaitent utiliser les classes [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] managées définies dans l'espace de noms <xref:System.Windows.Automation>. Pour obtenir les dernières informations sur [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consultez [API Windows Automation : UI Automation](http://go.microsoft.com/fwlink/?LinkID=156746).  
  
 Cette rubrique répertorie les types de contrôle et leurs modèles de contrôle associés.  
  
 Le tableau suivant organise les modèles de contrôle dans les catégories suivantes :  
  
-   Prise en charge. Le contrôle doit prendre en charge ce modèle de contrôle.  
  
-   Prise en charge conditionnelle. Le contrôle peut prendre en charge ce modèle de contrôle en fonction de l’état du contrôle.  
  
-   Non prise en charge. Le contrôle ne prend pas en charge ce modèle de contrôle. Les contrôles personnalisés peuvent prendre en charge ce modèle de contrôle.  
  
> [!NOTE]
>  Certains contrôles disposent d’une prise en charge conditionnelle pour plusieurs modèles de contrôle selon les fonctionnalités du contrôle. Par exemple, le contrôle d’élément de menu dispose d’une prise en charge conditionnelle pour le modèle de contrôle <xref:System.Windows.Automation.InvokePattern>, <xref:System.Windows.Automation.ExpandCollapsePattern>, <xref:System.Windows.Automation.TogglePattern> ou <xref:System.Windows.Automation.SelectionItemPattern>, selon sa fonction dans le contrôle de menu.  
  
<a name="control_mapping_clients"></a>   
## Modèles de contrôle UI Automation pour les clients  
  
|Type de contrôle|Pris en charge|Prise en charge conditionnelle|Non prise en charge|  
|----------------------|--------------------|------------------------------------|-------------------------|  
|Bouton|None|Invoke, Toggle, Expand Collapse|None|  
|Calendrier|Grid, Table|Selection, Scroll|Valeur|  
|Case à cocher|Basculer|None|None|  
|Combo Box|Développer\/Réduire|Selection, Value|Scroll|  
|DataGrid|Grille|Scroll, Selection, Table|None|  
|DataItem|Selection Item|Expand Collapse, Grid Item, Scroll Item, Table, Toggle, Value|None|  
|Document|Texte|Scroll, Value|None|  
|Modifier|None|Text, Range Value, Value|None|  
|Regrouper|None|Développer\/Réduire|None|  
|En\-tête|None|Transformer|None|  
|HeaderItem|None|Transform, Invoke|None|  
|Lien hypertexte|Appeler|Valeur|None|  
|Image|None|Grid Item, Table Item|Invoke, Selection Item|  
|Liste|None|Grid, Multiple View, Scroll, Selection|Table|  
|List Item|Selection Item|Expand Collapse, Grid Item, Invoke, Scroll Item, Toggle, Value|None|  
|Menu|None|None|None|  
|Barre de menus|None|Expand Collapse, Dock, Transform|None|  
|Élément de menu|None|Expand Collapse, Invoke, Selection Item, Toggle|None|  
|Volet|None|Dock. Scroll, Transform|Fenêtre|  
|ProgressBar|None|Range Value, Value|None|  
|RadioButton|Selection Item|None|Basculer|  
|Scroll Bar|None|Range Value|Scroll|  
|Séparateur|None|None|None|  
|Curseur|None|Range Value, Selection, Value|None|  
|Spinner|None|Range Value, Selection, Value|None|  
|Bouton partagé|Invoke, Expand Collapse|None|None|  
|Barre d'état|None|Grille|None|  
|Onglet|Sélection|Scroll|None|  
|TabItem|Selection Item|None|Appeler|  
|Table|Grid, Grid Item, Table, Table Item|None|None|  
|Texte|None|Grid Item, Table Item, Text|Valeur|  
|Thumb|Transformer|None|None|  
|Barre de titre|None|None|None|  
|Tool Bar|None|Dock, Expand Collapse, Transform|None|  
|Tool Tip|None|Text, Window|None|  
|Arborescence|None|Scroll, Selection|None|  
|TreeItem|Développer\/Réduire|Invoke, Scroll Item, Selection Item, Toggle|None|  
|Fenêtre|Transform, Window|Station d' accueil|None|  
  
> [!NOTE]
>  Si un type de contrôle ne possède aucun modèle de contrôle pris en charge répertorié, mais possède un ou plusieurs modèles de contrôle pris en charge de manière conditionnelle, l’un de ces modèles de contrôle conditionnels est constamment pris en charge.  
  
## Voir aussi  
 [UI Automation Overview](../../../docs/framework/ui-automation/ui-automation-overview.md)