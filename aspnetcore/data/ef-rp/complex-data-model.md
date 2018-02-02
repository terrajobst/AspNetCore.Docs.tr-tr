---
title: "Razor sayfalarının EF çekirdek - veri modeli - 8'in 5 ile"
author: rick-anderson
description: "Bu öğreticide daha fazla varlıkları ve ilişkileri ekleyebilir ve veri modelinin biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirebilirsiniz."
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 58bb773ba16314827da84909def05a8ef370479b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="db90a-103">Karmaşık veri model - EF çekirdek Razor sayfalarının öğretici (8'in 5) ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="db90a-103">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="db90a-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="db90a-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="db90a-105">Önceki öğreticileri üç varlıklarının oluşan bir temel veri modeli ile çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="db90a-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="db90a-106">Bu öğreticide:</span><span class="sxs-lookup"><span data-stu-id="db90a-106">In this tutorial:</span></span>

* <span data-ttu-id="db90a-107">Daha fazla varlıkları ve ilişkileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="db90a-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="db90a-108">Veri modeli biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="db90a-109">Tamamlanan veri modeli için sınıflar aşağıdaki çizimde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="db90a-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="db90a-111">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="db90a-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="db90a-112">Veri modeli öznitelikleri ile özelleştirme</span><span class="sxs-lookup"><span data-stu-id="db90a-112">Customize the data model with attributes</span></span>

<span data-ttu-id="db90a-113">Bu bölümde, veri modeli özniteliklerini kullanarak özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="db90a-114">Veri türü özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-114">The DataType attribute</span></span>

<span data-ttu-id="db90a-115">Öğrenci sayfaları şu anda kayıt tarihi süresini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="db90a-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="db90a-116">Genellikle, tarihi alanları, yalnızca tarih ve saati gösterir.</span><span class="sxs-lookup"><span data-stu-id="db90a-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="db90a-117">Güncelleştirme *Models/Student.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="db90a-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="db90a-118">[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) veritabanı geçerli bir tür daha fazla belirli bir veri türü özniteliği belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-118">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="db90a-119">Yalnızca tarih görüntülenmesi gerekir, bu durumda değil tarih ve saat.</span><span class="sxs-lookup"><span data-stu-id="db90a-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="db90a-120">[DataType numaralandırma](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) tarih, saat, PhoneNumber, para birimi, EmailAddress, vb. gibi birçok veri türleri için sağlar. `DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="db90a-120">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="db90a-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="db90a-121">For example:</span></span>

* <span data-ttu-id="db90a-122">`mailto:` Bağlantı için otomatik olarak oluşturulduğunda `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="db90a-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="db90a-123">Tarih Seçici için sağlanan `DataType.Date` çoğu tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="db90a-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="db90a-124">`DataType` Özniteliği yayar HTML 5 `data-` HTML 5 tarayıcılar tüketebilir (okunur veri tire) öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="db90a-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="db90a-125">`DataType` Öznitelikler yok doğrulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="db90a-126">`DataType.Date`Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="db90a-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="db90a-127">Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre tarih alanı görüntülenir [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="db90a-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="db90a-128">`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db90a-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="db90a-129">`ApplyFormatInEditMode` Ayarı, biçimlendirmeyi de düzenleme UI uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="db90a-130">Bazı alanlar kullanmamanız `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="db90a-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="db90a-131">Örneğin, para birimi simgesini genellikle bir düzenleme metin kutusunda görüntülenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="db90a-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="db90a-132">`DisplayFormat` Özniteliği kendisi tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="db90a-133">Bu genellikle kullanmak için iyi bir fikirdir `DataType` ile öznitelik `DisplayFormat` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="db90a-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="db90a-134">`DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir.</span><span class="sxs-lookup"><span data-stu-id="db90a-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="db90a-135">`DataType` Özniteliği kullanılamayan aşağıdaki yararları sağlar `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="db90a-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="db90a-136">Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db90a-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="db90a-137">Örneğin, bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, istemci-tarafı giriş doğrulaması vb. gösterir.</span><span class="sxs-lookup"><span data-stu-id="db90a-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="db90a-138">Varsayılan olarak, tarayıcı bölgesel ayarına göre doğru biçimde kullanarak verileri işler.</span><span class="sxs-lookup"><span data-stu-id="db90a-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="db90a-139">Daha fazla bilgi için bkz: [ \<Giriş > etiketi yardımcı belgelerine](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="db90a-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="db90a-140">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db90a-140">Run the app.</span></span> <span data-ttu-id="db90a-141">Öğrenciler dizin sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="db90a-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="db90a-142">Zamanlar artık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="db90a-142">Times are no longer displayed.</span></span> <span data-ttu-id="db90a-143">Kullanan her görünümü `Student` model zaman olmadan tarihi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="db90a-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="db90a-145">StringLength özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-145">The StringLength attribute</span></span>

<span data-ttu-id="db90a-146">Veri doğrulama kuralları ve doğrulama hata iletilerinin özniteliklerle belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="db90a-147">[StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği karakterden oluşan bir veri alanına izin verilen minimum ve maksimum uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-147">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="db90a-148">`StringLength` Özniteliği, istemci ve sunucu tarafı doğrulama da sağlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="db90a-149">En düşük değer veritabanı şemasını temel bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="db90a-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="db90a-150">Güncelleştirme `Student` aşağıdaki kodla model:</span><span class="sxs-lookup"><span data-stu-id="db90a-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="db90a-151">Önceki kod adları en fazla 50 karakter sınırlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="db90a-152">`StringLength` Özniteliği değil engelleyen bir kullanıcı için bir ad boşluk girerek.</span><span class="sxs-lookup"><span data-stu-id="db90a-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="db90a-153">[Yanıtta normal ifade](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği girişine kısıtlamaları uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db90a-153">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="db90a-154">Örneğin, aşağıdaki kod, büyük harf olması için ilk karakter ve alfabetik olarak geriye kalan karakterler gerektirir:</span><span class="sxs-lookup"><span data-stu-id="db90a-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="db90a-155">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="db90a-155">Run the app:</span></span>

* <span data-ttu-id="db90a-156">Öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="db90a-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="db90a-157">Seçin **Yeni Oluştur**ve en fazla 50 karakter uzunluğunda bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="db90a-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="db90a-158">Seçin **oluşturma**, istemci tarafı doğrulama hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="db90a-158">Select **Create**, client-side validation shows an error message.</span></span>

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="db90a-160">İçinde **SQL Server Nesne Gezgini** (SSOX) Öğrenci Tablo Tasarımcısı'nı çift tıklatarak açın **Öğrenci** tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![SSOX Öğrenciler tabloda geçişler önce](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="db90a-162">Önceki resimde için şemayı gösterilir `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="db90a-163">Ad alanlarını türüne sahip `nvarchar(MAX)` geçişler değil çalıştırıldıysa çünkü DB'de.</span><span class="sxs-lookup"><span data-stu-id="db90a-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="db90a-164">Daha sonra Bu öğreticide geçişler çalıştırdığınızda ad alanlarını hale `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="db90a-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="db90a-165">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-165">The Column attribute</span></span>

<span data-ttu-id="db90a-166">Öznitelik sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetleyebilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="db90a-167">Bu bölümde `Column` adını eşleştirmek için kullanılan öznitelik `FirstMidName` DB "FirstName" özelliğine.</span><span class="sxs-lookup"><span data-stu-id="db90a-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="db90a-168">DB oluşturulduğunda, model üzerinde özellik adlarını sütun adları için kullanılan (olmadığı dışında `Column` özniteliği kullanılır).</span><span class="sxs-lookup"><span data-stu-id="db90a-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="db90a-169">`Student` Modeli kullanır `FirstMidName` -ad alanı ikinci adı içeriyor olabilir çünkü alan.</span><span class="sxs-lookup"><span data-stu-id="db90a-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="db90a-170">Güncelleştirme *Student.cs* aşağıdaki vurgulanmış kodu dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="db90a-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="db90a-171">Yukarıdaki değişikliği ile `Student.FirstMidName` uygulamada eşlendiğini `FirstName` sütunu `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="db90a-172">Eklenmesi `Column` özniteliği değiştirir destekleyen model `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="db90a-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="db90a-173">Destekleyen model `SchoolContext` veritabanı artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="db90a-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="db90a-174">Geçişler uygulamadan önce uygulamayı çalıştırmak, şu özel durum oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="db90a-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="db90a-175">DB güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="db90a-175">To update the DB:</span></span>

* <span data-ttu-id="db90a-176">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db90a-176">Build the project.</span></span>
* <span data-ttu-id="db90a-177">Proje klasöründe bir komut penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="db90a-177">Open a command window in the project folder.</span></span> <span data-ttu-id="db90a-178">Yeni geçiş oluştur ve DB güncelleştirmek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="db90a-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="db90a-179">`dotnet ef migrations add ColumnFirstName` Komutu aşağıdaki uyarı iletisini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="db90a-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="db90a-180">Ad alanları artık olduğundan Uyarı oluşturulduğu 50 karakterle sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="db90a-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="db90a-181">Veritabanında bir adı 50'den fazla karakter olsaydı, 51 son karakter olarak kaybolur.</span><span class="sxs-lookup"><span data-stu-id="db90a-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="db90a-182">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="db90a-182">Test the app.</span></span>

<span data-ttu-id="db90a-183">Öğrenci tablo içinde SSOX açın:</span><span class="sxs-lookup"><span data-stu-id="db90a-183">Open the Student table in SSOX:</span></span>

![SSOX Öğrenciler tabloda geçişler sonra](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="db90a-185">Geçiş uygulanmadan adı sütun türü olan [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="db90a-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="db90a-186">Ad sütunlar şimdi: `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="db90a-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="db90a-187">Sütun adı değiştiğinden `FirstMidName` için `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="db90a-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="db90a-188">Aşağıdaki bölümde bazı aşamalarda uygulaması oluşturmaya derleyici hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db90a-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="db90a-189">Yönergeleri uygulamanızı oluşturmak ne zaman belirtin.</span><span class="sxs-lookup"><span data-stu-id="db90a-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="db90a-190">Öğrenci varlık güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="db90a-190">Student entity update</span></span>

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

<span data-ttu-id="db90a-192">Güncelleştirme *Models/Student.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="db90a-193">Gerekli özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-193">The Required attribute</span></span>

<span data-ttu-id="db90a-194">`Required` Öznitelik adı özellikleri gerekli alanlar yapar.</span><span class="sxs-lookup"><span data-stu-id="db90a-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="db90a-195">`Required` Öznitelik değer türleri gibi null türleri için gerekli değil (`DateTime`, `int`, `double`vb..).</span><span class="sxs-lookup"><span data-stu-id="db90a-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="db90a-196">Null olamaz türleri gerekli alanlar olarak otomatik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="db90a-197">`Required` Özniteliği yerine bir en az uzunluk parametresi `StringLength` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="db90a-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="db90a-198">Görüntüleme özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-198">The Display attribute</span></span>

<span data-ttu-id="db90a-199">`Display` Özniteliği belirtir. "Ad", "Son adı", "Tam adı" ve "Kayıt tarihi." Başlık metin kutuları olmalıdır</span><span class="sxs-lookup"><span data-stu-id="db90a-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="db90a-200">Örneğin "Lastname." sözcük bölme boşluk varsayılan başlıklar vardı</span><span class="sxs-lookup"><span data-stu-id="db90a-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="db90a-201">Hesaplanan FullName özelliği</span><span class="sxs-lookup"><span data-stu-id="db90a-201">The FullName calculated property</span></span>

<span data-ttu-id="db90a-202">`FullName`diğer iki özellik birleştirerek oluşturulan bir değer döndürür hesaplanan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="db90a-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="db90a-203">`FullName`, yalnızca bir get erişimcisine sahip ayarlanamıyor.</span><span class="sxs-lookup"><span data-stu-id="db90a-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="db90a-204">Hayır `FullName` sütun veritabanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db90a-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="db90a-205">Eğitmen varlık oluştur</span><span class="sxs-lookup"><span data-stu-id="db90a-205">Create the Instructor Entity</span></span>

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="db90a-207">Oluşturma *Models/Instructor.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="db90a-208">Çeşitli özelliklerin aynı olmasına dikkat edin `Student` ve `Instructor` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="db90a-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="db90a-209">Bu serideki sonraki uygulama devralma öğreticide artıklık ortadan kaldırmak için bu kodu bulunanad.</span><span class="sxs-lookup"><span data-stu-id="db90a-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="db90a-210">Birden çok öznitelik tek bir satırda olabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="db90a-211">`HireDate` Öznitelikleri yazılmaya gibi:</span><span class="sxs-lookup"><span data-stu-id="db90a-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="db90a-212">CourseAssignments ve OfficeAssignment Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="db90a-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="db90a-213">`CourseAssignments` Ve `OfficeAssignment` özelliklerdir Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="db90a-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="db90a-214">Bir eğitmen kurslar herhangi bir sayıda öğretmek, bu nedenle `CourseAssignments` koleksiyonu olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="db90a-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="db90a-215">Bir gezinme özelliği birden çok varlık barındırıyorsa:</span><span class="sxs-lookup"><span data-stu-id="db90a-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="db90a-216">Burada girişleri eklenmiş, silinmiş, güncelleştirilen ve bir liste türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db90a-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="db90a-217">Gezinti özelliği türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db90a-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="db90a-218">Varsa `ICollection<T>` belirtilirse, EF çekirdek oluşturur bir `HashSet<T>` varsayılan koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="db90a-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="db90a-219">`CourseAssignment` Varlık çok-çok ilişkilerde bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="db90a-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="db90a-220">Contoso University iş durumu bir eğitmen en çok bir office sağlayabilirsiniz kuralları.</span><span class="sxs-lookup"><span data-stu-id="db90a-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="db90a-221">`OfficeAssignment` Özelliği tek bir tutar `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="db90a-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="db90a-222">`OfficeAssignment`hiçbir office atanmışsa null şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="db90a-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="db90a-223">OfficeAssignment varlık oluştur</span><span class="sxs-lookup"><span data-stu-id="db90a-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="db90a-225">Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="db90a-226">Key özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-226">The Key attribute</span></span>

<span data-ttu-id="db90a-227">`[Key]` Öznitelik, özellik adı bir şey olduğunda bir özelliği (PK) birincil anahtarı olarak classnameID veya kimliği dışında tanımlamak için kullanılır</span><span class="sxs-lookup"><span data-stu-id="db90a-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="db90a-228">Arasında bir-sıfır-veya-bir ilişkisi olduğundan `Instructor` ve `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="db90a-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="db90a-229">Bir office atama yalnızca atandığı Eğitmen bağlantılı olarak zaten var.</span><span class="sxs-lookup"><span data-stu-id="db90a-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="db90a-230">`OfficeAssignment` PK etmektir Ayrıca, yabancı anahtar (FK) `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="db90a-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="db90a-231">EF çekirdek olamaz otomatik olarak tanıması `InstructorID` PK olarak `OfficeAssignment` nedeni:</span><span class="sxs-lookup"><span data-stu-id="db90a-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="db90a-232">`InstructorID`Kimliği veya classnameID adlandırma kuralını uyguladığınızdan değil.</span><span class="sxs-lookup"><span data-stu-id="db90a-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="db90a-233">Bu nedenle, `Key` tanımlamak için kullanılan öznitelik `InstructorID` PK olarak:</span><span class="sxs-lookup"><span data-stu-id="db90a-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="db90a-234">Sütunu için bir tanımlayıcı ilişkisi olduğundan varsayılan olmayan veritabanı-üretilmiş EF çekirdek anahtar değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="db90a-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="db90a-235">Eğitmen gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="db90a-235">The Instructor navigation property</span></span>

<span data-ttu-id="db90a-236">`OfficeAssignment` İçin gezinme özelliği `Instructor` varlıktır boş değer atanabilir olduğundan:</span><span class="sxs-lookup"><span data-stu-id="db90a-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="db90a-237">(Sınıfları boş değer atanabilir gibi) başvuru türleri.</span><span class="sxs-lookup"><span data-stu-id="db90a-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="db90a-238">Bir eğitmen office atama sahip olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="db90a-239">`OfficeAssignment` Varlık sahip atanamayan bir `Instructor` gezinti özelliği olduğundan:</span><span class="sxs-lookup"><span data-stu-id="db90a-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="db90a-240">`InstructorID`NULL olmayan olabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="db90a-241">Office atama bir eğitmen var olamaz.</span><span class="sxs-lookup"><span data-stu-id="db90a-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="db90a-242">Zaman bir `Instructor` varlık sahip ilgili `OfficeAssignment` varlık, her varlık, gezinti özelliğinin başka bir başvuru içeriyor.</span><span class="sxs-lookup"><span data-stu-id="db90a-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="db90a-243">`[Required]` Özniteliği uygulandığı `Instructor` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="db90a-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="db90a-244">Önceki kod ilgili Eğitmen olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="db90a-245">Önceki kod gerekli değildir çünkü `InstructorID` (aynı zamanda olan PK) yabancı anahtar null.</span><span class="sxs-lookup"><span data-stu-id="db90a-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="db90a-246">İndirmelere varlık değiştirme</span><span class="sxs-lookup"><span data-stu-id="db90a-246">Modify the Course Entity</span></span>

![İndirmelere varlık](complex-data-model/_static/course-entity.png)

<span data-ttu-id="db90a-248">Güncelleştirme *Models/Course.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="db90a-249">`Course` Varlık sahip bir yabancı anahtar (FK) özellik `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="db90a-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="db90a-250">`DepartmentID`işaret ilgili `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="db90a-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="db90a-251">`Course` Varlık sahip bir `Department` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="db90a-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="db90a-252">İlgili varlık için gezinme özelliği modele sahip olduğunda EF çekirdek FK özelliği için bir veri modeli gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="db90a-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="db90a-253">Gerekli nerede olursa olsun EF çekirdek FKs veritabanında otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db90a-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="db90a-254">EF çekirdek oluşturur [gölge özellikleri](https://docs.microsoft.com/ef/core/modeling/shadow-properties) otomatik olarak oluşturulan FKs için.</span><span class="sxs-lookup"><span data-stu-id="db90a-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="db90a-255">Veri modelinde FK sahip güncelleştirmeleri daha basit ve daha verimli hale getirebilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="db90a-256">Örneğin, bir model göz önünde bulundurun burada FK özelliği `DepartmentID` olan *değil* dahil.</span><span class="sxs-lookup"><span data-stu-id="db90a-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="db90a-257">Ne zaman bir indirmelere varlığı düzenlemek için getirilir:</span><span class="sxs-lookup"><span data-stu-id="db90a-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="db90a-258">`Department` Varlıktır açıkça yüklü değilse null.</span><span class="sxs-lookup"><span data-stu-id="db90a-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="db90a-259">İndirmelere varlık güncelleştirileceğini `Department` varlık gerekir ilk getirildi.</span><span class="sxs-lookup"><span data-stu-id="db90a-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="db90a-260">Zaman FK özelliği `DepartmentID` bulunan veri modelinde getirmek için gerek yoktur `Department` bir güncelleştirme öncesi varlık.</span><span class="sxs-lookup"><span data-stu-id="db90a-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="db90a-261">DatabaseGenerated özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="db90a-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` PK uygulama tarafından sağlanan yerine veritabanı tarafından oluşturulan özniteliği belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="db90a-263">Varsayılan olarak, PK değerleri DB tarafından oluşturulan EF çekirdek varsayar.</span><span class="sxs-lookup"><span data-stu-id="db90a-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="db90a-264">DB oluşturulan PK değerleri olan genellikle en iyi yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="db90a-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="db90a-265">İçin `Course` varlıklar, kullanıcı yinelenir belirtir</span><span class="sxs-lookup"><span data-stu-id="db90a-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="db90a-266">Örneğin, bir indirmelere sayı matematik departmanı için 1000 serisi, İngilizce departmanı 2000 serisi gibi.</span><span class="sxs-lookup"><span data-stu-id="db90a-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="db90a-267">`DatabaseGenerated` Özniteliği de varsayılan değerlerini oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="db90a-268">Örneğin, DB otomatik olarak bir satır oluşturulurken veya güncelleştirilirken tarihi kaydetmek için bir tarih alanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db90a-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="db90a-269">Daha fazla bilgi için bkz: [oluşturulan Özellikler](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="db90a-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="db90a-270">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="db90a-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="db90a-271">Yabancı anahtar (FK) özellikler ve gezinti özellikleri `Course` varlık yansıtacak aşağıdaki ilişkileri:</span><span class="sxs-lookup"><span data-stu-id="db90a-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="db90a-272">Bu yüzden bir indirmelere bir bölüme atanan bir `DepartmentID` FK ve `Department` gezinti özelliği.</span><span class="sxs-lookup"><span data-stu-id="db90a-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="db90a-273">Bir indirmelere içinde kayıtlı Öğrenciler herhangi bir sayıda olabilir böylece `Enrollments` gezinti özelliği olduğundan koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="db90a-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="db90a-274">Bir indirmelere birden çok Eğitmen tarafından öğrettin böylece `CourseAssignments` gezinti özelliği olduğundan koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="db90a-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="db90a-275">`CourseAssignment`açıklanan [daha sonra](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="db90a-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="db90a-276">Departman varlık oluştur</span><span class="sxs-lookup"><span data-stu-id="db90a-276">Create the Department entity</span></span>

![Departman varlık](complex-data-model/_static/department-entity.png)

<span data-ttu-id="db90a-278">Oluşturma *Models/Department.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="db90a-279">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="db90a-279">The Column attribute</span></span>

<span data-ttu-id="db90a-280">Daha önce `Column` özniteliği sütun adı eşlemesi değiştirmek için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="db90a-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="db90a-281">Kodunda `Department` varlık, `Column` özniteliği SQL veri türü eşlemesi değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db90a-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="db90a-282">`Budget` Sütun DB'de SQL Server para türü kullanılarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="db90a-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="db90a-283">Sütun eşlemesi genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="db90a-283">Column mapping is generally not required.</span></span> <span data-ttu-id="db90a-284">EF çekirdek genellikle özelliği için CLR türüne göre uygun SQL Server veri türünü seçer.</span><span class="sxs-lookup"><span data-stu-id="db90a-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="db90a-285">CLR `decimal` yazın eşlemeleri SQL Server'a `decimal` türü.</span><span class="sxs-lookup"><span data-stu-id="db90a-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="db90a-286">`Budget`para birimi, ve para veri türü para birimi için daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="db90a-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="db90a-287">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="db90a-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="db90a-288">FK ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="db90a-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="db90a-289">Bir bölüm olabilir veya bir yönetici olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="db90a-290">Bir yönetici, her zaman bir eğitmen olur.</span><span class="sxs-lookup"><span data-stu-id="db90a-290">An administrator is always an instructor.</span></span> <span data-ttu-id="db90a-291">Bu nedenle `InstructorID` FK olarak özellik eklenmiştir `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="db90a-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="db90a-292">Gezinti özelliği adlı `Administrator` ancak tutan bir `Instructor` varlık:</span><span class="sxs-lookup"><span data-stu-id="db90a-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="db90a-293">Özelliğin null atanabilir olup önceki kodda soru işareti (?) belirtir.</span><span class="sxs-lookup"><span data-stu-id="db90a-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="db90a-294">Bu yüzden kurslar gezinti özelliği bir bölüm birçok kurslar sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="db90a-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="db90a-295">Not: kurala göre art arda silme çok-çok ilişkileri ve null FKs için EF çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="db90a-296">Art arda delete döngüsel cascade delete kurallarında neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="db90a-297">Döngüsel art arda silme kuralları nedenler özel bir durum geçiş eklendiğinde.</span><span class="sxs-lookup"><span data-stu-id="db90a-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="db90a-298">Örneğin, varsa `Department.InstructorID` özelliği değildi tanımlı olarak boş değer atanabilir:</span><span class="sxs-lookup"><span data-stu-id="db90a-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="db90a-299">EF çekirdek departman silindiğinde Eğitmen silmek için bir cascade delete kuralı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="db90a-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="db90a-300">Departman silindiğinde Eğitmen silme amaçlanan bir davranış değildir.</span><span class="sxs-lookup"><span data-stu-id="db90a-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="db90a-301">İş kuralları gerekirse `InstructorID` özelliği null, aşağıdaki fluent API deyimini kullanın:</span><span class="sxs-lookup"><span data-stu-id="db90a-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="db90a-302">Önceki kod art arda silme departmanı Eğitmen ilişkiyi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="db90a-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="db90a-303">Güncelleştirme kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="db90a-303">Update the Enrollment entity</span></span>

<span data-ttu-id="db90a-304">Bir kaydı bir öğrenci tarafından gerçekleştirilen bir indirmelere içindir.</span><span class="sxs-lookup"><span data-stu-id="db90a-304">An enrollment record is for a one course taken by one student.</span></span>

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="db90a-306">Güncelleştirme *Models/Enrollment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="db90a-307">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="db90a-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="db90a-308">FK özellikler ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="db90a-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="db90a-309">Bu yüzden bir kaydı bir yol için kullanmamaktır bir `CourseID` FK özelliği ve `Course` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="db90a-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="db90a-310">Bu yüzden bir kaydı bir öğrenci için olan bir `StudentID` FK özelliği ve `Student` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="db90a-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="db90a-311">Çok-çok ilişkileri</span><span class="sxs-lookup"><span data-stu-id="db90a-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="db90a-312">Arasında çok-çok ilişkisi olduğundan `Student` ve `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="db90a-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="db90a-313">`Enrollment` Varlık işlevleri çoktan bire çok birleşme tablo olarak *yükü ile* veritabanındaki.</span><span class="sxs-lookup"><span data-stu-id="db90a-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="db90a-314">"Yükünü" anlamına gelir `Enrollment` tablo FKs yanı sıra ek verileri birleştirilmiş tablolar için içerir (Bu durumda, PK ve `Grade`).</span><span class="sxs-lookup"><span data-stu-id="db90a-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="db90a-315">Aşağıdaki çizimde, bu tür bir varlık şemada nasıl göründüğünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="db90a-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="db90a-316">(Bu diyagram için EF EF güç araçları kullanılarak oluşturulan 6.x.</span><span class="sxs-lookup"><span data-stu-id="db90a-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="db90a-317">Diyagram oluşturma öğreticinin bir parçası değil.)</span><span class="sxs-lookup"><span data-stu-id="db90a-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Öğrenci indirmelere çoklu ilişki çok](complex-data-model/_static/student-course.png)

<span data-ttu-id="db90a-319">Her ilişki ucu ve bir yıldız işareti (\*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.</span><span class="sxs-lookup"><span data-stu-id="db90a-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="db90a-320">Varsa `Enrollment` tablosunu kaydetmedi düzeyde bilgi dahil etmek, yalnızca iki FKs içermesi gerekir (`CourseID` ve `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="db90a-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="db90a-321">Çoktan bire çok birleşme tablo yükü olmadan bazen saf birleşim tablosundan (PJT) adı verilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="db90a-322">`Instructor` Ve `Course` varlığın saf birleştirme tablosunu kullanarak bir çok-çok ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="db90a-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="db90a-323">Not: EF 6.x destekler örtük birleştirme tablolarını çok-çok ilişkileri ancak EF çekirdek değildir.</span><span class="sxs-lookup"><span data-stu-id="db90a-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="db90a-324">Daha fazla bilgi için bkz: [çok-çok ilişkileri EF çekirdek 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="db90a-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="db90a-325">CourseAssignment varlık</span><span class="sxs-lookup"><span data-stu-id="db90a-325">The CourseAssignment entity</span></span>

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="db90a-327">Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="db90a-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="db90a-328">Eğitmen kurslar</span><span class="sxs-lookup"><span data-stu-id="db90a-328">Instructor-to-Courses</span></span>

![Eğitmen kurslar m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="db90a-330">Eğitmen kurslar çok-çok ilişkisi:</span><span class="sxs-lookup"><span data-stu-id="db90a-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="db90a-331">Bir varlık kümesi tarafından temsil edilebilir bir birleşim tablo gerektirir.</span><span class="sxs-lookup"><span data-stu-id="db90a-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="db90a-332">Saf birleşim tablosundan (tablo yükü olmadan) olur.</span><span class="sxs-lookup"><span data-stu-id="db90a-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="db90a-333">Bir birleştirme varlık adı yaygındır `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="db90a-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="db90a-334">Örneğin, bu deseni kullanarak Eğitmen kurslar birleştirme tablodur `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="db90a-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="db90a-335">Ancak, ilişkiyi tanımlayan bir ad kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="db90a-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="db90a-336">Veri modelleri basit başlatın ve büyütün.</span><span class="sxs-lookup"><span data-stu-id="db90a-336">Data models start out simple and grow.</span></span> <span data-ttu-id="db90a-337">Hayır yükü birleşimler (PJTs) yükü dahil etmek için sık geliştirin.</span><span class="sxs-lookup"><span data-stu-id="db90a-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="db90a-338">Açıklayıcı varlık adı ile başlatarak adı birleşim tablosundan değiştiğinde değiştirmek zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="db90a-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="db90a-339">Birleşim varlık ideal olarak, iş etki alanında kendi doğal (büyük olasılıkla tek sözcüklük) adına sahip.</span><span class="sxs-lookup"><span data-stu-id="db90a-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="db90a-340">Örneğin, Books ve müşteriler derecelendirmeleri adlı bir birleşim varlık ile ilişkilendirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="db90a-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="db90a-341">Eğitmen kurslar çoktan çoğa ilişki için `CourseAssignment` üzerinden tercih edilir `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="db90a-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="db90a-342">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="db90a-342">Composite key</span></span>

<span data-ttu-id="db90a-343">FKs boş değer atanabilir değil.</span><span class="sxs-lookup"><span data-stu-id="db90a-343">FKs are not nullable.</span></span> <span data-ttu-id="db90a-344">İçinde iki FKs `CourseAssignment` (`InstructorID` ve `CourseID`) birlikte her satır benzersizce `CourseAssignment` tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="db90a-345">`CourseAssignment`ayrılmış yinelenir gerektirmez</span><span class="sxs-lookup"><span data-stu-id="db90a-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="db90a-346">`InstructorID` Ve `CourseID` özellikleri bileşik yinelenir işlevi</span><span class="sxs-lookup"><span data-stu-id="db90a-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="db90a-347">EF çekirdek için bileşik BA belirtmek için tek yolu *fluent API*.</span><span class="sxs-lookup"><span data-stu-id="db90a-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="db90a-348">Sonraki bölümde bileşik yinelenir yapılandırmak nasıl gösterir</span><span class="sxs-lookup"><span data-stu-id="db90a-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="db90a-349">Bileşik anahtarın sağlar:</span><span class="sxs-lookup"><span data-stu-id="db90a-349">The composite key ensures:</span></span>

* <span data-ttu-id="db90a-350">Birden çok satır için bir indirmelere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="db90a-351">Birden çok satır için bir eğitmen izin verilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="db90a-352">Birden çok satır aynı eğitmen ve indirmelere için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="db90a-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="db90a-353">`Enrollment` Birleştirme varlık çoğaltmaları bu tür olası şekilde kendi PK tanımlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="db90a-354">Bu tür çoğaltmaları önlemek için:</span><span class="sxs-lookup"><span data-stu-id="db90a-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="db90a-355">FK alanları benzersiz bir dizin ekleyin veya</span><span class="sxs-lookup"><span data-stu-id="db90a-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="db90a-356">Yapılandırma `Enrollment` benzer birincil bileşik anahtar ile `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db90a-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="db90a-357">Daha fazla bilgi için bkz: [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="db90a-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="db90a-358">Güncelleştirme DB bağlamı</span><span class="sxs-lookup"><span data-stu-id="db90a-358">Update the DB context</span></span>

<span data-ttu-id="db90a-359">Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="db90a-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="db90a-360">Önceki kod yeni varlıklar ekler ve yapılandırır `CourseAssignment` varlığın bileşik yinelenir</span><span class="sxs-lookup"><span data-stu-id="db90a-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="db90a-361">Öznitelikleri Fluent API alternatifi</span><span class="sxs-lookup"><span data-stu-id="db90a-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="db90a-362">`OnModelCreating` Önceki içinde yöntemi kod *fluent API* EF çekirdek davranışı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="db90a-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="db90a-363">API, genellikle bir dizi yöntem çağrısı tek bir deyimde birleştirerek stringing tarafından kullanılmakta olduğu için "fluent" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="db90a-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="db90a-364">[Koddan](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) fluent API örneğidir:</span><span class="sxs-lookup"><span data-stu-id="db90a-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="db90a-365">Bu öğreticide, fluent API özniteliklerle yapılamayacak DB eşlemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db90a-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="db90a-366">Bununla birlikte, fluent API biçimlendirmeyi, doğrulama ve öznitelikleri ile yapılabilir eşleme kurallarını çoğu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db90a-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="db90a-367">Gibi bazı öznitelikler `MinimumLength` fluent API'si ile uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="db90a-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="db90a-368">`MinimumLength`Şema değiştirmez yalnızca bir minimum uzunluğu doğrulama kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="db90a-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="db90a-369">Bazı geliştiriciler özel olarak bunlar kendi sınıflar "temiz." tutabilirsiniz böylece fluent API kullanmayı tercih eder</span><span class="sxs-lookup"><span data-stu-id="db90a-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="db90a-370">Öznitelikler ve fluent API karışabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="db90a-371">(Bileşik PK belirtme) fluent API'si ile yalnızca yapılabilir bazı yapılandırmaları vardır.</span><span class="sxs-lookup"><span data-stu-id="db90a-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="db90a-372">Yalnızca özniteliklerle yapılabilir bazı yapılandırmalar vardır (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="db90a-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="db90a-373">Fluent API veya öznitelikleri kullanarak için önerilen yöntem:</span><span class="sxs-lookup"><span data-stu-id="db90a-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="db90a-374">Bu iki yaklaşımlardan birini seçin.</span><span class="sxs-lookup"><span data-stu-id="db90a-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="db90a-375">Tutarlı bir şekilde olabildiğince seçilen yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="db90a-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="db90a-376">Bu konuda kullanılan öznitelikler bazıları öğretici için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db90a-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="db90a-377">Yalnızca doğrulama (örneğin, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="db90a-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="db90a-378">EF Çekirdek yapılandırmasını yalnızca (örneğin, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="db90a-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="db90a-379">Doğrulama ve EF Çekirdek yapılandırmasını (örneğin, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="db90a-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="db90a-380">Öznitelikler fluent API karşılaştırması hakkında daha fazla bilgi için bkz: [yapılandırma yöntemlerini](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="db90a-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="db90a-381">Varlık diyagram gösteren ilişkileri</span><span class="sxs-lookup"><span data-stu-id="db90a-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="db90a-382">Aşağıdaki çizimde EF güç araçları için tamamlanan Okul modeli oluşturma diyagramda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="db90a-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="db90a-384">Önceki diyagramda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="db90a-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="db90a-385">Birden çok-çok ilişkisi satıra (1 \*).</span><span class="sxs-lookup"><span data-stu-id="db90a-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="db90a-386">Bir-sıfır-veya-bir ilişkisi satırın (1 için 0.. 1 çokluğa) arasında `Instructor` ve `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="db90a-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="db90a-387">Sıfır-veya-bir-çok ilişkisi satır (0.. 1 çokluğa için \*) arasında `Instructor` ve `Department` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="db90a-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="db90a-388">Çekirdek Test verilerle DB</span><span class="sxs-lookup"><span data-stu-id="db90a-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="db90a-389">Kodda güncelleştirme *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="db90a-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="db90a-390">Önceki kod yeni varlıklar için çekirdek verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="db90a-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="db90a-391">Bu kod çoğunu yeni varlık nesnesi oluşturur ve örnek verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="db90a-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="db90a-392">Örnek verileri test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db90a-392">The sample data is used for testing.</span></span> <span data-ttu-id="db90a-393">Yukarıdaki kod aşağıdaki çok-çok ilişkileri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="db90a-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="db90a-394">Not: [EF çekirdek 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) destekleyecek [verileri dengeli](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="db90a-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="db90a-395">Bir geçiş ekleme</span><span class="sxs-lookup"><span data-stu-id="db90a-395">Add a migration</span></span>

<span data-ttu-id="db90a-396">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db90a-396">Build the project.</span></span> <span data-ttu-id="db90a-397">Proje klasöründeki bir komut penceresi açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="db90a-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="db90a-398">Yukarıdaki komut, olası veri kaybı hakkında bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="db90a-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="db90a-399">Varsa `database update` komutu çalıştırıldığında, aşağıdaki hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="db90a-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="db90a-400">Var olan verilerle geçişler çalıştırdığınızda, mevcut verilerle memnun değil FK kısıtlamaları olabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="db90a-401">FK bir kısıtlama ihlali olduklarından Bu öğretici için yeni bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db90a-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="db90a-402">Bkz: [eski verilerle yabancı anahtar kısıtlamaları düzelttikten](#fk) geçerli DB'de FK ihlalleri gidermeye yönelik yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="db90a-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="db90a-403">Bağlantı dizesini değiştirin ve DB güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="db90a-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="db90a-404">Güncelleştirilmiş kod `DbInitializer` yeni varlıklar için çekirdek veri ekler.</span><span class="sxs-lookup"><span data-stu-id="db90a-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="db90a-405">EF yeni bir boş veritabanı oluşturmak için çekirdek zorlamak için:</span><span class="sxs-lookup"><span data-stu-id="db90a-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="db90a-406">DB bağlantı dizesi adı değiştirmek *appsettings.json* ContosoUniversity3 için.</span><span class="sxs-lookup"><span data-stu-id="db90a-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="db90a-407">Yeni bir ad bilgisayarda kullanılmamış bir adı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="db90a-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="db90a-408">Alternatif olarak, DB kullanarak silin:</span><span class="sxs-lookup"><span data-stu-id="db90a-408">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="db90a-409">**SQL Server Object Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="db90a-409">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="db90a-410">`database drop` CLI komutu:</span><span class="sxs-lookup"><span data-stu-id="db90a-410">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="db90a-411">Çalıştırma `database update` komut penceresinde:</span><span class="sxs-lookup"><span data-stu-id="db90a-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="db90a-412">Yukarıdaki komut, tüm geçişler çalışır.</span><span class="sxs-lookup"><span data-stu-id="db90a-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="db90a-413">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db90a-413">Run the app.</span></span> <span data-ttu-id="db90a-414">Uygulamanın çalıştığı çalışan `DbInitializer.Initialize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db90a-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="db90a-415">`DbInitializer.Initialize` Yeni DB doldurur.</span><span class="sxs-lookup"><span data-stu-id="db90a-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="db90a-416">DB içinde SSOX açın:</span><span class="sxs-lookup"><span data-stu-id="db90a-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="db90a-417">Genişletme **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="db90a-417">Expand the **Tables** node.</span></span> <span data-ttu-id="db90a-418">Oluşturulan tablolar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="db90a-418">The created tables are displayed.</span></span>
* <span data-ttu-id="db90a-419">SSOX önceden açılmışsa tıklatın **yenileme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="db90a-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="db90a-421">İncelemek **CourseAssignment** tablosu:</span><span class="sxs-lookup"><span data-stu-id="db90a-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="db90a-422">Sağ **CourseAssignment** tablo ve seçin **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="db90a-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="db90a-423">Doğrulama **CourseAssignment** tablo verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="db90a-423">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="db90a-425">Yabancı anahtar kısıtlamaları eski verilerle çözme</span><span class="sxs-lookup"><span data-stu-id="db90a-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="db90a-426">Bu bölümde isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="db90a-426">This section is optional.</span></span>

<span data-ttu-id="db90a-427">Var olan verilerle geçişler çalıştırdığınızda, mevcut verilerle memnun değil FK kısıtlamaları olabilir.</span><span class="sxs-lookup"><span data-stu-id="db90a-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="db90a-428">Var olan verileri geçirmek için üretim verileri ile adımları alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="db90a-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="db90a-429">Bu bölümde FK sabiti ihlallerini düzeltmek örneğidir.</span><span class="sxs-lookup"><span data-stu-id="db90a-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="db90a-430">Bu kod bir yedek olmadan değişiklik yok.</span><span class="sxs-lookup"><span data-stu-id="db90a-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="db90a-431">Önceki bölümde tamamladıysanız ve veritabanı güncelleştirilmiş Bu kod değişiklikleri yapmayın.</span><span class="sxs-lookup"><span data-stu-id="db90a-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="db90a-432">*{Timestamp}_ComplexDataModel.cs* dosyası aşağıdaki kod içerir:</span><span class="sxs-lookup"><span data-stu-id="db90a-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="db90a-433">Önceki kod atanamayan bir ekler `DepartmentID` için FK `Course` tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="db90a-434">Önceki öğretici DB'den satırları içeren `Course`, bu tablo geçişler tarafından güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="db90a-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="db90a-435">Yapmak için `ComplexDataModel` var olan verilerle geçiş çalışma:</span><span class="sxs-lookup"><span data-stu-id="db90a-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="db90a-436">Yeni bir sütun vermek için kodu değiştirin (`DepartmentID`) varsayılan bir değer.</span><span class="sxs-lookup"><span data-stu-id="db90a-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="db90a-437">Varsayılan departman olarak davranacak şekilde "Temp" adlı sahte bir bölüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db90a-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="db90a-438">Yabancı anahtar kısıtlamaları Düzelt</span><span class="sxs-lookup"><span data-stu-id="db90a-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="db90a-439">Güncelleştirme `ComplexDataModel` sınıfları `Up` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db90a-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="db90a-440">Açık *{timestamp}_ComplexDataModel.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="db90a-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="db90a-441">Açıklama ekler kod satırı çıkışı `DepartmentID` sütuna `Course` tablo.</span><span class="sxs-lookup"><span data-stu-id="db90a-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="db90a-442">Aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="db90a-442">Add the following highlighted code.</span></span> <span data-ttu-id="db90a-443">Sonra yeni kod gider `.CreateTable( name: "Department"` engelle:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="db90a-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="db90a-444">Varolan önceki değişikliklerle `Course` satır ilgili sonra "Temp" departman `ComplexDataModel` `Up` yöntemi çalışır.</span><span class="sxs-lookup"><span data-stu-id="db90a-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="db90a-445">Bir üretim uygulaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="db90a-445">A production app would:</span></span>

* <span data-ttu-id="db90a-446">Kod veya eklemek için komut dosyaları dahil `Department` satırlar ve ilişkili `Course` yeni satır `Department` satır.</span><span class="sxs-lookup"><span data-stu-id="db90a-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="db90a-447">"Temp" departman veya için varsayılan değer kullanmamanız `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="db90a-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="db90a-448">Sonraki öğretici ilgili veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="db90a-448">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="db90a-449">[Önceki](xref:data/ef-rp/migrations)
[sonraki](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="db90a-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
