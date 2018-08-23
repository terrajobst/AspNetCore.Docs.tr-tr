---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Basit doğrulama (C#) gerçekleştirme | Microsoft Docs
author: StephenWalther
description: Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğreneceksiniz. Bu öğreticide, model durumu ve doğrulama HTML Yardımcısı Stephen Walther sunar...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 14d7857c64268df3b998e05797f749f03509dd4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756306"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="fce26-104">Basit doğrulama (C#) gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="fce26-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="fce26-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="fce26-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="fce26-106">Bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="fce26-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="fce26-107">Bu öğreticide, model durumu ve doğrulama HTML Yardımcıları Stephen Walther tanıtır.</span><span class="sxs-lookup"><span data-stu-id="fce26-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="fce26-108">Bu öğreticide bir ASP.NET MVC uygulaması içindeki doğrulama nasıl gerçekleştirebileceğiniz açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="fce26-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="fce26-109">Örneğin, birisi, gerekli bir alan için bir değer içermeyen bir form göndermesinin önlenmesine hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fce26-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="fce26-110">Model durumu ve doğrulama HTML Yardımcıları kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fce26-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="fce26-111">Model durumu anlama</span><span class="sxs-lookup"><span data-stu-id="fce26-111">Understanding Model State</span></span>

<span data-ttu-id="fce26-112">Doğrulama hataları temsil eden bir model durumu - veya daha doğru bir şekilde ve model durumu sözlüğündeki-'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fce26-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="fce26-113">Örneğin, ürün sınıfı için bir veritabanı eklemeden önce bir ürün sınıf özelliklerini listeleme 1 Create() eylemi doğrular.</span><span class="sxs-lookup"><span data-stu-id="fce26-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="fce26-114">Ben bir denetleyiciye doğrulama veya veritabanı mantığınızı eklemek öneren değil.</span><span class="sxs-lookup"><span data-stu-id="fce26-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="fce26-115">Bir denetleyici yalnızca mantık uygulaması akış denetimi ile ilgili içermelidir.</span><span class="sxs-lookup"><span data-stu-id="fce26-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="fce26-116">Örneği basit tutmak için bir kısayol sürüyor.</span><span class="sxs-lookup"><span data-stu-id="fce26-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="fce26-117">**1 - Controllers\ProductController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="fce26-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="fce26-118">Listeleme 1'de ürün sınıfın adını, açıklamasını ve unitsInStock özelliklerini doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="fce26-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="fce26-119">Bu özelliklerin herhangi bir doğrulama testi başarısız olursa bir hata (denetleyici sınıfının ModelState özelliği tarafından gösterilen) model durumu sözlüğüne eklenir.</span><span class="sxs-lookup"><span data-stu-id="fce26-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="fce26-120">Model durumu hataları varsa ModelState.IsValid özelliği false döndürür.</span><span class="sxs-lookup"><span data-stu-id="fce26-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="fce26-121">HTML formu yeni bir ürün oluşturmaya yönelik bu durumda görünürler.</span><span class="sxs-lookup"><span data-stu-id="fce26-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="fce26-122">Aksi takdirde, doğrulama hatası varsa, yeni ürün veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="fce26-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="fce26-123">Doğrulama yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="fce26-123">Using the Validation Helpers</span></span>

<span data-ttu-id="fce26-124">ASP.NET MVC çerçevesi iki doğrulama Yardımcıları içerir: Html.ValidationMessage() Yardımcısı ve Html.ValidationSummary() Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="fce26-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="fce26-125">Doğrulama hatası iletilerini görüntülemek için Görünümü'nde bu iki Yardımcıları kullanın.</span><span class="sxs-lookup"><span data-stu-id="fce26-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="fce26-126">Html.ValidationMessage() ve Html.ValidationSummary() Yardımcıları, ASP.NET MVC yapı iskelesi tarafından otomatik olarak oluşturulan oluşturma ve düzenleme görünümlerinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="fce26-127">Oluştur görünümünün oluşturmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="fce26-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="fce26-128">Menü seçeneği ürün denetleyicisi Create() eylemi sağ tıklayıp **Görünüm Ekle** (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="fce26-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="fce26-129">İçinde **Görünüm Ekle** iletişim kutusunda etiketli onay **kesin türü belirtilmiş görünüm oluşturmak** (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="fce26-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="fce26-130">Gelen **görüntülemek veri sınıfı** açılan listesinde, ürün sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="fce26-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="fce26-131">Gelen **içeriği görüntüle** açılır listesi, select oluşturma.</span><span class="sxs-lookup"><span data-stu-id="fce26-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="fce26-132">Tıklayın **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fce26-132">Click the **Add** button.</span></span>


<span data-ttu-id="fce26-133">Uygulamanızı bir görünüm eklemeden önce oluşturduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="fce26-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="fce26-134">Sınıf listesi Aksi takdirde görünmez **görüntülemek veri sınıfı** açılır liste.</span><span class="sxs-lookup"><span data-stu-id="fce26-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="fce26-135">[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fce26-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="fce26-136">**Şekil 01**: Görünüm ekleme ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fce26-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="fce26-137">[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fce26-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="fce26-138">**Şekil 02**: kesin türü belirtilmiş görünüm oluşturmak ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="fce26-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="fce26-139">Bu adımları tamamladıktan sonra Oluştur görünümünün listeleme 2'de sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="fce26-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="fce26-140">**2 - Views\Product\Create.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="fce26-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="fce26-141">Listeleme 2'de Html.ValidationSummary() Yardımcısı hemen HTML form üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="fce26-142">Bu yardımcı, doğrulama hata iletilerinin listesini görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="fce26-143">Bir madde işaretli liste hataları Html.ValidationSummary() Yardımcısı işler.</span><span class="sxs-lookup"><span data-stu-id="fce26-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="fce26-144">Html.ValidationMessage() Yardımcısı her HTML form alanlarının yanındaki olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="fce26-145">Bu yardımcı, bir form alanının yanında şu hata iletisi görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="fce26-146">Bir hata olduğunda listeleme 2 söz konusu olduğunda, bir yıldız işareti Html.ValidationMessage() Yardımcısı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fce26-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="fce26-147">Şekil 3'te sayfanın eksik alanlar geçersiz değerler ile form gönderildiğinde, doğrulama Yardımcılar tarafından işlenen hata iletilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fce26-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="fce26-148">[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fce26-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="fce26-149">**Şekil 03**: sorunları gönderilen görünüm oluştur ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fce26-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="fce26-150">HTML görünümünü bir doğrulama hatası olduğunda alanlar da değiştirilmez giriş dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="fce26-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="fce26-151">Html.TextBox() yardımcı işleme bir *sınıfı "giriş doğrulama hata" =* Html.TextBox() Yardımcısı tarafından işlenen özelliği ilişkili doğrulama hatası olduğunda özniteliği.</span><span class="sxs-lookup"><span data-stu-id="fce26-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="fce26-152">Doğrulama hataları görünümünü kontrol etmek için kullanılan üç basamaklı stil sayfası sınıfları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fce26-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="fce26-153">Giriş-doğrulama-hata - uygulanan &lt;giriş&gt; Html.TextBox() Yardımcısı tarafından işlenen etiketi.</span><span class="sxs-lookup"><span data-stu-id="fce26-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="fce26-154">alan-doğrulama-hata - uygulanan &lt;span&gt; Html.ValidationMessage() Yardımcısı tarafından işlenen etiketi.</span><span class="sxs-lookup"><span data-stu-id="fce26-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="fce26-155">Doğrulama-Özeti-hata - uygulanan &lt;ul&gt; Html.ValidationSumamry() Yardımcısı tarafından işlenen etiketi.</span><span class="sxs-lookup"><span data-stu-id="fce26-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="fce26-156">Bu geçişli stil sayfası sınıfları değiştirebilir ve bu nedenle içerik klasöründe yer alan Site.css dosyasını değiştirerek doğrulama hatalarını görünümünü değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fce26-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="fce26-157">CSS ilgili doğrulama adları alınırken HtmlHelper sınıfı salt okunur statik özelliklerini içeren sınıflar.</span><span class="sxs-lookup"><span data-stu-id="fce26-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="fce26-158">Bu statik özellikleri ValidationInputCssClassName ValidationFieldCssClassName ve ValidationSummaryCssClassName adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="fce26-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="fce26-159">Doğrulama ve Postbinding doğrulama prebinding</span><span class="sxs-lookup"><span data-stu-id="fce26-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="fce26-160">Bir ürün oluşturmaya HTML formu göndermeden ve Fiyat alanının ve unitsInStock alan için hiçbir değer için geçersiz bir değer girin, Şekil 4'te görüntülenen doğrulama iletilerinin elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="fce26-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="fce26-161">Burada bu doğrulama hata iletileri gelir?</span><span class="sxs-lookup"><span data-stu-id="fce26-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="fce26-162">[![Yeni Proje iletişim kutusu](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fce26-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="fce26-163">**Şekil 04**: Prebinding doğrulama hataları ([tam boyutlu görüntüyü görmek için tıklatın](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="fce26-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="fce26-164">Doğrulama hata iletilerinin - HTML form alanlarını bir sınıfa bağlanır ve form alanlarını sınıfa bağlı sonra bu oluşturulan önce oluşturulanların aslında iki türü vardır.</span><span class="sxs-lookup"><span data-stu-id="fce26-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="fce26-165">Diğer bir deyişle, vardır prebinding doğrulama hatalarını ve postbinding doğrulama hataları.</span><span class="sxs-lookup"><span data-stu-id="fce26-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="fce26-166">Listeleme 1 ürün denetleyicisi tarafından kullanıma sunulan Create() eylem ürün sınıfının bir örneği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="fce26-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="fce26-167">Oluşturma metodun imzası şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="fce26-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="fce26-168">HTML form alanlarını oluşturma formundaki değerlerini bir model bağlayıcı olarak adlandırılan bir şey tarafından productToCreate sınıfa bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fce26-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="fce26-169">Varsayılan model bağlayıcısını bir hata iletisi model durumuna bir form alanı için bir form özelliği bağlanamıyor olduğunda otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="fce26-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="fce26-170">Varsayılan model bağlayıcısını, ürün sınıfın fiyat özelliği için dize "apple" bağlanamıyor.</span><span class="sxs-lookup"><span data-stu-id="fce26-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="fce26-171">Bir dize ondalık bir özelliğe atanamaz.</span><span class="sxs-lookup"><span data-stu-id="fce26-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="fce26-172">Bu nedenle, model bağlayıcı hata model durumuna ekler.</span><span class="sxs-lookup"><span data-stu-id="fce26-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="fce26-173">Varsayılan model bağlayıcısını null değerlere kabul etmeyen bir özellik için null değer atama yapılamıyor.</span><span class="sxs-lookup"><span data-stu-id="fce26-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="fce26-174">Özellikle, model bağlayıcı unitsInStock özelliği bir null değer atanamaz.</span><span class="sxs-lookup"><span data-stu-id="fce26-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="fce26-175">Bir kez daha, model bağlayıcı Vazgeçmeden ve bir hata iletisini model durumuna ekler.</span><span class="sxs-lookup"><span data-stu-id="fce26-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="fce26-176">Hata iletileri prebinding bu görünümünü özelleştirmek istiyorsanız, bu iletileri görmek için kaynak dizeleri oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="fce26-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="fce26-177">Özet</span><span class="sxs-lookup"><span data-stu-id="fce26-177">Summary</span></span>

<span data-ttu-id="fce26-178">ASP.NET MVC çerçevesi doğrulama temel mekanikleri açıklamak için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="fce26-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="fce26-179">Model durumu ve doğrulama HTML Yardımcıları kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="fce26-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="fce26-180">Ayrıca prebinding ve doğrulama postbinding arasında ayrım ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="fce26-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="fce26-181">Diğer öğreticileri, doğrulama kodunuzu denetleyicilerinizi ve model sınıflarınızı içine taşımak için çeşitli stratejileri açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="fce26-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fce26-182">[Önceki](displaying-a-table-of-database-data-cs.md)
> [İleri](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fce26-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
