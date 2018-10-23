---
title: NuGet entre los complementos de plataforma
description: NuGet cross platform complementos de NuGet.exe, dotnet.exe, msbuild.exe y Visual Studio
author: nkolev92
ms.author: nikolev
ms.date: 07/01/2018
ms.topic: conceptual
ms.openlocfilehash: fdefc5b6189051fd83b2de644080284c09dd85f4
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43548211"
---
# <a name="nuget-cross-platform-plugins"></a><span data-ttu-id="1d898-103">NuGet entre los complementos de plataforma</span><span class="sxs-lookup"><span data-stu-id="1d898-103">NuGet cross platform plugins</span></span>

<span data-ttu-id="1d898-104">En NuGet 4.8 se ha agregado compatibilidad con entre los complementos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="1d898-104">In NuGet 4.8+ support for cross platform plugins has been added.</span></span>
<span data-ttu-id="1d898-105">Con esto se logra mediante la creación de un nuevo modelo de extensibilidad de complemento, que tiene que ajustarse a un conjunto estricto de reglas de operación.</span><span class="sxs-lookup"><span data-stu-id="1d898-105">This was achieved with by building a new plugin extensibility model, that has to conform to a strict set of rules of operation.</span></span>
<span data-ttu-id="1d898-106">Los complementos son aplicaciones ejecutables autónomas (runnables en el mundo de .NET Core), que los clientes de NuGet que se inicie en un proceso independiente.</span><span class="sxs-lookup"><span data-stu-id="1d898-106">The plugins are self-contained executables (runnables in the .NET Core world), that the NuGet Clients launch in a separate process.</span></span>
<span data-ttu-id="1d898-107">Se trata de una escritura true una vez, ejecutar en cualquier parte de complemento.</span><span class="sxs-lookup"><span data-stu-id="1d898-107">This is a true write once, run everywhere plugin.</span></span> <span data-ttu-id="1d898-108">Funcionará con todas las herramientas de cliente de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d898-108">It will work with all NuGet client tools.</span></span>
<span data-ttu-id="1d898-109">Los complementos pueden ser .NET Framework (NuGet.exe, MSBuild.exe y Visual Studio) o .NET Core (dotnet.exe).</span><span class="sxs-lookup"><span data-stu-id="1d898-109">The plugins can be either .NET Framework (NuGet.exe, MSBuild.exe and Visual Studio), or .NET Core (dotnet.exe).</span></span>
<span data-ttu-id="1d898-110">Se define un protocolo de comunicación con control de versiones entre el cliente de NuGet y el complemento.</span><span class="sxs-lookup"><span data-stu-id="1d898-110">A versioned communication protocol between the NuGet Client and the plugin is defined.</span></span> <span data-ttu-id="1d898-111">Durante el protocolo de enlace de inicio, los 2 procesos negocian la versión del protocolo.</span><span class="sxs-lookup"><span data-stu-id="1d898-111">During the startup handshake, the 2 processes negotiate the protocol version.</span></span>

<span data-ttu-id="1d898-112">Con el fin de cubrir todos los escenarios de herramientas de cliente de NuGet, uno necesitaría un .NET Framework y un complemento de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d898-112">In order to cover all NuGet client tools scenarios, one would need both a .NET Framework and a .NET Core plugin.</span></span>
<span data-ttu-id="1d898-113">La continuación se describen las combinaciones de marco del cliente de los complementos.</span><span class="sxs-lookup"><span data-stu-id="1d898-113">The below describes the client/framework combinations of the plugins.</span></span>

| <span data-ttu-id="1d898-114">Herramienta de cliente</span><span class="sxs-lookup"><span data-stu-id="1d898-114">Client tool</span></span>  | <span data-ttu-id="1d898-115">Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-115">Framework</span></span> |
| ------------ | --------- |
| <span data-ttu-id="1d898-116">Programa para la mejora</span><span class="sxs-lookup"><span data-stu-id="1d898-116">Visual Studio</span></span> | <span data-ttu-id="1d898-117">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-117">.NET Framework</span></span> |
| <span data-ttu-id="1d898-118">dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="1d898-118">dotnet.exe</span></span> | <span data-ttu-id="1d898-119">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="1d898-119">.NET Core</span></span> |
| <span data-ttu-id="1d898-120">NuGet.exe</span><span class="sxs-lookup"><span data-stu-id="1d898-120">NuGet.exe</span></span> | <span data-ttu-id="1d898-121">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-121">.NET Framework</span></span> |
| <span data-ttu-id="1d898-122">MSBuild.exe</span><span class="sxs-lookup"><span data-stu-id="1d898-122">MSBuild.exe</span></span> | <span data-ttu-id="1d898-123">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-123">.NET Framework</span></span> |
| <span data-ttu-id="1d898-124">NuGet.exe en Mono</span><span class="sxs-lookup"><span data-stu-id="1d898-124">NuGet.exe on Mono</span></span> | <span data-ttu-id="1d898-125">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-125">.NET Framework</span></span> |

## <a name="how-does-it-work"></a><span data-ttu-id="1d898-126">¿Cómo funciona</span><span class="sxs-lookup"><span data-stu-id="1d898-126">How does it work</span></span>

<span data-ttu-id="1d898-127">El flujo de trabajo de alto nivel puede describirse como sigue:</span><span class="sxs-lookup"><span data-stu-id="1d898-127">The high level workflow can be described as follows:</span></span>

1. <span data-ttu-id="1d898-128">NuGet detecta complementos disponibles.</span><span class="sxs-lookup"><span data-stu-id="1d898-128">NuGet discovers available plugins.</span></span>
1. <span data-ttu-id="1d898-129">Cuando sea aplicable, NuGet se iterará en los complementos en el orden de prioridad e inicia uno por uno.</span><span class="sxs-lookup"><span data-stu-id="1d898-129">When applicable, NuGet will iterate over the plugins in priority order and starts them one by one.</span></span>
1. <span data-ttu-id="1d898-130">NuGet usará el primer complemento que puede atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1d898-130">NuGet will use the first plugin that can service the request.</span></span>
1. <span data-ttu-id="1d898-131">Los complementos se cerrará cuando ya no se necesitan.</span><span class="sxs-lookup"><span data-stu-id="1d898-131">The plugins will be shut down when they are no longer needed.</span></span>

## <a name="general-plugin-requirements"></a><span data-ttu-id="1d898-132">Requisitos del complemento de general</span><span class="sxs-lookup"><span data-stu-id="1d898-132">General plugin requirements</span></span>

<span data-ttu-id="1d898-133">La versión actual del protocolo es *2.0.0*.</span><span class="sxs-lookup"><span data-stu-id="1d898-133">The current protocol version is *2.0.0*.</span></span>
<span data-ttu-id="1d898-134">En esta versión, los requisitos son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="1d898-134">Under this version, the requirements are as follows:</span></span>

- <span data-ttu-id="1d898-135">Tiene un ensamblados de la firma Authenticode de válido y de confianza que se ejecutarán en Windows y Mono.</span><span class="sxs-lookup"><span data-stu-id="1d898-135">Have a valid, trusted Authenticode signature assemblies that will run on Windows and Mono.</span></span> <span data-ttu-id="1d898-136">No hay ningún requisito especial de confianza para los ensamblados que se ejecutan en Linux y Mac todavía.</span><span class="sxs-lookup"><span data-stu-id="1d898-136">There is no special trust requirement for assemblies run on Linux and Mac yet.</span></span> [<span data-ttu-id="1d898-137">Problema pertinente</span><span class="sxs-lookup"><span data-stu-id="1d898-137">Relevant issue</span></span>](https://github.com/NuGet/Home/issues/6702)
- <span data-ttu-id="1d898-138">Admite iniciar sin estado en el contexto de seguridad actual de las herramientas de cliente de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d898-138">Support stateless launching under the current security context of NuGet client tools.</span></span> <span data-ttu-id="1d898-139">Por ejemplo, las herramientas de cliente de NuGet no realizará elevación o inicialización adicional fuera el protocolo de complemento que se describe más adelante.</span><span class="sxs-lookup"><span data-stu-id="1d898-139">For example, NuGet client tools will not perform elevation or additional initialization outside of the plugin protocol described later.</span></span>
- <span data-ttu-id="1d898-140">Ser no interactivo, a menos que especifique explícitamente.</span><span class="sxs-lookup"><span data-stu-id="1d898-140">Be non interactive, unless explicitly specified.</span></span>
- <span data-ttu-id="1d898-141">Atenerse a la versión del protocolo negociado complemento.</span><span class="sxs-lookup"><span data-stu-id="1d898-141">Adhere to the negotiated plugin protocol version.</span></span>
- <span data-ttu-id="1d898-142">Responder a todas las solicitudes dentro de un período de tiempo razonable.</span><span class="sxs-lookup"><span data-stu-id="1d898-142">Respond to all requests within a reasonable time period.</span></span>
- <span data-ttu-id="1d898-143">Aceptar las solicitudes de cancelación para cualquier operación en curso.</span><span class="sxs-lookup"><span data-stu-id="1d898-143">Honor cancellation requests for any in-progress operation.</span></span>

<span data-ttu-id="1d898-144">La especificación técnica se describe con más detalle en las especificaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="1d898-144">The technical specification is described in more detail in the following specs:</span></span>

- [<span data-ttu-id="1d898-145">Complemento de descarga del paquete de NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-145">NuGet Package Download Plugin</span></span>](https://github.com/NuGet/Home/wiki/NuGet-Package-Download-Plugin)
- [<span data-ttu-id="1d898-146">NuGet entre el complemento de autenticación plat</span><span class="sxs-lookup"><span data-stu-id="1d898-146">NuGet cross plat authentication plugin</span></span>](https://github.com/NuGet/Home/wiki/NuGet-cross-plat-authentication-plugin)

## <a name="client---plugin-interaction"></a><span data-ttu-id="1d898-147">Cliente: interacción de complemento</span><span class="sxs-lookup"><span data-stu-id="1d898-147">Client - Plugin interaction</span></span>

<span data-ttu-id="1d898-148">Las herramientas de cliente de NuGet y los complementos de comunican con JSON a través de los flujos estándar (stdin, stdout, stderr).</span><span class="sxs-lookup"><span data-stu-id="1d898-148">NuGet client tools and the plugins communicate with JSON over standard streams (stdin, stdout, stderr).</span></span> <span data-ttu-id="1d898-149">Todos los datos deben estar codificados en UTF-8.</span><span class="sxs-lookup"><span data-stu-id="1d898-149">All data must be UTF-8 encoded.</span></span>
<span data-ttu-id="1d898-150">Los complementos se inician con el argumento "-Plugin".</span><span class="sxs-lookup"><span data-stu-id="1d898-150">The plugins are launched with the argument "-Plugin".</span></span> <span data-ttu-id="1d898-151">En caso de que un usuario inicia directamente un complemento ejecutable sin este argumento, el complemento puede proporcionar un mensaje informativo en lugar de esperar un protocolo de enlace de protocolo.</span><span class="sxs-lookup"><span data-stu-id="1d898-151">In case a user directly launches a plugin executable without this argument, the plugin can give an informative message instead of waiting for a protocol handshake.</span></span>
<span data-ttu-id="1d898-152">El tiempo de espera del protocolo de enlace de protocolo es 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="1d898-152">The protocol handshake timeout is 5 seconds.</span></span> <span data-ttu-id="1d898-153">El complemento debe completar la instalación de como aparte de una cantidad como sea posible.</span><span class="sxs-lookup"><span data-stu-id="1d898-153">The plugin should complete the setup in as short of an amount as possible.</span></span>
<span data-ttu-id="1d898-154">Las herramientas de cliente de NuGet consultará las operaciones admitidas de un complemento pasando el índice de servicio para un origen de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d898-154">NuGet client tools will query a plugin’s supported operations by passing in the service index for a NuGet source.</span></span> <span data-ttu-id="1d898-155">Un complemento puede utilizar el índice de servicio para comprobar la presencia de tipos de servicios admitidos.</span><span class="sxs-lookup"><span data-stu-id="1d898-155">A plugin may use the service index to check for the presence of supported service types.</span></span>

<span data-ttu-id="1d898-156">La comunicación entre las herramientas de cliente de NuGet y el complemento es bidireccional.</span><span class="sxs-lookup"><span data-stu-id="1d898-156">The communication between the NuGet client tools and the plugin is bidirectional.</span></span> <span data-ttu-id="1d898-157">Cada solicitud tiene un tiempo de espera de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="1d898-157">Each request has a timeout of 5 seconds.</span></span> <span data-ttu-id="1d898-158">Si se supone que las operaciones que tarden más el proceso respectivo debe enviar un mensaje de progreso para evitar que la solicitud de tiempo de espera. Después de un minuto de inactividad de un complemento se considera inactivo y se cierra.</span><span class="sxs-lookup"><span data-stu-id="1d898-158">If operations are supposed to take longer the respective process should send out a progress message to prevent the request from timing out. After 1 minute of inactivity a plugin is considered idle and is shut down.</span></span>

## <a name="plugin-installation-and-discovery"></a><span data-ttu-id="1d898-159">Detección e instalación del complemento</span><span class="sxs-lookup"><span data-stu-id="1d898-159">Plugin installation and discovery</span></span>

<span data-ttu-id="1d898-160">Los complementos se detectarán a través de una estructura de directorios basada en convenciones.</span><span class="sxs-lookup"><span data-stu-id="1d898-160">The plugins will be discovered via a convention based directory structure.</span></span>
<span data-ttu-id="1d898-161">Escenarios de integración y entrega continuas y usuarios avanzados pueden usar una variable de entorno para invalidar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="1d898-161">CI/CD scenarios and power users can use an environment variable to override the behavior.</span></span>

- <span data-ttu-id="1d898-162">`NUGET_PLUGIN_PATHS` -define los complementos que se usará para ese proceso de NuGet, prioridad reservado.</span><span class="sxs-lookup"><span data-stu-id="1d898-162">`NUGET_PLUGIN_PATHS` - defines the plugins that will be used for that NuGet process, priority reserved.</span></span> <span data-ttu-id="1d898-163">Si se establece esta variable de entorno, invalida la detección basada en convenciones.</span><span class="sxs-lookup"><span data-stu-id="1d898-163">If this environment variable is set, it overrides the convention based discovery.</span></span>
-  <span data-ttu-id="1d898-164">Ubicación del usuario, la ubicación de la página principal de NuGet en `%UserProfile%/.nuget/plugins`.</span><span class="sxs-lookup"><span data-stu-id="1d898-164">User-location, the NuGet Home location in `%UserProfile%/.nuget/plugins`.</span></span> <span data-ttu-id="1d898-165">Esta ubicación no puede invalidarse.</span><span class="sxs-lookup"><span data-stu-id="1d898-165">This location cannot be overriden.</span></span> <span data-ttu-id="1d898-166">Se usará un directorio raíz diferente para los complementos de .NET Core y .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1d898-166">A different root directory will be used for .NET Core and .NET Framework plugins.</span></span>

| <span data-ttu-id="1d898-167">Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-167">Framework</span></span> | <span data-ttu-id="1d898-168">Ubicación raíz de detección</span><span class="sxs-lookup"><span data-stu-id="1d898-168">Root discovery location</span></span>  |
| ------- | ------------------------ |
| <span data-ttu-id="1d898-169">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="1d898-169">.NET Core</span></span> |  `%UserProfile%/.nuget/plugins/netcore` |
| <span data-ttu-id="1d898-170">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1d898-170">.NET Framework</span></span> | `%UserProfile%/.nuget/plugins/netfx` |

<span data-ttu-id="1d898-171">Cada complemento debe instalarse en su propia carpeta.</span><span class="sxs-lookup"><span data-stu-id="1d898-171">Each plugin should be installed in its own folder.</span></span>
<span data-ttu-id="1d898-172">El punto de entrada de complemento será el nombre de la carpeta instalado, con las extensiones de archivo .dll para .NET Core y la extensión .exe para .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1d898-172">The plugin entry point will be the name of the installed folder, with the .dll extensions for .NET Core, and .exe extension for .NET Framework.</span></span>

```
.nuget
    plugins
        netfx
            myPlugin
                myPlugin.exe
                nuget.protocol.dll
                ...
        netcore
            myPlugin
                myPlugin.dll
                nuget.protocol.dll
                ...
```

> [!Note]
> <span data-ttu-id="1d898-173">Actualmente no hay ningún caso de usuario para la instalación de los complementos.</span><span class="sxs-lookup"><span data-stu-id="1d898-173">There is currently no user story for the installation of the plugins.</span></span> <span data-ttu-id="1d898-174">Es tan fácil como mover los archivos necesarios en la ubicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1d898-174">It's as simple as moving the required files into the predetermined location.</span></span>

## <a name="supported-operations"></a><span data-ttu-id="1d898-175">Operaciones admitidas</span><span class="sxs-lookup"><span data-stu-id="1d898-175">Supported operations</span></span>

<span data-ttu-id="1d898-176">Se admiten dos operaciones en el nuevo protocolo de complemento.</span><span class="sxs-lookup"><span data-stu-id="1d898-176">Two operations are supported under the new plugin protocol.</span></span>

| <span data-ttu-id="1d898-177">Nombre de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-177">Operation name</span></span> | <span data-ttu-id="1d898-178">Versión mínima del protocolo</span><span class="sxs-lookup"><span data-stu-id="1d898-178">Minimum protocol version</span></span> | <span data-ttu-id="1d898-179">Versión mínima de cliente de NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-179">Minimum NuGet client version</span></span> |
| -------------- | ----------------------- | --------------------- |
| <span data-ttu-id="1d898-180">Descargar paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-180">Download Package</span></span> | <span data-ttu-id="1d898-181">1.0.0</span><span class="sxs-lookup"><span data-stu-id="1d898-181">1.0.0</span></span> | <span data-ttu-id="1d898-182">4.3.0</span><span class="sxs-lookup"><span data-stu-id="1d898-182">4.3.0</span></span> |
| [<span data-ttu-id="1d898-183">Autenticación</span><span class="sxs-lookup"><span data-stu-id="1d898-183">Authentication</span></span>](NuGet-Cross-Platform-Authentication-Plugin.md) | <span data-ttu-id="1d898-184">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d898-184">2.0.0</span></span> | <span data-ttu-id="1d898-185">4.8.0</span><span class="sxs-lookup"><span data-stu-id="1d898-185">4.8.0</span></span> |

## <a name="running-plugins-under-the-correct-runtime"></a><span data-ttu-id="1d898-186">Ejecutar complementos en el runtime correcto</span><span class="sxs-lookup"><span data-stu-id="1d898-186">Running plugins under the correct runtime</span></span>

<span data-ttu-id="1d898-187">Para el paquete NuGet en escenarios de dotnet.exe, deben poder ejecutarse en ese tiempo de ejecución específico de la dotnet.exe complementos.</span><span class="sxs-lookup"><span data-stu-id="1d898-187">For the NuGet in dotnet.exe scenarios, plugins need to be able to execute under that specific runtime of the dotnet.exe.</span></span>
<span data-ttu-id="1d898-188">Está en el proveedor del complemento y el consumidor para asegurarse de que se utiliza una combinación de dotnet.exe/plugin compatible.</span><span class="sxs-lookup"><span data-stu-id="1d898-188">It's on the plugin provider and the consumer to make sure a compatible dotnet.exe/plugin combination is used.</span></span>
<span data-ttu-id="1d898-189">Podría producirse un problema potencial con los complementos de la ubicación del usuario cuando, por ejemplo, un dotnet.exe en el tiempo de ejecución 2.0 intenta usar un complemento escrito para el tiempo de ejecución 2.1.</span><span class="sxs-lookup"><span data-stu-id="1d898-189">A potential issue could arise with the user-location plugins when for example, a dotnet.exe under the 2.0 runtime tries to use a plugin written for the 2.1 runtime.</span></span>

## <a name="capabilities-caching"></a><span data-ttu-id="1d898-190">Capacidades de almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="1d898-190">Capabilities caching</span></span>

<span data-ttu-id="1d898-191">La comprobación de seguridad y la creación de instancias de los complementos es costoso.</span><span class="sxs-lookup"><span data-stu-id="1d898-191">The security verification and instantiation of the plugins is costly.</span></span> <span data-ttu-id="1d898-192">La operación de descarga se produce con más frecuencia que la operación de autenticación, sin embargo, el usuario medio de NuGet solo es probable que tenga un complemento de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1d898-192">The download operation happens way more frequently than the authentication operation, however the average NuGet user is only likely to have an authentication plugin.</span></span>
<span data-ttu-id="1d898-193">Para mejorar la experiencia, NuGet almacenará en caché las notificaciones de operación para la solicitud dada.</span><span class="sxs-lookup"><span data-stu-id="1d898-193">To improve the experience, NuGet will cache the operation claims for the given request.</span></span> <span data-ttu-id="1d898-194">Esta caché es por complemento con la clave de complemento que se va a la ruta de acceso del complemento y la expiración de caché capacidades es de 30 días.</span><span class="sxs-lookup"><span data-stu-id="1d898-194">This cache is per plugin with the plugin key being the plugin path, and the expiration for this capabilities cache is 30 days.</span></span> 

<span data-ttu-id="1d898-195">La memoria caché se encuentra en `%LocalAppData%/NuGet/plugins-cache` y puede invalidarse con la variable de entorno `NUGET_PLUGINS_CACHE_PATH`.</span><span class="sxs-lookup"><span data-stu-id="1d898-195">The cache is located in `%LocalAppData%/NuGet/plugins-cache` and be overriden with the environment variable `NUGET_PLUGINS_CACHE_PATH`.</span></span> <span data-ttu-id="1d898-196">Para borrar este [caché](../../consume-packages/managing-the-global-packages-and-cache-folders.md), puede ejecutar las variables locales con el `plugins-cache` opción.</span><span class="sxs-lookup"><span data-stu-id="1d898-196">To clear this [cache](../../consume-packages/managing-the-global-packages-and-cache-folders.md), one can run the locals command with the `plugins-cache` option.</span></span>
<span data-ttu-id="1d898-197">El `all` opción variables locales, ahora también se eliminarán la memoria caché de complementos.</span><span class="sxs-lookup"><span data-stu-id="1d898-197">The `all` locals option will now also delete the plugins cache.</span></span> 

## <a name="protocol-messages-index"></a><span data-ttu-id="1d898-198">Índice de los mensajes de protocolo</span><span class="sxs-lookup"><span data-stu-id="1d898-198">Protocol messages index</span></span>

<span data-ttu-id="1d898-199">Versión del protocolo *1.0.0* mensajes:</span><span class="sxs-lookup"><span data-stu-id="1d898-199">Protocol Version *1.0.0* messages:</span></span>

1.  <span data-ttu-id="1d898-200">Cerrar</span><span class="sxs-lookup"><span data-stu-id="1d898-200">Close</span></span>
    * <span data-ttu-id="1d898-201">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-201">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-202">La solicitud no contendrá ninguna carga.</span><span class="sxs-lookup"><span data-stu-id="1d898-202">The request will contain no payload</span></span>
    * <span data-ttu-id="1d898-203">Se espera ninguna respuesta.</span><span class="sxs-lookup"><span data-stu-id="1d898-203">No response is expected.</span></span>  <span data-ttu-id="1d898-204">Es la respuesta apropiada para que el proceso del complemento salir con prontitud.</span><span class="sxs-lookup"><span data-stu-id="1d898-204">The proper response is for the plugin process to promptly exit.</span></span>

2.  <span data-ttu-id="1d898-205">Copiar los archivos de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-205">Copy files in package</span></span>
    * <span data-ttu-id="1d898-206">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-206">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-207">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-207">The request will contain:</span></span>
        * <span data-ttu-id="1d898-208">el identificador de paquete y versión</span><span class="sxs-lookup"><span data-stu-id="1d898-208">the package ID and version</span></span>
        * <span data-ttu-id="1d898-209">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-209">the package source repository location</span></span>
        * <span data-ttu-id="1d898-210">ruta de acceso de directorio de destino</span><span class="sxs-lookup"><span data-stu-id="1d898-210">destination directory path</span></span>
        * <span data-ttu-id="1d898-211">una colección enumerable de archivos en el paquete que se copiarán en la ruta de acceso del directorio de destino</span><span class="sxs-lookup"><span data-stu-id="1d898-211">an enumerable of files in the package to be copied to the destination directory path</span></span>
    * <span data-ttu-id="1d898-212">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-212">A response will contain:</span></span>
        * <span data-ttu-id="1d898-213">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-213">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-214">una colección enumerable de rutas de acceso completas de los archivos copiados en el directorio de destino si la operación se realizó correctamente</span><span class="sxs-lookup"><span data-stu-id="1d898-214">an enumerable of full paths for copied files in the destination directory if the operation was successful</span></span>

3.  <span data-ttu-id="1d898-215">Copie el archivo de paquete (archivo .nupkg)</span><span class="sxs-lookup"><span data-stu-id="1d898-215">Copy package file (.nupkg)</span></span>
    * <span data-ttu-id="1d898-216">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-216">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-217">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-217">The request will contain:</span></span>
        * <span data-ttu-id="1d898-218">el identificador de paquete y versión</span><span class="sxs-lookup"><span data-stu-id="1d898-218">the package ID and version</span></span>
        * <span data-ttu-id="1d898-219">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-219">the package source repository location</span></span>
        * <span data-ttu-id="1d898-220">la ruta de acceso del archivo de destino</span><span class="sxs-lookup"><span data-stu-id="1d898-220">the destination file path</span></span>
    * <span data-ttu-id="1d898-221">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-221">A response will contain:</span></span>
        * <span data-ttu-id="1d898-222">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-222">a response code indicating the outcome of the operation</span></span>

4.  <span data-ttu-id="1d898-223">Obtener credenciales</span><span class="sxs-lookup"><span data-stu-id="1d898-223">Get credentials</span></span>
    * <span data-ttu-id="1d898-224">Solicitar dirección: complemento -> NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-224">Request direction:  plugin -> NuGet</span></span>
    * <span data-ttu-id="1d898-225">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-225">The request will contain:</span></span>
        * <span data-ttu-id="1d898-226">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-226">the package source repository location</span></span>
        * <span data-ttu-id="1d898-227">el código de estado HTTP obtenido en el repositorio de origen del paquete mediante las credenciales actuales</span><span class="sxs-lookup"><span data-stu-id="1d898-227">the HTTP status code obtained from the package source repository using current credentials</span></span>
    * <span data-ttu-id="1d898-228">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-228">A response will contain:</span></span>
        * <span data-ttu-id="1d898-229">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-229">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-230">un nombre de usuario, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-230">a username, if available</span></span>
        * <span data-ttu-id="1d898-231">una contraseña, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-231">a password, if available</span></span>

5.  <span data-ttu-id="1d898-232">Obtener archivos de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-232">Get files in package</span></span>
    * <span data-ttu-id="1d898-233">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-233">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-234">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-234">The request will contain:</span></span>
        * <span data-ttu-id="1d898-235">el identificador de paquete y versión</span><span class="sxs-lookup"><span data-stu-id="1d898-235">the package ID and version</span></span>
        * <span data-ttu-id="1d898-236">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-236">the package source repository location</span></span>
    * <span data-ttu-id="1d898-237">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-237">A response will contain:</span></span>
        * <span data-ttu-id="1d898-238">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-238">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-239">una colección enumerable de rutas de acceso de archivo en el paquete si la operación se realizó correctamente</span><span class="sxs-lookup"><span data-stu-id="1d898-239">an enumerable of file paths in the package if the operation was successful</span></span>

6.  <span data-ttu-id="1d898-240">Obtener notificaciones de operación</span><span class="sxs-lookup"><span data-stu-id="1d898-240">Get operation claims</span></span> 
    * <span data-ttu-id="1d898-241">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-241">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-242">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-242">The request will contain:</span></span>
        * <span data-ttu-id="1d898-243">el index.json de servicio para un origen del paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-243">the service index.json for a package source</span></span>
        * <span data-ttu-id="1d898-244">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-244">the package source repository location</span></span>
    * <span data-ttu-id="1d898-245">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-245">A response will contain:</span></span>
        * <span data-ttu-id="1d898-246">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-246">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-247">una colección enumerable de las operaciones admitidas (p. ej.: descarga del paquete) si la operación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="1d898-247">an enumerable of supported operations (e.g.:  package download) if the operation was successful.</span></span>  <span data-ttu-id="1d898-248">Si un complemento no es compatible con el origen del paquete, el complemento debe devolver un conjunto vacío de las operaciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="1d898-248">If a plugin does not support the package source, the plugin must return an empty set of supported operations.</span></span>

> [!Note]
> <span data-ttu-id="1d898-249">Este mensaje se ha actualizado en la versión *2.0.0*.</span><span class="sxs-lookup"><span data-stu-id="1d898-249">This message has been updated in version *2.0.0*.</span></span> <span data-ttu-id="1d898-250">Es en el cliente para mantener la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="1d898-250">It is on the client to preserve backward compatibility.</span></span>

7.  <span data-ttu-id="1d898-251">Obtener el hash de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-251">Get package hash</span></span>
    * <span data-ttu-id="1d898-252">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-252">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-253">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-253">The request will contain:</span></span>
        * <span data-ttu-id="1d898-254">el identificador de paquete y versión</span><span class="sxs-lookup"><span data-stu-id="1d898-254">the package ID and version</span></span>
        * <span data-ttu-id="1d898-255">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-255">the package source repository location</span></span>
        * <span data-ttu-id="1d898-256">el algoritmo hash</span><span class="sxs-lookup"><span data-stu-id="1d898-256">the hash algorithm</span></span>
    * <span data-ttu-id="1d898-257">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-257">A response will contain:</span></span>
        * <span data-ttu-id="1d898-258">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-258">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-259">un hash de archivo de paquete mediante el algoritmo hash solicitado si la operación se realizó correctamente</span><span class="sxs-lookup"><span data-stu-id="1d898-259">a package file hash using the requested hash algorithm if the operation was successful</span></span>

8.  <span data-ttu-id="1d898-260">Obtener las versiones del paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-260">Get package versions</span></span>
    * <span data-ttu-id="1d898-261">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-261">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-262">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-262">The request will contain:</span></span>
        * <span data-ttu-id="1d898-263">el identificador del paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-263">the package ID</span></span>
        * <span data-ttu-id="1d898-264">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-264">the package source repository location</span></span>
    * <span data-ttu-id="1d898-265">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-265">A response will contain:</span></span>
        * <span data-ttu-id="1d898-266">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-266">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-267">una colección enumerable de versiones del paquete si la operación se realizó correctamente</span><span class="sxs-lookup"><span data-stu-id="1d898-267">an enumerable of package versions if the operation was successful</span></span>

9.  <span data-ttu-id="1d898-268">Obtiene el índice de servicio</span><span class="sxs-lookup"><span data-stu-id="1d898-268">Get service index</span></span>
    * <span data-ttu-id="1d898-269">Solicitar dirección: complemento -> NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-269">Request direction:  plugin -> NuGet</span></span>
    * <span data-ttu-id="1d898-270">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-270">The request will contain:</span></span>
        * <span data-ttu-id="1d898-271">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-271">the package source repository location</span></span>
    * <span data-ttu-id="1d898-272">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-272">A response will contain:</span></span>
        * <span data-ttu-id="1d898-273">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-273">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-274">el índice de servicio si la operación se realizó correctamente</span><span class="sxs-lookup"><span data-stu-id="1d898-274">the service index if the operation was successful</span></span>

10.  <span data-ttu-id="1d898-275">Protocolo de enlace</span><span class="sxs-lookup"><span data-stu-id="1d898-275">Handshake</span></span>
     * <span data-ttu-id="1d898-276">Solicitar dirección: complemento de NuGet <> –</span><span class="sxs-lookup"><span data-stu-id="1d898-276">Request direction:  NuGet <-> plugin</span></span>
     * <span data-ttu-id="1d898-277">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-277">The request will contain:</span></span>
         * <span data-ttu-id="1d898-278">la versión actual del protocolo de complemento</span><span class="sxs-lookup"><span data-stu-id="1d898-278">the current plugin protocol version</span></span>
         * <span data-ttu-id="1d898-279">la versión de protocolo de complemento mínima admitida</span><span class="sxs-lookup"><span data-stu-id="1d898-279">the minimum supported plugin protocol version</span></span>
     * <span data-ttu-id="1d898-280">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-280">A response will contain:</span></span>
         * <span data-ttu-id="1d898-281">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-281">a response code indicating the outcome of the operation</span></span>
         * <span data-ttu-id="1d898-282">la versión de protocolo negociado si la operación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="1d898-282">the negotiated protocol version if the operation was successful.</span></span>  <span data-ttu-id="1d898-283">Finalización del complemento producirá un error.</span><span class="sxs-lookup"><span data-stu-id="1d898-283">A failure will result in termination of the plugin.</span></span>

11.  <span data-ttu-id="1d898-284">inicializar</span><span class="sxs-lookup"><span data-stu-id="1d898-284">Initialize</span></span>
     * <span data-ttu-id="1d898-285">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-285">Request direction:  NuGet -> plugin</span></span>
     * <span data-ttu-id="1d898-286">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-286">The request will contain:</span></span>
         * <span data-ttu-id="1d898-287">la herramienta de versión de cliente de NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-287">the NuGet client tool version</span></span>
         * <span data-ttu-id="1d898-288">el lenguaje eficaz de la herramienta de cliente NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d898-288">the NuGet client tool effective language.</span></span>  <span data-ttu-id="1d898-289">Esto toma en consideración la configuración ForceEnglishOutput, si usa.</span><span class="sxs-lookup"><span data-stu-id="1d898-289">This takes into consideration the ForceEnglishOutput setting, if used.</span></span>
         * <span data-ttu-id="1d898-290">la solicitud tiempo de espera predeterminado, que reemplaza el valor predeterminado de protocolo.</span><span class="sxs-lookup"><span data-stu-id="1d898-290">the default request timeout, which supersedes the protocol default.</span></span>
     * <span data-ttu-id="1d898-291">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-291">A response will contain:</span></span>
         * <span data-ttu-id="1d898-292">un código de respuesta que indica el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="1d898-292">a response code indicating the outcome of the operation.</span></span>  <span data-ttu-id="1d898-293">Finalización del complemento producirá un error.</span><span class="sxs-lookup"><span data-stu-id="1d898-293">A failure will result in termination of the plugin.</span></span>

12.  <span data-ttu-id="1d898-294">Registro</span><span class="sxs-lookup"><span data-stu-id="1d898-294">Log</span></span>
     * <span data-ttu-id="1d898-295">Solicitar dirección: complemento -> NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-295">Request direction:  plugin -> NuGet</span></span>
     * <span data-ttu-id="1d898-296">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-296">The request will contain:</span></span>
         * <span data-ttu-id="1d898-297">el nivel de registro para la solicitud</span><span class="sxs-lookup"><span data-stu-id="1d898-297">the log level for the request</span></span>
         * <span data-ttu-id="1d898-298">un mensaje para registrar</span><span class="sxs-lookup"><span data-stu-id="1d898-298">a message to log</span></span>
     * <span data-ttu-id="1d898-299">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-299">A response will contain:</span></span>
         * <span data-ttu-id="1d898-300">un código de respuesta que indica el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="1d898-300">a response code indicating the outcome of the operation.</span></span>

13.  <span data-ttu-id="1d898-301">Supervisar la salida del proceso de NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-301">Monitor NuGet process exit</span></span>
     * <span data-ttu-id="1d898-302">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-302">Request direction:  NuGet -> plugin</span></span>
     * <span data-ttu-id="1d898-303">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-303">The request will contain:</span></span>
         * <span data-ttu-id="1d898-304">el identificador de proceso de NuGet</span><span class="sxs-lookup"><span data-stu-id="1d898-304">the NuGet process ID</span></span>
     * <span data-ttu-id="1d898-305">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-305">A response will contain:</span></span>
         * <span data-ttu-id="1d898-306">un código de respuesta que indica el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="1d898-306">a response code indicating the outcome of the operation.</span></span>

14.  <span data-ttu-id="1d898-307">Captura previa de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-307">Prefetch package</span></span>
     * <span data-ttu-id="1d898-308">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-308">Request direction:  NuGet -> plugin</span></span>
     * <span data-ttu-id="1d898-309">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-309">The request will contain:</span></span>
         * <span data-ttu-id="1d898-310">el identificador de paquete y versión</span><span class="sxs-lookup"><span data-stu-id="1d898-310">the package ID and version</span></span>
         * <span data-ttu-id="1d898-311">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-311">the package source repository location</span></span>
     * <span data-ttu-id="1d898-312">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-312">A response will contain:</span></span>
         * <span data-ttu-id="1d898-313">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-313">a response code indicating the outcome of the operation</span></span>

15.  <span data-ttu-id="1d898-314">Establecer credenciales</span><span class="sxs-lookup"><span data-stu-id="1d898-314">Set credentials</span></span>
     * <span data-ttu-id="1d898-315">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-315">Request direction:  NuGet -> plugin</span></span>
     * <span data-ttu-id="1d898-316">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-316">The request will contain:</span></span>
         * <span data-ttu-id="1d898-317">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-317">the package source repository location</span></span>
         * <span data-ttu-id="1d898-318">el último paquete conocidos origen nombre de usuario, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-318">the last known package source username, if available</span></span>
         * <span data-ttu-id="1d898-319">la última contraseña de origen de paquetes conocidos, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-319">the last known package source password, if available</span></span>
         * <span data-ttu-id="1d898-320">el último conocido nombre de usuario proxy, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-320">the last known proxy username, if available</span></span>
         * <span data-ttu-id="1d898-321">la última contraseña proxy conocidos, si está disponible</span><span class="sxs-lookup"><span data-stu-id="1d898-321">the last known proxy password, if available</span></span>
     * <span data-ttu-id="1d898-322">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-322">A response will contain:</span></span>
         * <span data-ttu-id="1d898-323">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-323">a response code indicating the outcome of the operation</span></span>

16.  <span data-ttu-id="1d898-324">Definir el nivel de registro</span><span class="sxs-lookup"><span data-stu-id="1d898-324">Set log level</span></span>
     * <span data-ttu-id="1d898-325">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-325">Request direction:  NuGet -> plugin</span></span>
     * <span data-ttu-id="1d898-326">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-326">The request will contain:</span></span>
         * <span data-ttu-id="1d898-327">el nivel de registro predeterminado</span><span class="sxs-lookup"><span data-stu-id="1d898-327">the default log level</span></span>
     * <span data-ttu-id="1d898-328">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-328">A response will contain:</span></span>
         * <span data-ttu-id="1d898-329">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-329">a response code indicating the outcome of the operation</span></span>

<span data-ttu-id="1d898-330">Versión del protocolo *2.0.0* mensajes</span><span class="sxs-lookup"><span data-stu-id="1d898-330">Protocol Version *2.0.0* messages</span></span>

17. <span data-ttu-id="1d898-331">Obtener notificaciones de operación</span><span class="sxs-lookup"><span data-stu-id="1d898-331">Get Operation Claims</span></span>

* <span data-ttu-id="1d898-332">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-332">Request direction:  NuGet -> plugin</span></span>
    * <span data-ttu-id="1d898-333">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-333">The request will contain:</span></span>
        * <span data-ttu-id="1d898-334">el index.json de servicio para un origen del paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-334">the service index.json for a package source</span></span>
        * <span data-ttu-id="1d898-335">la ubicación del repositorio de origen de paquete</span><span class="sxs-lookup"><span data-stu-id="1d898-335">the package source repository location</span></span>
    * <span data-ttu-id="1d898-336">Contendrá una respuesta:</span><span class="sxs-lookup"><span data-stu-id="1d898-336">A response will contain:</span></span>
        * <span data-ttu-id="1d898-337">un código de respuesta que indica el resultado de la operación</span><span class="sxs-lookup"><span data-stu-id="1d898-337">a response code indicating the outcome of the operation</span></span>
        * <span data-ttu-id="1d898-338">una colección enumerable de las operaciones admitidas si la operación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="1d898-338">an enumerable of supported operations if the operation was successful.</span></span>  <span data-ttu-id="1d898-339">Si un complemento no es compatible con el origen del paquete, el complemento debe devolver un conjunto vacío de las operaciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="1d898-339">If a plugin does not support the package source, the plugin must return an empty set of supported operations.</span></span>

    <span data-ttu-id="1d898-340">Si el origen del índice y el paquete de servicio es null, el complemento puede responder con la autenticación.</span><span class="sxs-lookup"><span data-stu-id="1d898-340">If the service index and package source are null, then the plugin can answer with authentication.</span></span>

18. <span data-ttu-id="1d898-341">Obtener las credenciales de autenticación</span><span class="sxs-lookup"><span data-stu-id="1d898-341">Get Authentication Credentials</span></span>

* <span data-ttu-id="1d898-342">Solicitar dirección: NuGet -> plugin</span><span class="sxs-lookup"><span data-stu-id="1d898-342">Request direction: NuGet -> plugin</span></span>
* <span data-ttu-id="1d898-343">La solicitud contendrá:</span><span class="sxs-lookup"><span data-stu-id="1d898-343">The request will contain:</span></span>
    * <span data-ttu-id="1d898-344">URI</span><span class="sxs-lookup"><span data-stu-id="1d898-344">Uri</span></span>
    * <span data-ttu-id="1d898-345">isRetry</span><span class="sxs-lookup"><span data-stu-id="1d898-345">isRetry</span></span>
    * <span data-ttu-id="1d898-346">No interactivo</span><span class="sxs-lookup"><span data-stu-id="1d898-346">NonInteractive</span></span>
    * <span data-ttu-id="1d898-347">CanShowDialog</span><span class="sxs-lookup"><span data-stu-id="1d898-347">CanShowDialog</span></span>
* <span data-ttu-id="1d898-348">Contendrá una respuesta</span><span class="sxs-lookup"><span data-stu-id="1d898-348">A response will contain</span></span>
    * <span data-ttu-id="1d898-349">Nombre de usuario</span><span class="sxs-lookup"><span data-stu-id="1d898-349">Username</span></span>
    * <span data-ttu-id="1d898-350">Contraseña</span><span class="sxs-lookup"><span data-stu-id="1d898-350">Password</span></span>
    * <span data-ttu-id="1d898-351">Mensaje</span><span class="sxs-lookup"><span data-stu-id="1d898-351">Message</span></span>
    * <span data-ttu-id="1d898-352">Lista de tipos de autenticación</span><span class="sxs-lookup"><span data-stu-id="1d898-352">List of Auth Types</span></span>
    * <span data-ttu-id="1d898-353">MessageResponseCode</span><span class="sxs-lookup"><span data-stu-id="1d898-353">MessageResponseCode</span></span>