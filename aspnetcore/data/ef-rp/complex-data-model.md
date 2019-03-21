---
title: ASP.NET core'da - veri modeli - 8'in 5 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyin ve veri modelini, doğrulama, biçimlendirme ve eşleme kuralları belirterek özelleştirin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 311f72699b6291996a43d56247bd3d2bfab596e6
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320254"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="84518-103">ASP.NET core'da - veri modeli - 8'in 5 EF çekirdekli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="84518-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="84518-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84518-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="84518-105">Önceki öğreticilerde, üç varlıklarının oluşturulma bir temel veri modeli ile çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="84518-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="84518-106">Bu öğreticide:</span><span class="sxs-lookup"><span data-stu-id="84518-106">In this tutorial:</span></span>

* <span data-ttu-id="84518-107">Daha fazla varlıklar ve ilişkiler eklenir.</span><span class="sxs-lookup"><span data-stu-id="84518-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="84518-108">Veri modeli, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84518-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="84518-109">Varlık sınıflarının tamamlanmış veri modeli için aşağıdaki çizimde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="84518-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="84518-111">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="84518-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="84518-112">Veri modeli öznitelikleri ile özelleştirme</span><span class="sxs-lookup"><span data-stu-id="84518-112">Customize the data model with attributes</span></span>

<span data-ttu-id="84518-113">Bu bölümde, veri modeli, öznitelikleri kullanarak özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84518-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="84518-114">Veri türü özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-114">The DataType attribute</span></span>

<span data-ttu-id="84518-115">Öğrenci sayfaları, şu anda kayıt tarihi zamanı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="84518-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="84518-116">Genellikle, yalnızca tarih ve saat tarih alanları gösterin.</span><span class="sxs-lookup"><span data-stu-id="84518-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="84518-117">Güncelleştirme *Models/Student.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="84518-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="84518-118">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) özniteliği veritabanı iç türünden daha belirli bir veri türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="84518-119">Yalnızca tarih görüntülenmesi gerekir, bu durumda değil tarih ve saati.</span><span class="sxs-lookup"><span data-stu-id="84518-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="84518-120">[Veri türü sabit listesi](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) tarih, saat, telefon numarası, para birimi, EmailAddress, vb. gibi çok sayıda veri türleri için sağlar. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="84518-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="84518-121">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="84518-121">For example:</span></span>

* <span data-ttu-id="84518-122">`mailto:` Bağlantısını için otomatik olarak oluşturulur `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="84518-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="84518-123">Tarih Seçici için sağlanan `DataType.Date` çoğu tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="84518-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="84518-124">`DataType` Öznitelik HTML 5 yayan `data-` HTML 5 tarayıcılar kullanma (okunur veri dash) öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="84518-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="84518-125">`DataType` Öznitelikleri doğrulama sağlamadığınızdan.</span><span class="sxs-lookup"><span data-stu-id="84518-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="84518-126">`DataType.Date` Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="84518-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="84518-127">Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre tarih alanı görüntülenir [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="84518-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="84518-128">`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:</span><span class="sxs-lookup"><span data-stu-id="84518-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="84518-129">`ApplyFormatInEditMode` Ayar biçimlendirme de düzenleme UI uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="84518-130">Bazı alanlar kullanmamalısınız `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="84518-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="84518-131">Örneğin, para birimi simgesi genellikle bir düzenleme kutusuna görüntülenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="84518-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="84518-132">`DisplayFormat` Özniteliği kendisi tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="84518-133">Genel olarak kullanmak iyi bir fikir olduğunu `DataType` özniteliğini `DisplayFormat` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="84518-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="84518-134">`DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez.</span><span class="sxs-lookup"><span data-stu-id="84518-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="84518-135">`DataType` Özniteliği de kullanılamayan aşağıdaki faydaları sağlar `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="84518-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="84518-136">Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84518-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="84518-137">Örneğin, bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, istemci tarafı giriş doğrulaması vb. gösterir.</span><span class="sxs-lookup"><span data-stu-id="84518-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="84518-138">Varsayılan olarak, tarayıcının yerel ayarları temel alarak doğru biçimde kullanarak verileri işler.</span><span class="sxs-lookup"><span data-stu-id="84518-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="84518-139">Daha fazla bilgi için [ \<Giriş > etiketi Yardımcısı belgeleri](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="84518-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="84518-140">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84518-140">Run the app.</span></span> <span data-ttu-id="84518-141">Öğrenciler dizin sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="84518-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="84518-142">Süreleri artık gösterilir.</span><span class="sxs-lookup"><span data-stu-id="84518-142">Times are no longer displayed.</span></span> <span data-ttu-id="84518-143">Kullanan her görünüm `Student` model zaman olmadan tarihi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="84518-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="84518-145">StringLength özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-145">The StringLength attribute</span></span>

<span data-ttu-id="84518-146">Veri doğrulama kuralları ve doğrulama hata iletilerinin özniteliklerle belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="84518-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="84518-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği minimum ve maksimum veri alanında izin verilen karakter uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="84518-148">`StringLength` Özniteliği, istemci ve sunucu tarafı doğrulama da sağlar.</span><span class="sxs-lookup"><span data-stu-id="84518-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="84518-149">En düşük değer veritabanı şema üzerinde bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="84518-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="84518-150">Güncelleştirme `Student` model aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="84518-151">Yukarıdaki kod, adları en fazla 50 karakter sınırlar.</span><span class="sxs-lookup"><span data-stu-id="84518-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="84518-152">`StringLength` Özniteliği değil önlemek bir kullanıcı için bir ad boşluk girmesini.</span><span class="sxs-lookup"><span data-stu-id="84518-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="84518-153">[Yanıtta normal ifade](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği giriş kısıtlamaları uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84518-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="84518-154">Örneğin, aşağıdaki kod, ilk karakterin büyük harf olacak ve alfabetik olarak kalan karakterler gerektirir:</span><span class="sxs-lookup"><span data-stu-id="84518-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="84518-155">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="84518-155">Run the app:</span></span>

* <span data-ttu-id="84518-156">Öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="84518-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="84518-157">Seçin **Yeni Oluştur**ve en fazla 50 karakter uzunluğunda bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="84518-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="84518-158">Seçin **Oluştur**, istemci tarafı doğrulama hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="84518-158">Select **Create**, client-side validation shows an error message.</span></span>

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="84518-160">İçinde **SQL Server Nesne Gezgini** (SSOX) Öğrenci Tablo Tasarımcısı çift tıklayarak açın **Öğrenci** tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Öğrenciler SSOX tablosunda geçişleri önce](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="84518-162">Bir önceki resimde için şema gösterir `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="84518-163">Ad alanlarını türüne sahip `nvarchar(MAX)` geçişler değil çalıştırıldıysa çünkü DB'de.</span><span class="sxs-lookup"><span data-stu-id="84518-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="84518-164">Geçişleri daha sonra Bu öğreticide çalıştırdığınızda, ad alanlarını haline `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="84518-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="84518-165">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-165">The Column attribute</span></span>

<span data-ttu-id="84518-166">Öznitelikler, sınıfları ve özellikleri veritabanına nasıl eşleştiğini kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84518-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="84518-167">Bu bölümde, `Column` eşlemek için kullanılan öznitelik `FirstMidName` DB'de "FirstName" özelliği.</span><span class="sxs-lookup"><span data-stu-id="84518-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="84518-168">Bir veritabanı oluşturulduğunda, model üzerinde özellik adlarını sütun adları için kullanılır (olmadığı dışında `Column` özniteliği kullanılır).</span><span class="sxs-lookup"><span data-stu-id="84518-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="84518-169">`Student` Modeli kullanan `FirstMidName` -ad alanı da ikinci adı içeriyor olabileceğinden alan.</span><span class="sxs-lookup"><span data-stu-id="84518-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="84518-170">Güncelleştirme *Student.cs* aşağıdaki vurgulanmış kodu dosyası:</span><span class="sxs-lookup"><span data-stu-id="84518-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="84518-171">Önceki değişiklikle `Student.FirstMidName` uygulamada eşlendiğini `FirstName` sütununun `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="84518-172">Ek `Column` özniteliği değişiklikleri model yedekleme `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="84518-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="84518-173">Model yedekleme `SchoolContext` veritabanı artık eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="84518-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="84518-174">Geçişleri uygulamadan önce uygulamayı çalıştırmak, şu özel durum oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="84518-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="84518-175">DB güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="84518-175">To update the DB:</span></span>

* <span data-ttu-id="84518-176">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="84518-176">Build the project.</span></span>
* <span data-ttu-id="84518-177">Proje klasöründe bir komut penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="84518-177">Open a command window in the project folder.</span></span> <span data-ttu-id="84518-178">DB yeni bir geçiş oluşturmak için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="84518-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84518-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84518-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84518-180">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="84518-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="84518-181">`migrations add ColumnFirstName` Komutu aşağıdaki uyarı iletisi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="84518-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="84518-182">Ad alanları artık olduğundan uyarısı oluşturulur 50 karakterle sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="84518-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="84518-183">Bir DB adı 50 karakterden uzun olsaydı, son karakter için 51 kaybolur.</span><span class="sxs-lookup"><span data-stu-id="84518-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="84518-184">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="84518-184">Test the app.</span></span>

<span data-ttu-id="84518-185">Öğrenci tablosu içinde SSOX açın:</span><span class="sxs-lookup"><span data-stu-id="84518-185">Open the Student table in SSOX:</span></span>

![Geçiş sonrasında SSOX Öğrenciler tabloda](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="84518-187">Geçiş uygulanmadan önce ad sütunu türü olan [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="84518-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="84518-188">Ad sütunu sunulmuştur `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="84518-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="84518-189">Sütun adı değiştiğinde `FirstMidName` için `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="84518-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="84518-190">Aşağıdaki bölümde bazı aşamalarında bir uygulama oluşturmak, derleyici hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="84518-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="84518-191">Uygulama derleme zamanı yönergeleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="84518-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="84518-192">Öğrenci varlık güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="84518-192">Student entity update</span></span>

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

<span data-ttu-id="84518-194">Güncelleştirme *Models/Student.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="84518-195">Gerekli öznitelik</span><span class="sxs-lookup"><span data-stu-id="84518-195">The Required attribute</span></span>

<span data-ttu-id="84518-196">`Required` Öznitelik adı özellikleri gerekli alanları yapar.</span><span class="sxs-lookup"><span data-stu-id="84518-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="84518-197">`Required` Öznitelik değer türleri gibi atanamaz türler için gerekli olmayan (`DateTime`, `int`, `double`vb..).</span><span class="sxs-lookup"><span data-stu-id="84518-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="84518-198">Null olamaz türleri gerekli alanları otomatik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="84518-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="84518-199">`Required` Özniteliği yerine en küçük uzunluk parametre `StringLength` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="84518-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="84518-200">Görüntüleme özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-200">The Display attribute</span></span>

<span data-ttu-id="84518-201">`Display` Özniteliği, metin kutuları için açıklama yazısını "Ad", "Soyadı", "Tam adı" ve "Kayıt tarihi" olması gerektiğini belirtir</span><span class="sxs-lookup"><span data-stu-id="84518-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="84518-202">Örneğin "Lastname." sözcük bölme boşluk varsayılan açıklamalı alt yazılar vardı</span><span class="sxs-lookup"><span data-stu-id="84518-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="84518-203">Hesaplanan FullName özelliği</span><span class="sxs-lookup"><span data-stu-id="84518-203">The FullName calculated property</span></span>

<span data-ttu-id="84518-204">`FullName` diğer iki özellikleri birleştirilerek oluşturulur bir değer döndüren bir hesaplanan özelliktir.</span><span class="sxs-lookup"><span data-stu-id="84518-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="84518-205">`FullName` , yalnızca bir get erişimcisine sahip, ayarlanmış olamaz.</span><span class="sxs-lookup"><span data-stu-id="84518-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="84518-206">Hayır `FullName` sütunu veritabanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="84518-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="84518-207">Eğitmen varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="84518-207">Create the Instructor Entity</span></span>

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="84518-209">Oluşturma *Models/Instructor.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="84518-210">Birden çok öznitelik, bir satırda olabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="84518-211">`HireDate` Öznitelikleri yazılması gibi:</span><span class="sxs-lookup"><span data-stu-id="84518-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="84518-212">CourseAssignments ve OfficeAssignment Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="84518-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="84518-213">`CourseAssignments` Ve `OfficeAssignment` Gezinti özellikleri özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="84518-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="84518-214">Bir eğitmen kursları herhangi bir sayıda öğretebiliriz, bu nedenle `CourseAssignments` bir koleksiyon olarak tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="84518-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="84518-215">Bir gezinme özelliği birden çok varlık tutuyorsa:</span><span class="sxs-lookup"><span data-stu-id="84518-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="84518-216">Burada girişler eklenir, silindi, güncelleştirilmiş ve bir liste türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="84518-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="84518-217">Gezinti özelliği türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="84518-217">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="84518-218">Varsa `ICollection<T>` belirtilirse, EF Core oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="84518-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="84518-219">`CourseAssignment` Varlık çoktan çoğa ilişkilerde bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="84518-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="84518-220">Contoso University iş durumu bir eğitmen en fazla bir office olabilir kuralları.</span><span class="sxs-lookup"><span data-stu-id="84518-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="84518-221">`OfficeAssignment` Özelliği tek bir tutar `OfficeAssignment` varlık.</span><span class="sxs-lookup"><span data-stu-id="84518-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="84518-222">`OfficeAssignment` hiçbir office atanırsa null olur.</span><span class="sxs-lookup"><span data-stu-id="84518-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="84518-223">OfficeAssignment varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="84518-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="84518-225">Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="84518-226">Anahtar öznitelik</span><span class="sxs-lookup"><span data-stu-id="84518-226">The Key attribute</span></span>

<span data-ttu-id="84518-227">`[Key]` Özniteliği özellik adını bir şey olduğunda bir özelliği birincil anahtarla (PK) Classnameıd veya kimliği dışındaki tanımlamak için kullanılır</span><span class="sxs-lookup"><span data-stu-id="84518-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="84518-228">Arasında bir sıfır-veya-bire bir ilişki yoktur `Instructor` ve `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84518-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="84518-229">Atanan Eğitmeni ile ilgili bir ofis ataması yalnızca var.</span><span class="sxs-lookup"><span data-stu-id="84518-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="84518-230">`OfficeAssignment` PK etmektir aynı zamanda, yabancı anahtar (FK) `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="84518-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="84518-231">EF Core otomatik olarak tanıyamıyor `InstructorID` PK olarak `OfficeAssignment` çünkü:</span><span class="sxs-lookup"><span data-stu-id="84518-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="84518-232">`InstructorID` Kimliği veya Classnameıd adlandırma kuralını uyguladığınızdan değil.</span><span class="sxs-lookup"><span data-stu-id="84518-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="84518-233">Bu nedenle, `Key` tanımlamak için kullanılan öznitelik `InstructorID` PK olarak:</span><span class="sxs-lookup"><span data-stu-id="84518-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="84518-234">Sütunu için tanımlayıcı bir ilişkisi olduğundan varsayılan olarak, olmayan-veritabanında oluşturulmuş olarak anahtar EF Core değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="84518-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="84518-235">Eğitmen gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="84518-235">The Instructor navigation property</span></span>

<span data-ttu-id="84518-236">`OfficeAssignment` Gezinme özelliğinin bağını `Instructor` varlık boş değer atanabilir olmadığından:</span><span class="sxs-lookup"><span data-stu-id="84518-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="84518-237">(Boş değer atanabilir sınıflardır gibi) başvuru türleri.</span><span class="sxs-lookup"><span data-stu-id="84518-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="84518-238">Bir eğitmen bir ofis ataması olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-238">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="84518-239">`OfficeAssignment` Atanamayan bir varlık olan `Instructor` gezinme özelliği için:</span><span class="sxs-lookup"><span data-stu-id="84518-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="84518-240">`InstructorID` null atanamaz olduğundan.</span><span class="sxs-lookup"><span data-stu-id="84518-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="84518-241">Bir ofis ataması bir eğitmen var olamaz.</span><span class="sxs-lookup"><span data-stu-id="84518-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="84518-242">Olduğunda bir `Instructor` varlık ilgili olan `OfficeAssignment` varlık, her varlık, gezinti özelliğinin diğer bir başvuru içerir.</span><span class="sxs-lookup"><span data-stu-id="84518-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="84518-243">`[Required]` Özniteliği uygulandığı `Instructor` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="84518-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="84518-244">Yukarıdaki kod, ilgili Eğitmen olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="84518-245">Yukarıdaki kod gereksizdir çünkü `InstructorID` (aynı zamanda olan PK) yabancı anahtar null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="84518-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="84518-246">Kurs varlığı değiştirme</span><span class="sxs-lookup"><span data-stu-id="84518-246">Modify the Course Entity</span></span>

![Kurs varlık](complex-data-model/_static/course-entity.png)

<span data-ttu-id="84518-248">Güncelleştirme *Models/Course.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="84518-249">`Course` Varlık sahip bir yabancı anahtar (FK) özelliği `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="84518-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="84518-250">`DepartmentID` işaret ilgili `Department` varlık.</span><span class="sxs-lookup"><span data-stu-id="84518-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="84518-251">`Course` Varlık sahip bir `Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="84518-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="84518-252">Gezinme özelliğinin bağını ilgili varlık modeli sahip olduğunda EF Core için bir veri modeli FK özelliğini gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="84518-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="84518-253">İhtiyaç duyulan yere EF Core FKs veritabanında otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="84518-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="84518-254">EF Core oluşturur [gölge Özellikler](/ef/core/modeling/shadow-properties) otomatik olarak oluşturulan FKs için.</span><span class="sxs-lookup"><span data-stu-id="84518-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="84518-255">FK veri modelinde sahip güncelleştirmeleri daha basit ve daha verimli hale getirir.</span><span class="sxs-lookup"><span data-stu-id="84518-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="84518-256">Örneğin, bir model düşünün burada FK özelliği `DepartmentID` olduğu *değil* dahil.</span><span class="sxs-lookup"><span data-stu-id="84518-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="84518-257">Ne zaman bir kurs varlığı düzenlemek için getirilir:</span><span class="sxs-lookup"><span data-stu-id="84518-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="84518-258">`Department` Varlığa açıkça yüklü değilse null.</span><span class="sxs-lookup"><span data-stu-id="84518-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="84518-259">Kurs varlığı güncelleştirmek için `Department` varlık gereken ilk getirildi.</span><span class="sxs-lookup"><span data-stu-id="84518-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="84518-260">Zaman FK özelliği `DepartmentID` dahildir veri modelindeki getirilecek gerek yoktur `Department` bir güncelleştirmeden önce varlık.</span><span class="sxs-lookup"><span data-stu-id="84518-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="84518-261">DatabaseGenerated özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="84518-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` PK uygulama tarafından sağlanan yerine, veritabanı tarafından oluşturulan özniteliğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="84518-263">Varsayılan olarak EF Core PK değerleri DB tarafından oluşturulan varsayar.</span><span class="sxs-lookup"><span data-stu-id="84518-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="84518-264">DB PK oluşturulan değeri genellikle en iyi yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="84518-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="84518-265">İçin `Course` varlıklar, kullanıcı yinelenir belirtir</span><span class="sxs-lookup"><span data-stu-id="84518-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="84518-266">Örneğin, bir kurs sayı 1000 serisi matematik departmanı, 2000 serisi İngilizce departmanı gibi.</span><span class="sxs-lookup"><span data-stu-id="84518-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="84518-267">`DatabaseGenerated` Özniteliği de varsayılan değerleri oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="84518-268">Örneğin, DB, otomatik olarak bir satır oluşturulduğunda veya güncelleştirildiğinde tarihi kaydetmek için bir tarih alanı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84518-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="84518-269">Daha fazla bilgi için [üretilen özellikleri](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="84518-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="84518-270">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="84518-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="84518-271">Gezinti özellikleri ve yabancı anahtar (FK) özellikleri `Course` varlık ilişkileri takip yansıtır:</span><span class="sxs-lookup"><span data-stu-id="84518-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="84518-272">Bu yüzden bir kurs bir bölüme atanan bir `DepartmentID` FK ve `Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="84518-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="84518-273">Herhangi bir sayıda Öğrenciler içinde kayıtlı bir kurs olabilir böylece `Enrollments` olan bir koleksiyon gezinme özelliği:</span><span class="sxs-lookup"><span data-stu-id="84518-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="84518-274">Bir kurs birden çok Eğitmenler tarafından verilen böylece `CourseAssignments` olan bir koleksiyon gezinme özelliği:</span><span class="sxs-lookup"><span data-stu-id="84518-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="84518-275">`CourseAssignment` açıklanan [daha sonra](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="84518-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="84518-276">Departman varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="84518-276">Create the Department entity</span></span>

![Departman varlık](complex-data-model/_static/department-entity.png)

<span data-ttu-id="84518-278">Oluşturma *Models/Department.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="84518-279">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="84518-279">The Column attribute</span></span>

<span data-ttu-id="84518-280">Daha önce `Column` özniteliği sütun adı eşlemesini değiştirmek için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="84518-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="84518-281">Kodunda `Department` varlık `Column` özniteliği SQL veri türü eşlemesi değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84518-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="84518-282">`Budget` Sütun, DB SQL Server para türü kullanılarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="84518-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="84518-283">Sütun eşlemesi genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="84518-283">Column mapping is generally not required.</span></span> <span data-ttu-id="84518-284">EF Core özelliği için CLR türüne göre uygun SQL Server veri türü genellikle seçer.</span><span class="sxs-lookup"><span data-stu-id="84518-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="84518-285">CLR `decimal` türü bir SQL Server eşlenir `decimal` türü.</span><span class="sxs-lookup"><span data-stu-id="84518-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="84518-286">`Budget` para birimi, ve para veri türü için para birimi daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="84518-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="84518-287">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="84518-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="84518-288">FK ve gezinti özellikleri, aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="84518-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="84518-289">Bir bölüm olabilir veya bir yönetici olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="84518-290">Bir yönetici, her zaman bir eğitmen olur.</span><span class="sxs-lookup"><span data-stu-id="84518-290">An administrator is always an instructor.</span></span> <span data-ttu-id="84518-291">Bu nedenle `InstructorID` özelliği için FK olarak dahil edilen `Instructor` varlık.</span><span class="sxs-lookup"><span data-stu-id="84518-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="84518-292">Gezinme özelliğini adlı `Administrator` ancak tutan bir `Instructor` varlık:</span><span class="sxs-lookup"><span data-stu-id="84518-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="84518-293">Önceki kodda soru işareti (?), boş değer atanabilir bir özelliktir belirtir.</span><span class="sxs-lookup"><span data-stu-id="84518-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="84518-294">Kursları gezinti özelliği bu nedenle departman birçok kursları olabilir:</span><span class="sxs-lookup"><span data-stu-id="84518-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="84518-295">Not: Kural gereği, EF Core art arda silme için alamayan FKs ve çoktan çoğa ilişkiler için etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="84518-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="84518-296">Basamaklı silme döngüsel cascade delete kurallarında neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="84518-297">Döngüsel art arda silme kuralları nedenleri özel bir durum geçiş eklendiğinde.</span><span class="sxs-lookup"><span data-stu-id="84518-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="84518-298">Örneğin, varsa `Department.InstructorID` özelliği değildi tanımlı olarak boş değer atanabilir:</span><span class="sxs-lookup"><span data-stu-id="84518-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="84518-299">EF Core departman silindiğinde Eğitmen silmek için bir cascade delete kuralı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="84518-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="84518-300">Eğitmen departman silindiğinde, silme, amaçlanan bir davranış değildir.</span><span class="sxs-lookup"><span data-stu-id="84518-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="84518-301">İş kuralları gerekirse `InstructorID` özelliği NULL olmayan, aşağıdaki fluent API'si deyimi kullanın:</span><span class="sxs-lookup"><span data-stu-id="84518-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="84518-302">Yukarıdaki kod art arda silme departmanı Eğitmen ilişkiyi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="84518-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="84518-303">Güncelleştirme kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="84518-303">Update the Enrollment entity</span></span>

<span data-ttu-id="84518-304">Bir kaydı bir öğrenci tarafından gerçekleştirilen bir kurs içindir.</span><span class="sxs-lookup"><span data-stu-id="84518-304">An enrollment record is for one course taken by one student.</span></span>

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="84518-306">Güncelleştirme *Models/Enrollment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="84518-307">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="84518-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="84518-308">Aşağıdaki ilişkileri ve gezinti özelliklerini FK özelliklerini yansıtır:</span><span class="sxs-lookup"><span data-stu-id="84518-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="84518-309">Bir kaydı var. bir kursa olduğundan bir `CourseID` FK özelliği ve `Course` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="84518-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="84518-310">Bu yüzden bir kaydı bir öğrenci için olan bir `StudentID` FK özelliği ve `Student` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="84518-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="84518-311">Çoktan çoğa ilişkiler</span><span class="sxs-lookup"><span data-stu-id="84518-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="84518-312">Bir çoktan çoğa ilişki arasında `Student` ve `Course` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84518-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="84518-313">`Enrollment` Varlık işlevleri bire çok birleşme tablo olarak *yüküyle* veritabanında.</span><span class="sxs-lookup"><span data-stu-id="84518-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="84518-314">"Yüke sahip" anlamına gelir `Enrollment` tablo FKs yanı sıra birleştirilmiş tablolar için ek veri içerir (Bu durumda, PK ve `Grade`).</span><span class="sxs-lookup"><span data-stu-id="84518-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="84518-315">Aşağıdaki çizim, bu ilişkiler bir varlık diyagramda nasıl göründüğünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="84518-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="84518-316">(Bu diyagramda kullanılarak oluşturulan [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) EF için 6.x.</span><span class="sxs-lookup"><span data-stu-id="84518-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="84518-317">Diyagram oluşturma öğreticinin bir parçası değildir.)</span><span class="sxs-lookup"><span data-stu-id="84518-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Öğrenci Kursluk çok-çok ilişkisi](complex-data-model/_static/student-course.png)

<span data-ttu-id="84518-319">Her ilişki ucu ve bir yıldız işareti (\*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.</span><span class="sxs-lookup"><span data-stu-id="84518-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="84518-320">Varsa `Enrollment` tablo vermedi sınıf bilgileri eklemeyi unutmayın, yalnızca iki FKs içermesi gerekir (`CourseID` ve `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="84518-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="84518-321">Bire çok birleşme tablo yükü olmadan bazen saf birleşim tablosu (PJT) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="84518-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="84518-322">`Instructor` Ve `Course` varlıkların saf birleştirme tablo kullanarak bir çoktan çoğa ilişki.</span><span class="sxs-lookup"><span data-stu-id="84518-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="84518-323">Not: Çoktan çoğa ilişkiler ancak EF Core için örtük birleşim tabloları değil EF 6.x destekler.</span><span class="sxs-lookup"><span data-stu-id="84518-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="84518-324">Daha fazla bilgi için [çoktan çoğa ilişkilerde EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="84518-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="84518-325">CourseAssignment varlık</span><span class="sxs-lookup"><span data-stu-id="84518-325">The CourseAssignment entity</span></span>

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="84518-327">Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="84518-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="84518-328">Eğitmen kursları</span><span class="sxs-lookup"><span data-stu-id="84518-328">Instructor-to-Courses</span></span>

![Eğitmen kursları m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="84518-330">Eğitmen kursları çoktan çoğa ilişki:</span><span class="sxs-lookup"><span data-stu-id="84518-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="84518-331">Bir varlık kümesi tarafından temsil edilen bir birleşim tablo gerektirir.</span><span class="sxs-lookup"><span data-stu-id="84518-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="84518-332">Saf birleşim tablosu (tablo yükü olmadan) var.</span><span class="sxs-lookup"><span data-stu-id="84518-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="84518-333">Bir birleşim varlık adı yaygındır `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="84518-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="84518-334">Örneğin, bu düzeni kullanmak Eğitmen kursları birleştirme tablodur `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="84518-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="84518-335">Ancak, ilişkiyi tanımlayan bir ad kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="84518-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="84518-336">Veri modelleri basit ' ı başlatın ve büyütün.</span><span class="sxs-lookup"><span data-stu-id="84518-336">Data models start out simple and grow.</span></span> <span data-ttu-id="84518-337">Hayır-yükü birleşimler (PJTs) yükü dahil etmek için sık değişime uğramaktadır.</span><span class="sxs-lookup"><span data-stu-id="84518-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="84518-338">Bir tanımlayıcı varlık adı ile başlatarak ad birleştirme tablo değiştiğinde değiştirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="84518-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="84518-339">Birleştirme varlık ideal olarak, iş etki alanında kendi doğal (muhtemelen tek sözcük) adına sahip.</span><span class="sxs-lookup"><span data-stu-id="84518-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="84518-340">Örneğin, kitaplar ve müşterilerin derecelendirmeleri adlı birleştirme varlıkla bağ kurulamadı.</span><span class="sxs-lookup"><span data-stu-id="84518-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="84518-341">Eğitmen kursları çoktan çoğa ilişki `CourseAssignment` üzerinden tercih edilen `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="84518-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="84518-342">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="84518-342">Composite key</span></span>

<span data-ttu-id="84518-343">FKs boş değer atanabilir değil.</span><span class="sxs-lookup"><span data-stu-id="84518-343">FKs are not nullable.</span></span> <span data-ttu-id="84518-344">İçinde iki FKs `CourseAssignment` (`InstructorID` ve `CourseID`) birlikte her satırının benzersiz olarak tanımlanabilmesi `CourseAssignment` tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="84518-345">`CourseAssignment` adanmış bir yinelenir gerektirmez</span><span class="sxs-lookup"><span data-stu-id="84518-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="84518-346">`InstructorID` Ve `CourseID` özellikleri bir bileşik PK olarak işlevi</span><span class="sxs-lookup"><span data-stu-id="84518-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="84518-347">EF Core için bileşik BA belirtmek için tek yolu olan *fluent API'si*.</span><span class="sxs-lookup"><span data-stu-id="84518-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="84518-348">Sonraki bölümde, bileşik PK yapılandırma işlemi gösterilmektedir</span><span class="sxs-lookup"><span data-stu-id="84518-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="84518-349">Bileşik anahtarın sağlar:</span><span class="sxs-lookup"><span data-stu-id="84518-349">The composite key ensures:</span></span>

* <span data-ttu-id="84518-350">Birden çok satır bir kurs için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="84518-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="84518-351">Birden çok satır bir eğitmen için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="84518-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="84518-352">Birden çok satır aynı eğitmen ve kurs için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="84518-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="84518-353">`Enrollment` Birleştirme varlık çoğaltmaları bu tür mümkün olduğundan, kendi PK tanımlar.</span><span class="sxs-lookup"><span data-stu-id="84518-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="84518-354">Bu tür çoğaltmaları engellemek için:</span><span class="sxs-lookup"><span data-stu-id="84518-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="84518-355">FK alanlarda benzersiz bir dizin eklemek veya</span><span class="sxs-lookup"><span data-stu-id="84518-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="84518-356">Yapılandırma `Enrollment` benzer şekilde birincil Bileşik anahtarı `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="84518-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="84518-357">Daha fazla bilgi için [dizinleri](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="84518-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="84518-358">DB bağlamı güncelleştir</span><span class="sxs-lookup"><span data-stu-id="84518-358">Update the DB context</span></span>

<span data-ttu-id="84518-359">Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="84518-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="84518-360">Yukarıdaki kod, yeni varlıkları ekleyen ve yapılandırır `CourseAssignment` varlığın bileşik yinelenir</span><span class="sxs-lookup"><span data-stu-id="84518-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="84518-361">Fluent API'si alternatif öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="84518-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="84518-362">`OnModelCreating` Önceki yöntemidir kod *fluent API'si* EF Core davranışı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="84518-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="84518-363">API, genellikle bir dizi yöntem çağrılarını birleştirerek tek bir deyimde stringing kullanıldığı için "fluent" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="84518-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="84518-364">[Koddan](/ef/core/modeling/#methods-of-configuration) fluent API'si örneğidir:</span><span class="sxs-lookup"><span data-stu-id="84518-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="84518-365">Bu öğreticide, yalnızca özniteliklerle yapılamaz DB eşlemesi için fluent API'si kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84518-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="84518-366">Ancak, fluent API'si biçimlendirmeyi, doğrulama ve öznitelikleri ile yapılabilir eşleme kurallarını çoğu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84518-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="84518-367">Bazı öznitelikler gibi `MinimumLength` fluent API'si ile uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="84518-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="84518-368">`MinimumLength` Şema değişmez yalnızca bir minimum uzunluğu doğrulama kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="84518-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="84518-369">Bazı geliştiriciler özel olarak bunlar kendi varlık sınıfları "temiz" olan fluent API'sini kullanmayı tercih edin</span><span class="sxs-lookup"><span data-stu-id="84518-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="84518-370">Öznitelikleri ve fluent API'si karma olabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="84518-371">(Bir bileşik PK belirtme) fluent API'si ile yalnızca yapılabilir bazı yapılandırmalar vardır.</span><span class="sxs-lookup"><span data-stu-id="84518-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="84518-372">Özniteliklerle yalnızca yapılabilir bazı yapılandırmalar vardır (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="84518-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="84518-373">Fluent API'si veya öznitelikleri kullanarak için önerilen uygulama:</span><span class="sxs-lookup"><span data-stu-id="84518-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="84518-374">Bu iki yaklaşımdan birini seçin.</span><span class="sxs-lookup"><span data-stu-id="84518-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="84518-375">Tutarlı bir şekilde mümkün olduğunca seçilen yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="84518-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="84518-376">Bu konuda kullanılan öznitelikler bazıları öğreticisi için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="84518-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="84518-377">Yalnızca doğrulama (örneğin, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="84518-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="84518-378">EF Core yalnızca yapılandırmayı (örneğin, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="84518-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="84518-379">Doğrulama ve EF Core yapılandırma (örneğin, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="84518-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="84518-380">Fluent API'si ve öznitelikler hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri,](/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="84518-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="84518-381">Varlık diyagramda gösteren ilişkileri</span><span class="sxs-lookup"><span data-stu-id="84518-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="84518-382">Aşağıdaki resimde tamamlanmış Okul modelini EF Power Tools oluşturduğunuz diyagramda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="84518-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="84518-384">Yukarıdaki diyagramda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="84518-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="84518-385">Bire çok ilişkisi birkaç satır (1 \*).</span><span class="sxs-lookup"><span data-stu-id="84518-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="84518-386">Arasında bir sıfır-veya-bir ilişki satırı (için 1 0..1) `Instructor` ve `OfficeAssignment` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84518-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="84518-387">Sıfır-veya-bir-çok ilişki çizgisi (0..1 için \*) arasında `Instructor` ve `Department` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="84518-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="84518-388">Çekirdek DB Test verileri ile</span><span class="sxs-lookup"><span data-stu-id="84518-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="84518-389">Kodu güncelleştirme *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="84518-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="84518-390">Yukarıdaki kod yeni varlıklar için çekirdek veri sağlar.</span><span class="sxs-lookup"><span data-stu-id="84518-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="84518-391">Bu kod çoğu, yeni varlık nesnesi oluşturur ve örnek verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="84518-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="84518-392">Örnek verileri, test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84518-392">The sample data is used for testing.</span></span> <span data-ttu-id="84518-393">Bkz: `Enrollments` ve `CourseAssignments` nasıl çoktan çoğa tabloları birleştirme örnekleri sağlanmış için.</span><span class="sxs-lookup"><span data-stu-id="84518-393">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="84518-394">Bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="84518-394">Add a migration</span></span>

<span data-ttu-id="84518-395">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="84518-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84518-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84518-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84518-397">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="84518-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="84518-398">Yukarıdaki komut, olası veri kaybı ile ilgili bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="84518-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="84518-399">Varsa `database update` komutu çalıştırın, aşağıdaki hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="84518-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="84518-400">Geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="84518-400">Apply the migration</span></span>

<span data-ttu-id="84518-401">Varolan bir veritabanınız olduğuna göre gelecekteki değişiklikleri uygulamak konusunda düşünmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="84518-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="84518-402">Bu öğreticide iki yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="84518-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="84518-403">Bırakın ve veritabanını yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="84518-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="84518-404">[Varolan bir veritabanına geçiş Uygula](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="84518-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="84518-405">Bu yöntem daha karmaşık ve zaman alıcı olsa da, gerçek, üretim ortamları için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="84518-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="84518-406">**Not**: Bu, öğreticinin isteğe bağlı bir bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="84518-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="84518-407">Açılan yapın ve adımları yeniden oluşturun ve bu bölümü atlayın.</span><span class="sxs-lookup"><span data-stu-id="84518-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="84518-408">Bu bölümdeki adımları takip etmek istiyorsanız, yoksa açılan yapın ve adımları yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="84518-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="84518-409">Bırakın ve veritabanını yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="84518-409">Drop and re-create the database</span></span>

<span data-ttu-id="84518-410">Güncelleştirilmiş kod `DbInitializer` yeni varlıklar için çekirdek veri ekler.</span><span class="sxs-lookup"><span data-stu-id="84518-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="84518-411">Yeni bir veritabanı oluşturmak için EF Core zorlamak için bırakın ve DB güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="84518-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84518-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84518-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="84518-413">İçinde **Paket Yöneticisi Konsolu** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="84518-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="84518-414">Çalıştırma `Get-Help about_EntityFrameworkCore` Yardım bilgilerini almak için PMC'yi öğesinden.</span><span class="sxs-lookup"><span data-stu-id="84518-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84518-415">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="84518-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84518-416">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="84518-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="84518-417">Proje klasörünü içeren *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="84518-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="84518-418">Komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="84518-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="84518-419">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84518-419">Run the app.</span></span> <span data-ttu-id="84518-420">Uygulama çalıştırmaları çalıştıran `DbInitializer.Initialize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="84518-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="84518-421">`DbInitializer.Initialize` Yeni bir veritabanı doldurur.</span><span class="sxs-lookup"><span data-stu-id="84518-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="84518-422">Bir veritabanı içinde SSOX açın:</span><span class="sxs-lookup"><span data-stu-id="84518-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="84518-423">SSOX daha önce açıldı, tıklayın **Yenile** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="84518-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="84518-424">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="84518-424">Expand the **Tables** node.</span></span> <span data-ttu-id="84518-425">Oluşturulan bir tablo görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84518-425">The created tables are displayed.</span></span>

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="84518-427">İnceleme **CourseAssignment** tablosu:</span><span class="sxs-lookup"><span data-stu-id="84518-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="84518-428">Sağ **CourseAssignment** tablosunu seçip **görünüm verilerini**.</span><span class="sxs-lookup"><span data-stu-id="84518-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="84518-429">Doğrulama **CourseAssignment** tablo verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="84518-429">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="84518-431">Varolan bir veritabanına geçiş Uygula</span><span class="sxs-lookup"><span data-stu-id="84518-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="84518-432">Bu bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="84518-432">This section is optional.</span></span> <span data-ttu-id="84518-433">Bu adımlar önceki atladıysanız işe [kaldırın ve yeniden, veritabanı oluşturma](#drop) bölümü.</span><span class="sxs-lookup"><span data-stu-id="84518-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="84518-434">Geçişleri mevcut verilerle çalışırken mevcut verilerle karşılanmıyor FK kısıtlamaları olabilir.</span><span class="sxs-lookup"><span data-stu-id="84518-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="84518-435">Üretim verileriyle, mevcut verileri geçirmek için adım atılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="84518-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="84518-436">Bu bölümde, FK sabiti ihlallerini düzeltmek bir örnek sağlar.</span><span class="sxs-lookup"><span data-stu-id="84518-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="84518-437">Bir yedek olmadan bu kod değişiklikleri yapmayın.</span><span class="sxs-lookup"><span data-stu-id="84518-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="84518-438">Önceki bölümde tamamlandı ve veritabanını, bu kod değişiklikleri yapmayın.</span><span class="sxs-lookup"><span data-stu-id="84518-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="84518-439">*{Timestamp}_ComplexDataModel.cs* dosyası aşağıdaki kodu içerir:</span><span class="sxs-lookup"><span data-stu-id="84518-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="84518-440">Yukarıdaki kod atanamayan bir ekler `DepartmentID` FK için `Course` tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="84518-441">Önceki öğreticide Veritabanından satır içeren `Course`, bu tabloyu geçişleri tarafından güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="84518-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="84518-442">Yapmak `ComplexDataModel` mevcut verilerle geçiş iş:</span><span class="sxs-lookup"><span data-stu-id="84518-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="84518-443">Yeni bir sütun vermek için kodu değiştirin (`DepartmentID`) varsayılan bir değer.</span><span class="sxs-lookup"><span data-stu-id="84518-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="84518-444">Varsayılan departman olarak görev yapacak "Temp" adlı sahte bir bölüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="84518-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="84518-445">Yabancı anahtar kısıtlamalarını düzeltme</span><span class="sxs-lookup"><span data-stu-id="84518-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="84518-446">Güncelleştirme `ComplexDataModel` sınıfları `Up` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="84518-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="84518-447">Açık *{timestamp}_ComplexDataModel.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="84518-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="84518-448">Yorum ekleyen bir kod satırının `DepartmentID` sütuna `Course` tablo.</span><span class="sxs-lookup"><span data-stu-id="84518-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="84518-449">Aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84518-449">Add the following highlighted code.</span></span> <span data-ttu-id="84518-450">Sonra yeni kodu gider `.CreateTable( name: "Department"` engelle:</span><span class="sxs-lookup"><span data-stu-id="84518-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="84518-451">Varolan önceki değişikliklerle `Course` satır ilgili sonra "Temp" departman `ComplexDataModel` `Up` yöntemi çalışır.</span><span class="sxs-lookup"><span data-stu-id="84518-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="84518-452">Bir üretim uygulaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="84518-452">A production app would:</span></span>

* <span data-ttu-id="84518-453">Kod veya betiklerde eklemek için dahil `Department` satırları ve ilişkili `Course` yeni satır `Department` satır.</span><span class="sxs-lookup"><span data-stu-id="84518-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="84518-454">"Temp" departman veya için varsayılan değer kullanmamanız `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="84518-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="84518-455">Sonraki öğreticiye ilgili verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="84518-455">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84518-456">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="84518-456">Additional resources</span></span>

* [<span data-ttu-id="84518-457">Bu öğreticide (Kısım 1) YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="84518-457">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="84518-458">Bu öğreticide (Bölüm 2) YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="84518-458">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="84518-459">[Önceki](xref:data/ef-rp/migrations)
> [İleri](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="84518-459">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
