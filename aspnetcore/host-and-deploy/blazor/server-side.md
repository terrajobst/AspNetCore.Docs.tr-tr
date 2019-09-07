---
title: ASP.NET Core Blazor sunucu-tarafı barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu tarafı uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8da71faf6abc5929d6cd43d42fd896e378d99ef6
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773580"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="27658-103">Blazor sunucu tarafı barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="27658-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="27658-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="27658-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="27658-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="27658-105">Host configuration values</span></span>

<span data-ttu-id="27658-106">[Sunucu tarafı barındırma modelini](xref:blazor/hosting-models#server-side) kullanan sunucu tarafı uygulamalar [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="27658-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="27658-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="27658-107">Deployment</span></span>

<span data-ttu-id="27658-108">[Sunucu tarafı barındırma modeliyle](xref:blazor/hosting-models#server-side), Blazor bir ASP.NET Core uygulamasının içinden sunucuda yürütülür.</span><span class="sxs-lookup"><span data-stu-id="27658-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="27658-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="27658-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="27658-110">ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="27658-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="27658-111">Visual Studio, **Blazor Server uygulaması** proje şablonunu (`blazorserverside` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.</span><span class="sxs-lookup"><span data-stu-id="27658-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="27658-112">Bağlantı ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="27658-112">Connection scale out</span></span>

<span data-ttu-id="27658-113">Blazor sunucu tarafı uygulamalar, her kullanıcı için bir etkin SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="27658-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="27658-114">Üretim Blazor sunucu tarafı dağıtımı, uygulamanın gerektirdiği sayıda eşzamanlı bağlantıyı desteklemek için bir çözüm gerektirir.</span><span class="sxs-lookup"><span data-stu-id="27658-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="27658-115">[Azure SignalR hizmeti](/azure/azure-signalr/) bağlantıların ölçeklendirilmesini Işler ve Blazor sunucu tarafı uygulamaları için bir ölçeklendirme çözümü olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="27658-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="27658-116">Daha fazla bilgi için bkz. <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="27658-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="27658-117">SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="27658-117">SignalR configuration</span></span>

<span data-ttu-id="27658-118">SignalR, en yaygın Blazor sunucu tarafı senaryolar için ASP.NET Core tarafından yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="27658-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="27658-119">Özel ve gelişmiş senaryolar için [ek kaynaklar](#additional-resources) bölümündeki SignalR makalelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="27658-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27658-120">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="27658-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="27658-121">Azure SignalR hizmeti belgeleri</span><span class="sxs-lookup"><span data-stu-id="27658-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="27658-122">Hızlı Başlangıç: SignalR hizmetini kullanarak sohbet odası oluşturma</span><span class="sxs-lookup"><span data-stu-id="27658-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="27658-123">Azure App Service için ASP.NET Core Preview sürümünü dağıtın</span><span class="sxs-lookup"><span data-stu-id="27658-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
