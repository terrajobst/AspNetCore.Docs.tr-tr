---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: bradygaster
description: ASP.NET Core SignalR için desteklenen platformlar ile ilgili öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: e4e84baf0120036b473eac256107b46a4accfe37
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837721"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="96cae-103">ASP.NET Core SignalR tarafından desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="96cae-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="96cae-104">Sunucu sistem gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="96cae-104">Server system requirements</span></span>

<span data-ttu-id="96cae-105">SignalR için ASP.NET Core, ASP.NET Core destekleyen herhangi bir sunucu platformu destekler.</span><span class="sxs-lookup"><span data-stu-id="96cae-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="96cae-106">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="96cae-106">JavaScript client</span></span>

<span data-ttu-id="96cae-107">[JavaScript istemci](https://www.npmjs.com/package/@aspnet/signalr) NodeJS 8 ve sonraki sürümleri ve aşağıdaki tarayıcılardan çalışır:</span><span class="sxs-lookup"><span data-stu-id="96cae-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="96cae-108">Tarayıcı</span><span class="sxs-lookup"><span data-stu-id="96cae-108">Browser</span></span>                         | <span data-ttu-id="96cae-109">Sürüm</span><span class="sxs-lookup"><span data-stu-id="96cae-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="96cae-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="96cae-110">Microsoft Edge</span></span>                  | <span data-ttu-id="96cae-111">geçerli</span><span class="sxs-lookup"><span data-stu-id="96cae-111">current</span></span> |
| <span data-ttu-id="96cae-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="96cae-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="96cae-113">geçerli</span><span class="sxs-lookup"><span data-stu-id="96cae-113">current</span></span> |
| <span data-ttu-id="96cae-114">Google Chrome; Android içerir</span><span class="sxs-lookup"><span data-stu-id="96cae-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="96cae-115">geçerli</span><span class="sxs-lookup"><span data-stu-id="96cae-115">current</span></span> |
| <span data-ttu-id="96cae-116">Safari; iOS içerir</span><span class="sxs-lookup"><span data-stu-id="96cae-116">Safari; includes iOS</span></span>            | <span data-ttu-id="96cae-117">geçerli</span><span class="sxs-lookup"><span data-stu-id="96cae-117">current</span></span> |
| <span data-ttu-id="96cae-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="96cae-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="96cae-119">11</span><span class="sxs-lookup"><span data-stu-id="96cae-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="96cae-120">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="96cae-120">.NET client</span></span>

<span data-ttu-id="96cae-121">[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="96cae-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="96cae-122">Örneğin, [Xamarin geliştiricilerine SignalR kullanabileceğiniz](https://github.com/aspnet/Announcements/issues/305) Xamarin.Android 8.4.0.1 kullanarak Android uygulamaları oluşturmak ve daha sonra ve Xamarin.iOS 11.14.0.4 kullanarak iOS uygulamaları ve daha sonra.</span><span class="sxs-lookup"><span data-stu-id="96cae-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="96cae-123">IIS sunucusu çalıştırıyorsa, IIS 8.0 veya Windows Server 2012'de daha yüksek veya daha yüksek WebSockets aktarım gerektirir.</span><span class="sxs-lookup"><span data-stu-id="96cae-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="96cae-124">Diğer aktarımları tüm platformlarda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="96cae-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="96cae-125">Java istemci</span><span class="sxs-lookup"><span data-stu-id="96cae-125">Java client</span></span>

<span data-ttu-id="96cae-126">[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="96cae-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="96cae-127">Desteklenmeyen İstemcileri</span><span class="sxs-lookup"><span data-stu-id="96cae-127">Unsupported clients</span></span>

<span data-ttu-id="96cae-128">Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla.</span><span class="sxs-lookup"><span data-stu-id="96cae-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="96cae-129">Bunlar, şu anda desteklenmez ve hiçbir zaman olabilir.</span><span class="sxs-lookup"><span data-stu-id="96cae-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="96cae-130">C++ istemci</span><span class="sxs-lookup"><span data-stu-id="96cae-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="96cae-131">SWIFT istemci</span><span class="sxs-lookup"><span data-stu-id="96cae-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
