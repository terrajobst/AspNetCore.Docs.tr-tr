---
title: Ana bilgisayar ve sunucu tarafı Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Blazor sunucu-tarafı uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901049"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="9d5c1-103">Ana bilgisayar ve sunucu tarafı Blazor dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d5c1-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="9d5c1-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9d5c1-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9d5c1-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="9d5c1-105">Host configuration values</span></span>

<span data-ttu-id="9d5c1-106">Kullanan bir sunucu tarafı uygulamalar [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="9d5c1-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="9d5c1-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="9d5c1-107">Deployment</span></span>

<span data-ttu-id="9d5c1-108">İle [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side), Blazor sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="9d5c1-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="9d5c1-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9d5c1-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="9d5c1-110">ASP.NET Core uygulaması barındırma uyumlu bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="9d5c1-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="9d5c1-111">Visual Studio içerir **Blazor (sunucu tarafı)** proje şablonu (`blazorserverside` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="9d5c1-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="9d5c1-112">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9d5c1-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="9d5c1-113">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d5c1-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
