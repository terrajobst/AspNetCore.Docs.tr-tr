---
title: Doğrulama için bir ASP.NET Core Razor sayfası ekleme
author: rick-anderson
description: ASP.NET Core Razor sayfasına doğrulamanın nasıl keşfedin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: f46bddb618d2a030e29b7dfa1671ea53b0d4bcc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021358"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="2f230-103">Doğrulama için bir ASP.NET Core Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="2f230-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="2f230-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f230-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2f230-105">Bu bölümde, doğrulama mantığı eklenen `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="2f230-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="2f230-106">Doğrulama kuralları, bir kullanıcı oluşturur veya düzenler film dilediğiniz zaman uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f230-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="2f230-107">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="2f230-107">Validation</span></span>

<span data-ttu-id="2f230-108">Bir anahtar uyarlamanız yazılım geliştirme adlı [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**aşağıdakilerden **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="2f230-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="2f230-109">Razor sayfaları burada işlevselliği bir kez belirtildiğinden ve uygulama boyunca yansıtılır geliştirme teşvik eder.</span><span class="sxs-lookup"><span data-stu-id="2f230-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="2f230-110">KURU, bir uygulamada kod miktarını azaltmaya yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2f230-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="2f230-111">KURU kod daha az hata yapmaya açık ve test etmek ve sürdürmek daha kolay sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="2f230-112">Razor sayfaları ve Entity Framework tarafından sağlanan doğrulama desteği, KURU İlkesi iyi bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="2f230-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="2f230-113">Doğrulama kuralları (model sınıfında) tek bir yerde bildirimli olarak belirtilir ve kuralları uygulamada her yerde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f230-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="2f230-114">Film modeli doğrulama kuralları ekleme</span><span class="sxs-lookup"><span data-stu-id="2f230-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="2f230-115">Açık *Models/Movie.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="2f230-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="2f230-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) yerleşik bir sınıf ya da özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="2f230-117">DataAnnotations gibi biçimlendirme öznitelikleri de içeren `DataType` biçimlendirmesinde yardımcı olabilecek ve doğrulama sağlaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2f230-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="2f230-118">Güncelleştirme `Movie` yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="2f230-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2f230-119">Doğrulama özniteliklerinin zorlanan davranış modeli özellikleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="2f230-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="2f230-120">`Required` Ve `MinimumLength` öznitelikleri belirtmek bir özelliği bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f230-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="2f230-121">Ancak, hiçbir şey kullanıcı boş değer atanabilir bir tür için doğrulama kısıtlamasını karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="2f230-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="2f230-122">Atanamayan [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2f230-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="2f230-123">`RegularExpression` Öznitelik, kullanıcının girebileceği karakter sınırlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="2f230-124">Önceki kodda, `Genre` bir veya daha fazla büyük harf ile başlamalı ve sıfır veya daha fazla harf, tek veya çift tırnak işareti, boşluk karakteri veya tire ile izleyin.</span><span class="sxs-lookup"><span data-stu-id="2f230-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="2f230-125">`Rating` bir veya daha fazla büyük harf ile başlamalı ve ile sıfır veya daha fazla harf, sayı, tek veya çift tırnak, boşluk karakteri veya tire izleyin.</span><span class="sxs-lookup"><span data-stu-id="2f230-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="2f230-126">`Range` Özniteliği için belirtilen bir aralıktaki bir değer kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="2f230-127">`StringLength` Öznitelik, bir dizenin maksimum uzunluğunu ve isteğe bağlı olarak en az uzunluk ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="2f230-128">ASP.NET Core tarafından otomatik olarak uygulanan doğrulama kurallarına sahip bir uygulama daha sağlam hale getirmeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2f230-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="2f230-129">Otomatik doğrulama modeller üzerinde yeni bir kod eklendiğinde uygulanacaklarını hatırlamak zorunda olmadığınız için uygulama koruma yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2f230-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="2f230-130">Razor sayfaları kullanıcı Arabiriminde doğrulama hatası</span><span class="sxs-lookup"><span data-stu-id="2f230-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="2f230-131">Uygulamayı çalıştırın ve sayfaları/filmler gidin.</span><span class="sxs-lookup"><span data-stu-id="2f230-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="2f230-132">Seçin **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2f230-132">Select the **Create New** link.</span></span> <span data-ttu-id="2f230-133">Bazı geçersiz değerlerle formu doldurun.</span><span class="sxs-lookup"><span data-stu-id="2f230-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="2f230-134">JQuery istemci tarafı doğrulama hatası algıladığında, bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2f230-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatalarını film görünüm formu](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="2f230-136">Ondalık nokta veya virgül, girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="2f230-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="2f230-137">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-137">To support [jQuery validation](https://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="2f230-138">Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2f230-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="2f230-139">Şimdilik yalnızca 10 gibi tam sayı girin.</span><span class="sxs-lookup"><span data-stu-id="2f230-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="2f230-140">Form otomatik olarak işlenen geçersiz bir değer içeren her bir alan bir doğrulama hatası iletisini nasıl dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2f230-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="2f230-141">(Bir kullanıcının JavaScript devre dışı olduğunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f230-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="2f230-142">Önemli faydası ise **hiçbir** kod değişiklikleri oluşturma veya düzenleme sayfalarında gerekli.</span><span class="sxs-lookup"><span data-stu-id="2f230-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="2f230-143">DataAnnotations modele uygulandı sonra kullanıcı Arabirimi doğrulama etkinleştirildi.</span><span class="sxs-lookup"><span data-stu-id="2f230-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="2f230-144">Bu öğreticide otomatik olarak oluşturulan Razor sayfaları, doğrulama kurallarını çekilen (özelliklerini doğrulama öznitelikleri kullanarak `Movie` model sınıfı).</span><span class="sxs-lookup"><span data-stu-id="2f230-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="2f230-145">Aynı doğrulama düzenleme sayfasını kullanarak test doğrulama uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2f230-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="2f230-146">Form verileri, istemci tarafı doğrulama hata kalmayana kadar sunucusuna gönderilen değil.</span><span class="sxs-lookup"><span data-stu-id="2f230-146">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="2f230-147">Form verileri bir veya daha fazla aşağıdaki yaklaşımlardan tarafından gönderilen değil doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="2f230-147">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="2f230-148">Bir kesme noktası koymak `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f230-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="2f230-149">Form gönderilemiyor (seçin **Oluştur** veya **Kaydet**).</span><span class="sxs-lookup"><span data-stu-id="2f230-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="2f230-150">Kesme noktası hiçbir zaman ulaşılır.</span><span class="sxs-lookup"><span data-stu-id="2f230-150">The break point is never hit.</span></span>
* <span data-ttu-id="2f230-151">Kullanım [Fiddler aracı](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="2f230-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="2f230-152">Ağ trafiğini izlemek için tarayıcı geliştirici araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2f230-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="2f230-153">Sunucu tarafı doğrulama</span><span class="sxs-lookup"><span data-stu-id="2f230-153">Server-side validation</span></span>

<span data-ttu-id="2f230-154">Tarayıcıda JavaScript devre dışı bırakıldığında hatalarla formu göndermeden sunucuya gönderin.</span><span class="sxs-lookup"><span data-stu-id="2f230-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="2f230-155">İsteğe bağlı, test sunucu tarafı doğrulama:</span><span class="sxs-lookup"><span data-stu-id="2f230-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="2f230-156">Tarayıcıda JavaScript devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="2f230-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="2f230-157">Tarayıcınızın geliştirici araçlarını kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-157">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="2f230-158">Tarayıcıda JavaScript devre dışı bırakamazsınız, başka bir tarayıcı deneyin.</span><span class="sxs-lookup"><span data-stu-id="2f230-158">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="2f230-159">Bir kesme noktası kümesinde `OnPostAsync` yöntemi oluşturma veya düzenleme sayfası.</span><span class="sxs-lookup"><span data-stu-id="2f230-159">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="2f230-160">Bir form doğrulama hatalarını gönderin.</span><span class="sxs-lookup"><span data-stu-id="2f230-160">Submit a form with validation errors.</span></span>
* <span data-ttu-id="2f230-161">Model durumu geçersiz doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="2f230-161">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="2f230-162">Aşağıdaki kod bir bölümü gösterilmektedir *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş sayfası.</span><span class="sxs-lookup"><span data-stu-id="2f230-162">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="2f230-163">İlk formu görüntülemek ve bir hata olması durumunda formu görüntülemek için oluşturma ve düzenleme sayfaları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2f230-163">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="2f230-164">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2f230-164">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="2f230-165">[Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2f230-165">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="2f230-166">Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2f230-166">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="2f230-167">Oluşturma ve düzenleme sayfaları hiçbir doğrulama kuralları olması.</span><span class="sxs-lookup"><span data-stu-id="2f230-167">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="2f230-168">Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2f230-168">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="2f230-169">Bu doğrulama kuralları Düzenle Razor sayfaları için otomatik olarak uygulanacağını da `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="2f230-169">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="2f230-170">Doğrulama mantığını değişmesi gerektiğinde, yalnızca model içinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="2f230-170">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="2f230-171">Doğrulama uygulama genelinde tutarlı olarak uygulandığından (tek bir yerde Doğrulama mantığı tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="2f230-171">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="2f230-172">Tek bir yerde doğrulama kodu temiz tutmaya yardımcı olur ve bakımını ve güncelleştirmesini yapmak kolaylaşır.</span><span class="sxs-lookup"><span data-stu-id="2f230-172">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="2f230-173">Veri türü öznitelikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="2f230-173">Using DataType Attributes</span></span>

<span data-ttu-id="2f230-174">İnceleme `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2f230-174">Examine the `Movie` class.</span></span> <span data-ttu-id="2f230-175">`System.ComponentModel.DataAnnotations` Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-175">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="2f230-176">`DataType` Özniteliği uygulandığı `ReleaseDate` ve `Price` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="2f230-176">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="2f230-177">`DataType` Öznitelikler yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öznitelikleri gibi kaynakları `<a>` URL'leri için ve `<a href="mailto:EmailAddress.com">` e-posta için).</span><span class="sxs-lookup"><span data-stu-id="2f230-177">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="2f230-178">Kullanım `RegularExpression` verilerin biçimi doğrulamak için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2f230-178">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="2f230-179">`DataType` Özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2f230-179">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="2f230-180">`DataType` öznitelikleri doğrulama özniteliklerinin değildir.</span><span class="sxs-lookup"><span data-stu-id="2f230-180">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="2f230-181">Örnek uygulama, yalnızca tarih, saat görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2f230-181">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="2f230-182">`DataType` Tarih, saat, telefon numarası, para birimi, EmailAddress ve diğer birçok veri türlerini numaralandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-182">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="2f230-183">`DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="2f230-183">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="2f230-184">Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="2f230-184">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="2f230-185">Bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="2f230-185">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="2f230-186">`DataType` Öznitelikleri yayan HTML 5 `data-` HTML 5 tarayıcılar kullanma (okunur veri dash) öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="2f230-186">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="2f230-187">`DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f230-187">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="2f230-188">`DataType.Date` Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="2f230-188">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="2f230-189">Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="2f230-189">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2f230-190">`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama, Entity Framework Core doğru şekilde eşleyebilirsiniz biçimde gereklidir `Price` veritabanında para birimi.</span><span class="sxs-lookup"><span data-stu-id="2f230-190">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="2f230-191">Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="2f230-191">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="2f230-192">`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2f230-192">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="2f230-193">`ApplyFormatInEditMode` Ayar değeri düzenlemek için görüntülendiğinde biçimlendirme uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2f230-193">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="2f230-194">Bazı alanlar için bu davranışı istemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-194">You might not want that behavior for some fields.</span></span> <span data-ttu-id="2f230-195">Örneğin, para birimi değerleri düzenleme UI para birimi sembolü büyük olasılıkla istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-195">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="2f230-196">`DisplayFormat` Özniteliği tek başına kullanılabilir, ancak genel olarak kullanmak iyi bir fikir olduğunu `DataType` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2f230-196">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="2f230-197">`DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="2f230-197">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="2f230-198">Tarayıcı HTML5 özelliklerini (örneğin bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, vb. gösterilecek) etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-198">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="2f230-199">Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.</span><span class="sxs-lookup"><span data-stu-id="2f230-199">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="2f230-200">`DataType` Öznitelik verileri işlemek için sağ alanı şablon seçmek ASP.NET Core framework etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f230-200">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="2f230-201">`DisplayFormat` Tarafından kullanılan, kendi dize şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="2f230-201">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="2f230-202">Not: jQuery doğrulama ile işe yaramazsa `Range` özniteliği ve `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="2f230-202">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="2f230-203">Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kod her zaman bir istemci tarafı doğrulama hatası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="2f230-203">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="2f230-204">Bu genellikle Sabit tarihler kullanarak Modellerinizi derlemek için iyi bir uygulama değildir `Range` özniteliği ve `DateTime` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="2f230-204">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="2f230-205">Aşağıdaki kod, bir satır birleştirme öznitelikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="2f230-205">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2f230-206">[Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) EF çekirdekli Razor sayfaları işlemleriyle Gelişmiş gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f230-206">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="2f230-207">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="2f230-207">Publish to Azure</span></span>

<span data-ttu-id="2f230-208">Bkz: Azure'a dağıtma hakkında bilgi [öğretici: azure'da SQL veritabanı ile ASP.NET uygulaması derleme](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="2f230-208">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="2f230-209">Bu yönergeler, bir ASP.NET uygulaması için bir ASP.NET Core uygulaması içindir ancak adımlar aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2f230-209">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="2f230-210">Razor sayfaları giriş tamamlamak için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="2f230-210">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="2f230-211">Geri bildirim için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="2f230-211">We appreciate feedback.</span></span> <span data-ttu-id="2f230-212">[Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) olan Bu öğreticide kadar mükemmel bir izleyin.</span><span class="sxs-lookup"><span data-stu-id="2f230-212">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f230-213">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2f230-213">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="2f230-214">Önceki: yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="2f230-214">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
