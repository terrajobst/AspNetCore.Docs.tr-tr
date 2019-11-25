---
title: Desteklenen SignalR platformları ASP.NET Core
author: bradygaster
description: ASP.NET Core SignalRiçin desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 9b9cf1d57d61c333c485f23b7ab952c66814d2aa
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317474"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="b59d1-103">Desteklenen SignalR platformları ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b59d1-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="b59d1-104">Sunucu sistem gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="b59d1-104">Server system requirements</span></span>

<span data-ttu-id="b59d1-105">ASP.NET Core için SignalR, ASP.NET Core desteklediği tüm sunucu platformunu destekler.</span><span class="sxs-lookup"><span data-stu-id="b59d1-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="b59d1-106">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="b59d1-106">JavaScript client</span></span>

<span data-ttu-id="b59d1-107">[JavaScript Istemcisi](xref:signalr/javascript-client) NodeJS 8 ve sonraki sürümlerde ve aşağıdaki tarayıcılarda çalışır:</span><span class="sxs-lookup"><span data-stu-id="b59d1-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="b59d1-108">Browser</span><span class="sxs-lookup"><span data-stu-id="b59d1-108">Browser</span></span>                         | <span data-ttu-id="b59d1-109">Sürüm</span><span class="sxs-lookup"><span data-stu-id="b59d1-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="b59d1-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="b59d1-110">Microsoft Edge</span></span>                  | <span data-ttu-id="b59d1-111">Geçerli&dagger;</span><span class="sxs-lookup"><span data-stu-id="b59d1-111">Current&dagger;</span></span> |
| <span data-ttu-id="b59d1-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="b59d1-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="b59d1-113">Geçerli&dagger;</span><span class="sxs-lookup"><span data-stu-id="b59d1-113">Current&dagger;</span></span> |
| <span data-ttu-id="b59d1-114">Google Chrome; Android içerir</span><span class="sxs-lookup"><span data-stu-id="b59d1-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="b59d1-115">Geçerli&dagger;</span><span class="sxs-lookup"><span data-stu-id="b59d1-115">Current&dagger;</span></span> |
| <span data-ttu-id="b59d1-116">Uygulamasını iOS içerir</span><span class="sxs-lookup"><span data-stu-id="b59d1-116">Safari; includes iOS</span></span>            | <span data-ttu-id="b59d1-117">Geçerli&dagger;</span><span class="sxs-lookup"><span data-stu-id="b59d1-117">Current&dagger;</span></span> |
| <span data-ttu-id="b59d1-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b59d1-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="b59d1-119">11</span><span class="sxs-lookup"><span data-stu-id="b59d1-119">11</span></span>              |

<span data-ttu-id="b59d1-120">&dagger;*geçerli* , tarayıcının en son sürümünü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="b59d1-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="b59d1-121">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="b59d1-121">.NET client</span></span>

<span data-ttu-id="b59d1-122">[.Net Client](xref:signalr/dotnet-client) , ASP.NET Core tarafından desteklenen herhangi bir platformda çalışır.</span><span class="sxs-lookup"><span data-stu-id="b59d1-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="b59d1-123">Örneğin, Xamarin geliştiricileri, Xamarin. iOS 8.4.0.1 ve üstünü kullanarak Android uygulamaları oluşturmak için [SignalRkullanabilir](https://github.com/aspnet/Announcements/issues/305) ve Xamarin. iOS 11.14.0.4 ve üstünü kullanarak iOS uygulamaları oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="b59d1-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="b59d1-124">Sunucu IIS çalıştırıyorsa, WebSockets aktarımı Windows Server 2012 veya üzeri sürümlerde IIS 8,0 veya sonraki bir sürümü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b59d1-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="b59d1-125">Diğer aktarımlar tüm platformlarda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b59d1-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="b59d1-126">Java istemcisi</span><span class="sxs-lookup"><span data-stu-id="b59d1-126">Java client</span></span>

<span data-ttu-id="b59d1-127">[Java istemcisi](xref:signalr/java-client) , Java 8 ve sonraki sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="b59d1-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="b59d1-128">Desteklenmeyen istemciler</span><span class="sxs-lookup"><span data-stu-id="b59d1-128">Unsupported clients</span></span>

<span data-ttu-id="b59d1-129">Aşağıdaki istemciler mevcuttur, ancak deneysel veya resmi olmayan bir.</span><span class="sxs-lookup"><span data-stu-id="b59d1-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="b59d1-130">Şu anda desteklenmemektedir ve hiçbir şekilde bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b59d1-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="b59d1-131">[C++istemcilerinin](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span><span class="sxs-lookup"><span data-stu-id="b59d1-131">[C++ client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span></span>

* <span data-ttu-id="b59d1-132">[Swift istemcisi](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="b59d1-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
