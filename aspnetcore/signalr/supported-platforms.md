---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: tdykstra
description: ASP.NET Core SignalR için desteklenen platformlar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577632"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="64fb3-103">ASP.NET Core SignalR tarafından desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="64fb3-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="64fb3-104">Sunucu sistem gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="64fb3-104">Server system requirements</span></span>

<span data-ttu-id="64fb3-105">SignalR için ASP.NET Core, ASP.NET Core destekleyen herhangi bir sunucu platformu destekler.</span><span class="sxs-lookup"><span data-stu-id="64fb3-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="64fb3-106">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="64fb3-106">JavaScript client</span></span>

<span data-ttu-id="64fb3-107">[JavaScript istemci](https://www.npmjs.com/package/@aspnet/signalr) NodeJS 8 ve sonraki sürümleri ve aşağıdaki tarayıcılardan çalışır:</span><span class="sxs-lookup"><span data-stu-id="64fb3-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="64fb3-108">Tarayıcı</span><span class="sxs-lookup"><span data-stu-id="64fb3-108">Browser</span></span> | <span data-ttu-id="64fb3-109">Sürüm</span><span class="sxs-lookup"><span data-stu-id="64fb3-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="64fb3-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="64fb3-110">Microsoft Edge</span></span> | <span data-ttu-id="64fb3-111">geçerli</span><span class="sxs-lookup"><span data-stu-id="64fb3-111">current</span></span> |
| <span data-ttu-id="64fb3-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="64fb3-112">Mozilla Firefox</span></span> | <span data-ttu-id="64fb3-113">geçerli</span><span class="sxs-lookup"><span data-stu-id="64fb3-113">current</span></span> |
| <span data-ttu-id="64fb3-114">Google Chrome; Android içerir</span><span class="sxs-lookup"><span data-stu-id="64fb3-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="64fb3-115">geçerli</span><span class="sxs-lookup"><span data-stu-id="64fb3-115">current</span></span> |
| <span data-ttu-id="64fb3-116">Safari; iOS içerir</span><span class="sxs-lookup"><span data-stu-id="64fb3-116">Safari; includes iOS</span></span> | <span data-ttu-id="64fb3-117">geçerli</span><span class="sxs-lookup"><span data-stu-id="64fb3-117">current</span></span> |
| <span data-ttu-id="64fb3-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="64fb3-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="64fb3-119">11</span><span class="sxs-lookup"><span data-stu-id="64fb3-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="64fb3-120">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="64fb3-120">.NET client</span></span>

<span data-ttu-id="64fb3-121">[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen herhangi bir sunucu platformu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="64fb3-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="64fb3-122">Server IIS çalıştığında, Windows Server 2012 veya üzeri sürümde WebSockets taşıma IIS 8.0 veya sonraki bir sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="64fb3-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="64fb3-123">Diğer aktarımları tüm platformlarda desteklenir.</span><span class="sxs-lookup"><span data-stu-id="64fb3-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="64fb3-124">Java istemci</span><span class="sxs-lookup"><span data-stu-id="64fb3-124">Java client</span></span>

<span data-ttu-id="64fb3-125">[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="64fb3-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="64fb3-126">Desteklenmeyen İstemcileri</span><span class="sxs-lookup"><span data-stu-id="64fb3-126">Unsupported clients</span></span>

<span data-ttu-id="64fb3-127">Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla.</span><span class="sxs-lookup"><span data-stu-id="64fb3-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="64fb3-128">Bunlar artık desteklenmez ve her zamankinden desteklenmiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="64fb3-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="64fb3-129">C++ istemci</span><span class="sxs-lookup"><span data-stu-id="64fb3-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="64fb3-130">SWIFT istemci</span><span class="sxs-lookup"><span data-stu-id="64fb3-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
