---
title: "Exportation de sch&#233;mas &#224; partir de classes | Microsoft Docs"
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
  - "WCF, importation et exportation de schémas"
  - "schémas [WCF], exportation à partir de classes"
  - "schémas [WCF]"
  - "classe XsdDataContractExporter"
  - "Classe XsdDataContractImporter"
ms.assetid: bb57b962-70c1-45a9-93d5-e721e340a13f
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# Exportation de sch&#233;mas &#224; partir de classes
Pour générer des schémas XSD \(Schema definition language\) à partir des classes utilisées dans le modèle de contrat de données, utilisez la classe <xref:System.Runtime.Serialization.XsdDataContractExporter>. Cette rubrique décrit le processus de création de schémas.  
  
## Processus d'exportation  
 Le processus d'exportation de schéma démarre avec un ou plusieurs types et produit un <xref:System.Xml.Schema.XmlSchemaSet> qui décrit la projection XML de ces types.  
  
 Le `XmlSchemaSet` fait partie du modèle d'objet de schéma du [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] qui représente un jeu de documents de schéma XSD. Pour créer des documents XSD à partir d'un `XmlSchemaSet`, utilisez la collection de schémas de la propriété <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> de la classe `XmlSchemaSet`. Puis sérialisez chaque objet <xref:System.Xml.Schema.XmlSchema> à l'aide du <xref:System.Xml.Serialization.XmlSerializer>.  
  
#### Pour exporter des schémas  
  
1.  Créez une instance de <xref:System.Runtime.Serialization.XsdDataContractExporter>.  
  
2.  Facultatif. Passez un <xref:System.Xml.Schema.XmlSchemaSet> dans le constructeur. Dans ce cas, le schéma généré pendant l'exportation de schéma est ajouté à cette instance <xref:System.Xml.Schema.XmlSchemaSet> au lieu de démarrer avec un <xref:System.Xml.Schema.XmlSchemaSet> vide.  
  
3.  Facultatif. Appelez une des méthodes <xref:System.Runtime.Serialization.XsdDataContractExporter.CanExport%2A>. La méthode détermine si le type spécifié peut être exporté. La méthode a les mêmes surcharges que la méthode `Export` dans l'étape suivante.  
  
4.  Appelez une des méthodes <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A>. Il y a trois surcharges qui prennent un <xref:System.Type>, un <xref:System.Collections.Generic.List%601> d'objets `Type`, ou un <xref:System.Collections.Generic.List%601> d'objets <xref:System.Reflection.Assembly>. Dans le dernier cas, tous les types dans tous les assemblys donnés sont exportés.  
  
     Plusieurs appels à la méthode `Export` entraînent l'ajout de plusieurs éléments au même `XmlSchemaSet`. Un type n'est pas généré dans le `XmlSchemaSet` s'il est déjà présent. Par conséquent, il est préférable d'appeler `Export`  plusieurs fois sur le même `XsdDataContractExporter` que de créer plusieurs instances de la classe `XsdDataContractExporter`. Cela évite de générer plusieurs types de schémas en double.  
  
    > [!NOTE]
    >  En cas de défaillance pendant l'exportation, le `XmlSchemaSet` sera dans un état imprévisible.  
  
5.  Accédez à <xref:System.Xml.Schema.XmlSchemaSet> à l'aide de la propriété <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A>.  
  
## Options d'exportation  
 Vous pouvez définir la propriété <xref:System.Runtime.Serialization.XsdDataContractExporter.Options%2A> de <xref:System.Runtime.Serialization.XsdDataContractExporter> avec une instance de la classe <xref:System.Runtime.Serialization.ExportOptions> pour contrôler divers aspects du processus d'exportation. Spécifiquement, vous pouvez définir les options suivantes :  
  
-   <xref:System.Runtime.Serialization.ExportOptions.KnownTypes%2A>. Cette collection de `Type` représente les types connus pour les types qui sont exportés. \([!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Types connus de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).\) Ces types connus sont exportés sur chaque appel `Export` en plus des types passés à la méthode `Export`.  
  
-   <xref:System.Runtime.Serialization.ExportOptions.DataContractSurrogate%2A>. Un <xref:System.Runtime.Serialization.IDataContractSurrogate> peut être fourni par l'intermédiaire de cette propriété qui personnalisera le processus d'exportation.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Substituts de contrats de données](../../../../docs/framework/wcf/extending/data-contract-surrogates.md). Par défaut, aucun substitut n'est utilisé.  
  
## Méthodes d'assistance  
 En plus de son rôle principal chargé d'exporter le schéma, le `XsdDataContractExporter` offre plusieurs méthodes d'assistance utiles qui fournissent des informations relatives aux types. Elles incluent notamment :  
  
-   Méthode <xref:System.Runtime.Serialization.XsdDataContractExporter.GetRootElementName%2A>. Cette méthode prend un `Type` et retourne un <xref:System.Xml.XmlQualifiedName> qui représente le nom d'élément racine et l'espace de noms qui seraient utilisés si ce type était sérialisé comme objet racine.  
  
-   Méthode <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaTypeName%2A>. Cette méthode prend un `Type` et retourne un <xref:System.Xml.XmlQualifiedName> qui représente le nom du type de schéma XSD qui serait utilisé si ce type était exporté dans le schéma. Pour les types <xref:System.Xml.Serialization.IXmlSerializable> représentés comme types anonymes dans le schéma, cette méthode retourne `null`.  
  
-   Méthode <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaType%2A>. Cette méthode fonctionne uniquement avec les types <xref:System.Xml.Serialization.IXmlSerializable> représentés comme types anonymes dans le schéma et retourne `null` pour tous les autres types. Pour les types anonymes, cette méthode retourne un <xref:System.Xml.Schema.XmlSchemaType> qui représente un `Type` donné.  
  
 Les options d'exportation affectent toutes ces méthodes.  
  
## Voir aussi  
 <xref:System.Runtime.Serialization.DataContractSerializer>   
 <xref:System.Runtime.Serialization.XsdDataContractImporter>   
 <xref:System.Runtime.Serialization.XsdDataContractExporter>   
 [Importation et exportation de schémas](../../../../docs/framework/wcf/feature-details/schema-import-and-export.md)   
 [Importation du schéma pour générer des classes](../../../../docs/framework/wcf/feature-details/importing-schema-to-generate-classes.md)