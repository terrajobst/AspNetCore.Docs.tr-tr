---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: "Özel rota kısıtlaması (C#) oluşturma | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz basit bir uygulama bir rota engeller özel kısıtlama eşleşen w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="42a5a-104">Özel rota kısıtlaması (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="42a5a-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="42a5a-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="42a5a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="42a5a-106">Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="42a5a-107">Biz, bir tarayıcı isteğini bir uzak bilgisayardan yapıldığında eşleşen bir rota engeller basit bir özel kısıtlaması uygulamak.</span><span class="sxs-lookup"><span data-stu-id="42a5a-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="42a5a-108">Bu öğreticinin amacı özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir belirlemektir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="42a5a-109">Özel rota kısıtlaması, bazı özel koşul eşleşen sürece eşleşen bir rota engellemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="42a5a-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="42a5a-110">Bu öğreticide, bir Localhost rota kısıtlaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="42a5a-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="42a5a-111">Localhost rota kısıtlaması, yalnızca yerel bilgisayardan yapılan istekleri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="42a5a-112">Internet üzerinden uzaktan isteklerinden eşleşen değil.</span><span class="sxs-lookup"><span data-stu-id="42a5a-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="42a5a-113">Özel rota kısıtlaması IRouteConstraint arabirimi uygulayarak uygulayın.</span><span class="sxs-lookup"><span data-stu-id="42a5a-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="42a5a-114">Bu tek bir yöntem tanımlayan çok basit bir arabirim.</span><span class="sxs-lookup"><span data-stu-id="42a5a-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="42a5a-115">Yöntemi, bir Boole değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="42a5a-115">The method returns a Boolean value.</span></span> <span data-ttu-id="42a5a-116">False döndürürse kısıtlaması ile ilişkili yol tarayıcı isteğini eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="42a5a-117">Localhost kısıtlaması listeleme 1'de yer alır.</span><span class="sxs-lookup"><span data-stu-id="42a5a-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="42a5a-118">**1 - LocalhostConstraint.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="42a5a-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="42a5a-119">Listeleme 1 kısıtlamasında HttpRequest sınıfı tarafından kullanıma sunulan IsLocal özelliği yararlanır.</span><span class="sxs-lookup"><span data-stu-id="42a5a-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="42a5a-120">Bu özellik, istek IP adresini ya da 127.0.0.1 olduğunda veya istek IP sunucunun IP adresi ile aynı olduğunda true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="42a5a-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="42a5a-121">Bir rota Global.asax dosyasında tanımlanmış içindeki özel bir kısıtlaması kullanın.</span><span class="sxs-lookup"><span data-stu-id="42a5a-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="42a5a-122">Listeleme 2 Global.asax dosyasındaki Localhost kısıtlaması herhangi biri yerel sunucudan istek yaptıkları sürece bir yönetim sayfası istemesini engellemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="42a5a-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="42a5a-123">Örneğin, bir istek /Admin/DeleteAll için uzak bir sunucudan yapıldığında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="42a5a-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="42a5a-124">**2 - Global.asax listeleme**</span><span class="sxs-lookup"><span data-stu-id="42a5a-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="42a5a-125">Localhost kısıtlama yönetici rota tanımında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="42a5a-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="42a5a-126">Bu yol, bir uzak tarayıcı isteğiyle eşleşen olmaz.</span><span class="sxs-lookup"><span data-stu-id="42a5a-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="42a5a-127">Ancak, Global.asax dosyasında tanımlanmış diğer yollar aynı istekte eşleşebilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="42a5a-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="42a5a-128">Bir istek eşleşen belirli bir rota kısıtlama engeller ve tüm yollar Global.asax dosyasında tanımlanmış anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="42a5a-129">Varsayılan rota listeleme 2'deki Global.asax dosyasındaki yorum olarak belirtilmiştir, dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="42a5a-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="42a5a-130">Varsayılan rota eklerseniz, varsayılan yol yönetim denetleyicisi için istekleri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="42a5a-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="42a5a-131">Bu durumda, kendi isteklerini yönetici rota ile eşleşmekte olmayacaktır olsa da uzak kullanıcıların hala Eylemler yönetim denetleyicisinin çağıramadı.</span><span class="sxs-lookup"><span data-stu-id="42a5a-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="42a5a-132">[Önceki](creating-a-route-constraint-cs.md)
[sonraki](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="42a5a-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>