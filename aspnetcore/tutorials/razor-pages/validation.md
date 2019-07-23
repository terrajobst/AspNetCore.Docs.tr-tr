---
title: ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına nasıl doğrulama ekleneceğini öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 8495849c89ca3d6fd2b2006b61ce2ec75ff504a5
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "67815647"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="b2435-103">ASP.NET Core Razor sayfasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="b2435-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="b2435-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2435-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b2435-105">Bu bölümde, doğrulama mantığı `Movie` modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="b2435-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="b2435-106">Doğrulama kuralları, bir Kullanıcı bir filmi oluşturduğunda veya düzenleişinizde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="b2435-107">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="b2435-107">Validation</span></span>

<span data-ttu-id="b2435-108">Yazılım geliştirmeye yönelik temel bir temel [kuru](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeon **Y**ourself") olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b2435-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="b2435-109">Razor Pages, işlevselliği bir kez belirtildiğinde geliştirme ve uygulama genelinde yansıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b2435-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="b2435-110">Kuru şu şekilde yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="b2435-110">DRY can help:</span></span>

* <span data-ttu-id="b2435-111">Uygulamadaki kod miktarını azaltın.</span><span class="sxs-lookup"><span data-stu-id="b2435-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="b2435-112">Kodu daha az hata haline getirin ve test ve bakım yapmayı kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="b2435-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="b2435-113">Razor Pages ve Entity Framework tarafından sunulan doğrulama desteği, Kuru ilkesine iyi bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="b2435-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="b2435-114">Doğrulama kuralları tek bir yerde (model sınıfında) bildirimli olarak belirtilir ve kurallar uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="b2435-115">Razor Pages 'de doğrulama hatası Kullanıcı arabirimi</span><span class="sxs-lookup"><span data-stu-id="b2435-115">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="b2435-116">Uygulamayı çalıştırın ve sayfalar/Filmler ' e gidin.</span><span class="sxs-lookup"><span data-stu-id="b2435-116">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="b2435-117">**Yeni oluştur** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="b2435-117">Select the **Create New** link.</span></span> <span data-ttu-id="b2435-118">Formu, bazı geçersiz değerlerle doldurun.</span><span class="sxs-lookup"><span data-stu-id="b2435-118">Complete the form with some invalid values.</span></span> <span data-ttu-id="b2435-119">JQuery istemci tarafı doğrulaması hatayı algıladığında, bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b2435-119">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="b2435-121">Formun geçersiz bir değer içeren her alanda otomatik olarak bir doğrulama hata iletisi nasıl oluşturulduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b2435-121">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="b2435-122">Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (bir Kullanıcı JavaScript devre dışı bırakıldığında) zorlanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-122">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="b2435-123">Önemli bir avantaj, oluşturma veya düzenleme sayfalarında **hiçbir** kod değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b2435-123">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="b2435-124">Veri ek açıklamaları modele uygulandıktan sonra, doğrulama kullanıcı arabirimi etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b2435-124">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="b2435-125">Bu öğreticide oluşturulan Razor Pages, otomatik olarak doğrulama kurallarını ( `Movie` model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak) otomatik olarak çekti.</span><span class="sxs-lookup"><span data-stu-id="b2435-125">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="b2435-126">Düzenleme sayfasını kullanarak doğrulama testi, aynı doğrulama uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-126">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="b2435-127">Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya nakledilmez.</span><span class="sxs-lookup"><span data-stu-id="b2435-127">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="b2435-128">Form verilerinin aşağıdaki yaklaşımlardan bir veya daha fazlası tarafından nakledilmediğinden emin olun:</span><span class="sxs-lookup"><span data-stu-id="b2435-128">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="b2435-129">`OnPostAsync` Yöntemine bir kesme noktası koyun.</span><span class="sxs-lookup"><span data-stu-id="b2435-129">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="b2435-130">Formu gönder ( **Oluştur** veya **Kaydet**' i seçin).</span><span class="sxs-lookup"><span data-stu-id="b2435-130">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="b2435-131">Kesme noktası hiçbir şekilde isabet ettirilmez.</span><span class="sxs-lookup"><span data-stu-id="b2435-131">The break point is never hit.</span></span>
* <span data-ttu-id="b2435-132">[Fiddler aracını](https://www.telerik.com/fiddler)kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2435-132">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="b2435-133">Ağ trafiğini izlemek için tarayıcı Geliştirici Araçları ' nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2435-133">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="b2435-134">Sunucu tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b2435-134">Server-side validation</span></span>

<span data-ttu-id="b2435-135">Tarayıcıda JavaScript devre dışı bırakıldığında, formun hatalarla gönderilmesi sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-135">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="b2435-136">İsteğe bağlı, test sunucusu-tarafı doğrulaması:</span><span class="sxs-lookup"><span data-stu-id="b2435-136">Optional, test server-side validation:</span></span>

* <span data-ttu-id="b2435-137">Tarayıcıda JavaScript 'ı devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="b2435-137">Disable JavaScript in the browser.</span></span> <span data-ttu-id="b2435-138">Bunu tarayıcınızın geliştirici araçlarını kullanarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2435-138">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="b2435-139">Tarayıcıda JavaScript 'ı devre dışı bırakadıysanız başka bir tarayıcı deneyin.</span><span class="sxs-lookup"><span data-stu-id="b2435-139">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="b2435-140">Oluşturma veya düzenleme sayfasının `OnPostAsync` yönteminde bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b2435-140">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="b2435-141">Doğrulama hatalarıyla bir form gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2435-141">Submit a form with validation errors.</span></span>
* <span data-ttu-id="b2435-142">Model durumunun geçersiz olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="b2435-142">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="b2435-143">Aşağıdaki kod, öğreticide daha önce iskele aldığınız *Create. cshtml* sayfasının bir bölümünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2435-143">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="b2435-144">İlk formu görüntülemek ve bir hata durumunda formu yeniden görüntülemek için sayfa oluşturma ve düzenleme sayfaları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2435-144">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="b2435-145">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="b2435-145">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="b2435-146">[Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b2435-146">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="b2435-147">Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .</span><span class="sxs-lookup"><span data-stu-id="b2435-147">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="b2435-148">Oluşturma ve düzenleme sayfalarında hiçbir doğrulama kuralı yoktur.</span><span class="sxs-lookup"><span data-stu-id="b2435-148">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="b2435-149">Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b2435-149">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="b2435-150">Bu doğrulama kuralları, `Movie` modeli düzenlediğiniz Razor Pages otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-150">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="b2435-151">Doğrulama mantığının değişmesi gerektiğinde, yalnızca modelde yapılır.</span><span class="sxs-lookup"><span data-stu-id="b2435-151">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="b2435-152">Doğrulama, uygulamanın tamamında tutarlı bir şekilde uygulanır (doğrulama mantığı tek bir yerde tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="b2435-152">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="b2435-153">Tek bir yerde doğrulama, kodun temiz kalmasına yardımcı olur ve bakım ve güncelleştirme işlemlerini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b2435-153">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="b2435-154">DataType özniteliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="b2435-154">Using DataType Attributes</span></span>

<span data-ttu-id="b2435-155">`Movie` Sınıfını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="b2435-155">Examine the `Movie` class.</span></span> <span data-ttu-id="b2435-156">Ad `System.ComponentModel.DataAnnotations` alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2435-156">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="b2435-157">`DataType` `ReleaseDate` Özniteliği ve`Price` özelliklerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-157">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="b2435-158">Öznitelikler yalnızca görünüm altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL 'ler ve `<a href="mailto:EmailAddress.com">` e-posta için gibi `<a>` öznitelikleri sağlar). `DataType`</span><span class="sxs-lookup"><span data-stu-id="b2435-158">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="b2435-159">Veri biçimini doğrulamak için özniteliğinikullanın.`RegularExpression`</span><span class="sxs-lookup"><span data-stu-id="b2435-159">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="b2435-160">`DataType` Özniteliği, veritabanı iç türünden daha belirgin bir veri türü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2435-160">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="b2435-161">`DataType`Öznitelikler, doğrulama öznitelikleri değildir.</span><span class="sxs-lookup"><span data-stu-id="b2435-161">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="b2435-162">Örnek uygulamada, yalnızca tarih ve saat olmadan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b2435-162">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="b2435-163">`DataType` Sabit listesi, tarih, saat, PhoneNumber, para birimi, emaadresi gibi birçok veri türünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2435-163">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="b2435-164">`DataType` Özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-164">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="b2435-165">Örneğin, için `DataType.EmailAddress`bir `mailto:` bağlantı oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-165">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="b2435-166">HTML5 'i destekleyen tarayıcılarda için `DataType.Date` bir tarih seçici sağlanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-166">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="b2435-167">Öznitelikler HTML 5 tarayıcılarının kullandığı `data-` HTML 5 (bir veri Dash) özniteliklerini yayar. `DataType`</span><span class="sxs-lookup"><span data-stu-id="b2435-167">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="b2435-168">Öznitelikler herhangi bir **doğrulama sağlamaz.** `DataType`</span><span class="sxs-lookup"><span data-stu-id="b2435-168">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="b2435-169">`DataType.Date`görüntülenen tarihin biçimini belirtmez.</span><span class="sxs-lookup"><span data-stu-id="b2435-169">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="b2435-170">Varsayılan olarak, veri alanı sunucu `CultureInfo`' a göre varsayılan biçimlere göre görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b2435-170">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="b2435-171">Entity Framework Core veri ek açıklaması gerekir, bu nedenle veritabanında para birimiyle doğru şekilde eşleşebilirler `Price`. `[Column(TypeName = "decimal(18, 2)")]`</span><span class="sxs-lookup"><span data-stu-id="b2435-171">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="b2435-172">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="b2435-172">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="b2435-173">`DisplayFormat` Öznitelik, tarih biçimini açıkça belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b2435-173">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="b2435-174">`ApplyFormatInEditMode` Ayar, değer düzenlenmek üzere görüntülendiğinde biçimlendirmenin uygulanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="b2435-174">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="b2435-175">Bazı alanlar için bu davranışı istemiyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2435-175">You might not want that behavior for some fields.</span></span> <span data-ttu-id="b2435-176">Örneğin, para birimi değerlerinde, büyük olasılıkla düzenleme kullanıcı arabirimindeki para birimi sembolünü istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2435-176">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="b2435-177">Özniteliği kendisi tarafından kullanılabilir, ancak genellikle `DataType` özniteliği kullanmak iyi bir fikir olabilir. `DisplayFormat`</span><span class="sxs-lookup"><span data-stu-id="b2435-177">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="b2435-178">`DataType` Özniteliği, bir ekranda nasıl işlenirim aksine, verilerin semantiğini alır ve DisplayFormat ile elde olmadığınız avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="b2435-178">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="b2435-179">Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)</span><span class="sxs-lookup"><span data-stu-id="b2435-179">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="b2435-180">Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-180">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="b2435-181">`DataType` Öznitelik, ASP.NET Core çerçevesinin verileri işlemek için doğru alan şablonunu seçmesini sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b2435-181">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="b2435-182">Kendisi `DisplayFormat` tarafından kullanılıyorsa, dize şablonunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="b2435-182">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="b2435-183">Note: jQuery doğrulaması, `Range` ve `DateTime`özniteliğiyle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="b2435-183">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="b2435-184">Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b2435-184">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="b2435-185">Genellikle, modellerinizde sabit tarihleri derlemek iyi bir uygulamadır, bu nedenle `Range` özniteliğini kullanarak ve `DateTime` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="b2435-185">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="b2435-186">Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="b2435-186">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="b2435-187">[Razor Pages kullanmaya başlayın ve EF Core](xref:data/ef-rp/intro) gelişmiş EF Core işlemlerini Razor Pages gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2435-187">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="b2435-188">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="b2435-188">Publish to Azure</span></span>

<span data-ttu-id="b2435-189">Azure 'a dağıtma hakkında bilgi için bkz [. Öğretici: SQL veritabanı](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase)ile Azure 'da bir ASP.NET uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b2435-189">For information on deploying to Azure, see [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="b2435-190">Bu yönergeler bir ASP.NET Core uygulaması değil, bir ASP.NET uygulaması içindir, ancak adımlar aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b2435-190">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="b2435-191">Razor Pages için bu giriş tamamlanırken teşekkürler.</span><span class="sxs-lookup"><span data-stu-id="b2435-191">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="b2435-192">[Razor Pages kullanmaya başlayın ve](xref:data/ef-rp/intro) Bu öğreticiye en uygun harika bir izleme EF Core.</span><span class="sxs-lookup"><span data-stu-id="b2435-192">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2435-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b2435-193">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="b2435-194">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="b2435-194">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="b2435-195">Öncekini Yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="b2435-195">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
