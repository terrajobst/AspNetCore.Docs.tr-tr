---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: '7. Bölüm: ana oluşturma sayfası | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: c5b6cb0f2e48cdea3d6cc5cde72d08a99126f05f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825641"
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="eafcf-102">7. Bölüm: ana oluşturma sayfası</span><span class="sxs-lookup"><span data-stu-id="eafcf-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="eafcf-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eafcf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="eafcf-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="eafcf-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="eafcf-105">Ana oluşturma sayfası</span><span class="sxs-lookup"><span data-stu-id="eafcf-105">Creating the Main Page</span></span>

<span data-ttu-id="eafcf-106">Bu bölümde, uygulama ana sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="eafcf-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="eafcf-107">Bu sayfa, biz bunu birkaç adımda yaklaşımını şekilde yönetici sayfadan daha karmaşık olacaktır.</span><span class="sxs-lookup"><span data-stu-id="eafcf-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="eafcf-108">Bu doğrultuda, bazı daha gelişmiş Knockout.js teknikleri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="eafcf-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="eafcf-109">Temel düzen sayfasının şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="eafcf-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="eafcf-110">"Ürün", bir dizi ürün tutar.</span><span class="sxs-lookup"><span data-stu-id="eafcf-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="eafcf-111">"Sepeti" miktarlar ürünleriyle dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="eafcf-112">"Sepete Ekle"'ı tıklatarak, sepet güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="eafcf-113">"Siparişler", bir sipariş kimlikleri dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="eafcf-114">"Details" (miktarlar ürünleriyle) öğelerin bir dizisi olan bir sipariş ayrıntısı tutar.</span><span class="sxs-lookup"><span data-stu-id="eafcf-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="eafcf-115">Veri bağlama ya da betik HTML, bazı temel düzeni tanımlayarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="eafcf-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="eafcf-116">Views/Home/Index.cshtml dosyasını açın ve tüm içeriğini aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="eafcf-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="eafcf-117">Ardından, betikleri bölüm ekleme ve boş bir görünüm modeli oluşturun:</span><span class="sxs-lookup"><span data-stu-id="eafcf-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="eafcf-118">Daha önce ince ince tasarımı görünümü modelimizi gözlemlenenler ürünleri, sepet, siparişler ve Ayrıntılar için gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="eafcf-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="eafcf-119">Aşağıdaki değişkenleri ekleyip `AppViewModel` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="eafcf-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="eafcf-120">Kullanıcılar, öğeleri ürünleri listeden Sepete Ekle ve sepetinden öğeleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="eafcf-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="eafcf-121">Bu işlevler yalıtılacak bir ürünü temsil eden başka bir görünüm modeli sınıf oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="eafcf-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="eafcf-122">Aşağıdaki kodu ekleyin `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="eafcf-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="eafcf-123">`ProductViewModel` Sınıfı sepet gelen ve ürün taşımak için kullanılan iki işlevleri içerir: `addItemToCart` , sepete ürün bir birimi ekler ve `removeAllFromCart` ürün tüm miktarlarını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="eafcf-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="eafcf-124">Kullanıcılar, var olan bir sırayı seçin ve Sipariş ayrıntılarını alın.</span><span class="sxs-lookup"><span data-stu-id="eafcf-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="eafcf-125">Biz, başka bir görünüm modeli bu işlevselliği kapsülleyen:</span><span class="sxs-lookup"><span data-stu-id="eafcf-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="eafcf-126">`OrderDetailsViewModel` Bir sıra ile başlatılır ve sunucuya bir AJAX isteği göndererek sipariş ayrıntılarını getirir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="eafcf-127">Ayrıca, fark `total` özellikte `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="eafcf-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="eafcf-128">Bu özellik, gözlemlenebilir adlı özel bir tür bir [observable hesaplanan](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="eafcf-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="eafcf-129">Hesaplanan observable adından da anlaşılacağı gibi hesaplanan değeri verilerin bağlanacağı sağlar&#8212;toplam sırası bu durumda maliyet.</span><span class="sxs-lookup"><span data-stu-id="eafcf-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="eafcf-130">Ardından, bu işlevler için ekleme `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="eafcf-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="eafcf-131">`resetCart` tüm öğeleri sepetinden kaldırır.</span><span class="sxs-lookup"><span data-stu-id="eafcf-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="eafcf-132">`getDetails` için bir sipariş ayrıntıları alır (yeni bir pusing tarafından `OrderDetailsViewModel` üzerine `details` listesi).</span><span class="sxs-lookup"><span data-stu-id="eafcf-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="eafcf-133">`createOrder` Yeni bir sıra oluşturur ve sepet boşaltır.</span><span class="sxs-lookup"><span data-stu-id="eafcf-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="eafcf-134">Son olarak, ürünler ve siparişler için AJAX isteği yaparak görünüm modeli başlatın:</span><span class="sxs-lookup"><span data-stu-id="eafcf-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="eafcf-135">Tamam, bir sürü kod olan, ancak biz yerleşik yedekleme adım adım şekilde Umarım tasarım işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="eafcf-136">Şimdi biz HTML bazı Knockout.js bağlamaları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eafcf-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="eafcf-137">**Ürünleri**</span><span class="sxs-lookup"><span data-stu-id="eafcf-137">**Products**</span></span>

<span data-ttu-id="eafcf-138">Ürün listesi için olan bağlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eafcf-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="eafcf-139">Bu ürünler dizi içinde yinelenir ve fiyat ve adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="eafcf-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="eafcf-140">"Order Ekle" düğmesi, yalnızca kullanıcı oturum açtığında görünür olur.</span><span class="sxs-lookup"><span data-stu-id="eafcf-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="eafcf-141">"Order Ekle" düğmesi çağrıları `addItemToCart` üzerinde `ProductViewModel` ürün için örneği.</span><span class="sxs-lookup"><span data-stu-id="eafcf-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="eafcf-142">Bu güzel bir özelliği Knockout.js gösterir: görünüm modeli diğer görünüm modelleri içeriyorsa, iç modele bağlamalarını uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eafcf-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="eafcf-143">Bu örnekte, içinde bağlamaları `foreach` her bir uygulanan `ProductViewModel` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="eafcf-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="eafcf-144">Bu yaklaşım, tek bir görünüm-modeline tüm işlevlerin koyarak daha çok daha net olur.</span><span class="sxs-lookup"><span data-stu-id="eafcf-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="eafcf-145">**Cart**</span><span class="sxs-lookup"><span data-stu-id="eafcf-145">**Cart**</span></span>

<span data-ttu-id="eafcf-146">Sepet bağlamalarda şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eafcf-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="eafcf-147">Bu, sepet dizi içinde yinelenir ve ad, fiyat ve miktar görüntüler.</span><span class="sxs-lookup"><span data-stu-id="eafcf-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="eafcf-148">İşlevler görünümü-model bağlantıyı "Kaldır" ve "Sipariş oluştur" düğmesine bağlı olduklarını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eafcf-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="eafcf-149">**Siparişler**</span><span class="sxs-lookup"><span data-stu-id="eafcf-149">**Orders**</span></span>

<span data-ttu-id="eafcf-150">Siparişler listesi için olan bağlamaları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eafcf-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="eafcf-151">Bu siparişleri yinelenir ve sipariş kimliği gösterir</span><span class="sxs-lookup"><span data-stu-id="eafcf-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="eafcf-152">Tıklama olayı bağlantısına bağlı `getDetails` işlevi.</span><span class="sxs-lookup"><span data-stu-id="eafcf-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="eafcf-153">**Sipariş Ayrıntıları**</span><span class="sxs-lookup"><span data-stu-id="eafcf-153">**Order Details**</span></span>

<span data-ttu-id="eafcf-154">Sipariş ayrıntılarını bağlamalarda şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eafcf-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="eafcf-155">Bu sırada öğeler üzerinden yinelenir ve ürün, fiyatı ile miktarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="eafcf-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="eafcf-156">Yalnızca ayrıntıları dizi bir veya daha fazla öğe içeriyorsa, çevreleyen div görülebilir.</span><span class="sxs-lookup"><span data-stu-id="eafcf-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="eafcf-157">Sonuç</span><span class="sxs-lookup"><span data-stu-id="eafcf-157">Conclusion</span></span>

<span data-ttu-id="eafcf-158">Bu öğreticide, veritabanı ve veri katmanı üzerinde bir genel kullanıma yönelik arabirim sağlamak üzere ASP.NET Web API ile iletişim kurmak için Entity Framework kullanan bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="eafcf-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="eafcf-159">ASP.NET MVC 4 Sayfa yeniden yükler olmadan dinamik etkileşim sağlamak için HTML sayfalarında ve Knockout.js artı jQuery işlemek için kullanırız.</span><span class="sxs-lookup"><span data-stu-id="eafcf-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="eafcf-160">Ek kaynaklar:</span><span class="sxs-lookup"><span data-stu-id="eafcf-160">Additional resources:</span></span>

- [<span data-ttu-id="eafcf-161">ASP.NET Data Access içerik haritası</span><span class="sxs-lookup"><span data-stu-id="eafcf-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="eafcf-162">Entity Framework Geliştirici Merkezi</span><span class="sxs-lookup"><span data-stu-id="eafcf-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="eafcf-163">Önceki</span><span class="sxs-lookup"><span data-stu-id="eafcf-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
