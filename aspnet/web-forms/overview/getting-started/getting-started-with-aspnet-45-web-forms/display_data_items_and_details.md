---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görünen veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri gösterir.
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396231"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="1d12b-103">Görünen veri öğelerini ve ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="1d12b-103">Display data items and details</span></span>
====================
<span data-ttu-id="1d12b-104">tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="1d12b-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="1d12b-105">Bu öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="1d12b-106">Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First ile veri öğesi ayrıntılarını görüntülemeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="1d12b-107">Bu öğreticide, Wingtip çocuğunun Store öğretici serisinin bir parçası olarak önceki "Kullanıcı Arabirimi ve gezinti" öğreticisini üzerine inşa edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="1d12b-108">Bu öğreticiyi tamamladıktan sonra üzerinde ürünleri görürsünüz *ProductsList.aspx* sayfa ve bir ürün uygulamasının Ayrıntılar *ProductDetails.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="1d12b-109">Öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="1d12b-109">You'll learn how to:</span></span>

- <span data-ttu-id="1d12b-110">Veritabanından ürünleri görüntülemek için bir veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="1d12b-111">Bir veri denetimi seçilen verileri bağlayın</span><span class="sxs-lookup"><span data-stu-id="1d12b-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="1d12b-112">Veritabanından ürün ayrıntıları görüntülemek için bir veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="1d12b-113">Sorgu dizesinden bir değer almak ve veritabanından alınan verileri sınırlamak için bu değeri kullanın</span><span class="sxs-lookup"><span data-stu-id="1d12b-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="1d12b-114">Bu öğreticide sunulan özellikleri:</span><span class="sxs-lookup"><span data-stu-id="1d12b-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="1d12b-115">Model bağlama</span><span class="sxs-lookup"><span data-stu-id="1d12b-115">Model binding</span></span>
- <span data-ttu-id="1d12b-116">Değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="1d12b-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="1d12b-117">Veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-117">Add a data control</span></span>

<span data-ttu-id="1d12b-118">Sunucu denetimine veri bağlama için birkaç farklı Seçenekleri'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="1d12b-119">En yaygın şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1d12b-119">The most common include:</span></span>

 * <span data-ttu-id="1d12b-120">Veri kaynak denetimi ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-120">Adding a data source control</span></span>
 * <span data-ttu-id="1d12b-121">Kodu el ile ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-121">Adding code by hand</span></span>
 * <span data-ttu-id="1d12b-122">Model bağlama işlemini kullanma</span><span class="sxs-lookup"><span data-stu-id="1d12b-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="1d12b-123">Veri bağlama için bir veri kaynağı denetimi kullanma</span><span class="sxs-lookup"><span data-stu-id="1d12b-123">Use a data source control to bind data</span></span>

<span data-ttu-id="1d12b-124">Veri kaynağı denetim ekleme, verileri görüntüleyen denetime veri kaynak denetimine bağlamak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d12b-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="1d12b-125">Bu yaklaşım, bildirimli olarak yapabilirsiniz yerine programlı olarak sunucu tarafı denetimlerdir veri kaynaklarına bağlanın.</span><span class="sxs-lookup"><span data-stu-id="1d12b-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="1d12b-126">Kodu el ile veri bağlama</span><span class="sxs-lookup"><span data-stu-id="1d12b-126">Code by hand to bind data</span></span>

<span data-ttu-id="1d12b-127">El ile kodlama içerir:</span><span class="sxs-lookup"><span data-stu-id="1d12b-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="1d12b-128">Bir değer okuma</span><span class="sxs-lookup"><span data-stu-id="1d12b-128">Reading a value</span></span>
2. <span data-ttu-id="1d12b-129">Null olup olmadığı denetleniyor</span><span class="sxs-lookup"><span data-stu-id="1d12b-129">Checking if it's null</span></span>
3. <span data-ttu-id="1d12b-130">Uygun bir türe dönüştürme</span><span class="sxs-lookup"><span data-stu-id="1d12b-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="1d12b-131">Dönüştürme başarılı denetleniyor</span><span class="sxs-lookup"><span data-stu-id="1d12b-131">Checking conversion success</span></span>
5. <span data-ttu-id="1d12b-132">Sorgu değeri kullanma</span><span class="sxs-lookup"><span data-stu-id="1d12b-132">Using the value in the query</span></span> 

<span data-ttu-id="1d12b-133">Bu yaklaşım, veri erişim mantığı üzerinde tam denetime sahip olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d12b-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="1d12b-134">Model bağlama veri bağlamak için kullanın</span><span class="sxs-lookup"><span data-stu-id="1d12b-134">Use model binding to bind data</span></span>

<span data-ttu-id="1d12b-135">Model bağlama, sonuçları daha az kod ile bağlamanıza imkan sağlar ve uygulamanızda işlevini yeniden kullanma olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d12b-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="1d12b-136">Kod odaklı veri erişim mantığı ile çalışma, yine de bir zengin, veri bağlama çerçevesi sağlarken basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="1d12b-137">Ürünleri görüntülemek</span><span class="sxs-lookup"><span data-stu-id="1d12b-137">Display products</span></span>

<span data-ttu-id="1d12b-138">Bu öğreticide, veri bağlama için model bağlama kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1d12b-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="1d12b-139">Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. `SelectMethod` özelliğini sayfanın kodda bir yöntem adı.</span><span class="sxs-lookup"><span data-stu-id="1d12b-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="1d12b-140">Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar.</span><span class="sxs-lookup"><span data-stu-id="1d12b-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="1d12b-141">Açıkça çağırmaya gerek yoktur `DataBind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d12b-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="1d12b-142">İçinde **Çözüm Gezgini**açın *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="1d12b-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="1d12b-143">Mevcut biçimlendirme, bu işaretleme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1d12b-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="1d12b-144">Bu kod bir **ListView** adlı Denetim `productList` ürünleri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1d12b-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="1d12b-145">Şablonları ve stilleri ile tanımladığınız nasıl **ListView** denetim verilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1d12b-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="1d12b-146">Tüm yinelenen yapısındaki veriler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="1d12b-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="1d12b-147">Ancak bu **ListView** örnek yalnızca veritabanı verileri görüntüler, ayrıca, kodu olmadan etkinleştir kullanıcıların düzenlemek, eklemek ve verileri silmek için ve sıralama ve veri sayfasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="1d12b-148">Ayarlayarak `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` kullanılabilir ve Denetim türü kesin belirlenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="1d12b-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="1d12b-149">Önceki öğreticide belirtildiği gibi belirtme gibi IntelliSense ile öğesi nesne ayrıntıları seçebilirsiniz `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="1d12b-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Veri öğelerini ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="1d12b-151">Model bağlama belirtmek için kullanıyorsanız bir `SelectMethod` değeri.</span><span class="sxs-lookup"><span data-stu-id="1d12b-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="1d12b-152">Bu değer (`GetProducts`) kodu eklemeniz yöntemine karşılık gelen arkasından sonraki adımda ürünleri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1d12b-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="1d12b-153">Ürünleri görüntülemek için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-153">Add code to display products</span></span>

<span data-ttu-id="1d12b-154">Bu adımda, doldurmak için kod ekleyeceksiniz **ListView** ürün verileri veritabanından denetimi.</span><span class="sxs-lookup"><span data-stu-id="1d12b-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="1d12b-155">Tüm ürünler ve tek bir kategori ürünler gösteren kod destekler.</span><span class="sxs-lookup"><span data-stu-id="1d12b-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="1d12b-156">İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* seçip **kodu görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="1d12b-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="1d12b-157">Varolan kodda değiştirin *ProductList.aspx.cs* bu dosya:</span><span class="sxs-lookup"><span data-stu-id="1d12b-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="1d12b-158">Bu kod gösterir `GetProducts` yöntemi, **ListView** denetimin `ItemType` özelliği başvurularının *ProductList.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="1d12b-159">Sonuçları belirli veritabanı kategorisi sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerini değerinden *ProductList.aspx* ne zaman sayfasında *ProductList.aspx* sayfası için gitme.</span><span class="sxs-lookup"><span data-stu-id="1d12b-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="1d12b-160">`QueryStringAttribute` Sınıfını `System.Web.ModelBinding` ad alanı değeri sorgu dizesi değişkeni almak için kullanılan `id`.</span><span class="sxs-lookup"><span data-stu-id="1d12b-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="1d12b-161">Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya için bildirir `categoryId` çalışma zamanında parametresi.</span><span class="sxs-lookup"><span data-stu-id="1d12b-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="1d12b-162">Geçerli bir kategori sorgu dizesi olarak sayfasına geçildiğinde, sorgunun sonuçlarını eşleşen bu ürünlere veritabanında sınırlı `categoryId` değeri.</span><span class="sxs-lookup"><span data-stu-id="1d12b-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="1d12b-163">Örneğin, *ProductsList.aspx* sayfası URL'si şudur:</span><span class="sxs-lookup"><span data-stu-id="1d12b-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="1d12b-164">Sayfa yalnızca ürünleri görüntüler. burada `categoryId` eşittir `1`.</span><span class="sxs-lookup"><span data-stu-id="1d12b-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="1d12b-165">Hiçbir sorgu dizesi dahildir tüm ürünleri görüntülenir *ProductList.aspx* sayfa çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1d12b-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="1d12b-166">Bu yöntemlere ait değerlerin kaynakları olarak ifade edilir *değer sağlayıcıları* (gibi *QueryString*), ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri adlandırılır*değer sağlayıcısı öznitelikleri* (gibi `id`).</span><span class="sxs-lookup"><span data-stu-id="1d12b-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="1d12b-167">ASP.NET, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özellikleri gibi bir Web Forms uygulamasındaki değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen özniteliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="1d12b-168">Özel değer sağlayıcıları da yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="1d12b-169">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1d12b-169">Run the application</span></span>

<span data-ttu-id="1d12b-170">Tüm ürünler ya da bir kategorinin ürünler görüntülemek için uygulamayı şimdi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1d12b-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="1d12b-171">Tuşuna **F5** uygulamayı çalıştırmak için Visual Studio çalışırken.</span><span class="sxs-lookup"><span data-stu-id="1d12b-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="1d12b-172">Tarayıcı açılır ve gösterir *Default.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="1d12b-173">Seçin **otomobiller** ürün kategorisi Gezinti menüsünde.</span><span class="sxs-lookup"><span data-stu-id="1d12b-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="1d12b-174">*ProductList.aspx* sayfası görüntüler yalnızca gösteren **otomobiller** kategori ürünleri.</span><span class="sxs-lookup"><span data-stu-id="1d12b-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="1d12b-175">Bu öğreticide daha sonra ürün ayrıntıları görüntüleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Veri öğelerini ve ayrıntıları - otomobiller](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="1d12b-177">Seçin **ürünleri** üst gezinti menüsünde.</span><span class="sxs-lookup"><span data-stu-id="1d12b-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="1d12b-178">Yeniden *ProductList.aspx* sayfası görüntülenir, ancak isteğe bağlı olarak şu ürünlerin tam listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="1d12b-180">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="1d12b-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="1d12b-181">Ürün Ayrıntıları görüntülemek için bir veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-181">Add a data control to display product details</span></span>

<span data-ttu-id="1d12b-182">Ardından, işaretlemede değiştireceksiniz *ProductDetails.aspx* belirli ürün bilgilerini görüntülemek için önceki öğreticide eklenen sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="1d12b-183">İçinde **Çözüm Gezgini**açın *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="1d12b-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="1d12b-184">Mevcut biçimlendirme, bu işaretleme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1d12b-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="1d12b-185">Bu kod bir **FormView** belirli bir ürün ayrıntılarını görüntülemek için denetim.</span><span class="sxs-lookup"><span data-stu-id="1d12b-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="1d12b-186">Bu işaretleme verileri görüntülemek için kullanılan yöntemler gibi yöntemleri kullanan *ProductList.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="1d12b-187">**FormView** denetimi, tek bir kaydı aynı anda bir veri kaynağından görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d12b-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="1d12b-188">Kullanırken **FormView** denetimi görüntülemek ve veri bağlama değerlerini düzenlemek için şablonlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1d12b-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="1d12b-189">Bu şablonlar ifadeleri, bağlama denetimleri içerir ve biçimlendirme formun görünümü ve yeni işlevleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="1d12b-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="1d12b-190">Önceki biçimlendirme veritabanına bağlanırken, ek kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="1d12b-191">İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* ve ardından **kodu görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="1d12b-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="1d12b-192">*ProductDetails.aspx.cs* dosyası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="1d12b-193">Varolan kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1d12b-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="1d12b-194">Bu kod denetleyen bir "`productID`" sorgu dizesi değeri.</span><span class="sxs-lookup"><span data-stu-id="1d12b-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="1d12b-195">Geçerli sorgu dize bulunursa eşleşen ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="1d12b-196">Sorgu-dize bulunamadığında veya değeri geçerli değil, hiçbir ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="1d12b-197">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1d12b-197">Run the application</span></span>

<span data-ttu-id="1d12b-198">Görüntülenen tek bir ürün görmek için uygulamayı çalıştırabilirsiniz artık ürün kimliğini temel alan</span><span class="sxs-lookup"><span data-stu-id="1d12b-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="1d12b-199">Tuşuna **F5** uygulamayı çalıştırmak için Visual Studio çalışırken.</span><span class="sxs-lookup"><span data-stu-id="1d12b-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="1d12b-200">Tarayıcı açılır ve gösterir *Default.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="1d12b-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="1d12b-201">Seçin **Boats** kategori Gezinti menüsünde.</span><span class="sxs-lookup"><span data-stu-id="1d12b-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="1d12b-202">*ProductList.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="1d12b-203">Seçin **kağıt bot** ürün listesinde.</span><span class="sxs-lookup"><span data-stu-id="1d12b-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="1d12b-204">*ProductDetails.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d12b-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="1d12b-206">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="1d12b-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1d12b-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1d12b-207">Additional resources</span></span>

[<span data-ttu-id="1d12b-208">Model bağlama ve web forms ile verileri alma ve görüntüleme</span><span class="sxs-lookup"><span data-stu-id="1d12b-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="1d12b-209">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="1d12b-209">Next steps</span></span>

<span data-ttu-id="1d12b-210">Bu öğreticide, biçimlendirme ve ürünler ve ürün ayrıntıları görüntülemek için koddaki eklendi.</span><span class="sxs-lookup"><span data-stu-id="1d12b-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="1d12b-211">Kesin türü belirtilmiş veri denetimleri, model bağlama ve değer sağlayıcıları hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="1d12b-212">Sonraki öğreticide, Wingtip Toys örnek uygulamaya bir alışveriş sepeti ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="1d12b-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="1d12b-213">[Önceki](ui_and_navigation.md)
> [İleri](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="1d12b-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
