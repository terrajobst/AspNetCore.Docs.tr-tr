---
title: Itanium tabanlı sistemler için ASP.NET Core MVC ile EF Core - veri modeli - 5 / 10
author: rick-anderson
description: Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyin ve veri modelini, doğrulama, biçimlendirme ve eşleme kuralları belirterek özelleştirin.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 1d3c69c8c658b5ca2f0253b790b0dc75d44d3064
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194097"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="f6df2-103">Itanium tabanlı sistemler için ASP.NET Core MVC ile EF Core - veri modeli - 5 / 10</span><span class="sxs-lookup"><span data-stu-id="f6df2-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f6df2-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f6df2-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f6df2-105">Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="f6df2-106">Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="f6df2-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="f6df2-107">Önceki öğreticilerde, bir basit veri modeliyle üç varlıklarının oluşturulma çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="f6df2-108">Bu öğreticide, daha fazla varlıklar ve ilişkiler ekleyeceksiniz ve biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirterek veri modeli özelleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="f6df2-109">İşlemi tamamladığınızda, varlık sınıfları, aşağıdaki çizimde gösterilen tamamlanmış veri modeli oluşturan:</span><span class="sxs-lookup"><span data-stu-id="f6df2-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="f6df2-111">Öznitelikleri kullanarak veri modelini özelleştirin</span><span class="sxs-lookup"><span data-stu-id="f6df2-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="f6df2-112">Bu bölümde, veri modeli, biçimlendirme, doğrulama ve veritabanı eşleme kurallarını belirten öznitelikleri kullanarak özelleştirmek nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="f6df2-113">Daha sonra oluşturacağınız aşağıdaki bölümler birkaç ekleyerek eksiksiz Okul veri modeli sınıfları zaten oluşturduğunuz ve modelde kalan varlık türleri için yeni sınıflar oluşturma öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f6df2-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="f6df2-114">Veri türü özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-114">The DataType attribute</span></span>

<span data-ttu-id="f6df2-115">Tüm bu alan için önem verdiğiniz rağmen tarih Öğrenci kayıt tarihler için tüm web sayfalarının şu anda zaman tarihi ile birlikte görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f6df2-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="f6df2-116">Veri ek açıklama öznitelikleri kullanarak bir Yapabileceğiniz kodu görüntüleme biçimini gösteren veriler her görünümde düzeltir Değiştir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="f6df2-117">Bir özniteliğe ekleyeceksiniz, yapmak nasıl bir örnek görmek için `EnrollmentDate` özelliğinde `Student` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="f6df2-118">İçinde *Models/Student.cs*, ekleme bir `using` bildirimi `System.ComponentModel.DataAnnotations` ad alanı ve ekleme `DataType` ve `DisplayFormat` özniteliklerini `EnrollmentDate` özelliği, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="f6df2-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="f6df2-119">`DataType` Özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="f6df2-120">Bu durumda yalnızca tarih değil tarih ve saati izlemek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="f6df2-121">`DataType` Tarih, saat, telefon numarası, para birimi, EmailAddress ve diğer birçok veri türlerini numaralandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="f6df2-122">`DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="f6df2-123">Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`, ve bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="f6df2-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="f6df2-124">`DataType` Öznitelik HTML 5 yayan `data-` HTML 5 tarayıcılar anlayabilirsiniz (okunur veri dash) öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f6df2-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="f6df2-125">`DataType` Öznitelikleri, tüm doğrulama sağlamadığınızdan.</span><span class="sxs-lookup"><span data-stu-id="f6df2-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="f6df2-126">`DataType.Date` Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="f6df2-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="f6df2-127">Varsayılan olarak, sunucunun CultureInfo üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="f6df2-128">`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f6df2-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="f6df2-129">`ApplyFormatInEditMode` Ayar değeri düzenlemek için metin kutusunda görüntülendiğinde biçimlendirme da uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="f6df2-130">(, Bazı alanlar için--örneğin para birimi değerleri için para birimi simgesi metin kutusundaki düzenleme için istediğiniz değil istemeyebilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="f6df2-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="f6df2-131">Kullanabileceğiniz `DisplayFormat` özniteliği kendisi, ancak bu, genellikle kullanmak iyi bir fikir `DataType` ayrıca özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="f6df2-132">`DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve ile elde etmezsiniz aşağıdaki avantajları sağlar `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="f6df2-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="f6df2-133">Tarayıcı HTML5 özellikleri etkinleştirebilirsiniz (örneğin, bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantıları göstermek, bazı istemci-tarafı giriş doğrulama, vs.).</span><span class="sxs-lookup"><span data-stu-id="f6df2-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="f6df2-134">Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.</span><span class="sxs-lookup"><span data-stu-id="f6df2-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="f6df2-135">Daha fazla bilgi için [ \<Giriş > Etiket Yardımcısı belgeleri](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f6df2-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="f6df2-136">Uygulamayı çalıştırın, Öğrenciler dizin sayfasına gidin ve süreleri artık kayıt tarihlerini gösterilir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="f6df2-137">Aynı Öğrenci modelini kullanan herhangi bir görünüm için geçerli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-137">The same will be true for any view that uses the Student model.</span></span>

![Tarihleri gösteren Öğrenciler dizin sayfası](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="f6df2-139">StringLength özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-139">The StringLength attribute</span></span>

<span data-ttu-id="f6df2-140">Veri doğrulama kuralları ve öznitelikleri kullanarak bir doğrulama hata iletisi de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="f6df2-141">`StringLength` Özniteliği veritabanında en fazla uzunluk ayarlar ve istemci tarafı ve sunucu tarafı sağlayan ASP.NET MVC için doğrulama.</span><span class="sxs-lookup"><span data-stu-id="f6df2-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="f6df2-142">En az dize uzunluğu Bu öznitelikte belirtebilirsiniz, ancak en düşük değer, veritabanı şema üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="f6df2-143">Kullanıcılar için bir ad 50 karakterden uzun girmeyin sağlamak istediğinizi varsayın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="f6df2-144">Bu kısıtlama eklemek için Ekle `StringLength` özniteliklerini `LastName` ve `FirstMidName` aşağıdaki örnekte gösterildiği gibi özellikleri:</span><span class="sxs-lookup"><span data-stu-id="f6df2-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="f6df2-145">`StringLength` Özniteliği olmaz önlemek bir kullanıcı için bir ad boşluk girmesini.</span><span class="sxs-lookup"><span data-stu-id="f6df2-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="f6df2-146">Kullanabileceğiniz `RegularExpression` girişi kısıtlamaları uygulamak için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="f6df2-147">Örneğin, aşağıdaki kod, ilk karakterin büyük harf olacak ve alfabetik olarak kalan karakterler gerektirir:</span><span class="sxs-lookup"><span data-stu-id="f6df2-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="f6df2-148">`MaxLength` Özniteliği benzer işlevsellik sağlar `StringLength` özniteliği ancak istemci tarafı sağlamaz doğrulama.</span><span class="sxs-lookup"><span data-stu-id="f6df2-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="f6df2-149">Veritabanı modeli, artık veritabanı şemasını değişikliği gerektiren bir şekilde değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="f6df2-150">Geçişler, uygulama kullanıcı Arabirimi kullanarak veritabanına eklemiş olabileceğiniz herhangi bir veri kaybetmeden şemasını güncelleştirmek için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="f6df2-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="f6df2-151">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-151">Save your changes and build the project.</span></span> <span data-ttu-id="f6df2-152">Ardından proje klasöründe komut penceresi açın ve aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="f6df2-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="f6df2-153">`migrations add` Komut uyarır veri kaybı oluşabileceği uzunluğu en fazla iki sütun için daha kısa değişiklik yapmadığından.</span><span class="sxs-lookup"><span data-stu-id="f6df2-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="f6df2-154">Geçişleri adlı bir dosya oluşturur  *\<zaman damgası > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="f6df2-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="f6df2-155">Kod içinde bu dosyayı içeren `Up` yönteminin veritabanı geçerli bir veri modeli eşleşecek şekilde güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="f6df2-156">`database update` Komutun çalıştığını, kod.</span><span class="sxs-lookup"><span data-stu-id="f6df2-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="f6df2-157">Geçişleri dosya adının önüne zaman damgası, Entity Framework tarafından geçişlerin sıralamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="f6df2-158">Veritabanını Güncelleştir komutu çalıştırmadan önce birden çok geçiş oluşturabilirsiniz ve ardından tüm geçişlerde içinde oluşturuldukları sırada uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="f6df2-159">Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesinde **Yeni Oluştur**, 50 karakterden uzun ya da bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="f6df2-160">Tıkladığınızda **Oluştur**, istemci tarafı doğrulama hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-160">When you click **Create**, client side validation shows an error message.</span></span>

![Dize uzunluğu hataları gösteren sayfa Öğrenciler dizin](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="f6df2-162">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-162">The Column attribute</span></span>

<span data-ttu-id="f6df2-163">Sınıfları ve özellikleri veritabanına nasıl eşlendiğini denetlemek için öznitelikleri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="f6df2-164">Adı kullanmışsınız varsayalım `FirstMidName` -ad alanı da ikinci adı içeriyor olabileceğinden alan.</span><span class="sxs-lookup"><span data-stu-id="f6df2-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="f6df2-165">Ancak, veritabanı sütununun adı için istediğiniz `FirstName`veritabanına karşı özel sorgular yazmak kullanıcılar bu adı alışmanızı sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="f6df2-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="f6df2-166">Bu eşleme yapmak için kullanabileceğiniz `Column` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="f6df2-167">`Column` Özniteliği belirtir, bu, veritabanı oluşturulduktan sonra sütunun `Student` eşleyen tablo `FirstMidName` özelliği adlandırılacak `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="f6df2-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="f6df2-168">Diğer bir deyişle, ne zaman kodunuzu başvurduğu `Student.FirstMidName`, veri kaynağından gelir veya içinde güncelleştirilmesi `FirstName` sütununun `Student` tablo.</span><span class="sxs-lookup"><span data-stu-id="f6df2-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="f6df2-169">Sütun adları belirtmezseniz, özellik adı olarak aynı adı verilir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="f6df2-170">İçinde *Student.cs* ekleyin bir `using` bildirimi `System.ComponentModel.DataAnnotations.Schema` ve sütun ad özniteliğini ekleyin `FirstMidName` vurgulanan aşağıdaki kodda gösterildiği gibi özelliği:</span><span class="sxs-lookup"><span data-stu-id="f6df2-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="f6df2-171">Ek `Column` özniteliği değişiklikleri model yedekleme `SchoolContext`, veritabanı eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="f6df2-172">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-172">Save your changes and build the project.</span></span> <span data-ttu-id="f6df2-173">Ardından proje klasöründe komut penceresi açın ve başka bir geçiş oluşturmak için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="f6df2-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="f6df2-174">İçinde **SQL Server Nesne Gezgini**, Öğrenci Tablo Tasarımcısı çift tıklayarak açın **Öğrenci** tablo.</span><span class="sxs-lookup"><span data-stu-id="f6df2-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Geçiş sonrasında SSOX Öğrenciler tabloda](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="f6df2-176">İlk iki geçişlerin uygulamadan önce ad sütunu türü nvarchar(MAX) yoktu.</span><span class="sxs-lookup"><span data-stu-id="f6df2-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="f6df2-177">FirstName FirstMidName nvarchar(50) ve sütun adı değişti artık oldukları.</span><span class="sxs-lookup"><span data-stu-id="f6df2-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="f6df2-178">Aşağıdaki bölümlerde tüm varlık sınıfları oluşturma tamamlanmadan önce derlemek denerseniz derleyici hataları alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="f6df2-179">Öğrenci varlık için son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="f6df2-179">Final changes to the Student entity</span></span>

![Öğrenci varlık](complex-data-model/_static/student-entity.png)

<span data-ttu-id="f6df2-181">İçinde *Models/Student.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="f6df2-182">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="f6df2-183">Gerekli öznitelik</span><span class="sxs-lookup"><span data-stu-id="f6df2-183">The Required attribute</span></span>

<span data-ttu-id="f6df2-184">`Required` Öznitelik adı özellikleri gerekli alanları yapar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="f6df2-185">`Required` Öznitelik değer türleri gibi atanamaz türler için gerekli olmayan (DateTime, int, çift, float, vs.).</span><span class="sxs-lookup"><span data-stu-id="f6df2-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="f6df2-186">Null olamaz türleri gerekli alanları otomatik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="f6df2-187">Kullanarak kaldırabilirsiniz `Required` için en az uzunluk parametresi ile değiştirin ve öznitelik `StringLength` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="f6df2-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="f6df2-188">Görüntüleme özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-188">The Display attribute</span></span>

<span data-ttu-id="f6df2-189">`Display` Özniteliği, metin kutuları için açıklama yazısını "Ad", "Soyadı", "Tam adı" ve "Kayıt tarihi" özellik adı yerine (Sözcük bölünmesi boşluk olan) her örnek olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="f6df2-190">Hesaplanan FullName özelliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-190">The FullName calculated property</span></span>

<span data-ttu-id="f6df2-191">`FullName` diğer iki özellikleri birleştirilerek oluşturulur bir değer döndüren bir hesaplanan özelliktir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="f6df2-192">Bu nedenle yalnızca bir alma erişimcisi ve Hayır sahip `FullName` sütunu veritabanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="f6df2-193">Eğitmen varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="f6df2-193">Create the Instructor Entity</span></span>

![Eğitmen varlık](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="f6df2-195">Oluşturma *Models/Instructor.cs*, şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f6df2-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="f6df2-196">Çeşitli özellikler Öğrenci ve Eğitmen varlıkları aynı olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="f6df2-197">İçinde [uygulama devralma](inheritance.md) bu serideki sonraki öğretici, artıklığı ortadan kaldırmak için bu kodu yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="f6df2-198">Yazma böylece birden çok öznitelik bir satıra koyabilirsiniz `HireDate` gibi öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="f6df2-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="f6df2-199">CourseAssignments ve OfficeAssignment Gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="f6df2-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="f6df2-200">`CourseAssignments` Ve `OfficeAssignment` Gezinti özellikleri özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="f6df2-201">Bir eğitmen kursları herhangi bir sayıda öğretebiliriz, bu nedenle `CourseAssignments` bir koleksiyon olarak tanımlandı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f6df2-202">Bir gezinme özelliği birden çok varlık tutarsanız türünü, girişler eklenir, silindi, güncelleştirildi ve bir liste olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="f6df2-203">Belirtebileceğiniz `ICollection<T>` ya da bir tür gibi `List<T>` veya `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="f6df2-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="f6df2-204">Belirtirseniz `ICollection<T>`, EF oluşturur bir `HashSet<T>` varsayılan olarak koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="f6df2-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="f6df2-205">Bunlar neden neden `CourseAssignment` varlıkları açıklanmıştır aşağıda çoktan çoğa ilişkiler hakkında bölümünde.</span><span class="sxs-lookup"><span data-stu-id="f6df2-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="f6df2-206">Contoso University iş kuralları durum bir eğitmen yalnızca en fazla bir office olabilir böylece `OfficeAssignment` özelliği (Bu, hiçbir office atanırsa null olabilir) tek bir OfficeAssignment varlık içerir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="f6df2-207">OfficeAssignment varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="f6df2-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment varlık](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="f6df2-209">Oluşturma *Models/OfficeAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f6df2-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="f6df2-210">Anahtar öznitelik</span><span class="sxs-lookup"><span data-stu-id="f6df2-210">The Key attribute</span></span>

<span data-ttu-id="f6df2-211">Eğitmen ve OfficeAssignment varlıklar arasındaki bir sıfır-veya-bire bir ilişki yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="f6df2-212">Atanan Eğitmeni ile ilgili bir ofis ataması yalnızca var ve bu nedenle birincil anahtarı da Eğitmen varlık, yabancı anahtar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="f6df2-213">Ancak kimliği veya Classnameıd adlandırma kuralı adının izleyin değil çünkü Entity Framework otomatik olarak Instructorıd bu varlığın birincil anahtarı tanınmıyor.</span><span class="sxs-lookup"><span data-stu-id="f6df2-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="f6df2-214">Bu nedenle, `Key` özniteliği anahtar olarak tanımlamak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f6df2-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="f6df2-215">Ayrıca `Key` varlığın birincil anahtarı yok ancak Classnameıd veya kimliği dışındaki ad özelliği denemek istiyorsanız özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="f6df2-216">Sütunu için tanımlayıcı bir ilişkisi olduğundan varsayılan olarak, olmayan-veritabanında oluşturulmuş olarak anahtar EF değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="f6df2-217">Eğitmen gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-217">The Instructor navigation property</span></span>

<span data-ttu-id="f6df2-218">Boş değer atanabilir bir eğitmen varlık sahip `OfficeAssignment` gezinti özelliği (bir eğitmen bir ofis ataması olmayabilir çünkü), ve OfficeAssignment varlık atanamayan bir sahip `Instructor` gezinti özelliği (bir ofis ataması yapılamıyor çünkü Mevcut bir eğitmen-- `InstructorID` null değer alamaz).</span><span class="sxs-lookup"><span data-stu-id="f6df2-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="f6df2-219">Eğitmen varlığın ilgili OfficeAssignment varlık olduğunda, her varlık, gezinti özelliğinin diğer bir başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="f6df2-220">Put bir `[Required]` Eğitmen Gezinti özelliğindeki ilgili Eğitmen olmalıdır, ancak, çünkü bunu yapmak zorunda olmadığınız belirtmek için özniteliği `InstructorID` (aynı zamanda olan bu tablo anahtarı), yabancı anahtar null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="f6df2-221">Kurs varlığı değiştirme</span><span class="sxs-lookup"><span data-stu-id="f6df2-221">Modify the Course Entity</span></span>

![Kurs varlık](complex-data-model/_static/course-entity.png)

<span data-ttu-id="f6df2-223">İçinde *Models/Course.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="f6df2-224">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="f6df2-225">Kurs varlık bir yabancı anahtar özelliğine sahip `DepartmentID` ilgili bölüm varlık ve onu işaret ettiği sahip bir `Department` gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="f6df2-226">Entity Framework gezinme özelliğinin bağını ilgili varlık olduğunda veri modelinizi yabancı bir anahtar özellik eklemenize gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="f6df2-227">EF otomatik olarak ihtiyaç duyulan yere, veritabanında yabancı anahtarlar oluşturur ve oluşturur [gölge Özellikler](https://docs.microsoft.com/ef/core/modeling/shadow-properties) bunlar için.</span><span class="sxs-lookup"><span data-stu-id="f6df2-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="f6df2-228">Ancak yabancı anahtar veri modelinde sahip güncelleştirmeleri daha basit ve daha verimli hale getirebilir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="f6df2-229">Departman Varlığı düzenlemek için bir kurs varlık getiren, örneğin, null, yükleme şekilde kurs varlık güncelleştirdiğinizde varsa ilk bölüm varlık getirilemedi.</span><span class="sxs-lookup"><span data-stu-id="f6df2-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="f6df2-230">Yabancı anahtar özelliği `DepartmentID` içerdiği veri modelinde güncelleştirmeden önce departmanı varlık getiren gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f6df2-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="f6df2-231">DatabaseGenerated özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="f6df2-232">`DatabaseGenerated` Özniteliğini `None` parametresi `CourseID` birincil anahtar değerlerini kullanıcı tarafından sağlanan yerine veritabanı tarafından oluşturulan özelliği belirtir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="f6df2-233">Varsayılan olarak Entity Framework, birincil anahtar değerlerini veritabanı tarafından oluşturulan varsayar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="f6df2-234">Çoğu senaryoda, istediğiniz olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="f6df2-235">Ancak, kurs varlıklar için 1000 serisi gibi bir kullanıcı tarafından belirtilen kurs numarası bir bölümü, başka bir bölüme, 2000 serilerinin için kullanabilir ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="f6df2-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="f6df2-236">`DatabaseGenerated` Özniteliği de kullanılabilir olması durumunda veritabanı sütunlarını tarihi kaydetmek için kullanılan bir satır oluşturulurken veya güncelleştirilirken gibi varsayılan değerleri oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="f6df2-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="f6df2-237">Daha fazla bilgi için [üretilen özellikleri](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="f6df2-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f6df2-238">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="f6df2-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="f6df2-239">Kurs varlık Gezinti özellikleri ve yabancı anahtar özelliklerini aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="f6df2-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="f6df2-240">Bu yüzden bir kurs bir bölüme atanan bir `DepartmentID` yabancı anahtar ve `Department` yukarıda belirtilen nedenlerden dolayı gezinme özelliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="f6df2-241">Herhangi bir sayıda Öğrenciler içinde kayıtlı bir kurs olabilir böylece `Enrollments` olan bir koleksiyon gezinme özelliği:</span><span class="sxs-lookup"><span data-stu-id="f6df2-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="f6df2-242">Bir kurs birden çok Eğitmenler tarafından verilen böylece `CourseAssignments` olan bir koleksiyon gezinme özelliği (tür `CourseAssignment` açıklanan [daha sonra](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="f6df2-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="f6df2-243">Departman varlık oluşturma</span><span class="sxs-lookup"><span data-stu-id="f6df2-243">Create the Department entity</span></span>

![Departman varlık](complex-data-model/_static/department-entity.png)


<span data-ttu-id="f6df2-245">Oluşturma *Models/Department.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f6df2-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="f6df2-246">Sütun özniteliği</span><span class="sxs-lookup"><span data-stu-id="f6df2-246">The Column attribute</span></span>

<span data-ttu-id="f6df2-247">Daha önce kullandığınız `Column` sütun adı eşlemesini değiştirmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f6df2-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="f6df2-248">Departman varlık kodunda `Column` sütunu veritabanında SQL Server para türü kullanılarak tanımlanır için SQL veri türü eşlemesi değiştirmek için özniteliği kullanılıyor:</span><span class="sxs-lookup"><span data-stu-id="f6df2-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="f6df2-249">Sütun eşlemesi Entity Framework seçtiği özelliği için tanımladığınız CLR türüne göre uygun SQL Server veri türü için genelde gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="f6df2-250">CLR `decimal` türü bir SQL Server eşlenir `decimal` türü.</span><span class="sxs-lookup"><span data-stu-id="f6df2-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="f6df2-251">Ancak, bu durumda sütunu para birimi miktarları bulunduran ve para veri türü için daha uygun olduğunu biliyor.</span><span class="sxs-lookup"><span data-stu-id="f6df2-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f6df2-252">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="f6df2-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="f6df2-253">Yabancı anahtar ve gezinti özellikleri, aşağıdaki ilişkileri yansıtır:</span><span class="sxs-lookup"><span data-stu-id="f6df2-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="f6df2-254">Bir bölüm olabilir veya bir yönetici olmayabilir ve yönetici her zaman bir eğitmen.</span><span class="sxs-lookup"><span data-stu-id="f6df2-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="f6df2-255">Bu nedenle `InstructorID` Eğitmen varlığı için yabancı anahtar özelliği bulunur ve sonra bir soru işareti eklenir `int` özelliği null olarak işaretlemek için ataması yazın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="f6df2-256">Gezinme özelliğini adlı `Administrator` ancak bir eğitmen varlık tutar:</span><span class="sxs-lookup"><span data-stu-id="f6df2-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="f6df2-257">Kursları gezinti özelliği bu nedenle departman birçok kursları olabilir:</span><span class="sxs-lookup"><span data-stu-id="f6df2-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="f6df2-258">Kural gereği, art arda silme için alamayan yabancı anahtarlar ve çoktan çoğa ilişkiler için Entity Framework sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="f6df2-259">Bu, bir geçiş eklemeye çalıştığınızda bir özel durum neden olur, döngüsel art arda silme kuralları'nda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="f6df2-260">Örneğin, Department.InstructorID özelliği null olarak tanımlamadığınız varsa EF sorun olmasını istediğiniz değildir departman sildiğinizde Eğitmen silmek için bir cascade delete kuralı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="f6df2-261">İş kurallarınızı gerekirse `InstructorID` özelliğini alamayan, art arda silme ilişkiyi devre dışı bırakmak için aşağıdaki fluent API'si deyimi kullanılacak olurdu:</span><span class="sxs-lookup"><span data-stu-id="f6df2-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="f6df2-262">Kayıt varlığı değiştirme</span><span class="sxs-lookup"><span data-stu-id="f6df2-262">Modify the Enrollment entity</span></span>

![Kayıt varlık](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="f6df2-264">İçinde *Models/Enrollment.cs*, eklediğiniz kod daha önce aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f6df2-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f6df2-265">Yabancı anahtar ve gezinti özellikleri</span><span class="sxs-lookup"><span data-stu-id="f6df2-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="f6df2-266">Aşağıdaki ilişkileri ve gezinti özelliklerini yabancı anahtar özelliklerini yansıtır:</span><span class="sxs-lookup"><span data-stu-id="f6df2-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="f6df2-267">Yani bir kaydı tek bir kurs için olan bir `CourseID` yabancı anahtar özellik ve `Course` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="f6df2-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="f6df2-268">Bu yüzden bir kayıt için tek bir öğrenci, kaydıdır bir `StudentID` yabancı anahtar özellik ve `Student` gezinti özelliği:</span><span class="sxs-lookup"><span data-stu-id="f6df2-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="f6df2-269">Çoktan çoğa ilişkiler</span><span class="sxs-lookup"><span data-stu-id="f6df2-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="f6df2-270">Öğrenci ve kursu varlıklar arasında çoktan çoğa bir ilişki yoktur ve kayıt varlık işlevleri bire çok birleşme tablo olarak *yüküyle* veritabanında.</span><span class="sxs-lookup"><span data-stu-id="f6df2-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="f6df2-271">"Yüke sahip" kayıt tablosu yabancı anahtarlar için birleştirilmiş tablolarında (Bu durumda, bir birincil anahtar ve bir sınıf özelliği) yanı sıra ek verileri içerdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="f6df2-272">Aşağıdaki çizim, bu ilişkiler bir varlık diyagramda nasıl göründüğünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="f6df2-273">(Bu diyagramda EF için Entity Framework güç araçları kullanılarak oluşturulan 6.x; diyagram oluşturma, öğreticinin bir parçası değil, yalnızca kullanıldığı bir çizim olarak burada.)</span><span class="sxs-lookup"><span data-stu-id="f6df2-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Öğrenci Kursluk çok-çok ilişkisi](complex-data-model/_static/student-course.png)

<span data-ttu-id="f6df2-275">Her ilişki ucu ve bir yıldız işareti (\*) 1 diğer sırasında bir-çok ilişkisi belirten bulunur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="f6df2-276">Sınıf bilgileri kayıt tablosu eklemediyseniz, yalnızca CourseID ve StudentID iki yabancı anahtar içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="f6df2-277">Bu durumda, bu bire çok birleşme tablo yükü olmadan (veya saf birleşim tablosu) veritabanında olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="f6df2-278">Bu tür çoktan çoğa ilişki bir eğitmen ve kurs varlıkların ve sonraki adımınız yükü olmadan bir JOIN tablo olarak çalışması için bir varlık sınıfı oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="f6df2-279">(Çok-çok ilişkileri ve EF Core için örtük birleşim tabloları değil EF 6.x destekler.</span><span class="sxs-lookup"><span data-stu-id="f6df2-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="f6df2-280">Daha fazla bilgi için [tartışma EF Core GitHub deposunda](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="f6df2-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="f6df2-281">CourseAssignment varlık</span><span class="sxs-lookup"><span data-stu-id="f6df2-281">The CourseAssignment entity</span></span>

![CourseAssignment varlık](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="f6df2-283">Oluşturma *Models/CourseAssignment.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f6df2-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="f6df2-284">Varlık adları katılın</span><span class="sxs-lookup"><span data-stu-id="f6df2-284">Join entity names</span></span>

<span data-ttu-id="f6df2-285">Eğitmen kursları çoktan çoğa ilişki için veritabanında bir birleşim tablo gereklidir ve bir varlık kümesi tarafından temsil edilmesini vardır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="f6df2-286">Bir birleşim varlık adı yaygındır `EntityName1EntityName2`, bu durumda olacak `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="f6df2-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="f6df2-287">Ancak, ilişkiyi tanımlayan bir ad seçmenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="f6df2-288">Veri modelleri basit ' ı başlatın ve, sık yükü daha sonra başlama Hayır yükü birleştirmelerle büyütün.</span><span class="sxs-lookup"><span data-stu-id="f6df2-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="f6df2-289">Bir tanımlayıcı varlık adla başlatırsanız, daha sonra adını değiştirmek zorunda kalmazsınız.</span><span class="sxs-lookup"><span data-stu-id="f6df2-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="f6df2-290">Birleştirme varlık ideal olarak, iş etki alanında kendi doğal (muhtemelen tek sözcük) adına sahip.</span><span class="sxs-lookup"><span data-stu-id="f6df2-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="f6df2-291">Örneğin, kitaplar ve müşterilerin derecelendirmeleri bağ kurulamadı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="f6df2-292">Bu ilişki için `CourseAssignment` değerinden daha iyi bir seçimdir `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="f6df2-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="f6df2-293">Bileşik anahtar</span><span class="sxs-lookup"><span data-stu-id="f6df2-293">Composite key</span></span>

<span data-ttu-id="f6df2-294">Yabancı anahtarlar benzersiz olarak boş değer atanabilir ve birlikte olmadığından tablonun her satırı tanımlamak, ayrı bir birincil anahtar için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="f6df2-295">*Instructorıd* ve *CourseID* özellikleri, bileşik bir birincil anahtar olarak çalışması.</span><span class="sxs-lookup"><span data-stu-id="f6df2-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="f6df2-296">Kullanarak EF bileşik birincil anahtarları tanımlamak için tek yolu olduğundan *fluent API'si* (Bu öznitelikleri kullanarak yapılamaz).</span><span class="sxs-lookup"><span data-stu-id="f6df2-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="f6df2-297">Sonraki bölümde bileşik bir birincil anahtar yapılandırmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="f6df2-298">Bileşik anahtarın bir kurs ve bir eğitmen için birden çok satır için birden çok satır olabilse kurs ve aynı eğitmen için birden fazla satır olamaz sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="f6df2-299">`Enrollment` Birleştirme varlık çoğaltmaları bu tür mümkün olduğundan, kendi birincil anahtar tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="f6df2-300">Böyle yinelenen önlemek için benzersiz bir dizin yabancı anahtar alanları eklediğinizde veya yapılandırma `Enrollment` benzer şekilde birincil Bileşik anahtarı `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f6df2-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="f6df2-301">Daha fazla bilgi için [dizinleri](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="f6df2-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="f6df2-302">Veritabanı bağlamı güncelleştir</span><span class="sxs-lookup"><span data-stu-id="f6df2-302">Update the database context</span></span>

<span data-ttu-id="f6df2-303">Aşağıdaki vurgulanmış kodu ekleyin *Data/SchoolContext.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f6df2-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="f6df2-304">Bu kod, yeni varlıkları ekleyen ve CourseAssignment varlığın bileşik birincil anahtarını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="f6df2-305">Fluent API'si alternatif öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="f6df2-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="f6df2-306">Kodda `OnModelCreating` yöntemi `DbContext` sınıfının kullandığı *fluent API'si* EF davranışı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="f6df2-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="f6df2-307">Bu örnekte olduğu gibi tek bir deyimde içine bir dizi yöntem çağrılarını birlikte stringing genellikle kullanıldığından API'sı "fluent" çağrılan [EF Core belgeleri](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="f6df2-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="f6df2-308">Bu öğreticide, yalnızca özniteliklerle yapamayacağınız veritabanı eşleme fluent API'si kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="f6df2-309">Ancak, çoğu biçimlendirmeyi, doğrulama ve öznitelikleri kullanarak yapabileceğiniz eşleme kurallarını belirtmek için fluent API'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="f6df2-310">Bazı öznitelikler gibi `MinimumLength` fluent API'si ile uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="f6df2-311">Daha önce de belirtildiği `MinimumLength` şemasını değiştirmez, yalnızca bir istemci ve sunucu tarafı doğrulama kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="f6df2-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="f6df2-312">Bazı geliştiriciler özel olarak bunlar kendi varlık sınıfları "temiz" olan fluent API'sini kullanmayı tercih edin</span><span class="sxs-lookup"><span data-stu-id="f6df2-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="f6df2-313">İstediğiniz ve fluent API'sini kullanarak yalnızca yapılabilir birkaç özelleştirmeleri öznitelikleri ve fluent API'si karıştırabilirsiniz, ancak genel olarak önerilen uygulama bu iki yaklaşımdan birini seçin ve bu tutarlı bir şekilde mümkün olduğunca kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="f6df2-314">Her ikisini de kullanıyorsanız, çakışma olduğunda Fluent API'si özniteliklerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="f6df2-315">Fluent API'si ve öznitelikler hakkında daha fazla bilgi için bkz. [yapılandırma yöntemleri,](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="f6df2-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="f6df2-316">Varlık diyagramda gösteren ilişkileri</span><span class="sxs-lookup"><span data-stu-id="f6df2-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="f6df2-317">Aşağıdaki resimde tamamlanmış Okul model için Entity Framework Power Tools oluşturduğunuz diyagramda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Varlık diyagramı](complex-data-model/_static/diagram.png)

<span data-ttu-id="f6df2-319">Çoğa bir ilişki yanı sıra (1 \*), eğitmen ve OfficeAssignment varlıkları ve sıfır-veya-bir-çok ilişki çizgisi arasındaki bir sıfır-veya-bir ilişki satırı (için 1 0..1) burada görebilirsiniz (0..1 için *) arasında Eğitmen ve departman varlıklar.</span><span class="sxs-lookup"><span data-stu-id="f6df2-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="f6df2-320">Test verileri ile veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="f6df2-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="f6df2-321">Değiştirin *Data/DbInitializer.cs* oluşturduğunuz yeni varlıklar için çekirdek veri sağlamak için aşağıdaki kodu dosyası.</span><span class="sxs-lookup"><span data-stu-id="f6df2-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="f6df2-322">İlk öğreticide gördüğünüz gibi çoğu bu kod, yalnızca yeni varlık nesnesi oluşturur ve örnek veri özelliklerini test etmek için gerektiği gibi yükler.</span><span class="sxs-lookup"><span data-stu-id="f6df2-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="f6df2-323">Çoktan çoğa ilişkilerin nasıl işleneceğini dikkat edin: kod içindeki varlık oluşturarak ilişkileri oluşturur `Enrollments` ve `CourseAssignment` varlık kümeleri katılın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="f6df2-324">Bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="f6df2-324">Add a migration</span></span>

<span data-ttu-id="f6df2-325">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-325">Save your changes and build the project.</span></span> <span data-ttu-id="f6df2-326">Ardından proje klasöründe komut penceresi açın ve girin `migrations add` komut (yapma güncelleştirme database komutu henüz):</span><span class="sxs-lookup"><span data-stu-id="f6df2-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="f6df2-327">Olası veri kaybı hakkında bir uyarı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="f6df2-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="f6df2-328">Çalıştırmayı denediyseniz `database update` bu noktada komut (yapma, henüz), şu hatayı elde edersiniz:</span><span class="sxs-lookup"><span data-stu-id="f6df2-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="f6df2-329">ALTER TABLE deyimini "FK_dbo. yabancı anahtar kısıtlaması ile çakıştı. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="f6df2-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="f6df2-330">Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Departman", sütun 'DepartmentID'.</span><span class="sxs-lookup"><span data-stu-id="f6df2-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="f6df2-331">Bazen, mevcut verilerle geçişleri yürüttüğünüzde, yabancı anahtar kısıtlamaları karşılamak için veritabanına saplama verisini eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="f6df2-332">Oluşturulan kod `Up` yöntemi atanamayan DepartmentID yabancı anahtar kurs tabloya ekler.</span><span class="sxs-lookup"><span data-stu-id="f6df2-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="f6df2-333">Varsa satırları kurs tabloda kod çalıştığında `AddColumn` SQL Server hangi değerin null olamaz sütununda put bilmediği işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f6df2-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="f6df2-334">Bu öğretici için geçiş yeni bir veritabanı üzerinde çalıştırdığınız, ancak bir üretim uygulamasında aşağıdaki yönergeler bunu nasıl yapacağınız örneği böylece mevcut veri işleme geçiş yapması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f6df2-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="f6df2-335">Yeni bir sütun bir varsayılan değeri vermek için kodu değiştirmek zorunda mevcut verilerle çalışacak ve bir saplama bölüm oluşturun, bu geçiş yapmak için "varsayılan Departman olarak görev yapacak Temp" adlı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="f6df2-336">Sonuç olarak, kurs satır var olan tüm sonra "Temp" departmanına ilişkilendirilir `Up` yöntemi çalışır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="f6df2-337">Açık *{timestamp}_ComplexDataModel.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="f6df2-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>

* <span data-ttu-id="f6df2-338">DepartmentID sütun kurs tablosuna ekleyen bir kod satırı açıklama satırı yapın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="f6df2-339">Departman tablosu oluşturan kodu sonra aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f6df2-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="f6df2-340">Bir üretim uygulamasında, kod veya betiklerde departmanı satır eklemek ve yeni bölüm satırları kurs satırları ilişkili yazmalısınız.</span><span class="sxs-lookup"><span data-stu-id="f6df2-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="f6df2-341">Ardından artık "Temp" departman veya Course.DepartmentID sütununda Varsayılan değer ihtiyacınız olacaktı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="f6df2-342">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="f6df2-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="f6df2-343">Bağlantı dizesini değiştirebilir ve veritabanını güncelleştir</span><span class="sxs-lookup"><span data-stu-id="f6df2-343">Change the connection string and update the database</span></span>

<span data-ttu-id="f6df2-344">Artık bir artık yeni koda sahip `DbInitializer` yeni varlıklar için çekirdek veri için boş bir veritabanı ekler sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="f6df2-345">Yeni bir boş veritabanı oluşturma EF yapmak için bağlantı dizesinde veritabanının adını *appsettings.json* ContosoUniversity3 veya kullanmakta olduğunuz bilgisayarda kullanmadığınız bazı diğer adı.</span><span class="sxs-lookup"><span data-stu-id="f6df2-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="f6df2-346">Değişiklik Kaydet *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f6df2-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="f6df2-347">Veritabanı adının değiştirilmesi alternatif olarak, veritabanı silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="f6df2-348">Kullanım **SQL Server Nesne Gezgini** (SSOX) veya `database drop` CLI komutunu:</span><span class="sxs-lookup"><span data-stu-id="f6df2-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="f6df2-349">Veritabanı adı değiştirilmiş veya silinmiş veritabanı sonra çalıştırın `database update` geçişlerin yürütmek için komut penceresinde komutu.</span><span class="sxs-lookup"><span data-stu-id="f6df2-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="f6df2-350">Neden için uygulamayı çalıştırın `DbInitializer.Initialize` çalıştırın ve yeni veritabanını doldurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="f6df2-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="f6df2-351">Daha önce yaptığınız gibi veritabanı SSOX içinde açın ve genişletin **tabloları** tüm tabloların oluşturulduğunu görmek için düğümü.</span><span class="sxs-lookup"><span data-stu-id="f6df2-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="f6df2-352">(Önceki süreyi açın SSOX hala varsa, tıklayın **Yenile** düğmesi.)</span><span class="sxs-lookup"><span data-stu-id="f6df2-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![SSOX tablolarında](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="f6df2-354">Veritabanının çekirdeğini Başlatıcı kodu tetiklemek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="f6df2-355">Sağ **CourseAssignment** tablosunu seçip **görünüm verilerini** , veri içinde olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f6df2-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![SSOX CourseAssignment verileri](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="f6df2-357">Özet</span><span class="sxs-lookup"><span data-stu-id="f6df2-357">Summary</span></span>

<span data-ttu-id="f6df2-358">Artık daha karmaşık veri modeli ve karşılık gelen veritabanı vardır.</span><span class="sxs-lookup"><span data-stu-id="f6df2-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="f6df2-359">Aşağıdaki öğreticide ilgili veri erişim hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f6df2-359">In the following tutorial, you'll learn more about how to access related data.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="f6df2-360">[Önceki](migrations.md)
> [İleri](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="f6df2-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>
