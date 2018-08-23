---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: '5. Bölüm: Knockout.js ile dinamik kullanıcı Arabirimi oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752342"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="11b5d-102">5. Bölüm: Knockout.js ile dinamik kullanıcı Arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="11b5d-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="11b5d-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11b5d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="11b5d-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="11b5d-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="11b5d-105">Knockout.js ile dinamik kullanıcı Arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="11b5d-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="11b5d-106">Bu bölümde, yönetici görünümü işlevselliği eklemek için Knockout.js kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="11b5d-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="11b5d-107">[Knockout.js](http://knockoutjs.com/) HTML denetimleri verilere bağlamak için kolaylaştıran bir Javascript kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="11b5d-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="11b5d-108">Knockout.js Model-View-ViewModel (MVVM) desenini kullanır.</span><span class="sxs-lookup"><span data-stu-id="11b5d-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="11b5d-109">*Modeli* iş etki alanında (çalışması, Ürünlerimiz ve siparişler) verileri sunucu tarafı gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="11b5d-110">*Görünümü* sunu katmanı (HTML).</span><span class="sxs-lookup"><span data-stu-id="11b5d-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="11b5d-111">*Görünüm modeli* model verileri tutan bir Javascript nesnesi.</span><span class="sxs-lookup"><span data-stu-id="11b5d-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="11b5d-112">Görünüm modeli, kullanıcı arabiriminin bir kod soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="11b5d-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="11b5d-113">HTML gösteriminin bilgisi var.</span><span class="sxs-lookup"><span data-stu-id="11b5d-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="11b5d-114">Bunun yerine, "öğe listesi" gibi görünümün soyut özellikler temsil eder.</span><span class="sxs-lookup"><span data-stu-id="11b5d-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="11b5d-115">Görünüm veri görünüm modeline bağlı.</span><span class="sxs-lookup"><span data-stu-id="11b5d-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="11b5d-116">Görünüm modeli güncelleştirmeler Görünümü'nde otomatik olarak yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="11b5d-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="11b5d-117">Görünüm modeli görünümünden bir düğmeye tıklanması gibi olayları alır ve bir sipariş oluşturma gibi model üzerinde işlemleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="11b5d-118">İlk Görünüm modeli tanımlarız.</span><span class="sxs-lookup"><span data-stu-id="11b5d-118">First we'll define the view-model.</span></span> <span data-ttu-id="11b5d-119">Bundan sonra biz HTML biçimlendirmesi için Görünüm modeli bağlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="11b5d-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="11b5d-120">Aşağıdaki Razor bölümü için Admin.cshtml ekleyin:</span><span class="sxs-lookup"><span data-stu-id="11b5d-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="11b5d-121">Bu bölümde dosyasında herhangi bir yere ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11b5d-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="11b5d-122">Görünüm işlendiğinde bölümü HTML sayfasının en altında görünür kapatmadan önce sağ &lt;/body&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="11b5d-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="11b5d-123">Tüm bu sayfa için betik açıklama tarafından belirtilen komut dosyası etiketi içinde geçer:</span><span class="sxs-lookup"><span data-stu-id="11b5d-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="11b5d-124">İlk olarak, bir görünüm modeli sınıf tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="11b5d-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="11b5d-125">**ko.observableArray** denir Knockout, nesnenin özel bir tür bir *observable*.</span><span class="sxs-lookup"><span data-stu-id="11b5d-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="11b5d-126">Gelen [Knockout.js belgeleri](http://knockoutjs.com/documentation/observables.html): observable "aboneleri değişiklikleri bildiren bir JavaScript nesne" dir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="11b5d-127">Observable içeriğini değiştiğinde görünüm eşleşecek şekilde otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="11b5d-128">Doldurmak için `products` dizi, web API'sine bir AJAX isteği yapın.</span><span class="sxs-lookup"><span data-stu-id="11b5d-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="11b5d-129">Biz API görünüm paketi temel URI'sini depolanan geri çağırma (bkz [bölüm 4](using-web-api-with-entity-framework-part-4.md) öğreticinin).</span><span class="sxs-lookup"><span data-stu-id="11b5d-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="11b5d-130">Ardından, İşlevler görünümü-oluşturma, güncelleştirme ve silme ürünleri için eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="11b5d-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="11b5d-131">Bu işlevler, Web API AJAX çağrıları göndermek ve sonuçları görünüm modeli güncelleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="11b5d-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="11b5d-132">Şimdi en önemli kısmıdır: zaman DOM olan fulled yüklenen, çağrı **ko.applyBindings** işlev ve yeni bir örneğini geçirin `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="11b5d-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="11b5d-133">**Ko.applyBindings** yöntemi Knockout etkinleştirir ve görünüme bağlayan görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="11b5d-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="11b5d-134">Görünüm modeli sahibiz, bağlamaları oluşturabiliriz.</span><span class="sxs-lookup"><span data-stu-id="11b5d-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="11b5d-135">Knockout.js içinde ekleyerek bunu `data-bind` HTML öğeleri için öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="11b5d-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="11b5d-136">Örneğin, bir dizi için bir HTML liste bağlamak için kullanın `foreach` bağlama:</span><span class="sxs-lookup"><span data-stu-id="11b5d-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="11b5d-137">`foreach` Bağlama dizi aracılığıyla yinelenir ve alt öğeleri her nesne için dizide oluşturur.</span><span class="sxs-lookup"><span data-stu-id="11b5d-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="11b5d-138">Alt öğeleri üzerinde bağlamaları dizi nesnelerdeki özelliklerin başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="11b5d-139">Aşağıdaki bağlamaları "update-ürünleri" listesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="11b5d-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="11b5d-140">`<li>` Öğesi oluşur kapsamında **foreach** bağlama.</span><span class="sxs-lookup"><span data-stu-id="11b5d-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="11b5d-141">Knockout anlamına gelir her ürün için bir kez öğe işleme `products` dizisi.</span><span class="sxs-lookup"><span data-stu-id="11b5d-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="11b5d-142">Tüm bağlamaları içinde `<li>` öğesi, bu ürün örneğe bakın.</span><span class="sxs-lookup"><span data-stu-id="11b5d-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="11b5d-143">Örneğin, `$data.Name` başvurduğu `Name` ürün özelliği.</span><span class="sxs-lookup"><span data-stu-id="11b5d-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="11b5d-144">Metin girişleri değerlerini ayarlamak için kullanın `value` bağlama.</span><span class="sxs-lookup"><span data-stu-id="11b5d-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="11b5d-145">Model-görünüm üzerinde düğmeleri işlevlere bağlı kullanarak `click` bağlama.</span><span class="sxs-lookup"><span data-stu-id="11b5d-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="11b5d-146">Ürün örneği her işlev için parametre olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="11b5d-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="11b5d-147">Daha fazla bilgi için [Knockout.js belgeleri](http://knockoutjs.com/documentation/observables.html) çeşitli bağlamaları iyi açıklamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="11b5d-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="11b5d-148">Ardından, bağlama için ekleme **gönderme** Ürün Ekle formdaki olay:</span><span class="sxs-lookup"><span data-stu-id="11b5d-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="11b5d-149">Bu bağlama çağrıları `create` işlevi yeni ürün oluşturmak için Görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="11b5d-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="11b5d-150">Yönetici görünümü için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="11b5d-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="11b5d-151">Uygulamayı çalıştırın, yönetici hesabıyla oturum açın ve "Yönetici" bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="11b5d-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="11b5d-152">Ürünlerinin listesini görmek ve oluşturmak, güncelleştirmek veya ürünleri silmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="11b5d-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11b5d-153">[Önceki](using-web-api-with-entity-framework-part-4.md)
> [İleri](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="11b5d-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
