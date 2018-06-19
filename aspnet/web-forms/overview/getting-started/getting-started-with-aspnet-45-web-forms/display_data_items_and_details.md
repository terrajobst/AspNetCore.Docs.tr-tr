---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görüntü veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892575"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="f8e80-103">Görüntü veri öğelerini ve ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="f8e80-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="f8e80-104">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="f8e80-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="f8e80-105">[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="f8e80-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="f8e80-106">Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="f8e80-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="f8e80-107">Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.</span><span class="sxs-lookup"><span data-stu-id="f8e80-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="f8e80-108">Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First kullanarak veri öğesi ayrıntılarını görüntülemek açıklar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="f8e80-109">Bu öğretici önceki öğreticiyi "Kullanıcı Arabirimi ve gezinti" oluşturur ve Wingtip Oyuncak deposu öğretici serinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="f8e80-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="f8e80-110">Bu öğreticiyi tamamladıktan sonra ürünleri görmek kullanabileceksiniz *ProductsList.aspx* sayfası ve tek tek bir üründe ayrıntılarını *ProductDetails.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="f8e80-111">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="f8e80-111">What you'll learn:</span></span>

- <span data-ttu-id="f8e80-112">Veritabanından ürünleri görüntülemek için bir veri denetim ekleme.</span><span class="sxs-lookup"><span data-stu-id="f8e80-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="f8e80-113">Bir veri denetimi seçilen verileri bağlanma.</span><span class="sxs-lookup"><span data-stu-id="f8e80-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="f8e80-114">Veritabanından ürün ayrıntıları görüntülemek için bir veri denetim ekleme.</span><span class="sxs-lookup"><span data-stu-id="f8e80-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="f8e80-115">Nasıl Sorgu dizesinden bir değer almak ve veritabanından alınan veri sınırlamak için bu değeri kullanın.</span><span class="sxs-lookup"><span data-stu-id="f8e80-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="f8e80-116">Bu öğreticide sunulan özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f8e80-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="f8e80-117">Model Binding</span><span class="sxs-lookup"><span data-stu-id="f8e80-117">Model Binding</span></span>
- <span data-ttu-id="f8e80-118">Değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="f8e80-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="f8e80-119">Ürünleri görüntülemek için veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="f8e80-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="f8e80-120">Sunucu denetimine veri bağlama sırasında kullanabileceğiniz birkaç farklı seçenekler vardır.</span><span class="sxs-lookup"><span data-stu-id="f8e80-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="f8e80-121">En yaygın Seçenekler şunlardır: bir veri kaynağı denetimi ekleme, kod el ile ekleme veya model bağlama kullanarak.</span><span class="sxs-lookup"><span data-stu-id="f8e80-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="f8e80-122">Veri bağlama için bir veri kaynağı denetimi kullanma</span><span class="sxs-lookup"><span data-stu-id="f8e80-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="f8e80-123">Veri kaynağı denetim ekleme verileri görüntüler denetimi veri kaynağı denetimi bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="f8e80-124">Bu yaklaşım bildirimli olarak sunucu tarafı denetimleri doğrudan veri kaynakları için programlı bir yaklaşım kullanmak yerine bağlanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="f8e80-125">Veri bağlamak için el ile kodlama</span><span class="sxs-lookup"><span data-stu-id="f8e80-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="f8e80-126">El ile kod içerir ekleme değeri okunurken, null değerini denetimi, uygun türe dönüştürme yapma, dönüştürme başarılı olup olmadığını denetleme ve son olarak, sorguyu değeri'ı kullanarak.</span><span class="sxs-lookup"><span data-stu-id="f8e80-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="f8e80-127">Veri erişimi mantığınızı üzerinde tam denetimi korumak istediğinizde bu yaklaşımı kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="f8e80-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="f8e80-128">Veri bağlama bağlama modelini kullanma</span><span class="sxs-lookup"><span data-stu-id="f8e80-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="f8e80-129">Model bağlama kullanarak, çok daha az kod kullanarak sonuçları bağlamanıza olanak sağlar ve uygulamanız genelinde işlevini yeniden kullanma olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="f8e80-130">Bir zengin, veri bağlama çerçeve yararları hala korurken kod odaklı veri erişimi mantığı ile çalışma basitleştirmek için model bağlama amaçlar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="f8e80-131">Ürünler görüntüleme</span><span class="sxs-lookup"><span data-stu-id="f8e80-131">Displaying Products</span></span>

<span data-ttu-id="f8e80-132">Bu öğreticide, veri bağlamak için model bağlama kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="f8e80-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="f8e80-133">Model bağlama verileri seçmek için kullanılacak veri denetimini yapılandırmak için denetimin ayarlayın `SelectMethod` özelliğini sayfanın kodda bir yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="f8e80-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="f8e80-134">Veri denetimi sayfa yaşam döngüsündeki uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar.</span><span class="sxs-lookup"><span data-stu-id="f8e80-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="f8e80-135">Açıkça çağırmak için gerek yoktur `DataBind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f8e80-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="f8e80-136">Aşağıdaki adımları kullanarak, biçimlendirmede değiştireceksiniz *ProductList.aspx* sayfa ürünleri görüntüleyebilmesi sayfa.</span><span class="sxs-lookup"><span data-stu-id="f8e80-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="f8e80-137">İçinde **Çözüm Gezgini**, açık *ProductList.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="f8e80-138">Varolan biçimlendirme aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f8e80-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="f8e80-139">Bu kodu kullanan bir **ListView** ürünleri görüntülemek için "productList" adlı bir denetim.</span><span class="sxs-lookup"><span data-stu-id="f8e80-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="f8e80-140">**ListView** denetim şablonları ve stiller kullanarak tanımladığınız bir biçimde verileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f8e80-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="f8e80-141">Yinelenen bir yapı veriler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="f8e80-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="f8e80-142">Bu **ListView** örnek yalnızca veritabanı, ancak kullanıcılar düzenleme, ekleme ve verileri silmek ve sıralamak için etkinleştirebilir ve tüm Kodsuz sayfa verileri verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="f8e80-143">Ayarlayarak `ItemType` özelliğinde **ListView** denetlemek, veri bağlama ifadesi `Item` kullanılabilir ve denetim kesin türü belirtilmiş haline gelir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="f8e80-144">Önceki öğreticide belirtildiği gibi IntelliSense, belirtme gibi kullanarak öğesi nesnesi ayrıntılarını seçebilirsiniz `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="f8e80-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Görüntüleme veri öğeleri ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="f8e80-146">Ayrıca, belirtmek için model bağlama kullanmakta olduğunuz bir `SelectMethod` değeri.</span><span class="sxs-lookup"><span data-stu-id="f8e80-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="f8e80-147">Bu değer (`GetProducts`) sonraki adımda ürünler görüntülenecek arkasındaki kodda ekleyeceksiniz yöntemi karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="f8e80-148">Ürünleri görüntülemek için kod ekleme</span><span class="sxs-lookup"><span data-stu-id="f8e80-148">Adding Code to Display Products</span></span>

<span data-ttu-id="f8e80-149">Bu adımda, doldurmak için kod ekleyeceksiniz **ListView** ürün verileri veritabanından denetimiyle.</span><span class="sxs-lookup"><span data-stu-id="f8e80-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="f8e80-150">Kodu gösterme ürünleri tek tek Kategori yanı sıra tüm ürünleri gösteren tarafından destekleyecektir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="f8e80-151">İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* ve ardından **görünümü kodu**.</span><span class="sxs-lookup"><span data-stu-id="f8e80-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="f8e80-152">Varolan kodla *ProductList.aspx.cs* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="f8e80-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="f8e80-153">Bu kod gösterir `GetProducts` tarafından başvurulan yöntemi `ItemType` özelliği **ListView** denetim *ProductList.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="f8e80-154">Veritabanı belirli bir kategorideki sonuçları sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerini değerinden *ProductList.aspx* ne zaman sayfa *ProductList.aspx* sayfası için gittiğinizde.</span><span class="sxs-lookup"><span data-stu-id="f8e80-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="f8e80-155">`QueryStringAttribute` Sınıfını `System.Web.ModelBinding` ad alanı, sorgu dizesi değişkeni kimliği değerini almak için kullanılır. Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya bildirir `categoryId` çalışma zamanında parametre.</span><span class="sxs-lookup"><span data-stu-id="f8e80-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="f8e80-156">Geçerli bir kategori sayfasına bir sorgu dizesi geçirildiğinde, sorgunun sonuçlarını uyan ürünleri için veritabanında sınırlı `categoryId` değeri.</span><span class="sxs-lookup"><span data-stu-id="f8e80-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="f8e80-157">Örneği için URL'ye *ProductsList.aspx* sayfasıdır aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="f8e80-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="f8e80-158">Yalnızca ürün sayfasını görüntüler nerede `category` eşittir `1`.</span><span class="sxs-lookup"><span data-stu-id="f8e80-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="f8e80-159">Hiçbir sorgu dizesi için gezinirken eklenirse *ProductList.aspx* sayfası, tüm ürünleri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="f8e80-160">Bu yöntemlere ait değerlerin kaynakları denir *değer sağlayıcıları* (gibi *QueryString*), ve kullanacağı değer sağlayıcısını belirtmek parametre öznitelikleri değeri olarak adlandırılır Sağlayıcı öznitelikleri (gibi "`id`").</span><span class="sxs-lookup"><span data-stu-id="f8e80-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="f8e80-161">ASP.NET Web Forms uygulamasında, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimleri, Görünüm durumu, oturum durumu ve profil özellikleri gibi değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen öznitelikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="f8e80-162">Özel değer sağlayıcıları de yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8e80-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="f8e80-163">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f8e80-163">Running the Application</span></span>

<span data-ttu-id="f8e80-164">Tüm ürünleri veya ürün kategorisine göre sınırlı yalnızca bir dizi nasıl görüntüleyebileceğiniz görmek için uygulamayı şimdi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f8e80-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="f8e80-165">İçinde **Çözüm Gezgini**, sağ *Default.aspx* sayfasından seçim yapıp **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="f8e80-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="f8e80-166">Tarayıcıyı açın ve göstermek *Default.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="f8e80-167">Seçin **araba** ürün kategorisi Gezinti menüsünde.</span><span class="sxs-lookup"><span data-stu-id="f8e80-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="f8e80-168">*ProductList.aspx* sayfası, yalnızca "Araba" kategorisindeki ürünleri gösteren görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="f8e80-169">Daha sonra Bu öğreticide, ürün ayrıntıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f8e80-169">Later in this tutorial, you will display product details.</span></span>  

    ![Görüntüleme veri öğeleri ve ayrıntıları - araba](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="f8e80-171">Seçin **ürünleri** üst gezinti menüsünde.</span><span class="sxs-lookup"><span data-stu-id="f8e80-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="f8e80-172">Yeniden *ProductList.aspx* sayfası görüntülenir, ancak isteğe bağlı olarak bu zaman tüm ürünlerin listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Görüntüleme veri öğeleri ve ayrıntıları - ürünler](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="f8e80-174">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="f8e80-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="f8e80-175">Ürün Ayrıntıları görüntülemek için veri denetim ekleme</span><span class="sxs-lookup"><span data-stu-id="f8e80-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="f8e80-176">Ardından, biçimlendirmede değiştireceksiniz *ProductDetails.aspx* sayfa ayrı bir ürün hakkında bilgi görüntüleyebilmesi önceki öğreticide eklediğiniz sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="f8e80-177">İçinde **Çözüm Gezgini**, açık *ProductDetails.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="f8e80-178">Varolan biçimlendirme aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f8e80-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="f8e80-179">Bu kodu kullanan bir **FormView** ayrı bir ürün hakkındaki ayrıntıları görüntülemek için denetim.</span><span class="sxs-lookup"><span data-stu-id="f8e80-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="f8e80-180">Bu biçimlendirme verileri görüntülemek için kullanılan gelenler yöntemleri kullanır *ProductList.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="f8e80-181">**FormView** denetimi, bir veri kaynağından bir kerede tek bir kaydını görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f8e80-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="f8e80-182">Kullandığınızda **FormView** denetimi, görüntüle ve veri bağlama değerlerini düzenlemek için şablonlar oluşturma.</span><span class="sxs-lookup"><span data-stu-id="f8e80-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="f8e80-183">Şablonları görünümünü ve form işlevselliğini tanımlayan ifadeleri bağlama ve biçimlendirme denetimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="f8e80-184">Yukarıdaki biçimlendirme veritabanına bağlanmak için ek kod ekleme *ProductDetails.aspx* kodu.</span><span class="sxs-lookup"><span data-stu-id="f8e80-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="f8e80-185">İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* ve ardından **görünümü kodu**.</span><span class="sxs-lookup"><span data-stu-id="f8e80-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="f8e80-186">*ProductDetails.aspx.cs* dosya görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="f8e80-187">Var olan kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f8e80-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="f8e80-188">Bu kod denetler bir "`productID`" sorgu dizesi değeri.</span><span class="sxs-lookup"><span data-stu-id="f8e80-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="f8e80-189">Geçerli sorgu dize bulunursa, eşleşen ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="f8e80-190">Hiçbir sorgu dizesi bulunamadı veya sorgu dizesi değeri geçerli değil, hiçbir ürün görüntülenen *ProductDetails.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="f8e80-191">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f8e80-191">Running the Application</span></span>

<span data-ttu-id="f8e80-192">Görüntülenen bir bireysel ürün görmek için uygulamayı çalıştırın artık ürün kimliği temel alarak.</span><span class="sxs-lookup"><span data-stu-id="f8e80-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="f8e80-193">Tuşuna **F5** while uygulamayı çalıştırmak için Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8e80-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="f8e80-194">Tarayıcıyı açın ve göstermek *Default.aspx* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f8e80-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="f8e80-195">"Boats" kategorisi Gezinti menüsünden seçin.</span><span class="sxs-lookup"><span data-stu-id="f8e80-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="f8e80-196">*ProductList.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="f8e80-197">"Kağıt bot" Ürün ürün listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="f8e80-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="f8e80-198">*ProductDetails.aspx* sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f8e80-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Görüntüleme veri öğeleri ve ayrıntıları - ürünler](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="f8e80-200">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="f8e80-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="f8e80-201">Özet</span><span class="sxs-lookup"><span data-stu-id="f8e80-201">Summary</span></span>

<span data-ttu-id="f8e80-202">Serinin Bu öğreticide sahip eklediğiniz biçimlendirme ve kodun ürün listesini görüntülemek ve ürün ayrıntıları görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="f8e80-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="f8e80-203">Bu işlem sırasında kesin türü belirtilmiş veri denetimleri, model bağlama ve değer sağlayıcıları hakkında öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="f8e80-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="f8e80-204">Sonraki öğreticide Wingtip Toys örnek uygulamaya alışveriş sepeti ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f8e80-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8e80-205">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8e80-205">Additional Resources</span></span>

[<span data-ttu-id="f8e80-206">Model bağlama ve web forms ile verilerini görüntüleme ve alma</span><span class="sxs-lookup"><span data-stu-id="f8e80-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="f8e80-207">[Önceki](ui_and_navigation.md)
> [sonraki](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="f8e80-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
