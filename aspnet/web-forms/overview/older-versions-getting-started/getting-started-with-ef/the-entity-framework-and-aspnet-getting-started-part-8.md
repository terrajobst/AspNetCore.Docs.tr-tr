---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölümü 8 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="6fdef-104">Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölümü 8</span><span class="sxs-lookup"><span data-stu-id="6fdef-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="6fdef-105">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6fdef-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6fdef-106">Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6fdef-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="6fdef-107">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="6fdef-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="6fdef-108">Biçimlendirmek ve verileri doğrulamak için dinamik veri işlevlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6fdef-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="6fdef-109">Önceki öğreticide saklı yordamlar uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="6fdef-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="6fdef-110">Bu öğretici nasıl dinamik veri işlevlerini aşağıdaki faydaları sağlayabilir gösterir:</span><span class="sxs-lookup"><span data-stu-id="6fdef-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="6fdef-111">Alanlar, kendi veri türüne göre görüntülemek için otomatik olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6fdef-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="6fdef-112">Alanları otomatik olarak kendi veri türüne göre doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="6fdef-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="6fdef-113">Meta veri biçimlendirmeyi ve doğrulama davranışını özelleştirmek için veri modeli ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fdef-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="6fdef-114">Bunu yapmak için yalnızca tek bir yerde biçimlendirmeyi ve doğrulama kuralları ekleyebilirsiniz ve bunlar otomatik olarak her yerde uygulanan zaman dinamik veri denetimleri kullanarak alanlara erişim.</span><span class="sxs-lookup"><span data-stu-id="6fdef-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="6fdef-115">Bunun nasıl çalıştığını görmek için kullandığınız görüntülemek ve varolan alanları düzenlemek için denetimleri değiştireceğiz *Students.aspx* sayfası ve ekleyeceğiz biçimlendirmeyi ve doğrulama meta verileri için ad ve tarih alanlarını `Student` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="6fdef-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="6fdef-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="6fdef-117">DynamicField ve DynamicControl denetimleri kullanma</span><span class="sxs-lookup"><span data-stu-id="6fdef-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="6fdef-118">Açık *Students.aspx* sayfa ve `StudentsGridView` denetimi Değiştir **adı** ve **kayıt tarihi** `TemplateField` aşağıdaki biçimlendirme ile öğeleri:</span><span class="sxs-lookup"><span data-stu-id="6fdef-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="6fdef-119">Bu biçimlendirme kullanan `DynamicControl` yerine denetimleri `TextBox` ve `Label` Öğrenci denetimlerinde şablon alan adı ve kullanır bir `DynamicField` denetimi için kayıt tarihi.</span><span class="sxs-lookup"><span data-stu-id="6fdef-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="6fdef-120">Biçim dizeleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="6fdef-120">No format strings are specified.</span></span>

<span data-ttu-id="6fdef-121">Ekleme bir `ValidationSummary` sonra Denetim `StudentsGridView` denetim.</span><span class="sxs-lookup"><span data-stu-id="6fdef-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="6fdef-122">İçinde `SearchGridView` denetim değiştirmek için biçimlendirme **adı** ve **kayıt tarihi** sütunları, olarak yaptığınız `StudentsGridView` atlayın dışında kontrol `EditItemTemplate` öğesi.</span><span class="sxs-lookup"><span data-stu-id="6fdef-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="6fdef-123">`Columns` Öğesinin `SearchGridView` denetimi şimdi aşağıdaki biçimlendirme içerir:</span><span class="sxs-lookup"><span data-stu-id="6fdef-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="6fdef-124">Açık *Students.aspx.cs* ve aşağıdakileri ekleyin `using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="6fdef-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="6fdef-125">Sayfa için bir işleyici ekleyin `Init` olay:</span><span class="sxs-lookup"><span data-stu-id="6fdef-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="6fdef-126">Bu kod dinamik veri biçimlendirme ve bu verilere bağlı denetimler alanları için doğrulama sağlayacak belirtir `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="6fdef-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="6fdef-127">Sayfa çalıştırdığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsanız, bu genellikle çağırmayı unuttunuz anlamına gelir `EnableDynamicData` yönteminde `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="6fdef-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="6fdef-128">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6fdef-128">Run the page.</span></span>

<span data-ttu-id="6fdef-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="6fdef-130">İçinde **kayıt tarihi** sütun, özellik türü olduğundan süresi tarihi ile birlikte görüntülenir `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="6fdef-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="6fdef-131">Daha sonra düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="6fdef-131">You'll fix that later.</span></span>

<span data-ttu-id="6fdef-132">Şu an için dinamik veri otomatik olarak temel veri doğrulama sağlar dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6fdef-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="6fdef-133">Örneğin, **Düzenle**, tarih alanını temizleyin, tıklatın **güncelleştirme**, ve değeri veri modelinde boş değer atanabilir olmadığından dinamik veri otomatik olarak bu gerekli bir alan sağlar, bkz.</span><span class="sxs-lookup"><span data-stu-id="6fdef-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="6fdef-134">Alan ve bir hata iletisi sonra yıldız sayfasını görüntüler `ValidationSummary` denetimi:</span><span class="sxs-lookup"><span data-stu-id="6fdef-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="6fdef-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="6fdef-136">Atlayın `ValidationSummary` denetim fare işaretçisini hata iletisini görmek için yıldız tutabilir çünkü:</span><span class="sxs-lookup"><span data-stu-id="6fdef-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="6fdef-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="6fdef-138">Dinamik veri de girilen verilerin doğrulama **kayıt tarihi** alandır geçerli bir tarih:</span><span class="sxs-lookup"><span data-stu-id="6fdef-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="6fdef-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="6fdef-140">Gördüğünüz gibi genel bir hata iletisi budur.</span><span class="sxs-lookup"><span data-stu-id="6fdef-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="6fdef-141">Sonraki bölümde iletileri yanı sıra doğrulama ve biçimlendirme kuralları nasıl özelleştirileceği görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="6fdef-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="6fdef-142">Veri modeli için meta veri ekleme</span><span class="sxs-lookup"><span data-stu-id="6fdef-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="6fdef-143">Genellikle, dinamik veri tarafından sağlanan işlevleri özelleştirmek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="6fdef-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="6fdef-144">Örneğin, verilerin nasıl görüntüleneceğini ve hata iletileri içeriği değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fdef-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="6fdef-145">Genellikle ayrıca dinamik veri sağladıkları'den daha fazla işlevsellik sağlamak için veri doğrulama kuralları otomatik olarak veri türlerine göre özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6fdef-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="6fdef-146">Bunu yapmak için varlık türleri için karşılık gelen kısmi sınıflar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6fdef-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="6fdef-147">İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje, select **Başvuru Ekle**ve bir başvuru ekleyin `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="6fdef-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="6fdef-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="6fdef-149">İçinde *DAL* klasörü, yeni bir sınıf dosyası oluşturun, adlandırın *Student.cs*ve şablon kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6fdef-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="6fdef-150">Bu kod için bir parçalı sınıf oluşturur `Student` varlık.</span><span class="sxs-lookup"><span data-stu-id="6fdef-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="6fdef-151">`MetadataType` Bu sınıfa uygulanan öznitelik meta verileri belirtmek için kullandığınız sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6fdef-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="6fdef-152">Meta veri sınıfının herhangi bir ad olabilir, ancak varlık adı artı "Meta veri" kullanarak yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="6fdef-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="6fdef-153">Meta veri sınıfının özelliklerine uygulanan öznitelikleri biçimlendirme, doğrulama, kuralları ve hata iletileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="6fdef-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="6fdef-154">Burada gösterilen öznitelikleri aşağıdaki sonuçları olacaktır:</span><span class="sxs-lookup"><span data-stu-id="6fdef-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="6fdef-155">`EnrollmentDate`Tarih (olmadan bir saat) olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6fdef-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="6fdef-156">Daha düşük bir değer uzunluğu ve bir özel hata iletisi sağlanan veya her iki ad alanlarını 25 karakter olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fdef-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="6fdef-157">Her iki ad alanları gereklidir ve bir özel hata iletisi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6fdef-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="6fdef-158">Çalıştırma *Students.aspx* yeniden sayfasında ve tarihleri kez şimdi görüntülenen bakın:</span><span class="sxs-lookup"><span data-stu-id="6fdef-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="6fdef-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="6fdef-160">Bir satır düzenleyin ve ad alanlarının temizlemek deneyin.</span><span class="sxs-lookup"><span data-stu-id="6fdef-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="6fdef-161">Tıklatmadan önce bir alanı bırakın hemen alan hataları belirten bir yıldız işareti görünür **güncelleştirme**.</span><span class="sxs-lookup"><span data-stu-id="6fdef-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="6fdef-162">Tıkladığınızda **güncelleştirme**, belirttiğiniz hata iletisi metni sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6fdef-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="6fdef-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="6fdef-164">25 karakterden daha uzun ad girmeyi deneyin, tıklatın **güncelleştirme**, ve belirttiğiniz hata iletisi metni sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6fdef-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="6fdef-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="6fdef-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="6fdef-166">Bu biçimlendirmeyi ve doğrulama kuralları veri modelinin meta verilerindeki kurdu, kullandığınız sürece otomatik olarak bu alanlara değişikliğe izin verdiğinden ya da görüntüleyen her sayfada uygulanacak kurallar `DynamicControl` veya `DynamicField` kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="6fdef-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="6fdef-167">Bu programlama ve sınamayı daha kolay hale getirir, yedekli kod yazmak zorunda miktarını azaltır ve veri biçimlendirmeyi ve doğrulama bir uygulama genelinde tutarlı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6fdef-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="6fdef-168">Daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="6fdef-168">More Information</span></span>

<span data-ttu-id="6fdef-169">Bu öğretici Entity Framework ile çalışmaya başlama dizi sonlanır.</span><span class="sxs-lookup"><span data-stu-id="6fdef-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="6fdef-170">Entity Framework kullanmayı öğrenmenize yardımcı olacak daha fazla kaynak için devam [sonraki Entity Framework öğretici serideki ilk öğreticide](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) veya aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="6fdef-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="6fdef-171">Entity Framework ile ilgili SSS</span><span class="sxs-lookup"><span data-stu-id="6fdef-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="6fdef-172">Entity Framework ekip blogu</span><span class="sxs-lookup"><span data-stu-id="6fdef-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="6fdef-173">MSDN Kitaplığı'nda Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6fdef-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="6fdef-174">Entity Framework MSDN veri Geliştirici Merkezi</span><span class="sxs-lookup"><span data-stu-id="6fdef-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="6fdef-175">MSDN Kitaplığı'nda EntityDataSource Web sunucusu denetimine genel bakış</span><span class="sxs-lookup"><span data-stu-id="6fdef-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="6fdef-176">EntityDataSource denetim MSDN Kitaplığı'nda API Başvurusu</span><span class="sxs-lookup"><span data-stu-id="6fdef-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="6fdef-177">MSDN'de Entity Framework forumları</span><span class="sxs-lookup"><span data-stu-id="6fdef-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="6fdef-178">Julie Lerman'ın blogu</span><span class="sxs-lookup"><span data-stu-id="6fdef-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[<span data-ttu-id="6fdef-179">Önceki</span><span class="sxs-lookup"><span data-stu-id="6fdef-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
