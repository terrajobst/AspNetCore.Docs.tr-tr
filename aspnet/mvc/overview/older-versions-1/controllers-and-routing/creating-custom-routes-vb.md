---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Özel rotalar (VB) oluşturma | Microsoft Docs
author: microsoft
description: Özel rotalar için bir ASP.NET MVC uygulaması eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme konusunda bilgi edinin.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: cd7ad46161b3f44d915bc4b2fa201606b00d194a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754976"
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="18d1a-104">Özel rotalar oluşturma (VB)</span><span class="sxs-lookup"><span data-stu-id="18d1a-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="18d1a-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="18d1a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="18d1a-106">Özel rotalar için bir ASP.NET MVC uygulaması eklemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="18d1a-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="18d1a-107">Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme konusunda bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="18d1a-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="18d1a-108">Bu öğreticide bir ASP.NET MVC uygulaması için özel bir yol ekleme konusunda bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="18d1a-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="18d1a-109">Varsayılan rota tablosu ile özel bir rota Global.asax dosyasında değişiklik öğrenin.</span><span class="sxs-lookup"><span data-stu-id="18d1a-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="18d1a-110">ASP.NET MVC uygulamalarında varsayılan rota tablosu düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="18d1a-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="18d1a-111">Ancak, yönlendirme gereksinimleri özelleştirilmiş keşfedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18d1a-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="18d1a-112">Bu durumda, özel bir yol oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18d1a-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="18d1a-113">Örneğin, bir blog uygulaması oluşturmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="18d1a-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="18d1a-114">Şuna gelen istekleri işlemek isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="18d1a-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="18d1a-115">/ Arşiv/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="18d1a-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="18d1a-116">Bir kullanıcı bu isteği girdiğinde, karşılık gelen blog girişine tarihe iade etmek istediğiniz 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="18d1a-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="18d1a-117">Bu tür bir isteği işlemek için özel bir yol oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="18d1a-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="18d1a-118">Blog, /Archive/ gibi ara hangi istekleri işleme adlı yeni bir özel rota Global.asax dosyasında listeleniyor 1 içeren*girdisi tarihi*.</span><span class="sxs-lookup"><span data-stu-id="18d1a-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="18d1a-119">**1 - Global.asax (özel yönlendirme ile) listeleme**</span><span class="sxs-lookup"><span data-stu-id="18d1a-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="18d1a-120">Yol tablosuna eklediğiniz rotaları sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="18d1a-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="18d1a-121">Sunduğumuz yeni özel Blog rota önce var olan varsayılan yol eklenir.</span><span class="sxs-lookup"><span data-stu-id="18d1a-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="18d1a-122">Sıra ters, varsayılan yolu her zaman özel yol yerine çağrılmadığı.</span><span class="sxs-lookup"><span data-stu-id="18d1a-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="18d1a-123">Özel rota Blog/arşiv/ile başlayan herhangi bir istek eşleşir.</span><span class="sxs-lookup"><span data-stu-id="18d1a-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="18d1a-124">Bu nedenle, aşağıdaki URL'ler tümünün eşleşecek:</span><span class="sxs-lookup"><span data-stu-id="18d1a-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="18d1a-125">/ Arşiv/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="18d1a-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="18d1a-126">/ Arşiv/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="18d1a-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="18d1a-127">/ Arşiv/apple</span><span class="sxs-lookup"><span data-stu-id="18d1a-127">/Archive/apple</span></span>

<span data-ttu-id="18d1a-128">Özel rota, gelen istek arşiv adlı bir denetleyiciye eşler ve Entry() eylemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="18d1a-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="18d1a-129">Entry() yöntemi çağrıldığında, giriş tarihi entryDate adlı bir parametre olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="18d1a-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="18d1a-130">Blog özel rota denetleyicisiyle listeleme 2'de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18d1a-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="18d1a-131">**2 - ArchiveController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="18d1a-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="18d1a-132">Listeleme 2 Entry() yöntemi DateTime türünde bir parametre kabul ettiğini dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="18d1a-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="18d1a-133">MVC çerçevesi bir DateTime değerine URL'den girdisi tarihi otomatik olarak dönüştürülmesi akıllı.</span><span class="sxs-lookup"><span data-stu-id="18d1a-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="18d1a-134">URL'den giriş tarihi parametresi bir DateTime türüne dönüştürülemiyor, bir hata ortaya çıkar (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="18d1a-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="18d1a-135">**Şekil 1 - gelen parametre dönüştürme hatası**</span><span class="sxs-lookup"><span data-stu-id="18d1a-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="18d1a-136">[![Yeni Proje iletişim kutusu](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="18d1a-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="18d1a-137">**Şekil 01**: gelen parametre dönüştürme hatası ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="18d1a-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="18d1a-138">Özet</span><span class="sxs-lookup"><span data-stu-id="18d1a-138">Summary</span></span>

<span data-ttu-id="18d1a-139">Bu öğreticinin amacı, bir özel yol nasıl oluşturacağınızı göstermek için oluştu.</span><span class="sxs-lookup"><span data-stu-id="18d1a-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="18d1a-140">Blog girişleri temsil eder Global.asax dosyasındaki yol tablosuna bir özel yol eklemek öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="18d1a-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="18d1a-141">Blog girişleri isteklerinde ArchiveController adlı bir denetleyici ve Entry() adlı bir denetleyici eylemi için eşleme ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="18d1a-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="18d1a-142">[Önceki](asp-net-mvc-controller-overview-vb.md)
> [İleri](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="18d1a-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
