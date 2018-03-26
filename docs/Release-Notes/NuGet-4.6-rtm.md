---
title: "Notas de la versión de NuGet 4.6 RTM | Microsoft Docs"
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 3/7/2018
ms.topic: article
ms.prod: nuget
ms.technology: 
description: "Notas de la versión de NuGet 4.6.0, incluidos problemas conocidos, correcciones de errores, características agregadas y DCR."
keywords: "Notas de la versión de NuGet 4.6.0, correcciones de errores, problemas conocidos, características agregadas y DCR."
ms.reviewer:
- karann-msft
- unniravindranathan
- anangaur
ms.openlocfilehash: 32bfe8a7d5d60a0ad24444749beff64edcaab305
ms.sourcegitcommit: fa31ae10b5a5add7184bf7420554aade8adce6ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2018
---
# <a name="nuget-46-rtm-release-notes"></a><span data-ttu-id="c3101-104">Notas de la versión de NuGet 4.6 RTM</span><span class="sxs-lookup"><span data-stu-id="c3101-104">NuGet 4.6 RTM Release Notes</span></span>

<span data-ttu-id="c3101-105">[Visual Studio 2017 15.6 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) incluye [NuGet 4.6.0 RTM](https://dist.nuget.org/win-x86-commandline/v4.6.0/nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="c3101-105">[Visual Studio 2017 15.6 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.6.0](https://dist.nuget.org/win-x86-commandline/v4.6.0/nuget.exe).</span></span>

## <a name="summary-whats-new-in-this-release"></a><span data-ttu-id="c3101-106">Resumen: Novedades de esta versión</span><span class="sxs-lookup"><span data-stu-id="c3101-106">Summary: What's New in this Release</span></span>
* <span data-ttu-id="c3101-107">Hemos agregado compatibilidad para la [firma de paquetes](https://docs.microsoft.com/en-us/nuget/create-packages/sign-a-package).</span><span class="sxs-lookup"><span data-stu-id="c3101-107">We have added support for [signing packages](https://docs.microsoft.com/en-us/nuget/create-packages/sign-a-package).</span></span>  
* <span data-ttu-id="c3101-108">Visual Studio 2017 y nuget.exe ahora comprueban la integridad del paquete antes de la instalación, restaurando los paquetes de los [paquetes firmados](https://docs.microsoft.com/en-us/nuget/reference/signed-packages-reference).</span><span class="sxs-lookup"><span data-stu-id="c3101-108">Visual Studio 2017 and nuget.exe now verifies package integrity before installing, restoring packages for [signed packages](https://docs.microsoft.com/en-us/nuget/reference/signed-packages-reference).</span></span>
* <span data-ttu-id="c3101-109">Se ha mejorado el rendimiento de restauraciones sucesivas.</span><span class="sxs-lookup"><span data-stu-id="c3101-109">We have improved performance of successive restores.</span></span>

## <a name="known-issues"></a><span data-ttu-id="c3101-110">Problemas conocidos</span><span class="sxs-lookup"><span data-stu-id="c3101-110">Known issues</span></span>
### <a name="issues-with-net-standard-20-with-net-framework--nuget"></a><span data-ttu-id="c3101-111">Problemas con .NET 2.0 Standard con .NET Framework y NuGet</span><span class="sxs-lookup"><span data-stu-id="c3101-111">Issues with .NET Standard 2.0 with .NET Framework & NuGet</span></span> 

<span data-ttu-id="c3101-112">.NET Standard y sus herramientas se han diseñado de forma que los proyectos destinados a .NET Framework 4.6.1 puedan usar los paquetes y proyectos de NuGet que tienen como destino .NET Standard 2.0 o versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="c3101-112">.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier.</span></span> <span data-ttu-id="c3101-113">[En este documento](https://github.com/dotnet/standard/issues/481) se resumen los problemas relacionados con ese escenario, el plan para abordarlos, así como soluciones alternativas que se pueden implementar con el estado actual de las herramientas.</span><span class="sxs-lookup"><span data-stu-id="c3101-113">[This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.</span></span>

## <a name="top-issues-fixed-in-this-release"></a><span data-ttu-id="c3101-114">Principales problemas corregidos en esta versión</span><span class="sxs-lookup"><span data-stu-id="c3101-114">Top issues fixed in this release</span></span>

<span data-ttu-id="c3101-115">**Correcciones de rendimiento**</span><span class="sxs-lookup"><span data-stu-id="c3101-115">**Performance fixes**</span></span>
* <span data-ttu-id="c3101-116">No se escriben archivos de recursos cuando no hay ningún cambio - [#6491](https://github.com/NuGet/Home/issues/6491)</span><span class="sxs-lookup"><span data-stu-id="c3101-116">Don't write asset files when there is no change - [#6491](https://github.com/NuGet/Home/issues/6491)</span></span>
* <span data-ttu-id="c3101-117">La restauración provoca evaluaciones de MSBuild adicionales cuando el TFM de proyectos secundarios no coincide con el proyecto primario - [#6311](https://github.com/NuGet/Home/issues/6311)</span><span class="sxs-lookup"><span data-stu-id="c3101-117">Restore causes extra MSBuild evaluations when child projects' TFM do not match with the parent project's - [#6311](https://github.com/NuGet/Home/issues/6311)</span></span>
* <span data-ttu-id="c3101-118">Mejorar el rendimiento de la restauración de NoOp optimizando la creación de especificaciones del gráfico de dependencias - [#6252](https://github.com/NuGet/Home/issues/6252)</span><span class="sxs-lookup"><span data-stu-id="c3101-118">Improve NoOp restore perf by optimizing dependency graph spec creation - [#6252](https://github.com/NuGet/Home/issues/6252)</span></span>

<span data-ttu-id="c3101-119">**Errores**</span><span class="sxs-lookup"><span data-stu-id="c3101-119">**Bugs**</span></span>
* <span data-ttu-id="c3101-120">La inserción en la carpeta local deja nupkg bloqueado - [#6325](https://github.com/NuGet/Home/issues/6325)</span><span class="sxs-lookup"><span data-stu-id="c3101-120">Push to local folder leaves nupkg locked - [#6325](https://github.com/NuGet/Home/issues/6325)</span></span>
* <span data-ttu-id="c3101-121">Implementación de NuGet Plugin: varios problemas - [#6149](https://github.com/NuGet/Home/issues/6149)</span><span class="sxs-lookup"><span data-stu-id="c3101-121">NuGet Plugin implementation:  multiple issues - [#6149](https://github.com/NuGet/Home/issues/6149)</span></span>
* <span data-ttu-id="c3101-122">UIHang - Quitar llamada al servicio de consulta de la inicialización de MEF en VSSolutionManager - [#6110](https://github.com/NuGet/Home/issues/6110)</span><span class="sxs-lookup"><span data-stu-id="c3101-122">UIHang - Remove query service call from MEF initialization in VSSolutionManager - [#6110](https://github.com/NuGet/Home/issues/6110)</span></span>
* <span data-ttu-id="c3101-123">Error que indica una excepción para la tarea de descarga de paquetes cancelada - [#6096](https://github.com/NuGet/Home/issues/6096)</span><span class="sxs-lookup"><span data-stu-id="c3101-123">Error reporting exception for cancelled package download task - [#6096](https://github.com/NuGet/Home/issues/6096)</span></span>
* <span data-ttu-id="c3101-124">NuGet.exe reemplaza "+" por "%2B" en el nombre del ensamblado - [#5956](https://github.com/NuGet/Home/issues/5956)</span><span class="sxs-lookup"><span data-stu-id="c3101-124">NuGet.exe replaces '+' with '%2B' in assembly name - [#5956](https://github.com/NuGet/Home/issues/5956)</span></span>
* <span data-ttu-id="c3101-125">Fn+F1 no lleva a la página de ayuda adecuada para la consola y la interfaz de usuario de PM - [#5912](https://github.com/NuGet/Home/issues/5912)</span><span class="sxs-lookup"><span data-stu-id="c3101-125">Fn+F1 does not take to the right help page for PM UI and Console - [#5912](https://github.com/NuGet/Home/issues/5912)</span></span>
* <span data-ttu-id="c3101-126">VS NuGet escribe rutas de acceso absolutas en archivos de proyecto en determinadas circunstancias - [#5888](https://github.com/NuGet/Home/issues/5888)</span><span class="sxs-lookup"><span data-stu-id="c3101-126">VS NuGet writes absolute paths into project files under specific circumstances - [#5888](https://github.com/NuGet/Home/issues/5888)</span></span>
* <span data-ttu-id="c3101-127">Corrección de regresión 4.3: los marcadores de posición $product$ y $AssemblyGuid$ no se reemplazan en contentfile mediante la transformación - [#5880](https://github.com/NuGet/Home/issues/5880)</span><span class="sxs-lookup"><span data-stu-id="c3101-127">Fix 4.3 regression - Placeholders $product$ and $AssemblyGuid$ not replaced in contentfile through transformation - [#5880](https://github.com/NuGet/Home/issues/5880)</span></span>
* <span data-ttu-id="c3101-128">dotnet se restaura con varios bloqueos de orígenes - [#5817](https://github.com/NuGet/Home/issues/5817)</span><span class="sxs-lookup"><span data-stu-id="c3101-128">dotnet restore with multiple sources crashes - [#5817](https://github.com/NuGet/Home/issues/5817)</span></span>
* <span data-ttu-id="c3101-129">El paquete debe reevaluar versiones del proyecto para permitir el control de versiones git - [#4790](https://github.com/NuGet/Home/issues/4790)</span><span class="sxs-lookup"><span data-stu-id="c3101-129">Pack should re-evaluate project versions to allow git versioning - [#4790](https://github.com/NuGet/Home/issues/4790)</span></span>
* <span data-ttu-id="c3101-130">Se debe mejorar mucho para entender los errores al instalar un paquete incompatible - [#4555](https://github.com/NuGet/Home/issues/4555)</span><span class="sxs-lookup"><span data-stu-id="c3101-130">Improve hard to understand errors when you install an incompatible package - [#4555](https://github.com/NuGet/Home/issues/4555)</span></span>
* <span data-ttu-id="c3101-131">TemplateWizard necesita la opción de instalar paquetes como PackageReferences - [#4549](https://github.com/NuGet/Home/issues/4549)</span><span class="sxs-lookup"><span data-stu-id="c3101-131">TemplateWizard needs option to install packages as PackageReferences - [#4549](https://github.com/NuGet/Home/issues/4549)</span></span>
* <span data-ttu-id="c3101-132">Los archivos de propiedades entregados por paquetes se ignoran cuando MSBuild.exe se ejecuta desde fuera de un símbolo del sistema de desarrollador - [#4530](https://github.com/NuGet/Home/issues/4530)</span><span class="sxs-lookup"><span data-stu-id="c3101-132">Package-delivered props files ignored when MSBuild.exe is run from outside a Developer Command Prompt - [#4530](https://github.com/NuGet/Home/issues/4530)</span></span>
* <span data-ttu-id="c3101-133">Corregir mensajes de error deficientes al hacer referencia a la Biblioteca de .NET Standard que no es aplicable al proyecto - [#4423](https://github.com/NuGet/Home/issues/4423)</span><span class="sxs-lookup"><span data-stu-id="c3101-133">Fix poor error message when referencing .NET Standard Library that is not applicable to project - [#4423](https://github.com/NuGet/Home/issues/4423)</span></span>
* <span data-ttu-id="c3101-134">La adición de paquetes dotnet da error para los paquetes que apuntan a perfiles portátiles con poca información - [#4349](https://github.com/NuGet/Home/issues/4349)</span><span class="sxs-lookup"><span data-stu-id="c3101-134">dotnet add package fails for package targeting portable profile with little guidance - [#4349](https://github.com/NuGet/Home/issues/4349)</span></span>
* <span data-ttu-id="c3101-135">Paquete de dotnet: falta sufijo de versión en ProjectReference - [#4337](https://github.com/NuGet/Home/issues/4337)</span><span class="sxs-lookup"><span data-stu-id="c3101-135">dotnet pack - version suffix missing from ProjectReference - [#4337](https://github.com/NuGet/Home/issues/4337)</span></span>
* <span data-ttu-id="c3101-136">Errores de compilación y bloqueo de VS con plantilla de .NET Core - [#3973](https://github.com/NuGet/Home/issues/3973)</span><span class="sxs-lookup"><span data-stu-id="c3101-136">Build errors and VS crash with .NET Core template - [#3973](https://github.com/NuGet/Home/issues/3973)</span></span>
* <span data-ttu-id="c3101-137">No se puede cargar el índice de servicio para la https de origen:\* - [#3681](https://github.com/NuGet/Home/issues/3681)</span><span class="sxs-lookup"><span data-stu-id="c3101-137">Unable to load the service index for source https:\* - [#3681](https://github.com/NuGet/Home/issues/3681)</span></span>
* <span data-ttu-id="c3101-138">No funciona la lista -allversions de nuGet.exe - [#3441](https://github.com/NuGet/Home/issues/3441)</span><span class="sxs-lookup"><span data-stu-id="c3101-138">nuget.exe list -allversions doesn't work - [#3441](https://github.com/NuGet/Home/issues/3441)</span></span>
* <span data-ttu-id="c3101-139">Mensaje de error de resolución de dependencias que puede inducir a error - [#2984](https://github.com/NuGet/Home/issues/2984)</span><span class="sxs-lookup"><span data-stu-id="c3101-139">Misleading dependency resolution error message - [#2984](https://github.com/NuGet/Home/issues/2984)</span></span>
* <span data-ttu-id="c3101-140">La restauración de nuGet.exe no genera archivos .props y .targets para .msbuildproj (regresión en la actualización de la versión 3.3.0 a 3.4.4) - [#2921](https://github.com/NuGet/Home/issues/2921)</span><span class="sxs-lookup"><span data-stu-id="c3101-140">nuget.exe restore doesn't produce .props and .targets files for .msbuildproj (regression in v3.3.0-3.4.4 upgrade) - [#2921](https://github.com/NuGet/Home/issues/2921)</span></span>
* <span data-ttu-id="c3101-141">Retraso de la interfaz de usuario al actualizar un paquete NuGet con el archivo XAML abierto: [#2878](https://github.com/NuGet/Home/issues/2878)</span><span class="sxs-lookup"><span data-stu-id="c3101-141">UI delay when updating a NuGet package with XAML file open - [#2878](https://github.com/NuGet/Home/issues/2878)</span></span>
* <span data-ttu-id="c3101-142">Se produce un error en el proyecto del sitio web de IIS con caracteres no válidos en la ruta de acceso - [#2798](https://github.com/NuGet/Home/issues/2798)</span><span class="sxs-lookup"><span data-stu-id="c3101-142">WebSite project from IIS fails with illegal characters in path - [#2798](https://github.com/NuGet/Home/issues/2798)</span></span>
* <span data-ttu-id="c3101-143">La adición de NuGet se bloquea en CentOS - [#2708](https://github.com/NuGet/Home/issues/2708)</span><span class="sxs-lookup"><span data-stu-id="c3101-143">Nuget add hangs on CentOS - [#2708](https://github.com/NuGet/Home/issues/2708)</span></span>
* <span data-ttu-id="c3101-144">Se produce un error en la restauración con packagesavemode - nupkg para json.net - [#2706](https://github.com/NuGet/Home/issues/2706)</span><span class="sxs-lookup"><span data-stu-id="c3101-144">Restore with packagesavemode -nupkg fails for json.net - [#2706](https://github.com/NuGet/Home/issues/2706)</span></span>
* <span data-ttu-id="c3101-145">Filtro del administrador de paquetes no disponible en la ventana de resultados de VS para el comando restaurar - [#2704](https://github.com/NuGet/Home/issues/2704)</span><span class="sxs-lookup"><span data-stu-id="c3101-145">Package Manager filter not available in vs output window for restore command - [#2704](https://github.com/NuGet/Home/issues/2704)</span></span>


[<span data-ttu-id="c3101-146">Lista de todos los problemas corregidos en esta versión</span><span class="sxs-lookup"><span data-stu-id="c3101-146">List of all issues fixed in this release</span></span>](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.6")