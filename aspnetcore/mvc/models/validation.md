---
title: ASP.NET Core MVC 'de model doğrulaması
author: rick-anderson
description: ASP.NET Core MVC ve Razor Pages model doğrulaması hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: mvc/models/validation
ms.openlocfilehash: b697f02183c76b9a96471a748a86c144fde47bb0
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76268750"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="adf10-103">ASP.NET Core MVC ve Razor Pages model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="adf10-104">[Kirk Larkaya](https://github.com/serpent5) göre</span><span class="sxs-lookup"><span data-stu-id="adf10-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="adf10-105">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="adf10-106">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="adf10-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="adf10-107">Model durumu</span><span class="sxs-lookup"><span data-stu-id="adf10-107">Model state</span></span>

<span data-ttu-id="adf10-108">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="adf10-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="adf10-109">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="adf10-110">Örneğin, bir tamsayı alanına bir "x" girilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="adf10-111">Model bağlama sonrasında model doğrulaması oluşur ve verilerin iş kurallarına uygun olmadığı rapor hataları raporlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="adf10-112">Örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana 0 girilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="adf10-113">Hem model bağlama hem de model doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="adf10-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="adf10-114">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="adf10-115">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="adf10-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="adf10-116">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="adf10-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="adf10-117">Bu durumda, model durumu geçersiz olduğunda hata ayrıntılarını içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="adf10-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="adf10-118">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="adf10-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="adf10-119">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="adf10-119">Rerun validation</span></span>

<span data-ttu-id="adf10-120">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="adf10-121">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="adf10-122">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="adf10-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="adf10-123">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-123">Validation attributes</span></span>

<span data-ttu-id="adf10-124">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="adf10-125">Örnek uygulamadaki aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="adf10-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="adf10-126">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="adf10-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="adf10-127">Gösterilmemiştir `[ClassicMovieWithClientValidator]`.</span><span class="sxs-lookup"><span data-stu-id="adf10-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="adf10-128">`[ClassicMovieWithClientValidator]` özel bir öznitelik uygulamak için alternatif bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="adf10-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="adf10-129">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="adf10-129">Built-in attributes</span></span>

<span data-ttu-id="adf10-130">Yerleşik doğrulama özniteliklerinden bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="adf10-131">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="adf10-132">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="adf10-133">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="adf10-134">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="adf10-135">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="adf10-136">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="adf10-137">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="adf10-138">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Required]` özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="adf10-138">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="adf10-139">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="adf10-140">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="adf10-141">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="adf10-142">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Remote]` özniteliği](#remote-attribute) .</span><span class="sxs-lookup"><span data-stu-id="adf10-142">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="adf10-143">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="adf10-144">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="adf10-144">Error messages</span></span>

<span data-ttu-id="adf10-145">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="adf10-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="adf10-146">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="adf10-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="adf10-147">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="adf10-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="adf10-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="adf10-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="adf10-149">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="adf10-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="adf10-150">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="adf10-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="adf10-151">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="adf10-151">[Required] attribute</span></span>

<span data-ttu-id="adf10-152">.NET Core 3,0 ve sonraki sürümlerde doğrulama sistemi, null olamayan parametrelere veya bir `[Required]` özniteliğine sahip olduklarından, bağlantılı özelliklere davranır.</span><span class="sxs-lookup"><span data-stu-id="adf10-152">The validation system in .NET Core 3.0 and later treats non-nullable parameters or bound properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="adf10-153">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="adf10-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span> <span data-ttu-id="adf10-154">Bu davranış, `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes> yapılandırılarak devre dışı bırakılabilir:</span><span class="sxs-lookup"><span data-stu-id="adf10-154">This behavior can be disabled by configuring <xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes> in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="adf10-155">' ' CSharp Hizmetleri. AddControllers (Options = > seçenekleri. SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true); ...</span><span class="sxs-lookup"><span data-stu-id="adf10-155">\`\`csharp services.AddControllers(options => options.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true); ...</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="adf10-156">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="adf10-156">[Required] validation on the server</span></span>

<span data-ttu-id="adf10-157">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-157">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="adf10-158">Null yapılamayan bir alan her zaman geçerlidir ve `[Required]` özniteliğin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="adf10-158">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="adf10-159">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-159">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="adf10-160">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-160">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="adf10-161">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="adf10-161">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="adf10-162">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-162">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="adf10-163">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="adf10-163">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="adf10-164">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="adf10-164">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="adf10-165">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="adf10-165">[Required] validation on the client</span></span>

<span data-ttu-id="adf10-166">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="adf10-166">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="adf10-167">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="adf10-167">On the client:</span></span>

* <span data-ttu-id="adf10-168">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-168">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="adf10-169">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="adf10-169">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="adf10-170">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-170">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="adf10-171">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-171">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="adf10-172">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-172">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="adf10-173">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-173">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="adf10-174">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="adf10-174">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="adf10-175">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="adf10-175">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="adf10-176">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="adf10-176">[Remote] attribute</span></span>

<span data-ttu-id="adf10-177">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="adf10-177">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="adf10-178">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-178">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="adf10-179">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="adf10-179">To implement remote validation:</span></span>

1. <span data-ttu-id="adf10-180">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-180">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="adf10-181">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="adf10-181">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="adf10-182">`true`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-182">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="adf10-183">`false`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-183">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="adf10-184">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="adf10-184">Display the default error message.</span></span>
   * <span data-ttu-id="adf10-185">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-185">Any other string means the input is invalid.</span></span> <span data-ttu-id="adf10-186">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-186">Display the string as a custom error message.</span></span>

   <span data-ttu-id="adf10-187">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="adf10-187">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="adf10-188">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="adf10-188">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="adf10-189">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-189">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="adf10-190">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="adf10-190">Additional fields</span></span>

<span data-ttu-id="adf10-191">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-191">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="adf10-192">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-192">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="adf10-193">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="adf10-193">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="adf10-194">`AdditionalFields` "FirstName" ve "LastName" dizeleriyle açıkça ayarlanabilir, ancak [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-194">`AdditionalFields` could be set explicitly to the strings "FirstName" and "LastName", but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="adf10-195">Bu doğrulama için eylem yöntemi hem `firstName` hem de `lastName` bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="adf10-195">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="adf10-196">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="adf10-196">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="adf10-197">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="adf10-197">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="adf10-198">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="adf10-198">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="adf10-199">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-199">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="adf10-200">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-200">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="adf10-201">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="adf10-201">Alternatives to built-in attributes</span></span>

<span data-ttu-id="adf10-202">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-202">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="adf10-203">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="adf10-203">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="adf10-204">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="adf10-204">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="adf10-205">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="adf10-205">Custom attributes</span></span>

<span data-ttu-id="adf10-206">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-206">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="adf10-207"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="adf10-207">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="adf10-208">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-208">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="adf10-209">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-209">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="adf10-210">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-210">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="adf10-211">`[ClassicMovie]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="adf10-211">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="adf10-212">Yalnızca sunucuda çalışır.</span><span class="sxs-lookup"><span data-stu-id="adf10-212">Is only run on the server.</span></span>
* <span data-ttu-id="adf10-213">Klasik Filmler için, yayımlanma tarihini doğrular:</span><span class="sxs-lookup"><span data-stu-id="adf10-213">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="adf10-214">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-214">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="adf10-215">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="adf10-215">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="adf10-216">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="adf10-216">IValidatableObject</span></span>

<span data-ttu-id="adf10-217">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="adf10-217">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="adf10-218">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="adf10-218">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="adf10-219">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-219">Top-level node validation</span></span>

<span data-ttu-id="adf10-220">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="adf10-220">Top-level nodes include:</span></span>

* <span data-ttu-id="adf10-221">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="adf10-221">Action parameters</span></span>
* <span data-ttu-id="adf10-222">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-222">Controller properties</span></span>
* <span data-ttu-id="adf10-223">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="adf10-223">Page handler parameters</span></span>
* <span data-ttu-id="adf10-224">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-224">Page model properties</span></span>

<span data-ttu-id="adf10-225">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-225">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="adf10-226">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="adf10-226">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="adf10-227">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-227">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="adf10-228">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="adf10-228">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="adf10-229">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-229">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="adf10-230">İlk form bir `99` `Age` değerini bir sorgu dizesi parametresi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="adf10-230">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="adf10-231">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-231">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="adf10-232">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="adf10-232">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="adf10-233">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="adf10-233">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="adf10-234">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="adf10-234">Maximum errors</span></span>

<span data-ttu-id="adf10-235">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="adf10-235">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="adf10-236">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-236">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="adf10-237">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="adf10-237">Maximum recursion</span></span>

<span data-ttu-id="adf10-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="adf10-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="adf10-239">Derin olan veya sonsuz özyinelemeli olan modeller için doğrulama, yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-239">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="adf10-240">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="adf10-241">`MvcOptions.MaxValidationDepth` varsayılan değeri 32 ' dir.</span><span class="sxs-lookup"><span data-stu-id="adf10-241">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="adf10-242">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="adf10-242">Automatic short-circuit</span></span>

<span data-ttu-id="adf10-243">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="adf10-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="adf10-244">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="adf10-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="adf10-245">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="adf10-245">Disable validation</span></span>

<span data-ttu-id="adf10-246">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="adf10-246">To disable validation:</span></span>

1. <span data-ttu-id="adf10-247">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="adf10-248">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="adf10-249">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="adf10-250">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-250">Client-side validation</span></span>

<span data-ttu-id="adf10-251">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="adf10-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="adf10-252">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="adf10-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="adf10-253">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="adf10-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="adf10-254">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="adf10-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="adf10-255">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="adf10-256">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="adf10-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="adf10-257">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="adf10-258">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="adf10-259">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="adf10-260">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="adf10-260">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="adf10-261">HTML çıkışındaki `data-` özniteliklerinin `Movie.ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="adf10-262">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="adf10-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="adf10-263">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [Required ()](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve sonra bu iletiyi eşlik eden **\<span >** öğesinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="adf10-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="adf10-264">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="adf10-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="adf10-265">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="adf10-266">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="adf10-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

## <a name="unobtrusive-validation"></a><span data-ttu-id="adf10-267">Unobtrusive doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-267">Unobtrusive validation</span></span>

<span data-ttu-id="adf10-268">Obtrusive doğrulaması hakkında bilgi için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/1111)bakın.</span><span class="sxs-lookup"><span data-stu-id="adf10-268">For information on unobtrusive validation, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="adf10-269">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="adf10-269">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="adf10-270">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-270">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="adf10-271">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="adf10-271">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="adf10-272">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-272">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="adf10-273">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-273">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="adf10-274">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-274">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="adf10-275">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="adf10-275">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="adf10-276">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-276">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="adf10-277">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="adf10-277">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="adf10-278">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="adf10-278">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="adf10-279">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="adf10-279">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="adf10-280">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-280">Custom client-side validation</span></span>

<span data-ttu-id="adf10-281">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-281">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="adf10-282">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `[ClassicMovie]` ve `[ClassicMovieWithClientValidator]` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="adf10-282">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="adf10-283">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="adf10-283">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="adf10-284">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="adf10-284">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="adf10-285">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="adf10-285">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="adf10-286">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="adf10-286">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="adf10-287">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="adf10-287">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="adf10-288">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="adf10-288">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="adf10-289">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-289">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="adf10-290">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-290">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="adf10-291">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="adf10-291">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="adf10-292">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="adf10-292">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="adf10-293">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="adf10-293">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="adf10-294">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-294">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="adf10-295">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-295">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="adf10-296">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="adf10-296">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="adf10-297">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-297">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="adf10-298">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="adf10-298">To add client validation by using this method:</span></span>

1. <span data-ttu-id="adf10-299">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-299">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="adf10-300">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="adf10-300">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="adf10-301">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="adf10-301">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="adf10-302"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-302">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="adf10-303">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="adf10-303">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="adf10-304">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="adf10-304">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="adf10-305">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="adf10-305">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="adf10-306">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovieWithClientValidator` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-306">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="adf10-307">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="adf10-307">To add client validation by using this method:</span></span>

* <span data-ttu-id="adf10-308">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-308">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="adf10-309">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="adf10-309">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="adf10-310">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="adf10-310">Disable client-side validation</span></span>

<span data-ttu-id="adf10-311">Aşağıdaki kod Razor Pages istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="adf10-311">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="adf10-312">İstemci tarafı doğrulamayı devre dışı bırakmak için diğer seçenekler:</span><span class="sxs-lookup"><span data-stu-id="adf10-312">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="adf10-313">Tüm *. cshtml* dosyalarındaki `_ValidationScriptsPartial` başvurusunu açıklama olarak yapın.</span><span class="sxs-lookup"><span data-stu-id="adf10-313">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="adf10-314">*Pages\shared\_ValidationScriptsPartial. cshtml* dosyasının içeriğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="adf10-314">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="adf10-315">Yukarıdaki yaklaşım ASP.NET Core Identity Razor sınıfı kitaplığının istemci tarafında doğrulanmasını engellemez.</span><span class="sxs-lookup"><span data-stu-id="adf10-315">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="adf10-316">Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="adf10-316">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adf10-317">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="adf10-317">Additional resources</span></span>

* [<span data-ttu-id="adf10-318">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="adf10-318">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="adf10-319">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="adf10-319">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="adf10-320">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-320">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="adf10-321">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="adf10-321">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="adf10-322">Model durumu</span><span class="sxs-lookup"><span data-stu-id="adf10-322">Model state</span></span>

<span data-ttu-id="adf10-323">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="adf10-323">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="adf10-324">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır (örneğin, bir tamsayı bekleyen bir alana bir "x" girilir).</span><span class="sxs-lookup"><span data-stu-id="adf10-324">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="adf10-325">Model bağlama ve verilerin iş kurallarına uygun olmadığı rapor hataları (örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana bir 0 girildiğinde) oluşturulduktan sonra model doğrulaması oluşur.</span><span class="sxs-lookup"><span data-stu-id="adf10-325">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="adf10-326">Hem model bağlama hem de doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="adf10-326">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="adf10-327">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-327">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="adf10-328">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="adf10-328">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="adf10-329">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="adf10-329">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="adf10-330">Bu durumda, model durumu geçersiz olduğunda hata ayrıntılarını içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="adf10-330">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="adf10-331">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="adf10-331">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="adf10-332">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="adf10-332">Rerun validation</span></span>

<span data-ttu-id="adf10-333">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-333">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="adf10-334">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-334">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="adf10-335">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="adf10-335">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="adf10-336">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-336">Validation attributes</span></span>

<span data-ttu-id="adf10-337">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-337">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="adf10-338">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="adf10-338">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="adf10-339">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="adf10-339">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="adf10-340">Gösterilmemiştir `[ClassicMovie2]`, özel bir özniteliği uygulamak için alternatif bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="adf10-340">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="adf10-341">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="adf10-341">Built-in attributes</span></span>

<span data-ttu-id="adf10-342">Yerleşik doğrulama öznitelikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-342">Built-in validation attributes include:</span></span>

* <span data-ttu-id="adf10-343">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-343">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="adf10-344">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-344">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="adf10-345">Örneğin, *register.cshtml.cs* dosyası, girilen iki parola eşleşmesini doğrulamak için `[Compare]` kullanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-345">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="adf10-346">Kayıt kodunu görmek için [Scaffold kimliği](xref:security/authentication/scaffold-identity) .</span><span class="sxs-lookup"><span data-stu-id="adf10-346">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="adf10-347">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-347">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="adf10-348">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-348">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="adf10-349">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-349">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="adf10-350">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-350">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="adf10-351">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-351">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="adf10-352">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Required]` özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="adf10-352">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="adf10-353">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-353">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="adf10-354">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-354">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="adf10-355">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-355">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="adf10-356">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Remote]` özniteliği](#remote-attribute) .</span><span class="sxs-lookup"><span data-stu-id="adf10-356">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="adf10-357">İstemci tarafı doğrulama ile `[RegularExpression]` özniteliğini kullanırken, Regex istemcide JavaScript 'te yürütülür.</span><span class="sxs-lookup"><span data-stu-id="adf10-357">When using the `[RegularExpression]` attribute with client-side validation, the regex is executed in JavaScript on the client.</span></span> <span data-ttu-id="adf10-358">Bu, [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) eşleştirme davranışının kullanılacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-358">This means [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) matching behavior will be used.</span></span> <span data-ttu-id="adf10-359">Daha fazla bilgi için [bu GitHub sorunu](https://github.com/dotnet/corefx/issues/42487).</span><span class="sxs-lookup"><span data-stu-id="adf10-359">For more information, see [this GitHub issue](https://github.com/dotnet/corefx/issues/42487).</span></span>

<span data-ttu-id="adf10-360">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-360">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="adf10-361">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="adf10-361">Error messages</span></span>

<span data-ttu-id="adf10-362">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="adf10-362">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="adf10-363">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="adf10-363">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="adf10-364">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="adf10-364">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="adf10-365">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="adf10-365">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="adf10-366">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="adf10-366">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="adf10-367">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="adf10-367">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="adf10-368">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="adf10-368">[Required] attribute</span></span>

<span data-ttu-id="adf10-369">Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="adf10-369">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="adf10-370">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="adf10-370">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="adf10-371">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="adf10-371">[Required] validation on the server</span></span>

<span data-ttu-id="adf10-372">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-372">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="adf10-373">Null yapılamayan bir alan her zaman geçerlidir ve [gerekli] özniteliğinin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="adf10-373">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="adf10-374">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-374">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="adf10-375">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-375">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="adf10-376">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="adf10-376">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="adf10-377">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-377">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="adf10-378">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="adf10-378">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="adf10-379">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="adf10-379">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="adf10-380">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="adf10-380">[Required] validation on the client</span></span>

<span data-ttu-id="adf10-381">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="adf10-381">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="adf10-382">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="adf10-382">On the client:</span></span>

* <span data-ttu-id="adf10-383">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-383">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="adf10-384">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="adf10-384">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="adf10-385">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-385">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="adf10-386">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-386">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="adf10-387">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-387">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="adf10-388">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-388">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="adf10-389">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="adf10-389">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="adf10-390">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="adf10-390">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="adf10-391">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="adf10-391">[Remote] attribute</span></span>

<span data-ttu-id="adf10-392">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="adf10-392">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="adf10-393">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-393">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="adf10-394">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="adf10-394">To implement remote validation:</span></span>

1. <span data-ttu-id="adf10-395">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-395">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="adf10-396">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="adf10-396">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="adf10-397">`"true"`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-397">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="adf10-398">`"false"`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-398">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="adf10-399">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="adf10-399">Display the default error message.</span></span>
   * <span data-ttu-id="adf10-400">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-400">Any other string means the input is invalid.</span></span> <span data-ttu-id="adf10-401">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-401">Display the string as a custom error message.</span></span>

   <span data-ttu-id="adf10-402">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="adf10-402">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="adf10-403">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="adf10-403">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="adf10-404">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-404">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="adf10-405">`Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` metapackage kullanmıyorsanız [Microsoft. AspNetCore. Mvc. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-405">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="adf10-406">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="adf10-406">Additional fields</span></span>

<span data-ttu-id="adf10-407">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-407">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="adf10-408">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-408">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="adf10-409">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="adf10-409">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="adf10-410">`AdditionalFields`, `"FirstName"` ve `"LastName"`dizeler için açıkça ayarlanabilir, ancak [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-410">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="adf10-411">Bu doğrulama için eylem yöntemi hem adı hem de soyadı bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="adf10-411">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="adf10-412">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="adf10-412">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="adf10-413">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="adf10-413">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="adf10-414">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="adf10-414">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="adf10-415">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-415">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="adf10-416">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-416">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="adf10-417">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="adf10-417">Alternatives to built-in attributes</span></span>

<span data-ttu-id="adf10-418">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-418">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="adf10-419">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="adf10-419">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="adf10-420">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="adf10-420">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="adf10-421">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="adf10-421">Custom attributes</span></span>

<span data-ttu-id="adf10-422">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-422">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="adf10-423"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="adf10-423">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="adf10-424">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-424">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="adf10-425">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-425">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="adf10-426">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-426">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="adf10-427">`[ClassicMovie2]` özniteliği önce tarzı denetler ve yalnızca *Klasik*ise devam eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-427">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="adf10-428">Classics olarak tanımlanan filmler için, öznitelik oluşturucusuna geçirilen sınırdan daha sonra olmadığından emin olmak için yayın tarihini denetler.)</span><span class="sxs-lookup"><span data-stu-id="adf10-428">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="adf10-429">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-429">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="adf10-430">`IsValid` yöntemi tarihi ve tarzı denetler.</span><span class="sxs-lookup"><span data-stu-id="adf10-430">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="adf10-431">Doğrulama başarıyla tamamlandığında, `IsValid` bir `ValidationResult.Success` kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="adf10-431">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="adf10-432">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="adf10-432">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="adf10-433">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="adf10-433">IValidatableObject</span></span>

<span data-ttu-id="adf10-434">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="adf10-434">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="adf10-435">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="adf10-435">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="adf10-436">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-436">Top-level node validation</span></span>

<span data-ttu-id="adf10-437">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="adf10-437">Top-level nodes include:</span></span>

* <span data-ttu-id="adf10-438">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="adf10-438">Action parameters</span></span>
* <span data-ttu-id="adf10-439">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-439">Controller properties</span></span>
* <span data-ttu-id="adf10-440">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="adf10-440">Page handler parameters</span></span>
* <span data-ttu-id="adf10-441">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="adf10-441">Page model properties</span></span>

<span data-ttu-id="adf10-442">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-442">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="adf10-443">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="adf10-443">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="adf10-444">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-444">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="adf10-445">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="adf10-445">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="adf10-446">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="adf10-446">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="adf10-447">İlk form bir `99` `Age` değerini bir sorgu dizesi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="adf10-447">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="adf10-448">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="adf10-448">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="adf10-449">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="adf10-449">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="adf10-450">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="adf10-450">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="adf10-451">`CompatibilityVersion.Version_2_1` veya sonraki sürümleriyle çalışırken, en üst düzey düğüm doğrulama varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="adf10-451">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="adf10-452">Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-452">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="adf10-453">Varsayılan seçenek, burada gösterildiği gibi, (`Startup.ConfigureServices`) içinde <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliği ayarlanarak geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="adf10-453">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="adf10-454">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="adf10-454">Maximum errors</span></span>

<span data-ttu-id="adf10-455">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="adf10-455">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="adf10-456">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-456">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="adf10-457">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="adf10-457">Maximum recursion</span></span>

<span data-ttu-id="adf10-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="adf10-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="adf10-459">Çok derin olan veya sonsuz özyinelemeli özyinelemeli modeller için, doğrulama yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-459">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="adf10-460">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-460">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="adf10-461">`MvcOptions.MaxValidationDepth` varsayılan değeri, `CompatibilityVersion.Version_2_2` veya sonraki sürümleriyle çalışırken 32 ' dir.</span><span class="sxs-lookup"><span data-stu-id="adf10-461">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="adf10-462">Önceki sürümler için değer null, bu da derinlemesine bir kısıtlama kısıtlaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="adf10-462">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="adf10-463">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="adf10-463">Automatic short-circuit</span></span>

<span data-ttu-id="adf10-464">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="adf10-464">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="adf10-465">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="adf10-465">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="adf10-466">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="adf10-466">Disable validation</span></span>

<span data-ttu-id="adf10-467">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="adf10-467">To disable validation:</span></span>

1. <span data-ttu-id="adf10-468">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-468">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="adf10-469">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-469">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="adf10-470">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="adf10-470">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="adf10-471">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-471">Client-side validation</span></span>

<span data-ttu-id="adf10-472">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="adf10-472">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="adf10-473">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="adf10-473">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="adf10-474">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="adf10-474">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="adf10-475">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="adf10-475">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="adf10-476">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="adf10-476">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="adf10-477">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="adf10-477">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="adf10-478">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-478">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="adf10-479">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-479">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="adf10-480">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="adf10-480">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="adf10-481">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler.</span><span class="sxs-lookup"><span data-stu-id="adf10-481">The preceding tag helpers render the following HTML.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="adf10-482">HTML çıkışındaki `data-` özniteliklerinin `ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-482">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="adf10-483">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="adf10-483">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="adf10-484">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [Required ()](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve sonra bu iletiyi eşlik eden **\<span >** öğesinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="adf10-484">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="adf10-485">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="adf10-485">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="adf10-486">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-486">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="adf10-487">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="adf10-487">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="adf10-488">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="adf10-488">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="adf10-489">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="adf10-489">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="adf10-490">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="adf10-490">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="adf10-491">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="adf10-491">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="adf10-492">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="adf10-492">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="adf10-493">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="adf10-493">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="adf10-494">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="adf10-494">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="adf10-495">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-495">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="adf10-496">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="adf10-496">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="adf10-497">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="adf10-497">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="adf10-498">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="adf10-498">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="adf10-499">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="adf10-499">Custom client-side validation</span></span>

<span data-ttu-id="adf10-500">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-500">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="adf10-501">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `ClassicMovie` ve `ClassicMovie2` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="adf10-501">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="adf10-502">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="adf10-502">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="adf10-503">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="adf10-503">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="adf10-504">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="adf10-504">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="adf10-505">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="adf10-505">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="adf10-506">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="adf10-506">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="adf10-507">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="adf10-507">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="adf10-508">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="adf10-508">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="adf10-509">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="adf10-509">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="adf10-510">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="adf10-510">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="adf10-511">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="adf10-511">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="adf10-512">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="adf10-512">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="adf10-513">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="adf10-513">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="adf10-514">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="adf10-514">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="adf10-515">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="adf10-515">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="adf10-516">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-516">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="adf10-517">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="adf10-517">To add client validation by using this method:</span></span>

1. <span data-ttu-id="adf10-518">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-518">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="adf10-519">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="adf10-519">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="adf10-520">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="adf10-520">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="adf10-521"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-521">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="adf10-522">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="adf10-522">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="adf10-523">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="adf10-523">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="adf10-524">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="adf10-524">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="adf10-525">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie2` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="adf10-525">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="adf10-526">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="adf10-526">To add client validation by using this method:</span></span>

* <span data-ttu-id="adf10-527">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="adf10-527">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="adf10-528">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="adf10-528">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="adf10-529">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="adf10-529">Disable client-side validation</span></span>

<span data-ttu-id="adf10-530">Aşağıdaki kod, MVC görünümlerinde istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="adf10-530">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="adf10-531">Ve Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="adf10-531">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="adf10-532">İstemci doğrulamasını devre dışı bırakmaya yönelik başka bir seçenek de, *. cshtml* dosyanızdaki `_ValidationScriptsPartial` başvurusunu açıklamaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="adf10-532">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adf10-533">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="adf10-533">Additional resources</span></span>

* [<span data-ttu-id="adf10-534">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="adf10-534">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="adf10-535">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="adf10-535">Model Binding</span></span>](model-binding.md)

::: moniker-end
