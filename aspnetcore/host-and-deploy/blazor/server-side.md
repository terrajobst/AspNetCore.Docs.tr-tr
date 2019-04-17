---
title: Ana bilgisayar ve sunucu tarafı Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Blazor sunucu-tarafı uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614915"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="e658c-103">Ana bilgisayar ve sunucu tarafı Blazor dağıtma</span><span class="sxs-lookup"><span data-stu-id="e658c-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="e658c-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e658c-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="e658c-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="e658c-105">Host configuration values</span></span>

<span data-ttu-id="e658c-106">Kullanan bir sunucu tarafı uygulamalar [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side-hosting-model) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="e658c-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="e658c-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="e658c-107">Deployment</span></span>

<span data-ttu-id="e658c-108">İle [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side-hosting-model), Blazor sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e658c-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="e658c-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="e658c-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="e658c-110">Yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte bir uygulamadır ve iki uygulamanın birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="e658c-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="e658c-111">ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e658c-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="e658c-112">Sunucu tarafı dağıtımı için Visual Studio içerir **Razor bileşenleri** proje şablonu (`razorcomponents` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="e658c-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="e658c-113">ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="e658c-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="e658c-114">Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="e658c-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
