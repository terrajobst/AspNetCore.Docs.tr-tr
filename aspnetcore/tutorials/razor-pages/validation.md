---
title: ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına nasıl doğrulama ekleneceğini öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/validation
ms.openlocfilehash: c2397a535fa2c128f18d65323d0f4920af914205
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334214"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="6b46b-103">ASP.NET Core Razor sayfasına doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="6b46b-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="6b46b-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="6b46b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b46b-105">Bu bölümde, `Movie` modeline doğrulama mantığı eklenir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="6b46b-106">Doğrulama kuralları, bir Kullanıcı bir filmi oluşturduğunda veya düzenleişinizde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="6b46b-107">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="6b46b-107">Validation</span></span>

<span data-ttu-id="6b46b-108">Yazılım geliştirmeye yönelik temel bir temel [kuru](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeon **Y**ourself") olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="6b46b-109">Razor Pages, işlevselliği bir kez belirtildiğinde geliştirme ve uygulama genelinde yansıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="6b46b-110">Kuru şu şekilde yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="6b46b-110">DRY can help:</span></span>

* <span data-ttu-id="6b46b-111">Uygulamadaki kod miktarını azaltın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="6b46b-112">Kodu daha az hata haline getirin ve test ve bakım yapmayı kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="6b46b-113">Razor Pages ve Entity Framework tarafından sunulan doğrulama desteği, Kuru ilkesine iyi bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="6b46b-114">Doğrulama kuralları tek bir yerde (model sınıfında) bildirimli olarak belirtilir ve kurallar uygulamada her yerde zorlanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

## <a name="add-validation-rules-to-the-movie-model"></a><span data-ttu-id="6b46b-115">Film modeline doğrulama kuralları ekleme</span><span class="sxs-lookup"><span data-stu-id="6b46b-115">Add validation rules to the movie model</span></span>

<span data-ttu-id="6b46b-116">Dataaçıklamalarda ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-116">The DataAnnotations namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="6b46b-117">Veri açıklamaları, biçimlendirme ile yardım eden ve herhangi bir doğrulama sağlamayan `DataType` gibi biçimlendirme öznitelikleri de içerir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="6b46b-118">@No__t-0 sınıfını, yerleşik `Required`, `StringLength`, `RegularExpression` ve `Range` doğrulama özniteliklerinden faydalanmak için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-118">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="6b46b-119">Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak istediğiniz davranışı belirtir:</span><span class="sxs-lookup"><span data-stu-id="6b46b-119">The validation attributes specify behavior that you want to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="6b46b-120">@No__t-0 ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir; Ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="6b46b-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="6b46b-121">@No__t-0 özniteliği, hangi karakterlerin girişi yapabileceğini sınırlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-121">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="6b46b-122">Yukarıdaki kodda, "tarz":</span><span class="sxs-lookup"><span data-stu-id="6b46b-122">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="6b46b-123">Yalnızca harfler kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-123">Must only use letters.</span></span>
  * <span data-ttu-id="6b46b-124">İlk harfin büyük harfle olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-124">The first letter is required to be uppercase.</span></span> <span data-ttu-id="6b46b-125">Boşluk, sayı ve özel karakterlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6b46b-125">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="6b46b-126">@No__t-0 "derecelendirmesi":</span><span class="sxs-lookup"><span data-stu-id="6b46b-126">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="6b46b-127">İlk karakterin büyük harf olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-127">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="6b46b-128">Sonraki boşlukların içindeki özel karakter ve sayılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-128">Allows special characters and numbers in  subsequent spaces.</span></span> <span data-ttu-id="6b46b-129">"PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="6b46b-129">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="6b46b-130">@No__t-0 özniteliği, bir değeri belirtilen bir Aralık içinde kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-130">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="6b46b-131">@No__t-0 özniteliği, bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-131">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="6b46b-132">Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-132">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="6b46b-133">Doğrulama kurallarının otomatik olarak uygulanmasını ASP.NET Core uygulamanızın daha sağlam olmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="6b46b-133">Having validation rules automatically enforced by ASP.NET Core helps make your app more robust.</span></span> <span data-ttu-id="6b46b-134">Ayrıca, bir şeyi doğrulamayı unutmanızı ve veritabanına yanlışlıkla veri vermemesini de sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-134">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="6b46b-135">Razor Pages 'de doğrulama hatası Kullanıcı arabirimi</span><span class="sxs-lookup"><span data-stu-id="6b46b-135">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="6b46b-136">Uygulamayı çalıştırın ve sayfalar/Filmler ' e gidin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-136">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="6b46b-137">**Yeni oluştur** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-137">Select the **Create New** link.</span></span> <span data-ttu-id="6b46b-138">Formu, bazı geçersiz değerlerle doldurun.</span><span class="sxs-lookup"><span data-stu-id="6b46b-138">Complete the form with some invalid values.</span></span> <span data-ttu-id="6b46b-139">JQuery istemci tarafı doğrulaması hatayı algıladığında, bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6b46b-139">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

<span data-ttu-id="6b46b-141">Formun geçersiz bir değer içeren her alanda otomatik olarak bir doğrulama hata iletisi nasıl oluşturulduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-141">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="6b46b-142">Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (bir Kullanıcı JavaScript devre dışı bırakıldığında) zorlanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-142">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="6b46b-143">Önemli bir avantaj, oluşturma veya düzenleme sayfalarında **hiçbir** kod değişikliği gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-143">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="6b46b-144">Veri ek açıklamaları modele uygulandıktan sonra, doğrulama kullanıcı arabirimi etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-144">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="6b46b-145">Bu öğreticide oluşturulan Razor Pages otomatik olarak doğrulama kurallarını (`Movie` Model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak) otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-145">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="6b46b-146">Düzenleme sayfasını kullanarak doğrulama testi, aynı doğrulama uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-146">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="6b46b-147">Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya nakledilmez.</span><span class="sxs-lookup"><span data-stu-id="6b46b-147">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="6b46b-148">Form verilerinin aşağıdaki yaklaşımlardan bir veya daha fazlası tarafından nakledilmediğinden emin olun:</span><span class="sxs-lookup"><span data-stu-id="6b46b-148">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="6b46b-149">@No__t-0 yöntemine bir kesme noktası koyun.</span><span class="sxs-lookup"><span data-stu-id="6b46b-149">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="6b46b-150">Formu gönder ( **Oluştur** veya **Kaydet**' i seçin).</span><span class="sxs-lookup"><span data-stu-id="6b46b-150">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="6b46b-151">Kesme noktası hiçbir şekilde isabet ettirilmez.</span><span class="sxs-lookup"><span data-stu-id="6b46b-151">The break point is never hit.</span></span>
* <span data-ttu-id="6b46b-152">[Fiddler aracını](https://www.telerik.com/fiddler)kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-152">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="6b46b-153">Ağ trafiğini izlemek için tarayıcı Geliştirici Araçları ' nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-153">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="6b46b-154">Sunucu tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="6b46b-154">Server-side validation</span></span>

<span data-ttu-id="6b46b-155">Tarayıcıda JavaScript devre dışı bırakıldığında, formun hatalarla gönderilmesi sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-155">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="6b46b-156">İsteğe bağlı, test sunucusu-tarafı doğrulaması:</span><span class="sxs-lookup"><span data-stu-id="6b46b-156">Optional, test server-side validation:</span></span>

* <span data-ttu-id="6b46b-157">Tarayıcıda JavaScript 'ı devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-157">Disable JavaScript in the browser.</span></span> <span data-ttu-id="6b46b-158">Tarayıcının geliştirici araçlarını kullanarak JavaScript 'ı devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-158">You can disable JavaScript using browser's developer tools.</span></span> <span data-ttu-id="6b46b-159">Tarayıcıda JavaScript 'ı devre dışı bırakadıysanız başka bir tarayıcı deneyin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-159">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="6b46b-160">Oluşturma veya düzenleme sayfasının `OnPostAsync` yönteminde bir kesme noktası ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-160">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="6b46b-161">Geçersiz verilerle form gönderme.</span><span class="sxs-lookup"><span data-stu-id="6b46b-161">Submit a form with invalid data.</span></span>
* <span data-ttu-id="6b46b-162">Model durumunun geçersiz olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="6b46b-162">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="6b46b-163">Aşağıdaki kod, öğreticide daha önce *Create. cshtml* sayfa scafkatın bir bölümünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-163">The following code shows a portion of the *Create.cshtml* page scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="6b46b-164">İlk formu görüntülemek ve bir hata durumunda formu yeniden görüntülemek için sayfa oluşturma ve düzenleme sayfaları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-164">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="6b46b-165">[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-165">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="6b46b-166">[Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6b46b-166">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="6b46b-167">Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .</span><span class="sxs-lookup"><span data-stu-id="6b46b-167">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="6b46b-168">Oluşturma ve düzenleme sayfalarında hiçbir doğrulama kuralı yoktur.</span><span class="sxs-lookup"><span data-stu-id="6b46b-168">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="6b46b-169">Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-169">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="6b46b-170">Bu doğrulama kuralları, `Movie` modelini düzenleyebilen Razor Pages otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-170">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="6b46b-171">Doğrulama mantığının değişmesi gerektiğinde, yalnızca modelde yapılır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-171">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="6b46b-172">Doğrulama, uygulamanın tamamında tutarlı bir şekilde uygulanır (doğrulama mantığı tek bir yerde tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="6b46b-172">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="6b46b-173">Tek bir yerde doğrulama, kodun temiz kalmasına yardımcı olur ve bakım ve güncelleştirme işlemlerini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-173">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="6b46b-174">DataType özniteliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6b46b-174">Using DataType Attributes</span></span>

<span data-ttu-id="6b46b-175">@No__t-0 sınıfını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-175">Examine the `Movie` class.</span></span> <span data-ttu-id="6b46b-176">@No__t-0 ad alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-176">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="6b46b-177">@No__t-0 özniteliği `ReleaseDate` ve `Price` özelliklerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-177">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="6b46b-178">@No__t-0 öznitelikleri yalnızca görünüm altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL için `<a>` ve e-posta için `<a href="mailto:EmailAddress.com">` gibi öznitelikleri sağlar).</span><span class="sxs-lookup"><span data-stu-id="6b46b-178">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="6b46b-179">Verilerin biçimini doğrulamak için `RegularExpression` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b46b-179">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="6b46b-180">@No__t-0 özniteliği, veritabanı iç türünden daha belirgin bir veri türü belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-180">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="6b46b-181">`DataType` öznitelikleri doğrulama öznitelikleri değildir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-181">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="6b46b-182">Örnek uygulamada, yalnızca tarih ve saat olmadan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-182">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="6b46b-183">@No__t-0 numaralandırması, tarih, saat, PhoneNumber, para birimi, Emaadresi ve daha fazlası gibi birçok veri türü sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-183">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="6b46b-184">@No__t-0 özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-184">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="6b46b-185">Örneğin, `DataType.EmailAddress` için `mailto:` bağlantısı oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-185">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="6b46b-186">HTML5 'i destekleyen tarayıcılarda `DataType.Date` için bir tarih seçici sağlanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-186">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="6b46b-187">@No__t-0 öznitelikleri HTML 5 tarayıcıların kullandığı HTML 5 `data-` (veri Dash) özniteliklerini yayar.</span><span class="sxs-lookup"><span data-stu-id="6b46b-187">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="6b46b-188">@No__t-0 öznitelikleri herhangi bir **doğrulama sağlamaz.**</span><span class="sxs-lookup"><span data-stu-id="6b46b-188">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="6b46b-189">`DataType.Date`, görüntülenen tarihin biçimini belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="6b46b-189">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="6b46b-190">Varsayılan olarak, veri alanı, sunucunun `CultureInfo` ' a göre varsayılan biçimlere göre görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-190">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="6b46b-191">@No__t-0 veri ek açıklaması gereklidir, bu nedenle Entity Framework Core veritabanındaki para birimine `Price` ' i doğru şekilde eşleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-191">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="6b46b-192">Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="6b46b-192">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="6b46b-193">@No__t-0 özniteliği, açıkça tarih biçimini belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6b46b-193">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="6b46b-194">@No__t-0 ayarı, değer düzenlenmek üzere görüntülendiğinde biçimlendirmenin uygulanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-194">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="6b46b-195">Bazı alanlar için bu davranışı istemiyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-195">You might not want that behavior for some fields.</span></span> <span data-ttu-id="6b46b-196">Örneğin, para birimi değerlerinde, büyük olasılıkla düzenleme kullanıcı arabirimindeki para birimi sembolünü istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-196">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="6b46b-197">@No__t-0 özniteliği kendisi tarafından kullanılabilir, ancak genellikle `DataType` özniteliğini kullanmak iyi bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-197">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="6b46b-198">@No__t-0 özniteliği, verilerin semantiğini bir ekranda nasıl işleneceğini değil ve DisplayFormat ile elde olmadığınız avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="6b46b-198">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="6b46b-199">Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)</span><span class="sxs-lookup"><span data-stu-id="6b46b-199">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="6b46b-200">Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-200">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="6b46b-201">@No__t-0 özniteliği, ASP.NET Core çerçevesinin verileri işlemek için doğru alan şablonunu seçmesini sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-201">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="6b46b-202">Kendisi tarafından kullanılıyorsa `DisplayFormat`, dize şablonunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-202">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="6b46b-203">Note: jQuery doğrulaması `Range` özniteliğiyle ve `DateTime` ile çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-203">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="6b46b-204">Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6b46b-204">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="6b46b-205">Modellerinizde sabit tarihleri derlemek genellikle iyi bir uygulamadır, bu nedenle `Range` özniteliği ve `DateTime` kullanılması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="6b46b-205">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="6b46b-206">Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="6b46b-206">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="6b46b-207">[Razor Pages kullanmaya başlayın ve EF Core](xref:data/ef-rp/intro) gelişmiş EF Core işlemlerini Razor Pages gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-207">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="6b46b-208">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="6b46b-208">Apply migrations</span></span>

<span data-ttu-id="6b46b-209">Sınıfa uygulanan Dataek açıklamaları şemayı değiştirir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-209">The DataAnnotations applied to the class change the schema.</span></span> <span data-ttu-id="6b46b-210">Örneğin, `Title` alanına uygulanan veri ek açıklamaları:</span><span class="sxs-lookup"><span data-stu-id="6b46b-210">For example, the DataAnnotations applied to the `Title` field:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* <span data-ttu-id="6b46b-211">Karakterleri 60 olarak sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-211">Limits the characters to 60.</span></span>
* <span data-ttu-id="6b46b-212">@No__t-0 değerine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="6b46b-212">Doesn't allow a `null` value.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6b46b-213">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b46b-213">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6b46b-214">@No__t-0 tablosu şu anda aşağıdaki şemaya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6b46b-214">The `Movie` table currently has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

<span data-ttu-id="6b46b-215">Önceki şema değişiklikleri, EF 'in özel durum oluşturmasına neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="6b46b-215">The preceding schema changes don't cause EF to throw an exception.</span></span> <span data-ttu-id="6b46b-216">Ancak, şemanın modelle tutarlı olması için bir geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b46b-216">However, create a migration so the schema is consistent with the model.</span></span>

<span data-ttu-id="6b46b-217">**Araçlar** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi konsolu**' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="6b46b-217">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="6b46b-218">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6b46b-218">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

<span data-ttu-id="6b46b-219">`Update-Database` `New_DataAnnotations` sınıfının `Up` yöntemlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6b46b-219">`Update-Database` runs the `Up` methods of the `New_DataAnnotations` class.</span></span> <span data-ttu-id="6b46b-220">@No__t-0 yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6b46b-220">Examine the `Up` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

<span data-ttu-id="6b46b-221">Güncelleştirilmiş `Movie` tablosu aşağıdaki şemaya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6b46b-221">The updated `Movie` table has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6b46b-222">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b46b-222">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6b46b-223">SQLite için geçişler gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b46b-223">Migrations are not required for SQLite.</span></span>

---

### <a name="publish-to-azure"></a><span data-ttu-id="6b46b-224">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="6b46b-224">Publish to Azure</span></span>

<span data-ttu-id="6b46b-225">Azure 'a dağıtma hakkında bilgi için bkz. [öğretici: Azure 'DA SQL veritabanı ile ASP.NET Core uygulama oluşturma](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span><span class="sxs-lookup"><span data-stu-id="6b46b-225">For information on deploying to Azure, see [Tutorial: Build an ASP.NET Core app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span></span>

<span data-ttu-id="6b46b-226">Razor Pages için bu giriş tamamlanırken teşekkürler.</span><span class="sxs-lookup"><span data-stu-id="6b46b-226">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="6b46b-227">[Razor Pages kullanmaya başlayın ve](xref:data/ef-rp/intro) Bu öğreticiye en uygun harika bir izleme EF Core.</span><span class="sxs-lookup"><span data-stu-id="6b46b-227">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b46b-228">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6b46b-228">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="6b46b-229">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="6b46b-229">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="6b46b-230">Önceki: yeni bir alan ekleme</span><span class="sxs-lookup"><span data-stu-id="6b46b-230">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
