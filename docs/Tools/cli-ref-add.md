---
title: NuGet CLI Agregar comando | Documentos de Microsoft
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/18/2018
ms.topic: reference
ms.prod: nuget
ms.technology: 
description: Referencia para el nuget.exe agregar (comando)
keywords: NuGet Agregar referencia, agregue el comando del paquete
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 70c86f8d240bd308224f6b7887b630cc1e953bf8
ms.sourcegitcommit: 262d026beeffd4f3b6fc47d780a2f701451663a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2018
---
# <a name="add-command-nuget-cli"></a>Agregar comando (NuGet CLI)

**Se aplica a**: paquete de publicación &bullet; **versiones compatibles**: 3.3 +

Agrega un paquete específico a un origen de paquete no son HTTP (una carpeta o ruta de acceso UNC) en un formato jerárquico y en el que se crean las carpetas para el número de identificador y la versión del paquete. Por ejemplo:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          ├─<packageID>.<version>.nupkg.sha512
          └─<packageID>.nuspec

Al restaurar o actualizar en el origen del paquete, formato jerárquico proporciona un rendimiento significativamente mejor.

Para expandir todos los archivos en el paquete en el origen del paquete de destino, use el `-Expand` cambiar. Normalmente esto tiene como resultado en subcarpetas adicionales que aparecen en el destino, como `tools` y `lib`.

## <a name="usage"></a>Uso

```cli
nuget add <packagePath> -Source <sourcePath> [options]
```

donde `<packagePath>` es la ruta de acceso al paquete que se agregará y `<sourcePath>` especifica el origen del paquete basada en la carpeta a la que se agregará el paquete. No se admiten orígenes HTTP.

## <a name="options"></a>Opciones

| Opción | Descripción |
| --- | --- |
| ConfigFile | El archivo de configuración de NuGet para aplicar. Si no se especifica, *%AppData%\NuGet\NuGet.Config* se utiliza.| 
| Expand | Agrega todos los archivos en el paquete en el origen del paquete. |
| ForceEnglishOutput | *(3.5 +)*  Fuerza nuget.exe ejecutándose con una referencia cultural invariable, basados en el inglés. |
| Ayuda | Muestra información de ayuda para el comando. |
| No interactivo | Suprime los mensajes para la entrada de usuario o confirmaciones. |
| Nivel de detalle | Especifica la cantidad de detalle que se muestra en la salida: *normal*, *quiet*, *detallada*. |

Consulte también [variables de entorno](cli-ref-environment-variables.md)

## <a name="examples"></a>Ejemplos

```cli
nuget add foo.nupkg -Source c:\bar\

nuget add foo.nupkg -Source \\bar\packages\
```