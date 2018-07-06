---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Özel rota kısıtlaması (C#) oluşturma | Microsoft Docs
author: StephenWalther
description: Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz basit bir uygulama bir yolu olmasını önleyen özel kısıtlaması eşleşen w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831819"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="4cc23-104">Özel rota kısıtlaması (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="4cc23-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="4cc23-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4cc23-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4cc23-106">Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cc23-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="4cc23-107">Biz, bir tarayıcı isteğini bir uzak bilgisayardan yapıldığında eşleşen bir rota engelleyen basit bir özel kısıtlaması uygular.</span><span class="sxs-lookup"><span data-stu-id="4cc23-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="4cc23-108">Bu öğreticinin özel rota kısıtlaması nasıl oluşturacağınızı göstermek için hedeftir.</span><span class="sxs-lookup"><span data-stu-id="4cc23-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="4cc23-109">Özel rota kısıtlaması, bazı özel koşul sisteminkiyle eşleşmediği sürece eşleşen bir rota önlemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cc23-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="4cc23-110">Bu öğreticide, Localhost rota kısıtlaması oluştururuz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="4cc23-111">Localhost rota kısıtlaması, yalnızca yerel bilgisayardan yapılan istekleri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4cc23-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="4cc23-112">Internet üzerinden uzak isteklerinden karşılaştırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4cc23-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="4cc23-113">Özel rota kısıtlaması, IRouteConstraint arabirimi uygulayarak uygulayın.</span><span class="sxs-lookup"><span data-stu-id="4cc23-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="4cc23-114">Bu tek bir yöntemi açıklayan son derece basit bir arabirim.</span><span class="sxs-lookup"><span data-stu-id="4cc23-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="4cc23-115">Yöntemi, bir Boole değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cc23-115">The method returns a Boolean value.</span></span> <span data-ttu-id="4cc23-116">False döndürürse, rota kısıtlaması ile ilişkili bir tarayıcı isteğini eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="4cc23-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="4cc23-117">Localhost kısıtlaması listeleme 1'de yer alır.</span><span class="sxs-lookup"><span data-stu-id="4cc23-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="4cc23-118">**1 - LocalhostConstraint.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="4cc23-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="4cc23-119">1 listeleme kısıtlamasındaki HttpRequest sınıfını tarafından kullanıma sunulan IsLocal özelliği yararlanır.</span><span class="sxs-lookup"><span data-stu-id="4cc23-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="4cc23-120">Bu özellik, isteğin IP adresi ya da 127.0.0.1 olduğunda veya isteğin IP sunucunun IP adresi ile aynı olduğunda true değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cc23-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="4cc23-121">Bir rota Global.asax dosyasında tanımlanan özel bir kısıtlamada kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="4cc23-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="4cc23-122">2 listeleme Global.asax dosyasında Localhost kısıtlaması herhangi bir yönetim sayfası yerel sunucudan istek olmayacaksa istemesini engellemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cc23-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="4cc23-123">Örneğin, bir Uzak sunucudan yapıldığında /Admin/DeleteAll isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4cc23-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="4cc23-124">**2 - Global.asax listeleme**</span><span class="sxs-lookup"><span data-stu-id="4cc23-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="4cc23-125">Localhost kısıtlaması yönetici rota tanımında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cc23-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="4cc23-126">Bu yol, uzak tarayıcı isteğiyle eşleşen olmaz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="4cc23-127">Ancak, Global.asax dosyasında tanımlanan diğer yolları aynı istekte eşleşebilir farkında olun.</span><span class="sxs-lookup"><span data-stu-id="4cc23-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="4cc23-128">Bir isteği eşleşen dizinden bir kısıtlama belirli bir rota engeller ve tüm yollar Global.asax dosyasında tanımlanmış anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="4cc23-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="4cc23-129">Varsayılan rota Global.asax dosyasında listeleniyor 2'den yorum olarak belirtilmiştir, dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="4cc23-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="4cc23-130">Ardından varsayılan yolu içeriyorsa, varsayılan yol yönetim denetleyicisinin istekleri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4cc23-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="4cc23-131">Bu durumda, isteklerin yönetici rota ile eşleşmekte mıydı olsa bile uzak kullanıcılar yine de yönetim denetleyicisinin eylemleri çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4cc23-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cc23-132">[Önceki](creating-a-route-constraint-cs.md)
> [İleri](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4cc23-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
