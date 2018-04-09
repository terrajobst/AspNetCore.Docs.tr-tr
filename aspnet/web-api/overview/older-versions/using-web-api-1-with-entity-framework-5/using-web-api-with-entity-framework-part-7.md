---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: '7. Kısım: ana oluşturma sayfası | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="81838-102">7. Kısım: ana oluşturma sayfası</span><span class="sxs-lookup"><span data-stu-id="81838-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="81838-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81838-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="81838-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="81838-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="81838-105">Ana oluşturma sayfası</span><span class="sxs-lookup"><span data-stu-id="81838-105">Creating the Main Page</span></span>

<span data-ttu-id="81838-106">Bu bölümde, ana uygulama sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="81838-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="81838-107">Biz, birkaç adımda yaklaşımını böylece bu sayfayı yönetici sayfadan daha karmaşık olacaktır.</span><span class="sxs-lookup"><span data-stu-id="81838-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="81838-108">Yol boyunca daha gelişmiş bazı Knockout.js teknikleri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="81838-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="81838-109">Sayfa temel düzenini şöyledir:</span><span class="sxs-lookup"><span data-stu-id="81838-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="81838-110">"Ürünler" ürünleri dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="81838-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="81838-111">"Sepeti" miktarları ürünleriyle dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="81838-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="81838-112">"Sepete Ekle" tıklatarak Sepeti güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="81838-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="81838-113">"Siparişler" sipariş kimlikleri bir dizi tutar.</span><span class="sxs-lookup"><span data-stu-id="81838-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="81838-114">Bir dizi öğelerinin (miktarları ürünleriyle) bir Sipariş Ayrıntısı "Ayrıntılar" tutar</span><span class="sxs-lookup"><span data-stu-id="81838-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="81838-115">Veri bağlama ya da komut dosyası HTML'de, bazı temel düzeni tanımlayarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="81838-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="81838-116">Views/Home/Index.cshtml dosyasını açın ve tüm içeriğini aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="81838-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="81838-117">Ardından, komut dosyaları Bölüm Ekle ve boş bir görünüm model oluşturun:</span><span class="sxs-lookup"><span data-stu-id="81838-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="81838-118">Daha önce ince ince tasarımı bizim görünüm modeli gözlemlenenler ürünler, Sepeti, siparişler ve Ayrıntılar için gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="81838-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="81838-119">Aşağıdaki değişkenleri eklemek `AppViewModel` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="81838-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="81838-120">Kullanıcıların Sepeti ürünleri listesinden öğe eklemek ve Alışveriş sepetinden öğeleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="81838-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="81838-121">Bu işlevler kapsüllemek için bir ürün temsil eden başka bir görünüm modeli sınıf oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="81838-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="81838-122">Aşağıdaki kodu ekleyin `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="81838-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="81838-123">`ProductViewModel` Sınıfı Sepeti gelen ve giden ürün taşımak için kullanılan iki işlevler içerir: `addItemToCart` sepete ürün bir birimi ekler ve `removeAllFromCart` ürün tüm miktarlarını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="81838-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="81838-124">Kullanıcılar, var olan bir sırayı seçin ve Sipariş ayrıntılarını alır.</span><span class="sxs-lookup"><span data-stu-id="81838-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="81838-125">Biz bu işlevi başka bir görünüm modelini kapsülleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="81838-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="81838-126">`OrderDetailsViewModel` Bir sıra ile başlatıldı ve sunucuya bir AJAX isteği göndererek sipariş ayrıntılarını getirir.</span><span class="sxs-lookup"><span data-stu-id="81838-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="81838-127">Ayrıca, fark `total` özelliği `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="81838-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="81838-128">Bu özellik observable adlı özel bir tür olan bir [observable hesaplanan](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="81838-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="81838-129">Hesaplanan observable adından da anlaşılacağı gibi hesaplanan değeri veri bağlama sağlar&#8212;bu durumda, sırasını toplam maliyeti.</span><span class="sxs-lookup"><span data-stu-id="81838-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="81838-130">Ardından, bu işlevler eklemek `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="81838-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="81838-131">`resetCart` Alışveriş sepetinden tüm öğeleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="81838-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="81838-132">`getDetails` Ayrıntılar için bir sıra alır (yeni bir pusing tarafından `OrderDetailsViewModel` üzerine `details` listesi).</span><span class="sxs-lookup"><span data-stu-id="81838-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="81838-133">`createOrder` Yeni bir sipariş oluşturur ve sepeti boşaltır.</span><span class="sxs-lookup"><span data-stu-id="81838-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="81838-134">Son olarak, ürün ve siparişleri AJAX istekleri yaparak görünüm modeli başlatın:</span><span class="sxs-lookup"><span data-stu-id="81838-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="81838-135">Tamam, çok fazla kod olan, ancak bunu oluşturduğumuz yukarı adım adım, bu nedenle umarız tasarım işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="81838-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="81838-136">Şimdi biz HTML bazı Knockout.js bağlamaları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="81838-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="81838-137">**Ürünler**</span><span class="sxs-lookup"><span data-stu-id="81838-137">**Products**</span></span>

<span data-ttu-id="81838-138">Ürün listesi için olan bağlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="81838-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="81838-139">Bu ürünler dizi tekrarlanan ve fiyat ve adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="81838-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="81838-140">Yalnızca kullanıcı oturum açtığında "Sırada Ekle" düğmesi görünür olur.</span><span class="sxs-lookup"><span data-stu-id="81838-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="81838-141">"Sırada Ekle" düğmesi çağrıları `addItemToCart` üzerinde `ProductViewModel` ürün için örneği.</span><span class="sxs-lookup"><span data-stu-id="81838-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="81838-142">Bu iyi bir özelliği olan Knockout.js gösterir: görünüm modeli diğer görünüm modelleri içerdiğinde, iç model bağlamaları uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="81838-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="81838-143">Bu örnekte, içinde bağlamaları `foreach` her biri için uygulanan `ProductViewModel` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="81838-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="81838-144">Bu yaklaşım tüm işlevleri tek bir görünüm-modeline koyma daha çok temizleyici olur.</span><span class="sxs-lookup"><span data-stu-id="81838-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="81838-145">**Sepeti**</span><span class="sxs-lookup"><span data-stu-id="81838-145">**Cart**</span></span>

<span data-ttu-id="81838-146">Alışveriş için olan bağlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="81838-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="81838-147">Bu Sepeti dizi tekrarlanan ve adı, fiyat ve miktar görüntüler.</span><span class="sxs-lookup"><span data-stu-id="81838-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="81838-148">Bağlantıyı "Kaldır" ve "Sipariş oluştur" düğmesine görünüm modeli işlevlere bağlı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="81838-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="81838-149">**Siparişleri**</span><span class="sxs-lookup"><span data-stu-id="81838-149">**Orders**</span></span>

<span data-ttu-id="81838-150">Siparişleri listesi için olan bağlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="81838-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="81838-151">Bu sipariş tekrarlanan ve sipariş kimliği gösterir</span><span class="sxs-lookup"><span data-stu-id="81838-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="81838-152">Bağlantıyı click olayını bağlı `getDetails` işlevi.</span><span class="sxs-lookup"><span data-stu-id="81838-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="81838-153">**Sipariş ayrıntılarını**</span><span class="sxs-lookup"><span data-stu-id="81838-153">**Order Details**</span></span>

<span data-ttu-id="81838-154">Sipariş ayrıntılarını bağlantılarında şunlardır:</span><span class="sxs-lookup"><span data-stu-id="81838-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="81838-155">Bu sırada öğeler üzerinde yinelenir ve ürün, fiyat ve miktarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="81838-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="81838-156">Çevresindeki div yalnızca ayrıntıları dizi bir veya daha fazla öğe içeriyorsa görünür olur.</span><span class="sxs-lookup"><span data-stu-id="81838-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="81838-157">Sonuç</span><span class="sxs-lookup"><span data-stu-id="81838-157">Conclusion</span></span>

<span data-ttu-id="81838-158">Bu öğreticide, veritabanı ve veri katmanı üzerinde genel kullanıma yönelik arabirimi sağlamak için ASP.NET Web API ile iletişim kurmak için Entity Framework kullanan bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="81838-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="81838-159">HTML sayfaları ve Knockout.js ve jQuery sayfa yeniden yükler olmadan dinamik etkileşimlerini sağlamak üzere işlemek için ASP.NET MVC 4 kullanırız.</span><span class="sxs-lookup"><span data-stu-id="81838-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="81838-160">Ek kaynaklar:</span><span class="sxs-lookup"><span data-stu-id="81838-160">Additional resources:</span></span>

- [<span data-ttu-id="81838-161">ASP.NET Data Access içerik haritası</span><span class="sxs-lookup"><span data-stu-id="81838-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="81838-162">Entity Framework Geliştirici Merkezi</span><span class="sxs-lookup"><span data-stu-id="81838-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="81838-163">Önceki</span><span class="sxs-lookup"><span data-stu-id="81838-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
