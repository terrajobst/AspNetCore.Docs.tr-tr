---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Özel yollar (C#) oluşturma | Microsoft Docs
author: microsoft
description: Bir ASP.NET MVC uygulaması için özel yollar eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="e9a32-104">Özel yollar oluşturma (C#)</span><span class="sxs-lookup"><span data-stu-id="e9a32-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="e9a32-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e9a32-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e9a32-106">Bir ASP.NET MVC uygulaması için özel yollar eklemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="e9a32-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="e9a32-107">Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme öğrenin.</span><span class="sxs-lookup"><span data-stu-id="e9a32-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="e9a32-108">Bu öğreticide, bir ASP.NET MVC uygulaması için özel bir yol ekleme konusunda bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="e9a32-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="e9a32-109">Varsayılan rota tablosu ile özel bir rota Global.asax dosyasında değişiklik öğrenin.</span><span class="sxs-lookup"><span data-stu-id="e9a32-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="e9a32-110">Birçok basit ASP.NET MVC uygulamaları için varsayılan rota tablosu düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="e9a32-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="e9a32-111">Ancak, yönlendirme gereksinimlerini özelleştirilmiş fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a32-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="e9a32-112">Bu durumda, özel bir yol oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a32-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="e9a32-113">Örneğin, bir blog uygulaması oluşturduğunuzu düşünün.</span><span class="sxs-lookup"><span data-stu-id="e9a32-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="e9a32-114">Şuna gelen istekleri işleyen isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e9a32-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="e9a32-115">/ Arşiv/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="e9a32-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="e9a32-116">Bir kullanıcı bu isteği girdiğinde, karşılık gelen blog girişine tarihe döndürmek istediğiniz 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="e9a32-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="e9a32-117">Bu tür bir isteği işlemek için özel bir yol oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9a32-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="e9a32-118">Blog, /Archive/ gibi görünen hangi istekleri işleme adlı yeni özel bir rota Global.asax dosyasında listeleniyor 1 içeren*giriş tarihi*.</span><span class="sxs-lookup"><span data-stu-id="e9a32-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="e9a32-119">**1 - Global.asax (özel rota ile) listeleme**</span><span class="sxs-lookup"><span data-stu-id="e9a32-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="e9a32-120">Yol tablosu ekleme yolları sıralama önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e9a32-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="e9a32-121">Bizim yeni özel Blog rota önce var olan varsayılan yol eklenir.</span><span class="sxs-lookup"><span data-stu-id="e9a32-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="e9a32-122">Sırasını tersine, varsayılan yolu her zaman yerine özel rota çağrılmadığı.</span><span class="sxs-lookup"><span data-stu-id="e9a32-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="e9a32-123">Özel Blog rota başlatır/arşiv/herhangi bir istek eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e9a32-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="e9a32-124">Bu nedenle, aşağıdaki URL'ler tümünün eşleşecek:</span><span class="sxs-lookup"><span data-stu-id="e9a32-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="e9a32-125">/ Arşiv/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="e9a32-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="e9a32-126">/ Arşiv/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="e9a32-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="e9a32-127">/ Arşiv/apple</span><span class="sxs-lookup"><span data-stu-id="e9a32-127">/Archive/apple</span></span>

<span data-ttu-id="e9a32-128">Özel rota gelen istek arşiv adlı bir denetleyiciye eşler ve Entry() eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="e9a32-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="e9a32-129">Giriş tarihi Entry() yöntemi çağrıldığında entryDate adlı bir parametre geçirilir.</span><span class="sxs-lookup"><span data-stu-id="e9a32-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="e9a32-130">2. listeleme denetleyicisiyle Blog özel rota kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a32-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="e9a32-131">**2 - ArchiveController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="e9a32-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="e9a32-132">Listeleme 2 Entry() yönteminde DateTime türünde bir parametre kabul eden dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e9a32-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="e9a32-133">MVC çerçevesi bir DateTime değerine URL'den giriş tarihi otomatik olarak dönüştürmek akıllıca olur.</span><span class="sxs-lookup"><span data-stu-id="e9a32-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="e9a32-134">URL giriş tarihi parametresinden DateTime olarak dönüştürülemiyorsa bir hata oluşturulur (bkz: Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="e9a32-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="e9a32-135">**Şekil 1 - parametre dönüştürme gelen hata**</span><span class="sxs-lookup"><span data-stu-id="e9a32-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="e9a32-136">[![Yeni Proje iletişim kutusu](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e9a32-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="e9a32-137">**Şekil 01**: parametre dönüştürme gelen hata ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e9a32-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="e9a32-138">Özet</span><span class="sxs-lookup"><span data-stu-id="e9a32-138">Summary</span></span>

<span data-ttu-id="e9a32-139">Özel rota nasıl oluşturabileceğinizi göstermek için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="e9a32-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="e9a32-140">Yol tablosu blog girdileri temsil eder Global.asax dosyasında özel bir rota eklemek öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a32-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="e9a32-141">Biz ArchiveController adlı bir denetleyici ve Entry() adlı bir denetleyici eylemi için blog girdileri isteklerinde eşlemek açıklanır.</span><span class="sxs-lookup"><span data-stu-id="e9a32-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9a32-142">[Önceki](aspnet-mvc-controllers-overview-cs.md)
> [sonraki](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e9a32-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
