---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görünen veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin Web ASP.NET 4.7 ve Microsoft Visual Studio Community 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri sağlanır.
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207440"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="653ce-103">Görünen veri öğelerini ve ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="653ce-103">Display data items and details</span></span>
====================
<span data-ttu-id="653ce-104">tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="653ce-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="653ce-105">Bu öğretici serisinde, Web için ASP.NET 4.7 ve Microsoft Visual Studio Community 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="653ce-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="653ce-106">Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First ile veri öğesi ayrıntılarını görüntülemek nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="653ce-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="653ce-107">Bu öğreticide, Wingtip çocuğunun Store öğretici serisinin bir parçası olarak önceki "Kullanıcı Arabirimi ve gezinti" öğreticisini üzerine inşa edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="653ce-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="653ce-108">Tamamlanmış bir öğreticide, ürünlerin *ProductsList.aspx* sayfa ve bir ürün uygulamasının Ayrıntılar *ProductDetails.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="653ce-109">Öğrenecekleriniz</span><span class="sxs-lookup"><span data-stu-id="653ce-109">What you learn</span></span>

- <span data-ttu-id="653ce-110">Veritabanı ürünleri görüntülemek için bir veri denetim ekleyin.</span><span class="sxs-lookup"><span data-stu-id="653ce-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="653ce-111">Veri denetimi, seçilen verilere bağlanın.</span><span class="sxs-lookup"><span data-stu-id="653ce-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="653ce-112">Ürün ayrıntılarını görüntülemek için bir veri denetim ekleyin.</span><span class="sxs-lookup"><span data-stu-id="653ce-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="653ce-113">Bir sorgu dizesi değerini ayrıştırabilir ve alınan veritabanı verilerini filtrelemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="653ce-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="653ce-114">Bu öğreticide sunulan özellikler, model bağlama ve değer sağlayıcılarını içerir.</span><span class="sxs-lookup"><span data-stu-id="653ce-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="653ce-115">Ürünleri görüntülemek için bir veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="653ce-115">Add a data control to display products</span></span>
 
<span data-ttu-id="653ce-116">Bir sunucu denetimine veri bağlama için birkaç seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="653ce-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="653ce-117">En yaygın şunlardır:</span><span class="sxs-lookup"><span data-stu-id="653ce-117">The most common include:</span></span>

 * <span data-ttu-id="653ce-118">Veri kaynak denetimi ekleme</span><span class="sxs-lookup"><span data-stu-id="653ce-118">Adding a data source control</span></span>
 * <span data-ttu-id="653ce-119">Kodu el ile ekleme</span><span class="sxs-lookup"><span data-stu-id="653ce-119">Adding code by hand</span></span>
 * <span data-ttu-id="653ce-120">Model bağlama uygulama</span><span class="sxs-lookup"><span data-stu-id="653ce-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="653ce-121">Veri bağlama için bir veri kaynağı denetimi kullanma</span><span class="sxs-lookup"><span data-stu-id="653ce-121">Use a data source control to bind data</span></span>

<span data-ttu-id="653ce-122">Veri kaynak denetimi ekleme veri kaynak denetimi veri görüntüleyen denetimine bağlar.</span><span class="sxs-lookup"><span data-stu-id="653ce-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="653ce-123">Bu yaklaşım, bildirimli olarak yapabilirsiniz yerine programlı olarak sunucu tarafı denetimlerdir veri kaynaklarına bağlanın.</span><span class="sxs-lookup"><span data-stu-id="653ce-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="653ce-124">Kodu el ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="653ce-124">Code by hand to bind data</span></span>

<span data-ttu-id="653ce-125">El ile kodlama içerir:</span><span class="sxs-lookup"><span data-stu-id="653ce-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="653ce-126">Bir değer okuma</span><span class="sxs-lookup"><span data-stu-id="653ce-126">Reading a value</span></span>
2. <span data-ttu-id="653ce-127">Null olup olmadığı denetleniyor</span><span class="sxs-lookup"><span data-stu-id="653ce-127">Checking if it's null</span></span>
3. <span data-ttu-id="653ce-128">Uygun bir türe dönüştürme</span><span class="sxs-lookup"><span data-stu-id="653ce-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="653ce-129">Dönüştürme başarılı denetleniyor</span><span class="sxs-lookup"><span data-stu-id="653ce-129">Checking conversion success</span></span>
5. <span data-ttu-id="653ce-130">Bir sorgu dönüştürülen değer ile yapma</span><span class="sxs-lookup"><span data-stu-id="653ce-130">Making a query with the converted value</span></span> 

<span data-ttu-id="653ce-131">Bu yaklaşımda, veri erişim mantığı üzerinde tam denetime sahip.</span><span class="sxs-lookup"><span data-stu-id="653ce-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="653ce-132">Model bağlama veri bağlamak için kullanın</span><span class="sxs-lookup"><span data-stu-id="653ce-132">Use model binding to bind data</span></span>

<span data-ttu-id="653ce-133">Model bağlama ile sonuçları çok daha az kod ile bağlama ve uygulamanızda işlevini yeniden kullanma olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="653ce-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="653ce-134">Kod odaklı veri erişim mantığı ile çalışma, yine de bir zengin, veri bağlama çerçevesi sağlarken basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="653ce-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="653ce-135">Ürünleri görüntülemek</span><span class="sxs-lookup"><span data-stu-id="653ce-135">Display products</span></span>

<span data-ttu-id="653ce-136">Bu öğreticide, veri bağlama için model bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="653ce-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="653ce-137">Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. `SelectMethod` özelliği bir yönteme sayfanın kod.</span><span class="sxs-lookup"><span data-stu-id="653ce-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="653ce-138">Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar.</span><span class="sxs-lookup"><span data-stu-id="653ce-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="653ce-139">Açıkça çağırmaya gerek yoktur `DataBind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="653ce-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="653ce-140">Aşağıdaki adımları izleyerek çalışmaya, değişiklik *ProductList.aspx* ürünleri görüntülemek üzere biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="653ce-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="653ce-141">İçinde **Çözüm Gezgini**açın *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="653ce-142">Mevcut biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="653ce-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="653ce-143">Önceki biçimlendirme kullanan bir **ListView** adlı Denetim `productList` ürünleri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="653ce-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="653ce-144">Şablonları ve stilleri ile tanımladığınız nasıl **ListView** denetim verilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="653ce-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="653ce-145">Tüm yinelenen yapısındaki veriler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="653ce-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="653ce-146">Ancak bu **ListView** örnek yalnızca veritabanı verileri görüntüler, ayrıca, kodu olmadan etkinleştir kullanıcıların düzenlemek, eklemek ve verileri silmek için ve sıralama ve veri sayfasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="653ce-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="653ce-147">Ayarladığınızda `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` kullanılabilir ve Denetim türü kesin belirlenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="653ce-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="653ce-148">Önceki öğreticide belirtildiği gibi belirtme gibi IntelliSense ile öğesi nesne ayrıntıları seçebilirsiniz `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="653ce-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Veri öğelerini ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="653ce-150">Model bağlamayla belirtme bir `SelectMethod` değeri (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="653ce-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="653ce-151">Bu koda Ekle yöntemi, arkasından sonraki adımda ürünleri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="653ce-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="653ce-152">Ürünleri görüntülemek için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="653ce-152">Add code to display products</span></span>

<span data-ttu-id="653ce-153">Bu adımda, doldurmak için kod ekleme **ListView** denetimi ile veritabanı ürün verileri.</span><span class="sxs-lookup"><span data-stu-id="653ce-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="653ce-154">Tüm ürünler ve tek bir kategori ürünler gösteren kod destekler.</span><span class="sxs-lookup"><span data-stu-id="653ce-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="653ce-155">İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* seçip **kodu görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="653ce-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="653ce-156">Varolan kodda değiştirin *ProductList.aspx.cs* bu dosya:</span><span class="sxs-lookup"><span data-stu-id="653ce-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="653ce-157">Bu kod gösterir `GetProducts` yöntemi, **ListView** denetimin `ItemType` özelliği başvurularının *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="653ce-158">Sonuçları belirli veritabanı kategorisi sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerinden *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="653ce-159">`QueryStringAttribute` Sınıfını `System.Web.ModelBinding` sorgu dizesi değişkeni almak için kullanılan ad alanı `id`değerini.</span><span class="sxs-lookup"><span data-stu-id="653ce-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="653ce-160">Bu model bağlama için bir sorgu dizesi değeri çalışma zamanında bağlamak için bildirir `categoryId` parametresi.</span><span class="sxs-lookup"><span data-stu-id="653ce-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="653ce-161">Geçerli bir kategori olduğunda (`categoryId`) olan sonuçlarını geçti, o kategorinin veritabanı ürünler için sınırlı.</span><span class="sxs-lookup"><span data-stu-id="653ce-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="653ce-162">Örneğin, *ProductsList.aspx* sayfası URL'si şudur:</span><span class="sxs-lookup"><span data-stu-id="653ce-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="653ce-163">Sayfa yalnızca ürünleri görüntüler. burada `categoryId` eşittir `1`.</span><span class="sxs-lookup"><span data-stu-id="653ce-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="653ce-164">Hiçbir sorgu dizesi geçirilen tüm ürünleri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="653ce-165">Bu yöntemlere ait değer kaynaklarının adlı *değer sağlayıcıları* (gibi `QueryString`), ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri adlı *değer sağlayıcısı öznitelikleri* () gibi `id`).</span><span class="sxs-lookup"><span data-stu-id="653ce-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="653ce-166">ASP.NET değer sağlayıcıları ve tüm tipik Web Forms uygulaması kullanıcı giriş kaynakları özniteliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="653ce-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="653ce-167">Bunlar, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özelliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="653ce-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="653ce-168">Özel değer sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="653ce-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="653ce-169">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="653ce-169">Run the application</span></span>

<span data-ttu-id="653ce-170">Tüm ürünler ya da bir kategorinin ürünler görüntülemek için uygulamayı şimdi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="653ce-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="653ce-171">Visual Studio'da **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="653ce-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="653ce-172">Tarayıcı açılır ve gösterir *Default.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="653ce-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="653ce-173">Ürün kategorisi menüden **otomobiller**.</span><span class="sxs-lookup"><span data-stu-id="653ce-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="653ce-174">*ProductList.aspx* sayfası görüntülenirse, yalnızca ürünleri gösteren **otomobiller** kategorisi.</span><span class="sxs-lookup"><span data-stu-id="653ce-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="653ce-175">Bu öğreticide daha sonra ürün ayrıntıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="653ce-175">Later in this tutorial, you display product details.</span></span>

    ![Veri öğelerini ve ayrıntıları - otomobiller](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="653ce-177">Seçin **ürünleri** üstteki menüden.</span><span class="sxs-lookup"><span data-stu-id="653ce-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="653ce-178">*ProductList.aspx* sayfası artık tüm ürünleri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="653ce-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="653ce-180">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="653ce-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="653ce-181">Ürün ayrıntılarını görüntülemek için bir veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="653ce-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="653ce-182">Değiştirme *ProductDetails.aspx* biçimlendirme belirli ürün bilgilerini görüntülemek için önceki öğreticide eklendi:</span><span class="sxs-lookup"><span data-stu-id="653ce-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="653ce-183">İçinde **Çözüm Gezgini**açın *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="653ce-184">Mevcut biçimlendirme, bu işaretleme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="653ce-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="653ce-185">Bu işaretleme kullanan bir **FormView** belirli bir ürün ayrıntılarını görüntülemek için denetim.</span><span class="sxs-lookup"><span data-stu-id="653ce-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="653ce-186">Verileri görüntülemek için kullanılanlar gibi yöntemleri kullanan *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="653ce-187">**FormView** denetimi, tek bir kaydı aynı anda bir veri kaynağından görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="653ce-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="653ce-188">Kullanırken **FormView** denetimi görüntülemek ve veri bağlama değerlerini düzenlemek için şablonlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="653ce-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="653ce-189">Bu şablonlar ifadeleri, bağlama denetimleri içerir ve biçimlendirme formun görünümü ve yeni işlevleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="653ce-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="653ce-190">Önceki biçimlendirme veritabanına bağlanırken, ek kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="653ce-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="653ce-191">İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* seçip **kodu görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="653ce-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="653ce-192">*ProductDetails.aspx.cs* dosyası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="653ce-193">Mevcut kodu şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="653ce-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="653ce-194">Bu kod denetleyen bir "`productID`" sorgu dizesi değeri.</span><span class="sxs-lookup"><span data-stu-id="653ce-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="653ce-195">Geçerli bir değer bulunursa eşleşen ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="653ce-196">Sorgu dizesi bulunamadığında veya değeri geçerli değil, hiçbir ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="653ce-197">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="653ce-197">Run the application</span></span>

<span data-ttu-id="653ce-198">Ürün kimliği temel alınarak belirli ürün ayrıntıları görmek için uygulamayı Şimdi Çalıştır</span><span class="sxs-lookup"><span data-stu-id="653ce-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="653ce-199">Visual Studio'da **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="653ce-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="653ce-200">Tarayıcı açılır *Default.aspx*.</span><span class="sxs-lookup"><span data-stu-id="653ce-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="653ce-201">Kategori menüden **Boats**.</span><span class="sxs-lookup"><span data-stu-id="653ce-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="653ce-202">*ProductList.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="653ce-203">Seçin **kağıt bot**.</span><span class="sxs-lookup"><span data-stu-id="653ce-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="653ce-204">*ProductDetails.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="653ce-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="653ce-206">Sonraki öğreticide, Wingtip Toys uygulamaya alışveriş sepetine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="653ce-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="653ce-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="653ce-207">Additional resources</span></span>

[<span data-ttu-id="653ce-208">Model bağlama ve web forms ile verileri alma ve görüntüleme</span><span class="sxs-lookup"><span data-stu-id="653ce-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="653ce-209">[Önceki](ui_and_navigation.md)
> [İleri](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="653ce-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
