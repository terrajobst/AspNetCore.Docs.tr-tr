---
title: ASP.NET Core veri modelinde EF Core ile Razor Pages-5/8
author: rick-anderson
description: Bu öğreticide, daha fazla varlık ve ilişki ekleyin ve biçimlendirme, doğrulama ve eşleme kurallarını belirterek veri modelini özelleştirin.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 96e4acf0d6c9c079ebee32fc2f9951fdd668931b
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914966"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="9005b-103">ASP.NET Core veri modelinde EF Core ile Razor Pages-5/8</span><span class="sxs-lookup"><span data-stu-id="9005b-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="9005b-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9005b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9005b-105">Önceki öğreticiler, üç varlıktan oluşan temel bir veri modeliyle çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="9005b-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="9005b-106">Bu öğreticide:</span><span class="sxs-lookup"><span data-stu-id="9005b-106">In this tutorial:</span></span>

* <span data-ttu-id="9005b-107">Daha fazla varlık ve ilişki eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="9005b-108">Veri modeli biçimlendirme, doğrulama ve veritabanı eşleme kuralları belirtilerek özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="9005b-109">Tamamlanan veri modeli aşağıdaki çizimde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9005b-109">The completed data model is shown in the following illustration:</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="9005b-111">Öğrenci varlık</span><span class="sxs-lookup"><span data-stu-id="9005b-111">The Student entity</span></span>

![Öğrenci varlığı](complex-data-model/_static/student-entity.png)

<span data-ttu-id="9005b-113">*Modeller/öğrenci. cs* dosyasındaki kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="9005b-114">Yukarıdaki kod, bir `FullName` özelliği ekler ve var olan özelliklere aşağıdaki öznitelikleri ekler:</span><span class="sxs-lookup"><span data-stu-id="9005b-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="9005b-115">FullName hesaplanmış özelliği</span><span class="sxs-lookup"><span data-stu-id="9005b-115">The FullName calculated property</span></span>

<span data-ttu-id="9005b-116">`FullName`, iki diğer özelliğin bitiştirerek oluşturulmuş bir değer döndüren hesaplanmış bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9005b-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="9005b-117">`FullName`ayarlanamaz, bu nedenle yalnızca bir get erişimcisi vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="9005b-118">Veritabanında `FullName` hiçbir sütun oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="9005b-119">DataType özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="9005b-120">Öğrenci kayıt tarihleri için, tüm sayfalar şu anda tarihle birlikte tarih ile görüntülenir, ancak yalnızca tarihin ilgili olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="9005b-121">Veri ek açıklaması özniteliklerini kullanarak, verileri gösteren her sayfada görüntü biçimini giderecek bir kod değişikliği yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9005b-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="9005b-122">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) özniteliği, veritabanı iç türünden daha belirgin bir veri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="9005b-123">Bu durumda, tarih ve saat değil yalnızca tarih görüntülenmelidir.</span><span class="sxs-lookup"><span data-stu-id="9005b-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="9005b-124">Veri [türü numaralandırması](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) , tarih, saat, PhoneNumber, para birimi, emaadresi vb. gibi birçok veri türü sağlar. `DataType` Özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="9005b-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9005b-125">For example:</span></span>

* <span data-ttu-id="9005b-126">`mailto:` Bağlantı için`DataType.EmailAddress`otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9005b-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="9005b-127">Tarih Seçici çoğu tarayıcıda için `DataType.Date` verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="9005b-128">Öznitelik HTML 5 `data-` (bir veri Dash) özniteliklerini yayar. `DataType`</span><span class="sxs-lookup"><span data-stu-id="9005b-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="9005b-129">`DataType` Öznitelikler doğrulama sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="9005b-130">DisplayFormat özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="9005b-131">`DataType.Date`görüntülenen tarihin biçimini belirtmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="9005b-132">Varsayılan olarak, Tarih alanı sunucunun [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)öğesine göre varsayılan biçimlere göre görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="9005b-133">`DisplayFormat` Öznitelik, tarih biçimini açıkça belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="9005b-134">`ApplyFormatInEditMode` Ayar, biçimlendirmenin düzenleme kullanıcı arabirimine de uygulanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="9005b-135">Bazı alanlar kullanmamanız `ApplyFormatInEditMode`gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="9005b-136">Örneğin, para birimi simgesi genellikle bir düzenleme metin kutusunda gösterilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9005b-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="9005b-137">`DisplayFormat` Özniteliği kendi başına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="9005b-138">Özniteliği `DataType` özniteliği`DisplayFormat` ile kullanmak genellikle iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="9005b-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="9005b-139">`DataType` Özniteliği, bir ekranda nasıl işleneceğini değil, verilerin semantiğini alır.</span><span class="sxs-lookup"><span data-stu-id="9005b-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="9005b-140">Özniteliği `DataType` , içinde `DisplayFormat`kullanılamayan aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9005b-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="9005b-141">Tarayıcı HTML5 özelliklerini etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="9005b-142">Örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını ve istemci tarafı giriş doğrulamasını gösterin.</span><span class="sxs-lookup"><span data-stu-id="9005b-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="9005b-143">Varsayılan olarak tarayıcı, verileri yerel ayara göre doğru biçimi kullanarak işler.</span><span class="sxs-lookup"><span data-stu-id="9005b-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="9005b-144">Daha fazla bilgi için bkz [ \<. Giriş > etiketi Yardımcısı belgeleri](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9005b-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="9005b-145">StringLength özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="9005b-146">Veri doğrulama kuralları ve doğrulama hatası iletileri özniteliklerle belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="9005b-147">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği, bir veri alanında izin verilen en düşük ve en fazla karakter uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="9005b-148">Gösterilen kod, adları 50 karakterden fazla olmayacak şekilde sınırlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="9005b-149">En küçük dize uzunluğunu ayarlayan bir örnek [daha sonra](#the-required-attribute)gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="9005b-150">`StringLength` Öznitelik Ayrıca istemci tarafı ve sunucu tarafı doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="9005b-151">En küçük değerin veritabanı şeması üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="9005b-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="9005b-152">Öznitelik `StringLength` , bir kullanıcının ad için boşluk girmesini engellemez.</span><span class="sxs-lookup"><span data-stu-id="9005b-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="9005b-153">[Cevap içerisinde RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği, girişe kısıtlamalar uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="9005b-154">Örneğin, aşağıdaki kod ilk karakterin büyük küçük harf olmasını ve geri kalan karakterlerin alfabetik olmasını gerektirir:</span><span class="sxs-lookup"><span data-stu-id="9005b-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9005b-156">**SQL Server Nesne Gezgini** (ssox) içinde **öğrenci** tablosuna çift tıklayarak öğrenci tablosu tasarımcısını açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Geçişle önce SSOX 'teki öğrenciler tablosu](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="9005b-158">Önceki görüntüde `Student` tablo için şema gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="9005b-159">Ad alanlarının türü `nvarchar(MAX)`vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="9005b-160">Bu öğreticide daha sonra bir geçiş oluşturulup uygulandığında, ad alanları dize uzunluğu özniteliklerinin bir `nvarchar(50)` sonucu olarak olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9005b-162">SQLite aracınız içinde `Student` tablo için sütun tanımlarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="9005b-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="9005b-163">Ad alanlarının türü `Text`vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-163">The name fields have type `Text`.</span></span> <span data-ttu-id="9005b-164">İlk ad alanının çağrıldığından `FirstMidName`emin olun.</span><span class="sxs-lookup"><span data-stu-id="9005b-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="9005b-165">Sonraki bölümde, bu sütunun adını olarak `FirstName`değiştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9005b-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="9005b-166">Column özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="9005b-167">Öznitelikler sınıfların ve özelliklerin veritabanına nasıl eşlenildiğini denetleyebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="9005b-168">Modelinde öznitelik, `FirstMidName` özelliğin adını veritabanında "FirstName" olarak eşlemek için kullanılır. `Column` `Student`</span><span class="sxs-lookup"><span data-stu-id="9005b-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="9005b-169">Veritabanı oluşturulduğunda, modeldeki Özellik adları sütun adları için kullanılır ( `Column` özniteliği kullanıldığı durumlar dışında).</span><span class="sxs-lookup"><span data-stu-id="9005b-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="9005b-170">Model, birinci `FirstMidName` ad alanı için kullanılır çünkü alan de bir orta ad içerebilir. `Student`</span><span class="sxs-lookup"><span data-stu-id="9005b-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="9005b-171">Özniteliği ile, `Student.FirstMidName` `Student` veri modelinde tablonun sütunuyla eşlenir. `FirstName` `[Column]`</span><span class="sxs-lookup"><span data-stu-id="9005b-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="9005b-172">`Column` Özniteliği ekleme modeli, `SchoolContext`öğesini yedekleyen olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="9005b-173">' İ destekleyen `SchoolContext` model artık veritabanıyla eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="9005b-174">Bu tutarsızlık, daha sonra bu öğreticide bir geçiş eklenerek çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="9005b-175">Gerekli öznitelik</span><span class="sxs-lookup"><span data-stu-id="9005b-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="9005b-176">`Required` Özniteliği, ad özellikleri gerekli alanları yapar.</span><span class="sxs-lookup"><span data-stu-id="9005b-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="9005b-177">Öznitelik, değer türleri ( `DateTime`örneğin `int`,, ve `double`) gibi null yapılamayan türler için gerekli değildir. `Required`</span><span class="sxs-lookup"><span data-stu-id="9005b-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="9005b-178">Null olmayan türler otomatik olarak gerekli alanlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="9005b-179">Öznitelik, `StringLength` özniteliğinde bir minimum length parametresiyle değiştirilebilir: `Required`</span><span class="sxs-lookup"><span data-stu-id="9005b-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="9005b-180">Display özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="9005b-181">`Display` Öznitelik, metin kutuları için başlığın "ad", "soyadı", "tam ad" ve "kayıt tarihi" olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="9005b-182">Varsayılan açıklamalı alt yazıların sözcükleri bölen bir boşluk yoktu, örneğin "LastName".</span><span class="sxs-lookup"><span data-stu-id="9005b-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="9005b-183">Geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="9005b-183">Create a migration</span></span>

<span data-ttu-id="9005b-184">Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="9005b-185">Bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9005b-185">An exception is thrown.</span></span> <span data-ttu-id="9005b-186">Özniteliği, EF 'in adlı `FirstName`bir sütun bulmasını beklemesine neden olur, ancak veritabanındaki sütun adı hala kalır `FirstMidName`. `[Column]`</span><span class="sxs-lookup"><span data-stu-id="9005b-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9005b-188">Hata iletisi aşağıdaki örneğe benzer:</span><span class="sxs-lookup"><span data-stu-id="9005b-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="9005b-189">PMC 'de yeni bir geçiş oluşturmak ve veritabanını güncelleştirmek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="9005b-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="9005b-190">Bu komutlardan ilki aşağıdaki uyarı iletisini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9005b-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="9005b-191">Ad alanları artık 50 karakterle sınırlı olduğundan uyarı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9005b-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="9005b-192">Veritabanındaki bir ad 50 karakterden fazlasına sahipse, son karakter 51 ' i kaybedersiniz.</span><span class="sxs-lookup"><span data-stu-id="9005b-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="9005b-193">Öğrenci tablosunu SSOX içinde açın:</span><span class="sxs-lookup"><span data-stu-id="9005b-193">Open the Student table in SSOX:</span></span>

  ![Geçişlerde SSOX 'teki öğrenciler tablosu](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="9005b-195">Geçiş uygulanmadan önce ad sütunları [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)türünde idi.</span><span class="sxs-lookup"><span data-stu-id="9005b-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="9005b-196">Ad sütunları artık `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="9005b-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="9005b-197">Sütun adı `FirstMidName` olarak `FirstName`değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="9005b-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9005b-199">Hata iletisi aşağıdaki örneğe benzer:</span><span class="sxs-lookup"><span data-stu-id="9005b-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="9005b-200">Proje klasöründe bir komut penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-200">Open a command window in the project folder.</span></span> <span data-ttu-id="9005b-201">Yeni bir geçiş oluşturmak ve veritabanını güncelleştirmek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="9005b-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="9005b-202">Veritabanı güncelleştirme komutu aşağıdaki örnekte olduğu gibi bir hata görüntüler:</span><span class="sxs-lookup"><span data-stu-id="9005b-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="9005b-203">Bu öğreticide, bu hatayı geçmenin yolu ilk geçişi silmek ve yeniden oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="9005b-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="9005b-204">Daha fazla bilgi için, [geçiş öğreticisinin](xref:data/ef-rp/migrations)en üstündeki SQLite uyarı notuna bakın.</span><span class="sxs-lookup"><span data-stu-id="9005b-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="9005b-205">*Geçişler* klasörünü silin.</span><span class="sxs-lookup"><span data-stu-id="9005b-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="9005b-206">Veritabanını bırakmak, yeni bir başlangıç geçişi oluşturmak ve geçişi uygulamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="9005b-207">SQLite araclarınızın öğrenci tablosunu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="9005b-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="9005b-208">FirstMidName sütunu artık FirstName.</span><span class="sxs-lookup"><span data-stu-id="9005b-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="9005b-209">Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="9005b-210">Saatin giriş veya tarih ile birlikte görüntülenmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9005b-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="9005b-211">**Yeni oluştur**' u seçin ve 50 karakterden daha uzun bir ad girmeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="9005b-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="9005b-212">Aşağıdaki bölümlerde, uygulamanın bazı aşamalardan oluşturulması derleyici hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="9005b-213">Yönergeler uygulamanın ne zaman derbir olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="9005b-214">Eğitmen varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-214">The Instructor Entity</span></span>

![Eğitmen varlığı](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="9005b-216">Aşağıdaki kodla *modeller/eğitmen. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="9005b-217">Birden çok öznitelik tek bir satırda olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="9005b-218">`HireDate` Öznitelikler aşağıdaki gibi yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="9005b-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="9005b-219">Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-219">Navigation properties</span></span>

<span data-ttu-id="9005b-220">`CourseAssignments` Ve`OfficeAssignment` özellikleri gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="9005b-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="9005b-221">Bir eğitmen herhangi bir sayıda kurs öğretebilir, bu `CourseAssignments` nedenle bir koleksiyon olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9005b-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="9005b-222">Bir eğitmenin en fazla bir ofisi olabilir, bu nedenle `OfficeAssignment` özellik tek `OfficeAssignment` bir varlık içerir.</span><span class="sxs-lookup"><span data-stu-id="9005b-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="9005b-223">`OfficeAssignment`hiçbir Office atanmamışsa null olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="9005b-224">OfficeAssignment varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-224">The OfficeAssignment entity</span></span>

![OfficeAssignment varlığı](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="9005b-226">Aşağıdaki kodla *modeller/OfficeAssignment. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="9005b-227">Anahtar özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-227">The Key attribute</span></span>

<span data-ttu-id="9005b-228">`[Key]` Öznitelik, özellik adı classnameıd veya ID dışında bir şey olduğunda, bir özelliği birincil anahtar (PK) olarak tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="9005b-229">`Instructor` Ve`OfficeAssignment` varlıkları arasında bire sıfır veya-bir ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="9005b-230">Office ataması, atandığı eğitmenle ilişkili olarak yalnızca vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="9005b-231">PK Ayrıca `Instructor` varlığa ait yabancı anahtardır (FK). `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="9005b-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="9005b-232">EF Core, ID veya `InstructorID` classnameıd adlandırma `OfficeAssignment` kuralını `InstructorID` takip ettiğinden, ' ın ' ın ' i tarafından otomatik olarak tanıyamamaktadır.</span><span class="sxs-lookup"><span data-stu-id="9005b-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="9005b-233">Bu nedenle, `Key` özniteliği PK olarak tanımlamak `InstructorID` için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9005b-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="9005b-234">Varsayılan olarak, EF Core, sütun tanımlayıcı bir ilişki için olduğundan, anahtarı veritabanı olmayan bir şekilde değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="9005b-235">Eğitmen gezintisi özelliği</span><span class="sxs-lookup"><span data-stu-id="9005b-235">The Instructor navigation property</span></span>

<span data-ttu-id="9005b-236">Belirtilen `Instructor.OfficeAssignment` bir eğitmen için bir `OfficeAssignment` satır olmadığı için, gezinti özelliği null olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="9005b-237">Bir eğitmenin Office ataması olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="9005b-238">Yabancı anahtar`InstructorID` türü `OfficeAssignment.Instructor` nullyapılamayanbirdeğertürüolduğundan,gezintiözelliğiherzamanbireğitmen`int`varlığına sahip olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="9005b-239">Bir Office ataması, bir eğitmen olmadan bulunamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="9005b-240">Bir `Instructor` varlık ilişkili `OfficeAssignment` bir varlığa sahip olduğunda, her varlığın gezinti özelliğinde diğer bir başvurusu vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="9005b-241">Kurs varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-241">The Course Entity</span></span>

![Kurs varlığı](complex-data-model/_static/course-entity.png)

<span data-ttu-id="9005b-243">*Modelleri/kursu. cs* aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="9005b-244">Varlığın yabancı anahtar (FK) özelliği `DepartmentID`vardır. `Course`</span><span class="sxs-lookup"><span data-stu-id="9005b-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="9005b-245">`DepartmentID`ilgili `Department` varlığa işaret eder.</span><span class="sxs-lookup"><span data-stu-id="9005b-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="9005b-246">`Course` Varlığın bir`Department` gezinti özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="9005b-247">EF Core, modelin ilgili bir varlık için gezinti özelliği olduğunda bir veri modeli için yabancı anahtar özelliği gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="9005b-248">EF Core, gerektiği yerde otomatik olarak veritabanında FKs 'ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="9005b-249">EF Core otomatik olarak oluşturulan FKs 'ler için [gölge Özellikler](/ef/core/modeling/shadow-properties) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="9005b-250">Ancak, doğrudan veri modelinde FK dahil edilmesi, güncelleştirmelerin daha basit ve daha verimli olmasını sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="9005b-251">Örneğin, FK özelliğinin `DepartmentID` dahil *olmadığı* bir model düşünün.</span><span class="sxs-lookup"><span data-stu-id="9005b-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="9005b-252">Bir kurs varlığı düzenlemek üzere getirilirken:</span><span class="sxs-lookup"><span data-stu-id="9005b-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="9005b-253">Açık bir şekilde yüklenmediyse özelliğinullolur.`Department`</span><span class="sxs-lookup"><span data-stu-id="9005b-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="9005b-254">Kurs varlığını `Department` güncelleştirmek için önce varlığın getirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="9005b-255">FK özelliği `DepartmentID` veri modeline dahil edildiğinde, bir güncelleştirmeden önce `Department` varlığı getirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="9005b-256">DatabaseGenerated özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="9005b-257">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Özniteliği, PK 'nin veritabanı tarafından oluşturulması yerine uygulama tarafından sağlandığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="9005b-258">Varsayılan olarak, EF Core PK değerlerinin veritabanı tarafından oluşturulduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="9005b-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="9005b-259">Veritabanı tarafından oluşturulan genellikle en iyi yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="9005b-260">Varlıklar `Course` için Kullanıcı PK 'yi belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="9005b-261">Örneğin, matematik departmanı için 1000 serisi, Ingilizce departmanı için 2000 serisi gibi bir kurs numarası.</span><span class="sxs-lookup"><span data-stu-id="9005b-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="9005b-262">`DatabaseGenerated` Öznitelik varsayılan değerler oluşturmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="9005b-263">Örneğin, veritabanı bir satırın oluşturulduğu veya güncelleştirildiği tarihi kaydetmek için otomatik olarak bir tarih alanı oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="9005b-264">Daha fazla bilgi için bkz. [üretilen Özellikler](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="9005b-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-265">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-266">`Course` Varlıktaki yabancı anahtar (FK) özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="9005b-267">Bir kurs bir departmana atanır, bu nedenle bir `DepartmentID` FK ve bir `Department` gezinti özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="9005b-268">Bir kurs, kayıtlı sayıda öğrenciye sahip olabilir, bu nedenle `Enrollments` gezinti özelliği bir koleksiyondur:</span><span class="sxs-lookup"><span data-stu-id="9005b-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="9005b-269">Kurs, birden fazla eğitmen tarafından tada olabilir, bu nedenle `CourseAssignments` gezinti özelliği bir koleksiyon olur:</span><span class="sxs-lookup"><span data-stu-id="9005b-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="9005b-270">`CourseAssignment`[daha sonra](#many-to-many-relationships)açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9005b-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="9005b-271">Departman varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-271">The Department entity</span></span>

![Bölüm varlığı](complex-data-model/_static/department-entity.png)

<span data-ttu-id="9005b-273">Aşağıdaki kodla *modeller/departman. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="9005b-274">Column özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-274">The Column attribute</span></span>

<span data-ttu-id="9005b-275">Daha önce `Column` öznitelik, sütun adı eşlemesini değiştirmek için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="9005b-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="9005b-276">`Department` Varlığın kodunda`Column` , özniteliği SQL veri türü eşlemesini değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="9005b-277">`Budget` Sütun, veritabanında SQL Server para türü kullanılarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="9005b-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="9005b-278">Sütun eşlemesi genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-278">Column mapping is generally not required.</span></span> <span data-ttu-id="9005b-279">EF Core, özelliğin CLR türüne göre uygun SQL Server veri türünü seçer.</span><span class="sxs-lookup"><span data-stu-id="9005b-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="9005b-280">CLR `decimal` türü SQL Server `decimal` bir türe eşlenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="9005b-281">`Budget`para birimi için, para veri türü ise para birimi için daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="9005b-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-282">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-283">FK ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="9005b-284">Bir departman yönetici olabilir veya olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="9005b-285">Yönetici her zaman bir eğitmendir.</span><span class="sxs-lookup"><span data-stu-id="9005b-285">An administrator is always an instructor.</span></span> <span data-ttu-id="9005b-286">Bu nedenle, `Instructor` özelliği varlığa FK olarak dahil edilir. `InstructorID`</span><span class="sxs-lookup"><span data-stu-id="9005b-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="9005b-287">Gezinti özelliği adlandırılır `Administrator` ancak bir `Instructor` varlık barındırır:</span><span class="sxs-lookup"><span data-stu-id="9005b-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="9005b-288">Önceki koddaki soru işareti (?) özelliği null yapılabilir olduğunu belirtiyor.</span><span class="sxs-lookup"><span data-stu-id="9005b-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="9005b-289">Bir departmanın birçok kursu olabilir, bu nedenle bir kurs gezintisi özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="9005b-290">Kural gereği EF Core, null yapılamayan ve çok-çok ilişkiler için basamaklı silme imkanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="9005b-291">Bu varsayılan davranış, dairesel basamaklı silme kurallarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="9005b-292">Bir geçiş eklendiğinde dairesel basamaklı silme kuralları bir özel duruma neden olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="9005b-293">Örneğin, `Department.InstructorID` özellik null yapılamayan olarak tanımlanmışsa EF Core basamaklı silme kuralı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="9005b-294">Bu durumda, yönetici olarak atanan eğitmen silindiğinde departman silinir.</span><span class="sxs-lookup"><span data-stu-id="9005b-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="9005b-295">Bu senaryoda kısıtlama kuralı daha anlamlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="9005b-296">Aşağıdaki Fluent API bir kısıtlama kuralı ayarlar ve art arda silmeyi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9005b-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="9005b-297">Kayıt varlık</span><span class="sxs-lookup"><span data-stu-id="9005b-297">The Enrollment entity</span></span>

<span data-ttu-id="9005b-298">Kayıt kaydı, tek bir öğrenci tarafından gerçekleştirilen bir kurs içindir.</span><span class="sxs-lookup"><span data-stu-id="9005b-298">An enrollment record is for one course taken by one student.</span></span>

![Kayıt varlığı](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="9005b-300">*Modelleri/kaydı. cs* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-301">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-302">FK özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="9005b-303">Kayıt kaydı tek bir kurs için olduğundan, `CourseID` FK özelliği ve bir `Course` gezinti özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="9005b-304">Kayıt kaydı bir öğrenci içindir, bu nedenle bir `StudentID` FK özelliği ve bir `Student` gezinti özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="9005b-305">Çoktan çoğa Ilişkiler</span><span class="sxs-lookup"><span data-stu-id="9005b-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="9005b-306">`Student` Ve`Course` varlıkları arasında çoktan çoğa bir ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="9005b-307">Varlık `Enrollment` , veritabanında *Yük içeren* çoktan çoğa bir JOIN tablosu olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="9005b-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="9005b-308">"Yükle", `Enrollment` tablonun birleştirilmiş tablolar için FKS 'ler (Bu durumda, PK ve `Grade`) gibi ek veriler içerdiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9005b-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="9005b-309">Aşağıdaki çizimde bu ilişkilerin bir varlık diyagramında nasıl göründüğünü gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="9005b-310">(Bu diyagram EF 6. x için [EF güç araçları](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="9005b-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="9005b-311">Diyagram oluşturmak öğreticinin bir parçası değildir.)</span><span class="sxs-lookup"><span data-stu-id="9005b-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Öğrenci-çok fazla ilişki](complex-data-model/_static/student-course.png)

<span data-ttu-id="9005b-313">Her ilişki ucu ve bir yıldız işareti (\*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.</span><span class="sxs-lookup"><span data-stu-id="9005b-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="9005b-314">Tablo, sınıf bilgileri içermiyorsa, yalnızca iki FKS (`CourseID` ve `StudentID`) içermesi gerekir. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="9005b-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="9005b-315">Yük olmadan çoktan çoğa bir JOIN tablosu bazen saf JOIN tablosu (PJT) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="9005b-316">`Instructor` Ve`Course` varlıklarının saf bir JOIN tablosu kullanılarak çoktan çoğa bir ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="9005b-317">Not: EF 6. x, çoktan çoğa ilişkiler için örtük birleştirmeyi destekler, ancak EF Core değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="9005b-318">Daha fazla bilgi için [EF Core 2,0 ' de çoktan çoğa ilişkiler](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="9005b-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="9005b-319">Courseatama varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-319">The CourseAssignment entity</span></span>

![Courseatama varlığı](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="9005b-321">Aşağıdaki kodla *modeller/Courseatama. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="9005b-322">Eğitmenden çok-çok ilişkisi için bir JOIN tablosu gerekir ve bu ekleme tablosu için varlık kurs atamasıdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="9005b-324">Bir JOIN varlığına `EntityName1EntityName2`ad vermek yaygındır.</span><span class="sxs-lookup"><span data-stu-id="9005b-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="9005b-325">Örneğin, bu model kullanılarak eğitmen-kurslar 'a katılması tablosu olacaktır `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="9005b-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="9005b-326">Ancak, ilişkiyi açıklayan bir ad kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="9005b-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="9005b-327">Veri modelleri basit ve büyümeye başlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-327">Data models start out simple and grow.</span></span> <span data-ttu-id="9005b-328">Yük (PJTs) olmayan ekleme tabloları genellikle yükü içerecek şekilde gelişmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="9005b-329">Açıklayıcı bir varlık adıyla başlayarak, ekleme tablosu değiştiğinde adın değiştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="9005b-330">İdeal olarak, JOIN varlığının iş etki alanında kendi doğal (muhtemelen tek bir kelime) adına sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="9005b-331">Örneğin, kitaplar ve müşteriler, derecelendirmeler adlı bir JOIN varlığıyla bağlantı kurulabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="9005b-332">Eğitmenin kursa çok-çok ilişkisi `CourseAssignment` için `CourseInstructor`tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="9005b-333">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="9005b-333">Composite key</span></span>

<span data-ttu-id="9005b-334">( `CourseAssignment` `InstructorID` Ve )`CourseID`içindekiiki FKS, tablonunhersatırınıbenzersiz`CourseAssignment` bir şekilde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="9005b-335">`CourseAssignment`adanmış bir PK gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="9005b-336">`InstructorID` Ve`CourseID` özellikleri bileşik bir PK olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="9005b-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="9005b-337">EF Core bileşik PKs 'leri belirtmenin tek yolu *Fluent API*' dir.</span><span class="sxs-lookup"><span data-stu-id="9005b-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="9005b-338">Sonraki bölümde, bileşik PK 'nin nasıl yapılandırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="9005b-339">Bileşik anahtar şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9005b-339">The composite key ensures that:</span></span>

* <span data-ttu-id="9005b-340">Tek bir kurs için birden çok satıra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="9005b-341">Birden çok satıra bir eğitmen için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="9005b-342">Aynı eğitmen ve kurs için birden çok satıra izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="9005b-343">`Enrollment` JOIN varlığı kendi PK 'yi tanımlar, bu nedenle bu sıralamanın yinelemeleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9005b-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="9005b-344">Bu tür yinelemeleri engellemek için:</span><span class="sxs-lookup"><span data-stu-id="9005b-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="9005b-345">FK alanlara benzersiz bir dizin ekleyin veya</span><span class="sxs-lookup"><span data-stu-id="9005b-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="9005b-346">`Enrollment` İle`CourseAssignment`benzer bir birincil bileşik anahtarla yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="9005b-347">Daha fazla bilgi için bkz. [dizinler](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="9005b-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="9005b-348">Veritabanı bağlamını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="9005b-348">Update the database context</span></span>

<span data-ttu-id="9005b-349">*Data/SchoolContext. cs* öğesini şu kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="9005b-350">Yukarıdaki kod, yeni varlıkları ekler ve `CourseAssignment` varlığın bileşik PK 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="9005b-351">Tutarlı API 'nin özniteliklere alternatif</span><span class="sxs-lookup"><span data-stu-id="9005b-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="9005b-352">Önceki koddaki yöntemi EF Core davranışını yapılandırmak için Fluent API kullanır. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="9005b-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="9005b-353">Genellikle tek bir bildirimde bir dizi yöntem çağrısı olan dize olarak kullanıldığından, API "akıcı" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="9005b-354">[Aşağıdaki kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) Fluent API bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="9005b-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="9005b-355">Bu öğreticide, Fluent API yalnızca özniteliklerle yapılamadığını veritabanı eşlemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="9005b-356">Ancak Fluent API, özniteliklerle yapılabilecek biçimlendirme, doğrulama ve eşleme kurallarının çoğunu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="9005b-357">Gibi bazı öznitelikler `MinimumLength` Fluent API uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="9005b-358">`MinimumLength`şemayı değiştirmez, yalnızca bir minimum uzunluk doğrulama kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="9005b-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="9005b-359">Bazı geliştiriciler, varlık sınıflarının "temiz" olmasını sağlamak için Fluent API özel olarak kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="9005b-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="9005b-360">Öznitelikler ve Fluent API karışık olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="9005b-361">Yalnızca Fluent API (bileşik bir PK belirterek) yapılabilecek bazı konfigürasyonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="9005b-362">Yalnızca özniteliklerle (`MinimumLength`) yapılabilecek bazı konfigürasyonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="9005b-363">Fluent API veya özniteliklerini kullanmak için önerilen uygulama:</span><span class="sxs-lookup"><span data-stu-id="9005b-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="9005b-364">Bu iki yaklaşımdan birini seçin.</span><span class="sxs-lookup"><span data-stu-id="9005b-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="9005b-365">Seçilen yaklaşımı mümkün olduğunca düzenli olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="9005b-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="9005b-366">Bu öğreticide kullanılan özniteliklerin bazıları için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9005b-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="9005b-367">Yalnızca doğrulama (örneğin, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="9005b-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="9005b-368">Yalnızca yapılandırma EF Core (örneğin, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="9005b-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="9005b-369">Doğrulama ve EF Core yapılandırma (örneğin, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="9005b-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="9005b-370">Öznitelikler ile Fluent API hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="9005b-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="9005b-371">Varlık diyagramı</span><span class="sxs-lookup"><span data-stu-id="9005b-371">Entity diagram</span></span>

<span data-ttu-id="9005b-372">Aşağıdaki çizimde, tamamlanmış okul modeli için EF Power Tools 'un oluştura diyagramı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="9005b-374">Önceki diyagramda şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="9005b-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="9005b-375">Birden çok çoktan çoğa ilişki satırı (1 ile \*).</span><span class="sxs-lookup"><span data-stu-id="9005b-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="9005b-376">`Instructor` Ve`OfficeAssignment` varlıkları arasında bire sıfır veya-bir ilişki çizgisi (1 ila 0.. 1).</span><span class="sxs-lookup"><span data-stu-id="9005b-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="9005b-377">`Instructor` Ve`Department` varlıkları arasında sıfır veya-bire çok ilişki çizgisi (0.. 1-\*).</span><span class="sxs-lookup"><span data-stu-id="9005b-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="9005b-378">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="9005b-378">Seed the database</span></span>

<span data-ttu-id="9005b-379">*Data/Dbınizer. cs*dosyasındaki kodu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="9005b-380">Yukarıdaki kod, yeni varlıklar için tohum verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="9005b-381">Bu kodun çoğu yeni varlık nesneleri oluşturur ve örnek verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="9005b-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="9005b-382">Örnek veriler test için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-382">The sample data is used for testing.</span></span> <span data-ttu-id="9005b-383">Birden `Enrollments` çok `CourseAssignments` -çok JOIN tablosunun nasıl çalıştırılabilir olduğunu gösteren örnekler için bkz. ve.</span><span class="sxs-lookup"><span data-stu-id="9005b-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="9005b-384">Geçiş Ekle</span><span class="sxs-lookup"><span data-stu-id="9005b-384">Add a migration</span></span>

<span data-ttu-id="9005b-385">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9005b-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9005b-387">PMC 'de aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="9005b-388">Yukarıdaki komut, olası veri kaybı hakkında bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9005b-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="9005b-389">`database update` Komut çalıştırıldığında, aşağıdaki hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="9005b-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="9005b-390">Sonraki bölümde, bu hatayla ilgili ne yapılacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9005b-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9005b-392">Bir geçiş ekler ve `database update` komutu çalıştırırsanız, aşağıdaki hata üretilir:</span><span class="sxs-lookup"><span data-stu-id="9005b-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="9005b-393">Sonraki bölümde, bu hatanın nasıl önleneceğini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9005b-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="9005b-394">Geçişi uygulayın veya bırakıp yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="9005b-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="9005b-395">Artık var olan bir veritabanınız olduğuna göre, değişikliklere nasıl uygulanacağını düşünmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="9005b-396">Bu öğreticide iki alternatif gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9005b-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="9005b-397">[Veritabanını bırakıp yeniden oluşturun](#drop).</span><span class="sxs-lookup"><span data-stu-id="9005b-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="9005b-398">SQLite kullanıyorsanız bu bölümü seçin.</span><span class="sxs-lookup"><span data-stu-id="9005b-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="9005b-399">[Geçişi mevcut veritabanına uygulayın](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="9005b-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="9005b-400">Bu bölümdeki yönergeler yalnızca SQL Server için geçerlidir, **SQLite için değildir**.</span><span class="sxs-lookup"><span data-stu-id="9005b-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="9005b-401">Her iki seçenek de SQL Server için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9005b-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="9005b-402">Apply-Migration yöntemi daha karmaşıktır ve zaman alabilir. Bu, gerçek dünyada üretim ortamları için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="9005b-403">Veritabanını bırakıp yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="9005b-403">Drop and re-create the database</span></span>

<span data-ttu-id="9005b-404">SQL Server kullanıyorsanız ve aşağıdaki bölümde uygulanacak geçiş yaklaşımını yapmak istiyorsanız [Bu bölümü atlayın](#apply-the-migration) .</span><span class="sxs-lookup"><span data-stu-id="9005b-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="9005b-405">Yeni bir veritabanı oluşturmak için EF Core zorlamak için veritabanını bırakıp güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9005b-407">**Paket Yöneticisi konsolunda** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="9005b-408">*Geçişler* klasörünü silin, ardından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-409">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9005b-410">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="9005b-411">Proje klasörü *Contosouniversity. csproj* dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="9005b-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="9005b-412">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="9005b-413">*Geçişler* klasörünü silin, ardından aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="9005b-414">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-414">Run the app.</span></span> <span data-ttu-id="9005b-415">Uygulamayı çalıştırmak `DbInitializer.Initialize` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="9005b-416">Yeni `DbInitializer.Initialize` veritabanını doldurur.</span><span class="sxs-lookup"><span data-stu-id="9005b-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9005b-418">Veritabanını SSOX içinde açın:</span><span class="sxs-lookup"><span data-stu-id="9005b-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="9005b-419">Daha önce SSOX açıldıysa **Yenile** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="9005b-420">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="9005b-420">Expand the **Tables** node.</span></span> <span data-ttu-id="9005b-421">Oluşturulan tablolar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-421">The created tables are displayed.</span></span>

  ![SSOX içindeki tablolar](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="9005b-423">**Courseatama** tablosunu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="9005b-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="9005b-424">**Courseatama** tablosuna sağ tıklayın ve **verileri görüntüle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="9005b-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="9005b-425">**Courseatama** tablosunun veri içerdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![SSOX 'te Courseatama verileri](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-427">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9005b-428">Veritabanını incelemek için SQLite aracınızı kullanın:</span><span class="sxs-lookup"><span data-stu-id="9005b-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="9005b-429">Yeni tablolar ve sütunlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-429">New tables and columns.</span></span>
* <span data-ttu-id="9005b-430">Tablolardaki verileri, örneğin, **Courseatama** tablosu.</span><span class="sxs-lookup"><span data-stu-id="9005b-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="9005b-431">Geçişi Uygula</span><span class="sxs-lookup"><span data-stu-id="9005b-431">Apply the migration</span></span>

<span data-ttu-id="9005b-432">Bu bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-432">This section is optional.</span></span> <span data-ttu-id="9005b-433">Bu adımlar yalnızca SQL Server LocalDB için çalışır ve yalnızca önceki [bırakma ve veritabanını yeniden oluştur](#drop) bölümünü atladıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="9005b-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="9005b-434">Geçişler mevcut verilerle çalıştırıldığında, mevcut verilerin karşılanmadığı FK kısıtlamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="9005b-435">Üretim verileriyle, mevcut verilerin geçirilmesi için adımların alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="9005b-436">Bu bölüm, FK kısıtlama ihlallerinin düzeltilmesiyle bir örnek sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="9005b-437">Bu kod değişikliklerini yedekleme olmadan yapmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="9005b-438">Önceki [bırakmayı tamamlayıp veritabanını yeniden oluşturduktan sonra](#drop) Bu kod değişikliklerini yapmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="9005b-439">*{Timestamp} _ComplexDataModel. cs* dosyası aşağıdaki kodu içerir:</span><span class="sxs-lookup"><span data-stu-id="9005b-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="9005b-440">Yukarıdaki kod, `DepartmentID` `Course` tabloya null yapılamayan bir FK ekler.</span><span class="sxs-lookup"><span data-stu-id="9005b-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="9005b-441">Önceki öğreticideki veritabanı, içindeki `Course`satırları içerir, böylece tablo geçişler tarafından güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="9005b-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="9005b-442">`ComplexDataModel` Geçişin mevcut verilerle çalışmasını sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="9005b-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="9005b-443">Yeni sütuna (`DepartmentID`) varsayılan değer vermek için kodu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9005b-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="9005b-444">Varsayılan departman olarak davranacak "Temp" adlı sahte bir departman oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9005b-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="9005b-445">Yabancı anahtar kısıtlamalarını çözme</span><span class="sxs-lookup"><span data-stu-id="9005b-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="9005b-446">Geçiş sınıfında, `Up` yöntemi güncelleştirin: `ComplexDataModel`</span><span class="sxs-lookup"><span data-stu-id="9005b-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="9005b-447">*{Timestamp} _ComplexDataModel. cs* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="9005b-448">`DepartmentID` Sütunu `Course` tabloya ekleyen kod satırını açıklama olarak yapın.</span><span class="sxs-lookup"><span data-stu-id="9005b-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="9005b-449">Aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9005b-449">Add the following highlighted code.</span></span> <span data-ttu-id="9005b-450">Yeni kod `.CreateTable( name: "Department"` bloğundan sonra geçer:</span><span class="sxs-lookup"><span data-stu-id="9005b-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="9005b-451">[! Code-CSharp [] (giriş/örnekler/cu30snapshots/5-Complex/geçişler/ComplexDataModel. cs? Name = snippet_CreateDefaultValue & vurgulama = 23-31)]</span><span class="sxs-lookup"><span data-stu-id="9005b-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="9005b-452">Önceki değişikliklerle, varolan `Course` satırlar, `ComplexDataModel.Up` Yöntem çalıştıktan sonra "geçici" departmanıyla ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="9005b-453">Burada gösterilen durumu işlemenin yolu, bu öğretici için basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="9005b-454">Bir üretim uygulaması şöyle olacaktır:</span><span class="sxs-lookup"><span data-stu-id="9005b-454">A production app would:</span></span>

* <span data-ttu-id="9005b-455">Yeni `Course` `Department` satırlarasatırlarveilgilisatırlareklemekiçinkodveyakomutdosyalarıekleyin.`Department`</span><span class="sxs-lookup"><span data-stu-id="9005b-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="9005b-456">İçin `Course.DepartmentID`"geçici" Departmanı veya varsayılan değeri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9005b-458">**Paket Yöneticisi konsolunda** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="9005b-459">`DbInitializer.Initialize` Yöntemi yalnızca boş bir veritabanıyla çalışacak şekilde tasarlandığından, öğrenci ve kurs tablolarındaki tüm satırları silmek için ssox kullanın.</span><span class="sxs-lookup"><span data-stu-id="9005b-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="9005b-460">(Cascade silme, kayıt tablosundan işlem gerçekleştirir.)</span><span class="sxs-lookup"><span data-stu-id="9005b-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-461">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9005b-462">Visual Studio Code ile SQL Server LocalDB kullanıyorsanız şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="9005b-463">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-463">Run the app.</span></span> <span data-ttu-id="9005b-464">Uygulamayı çalıştırmak `DbInitializer.Initialize` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="9005b-465">Yeni `DbInitializer.Initialize` veritabanını doldurur.</span><span class="sxs-lookup"><span data-stu-id="9005b-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9005b-466">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9005b-466">Next steps</span></span>

<span data-ttu-id="9005b-467">Sonraki iki öğretici ilgili verilerin nasıl okunacağını ve güncelleştirilmesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9005b-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9005b-468">[Önceki öğretici](xref:data/ef-rp/migrations)
> [sonraki öğretici](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="9005b-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9005b-469">Önceki öğreticiler, üç varlıktan oluşan temel bir veri modeliyle çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="9005b-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="9005b-470">Bu öğreticide:</span><span class="sxs-lookup"><span data-stu-id="9005b-470">In this tutorial:</span></span>

* <span data-ttu-id="9005b-471">Daha fazla varlık ve ilişki eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="9005b-472">Veri modeli biçimlendirme, doğrulama ve veritabanı eşleme kuralları belirtilerek özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="9005b-473">Tamamlanan veri modeli için varlık sınıfları aşağıdaki çizimde gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9005b-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="9005b-475">Çözemediğiniz sorunlarla karşılaşırsanız, [tamamlanmış uygulamayı](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)indirin.</span><span class="sxs-lookup"><span data-stu-id="9005b-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="9005b-476">Veri modelini özniteliklerle özelleştirme</span><span class="sxs-lookup"><span data-stu-id="9005b-476">Customize the data model with attributes</span></span>

<span data-ttu-id="9005b-477">Bu bölümde, veri modeli öznitelikler kullanılarak özelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="9005b-478">DataType özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-478">The DataType attribute</span></span>

<span data-ttu-id="9005b-479">Öğrenci sayfaları Şu anda kayıt tarihinin saatini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9005b-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="9005b-480">Genellikle, tarih alanları saati değil yalnızca tarihi gösterir.</span><span class="sxs-lookup"><span data-stu-id="9005b-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="9005b-481">*Modelleri/öğrenci. cs* 'yi aşağıdaki vurgulanmış kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="9005b-482">[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) özniteliği, veritabanı iç türünden daha belirgin bir veri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="9005b-483">Bu durumda, tarih ve saat değil yalnızca tarih görüntülenmelidir.</span><span class="sxs-lookup"><span data-stu-id="9005b-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="9005b-484">Veri [türü numaralandırması](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) , tarih, saat, PhoneNumber, para birimi, emaadresi vb. gibi birçok veri türü sağlar. `DataType` Özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="9005b-485">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9005b-485">For example:</span></span>

* <span data-ttu-id="9005b-486">`mailto:` Bağlantı için`DataType.EmailAddress`otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9005b-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="9005b-487">Tarih Seçici çoğu tarayıcıda için `DataType.Date` verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9005b-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="9005b-488">Özniteliği HTML 5 tarayıcılarının kullandığı `data-` HTML 5 (bir veri Dash) özniteliklerini yayar. `DataType`</span><span class="sxs-lookup"><span data-stu-id="9005b-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="9005b-489">`DataType` Öznitelikler doğrulama sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="9005b-490">`DataType.Date`görüntülenen tarihin biçimini belirtmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="9005b-491">Varsayılan olarak, Tarih alanı sunucunun [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)öğesine göre varsayılan biçimlere göre görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="9005b-492">`DisplayFormat` Öznitelik, tarih biçimini açıkça belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9005b-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="9005b-493">`ApplyFormatInEditMode` Ayar, biçimlendirmenin düzenleme kullanıcı arabirimine de uygulanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="9005b-494">Bazı alanlar kullanmamanız `ApplyFormatInEditMode`gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="9005b-495">Örneğin, para birimi simgesi genellikle bir düzenleme metin kutusunda gösterilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9005b-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="9005b-496">`DisplayFormat` Özniteliği kendi başına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="9005b-497">Özniteliği `DataType` özniteliği`DisplayFormat` ile kullanmak genellikle iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="9005b-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="9005b-498">`DataType` Özniteliği, bir ekranda nasıl işleneceğini değil, verilerin semantiğini alır.</span><span class="sxs-lookup"><span data-stu-id="9005b-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="9005b-499">Özniteliği `DataType` , içinde `DisplayFormat`kullanılamayan aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9005b-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="9005b-500">Tarayıcı HTML5 özelliklerini etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="9005b-501">Örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını, istemci tarafı giriş doğrulamasını vb. göster</span><span class="sxs-lookup"><span data-stu-id="9005b-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="9005b-502">Varsayılan olarak tarayıcı, verileri yerel ayara göre doğru biçimi kullanarak işler.</span><span class="sxs-lookup"><span data-stu-id="9005b-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="9005b-503">Daha fazla bilgi için bkz [ \<. Giriş > etiketi Yardımcısı belgeleri](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9005b-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="9005b-504">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-504">Run the app.</span></span> <span data-ttu-id="9005b-505">Öğrenciler dizin sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="9005b-506">Süreler artık görüntülenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9005b-506">Times are no longer displayed.</span></span> <span data-ttu-id="9005b-507">`Student` Modeli kullanan her görünüm tarihi zaman içinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9005b-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Öğrenciler Dizin sayfası tarihleri zamansız gösterme](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="9005b-509">StringLength özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-509">The StringLength attribute</span></span>

<span data-ttu-id="9005b-510">Veri doğrulama kuralları ve doğrulama hatası iletileri özniteliklerle belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="9005b-511">[StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) özniteliği, bir veri alanında izin verilen en düşük ve en fazla karakter uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="9005b-512">`StringLength` Öznitelik Ayrıca istemci tarafı ve sunucu tarafı doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="9005b-513">En küçük değerin veritabanı şeması üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="9005b-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="9005b-514">`Student` Modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="9005b-515">Yukarıdaki kod, adları 50 karakterden fazla olmayacak şekilde sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="9005b-516">Öznitelik `StringLength` , bir kullanıcının ad için boşluk girmesini engellemez.</span><span class="sxs-lookup"><span data-stu-id="9005b-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="9005b-517">[Cevap içerisinde RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) özniteliği, girişe kısıtlamalar uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="9005b-518">Örneğin, aşağıdaki kod ilk karakterin büyük küçük harf olmasını ve geri kalan karakterlerin alfabetik olmasını gerektirir:</span><span class="sxs-lookup"><span data-stu-id="9005b-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="9005b-519">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-519">Run the app:</span></span>

* <span data-ttu-id="9005b-520">Öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="9005b-521">**Yeni oluştur**' u seçin ve 50 karakterden daha uzun bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="9005b-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="9005b-522">**Oluştur**' u seçin, istemci tarafı doğrulama bir hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="9005b-522">Select **Create**, client-side validation shows an error message.</span></span>

![Öğrenciler Dizin sayfası dize uzunluğu hatalarını gösteriyor](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="9005b-524">**SQL Server Nesne Gezgini** (ssox) içinde **öğrenci** tablosuna çift tıklayarak öğrenci tablosu tasarımcısını açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Geçişle önce SSOX 'teki öğrenciler tablosu](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="9005b-526">Önceki görüntüde `Student` tablo için şema gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="9005b-527">Veritabanı üzerinde geçişler çalıştırılmadığından `nvarchar(MAX)` ad alanlarının türü var.</span><span class="sxs-lookup"><span data-stu-id="9005b-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="9005b-528">Bu öğreticide daha sonra geçişler çalıştırıldığında, ad alanları olur `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="9005b-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="9005b-529">Column özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-529">The Column attribute</span></span>

<span data-ttu-id="9005b-530">Öznitelikler sınıfların ve özelliklerin veritabanına nasıl eşlenildiğini denetleyebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="9005b-531">Bu bölümde, `Column` özniteliği, `FirstMidName` özelliğin adını DB 'deki "FirstName" olarak eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="9005b-532">DB oluşturulduğunda modeldeki Özellik adları sütun adları için kullanılır ( `Column` özniteliği kullanıldığı durumlar dışında).</span><span class="sxs-lookup"><span data-stu-id="9005b-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="9005b-533">Model, birinci `FirstMidName` ad alanı için kullanılır çünkü alan de bir orta ad içerebilir. `Student`</span><span class="sxs-lookup"><span data-stu-id="9005b-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="9005b-534">*Student.cs* dosyasını aşağıdaki vurgulanmış kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="9005b-535">Uygulamanın önceki değişikliği `Student.FirstMidName` ile `Student` tablonun `FirstName` sütunuyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="9005b-536">`Column` Özniteliği ekleme modeli, `SchoolContext`öğesini yedekleyen olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="9005b-537">' İ destekleyen `SchoolContext` model artık veritabanıyla eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="9005b-538">Geçiş uygulamadan önce uygulama çalışıyorsa aşağıdaki özel durum oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="9005b-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="9005b-539">DB 'yi güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="9005b-539">To update the DB:</span></span>

* <span data-ttu-id="9005b-540">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9005b-540">Build the project.</span></span>
* <span data-ttu-id="9005b-541">Proje klasöründe bir komut penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-541">Open a command window in the project folder.</span></span> <span data-ttu-id="9005b-542">Yeni bir geçiş oluşturmak ve DB 'yi güncelleştirmek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="9005b-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-544">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="9005b-545">`migrations add ColumnFirstName` Komut aşağıdaki uyarı iletisini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9005b-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="9005b-546">Ad alanları artık 50 karakterle sınırlı olduğundan uyarı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9005b-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="9005b-547">VERITABANıNDAKI bir ad 50 karakterden fazlasına sahipse, son karakter 51 ' i kaybedersiniz.</span><span class="sxs-lookup"><span data-stu-id="9005b-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="9005b-548">Uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="9005b-548">Test the app.</span></span>

<span data-ttu-id="9005b-549">Öğrenci tablosunu SSOX içinde açın:</span><span class="sxs-lookup"><span data-stu-id="9005b-549">Open the Student table in SSOX:</span></span>

![Geçişlerde SSOX 'teki öğrenciler tablosu](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="9005b-551">Geçiş uygulanmadan önce ad sütunları [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)türünde idi.</span><span class="sxs-lookup"><span data-stu-id="9005b-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="9005b-552">Ad sütunları artık `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="9005b-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="9005b-553">Sütun adı `FirstMidName` olarak `FirstName`değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="9005b-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="9005b-554">Aşağıdaki bölümde, uygulamanın bazı aşamalardan oluşturulması derleyici hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="9005b-555">Yönergeler uygulamanın ne zaman derbir olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="9005b-556">Öğrenci varlık güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="9005b-556">Student entity update</span></span>

![Öğrenci varlığı](complex-data-model/_static/student-entity.png)

<span data-ttu-id="9005b-558">*Modelleri/öğrenci. cs* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="9005b-559">Gerekli öznitelik</span><span class="sxs-lookup"><span data-stu-id="9005b-559">The Required attribute</span></span>

<span data-ttu-id="9005b-560">`Required` Özniteliği, ad özellikleri gerekli alanları yapar.</span><span class="sxs-lookup"><span data-stu-id="9005b-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="9005b-561">Öznitelik `Required` , değer türleri (`DateTime`, `int`, `double`vb.) gibi null yapılamayan türler için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="9005b-562">Null olmayan türler otomatik olarak gerekli alanlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="9005b-563">Öznitelik, `StringLength` özniteliğinde bir minimum length parametresiyle değiştirilebilir: `Required`</span><span class="sxs-lookup"><span data-stu-id="9005b-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="9005b-564">Display özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-564">The Display attribute</span></span>

<span data-ttu-id="9005b-565">`Display` Öznitelik, metin kutuları için başlığın "ad", "soyadı", "tam ad" ve "kayıt tarihi" olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="9005b-566">Varsayılan açıklamalı alt yazıların sözcükleri bölen bir boşluk yoktu, örneğin "LastName".</span><span class="sxs-lookup"><span data-stu-id="9005b-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="9005b-567">FullName hesaplanmış özelliği</span><span class="sxs-lookup"><span data-stu-id="9005b-567">The FullName calculated property</span></span>

<span data-ttu-id="9005b-568">`FullName`, iki diğer özelliğin bitiştirerek oluşturulmuş bir değer döndüren hesaplanmış bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9005b-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="9005b-569">`FullName`ayarlanamaz, yalnızca bir get erişimcisine sahip.</span><span class="sxs-lookup"><span data-stu-id="9005b-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="9005b-570">Veritabanında `FullName` hiçbir sütun oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="9005b-571">Eğitmen varlığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9005b-571">Create the Instructor Entity</span></span>

![Eğitmen varlığı](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="9005b-573">Aşağıdaki kodla *modeller/eğitmen. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="9005b-574">Birden çok öznitelik tek bir satırda olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="9005b-575">`HireDate` Öznitelikler aşağıdaki gibi yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="9005b-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="9005b-576">Courseatamalar ve OfficeAssignment gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="9005b-577">`CourseAssignments` Ve`OfficeAssignment` özellikleri gezinti özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="9005b-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="9005b-578">Bir eğitmen herhangi bir sayıda kurs öğretebilir, bu `CourseAssignments` nedenle bir koleksiyon olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9005b-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="9005b-579">Bir gezinti özelliği birden çok varlık tutuyorsa:</span><span class="sxs-lookup"><span data-stu-id="9005b-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="9005b-580">Girişlerin eklenebileceği, silinebileceği ve güncelleştirilebileceği bir liste türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="9005b-581">Gezinti özelliği türleri şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="9005b-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="9005b-582">Belirtilmişse, EF Core varsayılan olarak bir `HashSet<T>` koleksiyon oluşturur. `ICollection<T>`</span><span class="sxs-lookup"><span data-stu-id="9005b-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="9005b-583">`CourseAssignment` Varlık, çoktan çoğa ilişkilerin bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9005b-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="9005b-584">Contoso Üniversitesi iş kuralları, bir eğitmenin en fazla bir ofisiniz olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="9005b-585">`OfficeAssignment` Özelliği tek`OfficeAssignment` bir varlık içerir.</span><span class="sxs-lookup"><span data-stu-id="9005b-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="9005b-586">`OfficeAssignment`hiçbir Office atanmamışsa null olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="9005b-587">OfficeAssignment varlığını oluşturma</span><span class="sxs-lookup"><span data-stu-id="9005b-587">Create the OfficeAssignment entity</span></span>

![OfficeAssignment varlığı](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="9005b-589">Aşağıdaki kodla *modeller/OfficeAssignment. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="9005b-590">Anahtar özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-590">The Key attribute</span></span>

<span data-ttu-id="9005b-591">`[Key]` Öznitelik, özellik adı classnameıd veya ID dışında bir şey olduğunda, bir özelliği birincil anahtar (PK) olarak tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="9005b-592">`Instructor` Ve`OfficeAssignment` varlıkları arasında bire sıfır veya-bir ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="9005b-593">Office ataması, atandığı eğitmenle ilişkili olarak yalnızca vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="9005b-594">PK Ayrıca `Instructor` varlığa ait yabancı anahtardır (FK). `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="9005b-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="9005b-595">EF Core, `InstructorID` `OfficeAssignment` şu nedenle otomatik olarak tanıyamaz:</span><span class="sxs-lookup"><span data-stu-id="9005b-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="9005b-596">`InstructorID`ID veya Classnameıd adlandırma kuralını takip etmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="9005b-597">Bu nedenle, `Key` özniteliği PK olarak tanımlamak `InstructorID` için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9005b-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="9005b-598">Varsayılan olarak, EF Core, sütun tanımlayıcı bir ilişki için olduğundan, anahtarı veritabanı olmayan bir şekilde değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="9005b-599">Eğitmen gezintisi özelliği</span><span class="sxs-lookup"><span data-stu-id="9005b-599">The Instructor navigation property</span></span>

<span data-ttu-id="9005b-600">Varlık için gezinti özelliği null yapılabilir çünkü: `OfficeAssignment` `Instructor`</span><span class="sxs-lookup"><span data-stu-id="9005b-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="9005b-601">Başvuru türleri (örneğin, sınıflar Nullable).</span><span class="sxs-lookup"><span data-stu-id="9005b-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="9005b-602">Bir eğitmenin Office ataması olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="9005b-603">Varlık null atanamaz `Instructor` bir gezinti özelliğine sahip, çünkü: `OfficeAssignment`</span><span class="sxs-lookup"><span data-stu-id="9005b-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="9005b-604">`InstructorID`null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="9005b-605">Bir Office ataması, bir eğitmen olmadan bulunamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="9005b-606">Bir `Instructor` varlık ilişkili `OfficeAssignment` bir varlığa sahip olduğunda, her varlığın gezinti özelliğinde diğer bir başvurusu vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="9005b-607">Öznitelik, `Instructor` gezinti özelliğine uygulanabilir: `[Required]`</span><span class="sxs-lookup"><span data-stu-id="9005b-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="9005b-608">Yukarıdaki kod, ilgili bir eğitmen olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="9005b-609">`InstructorID` Yabancı anahtar (aynı zamanda PK) null değer atanamaz olduğundan, yukarıdaki kod gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="9005b-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="9005b-610">Kurs varlığını değiştirme</span><span class="sxs-lookup"><span data-stu-id="9005b-610">Modify the Course Entity</span></span>

![Kurs varlığı](complex-data-model/_static/course-entity.png)

<span data-ttu-id="9005b-612">*Modelleri/kursu. cs* aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="9005b-613">Varlığın yabancı anahtar (FK) özelliği `DepartmentID`vardır. `Course`</span><span class="sxs-lookup"><span data-stu-id="9005b-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="9005b-614">`DepartmentID`ilgili `Department` varlığa işaret eder.</span><span class="sxs-lookup"><span data-stu-id="9005b-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="9005b-615">`Course` Varlığın bir`Department` gezinti özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="9005b-616">EF Core, modelin ilgili bir varlık için gezinti özelliği olduğunda bir veri modeli için FK özelliği gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="9005b-617">EF Core, gerektiği yerde otomatik olarak veritabanında FKs 'ler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="9005b-618">EF Core otomatik olarak oluşturulan FKs 'ler için [gölge Özellikler](/ef/core/modeling/shadow-properties) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9005b-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="9005b-619">FK 'in veri modelinde olması, güncelleştirmeleri daha basit ve daha verimli hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="9005b-620">Örneğin, FK özelliğinin `DepartmentID` dahil *olmadığı* bir model düşünün.</span><span class="sxs-lookup"><span data-stu-id="9005b-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="9005b-621">Bir kurs varlığı düzenlemek üzere getirilirken:</span><span class="sxs-lookup"><span data-stu-id="9005b-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="9005b-622">Açık bir şekilde yüklenmediyse varlıknullolur.`Department`</span><span class="sxs-lookup"><span data-stu-id="9005b-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="9005b-623">Kurs varlığını `Department` güncelleştirmek için önce varlığın getirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="9005b-624">FK özelliği `DepartmentID` veri modeline dahil edildiğinde, bir güncelleştirmeden önce `Department` varlığı getirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="9005b-625">DatabaseGenerated özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="9005b-626">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Özniteliği, PK 'nin veritabanı tarafından oluşturulması yerine uygulama tarafından sağlandığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="9005b-627">Varsayılan olarak, EF Core PK değerlerinin DB tarafından oluşturulduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="9005b-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="9005b-628">VERITABANı tarafından oluşturulan PK değerleri genellikle en iyi yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="9005b-629">Varlıklar `Course` için Kullanıcı PK 'yi belirtir.</span><span class="sxs-lookup"><span data-stu-id="9005b-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="9005b-630">Örneğin, matematik departmanı için 1000 serisi, Ingilizce departmanı için 2000 serisi gibi bir kurs numarası.</span><span class="sxs-lookup"><span data-stu-id="9005b-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="9005b-631">`DatabaseGenerated` Öznitelik varsayılan değerler oluşturmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="9005b-632">Örneğin, VERITABANı bir satırın oluşturulduğu veya güncelleştirildiği tarihi kaydetmek için otomatik olarak bir tarih alanı oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="9005b-633">Daha fazla bilgi için bkz. [üretilen Özellikler](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="9005b-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-634">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-635">`Course` Varlıktaki yabancı anahtar (FK) özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="9005b-636">Bir kurs bir departmana atanır, bu nedenle bir `DepartmentID` FK ve bir `Department` gezinti özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="9005b-637">Bir kurs, kayıtlı sayıda öğrenciye sahip olabilir, bu nedenle `Enrollments` gezinti özelliği bir koleksiyondur:</span><span class="sxs-lookup"><span data-stu-id="9005b-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="9005b-638">Kurs, birden fazla eğitmen tarafından tada olabilir, bu nedenle `CourseAssignments` gezinti özelliği bir koleksiyon olur:</span><span class="sxs-lookup"><span data-stu-id="9005b-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="9005b-639">`CourseAssignment`[daha sonra](#many-to-many-relationships)açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9005b-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="9005b-640">Departman varlığı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9005b-640">Create the Department entity</span></span>

![Bölüm varlığı](complex-data-model/_static/department-entity.png)

<span data-ttu-id="9005b-642">Aşağıdaki kodla *modeller/departman. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="9005b-643">Column özniteliği</span><span class="sxs-lookup"><span data-stu-id="9005b-643">The Column attribute</span></span>

<span data-ttu-id="9005b-644">Daha önce `Column` öznitelik, sütun adı eşlemesini değiştirmek için kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="9005b-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="9005b-645">`Department` Varlığın kodunda`Column` , özniteliği SQL veri türü eşlemesini değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="9005b-646">`Budget` Sütun, veritabanında SQL Server para türü kullanılarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="9005b-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="9005b-647">Sütun eşlemesi genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-647">Column mapping is generally not required.</span></span> <span data-ttu-id="9005b-648">EF Core genellikle özelliğin CLR türüne göre uygun SQL Server veri türünü seçer.</span><span class="sxs-lookup"><span data-stu-id="9005b-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="9005b-649">CLR `decimal` türü SQL Server `decimal` bir türe eşlenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="9005b-650">`Budget`para birimi için, para veri türü ise para birimi için daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="9005b-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-651">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-652">FK ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="9005b-653">Bir departman yönetici olabilir veya olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="9005b-654">Yönetici her zaman bir eğitmendir.</span><span class="sxs-lookup"><span data-stu-id="9005b-654">An administrator is always an instructor.</span></span> <span data-ttu-id="9005b-655">Bu nedenle, `Instructor` özelliği varlığa FK olarak dahil edilir. `InstructorID`</span><span class="sxs-lookup"><span data-stu-id="9005b-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="9005b-656">Gezinti özelliği adlandırılır `Administrator` ancak bir `Instructor` varlık barındırır:</span><span class="sxs-lookup"><span data-stu-id="9005b-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="9005b-657">Önceki koddaki soru işareti (?) özelliği null yapılabilir olduğunu belirtiyor.</span><span class="sxs-lookup"><span data-stu-id="9005b-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="9005b-658">Bir departmanın birçok kursu olabilir, bu nedenle bir kurs gezintisi özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="9005b-659">Not: Kural gereği EF Core, null yapılamayan ve çok-çok ilişkiler için basamaklı silme imkanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="9005b-660">Basamaklı silme, dairesel basamaklı silme kurallarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="9005b-661">Döngüsel basamaklı silme kuralları, bir geçiş eklendiğinde özel duruma neden olur.</span><span class="sxs-lookup"><span data-stu-id="9005b-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="9005b-662">Örneğin, `Department.InstructorID` özellik null yapılamayan olarak tanımlanmışsa:</span><span class="sxs-lookup"><span data-stu-id="9005b-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="9005b-663">EF Core, eğitmen silindiğinde departmanı silmek için bir basamakla silme kuralı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="9005b-664">Eğitmen silindiğinde departmanı silmek amaçlanan davranış değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="9005b-665">Aşağıdaki Fluent API Cascade yerine bir kısıtlama kuralı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="9005b-666">Yukarıdaki kod, departman-eğitmen ilişkisindeki basamaklı silmeyi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9005b-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="9005b-667">Kayıt varlığını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="9005b-667">Update the Enrollment entity</span></span>

<span data-ttu-id="9005b-668">Kayıt kaydı, tek bir öğrenci tarafından gerçekleştirilen bir kurs içindir.</span><span class="sxs-lookup"><span data-stu-id="9005b-668">An enrollment record is for one course taken by one student.</span></span>

![Kayıt varlığı](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="9005b-670">*Modelleri/kaydı. cs* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="9005b-671">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="9005b-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="9005b-672">FK özellikleri ve gezinti özellikleri aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="9005b-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="9005b-673">Kayıt kaydı tek bir kurs için olduğundan, `CourseID` FK özelliği ve bir `Course` gezinti özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="9005b-674">Kayıt kaydı bir öğrenci içindir, bu nedenle bir `StudentID` FK özelliği ve bir `Student` gezinti özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="9005b-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="9005b-675">Çoktan çoğa Ilişkiler</span><span class="sxs-lookup"><span data-stu-id="9005b-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="9005b-676">`Student` Ve`Course` varlıkları arasında çoktan çoğa bir ilişki vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="9005b-677">Varlık `Enrollment` , veritabanında *Yük içeren* çoktan çoğa bir JOIN tablosu olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="9005b-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="9005b-678">"Yükle", `Enrollment` tablonun birleştirilmiş tablolar için FKS 'ler (Bu durumda, PK ve `Grade`) gibi ek veriler içerdiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9005b-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="9005b-679">Aşağıdaki çizimde bu ilişkilerin bir varlık diyagramında nasıl göründüğünü gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="9005b-680">(Bu diyagram EF 6. x için [EF güç araçları](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) kullanılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="9005b-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="9005b-681">Diyagram oluşturmak öğreticinin bir parçası değildir.)</span><span class="sxs-lookup"><span data-stu-id="9005b-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Öğrenci-çok fazla ilişki](complex-data-model/_static/student-course.png)

<span data-ttu-id="9005b-683">Her ilişki ucu ve bir yıldız işareti (\*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.</span><span class="sxs-lookup"><span data-stu-id="9005b-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="9005b-684">Tablo, sınıf bilgileri içermiyorsa, yalnızca iki FKS (`CourseID` ve `StudentID`) içermesi gerekir. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="9005b-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="9005b-685">Yük olmadan çoktan çoğa bir JOIN tablosu bazen saf JOIN tablosu (PJT) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="9005b-686">`Instructor` Ve`Course` varlıklarının saf bir JOIN tablosu kullanılarak çoktan çoğa bir ilişkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="9005b-687">Not: EF 6. x, çoktan çoğa ilişkiler için örtük birleştirmeyi destekler, ancak EF Core değildir.</span><span class="sxs-lookup"><span data-stu-id="9005b-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="9005b-688">Daha fazla bilgi için [EF Core 2,0 ' de çoktan çoğa ilişkiler](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="9005b-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="9005b-689">Courseatama varlığı</span><span class="sxs-lookup"><span data-stu-id="9005b-689">The CourseAssignment entity</span></span>

![Courseatama varlığı](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="9005b-691">Aşağıdaki kodla *modeller/Courseatama. cs* oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9005b-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="9005b-692">Eğitmenden kurslar</span><span class="sxs-lookup"><span data-stu-id="9005b-692">Instructor-to-Courses</span></span>

![Eğitmenden kurslar M:d](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="9005b-694">Eğitmenin kurslardan çok-çok ilişkisi:</span><span class="sxs-lookup"><span data-stu-id="9005b-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="9005b-695">Bir varlık kümesiyle temsil etmelidir bir JOIN tablosu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9005b-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="9005b-696">, Saf bir JOIN tablosu (yük içermeyen tablo).</span><span class="sxs-lookup"><span data-stu-id="9005b-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="9005b-697">Bir JOIN varlığına `EntityName1EntityName2`ad vermek yaygındır.</span><span class="sxs-lookup"><span data-stu-id="9005b-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="9005b-698">Örneğin, bu model `CourseInstructor`kullanılarak eğitmen-kurslar 'a katılması tablosu.</span><span class="sxs-lookup"><span data-stu-id="9005b-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="9005b-699">Ancak, ilişkiyi açıklayan bir ad kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="9005b-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="9005b-700">Veri modelleri basit ve büyümeye başlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-700">Data models start out simple and grow.</span></span> <span data-ttu-id="9005b-701">Yük yükü dahil olmak üzere genellikle yük-yük birleştirmeleri (PJTs) gelişmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="9005b-702">Açıklayıcı bir varlık adıyla başlayarak, ekleme tablosu değiştiğinde adın değiştirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="9005b-703">İdeal olarak, JOIN varlığının iş etki alanında kendi doğal (muhtemelen tek bir kelime) adına sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="9005b-704">Örneğin, kitaplar ve müşteriler, derecelendirmeler adlı bir JOIN varlığıyla bağlantı kurulabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="9005b-705">Eğitmenin kursa çok-çok ilişkisi `CourseAssignment` için `CourseInstructor`tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="9005b-706">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="9005b-706">Composite key</span></span>

<span data-ttu-id="9005b-707">FKs null değer atanamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-707">FKs are not nullable.</span></span> <span data-ttu-id="9005b-708">( `CourseAssignment` `InstructorID` Ve )`CourseID`içindekiiki FKS, tablonunhersatırınıbenzersiz`CourseAssignment` bir şekilde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="9005b-709">`CourseAssignment`adanmış bir PK gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="9005b-710">`InstructorID` Ve`CourseID` özellikleri bileşik bir PK olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="9005b-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="9005b-711">EF Core bileşik PKs 'leri belirtmenin tek yolu *Fluent API*' dir.</span><span class="sxs-lookup"><span data-stu-id="9005b-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="9005b-712">Sonraki bölümde, bileşik PK 'nin nasıl yapılandırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="9005b-713">Bileşik anahtar şunları sağlar:</span><span class="sxs-lookup"><span data-stu-id="9005b-713">The composite key ensures:</span></span>

* <span data-ttu-id="9005b-714">Tek bir kurs için birden çok satıra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="9005b-715">Birden çok satıra bir eğitmen için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="9005b-716">Aynı eğitmen ve kurs için birden çok satıra izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="9005b-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="9005b-717">`Enrollment` JOIN varlığı kendi PK 'yi tanımlar, bu nedenle bu sıralamanın yinelemeleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9005b-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="9005b-718">Bu tür yinelemeleri engellemek için:</span><span class="sxs-lookup"><span data-stu-id="9005b-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="9005b-719">FK alanlara benzersiz bir dizin ekleyin veya</span><span class="sxs-lookup"><span data-stu-id="9005b-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="9005b-720">`Enrollment` İle`CourseAssignment`benzer bir birincil bileşik anahtarla yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="9005b-721">Daha fazla bilgi için bkz. [dizinler](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="9005b-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="9005b-722">DB bağlamını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="9005b-722">Update the DB context</span></span>

<span data-ttu-id="9005b-723">*Data/SchoolContext. cs*' ye aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9005b-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="9005b-724">Yukarıdaki kod, yeni varlıkları ekler ve `CourseAssignment` varlığın bileşik PK 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="9005b-725">Tutarlı API 'nin özniteliklere alternatif</span><span class="sxs-lookup"><span data-stu-id="9005b-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="9005b-726">Önceki koddaki yöntemi EF Core davranışını yapılandırmak için Fluent API kullanır. `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="9005b-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="9005b-727">Genellikle tek bir bildirimde bir dizi yöntem çağrısı olan dize olarak kullanıldığından, API "akıcı" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="9005b-728">[Aşağıdaki kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) Fluent API bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="9005b-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="9005b-729">Bu öğreticide, Fluent API yalnızca özniteliklerle yapılamadığını DB eşlemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="9005b-730">Ancak Fluent API, özniteliklerle yapılabilecek biçimlendirme, doğrulama ve eşleme kurallarının çoğunu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="9005b-731">Gibi bazı öznitelikler `MinimumLength` Fluent API uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="9005b-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="9005b-732">`MinimumLength`şemayı değiştirmez, yalnızca bir minimum uzunluk doğrulama kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="9005b-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="9005b-733">Bazı geliştiriciler, varlık sınıflarının "temiz" olmasını sağlamak için Fluent API özel olarak kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="9005b-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="9005b-734">Öznitelikler ve Fluent API karışık olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="9005b-735">Yalnızca Fluent API (bileşik bir PK belirterek) yapılabilecek bazı konfigürasyonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="9005b-736">Yalnızca özniteliklerle (`MinimumLength`) yapılabilecek bazı konfigürasyonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="9005b-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="9005b-737">Fluent API veya özniteliklerini kullanmak için önerilen uygulama:</span><span class="sxs-lookup"><span data-stu-id="9005b-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="9005b-738">Bu iki yaklaşımdan birini seçin.</span><span class="sxs-lookup"><span data-stu-id="9005b-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="9005b-739">Seçilen yaklaşımı mümkün olduğunca düzenli olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="9005b-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="9005b-740">Bu öğreticide kullanılan özniteliklerin bazıları için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9005b-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="9005b-741">Yalnızca doğrulama (örneğin, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="9005b-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="9005b-742">Yalnızca yapılandırma EF Core (örneğin, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="9005b-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="9005b-743">Doğrulama ve EF Core yapılandırma (örneğin, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="9005b-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="9005b-744">Öznitelikler ile Fluent API hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="9005b-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="9005b-745">Ilişkileri gösteren varlık diyagramı</span><span class="sxs-lookup"><span data-stu-id="9005b-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="9005b-746">Aşağıdaki çizimde, tamamlanmış okul modeli için EF Power Tools 'un oluştura diyagramı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9005b-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="9005b-748">Önceki diyagramda şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="9005b-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="9005b-749">Birden çok çoktan çoğa ilişki satırı (1 ile \*).</span><span class="sxs-lookup"><span data-stu-id="9005b-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="9005b-750">`Instructor` Ve`OfficeAssignment` varlıkları arasında bire sıfır veya-bir ilişki çizgisi (1 ila 0.. 1).</span><span class="sxs-lookup"><span data-stu-id="9005b-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="9005b-751">`Instructor` Ve`Department` varlıkları arasında sıfır veya-bire çok ilişki çizgisi (0.. 1-\*).</span><span class="sxs-lookup"><span data-stu-id="9005b-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="9005b-752">VERITABANıNı test verileriyle çekirdek olarak</span><span class="sxs-lookup"><span data-stu-id="9005b-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="9005b-753">*Data/Dbınizer. cs*dosyasındaki kodu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="9005b-754">Yukarıdaki kod, yeni varlıklar için tohum verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="9005b-755">Bu kodun çoğu yeni varlık nesneleri oluşturur ve örnek verileri yükler.</span><span class="sxs-lookup"><span data-stu-id="9005b-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="9005b-756">Örnek veriler test için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9005b-756">The sample data is used for testing.</span></span> <span data-ttu-id="9005b-757">Birden `Enrollments` çok `CourseAssignments` -çok JOIN tablosunun nasıl çalıştırılabilir olduğunu gösteren örnekler için bkz. ve.</span><span class="sxs-lookup"><span data-stu-id="9005b-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="9005b-758">Geçiş Ekle</span><span class="sxs-lookup"><span data-stu-id="9005b-758">Add a migration</span></span>

<span data-ttu-id="9005b-759">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9005b-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-761">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="9005b-762">Yukarıdaki komut, olası veri kaybı hakkında bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9005b-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="9005b-763">`database update` Komut çalıştırıldığında, aşağıdaki hata oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="9005b-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="9005b-764">Geçişi Uygula</span><span class="sxs-lookup"><span data-stu-id="9005b-764">Apply the migration</span></span>

<span data-ttu-id="9005b-765">Artık var olan bir veritabanınız olduğuna göre, bundan sonraki değişikliklere nasıl uygulanacağını düşünmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="9005b-766">Bu öğreticide iki yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9005b-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="9005b-767">Veritabanını bırakıp yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="9005b-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="9005b-768">[Geçişi mevcut veritabanına uygulayın](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="9005b-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="9005b-769">Bu yöntem daha karmaşıktır ve zaman alabilir. Bu, gerçek dünyada üretim ortamları için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="9005b-770">**Not**: Bu, öğreticinin isteğe bağlı bir bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="9005b-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="9005b-771">Bırakma ve yeniden oluşturma adımlarını gerçekleştirebilir ve bu bölümü atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9005b-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="9005b-772">Bu bölümdeki adımları izlemek isterseniz, bırakma ve yeniden oluşturma adımlarını yapmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="9005b-773">Veritabanını bırakıp yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="9005b-773">Drop and re-create the database</span></span>

<span data-ttu-id="9005b-774">Güncelleştirilmiş `DbInitializer` kod, yeni varlıklar için tohum verileri ekler.</span><span class="sxs-lookup"><span data-stu-id="9005b-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="9005b-775">Yeni bir VERITABANı oluşturmak için EF Core zorlamak için DB 'yi bırakıp güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9005b-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9005b-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9005b-777">**Paket Yöneticisi konsolunda** (PMC), aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9005b-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="9005b-778">Yardım `Get-Help about_EntityFrameworkCore` bilgileri almak için PMC 'den çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9005b-779">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9005b-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9005b-780">Bir komut penceresi açın ve proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="9005b-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="9005b-781">Proje klasörü *Startup.cs* dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="9005b-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="9005b-782">Komut penceresine şunu girin:</span><span class="sxs-lookup"><span data-stu-id="9005b-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="9005b-783">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9005b-783">Run the app.</span></span> <span data-ttu-id="9005b-784">Uygulamayı çalıştırmak `DbInitializer.Initialize` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="9005b-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="9005b-785">Yeni DB 'yi doldurur. `DbInitializer.Initialize`</span><span class="sxs-lookup"><span data-stu-id="9005b-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="9005b-786">VERITABANıNı SSOX içinde açın:</span><span class="sxs-lookup"><span data-stu-id="9005b-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="9005b-787">Daha önce SSOX açıldıysa **Yenile** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="9005b-788">Genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="9005b-788">Expand the **Tables** node.</span></span> <span data-ttu-id="9005b-789">Oluşturulan tablolar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9005b-789">The created tables are displayed.</span></span>

![SSOX içindeki tablolar](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="9005b-791">**Courseatama** tablosunu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="9005b-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="9005b-792">**Courseatama** tablosuna sağ tıklayın ve **verileri görüntüle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="9005b-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="9005b-793">**Courseatama** tablosunun veri içerdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-793">Verify the **CourseAssignment** table contains data.</span></span>

![SSOX 'te Courseatama verileri](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="9005b-795">Geçişi mevcut veritabanına Uygula</span><span class="sxs-lookup"><span data-stu-id="9005b-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="9005b-796">Bu bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9005b-796">This section is optional.</span></span> <span data-ttu-id="9005b-797">Bu adımlar yalnızca önceki [bırakma ve veritabanını yeniden oluşturma](#drop) bölümünü atladıysanız çalışır.</span><span class="sxs-lookup"><span data-stu-id="9005b-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="9005b-798">Geçişler mevcut verilerle çalıştırıldığında, mevcut verilerin karşılanmadığı FK kısıtlamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="9005b-799">Üretim verileriyle, mevcut verilerin geçirilmesi için adımların alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9005b-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="9005b-800">Bu bölüm, FK kısıtlama ihlallerinin düzeltilmesiyle bir örnek sağlar.</span><span class="sxs-lookup"><span data-stu-id="9005b-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="9005b-801">Bu kod değişikliklerini yedekleme olmadan yapmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="9005b-802">Önceki bölümü tamamlayıp veritabanını güncelleştirdiyseniz, bu kod değişikliklerini yapmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="9005b-803">*{Timestamp} _ComplexDataModel. cs* dosyası aşağıdaki kodu içerir:</span><span class="sxs-lookup"><span data-stu-id="9005b-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="9005b-804">Yukarıdaki kod, `DepartmentID` `Course` tabloya null yapılamayan bir FK ekler.</span><span class="sxs-lookup"><span data-stu-id="9005b-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="9005b-805">Önceki öğreticideki veritabanı, içindeki `Course`satırları içerir, böylece tablo geçişler tarafından güncelleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="9005b-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="9005b-806">`ComplexDataModel` Geçişin mevcut verilerle çalışmasını sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="9005b-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="9005b-807">Yeni sütuna (`DepartmentID`) varsayılan değer vermek için kodu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9005b-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="9005b-808">Varsayılan departman olarak davranacak "Temp" adlı sahte bir departman oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9005b-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="9005b-809">Yabancı anahtar kısıtlamalarını çözme</span><span class="sxs-lookup"><span data-stu-id="9005b-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="9005b-810">`ComplexDataModel` Classes`Up` metodunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9005b-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="9005b-811">*{Timestamp} _ComplexDataModel. cs* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="9005b-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="9005b-812">`DepartmentID` Sütunu `Course` tabloya ekleyen kod satırını açıklama olarak yapın.</span><span class="sxs-lookup"><span data-stu-id="9005b-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="9005b-813">Aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9005b-813">Add the following highlighted code.</span></span> <span data-ttu-id="9005b-814">Yeni kod `.CreateTable( name: "Department"` bloğundan sonra geçer:</span><span class="sxs-lookup"><span data-stu-id="9005b-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="9005b-815">Önceki değişikliklerle, varolan `Course` satırlar, `Up` Yöntem çalıştıktan `ComplexDataModel` sonra "geçici" departmanıyla ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="9005b-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="9005b-816">Bir üretim uygulaması şöyle olacaktır:</span><span class="sxs-lookup"><span data-stu-id="9005b-816">A production app would:</span></span>

* <span data-ttu-id="9005b-817">Yeni `Course` `Department` satırlarasatırlarveilgilisatırlareklemekiçinkodveyakomutdosyalarıekleyin.`Department`</span><span class="sxs-lookup"><span data-stu-id="9005b-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="9005b-818">İçin `Course.DepartmentID`"geçici" Departmanı veya varsayılan değeri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="9005b-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="9005b-819">Sonraki öğreticide ilgili veriler ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9005b-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9005b-820">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9005b-820">Additional resources</span></span>

* [<span data-ttu-id="9005b-821">Bu öğreticinin YouTube sürümü (Bölüm 1)</span><span class="sxs-lookup"><span data-stu-id="9005b-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="9005b-822">Bu öğreticinin YouTube sürümü (Bölüm 2)</span><span class="sxs-lookup"><span data-stu-id="9005b-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="9005b-823">[Önceki](xref:data/ef-rp/migrations)İleri
> [](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="9005b-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end