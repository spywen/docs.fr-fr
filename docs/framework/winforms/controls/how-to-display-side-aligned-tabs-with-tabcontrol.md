---
title: "Comment&#160;: afficher des onglets align&#233;s sur le c&#244;t&#233; &#224; l&#39;aide de TabControl | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "pages d'onglets, afficher des onglets alignés sur le côté"
  - "TabControl (contrôle Windows Forms), afficher des onglets alignés sur le côté"
  - "onglets, afficher des onglets alignés sur le côté"
ms.assetid: 110d5abd-3ae3-4ded-95bf-778aaac798a0
caps.latest.revision: 10
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 10
---
# Comment&#160;: afficher des onglets align&#233;s sur le c&#244;t&#233; &#224; l&#39;aide de TabControl
La propriété <xref:System.Windows.Forms.TabControl.Alignment%2A> de <xref:System.Windows.Forms.TabControl> prend en charge l'affichage vertical des onglets \(le long du bord gauche ou droit du contrôle\), par opposition à l'affichage horizontal \(en haut ou en bas du contrôle\).  Par défaut, cet affichage vertical offre une expérience utilisateur médiocre, car la propriété <xref:System.Windows.Forms.TabPage.Text%2A> de l'objet <xref:System.Windows.Forms.TabPage> ne s'affiche pas dans l'onglet quand les styles visuels sont activés.  De plus, il n'existe aucun moyen direct de contrôler la direction du texte dans l'onglet.  Vous pouvez utiliser Owner Draw sur <xref:System.Windows.Forms.TabControl> pour améliorer cette expérience.  
  
 La procédure suivante montre comment afficher des onglets alignés à droite, avec le texte de l'onglet affiché de gauche à droite, à l'aide de la fonctionnalité « Owner Draw ».  
  
### Pour afficher des onglets alignés à droite  
  
1.  Ajoutez un <xref:System.Windows.Forms.TabControl> à votre formulaire.  
  
2.  Affectez à la propriété <xref:System.Windows.Forms.TabControl.Alignment%2A> la valeur <xref:System.Windows.Forms.TabAlignment>.  
  
3.  Affectez à la propriété <xref:System.Windows.Forms.TabControl.SizeMode%2A> la valeur <xref:System.Windows.Forms.TabSizeMode> pour que tous les onglets aient la même largeur.  
  
4.  Affectez à la propriété <xref:System.Windows.Forms.TabControl.ItemSize%2A> la taille fixe préférée pour les onglets.  N'oubliez pas que la propriété <xref:System.Windows.Forms.TabControl.ItemSize%2A> se comporte comme si les onglets étaient en haut, bien qu'ils soient alignés à droite.  Ainsi, pour que les onglets soient plus larges vous devez modifier la propriété <xref:System.Drawing.Size.Height%2A>, et pour qu'ils soient plus hauts vous devez modifier la propriété <xref:System.Drawing.Size.Width%2A>.  
  
     Pour un résultat optimal avec l'exemple de code ci\-dessous, affectez la valeur 25 à la propriété <xref:System.Drawing.Size.Width%2A> et la valeur 100 à la propriété <xref:System.Drawing.Size.Height%2A>.  
  
5.  Affectez à la propriété <xref:System.Windows.Forms.TabControl.DrawMode%2A> la valeur <xref:System.Windows.Forms.TabDrawMode>.  
  
6.  Définissez un gestionnaire pour l'événement <xref:System.Windows.Forms.TabControl.DrawItem> de <xref:System.Windows.Forms.TabControl> qui affiche le texte de gauche à droite.  
  
     [!code-csharp[TabControl.RightAlignedTabs#1](../../../../samples/snippets/csharp/VS_Snippets_Winforms/TabControl.RightAlignedTabs/CS/Form1.cs#1)]
     [!code-vb[TabControl.RightAlignedTabs#1](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/TabControl.RightAlignedTabs/VB/Form1.vb#1)]  
  
## Voir aussi  
 [TabControl, contrôle](../../../../docs/framework/winforms/controls/tabcontrol-control-windows-forms.md)