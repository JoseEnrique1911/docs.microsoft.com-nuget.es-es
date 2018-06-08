---
title: comandos de NuGet dotnet.
description: Una referencia breve para los comandos relacionados con NuGet mediante la interfaz de línea de comandos de dotnet.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 01/23/2018
ms.topic: conceptual
ms.openlocfilehash: dd30c5d5e29ff7ef7c6622a9b93c32b908198d52
ms.sourcegitcommit: 2a6d200012cdb4cbf5ab1264f12fecf9ae12d769
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34817026"
---
# <a name="dotnet-commands"></a>comandos de dotnet

El `dotnet` interfaz de línea de comandos, que se ejecuta en Windows, Mac OS X y Linux, proporciona una serie de comandos nuget.exe esenciales, como se muestra a continuación. Si dotnet satisface sus necesidades, no es necesario utilizar `nuget.exe`.

Para obtener información completa sobre `dotnet`, consulte [herramientas de interfaz de línea de comandos (CLI) de .NET Core](/dotnet/core/tools/?tabs=netcore2x).

## <a name="package-consumption"></a>Consumo de paquete

- [**dotnet Agregar paquete**](/dotnet/core/tools/dotnet-add-package): agrega una referencia de paquete al archivo de proyecto, a continuación, ejecuta `dotnet restore` para instalar el paquete.
- [**dotnet quitar paquete**](/dotnet/core/tools/dotnet-remove-package): quita una referencia de paquete desde el archivo de proyecto.
- [**restauración de dotnet**](/dotnet/core/tools/dotnet-restore?tabs=netcore2x): restaura las dependencias y las herramientas de generación de un proyecto. A partir de NuGet 4.0, se ejecuta el mismo código que `nuget restore`.
- [**variables locales de nuget dotnet**](/dotnet/core/tools/dotnet-nuget-locals): enumera las ubicaciones de la *global paquetes*, *caché http*, y *temp* carpetas y borra el contenido de esas carpetas.

## <a name="package-creation"></a>Creación de paquetes

- [**módulo de dotnet**](/dotnet/core/tools/dotnet-pack?tabs=netcore2x): el código de los módulos en un paquete de NuGet. A partir de NuGet 4.0, se ejecuta el mismo código que `nuget pack`.
- [**inserción de nuget dotnet**](/dotnet/core/tools/dotnet-nuget-push): inserta un paquete en un servidor y lo publica, aplicables a nuget.org, Visual Studio Team Services y servidores de NuGet de terceros.
- [**eliminar dotnet nuget**](/dotnet/core/tools/dotnet-nuget-delete): elimina o unlists un paquete desde un host, se aplica a nuget.org, Visual Studio Team Services y servidores de NuGet de terceros.