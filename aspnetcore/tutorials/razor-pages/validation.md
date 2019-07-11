---
title: Doğrulama için bir ASP.NET Core Razor sayfası ekleme
author: rick-anderson
description: ASP.NET Core Razor sayfasına doğrulamanın nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 8495849c89ca3d6fd2b2006b61ce2ec75ff504a5
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815647"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="0f9da-103">Doğrulama için bir ASP.NET Core Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="0f9da-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="0f9da-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0f9da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0f9da-105">Bu bölümde, doğrulama mantığı eklenen `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="0f9da-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="0f9da-106">Doğrulama kuralları, bir kullanıcı oluşturur veya düzenler film dilediğiniz zaman uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="0f9da-107">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="0f9da-107">Validation</span></span>

<span data-ttu-id="0f9da-108">Bir anahtar uyarlamanız yazılım geliştirme adlı [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**aşağıdakilerden **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="0f9da-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="0f9da-109">Razor sayfaları burada işlevselliği bir kez belirtildiğinden ve uygulama boyunca yansıtılır geliştirme teşvik eder.</span><span class="sxs-lookup"><span data-stu-id="0f9da-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="0f9da-110">KURU yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="0f9da-110">DRY can help:</span></span>

* <span data-ttu-id="0f9da-111">Bir uygulamada kod miktarını azaltır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="0f9da-112">Kodu daha az hata yapmaya açık ve test etmek ve sürdürmek daha kolay olun.</span><span class="sxs-lookup"><span data-stu-id="0f9da-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="0f9da-113">Razor sayfaları ve Entity Framework tarafından sağlanan doğrulama desteği, KURU İlkesi iyi bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="0f9da-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="0f9da-114">Doğrulama kuralları (model sınıfında) tek bir yerde bildirimli olarak belirtilir ve kuralları uygulamada her yerde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="0f9da-115">Razor sayfaları kullanıcı Arabiriminde doğrulama hatası</span><span class="sxs-lookup"><span data-stu-id="0f9da-115">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="0f9da-116">Uygulamayı çalıştırın ve sayfaları/filmler gidin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-116">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="0f9da-117">Seçin **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="0f9da-117">Select the **Create New** link.</span></span> <span data-ttu-id="0f9da-118">Bazı geçersiz değerlerle formu doldurun.</span><span class="sxs-lookup"><span data-stu-id="0f9da-118">Complete the form with some invalid values.</span></span> <span data-ttu-id="0f9da-119">JQuery istemci tarafı doğrulama hatası algıladığında, bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0f9da-119">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatalarını film görünüm formu](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="0f9da-121">Form otomatik olarak işlenen geçersiz bir değer içeren her bir alan bir doğrulama hatası iletisini nasıl dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-121">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="0f9da-122">(Bir kullanıcının JavaScript devre dışı olduğunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-122">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="0f9da-123">Önemli faydası ise **hiçbir** kod değişiklikleri oluşturma veya düzenleme sayfalarında gerekli.</span><span class="sxs-lookup"><span data-stu-id="0f9da-123">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="0f9da-124">DataAnnotations modele uygulandı sonra kullanıcı Arabirimi doğrulama etkinleştirildi.</span><span class="sxs-lookup"><span data-stu-id="0f9da-124">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="0f9da-125">Bu öğreticide otomatik olarak oluşturulan Razor sayfaları, doğrulama kurallarını çekilen (özelliklerini doğrulama öznitelikleri kullanarak `Movie` model sınıfı).</span><span class="sxs-lookup"><span data-stu-id="0f9da-125">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="0f9da-126">Aynı doğrulama düzenleme sayfasını kullanarak test doğrulama uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-126">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="0f9da-127">Form verileri, istemci tarafı doğrulama hata kalmayana kadar sunucusuna gönderilen değil.</span><span class="sxs-lookup"><span data-stu-id="0f9da-127">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="0f9da-128">Form verileri bir veya daha fazla aşağıdaki yaklaşımlardan tarafından gönderilen değil doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="0f9da-128">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="0f9da-129">Bir kesme noktası koymak `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0f9da-129">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="0f9da-130">Form gönderilemiyor (seçin **Oluştur** veya **Kaydet**).</span><span class="sxs-lookup"><span data-stu-id="0f9da-130">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="0f9da-131">Kesme noktası hiçbir zaman ulaşılır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-131">The break point is never hit.</span></span>
* <span data-ttu-id="0f9da-132">Kullanım [Fiddler aracı](https://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="0f9da-132">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="0f9da-133">Ağ trafiğini izlemek için tarayıcı geliştirici araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="0f9da-133">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="0f9da-134">Sunucu tarafı doğrulama</span><span class="sxs-lookup"><span data-stu-id="0f9da-134">Server-side validation</span></span>

<span data-ttu-id="0f9da-135">Tarayıcıda JavaScript devre dışı bırakıldığında hatalarla formu göndermeden sunucuya gönderin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-135">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="0f9da-136">İsteğe bağlı, test sunucu tarafı doğrulama:</span><span class="sxs-lookup"><span data-stu-id="0f9da-136">Optional, test server-side validation:</span></span>

* <span data-ttu-id="0f9da-137">Tarayıcıda JavaScript devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="0f9da-137">Disable JavaScript in the browser.</span></span> <span data-ttu-id="0f9da-138">Tarayıcınızın geliştirici araçlarını kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-138">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="0f9da-139">Tarayıcıda JavaScript devre dışı bırakamazsınız, başka bir tarayıcı deneyin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-139">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="0f9da-140">Bir kesme noktası kümesinde `OnPostAsync` yöntemi oluşturma veya düzenleme sayfası.</span><span class="sxs-lookup"><span data-stu-id="0f9da-140">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="0f9da-141">Bir form doğrulama hatalarını gönderin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-141">Submit a form with validation errors.</span></span>
* <span data-ttu-id="0f9da-142">Model durumu geçersiz doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="0f9da-142">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="0f9da-143">Aşağıdaki kod bir bölümü gösterilmektedir *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş sayfası.</span><span class="sxs-lookup"><span data-stu-id="0f9da-143">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="0f9da-144">İlk formu görüntülemek ve bir hata olması durumunda formu görüntülemek için oluşturma ve düzenleme sayfaları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-144">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="0f9da-145">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0f9da-145">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="0f9da-146">[Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0f9da-146">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="0f9da-147">Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0f9da-147">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="0f9da-148">Oluşturma ve düzenleme sayfaları hiçbir doğrulama kuralları olması.</span><span class="sxs-lookup"><span data-stu-id="0f9da-148">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="0f9da-149">Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0f9da-149">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="0f9da-150">Bu doğrulama kuralları Düzenle Razor sayfaları için otomatik olarak uygulanacağını da `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="0f9da-150">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="0f9da-151">Doğrulama mantığını değişmesi gerektiğinde, yalnızca model içinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-151">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="0f9da-152">Doğrulama uygulama genelinde tutarlı olarak uygulandığından (tek bir yerde Doğrulama mantığı tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="0f9da-152">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="0f9da-153">Tek bir yerde doğrulama kodu temiz tutmaya yardımcı olur ve bakımını ve güncelleştirmesini yapmak kolaylaşır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-153">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="0f9da-154">Veri türü öznitelikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="0f9da-154">Using DataType Attributes</span></span>

<span data-ttu-id="0f9da-155">İnceleme `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0f9da-155">Examine the `Movie` class.</span></span> <span data-ttu-id="0f9da-156">`System.ComponentModel.DataAnnotations` Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f9da-156">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="0f9da-157">`DataType` Özniteliği uygulandığı `ReleaseDate` ve `Price` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="0f9da-157">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="0f9da-158">`DataType` Öznitelikler yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öznitelikleri gibi kaynakları `<a>` URL'leri için ve `<a href="mailto:EmailAddress.com">` e-posta için).</span><span class="sxs-lookup"><span data-stu-id="0f9da-158">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="0f9da-159">Kullanım `RegularExpression` verilerin biçimi doğrulamak için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f9da-159">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="0f9da-160">`DataType` Özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-160">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="0f9da-161">`DataType` öznitelikleri doğrulama özniteliklerinin değildir.</span><span class="sxs-lookup"><span data-stu-id="0f9da-161">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="0f9da-162">Örnek uygulama, yalnızca tarih, saat görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0f9da-162">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="0f9da-163">`DataType` Tarih, saat, telefon numarası, para birimi, EmailAddress ve diğer birçok veri türlerini numaralandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f9da-163">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="0f9da-164">`DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-164">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="0f9da-165">Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="0f9da-165">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="0f9da-166">Bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="0f9da-166">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="0f9da-167">`DataType` Öznitelikleri yayan HTML 5 `data-` HTML 5 tarayıcılar kullanma (okunur veri dash) öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="0f9da-167">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="0f9da-168">`DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f9da-168">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="0f9da-169">`DataType.Date` Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="0f9da-169">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="0f9da-170">Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="0f9da-170">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="0f9da-171">`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama, Entity Framework Core doğru şekilde eşleyebilirsiniz biçimde gereklidir `Price` veritabanında para birimi.</span><span class="sxs-lookup"><span data-stu-id="0f9da-171">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="0f9da-172">Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="0f9da-172">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="0f9da-173">`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0f9da-173">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="0f9da-174">`ApplyFormatInEditMode` Ayar değeri düzenlemek için görüntülendiğinde biçimlendirme uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="0f9da-174">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="0f9da-175">Bazı alanlar için bu davranışı istemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-175">You might not want that behavior for some fields.</span></span> <span data-ttu-id="0f9da-176">Örneğin, para birimi değerleri düzenleme UI para birimi sembolü büyük olasılıkla istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-176">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="0f9da-177">`DisplayFormat` Özniteliği tek başına kullanılabilir, ancak genel olarak kullanmak iyi bir fikir olduğunu `DataType` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f9da-177">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="0f9da-178">`DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="0f9da-178">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="0f9da-179">Tarayıcı HTML5 özelliklerini (örneğin bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, vb. gösterilecek) etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-179">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="0f9da-180">Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.</span><span class="sxs-lookup"><span data-stu-id="0f9da-180">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="0f9da-181">`DataType` Öznitelik verileri işlemek için sağ alanı şablon seçmek ASP.NET Core framework etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-181">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="0f9da-182">`DisplayFormat` Tarafından kullanılan, kendi dize şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-182">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="0f9da-183">Not: jQuery doğrulama ile işe yaramazsa `Range` özniteliği ve `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="0f9da-183">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="0f9da-184">Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kod her zaman bir istemci tarafı doğrulama hatası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0f9da-184">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="0f9da-185">Bu genellikle Sabit tarihler kullanarak Modellerinizi derlemek için iyi bir uygulama değildir `Range` özniteliği ve `DateTime` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="0f9da-185">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="0f9da-186">Aşağıdaki kod, bir satır birleştirme öznitelikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="0f9da-186">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="0f9da-187">[Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) EF çekirdekli Razor sayfaları işlemleriyle Gelişmiş gösterir.</span><span class="sxs-lookup"><span data-stu-id="0f9da-187">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="0f9da-188">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="0f9da-188">Publish to Azure</span></span>

<span data-ttu-id="0f9da-189">Azure'a dağıtma hakkında daha fazla bilgi için bkz. [Öğreticisi: Azure'da SQL veritabanı ile ASP.NET uygulaması derleme](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="0f9da-189">For information on deploying to Azure, see [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="0f9da-190">Bu yönergeler, bir ASP.NET uygulaması için bir ASP.NET Core uygulaması içindir ancak adımlar aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0f9da-190">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="0f9da-191">Razor sayfaları giriş tamamlamak için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="0f9da-191">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0f9da-192">[Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) olan Bu öğreticide kadar mükemmel bir izleyin.</span><span class="sxs-lookup"><span data-stu-id="0f9da-192">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f9da-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0f9da-193">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="0f9da-194">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="0f9da-194">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="0f9da-195">Önceki: Yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="0f9da-195">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
