---
title: Bir ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına doğrulama ekleme bulur.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/validation
ms.openlocfilehash: cabf3d955ef2eb17b3bcb40170a9de7b53ffd107
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077637"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="ab559-103">Bir ASP.NET Core Razor sayfasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="ab559-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="ab559-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab559-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab559-105">Bu bölümde, doğrulama mantığını eklenen `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="ab559-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="ab559-106">Doğrulama kuralları, bir kullanıcı oluşturur veya bir filmi düzenler dilediğiniz zaman uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ab559-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="ab559-107">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="ab559-107">Validation</span></span>

<span data-ttu-id="ab559-108">Yazılım geliştirme anahtar tenet adlı [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**aşağıdakilerden **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="ab559-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="ab559-109">Razor sayfalarının burada işlevselliği bir kez belirtildiğinden ve uygulama boyunca yansıtılır geliştirme önerir.</span><span class="sxs-lookup"><span data-stu-id="ab559-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="ab559-110">KURU uygulama kodunda sayısını azaltmanıza yardımcı.</span><span class="sxs-lookup"><span data-stu-id="ab559-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="ab559-111">KURU kod daha az hata eğilimindedir ve test ve sürdürmek daha kolay yapar.</span><span class="sxs-lookup"><span data-stu-id="ab559-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="ab559-112">Razor sayfalarının ve Entity Framework tarafından sağlanan doğrulama desteği KURU İlkesi iyi bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="ab559-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="ab559-113">Doğrulama kuralları bildirimli olarak tek bir yerde (modeli sınıfı) belirtilir ve kuralların her yerde uygulamada uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ab559-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="ab559-114">Film model için doğrulama kuralları ekleme</span><span class="sxs-lookup"><span data-stu-id="ab559-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="ab559-115">Açık *Movie.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="ab559-115">Open the *Movie.cs* file.</span></span> <span data-ttu-id="ab559-116">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) yerleşik bir sınıf veya özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-116">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="ab559-117">DataAnnotations de içeren biçimlendirme öznitelikleri gibi `DataType` Yardım biçimlendirmesiyle ve doğrulama sağlaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ab559-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="ab559-118">Güncelleştirme `Movie` yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="ab559-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs)]

::: moniker-end

<span data-ttu-id="ab559-119">Doğrulama öznitelikleri zorlanır davranışı model özellikleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="ab559-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="ab559-120">`Required` Ve `MinimumLength` öznitelikleri gösteren bir özelliği bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ab559-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="ab559-121">Ancak, hiçbir şey kullanıcı null atanabilir bir tür için doğrulama kısıtlamasını boşluk geçmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="ab559-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="ab559-122">Olamayan [değer türleri](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ab559-122">Non-nullable [value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="ab559-123">`RegularExpression` Öznitelik, kullanıcının girebileceği karakterleri sınırlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="ab559-124">Önceki kod `Genre` bir veya daha fazla büyük harf ile başlamalı ve sıfır veya daha fazla harf, tek veya çift tırnak, boşluk karakterleri veya kısa çizgi ile izleyin.</span><span class="sxs-lookup"><span data-stu-id="ab559-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="ab559-125">`Rating` bir veya daha fazla büyük harf ile başlamalı ve ile sıfır veya daha fazla harf, rakam, tek veya çift tırnak, boşluk karakterleri veya tire izleyin.</span><span class="sxs-lookup"><span data-stu-id="ab559-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="ab559-126">`Range` Özniteliği belirli bir aralık için bir değer kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="ab559-127">`StringLength` Öznitelik, bir dize uzunluğu en fazla ve isteğe bağlı olarak en az uzunluk ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="ab559-128">Doğrulama kuralları otomatik olarak ASP.NET Core tarafından zorlanan sahip yardımcı olan bir uygulama daha sağlam hale.</span><span class="sxs-lookup"><span data-stu-id="ab559-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="ab559-129">Otomatik doğrulama modellerinde yeni kod eklendiğinde, bunları uygulamak olmadığından uygulama korunmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="ab559-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="ab559-130">Doğrulama hatası Razor sayfalarında kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="ab559-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="ab559-131">Uygulamayı çalıştırın ve sayfaları/filmlere gidin.</span><span class="sxs-lookup"><span data-stu-id="ab559-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="ab559-132">Seçin **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ab559-132">Select the **Create New** link.</span></span> <span data-ttu-id="ab559-133">Bazı geçersiz değerlerle formu doldurun.</span><span class="sxs-lookup"><span data-stu-id="ab559-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="ab559-134">JQuery istemci tarafı doğrulama hatası algıladığında, bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ab559-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Film birden çok jQuery istemci tarafı doğrulama hataları olan formu görüntüle](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="ab559-136">Ondalık ayırıcıların veya virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="ab559-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="ab559-137">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce dışındaki yerel ayarlarda (",") ondalık ve ABD İngilizcesi dışındaki tarih biçimleri için uygulamanızı globalize için adımlar atmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ab559-137">To support [jQuery validation](https://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="ab559-138">Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ab559-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="ab559-139">Şimdilik, yalnızca tam sayılar 10 gibi girin.</span><span class="sxs-lookup"><span data-stu-id="ab559-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="ab559-140">Form otomatik olarak işlenen geçersiz bir değer içeren her bir alan bir doğrulama hata iletisine nasıl dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ab559-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="ab559-141">(Bir kullanıcının JavaScript devre dışı olduğunda) hataları istemci-tarafı (JavaScript ve Jquery'de kullanarak) ve sunucu tarafı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ab559-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="ab559-142">Önemli faydası ise **hiçbir** kod değişiklikleri oluşturma veya düzenleme sayfalarında gerekli.</span><span class="sxs-lookup"><span data-stu-id="ab559-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="ab559-143">DataAnnotations modeline uygulanan sonra UI doğrulama etkinleştirildi.</span><span class="sxs-lookup"><span data-stu-id="ab559-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="ab559-144">Bu öğreticide otomatik olarak oluşturulan Razor sayfalarının çekilen doğrulama kurallarını (doğrulama öznitelikleri özelliklerini kullanarak `Movie` model sınıfı).</span><span class="sxs-lookup"><span data-stu-id="ab559-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="ab559-145">Sınama doğrulaması aynı doğrulama düzen sayfası kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ab559-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="ab559-146">İstemci tarafı doğrulama hataları kadar form verilerini sunucuya gönderilen değil.</span><span class="sxs-lookup"><span data-stu-id="ab559-146">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="ab559-147">Form verileri bir veya daha fazla aşağıdaki yaklaşımlardan tarafından gönderilen değil doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="ab559-147">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="ab559-148">Bir kesme noktası koyun `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ab559-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="ab559-149">Form gönderme (seçin **oluşturma** veya **kaydetmek**).</span><span class="sxs-lookup"><span data-stu-id="ab559-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="ab559-150">Kesme noktası hiçbir zaman ulaştı.</span><span class="sxs-lookup"><span data-stu-id="ab559-150">The break point is never hit.</span></span>
* <span data-ttu-id="ab559-151">Kullanım [Fiddler aracı](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="ab559-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="ab559-152">Tarayıcının geliştirici araçları, ağ trafiğini izlemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ab559-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="ab559-153">Sunucu tarafında doğrulama</span><span class="sxs-lookup"><span data-stu-id="ab559-153">Server-side validation</span></span>

<span data-ttu-id="ab559-154">JavaScript tarayıcıda devre dışı bırakıldığında, hatalarla form gönderme sunucuya gönderin.</span><span class="sxs-lookup"><span data-stu-id="ab559-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="ab559-155">İsteğe bağlı, test sunucu tarafında doğrulama:</span><span class="sxs-lookup"><span data-stu-id="ab559-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="ab559-156">JavaScript tarayıcıda devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="ab559-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="ab559-157">Tarayıcıda JavaScript devre dışı bırakamazsınız, başka bir tarayıcı deneyin.</span><span class="sxs-lookup"><span data-stu-id="ab559-157">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="ab559-158">Bir kesme noktası kümesinde `OnPostAsync` oluşturma veya düzenleme sayfasının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ab559-158">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="ab559-159">Doğrulama hataları olan bir form gönderme.</span><span class="sxs-lookup"><span data-stu-id="ab559-159">Submit a form with validation errors.</span></span>
* <span data-ttu-id="ab559-160">Model durumu geçersiz doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="ab559-160">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="ab559-161">Aşağıdaki kod bir kısmı gösterir *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş sayfası.</span><span class="sxs-lookup"><span data-stu-id="ab559-161">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="ab559-162">İlk form görüntülemek ve bir hata durumunda formu yeniden görüntüleyin, oluşturma ve düzenleme sayfaları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab559-162">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="ab559-163">[Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve istemci tarafında jQuery doğrulama için gereken HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="ab559-163">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="ab559-164">[Doğrulama etiket Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ab559-164">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="ab559-165">Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ab559-165">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="ab559-166">Oluşturma ve düzenleme sayfaları hiçbir doğrulama kuralları olması.</span><span class="sxs-lookup"><span data-stu-id="ab559-166">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="ab559-167">Doğrulama kuralları ve hata dizesi yalnızca belirtilen `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ab559-167">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="ab559-168">Bu doğrulama kuralları otomatik olarak Düzenle Razor sayfalara uygulanır `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="ab559-168">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="ab559-169">Doğrulama mantığını değiştirmek gerektiğinde, model yalnızca yapılır.</span><span class="sxs-lookup"><span data-stu-id="ab559-169">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="ab559-170">Doğrulama uygulama genelinde tutarlı olarak uygulandığı (tek bir yerde doğrulama mantığını tanımlanmış).</span><span class="sxs-lookup"><span data-stu-id="ab559-170">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="ab559-171">Tek bir yerde doğrulama kodu temiz tutmaya yardımcı olur ve bakımını yapmak ve güncelleştirmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ab559-171">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="ab559-172">Veri türü öznitelikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="ab559-172">Using DataType Attributes</span></span>

<span data-ttu-id="ab559-173">İncelemek `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ab559-173">Examine the `Movie` class.</span></span> <span data-ttu-id="ab559-174">`System.ComponentModel.DataAnnotations` Ad alanı, yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-174">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="ab559-175">`DataType` Özniteliği uygulanan `ReleaseDate` ve `Price` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="ab559-175">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="ab559-176">`DataType` Öznitelikler verilerin biçimlendirilmesi görünüm altyapısı için ipuçları yalnızca sağlar (ve öznitelikler gibi kaynakları `<a>` URL'SİNİN için ve `<a href="mailto:EmailAddress.com">` e-posta için).</span><span class="sxs-lookup"><span data-stu-id="ab559-176">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="ab559-177">Kullanım `RegularExpression` veri biçimi doğrulamak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="ab559-177">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="ab559-178">`DataType` Özniteliği veritabanı geçerli bir tür daha fazla belirli bir veri türünü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ab559-178">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="ab559-179">`DataType` öznitelikler doğrulama öznitelikleri değildir.</span><span class="sxs-lookup"><span data-stu-id="ab559-179">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="ab559-180">Örnek uygulama, yalnızca tarih, saat görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ab559-180">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="ab559-181">`DataType` Tarih, saat, PhoneNumber, para birimi, EmailAddress ve daha fazla gibi birçok veri türleri için numaralandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-181">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="ab559-182">`DataType` Özniteliği de otomatik olarak türüne özgü özellikleri sağlamak uygulama etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="ab559-182">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="ab559-183">Örneğin, bir `mailto:` bağlantı için oluşturulabilir `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="ab559-183">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="ab559-184">Bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda.</span><span class="sxs-lookup"><span data-stu-id="ab559-184">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="ab559-185">`DataType` Öznitelikleri yayar HTML 5 `data-` HTML 5 tarayıcılar tüketebilir (okunur veri tire) öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="ab559-185">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="ab559-186">`DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab559-186">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="ab559-187">`DataType.Date` Görüntülenen tarih biçimi belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="ab559-187">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="ab559-188">Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="ab559-188">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="ab559-189">`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklamasını, Entity Framework Çekirdek doğru eşleyebilir gereklidir `Price` veritabanındaki para birimi.</span><span class="sxs-lookup"><span data-stu-id="ab559-189">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="ab559-190">Daha fazla bilgi için bkz: [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="ab559-190">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>
::: moniker-end

<span data-ttu-id="ab559-191">`DisplayFormat` Özniteliği açıkça tarih biçimini belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="ab559-191">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="ab559-192">`ApplyFormatInEditMode` Ayarı, düzenleme için değer görüntülendiğinde biçimlendirme uygulanması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="ab559-192">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="ab559-193">Bazı alanları için bu davranışı istemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab559-193">You might not want that behavior for some fields.</span></span> <span data-ttu-id="ab559-194">Örneğin, para birimi değerleri düzenleme UI para birimi simgesini büyük olasılıkla istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab559-194">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="ab559-195">`DisplayFormat` Özniteliği tek başına kullanılabilir, ancak genellikle kullanmak iyi bir fikir değil `DataType` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ab559-195">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="ab559-196">`DataType` Özniteliği bir ekranda işlemesini nasıl aksine veri semantiği iletir ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="ab559-196">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="ab559-197">Tarayıcı HTML5 özellikleri (örneğin. bir Takvim denetimi, yerel ayar uygun para birimi simgesini, e-posta bağlantılar, vb. göstermek) etkinleştirebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="ab559-197">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="ab559-198">Varsayılan olarak, tarayıcı, bölgesel ayarına göre doğru biçimi kullanarak bir veri oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="ab559-198">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="ab559-199">`DataType` Özniteliği verileri işlemek için sağ alan şablon seçmek ASP.NET Core framework etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ab559-199">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="ab559-200">`DisplayFormat` Tarafından kullanılan kendisini dize şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="ab559-200">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="ab559-201">Not: jQuery doğrulama ile işe yaramazsa `Range` özniteliği ve `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="ab559-201">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="ab559-202">Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kodu her zaman bir istemci tarafı doğrulama hatası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ab559-202">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="ab559-203">Bu genellikle bunu kullanarak Modellerinizi sabit tarihler derlemek için iyi bir uygulama değil `Range` özniteliği ve `DateTime` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="ab559-203">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="ab559-204">Aşağıdaki kod, tek bir satırda birleştirme öznitelikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="ab559-204">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ab559-205">[Razor sayfalarının ve EF çekirdek kullanmaya başlama](xref:data/ef-rp/intro) Razor sayfalarının EF çekirdek işlemleriyle Gelişmiş gösterir.</span><span class="sxs-lookup"><span data-stu-id="ab559-205">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="ab559-206">Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="ab559-206">Publish to Azure</span></span>

<span data-ttu-id="ab559-207">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Azure yayımlama hakkında yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="ab559-207">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab559-208">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ab559-208">Additional resources</span></span>

* [<span data-ttu-id="ab559-209">Formlarla Çalışma</span><span class="sxs-lookup"><span data-stu-id="ab559-209">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="ab559-210">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="ab559-210">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="ab559-211">Etiket Yardımcıları giriş</span><span class="sxs-lookup"><span data-stu-id="ab559-211">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="ab559-212">Yazar etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="ab559-212">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> <span data-ttu-id="ab559-213">[Önceki: yeni bir alan ekleyerek](xref:tutorials/razor-pages/new-field)
> [sonraki: dosyaları karşıya yükleme](xref:tutorials/razor-pages/uploading-files)</span><span class="sxs-lookup"><span data-stu-id="ab559-213">[Previous: Adding a new field](xref:tutorials/razor-pages/new-field)
[Next: Uploading files](xref:tutorials/razor-pages/uploading-files)</span></span>
