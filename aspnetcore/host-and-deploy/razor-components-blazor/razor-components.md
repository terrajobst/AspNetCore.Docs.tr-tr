---
title: Barındırma ve Razor bileşenleri dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Razor bileşenleri uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/razor-components
ms.openlocfilehash: 18b09dfc80e2f517c7750ce0fe1510641624aea7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069776"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="781e8-103">Barındırma ve Razor bileşenleri dağıtma</span><span class="sxs-lookup"><span data-stu-id="781e8-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="781e8-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="781e8-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="781e8-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="781e8-105">Host configuration values</span></span>

<span data-ttu-id="781e8-106">Razor bileşenleri kullanan uygulamalar [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="781e8-106">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="781e8-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="781e8-107">Deployment</span></span>

<span data-ttu-id="781e8-108">İle [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model), Razor bileşenleri sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="781e8-108">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="781e8-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="781e8-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="781e8-110">Yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte bir uygulamadır ve iki uygulamanın birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="781e8-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="781e8-111">ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="781e8-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="781e8-112">Sunucu tarafı dağıtımı için Visual Studio içerir **Razor bileşenleri** proje şablonu (`razorcomponents` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="781e8-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="781e8-113">ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="781e8-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="781e8-114">Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="781e8-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
