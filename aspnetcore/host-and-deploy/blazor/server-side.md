---
title: Barındırma ve ASP.NET Core Blazor sunucu tarafı dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core kullanarak Blazor sunucu-tarafı uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8b332c2fb439e9832d604ed26f972b266eed2507
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406117"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="492e4-103">Ana bilgisayar ve sunucu tarafı Blazor dağıtma</span><span class="sxs-lookup"><span data-stu-id="492e4-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="492e4-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="492e4-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="492e4-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="492e4-105">Host configuration values</span></span>

<span data-ttu-id="492e4-106">Kullanan bir sunucu tarafı uygulamalar [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side) kabul edebilir [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="492e4-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="492e4-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="492e4-107">Deployment</span></span>

<span data-ttu-id="492e4-108">İle [sunucu tarafı barındırma modeli](xref:blazor/hosting-models#server-side), Blazor sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="492e4-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="492e4-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="492e4-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="492e4-110">ASP.NET Core uygulaması barındırma uyumlu bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="492e4-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="492e4-111">Visual Studio içerir **Blazor (sunucu tarafı)** proje şablonu (`blazorserverside` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="492e4-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="492e4-112">Bağlantı ölçeği genişletme</span><span class="sxs-lookup"><span data-stu-id="492e4-112">Connection scale out</span></span>

<span data-ttu-id="492e4-113">Blazor sunucu tarafı uygulamalar, her kullanıcı için etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="492e4-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="492e4-114">Bir üretim Blazor sunucu tarafı dağıtım, uygulamanın gerektirdiği sayıda eş zamanlı bağlantı desteklemek için bir çözüm gerektirir.</span><span class="sxs-lookup"><span data-stu-id="492e4-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="492e4-115">[Azure SignalR hizmeti](/azure/azure-signalr/) bağlantıları ölçeklendirme işler ve Blazor sunucu tarafı uygulamalar için ölçeklendirme çözümü olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="492e4-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="492e4-116">Daha fazla bilgi için bkz. <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="492e4-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="492e4-117">SignalR yapılandırma</span><span class="sxs-lookup"><span data-stu-id="492e4-117">SignalR configuration</span></span>

<span data-ttu-id="492e4-118">SignalR en yaygın Blazor sunucu tarafı senaryolar için ASP.NET Core tarafından yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="492e4-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="492e4-119">Özel ve Gelişmiş senaryolar için SignalR makalelerinde başvurun [ek kaynaklar](#additional-resources) bölümü.</span><span class="sxs-lookup"><span data-stu-id="492e4-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="492e4-120">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="492e4-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="492e4-121">Azure SignalR hizmeti belgeleri</span><span class="sxs-lookup"><span data-stu-id="492e4-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="492e4-122">Hızlı Başlangıç: SignalR hizmetini kullanarak sohbet odası oluşturamadı.</span><span class="sxs-lookup"><span data-stu-id="492e4-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="492e4-123">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="492e4-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
