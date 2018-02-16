---
title: "Solución de problemas en la restauración de paquetes de NuGet en Visual Studio | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
description: "Descripción de errores habituales de restauración de NuGet en Visual Studio y cómo solucionarlos."
keywords: "Restauración de paquetes de NuGet, restauración de paquetes, solución de problemas, solucionar problemas"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: c0993e2585452e3c64da28d14bb1bbe1bea27768
ms.sourcegitcommit: b0af28d1c809c7e951b0817d306643fcc162a030
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2018
---
# <a name="troubleshooting-package-restore-errors-in-visual-studio"></a>Solucionar problemas de errores de restauración de paquetes en Visual Studio

> [!Note]
> Esta página se centra en los errores habituales al restaurar paquetes en Visual Studio y los pasos necesarios para resolverlos. Para ver procedimientos de restauración de paquetes, vea [Restauración de paquetes](../consume-packages/package-restore.md#enabling-and-disabling-package-restore).

De forma predeterminada, al crear un proyecto en Visual Studio, se restauran automáticamente los paquetes de NuGet a los que se hace referencia en el proyecto. Pero se producirán errores de compilación si se deshabilita la restauración de paquetes en **Herramientas > Opciones > Administrador de paquetes NuGet > Restauración de paquetes** y los paquetes necesarios no están disponibles en el equipo. En estos casos pueden aparecer los errores siguientes:

```output
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

```output
One or more NuGet packages need to be restored but couldn't be because consent has
not been granted. To give consent, open the Visual Studio Options dialog, click on
the NuGet Package Manager node and check 'Allow NuGet to download missing packages
during build.' You can also give consent by setting the environment variable
'EnableNuGetPackageRestore' to 'true'. Missing packages: {name} 
```

Para habilitar la restauración de paquetes, abra **Herramientas > Opciones > Administrador de paquetes NuGet** y seleccione las opciones de **Permitir a NuGet descargar los paquetes que falten** y **Comprobar automáticamente los paquetes que falten al compilar en Visual Studio**:

![habilitar la restauración de paquetes de NuGet en Herramientas/Opciones](../consume-packages/media/restore-01-autorestoreoptions.png)