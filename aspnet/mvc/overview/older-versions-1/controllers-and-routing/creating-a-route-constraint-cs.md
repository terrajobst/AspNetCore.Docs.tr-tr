---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Rota kısıtlaması (C#) oluşturma | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: f0dbbb880bc679da934444b87ef24d040b69e2f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828260"
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="8deaf-103">Rota kısıtlaması (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="8deaf-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="8deaf-104">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8deaf-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8deaf-105">Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir.</span><span class="sxs-lookup"><span data-stu-id="8deaf-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="8deaf-106">Rota kısıtlamalarını belirli bir rota ile eşleşmekte tarayıcı istekleri kısıtlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8deaf-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="8deaf-107">Rota kısıtlaması belirtmek için normal bir ifade kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8deaf-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="8deaf-108">Örneğin, rota, Global.asax dosyasında listeleniyor 1'de tanımladığınız düşünün.</span><span class="sxs-lookup"><span data-stu-id="8deaf-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="8deaf-109">**1 - Global.asax.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="8deaf-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="8deaf-110">Kod 1 ürün adlı bir rota içerir.</span><span class="sxs-lookup"><span data-stu-id="8deaf-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="8deaf-111">Tarayıcı istek listeleme 2'de yer alan ProductController eşlemek için ürün yol kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8deaf-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="8deaf-112">**2 - Controllers\ProductController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="8deaf-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="8deaf-113">Ürün denetleyicisi tarafından kullanıma sunulan Details() eylem ProductID adlı tek bir parametre kabul ettiğini dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8deaf-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="8deaf-114">Bu parametre, bir tamsayı parametresidir.</span><span class="sxs-lookup"><span data-stu-id="8deaf-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="8deaf-115">Listeleme 1'de tanımlanan yol, aşağıdaki URL'lerden herhangi birini eşleşmesi:</span><span class="sxs-lookup"><span data-stu-id="8deaf-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="8deaf-116">/ Ürün/23</span><span class="sxs-lookup"><span data-stu-id="8deaf-116">/Product/23</span></span>
- <span data-ttu-id="8deaf-117">Ürün/7</span><span class="sxs-lookup"><span data-stu-id="8deaf-117">/Product/7</span></span>

<span data-ttu-id="8deaf-118">Ne yazık ki, yol aşağıdaki URL'ler eşleşir:</span><span class="sxs-lookup"><span data-stu-id="8deaf-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="8deaf-119">/ Ürün/filan</span><span class="sxs-lookup"><span data-stu-id="8deaf-119">/Product/blah</span></span>
- <span data-ttu-id="8deaf-120">/ Ürün/apple</span><span class="sxs-lookup"><span data-stu-id="8deaf-120">/Product/apple</span></span>

<span data-ttu-id="8deaf-121">Bir tamsayı değeri dışında bir şey içeren bir istekte Details() eylemi bir tam sayı parametresi beklediği hataya neden olur.</span><span class="sxs-lookup"><span data-stu-id="8deaf-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="8deaf-122">URL /Product/apple tarayıcınıza yazın örneğin, daha sonra hata sayfası Şekil 1'de alırsınız.</span><span class="sxs-lookup"><span data-stu-id="8deaf-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="8deaf-123">[![Yeni Proje iletişim kutusu](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8deaf-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="8deaf-124">**Şekil 01**: explode bir sayfayı görmeden ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8deaf-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="8deaf-125">Ne yapmak istediğinizden, yalnızca uygun tamsayı ProductID içeren URL'ler aynı olur.</span><span class="sxs-lookup"><span data-stu-id="8deaf-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="8deaf-126">Rota ile eşleşmekte URL'leri kısıtlamak için bir rota tanımlarken bir kısıtlama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8deaf-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="8deaf-127">Yalnızca tamsayılara eşleşen bir normal ifade kısıtlaması listeleme 3'te değiştirilmiş ürün yol içerir.</span><span class="sxs-lookup"><span data-stu-id="8deaf-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="8deaf-128">**3 - Global.asax.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="8deaf-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="8deaf-129">Normal ifade \d+ tamsayılar bir veya daha fazla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="8deaf-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="8deaf-130">Bu sınırlama aşağıdaki URL'ler ile eşleştirilecek ürün yol neden olur:</span><span class="sxs-lookup"><span data-stu-id="8deaf-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="8deaf-131">/ Ürün/3</span><span class="sxs-lookup"><span data-stu-id="8deaf-131">/Product/3</span></span>
- <span data-ttu-id="8deaf-132">/ Ürün/8999</span><span class="sxs-lookup"><span data-stu-id="8deaf-132">/Product/8999</span></span>

<span data-ttu-id="8deaf-133">Ancak aşağıdaki URL'leri:</span><span class="sxs-lookup"><span data-stu-id="8deaf-133">But not the following URLs:</span></span>

- <span data-ttu-id="8deaf-134">/ Ürün/apple</span><span class="sxs-lookup"><span data-stu-id="8deaf-134">/Product/apple</span></span>
- <span data-ttu-id="8deaf-135">/ Ürün</span><span class="sxs-lookup"><span data-stu-id="8deaf-135">/Product</span></span>

- <span data-ttu-id="8deaf-136">Bu tarayıcı istekleri başka bir yol tarafından işlenecek veya eşleşen yol yok, ise bir *kaynak bulunamadı* hata döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8deaf-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8deaf-137">[Önceki](creating-custom-routes-cs.md)
> [İleri](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8deaf-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
