---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "5. Kısım: dinamik bir kullanıcı Arabirimi ile Knockout.js oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20ebdb1b8ba710e0fbc6040f7cd4064b44658c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="4113b-102">5. Kısım: dinamik kullanıcı arabirimini Knockout.js ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="4113b-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="4113b-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4113b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4113b-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="4113b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="4113b-105">Knockout.js ile dinamik kullanıcı Arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4113b-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="4113b-106">Bu bölümde, yönetici görünümüne işlevsellik eklemek için Knockout.js kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="4113b-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="4113b-107">[Knockout.js](http://knockoutjs.com/) , HTML denetimlerini verilere bağlama kolaylaştırır bir Javascript kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="4113b-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="4113b-108">Knockout.js Model View ViewModel (MVVM) desen kullanır.</span><span class="sxs-lookup"><span data-stu-id="4113b-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="4113b-109">*Modeli* iş etki alanında (servis talebi, ürünlerimizi ve siparişleri) verileri sunucu tarafı gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="4113b-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="4113b-110">*Görünüm* sunu (HTML) katmanıdır.</span><span class="sxs-lookup"><span data-stu-id="4113b-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="4113b-111">*Görünüm modeli* modeli verilerini tutan bir Javascript nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="4113b-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="4113b-112">Görünüm modeli UI kod soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="4113b-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="4113b-113">HTML temsilini olanağıyla sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4113b-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="4113b-114">Bunun yerine, "bir öğe listesi" gibi görünümün soyut özellikleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4113b-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="4113b-115">Görünüm veri görünüm modeline bağlı.</span><span class="sxs-lookup"><span data-stu-id="4113b-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="4113b-116">Görünüm modeli güncelleştirmeleri otomatik olarak görünümünde yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="4113b-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="4113b-117">Görünüm modeli de görünümünden düğme tıklamaları gibi olayları alır ve bir sıra oluşturma gibi bir modeli işlemleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4113b-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="4113b-118">Öncelikle biz görünüm modeli tanımlamanız.</span><span class="sxs-lookup"><span data-stu-id="4113b-118">First we'll define the view-model.</span></span> <span data-ttu-id="4113b-119">Bundan sonra biz HTML biçimlendirmesi görünüm modeline bağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="4113b-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="4113b-120">Aşağıdaki Razor bölümü için Admin.cshtml ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4113b-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="4113b-121">Bu bölümde dosyasında herhangi bir yere ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4113b-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="4113b-122">Görünüm işlendiğinde bölümü HTML sayfasının en altında görünür kapatmadan önce sağ &lt;/body&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="4113b-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="4113b-123">Bu sayfa için komut tümünün açıklamaya göre belirtilen komut dosyası etiketinin içine geçer:</span><span class="sxs-lookup"><span data-stu-id="4113b-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="4113b-124">İlk olarak, bir görünüm model sınıfı tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="4113b-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="4113b-125">**ko.observableArray** özel türde bir adlı Boşaltılan nesnesinde bir *observable*.</span><span class="sxs-lookup"><span data-stu-id="4113b-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="4113b-126">Gelen [Knockout.js belgelerine](http://knockoutjs.com/documentation/observables.html): observable "aboneleri değişiklikler hakkında bilgilendirebilir bir JavaScript." nesnedir</span><span class="sxs-lookup"><span data-stu-id="4113b-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="4113b-127">Observable içeriğini değiştirdiğinizde, görünümü eşleşecek şekilde otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4113b-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="4113b-128">Doldurmak için `products` dizisindeki, web API AJAX isteği olun.</span><span class="sxs-lookup"><span data-stu-id="4113b-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="4113b-129">Biz API görünüm paketi taban URI depolanan geri çağırma (bkz [bölümü 4](using-web-api-with-entity-framework-part-4.md) öğreticinin).</span><span class="sxs-lookup"><span data-stu-id="4113b-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="4113b-130">Ardından, İşlevler görünümü-oluşturma, güncelleştirme ve silme ürünleri için modeline ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4113b-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="4113b-131">Bu işlevler AJAX çağrıları Web API'ye gönderin ve sonuçları görünüm modeli güncelleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="4113b-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="4113b-132">Şimdi en önemli bölümü: zaman DOM olduğu fulled yüklenen, çağrı **ko.applyBindings** işlev ve yeni bir örneğini içinde geçirmek `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="4113b-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="4113b-133">**Ko.applyBindings** yöntemi Boşaltılan etkinleştirir ve görünümüne bağlayan görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="4113b-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="4113b-134">Görünüm modeli sahibiz, bağlamaları oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="4113b-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="4113b-135">Knockout.js içinde ekleyerek bunu `data-bind` öznitelikleri HTML öğeleri.</span><span class="sxs-lookup"><span data-stu-id="4113b-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="4113b-136">Örneğin, bir dizi için bir HTML liste bağlamak için kullanın `foreach` bağlama:</span><span class="sxs-lookup"><span data-stu-id="4113b-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="4113b-137">`foreach` Bağlama dizisini yineler ve alt dizideki her nesne için öğeleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4113b-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="4113b-138">Alt öğeler üzerinde bağlamaları dizi nesnelerdeki özellikleri başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4113b-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="4113b-139">Aşağıdaki bağlamaları "Güncelleştirme ürünleri" listesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4113b-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="4113b-140">`<li>` Öğesi oluşur kapsamında **foreach** bağlama.</span><span class="sxs-lookup"><span data-stu-id="4113b-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="4113b-141">Anlamına gelir Boşaltılan her ürün için bir kez öğe işleme `products` dizi.</span><span class="sxs-lookup"><span data-stu-id="4113b-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="4113b-142">Tüm bağlamaları içinde `<li>` öğesi bu ürün örneğine bakın.</span><span class="sxs-lookup"><span data-stu-id="4113b-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="4113b-143">Örneğin, `$data.Name` başvurduğu `Name` ürün özelliği.</span><span class="sxs-lookup"><span data-stu-id="4113b-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="4113b-144">Metin girdi değerleri ayarlamak için kullanma `value` bağlama.</span><span class="sxs-lookup"><span data-stu-id="4113b-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="4113b-145">Düğmeleri model-view üzerinde işlevlere bağlı kullanarak `click` bağlama.</span><span class="sxs-lookup"><span data-stu-id="4113b-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="4113b-146">Ürün örneği her işlevi için parametre olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="4113b-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="4113b-147">Daha fazla bilgi için [Knockout.js belgelerine](http://knockoutjs.com/documentation/observables.html) çeşitli bağlamaları iyi açıklamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="4113b-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="4113b-148">Ardından, için bir bağlama eklemek **gönderme** Ürün Ekle form üzerinde olay:</span><span class="sxs-lookup"><span data-stu-id="4113b-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="4113b-149">Bu bağlama çağrıları `create` yeni ürün oluşturmak için Görünüm modeli üzerinde işlevi.</span><span class="sxs-lookup"><span data-stu-id="4113b-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="4113b-150">Yönetim görünümü için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="4113b-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="4113b-151">Uygulamayı çalıştırın, yönetici hesabıyla oturum açın ve "Yönetici" bağlantısını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4113b-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="4113b-152">Ürünlerinin listesini görmek ve oluşturma, güncelleştirme veya silme ürünleri kurabilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4113b-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4113b-153">[Önceki](using-web-api-with-entity-framework-part-4.md)
[sonraki](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="4113b-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
