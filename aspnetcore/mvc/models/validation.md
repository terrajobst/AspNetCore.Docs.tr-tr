---
title: ASP.NET Core MVC 'de model doğrulaması
author: rick-anderson
description: ASP.NET Core MVC ve Razor Pages model doğrulaması hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/models/validation
ms.openlocfilehash: 7a6017141eb1016128c4a135c187479717580bb5
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881037"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="79f63-103">ASP.NET Core MVC ve Razor Pages model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="79f63-104">[Kirk Larkaya](https://github.com/serpent5) göre</span><span class="sxs-lookup"><span data-stu-id="79f63-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="79f63-105">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="79f63-106">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="79f63-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="79f63-107">Model durumu</span><span class="sxs-lookup"><span data-stu-id="79f63-107">Model state</span></span>

<span data-ttu-id="79f63-108">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="79f63-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="79f63-109">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="79f63-110">Örneğin, bir tamsayı alanına bir "x" girilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="79f63-111">Model bağlama sonrasında model doğrulaması oluşur ve verilerin iş kurallarına uygun olmadığı rapor hataları raporlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="79f63-112">Örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana 0 girilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="79f63-113">Hem model bağlama hem de model doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="79f63-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="79f63-114">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="79f63-115">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="79f63-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="79f63-116">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="79f63-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="79f63-117">Bu durumda, model durumu geçersiz olduğunda hata ayrıntılarını içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="79f63-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="79f63-118">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="79f63-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="79f63-119">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="79f63-119">Rerun validation</span></span>

<span data-ttu-id="79f63-120">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="79f63-121">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="79f63-122">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="79f63-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="79f63-123">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-123">Validation attributes</span></span>

<span data-ttu-id="79f63-124">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="79f63-125">Örnek uygulamadaki aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="79f63-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="79f63-126">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="79f63-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="79f63-127">Gösterilmemiştir `[ClassicMovieWithClientValidator]`.</span><span class="sxs-lookup"><span data-stu-id="79f63-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="79f63-128">`[ClassicMovieWithClientValidator]` özel bir öznitelik uygulamak için alternatif bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="79f63-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="79f63-129">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="79f63-129">Built-in attributes</span></span>

<span data-ttu-id="79f63-130">Yerleşik doğrulama özniteliklerinden bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="79f63-131">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="79f63-132">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="79f63-133">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="79f63-134">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="79f63-135">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="79f63-136">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="79f63-137">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="79f63-138">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Required]` özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="79f63-138">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="79f63-139">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="79f63-140">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="79f63-141">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="79f63-142">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. `[`[Remote] ' Attribute] (#remote-özniteliği).</span><span class="sxs-lookup"><span data-stu-id="79f63-142">See `[`[Remote]\` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="79f63-143">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="79f63-144">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="79f63-144">Error messages</span></span>

<span data-ttu-id="79f63-145">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="79f63-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="79f63-146">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="79f63-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="79f63-147">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="79f63-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="79f63-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="79f63-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="79f63-149">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="79f63-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="79f63-150">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="79f63-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="79f63-151">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="79f63-151">[Required] attribute</span></span>

<span data-ttu-id="79f63-152">Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="79f63-152">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="79f63-153">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="79f63-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="79f63-154">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="79f63-154">[Required] validation on the server</span></span>

<span data-ttu-id="79f63-155">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-155">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="79f63-156">Null yapılamayan bir alan her zaman geçerlidir ve `[Required]` özniteliğin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="79f63-156">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="79f63-157">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-157">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="79f63-158">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-158">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="79f63-159">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="79f63-159">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="79f63-160">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-160">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="79f63-161">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="79f63-161">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="79f63-162">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="79f63-162">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="79f63-163">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="79f63-163">[Required] validation on the client</span></span>

<span data-ttu-id="79f63-164">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="79f63-164">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="79f63-165">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="79f63-165">On the client:</span></span>

* <span data-ttu-id="79f63-166">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-166">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="79f63-167">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="79f63-167">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="79f63-168">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-168">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="79f63-169">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-169">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="79f63-170">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-170">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="79f63-171">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-171">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="79f63-172">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="79f63-172">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="79f63-173">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="79f63-173">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="79f63-174">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="79f63-174">[Remote] attribute</span></span>

<span data-ttu-id="79f63-175">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="79f63-175">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="79f63-176">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-176">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="79f63-177">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="79f63-177">To implement remote validation:</span></span>

1. <span data-ttu-id="79f63-178">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-178">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="79f63-179">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="79f63-179">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="79f63-180">`true`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-180">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="79f63-181">`false`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-181">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="79f63-182">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="79f63-182">Display the default error message.</span></span>
   * <span data-ttu-id="79f63-183">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-183">Any other string means the input is invalid.</span></span> <span data-ttu-id="79f63-184">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-184">Display the string as a custom error message.</span></span>

   <span data-ttu-id="79f63-185">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="79f63-185">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="79f63-186">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="79f63-186">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="79f63-187">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-187">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="79f63-188">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="79f63-188">Additional fields</span></span>

<span data-ttu-id="79f63-189">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-189">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="79f63-190">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-190">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="79f63-191">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="79f63-191">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="79f63-192">`AdditionalFields` "FirstName" ve "LastName" dizeleriyle açıkça ayarlanabilir, ancak [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-192">`AdditionalFields` could be set explicitly to the strings "FirstName" and "LastName", but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="79f63-193">Bu doğrulama için eylem yöntemi hem `firstName` hem de `lastName` bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="79f63-193">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="79f63-194">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="79f63-194">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="79f63-195">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="79f63-195">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="79f63-196">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="79f63-196">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="79f63-197">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-197">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="79f63-198">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-198">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="79f63-199">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="79f63-199">Alternatives to built-in attributes</span></span>

<span data-ttu-id="79f63-200">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-200">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="79f63-201">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="79f63-201">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="79f63-202">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="79f63-202">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="79f63-203">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="79f63-203">Custom attributes</span></span>

<span data-ttu-id="79f63-204">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-204">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="79f63-205"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="79f63-205">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="79f63-206">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-206">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="79f63-207">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-207">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="79f63-208">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-208">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="79f63-209">`[ClassicMovie]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="79f63-209">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="79f63-210">Yalnızca sunucuda çalışır.</span><span class="sxs-lookup"><span data-stu-id="79f63-210">Is only run on the server.</span></span>
* <span data-ttu-id="79f63-211">Klasik Filmler için, yayımlanma tarihini doğrular:</span><span class="sxs-lookup"><span data-stu-id="79f63-211">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="79f63-212">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-212">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="79f63-213">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="79f63-213">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="79f63-214">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="79f63-214">IValidatableObject</span></span>

<span data-ttu-id="79f63-215">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="79f63-215">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="79f63-216">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="79f63-216">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="79f63-217">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-217">Top-level node validation</span></span>

<span data-ttu-id="79f63-218">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="79f63-218">Top-level nodes include:</span></span>

* <span data-ttu-id="79f63-219">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="79f63-219">Action parameters</span></span>
* <span data-ttu-id="79f63-220">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-220">Controller properties</span></span>
* <span data-ttu-id="79f63-221">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="79f63-221">Page handler parameters</span></span>
* <span data-ttu-id="79f63-222">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-222">Page model properties</span></span>

<span data-ttu-id="79f63-223">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-223">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="79f63-224">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="79f63-224">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="79f63-225">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-225">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="79f63-226">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="79f63-226">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="79f63-227">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-227">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="79f63-228">İlk form bir `99` `Age` değerini bir sorgu dizesi parametresi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="79f63-228">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="79f63-229">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-229">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="79f63-230">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="79f63-230">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="79f63-231">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="79f63-231">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="79f63-232">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="79f63-232">Maximum errors</span></span>

<span data-ttu-id="79f63-233">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="79f63-233">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="79f63-234">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-234">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="79f63-235">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="79f63-235">Maximum recursion</span></span>

<span data-ttu-id="79f63-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="79f63-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="79f63-237">Derin olan veya sonsuz özyinelemeli olan modeller için doğrulama, yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-237">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="79f63-238">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="79f63-239">`MvcOptions.MaxValidationDepth` varsayılan değeri 32 ' dir.</span><span class="sxs-lookup"><span data-stu-id="79f63-239">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="79f63-240">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="79f63-240">Automatic short-circuit</span></span>

<span data-ttu-id="79f63-241">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="79f63-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="79f63-242">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="79f63-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="79f63-243">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="79f63-243">Disable validation</span></span>

<span data-ttu-id="79f63-244">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="79f63-244">To disable validation:</span></span>

1. <span data-ttu-id="79f63-245">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="79f63-246">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="79f63-247">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="79f63-248">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-248">Client-side validation</span></span>

<span data-ttu-id="79f63-249">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="79f63-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="79f63-250">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="79f63-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="79f63-251">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="79f63-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="79f63-252">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="79f63-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="79f63-253">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="79f63-254">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="79f63-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="79f63-255">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="79f63-256">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="79f63-257">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="79f63-258">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler:</span><span class="sxs-lookup"><span data-stu-id="79f63-258">The preceding tag helpers render the following HTML:</span></span>

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

<span data-ttu-id="79f63-259">HTML çıkışındaki `data-` özniteliklerinin `Movie.ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="79f63-260">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="79f63-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="79f63-261">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [Required ()](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve sonra bu iletiyi eşlik eden **\<span >** öğesinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="79f63-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="79f63-262">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="79f63-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="79f63-263">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="79f63-264">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="79f63-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

## <a name="unobtrusive-validation"></a><span data-ttu-id="79f63-265">Unobtrusive doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-265">Unobtrusive validation</span></span>

<span data-ttu-id="79f63-266">Obtrusive doğrulaması hakkında bilgi için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/1111)bakın.</span><span class="sxs-lookup"><span data-stu-id="79f63-266">For information on unobtrusive validation, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="79f63-267">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="79f63-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="79f63-268">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="79f63-269">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="79f63-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="79f63-270">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="79f63-271">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="79f63-272">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="79f63-273">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="79f63-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="79f63-274">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="79f63-275">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="79f63-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="79f63-276">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="79f63-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="79f63-277">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="79f63-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="79f63-278">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-278">Custom client-side validation</span></span>

<span data-ttu-id="79f63-279">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="79f63-280">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `[ClassicMovie]` ve `[ClassicMovieWithClientValidator]` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="79f63-280">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="79f63-281">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="79f63-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="79f63-282">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="79f63-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="79f63-283">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="79f63-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="79f63-284">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="79f63-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="79f63-285">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="79f63-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="79f63-286">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="79f63-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="79f63-287">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="79f63-288">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="79f63-289">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="79f63-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="79f63-290">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="79f63-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="79f63-291">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="79f63-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="79f63-292">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="79f63-293">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="79f63-294">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="79f63-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="79f63-295">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="79f63-296">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="79f63-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="79f63-297">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="79f63-298">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="79f63-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="79f63-299">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="79f63-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="79f63-300"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="79f63-301">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="79f63-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="79f63-302">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="79f63-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="79f63-303">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="79f63-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="79f63-304">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovieWithClientValidator` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="79f63-305">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="79f63-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="79f63-306">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="79f63-307">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="79f63-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="79f63-308">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="79f63-308">Disable client-side validation</span></span>

<span data-ttu-id="79f63-309">Aşağıdaki kod Razor Pages istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="79f63-309">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="79f63-310">İstemci tarafı doğrulamayı devre dışı bırakmak için diğer seçenekler:</span><span class="sxs-lookup"><span data-stu-id="79f63-310">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="79f63-311">Tüm *. cshtml* dosyalarındaki `_ValidationScriptsPartial` başvurusunu açıklama olarak yapın.</span><span class="sxs-lookup"><span data-stu-id="79f63-311">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="79f63-312">*Pages\shared\_ValidationScriptsPartial. cshtml* dosyasının içeriğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="79f63-312">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="79f63-313">Yukarıdaki yaklaşım ASP.NET Core Identity Razor sınıfı kitaplığının istemci tarafında doğrulanmasını engellemez.</span><span class="sxs-lookup"><span data-stu-id="79f63-313">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="79f63-314">Daha fazla bilgi için bkz. <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="79f63-314">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79f63-315">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="79f63-315">Additional resources</span></span>

* [<span data-ttu-id="79f63-316">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="79f63-316">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="79f63-317">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="79f63-317">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="79f63-318">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-318">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="79f63-319">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="79f63-319">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="79f63-320">Model durumu</span><span class="sxs-lookup"><span data-stu-id="79f63-320">Model state</span></span>

<span data-ttu-id="79f63-321">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="79f63-321">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="79f63-322">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır (örneğin, bir tamsayı bekleyen bir alana bir "x" girilir).</span><span class="sxs-lookup"><span data-stu-id="79f63-322">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="79f63-323">Model bağlama ve verilerin iş kurallarına uygun olmadığı rapor hataları (örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana bir 0 girildiğinde) oluşturulduktan sonra model doğrulaması oluşur.</span><span class="sxs-lookup"><span data-stu-id="79f63-323">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="79f63-324">Hem model bağlama hem de doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="79f63-324">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="79f63-325">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-325">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="79f63-326">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="79f63-326">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="79f63-327">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="79f63-327">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="79f63-328">Bu durumda, model durumu geçersiz olduğunda hata ayrıntılarını içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="79f63-328">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="79f63-329">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="79f63-329">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="79f63-330">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="79f63-330">Rerun validation</span></span>

<span data-ttu-id="79f63-331">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-331">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="79f63-332">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-332">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="79f63-333">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="79f63-333">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="79f63-334">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-334">Validation attributes</span></span>

<span data-ttu-id="79f63-335">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-335">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="79f63-336">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="79f63-336">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="79f63-337">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="79f63-337">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="79f63-338">Gösterilmemiştir `[ClassicMovie2]`, özel bir özniteliği uygulamak için alternatif bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="79f63-338">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="79f63-339">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="79f63-339">Built-in attributes</span></span>

<span data-ttu-id="79f63-340">Yerleşik doğrulama öznitelikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-340">Built-in validation attributes include:</span></span>

* <span data-ttu-id="79f63-341">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-341">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="79f63-342">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-342">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="79f63-343">Örneğin, *register.cshtml.cs* dosyası, girilen iki parola eşleşmesini doğrulamak için `[Compare]` kullanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-343">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="79f63-344">Kayıt kodunu görmek için [Scaffold kimliği](xref:security/authentication/scaffold-identity) .</span><span class="sxs-lookup"><span data-stu-id="79f63-344">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="79f63-345">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-345">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="79f63-346">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-346">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="79f63-347">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-347">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="79f63-348">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-348">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="79f63-349">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-349">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="79f63-350">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Required]` özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="79f63-350">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="79f63-351">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-351">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="79f63-352">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-352">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="79f63-353">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-353">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="79f63-354">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [`[Remote]` özniteliği](#remote-attribute) .</span><span class="sxs-lookup"><span data-stu-id="79f63-354">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="79f63-355">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-355">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="79f63-356">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="79f63-356">Error messages</span></span>

<span data-ttu-id="79f63-357">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="79f63-357">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="79f63-358">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="79f63-358">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="79f63-359">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="79f63-359">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="79f63-360">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="79f63-360">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="79f63-361">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="79f63-361">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="79f63-362">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="79f63-362">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="79f63-363">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="79f63-363">[Required] attribute</span></span>

<span data-ttu-id="79f63-364">Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="79f63-364">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="79f63-365">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="79f63-365">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="79f63-366">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="79f63-366">[Required] validation on the server</span></span>

<span data-ttu-id="79f63-367">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-367">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="79f63-368">Null yapılamayan bir alan her zaman geçerlidir ve [gerekli] özniteliğinin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="79f63-368">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="79f63-369">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-369">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="79f63-370">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-370">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="79f63-371">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="79f63-371">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="79f63-372">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-372">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="79f63-373">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="79f63-373">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="79f63-374">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="79f63-374">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="79f63-375">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="79f63-375">[Required] validation on the client</span></span>

<span data-ttu-id="79f63-376">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="79f63-376">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="79f63-377">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="79f63-377">On the client:</span></span>

* <span data-ttu-id="79f63-378">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-378">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="79f63-379">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="79f63-379">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="79f63-380">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-380">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="79f63-381">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-381">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="79f63-382">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-382">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="79f63-383">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-383">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="79f63-384">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="79f63-384">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="79f63-385">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="79f63-385">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="79f63-386">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="79f63-386">[Remote] attribute</span></span>

<span data-ttu-id="79f63-387">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="79f63-387">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="79f63-388">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-388">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="79f63-389">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="79f63-389">To implement remote validation:</span></span>

1. <span data-ttu-id="79f63-390">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-390">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="79f63-391">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="79f63-391">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="79f63-392">`"true"`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-392">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="79f63-393">`"false"`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-393">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="79f63-394">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="79f63-394">Display the default error message.</span></span>
   * <span data-ttu-id="79f63-395">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-395">Any other string means the input is invalid.</span></span> <span data-ttu-id="79f63-396">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-396">Display the string as a custom error message.</span></span>

   <span data-ttu-id="79f63-397">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="79f63-397">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="79f63-398">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="79f63-398">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="79f63-399">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-399">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="79f63-400">`Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` metapackage kullanmıyorsanız [Microsoft. AspNetCore. Mvc. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-400">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="79f63-401">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="79f63-401">Additional fields</span></span>

<span data-ttu-id="79f63-402">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-402">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="79f63-403">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-403">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="79f63-404">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="79f63-404">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="79f63-405">`AdditionalFields`, `"FirstName"` ve `"LastName"`dizeler için açıkça ayarlanabilir, ancak [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-405">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="79f63-406">Bu doğrulama için eylem yöntemi hem adı hem de soyadı bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="79f63-406">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="79f63-407">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="79f63-407">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="79f63-408">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="79f63-408">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="79f63-409">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="79f63-409">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="79f63-410">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-410">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="79f63-411">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-411">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="79f63-412">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="79f63-412">Alternatives to built-in attributes</span></span>

<span data-ttu-id="79f63-413">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-413">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="79f63-414">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="79f63-414">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="79f63-415">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="79f63-415">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="79f63-416">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="79f63-416">Custom attributes</span></span>

<span data-ttu-id="79f63-417">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-417">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="79f63-418"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="79f63-418">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="79f63-419">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-419">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="79f63-420">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-420">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="79f63-421">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-421">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="79f63-422">`[ClassicMovie2]` özniteliği önce tarzı denetler ve yalnızca *Klasik*ise devam eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-422">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="79f63-423">Classics olarak tanımlanan filmler için, öznitelik oluşturucusuna geçirilen sınırdan daha sonra olmadığından emin olmak için yayın tarihini denetler.)</span><span class="sxs-lookup"><span data-stu-id="79f63-423">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="79f63-424">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-424">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="79f63-425">`IsValid` yöntemi tarihi ve tarzı denetler.</span><span class="sxs-lookup"><span data-stu-id="79f63-425">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="79f63-426">Doğrulama başarıyla tamamlandığında, `IsValid` bir `ValidationResult.Success` kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="79f63-426">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="79f63-427">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="79f63-427">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="79f63-428">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="79f63-428">IValidatableObject</span></span>

<span data-ttu-id="79f63-429">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="79f63-429">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="79f63-430">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="79f63-430">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="79f63-431">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-431">Top-level node validation</span></span>

<span data-ttu-id="79f63-432">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="79f63-432">Top-level nodes include:</span></span>

* <span data-ttu-id="79f63-433">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="79f63-433">Action parameters</span></span>
* <span data-ttu-id="79f63-434">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-434">Controller properties</span></span>
* <span data-ttu-id="79f63-435">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="79f63-435">Page handler parameters</span></span>
* <span data-ttu-id="79f63-436">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="79f63-436">Page model properties</span></span>

<span data-ttu-id="79f63-437">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-437">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="79f63-438">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="79f63-438">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="79f63-439">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-439">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="79f63-440">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="79f63-440">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="79f63-441">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="79f63-441">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="79f63-442">İlk form bir `99` `Age` değerini bir sorgu dizesi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="79f63-442">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="79f63-443">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="79f63-443">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="79f63-444">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="79f63-444">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="79f63-445">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="79f63-445">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="79f63-446">`CompatibilityVersion.Version_2_1` veya sonraki sürümleriyle çalışırken, en üst düzey düğüm doğrulama varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="79f63-446">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="79f63-447">Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-447">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="79f63-448">Varsayılan seçenek, burada gösterildiği gibi, (`Startup.ConfigureServices`) içinde <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliği ayarlanarak geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="79f63-448">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="79f63-449">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="79f63-449">Maximum errors</span></span>

<span data-ttu-id="79f63-450">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="79f63-450">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="79f63-451">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-451">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="79f63-452">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="79f63-452">Maximum recursion</span></span>

<span data-ttu-id="79f63-453"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="79f63-453"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="79f63-454">Çok derin olan veya sonsuz özyinelemeli özyinelemeli modeller için, doğrulama yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-454">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="79f63-455">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-455">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="79f63-456">`MvcOptions.MaxValidationDepth` varsayılan değeri, `CompatibilityVersion.Version_2_2` veya sonraki sürümleriyle çalışırken 32 ' dir.</span><span class="sxs-lookup"><span data-stu-id="79f63-456">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="79f63-457">Önceki sürümler için değer null, bu da derinlemesine bir kısıtlama kısıtlaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="79f63-457">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="79f63-458">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="79f63-458">Automatic short-circuit</span></span>

<span data-ttu-id="79f63-459">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="79f63-459">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="79f63-460">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="79f63-460">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="79f63-461">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="79f63-461">Disable validation</span></span>

<span data-ttu-id="79f63-462">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="79f63-462">To disable validation:</span></span>

1. <span data-ttu-id="79f63-463">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-463">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="79f63-464">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-464">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="79f63-465">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79f63-465">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="79f63-466">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-466">Client-side validation</span></span>

<span data-ttu-id="79f63-467">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="79f63-467">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="79f63-468">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="79f63-468">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="79f63-469">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="79f63-469">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="79f63-470">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="79f63-470">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="79f63-471">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="79f63-471">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="79f63-472">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="79f63-472">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="79f63-473">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-473">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="79f63-474">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-474">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="79f63-475">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="79f63-475">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="79f63-476">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler.</span><span class="sxs-lookup"><span data-stu-id="79f63-476">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="79f63-477">HTML çıkışındaki `data-` özniteliklerinin `ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-477">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="79f63-478">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="79f63-478">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="79f63-479">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [Required ()](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve sonra bu iletiyi eşlik eden **\<span >** öğesinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="79f63-479">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="79f63-480">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="79f63-480">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="79f63-481">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-481">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="79f63-482">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="79f63-482">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="79f63-483">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="79f63-483">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="79f63-484">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="79f63-484">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="79f63-485">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="79f63-485">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="79f63-486">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="79f63-486">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="79f63-487">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="79f63-487">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="79f63-488">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="79f63-488">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="79f63-489">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="79f63-489">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="79f63-490">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-490">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="79f63-491">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="79f63-491">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="79f63-492">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="79f63-492">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="79f63-493">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="79f63-493">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="79f63-494">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="79f63-494">Custom client-side validation</span></span>

<span data-ttu-id="79f63-495">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-495">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="79f63-496">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `ClassicMovie` ve `ClassicMovie2` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="79f63-496">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="79f63-497">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="79f63-497">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="79f63-498">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="79f63-498">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="79f63-499">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="79f63-499">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="79f63-500">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="79f63-500">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="79f63-501">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="79f63-501">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="79f63-502">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="79f63-502">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="79f63-503">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="79f63-503">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="79f63-504">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="79f63-504">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="79f63-505">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="79f63-505">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="79f63-506">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="79f63-506">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="79f63-507">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="79f63-507">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="79f63-508">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="79f63-508">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="79f63-509">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="79f63-509">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="79f63-510">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="79f63-510">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="79f63-511">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-511">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="79f63-512">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="79f63-512">To add client validation by using this method:</span></span>

1. <span data-ttu-id="79f63-513">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-513">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="79f63-514">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="79f63-514">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="79f63-515">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="79f63-515">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="79f63-516"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-516">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="79f63-517">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="79f63-517">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="79f63-518">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="79f63-518">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="79f63-519">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="79f63-519">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="79f63-520">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie2` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="79f63-520">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="79f63-521">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="79f63-521">To add client validation by using this method:</span></span>

* <span data-ttu-id="79f63-522">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79f63-522">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="79f63-523">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="79f63-523">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="79f63-524">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="79f63-524">Disable client-side validation</span></span>

<span data-ttu-id="79f63-525">Aşağıdaki kod, MVC görünümlerinde istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="79f63-525">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="79f63-526">Ve Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="79f63-526">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="79f63-527">İstemci doğrulamasını devre dışı bırakmaya yönelik başka bir seçenek de, *. cshtml* dosyanızdaki `_ValidationScriptsPartial` başvurusunu açıklamaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="79f63-527">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79f63-528">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="79f63-528">Additional resources</span></span>

* [<span data-ttu-id="79f63-529">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="79f63-529">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="79f63-530">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="79f63-530">Model Binding</span></span>](model-binding.md)

::: moniker-end
