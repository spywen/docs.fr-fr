---
title: Types de base | Guide C#
description: "Découvrez les types de base (numérique, chaîne et objet) disponibles dans tous les programmes C#"
keywords: .NET, .NET Core, C#
author: stevehoag
ms.author: shoag
ms.date: 10/10/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 57c5524a18535778db337500b0a7e9013ce93519
ms.lasthandoff: 03/13/2017

---

# <a name="types-variables-and-values"></a>Types, variables et valeurs  
C# est un langage fortement typé. Chaque variable et chaque constante a un type, tout comme chaque expression qui fournit une valeur. Chaque signature de méthode spécifie un type pour chaque paramètre d’entrée et pour la valeur de retour. La bibliothèque de classes .NET Framework définit un ensemble de types numériques intégrés, ainsi que des types plus complexes qui représentent un large éventail de constructions logiques, telles que le système de fichiers, les connexions réseau, les collections et tableaux d’objets, et les dates. Un programme C# classique utilise des types fournis dans la bibliothèque de classes, mais aussi des types définis par l’utilisateur qui modélisent les concepts propres au domaine du problème du programme.  
  
Un type peut stocker les informations suivantes :  
  
-   Espace de stockage nécessaire pour une variable du type.  
  
-   Valeurs minimale et maximale que le type peut représenter.  
  
-   Membres (méthodes, champs, événements, etc.) que le type contient.  
  
-   Type de base dont le type est hérité.  
  
-   Emplacement où sera allouée la mémoire pour les variables au moment de l’exécution.  
  
-   Sortes d’opérations autorisées.  
  
Le compilateur utilise les informations de type pour s’assurer que toutes les opérations qui sont effectuées dans votre code sont de *type safe*. Par exemple, si vous déclarez une variable de type [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx), le compilateur vous permet d’utiliser la variable dans des opérations d’addition et de soustraction. Si vous essayez d’effectuer ces mêmes opérations avec une variable de type [bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx), le compilateur génère une erreur, comme illustré dans l’exemple suivant :  
  
[!code-csharp[Cohérence des types](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
>  Si vous développez habituellement du code dans C ou C++, notez que dans C#, le type [bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx) n’est pas convertible en [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx).  
  
Le compilateur incorpore les informations de type dans le fichier exécutable sous forme de métadonnées. Le common language runtime (CLR) utilise ces métadonnées au moment de l’exécution pour garantir la cohérence des types quand il alloue et libère de la mémoire.  

## <a name="specifying-types-in-variable-declarations"></a>Spécification de types dans les déclarations de variable  
Quand vous déclarez une variable ou une constante dans un programme, vous devez spécifier son type ou utiliser le mot clé [var](https://msdn.microsoft.com/library/bb383973.aspx) pour permettre au compilateur de déduire le type. L’exemple suivant montre des déclarations de variable qui utilisent à la fois des types numériques intégrés et des types complexes définis par l’utilisateur :  
  
[!code-csharp[Déclaration de variable](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
Les types des paramètres de méthode et des valeurs de retour sont spécifiés dans la signature de méthode. La signature suivante présente une méthode qui nécessite l’argument d’entrée [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) et retourne une chaîne :  
  
[!code-csharp[Signature de méthode](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
Une fois qu’elle a été déclarée, une variable ne peut pas être redéclarée avec un nouveau type, et elle ne peut pas être définie avec une valeur qui n’est pas compatible avec son type déclaré. Par exemple, vous ne pouvez pas déclarer une variable de type [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) et lui assigner la valeur booléenne [true](https://msdn.microsoft.com/library/06d3w013.aspx). Toutefois, les valeurs peuvent être converties en d’autres types, par exemple, quand elles sont assignées à de nouvelles variables ou passées comme arguments de méthode. Une *conversion de type* qui ne cause pas de perte de données est effectuée automatiquement par le compilateur. Une conversion susceptible de causer la perte de données nécessite un *cast* dans le code source. 

Pour plus d’informations, consultez [Cast et conversions de types](https://msdn.microsoft.com/library/ms173105.aspx).
 
## <a name="built-in-types"></a>Types intégrés
C# fournit un jeu standard de types numériques intégrés pour représenter les nombres entiers, les valeurs à virgule flottante, les expressions booléennes, les caractères de texte, les valeurs décimales et d’autres types de données. Il existe également des types **string** et **object** intégrés, que vous pouvez utiliser dans n’importe quel programme C#. Pour plus d’informations sur les types intégrés, consultez [Tableaux de référence des types](https://msdn.microsoft.com/library/1dhd7f2x.aspx).  
  
## <a name="custom-types"></a>Types personnalisés  
Utilisez des constructions [struct](https://msdn.microsoft.com/library/ah19swz4.aspx), [class](https://msdn.microsoft.com/library/0b0thckt.aspx), [interface](https://msdn.microsoft.com/library/87d83y5b.aspx) et [enum](https://msdn.microsoft.com/library/sbbt4032.aspx) pour créer vos propres types personnalisés. La bibliothèque de classes .NET Framework est une collection de types personnalisés fournie par Microsoft que vous pouvez utiliser dans vos propres applications. Par défaut, les types les plus fréquemment utilisés dans la bibliothèque de classes sont disponibles dans tous les programmes C#. Les autres types sont disponibles uniquement si vous ajoutez explicitement une référence de projet à l’assembly dans lequel ils sont définis. Une fois que le compilateur a une référence à l’assembly, vous pouvez déclarer des variables (et des constantes) des types qui ont été déclarés dans cet assembly dans le code source. Pour plus d’informations, consultez [Bibliothèque de classes .NET Framework](https://msdn.microsoft.com/library/gg145045(v=vs.110).aspx).  
  
## <a name="generic-types"></a>Types génériques  
Un type peut être déclaré avec un ou plusieurs *paramètres de type* qui servent d’espace réservé pour le type réel (le *type concret*) que le code client fournit quand il crée une instance du type. Ces types sont appelés *types génériques*. Par exemple, le type .NET Framework @System.Collections.Generic.List%601 a un paramètre de type qui, par convention, porte le nom *T*. Quand vous créez une instance du type, vous spécifiez le type des objets que la liste doit contenir, par exemple, le type string pour des chaînes :  
  
[!code-csharp[Types génériques](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)] 
  
L’utilisation du paramètre de type rend possible la réutilisation de la même classe pour contenir tout type d’élément, sans avoir à convertir chaque élément en type [object](https://msdn.microsoft.com/library/9kkx3h3c.aspx). Les classes de collections génériques sont appelées *collections fortement typées*, car le compilateur connaît le type spécifique des éléments de chaque collection et il peut déclencher une erreur au moment de la compilation. C’est le cas, par exemple, si vous essayez d’ajouter un entier à l’objet `strings` dans l’exemple précédent. Pour plus d’informations, consultez [Génériques](programming-guide/generics/index.md). 

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>Types implicites, types anonymes et types tuple  
Comme indiqué précédemment, vous pouvez assigner implicitement un type à une variable locale (mais pas les membres de la classe) à l’aide du mot clé [var](https://msdn.microsoft.com/library/bb383973.aspx). La variable reçoit toujours un type au moment de la compilation, mais le type est fourni par le compilateur. Pour plus d’informations, consultez [Variables locales implicitement typées](https://msdn.microsoft.com/library/bb384061.aspx).  
  
Dans certains cas, il est difficile de créer un type nommé pour des ensembles simples de valeurs associées que vous ne souhaitez pas stocker ou passer hors de la portée de la méthode. Vous pouvez alors créer des *types anonymes*. Pour plus d’informations, consultez [Types anonymes](https://msdn.microsoft.com/library/bb397696.aspx).

Il est courant de vouloir retourner plusieurs valeurs d’une méthode. Vous pouvez créer des *types tuple* qui retournent plusieurs valeurs dans un seul appel de méthode. Pour plus d’informations, consultez [Tuples](tuples.md)

## <a name="the-common-type-system"></a>Système de type commun (CTS)  
Il est important de comprendre ces deux points fondamentaux à propos du système de type dans le .NET Framework :  
  
-   Le système prend en charge le principe d’héritage. Les types peuvent dériver d’autres types, appelés *types de base*. Le type dérivé hérite (avec certaines restrictions) des méthodes, des propriétés et des autres membres du type de base. Le type de base peut, à son tour, dériver d’un autre type, auquel cas le type dérivé hérite des membres des deux types de base dans sa hiérarchie d’héritage. Tous les types, y compris les types numériques intégrés tels que @System.Int32 (mot clé C# : `int`), dérivent au final d’un seul type de base, qui est @System.Object (mot clé C# : `object`). Cette hiérarchie de types unifiée est appelée [Système de type commun](../standard/common-type-system.md) (CTS). Pour plus d’informations sur l’héritage dans C#, consultez [Héritage](https://msdn.microsoft.com/library/ms173149.aspx).  
  
-   Chaque type du CTS est défini comme *type valeur* ou *type référence*. Cela inclut tous les types personnalisés fournis dans la bibliothèque de classes .NET Framework, ainsi que les types définis par l’utilisateur. Les types que vous définissez à l’aide du mot clé [struct](https://msdn.microsoft.com/library/ah19swz4.aspx) sont des types valeur ; tous les types numériques intégrés sont des **structs**. Pour plus d’informations sur les types valeur, consultez [Structs](structs.md). Les types que vous définissez à l’aide du mot clé [class](https://msdn.microsoft.com/library/0b0thckt.aspx) sont des types référence. Pour plus d’informations sur les types référence, consultez [Classes](classes.md). Les types référence et les types valeur ne suivent pas les mêmes règles de compilation et ont un comportement différent au moment de l’exécution.
 
  
## <a name="see-also"></a>Voir aussi
[Structures](structs.md)

[Classes](classes.md)
