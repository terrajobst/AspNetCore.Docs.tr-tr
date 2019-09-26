---
title: SignalR istemci özellikleri
author: bradygaster
description: Çeşitli ASP.NET Core SignalR istemcileri tarafından desteklenen özellikleri öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301185"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="1b40b-103">ASP.NET Core SignalR istemci özellikleri</span><span class="sxs-lookup"><span data-stu-id="1b40b-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="1b40b-104">Özellik dağıtımı</span><span class="sxs-lookup"><span data-stu-id="1b40b-104">Feature distribution</span></span>

<span data-ttu-id="1b40b-105">Aşağıdaki tabloda, gerçek zamanlı destek sunan istemciler için özellikler ve destek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1b40b-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="1b40b-106">Özellik</span><span class="sxs-lookup"><span data-stu-id="1b40b-106">Feature</span></span> | <span data-ttu-id="1b40b-107">.NET</span><span class="sxs-lookup"><span data-stu-id="1b40b-107">.NET</span></span> | <span data-ttu-id="1b40b-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1b40b-108">JavaScript</span></span> | <span data-ttu-id="1b40b-109">Java</span><span class="sxs-lookup"><span data-stu-id="1b40b-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="1b40b-110">Azure SignalR hizmeti desteği</span><span class="sxs-lookup"><span data-stu-id="1b40b-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="1b40b-111">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-111">✔</span></span>|<span data-ttu-id="1b40b-112">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-112">✔</span></span>|<span data-ttu-id="1b40b-113">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-113">✔</span></span>|
| [<span data-ttu-id="1b40b-114">Sunucudan istemciye akış</span><span class="sxs-lookup"><span data-stu-id="1b40b-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="1b40b-115">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-115">✔</span></span>|<span data-ttu-id="1b40b-116">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-116">✔</span></span>|<span data-ttu-id="1b40b-117">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-117">✔</span></span>|
| [<span data-ttu-id="1b40b-118">İstemciden sunucuya akış</span><span class="sxs-lookup"><span data-stu-id="1b40b-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="1b40b-119">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-119">✔</span></span>|<span data-ttu-id="1b40b-120">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-120">✔</span></span>|<span data-ttu-id="1b40b-121">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-121">✔</span></span>|
| <span data-ttu-id="1b40b-122">Otomatik yeniden bağlanma ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="1b40b-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="1b40b-123">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-123">✔</span></span>|<span data-ttu-id="1b40b-124">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-124">✔</span></span>| |
| <span data-ttu-id="1b40b-125">WebSockets taşıma</span><span class="sxs-lookup"><span data-stu-id="1b40b-125">WebSockets Transport</span></span> |<span data-ttu-id="1b40b-126">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-126">✔</span></span>|<span data-ttu-id="1b40b-127">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-127">✔</span></span>|<span data-ttu-id="1b40b-128">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-128">✔</span></span>|
| <span data-ttu-id="1b40b-129">Sunucu tarafından gönderilen olay aktarımı</span><span class="sxs-lookup"><span data-stu-id="1b40b-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="1b40b-130">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-130">✔</span></span>|<span data-ttu-id="1b40b-131">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-131">✔</span></span>| |
| <span data-ttu-id="1b40b-132">Uzun yoklama taşıması</span><span class="sxs-lookup"><span data-stu-id="1b40b-132">Long Polling Transport</span></span> |<span data-ttu-id="1b40b-133">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-133">✔</span></span>|<span data-ttu-id="1b40b-134">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-134">✔</span></span>|<span data-ttu-id="1b40b-135">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-135">✔</span></span>|
| <span data-ttu-id="1b40b-136">JSON hub 'ı Protokolü</span><span class="sxs-lookup"><span data-stu-id="1b40b-136">JSON Hub Protocol</span></span> |<span data-ttu-id="1b40b-137">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-137">✔</span></span>|<span data-ttu-id="1b40b-138">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-138">✔</span></span>|<span data-ttu-id="1b40b-139">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-139">✔</span></span>|
| <span data-ttu-id="1b40b-140">MessagePack Hub Protokolü</span><span class="sxs-lookup"><span data-stu-id="1b40b-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="1b40b-141">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-141">✔</span></span>|<span data-ttu-id="1b40b-142">✔</span><span class="sxs-lookup"><span data-stu-id="1b40b-142">✔</span></span>| |

<span data-ttu-id="1b40b-143">Java istemcisinde otomatik yeniden bağlanma desteği, [sorun izleyici](https://github.com/aspnet/AspNetCore/issues/8711)'de izlenir.</span><span class="sxs-lookup"><span data-stu-id="1b40b-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1b40b-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1b40b-144">Additional resources</span></span>

* [<span data-ttu-id="1b40b-145">ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1b40b-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1b40b-146">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="1b40b-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="1b40b-147">Merkezler</span><span class="sxs-lookup"><span data-stu-id="1b40b-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1b40b-148">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1b40b-148">JavaScript client</span></span>](xref:signalr/javascript-client)
