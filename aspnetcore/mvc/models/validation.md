---
title: ASP.NET Core MVC 'de model doğrulaması
author: rick-anderson
description: ASP.NET Core MVC ve Razor Pages model doğrulaması hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2019
uid: mvc/models/validation
ms.openlocfilehash: 8e9e588c8665962b2fe285b0feab977b16938154
ms.sourcegitcommit: 99cdd60a26ff0970880bb43c558d78b1ef17c237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2019
ms.locfileid: "73915520"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="349e1-103">ASP.NET Core MVC ve Razor Pages model doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="349e1-104">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="349e1-105">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="349e1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="349e1-106">Model durumu</span><span class="sxs-lookup"><span data-stu-id="349e1-106">Model state</span></span>

<span data-ttu-id="349e1-107">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="349e1-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="349e1-108">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır (örneğin, bir tamsayı bekleyen bir alana bir "x" girilir).</span><span class="sxs-lookup"><span data-stu-id="349e1-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="349e1-109">Model bağlama ve verilerin iş kurallarına uygun olmadığı rapor hataları (örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana bir 0 girildiğinde) oluşturulduktan sonra model doğrulaması oluşur.</span><span class="sxs-lookup"><span data-stu-id="349e1-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="349e1-110">Hem model bağlama hem de doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="349e1-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="349e1-111">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="349e1-112">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="349e1-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="349e1-113">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="349e1-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="349e1-114">Bu durumda, model durumu geçersiz olduğunda sorun ayrıntıları içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="349e1-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="349e1-115">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="349e1-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="349e1-116">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="349e1-116">Rerun validation</span></span>

<span data-ttu-id="349e1-117">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="349e1-118">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="349e1-119">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="349e1-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="349e1-120">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-120">Validation attributes</span></span>

<span data-ttu-id="349e1-121">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="349e1-122">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="349e1-122">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="349e1-123">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="349e1-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="349e1-124">(Gösterilmez `[ClassicMovie2]`, özel bir özniteliği uygulamak için alternatif bir yol gösterir.)</span><span class="sxs-lookup"><span data-stu-id="349e1-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="349e1-125">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="349e1-125">Built-in attributes</span></span>

<span data-ttu-id="349e1-126">Yerleşik doğrulama özniteliklerinden bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="349e1-127">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="349e1-128">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="349e1-129">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="349e1-130">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="349e1-131">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="349e1-132">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="349e1-133">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="349e1-134">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Required] özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="349e1-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="349e1-135">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="349e1-136">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="349e1-137">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="349e1-138">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Remote] Attribute](#remote-attribute) .</span><span class="sxs-lookup"><span data-stu-id="349e1-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="349e1-139">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="349e1-140">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="349e1-140">Error messages</span></span>

<span data-ttu-id="349e1-141">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="349e1-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="349e1-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="349e1-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="349e1-143">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="349e1-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="349e1-144">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="349e1-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="349e1-145">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="349e1-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="349e1-146">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="349e1-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="349e1-147">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="349e1-147">[Required] attribute</span></span>

<span data-ttu-id="349e1-148">Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="349e1-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="349e1-149">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="349e1-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="349e1-150">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="349e1-150">[Required] validation on the server</span></span>

<span data-ttu-id="349e1-151">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="349e1-152">Null yapılamayan bir alan her zaman geçerlidir ve [gerekli] özniteliğinin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="349e1-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="349e1-153">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="349e1-154">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="349e1-155">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="349e1-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="349e1-156">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="349e1-157">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="349e1-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="349e1-158">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="349e1-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="349e1-159">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="349e1-159">[Required] validation on the client</span></span>

<span data-ttu-id="349e1-160">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="349e1-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="349e1-161">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="349e1-161">On the client:</span></span>

* <span data-ttu-id="349e1-162">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="349e1-163">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="349e1-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="349e1-164">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="349e1-165">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="349e1-166">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="349e1-167">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="349e1-168">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="349e1-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="349e1-169">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="349e1-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="349e1-170">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="349e1-170">[Remote] attribute</span></span>

<span data-ttu-id="349e1-171">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="349e1-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="349e1-172">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="349e1-173">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="349e1-173">To implement remote validation:</span></span>

1. <span data-ttu-id="349e1-174">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="349e1-175">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="349e1-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="349e1-176">`"true"`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="349e1-177">`"false"`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="349e1-178">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="349e1-178">Display the default error message.</span></span>
   * <span data-ttu-id="349e1-179">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="349e1-180">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="349e1-181">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="349e1-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="349e1-182">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="349e1-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="349e1-183">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-183">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="349e1-184">`Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` metapackage kullanmıyorsanız [Microsoft. AspNetCore. Mvc. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-184">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="349e1-185">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="349e1-185">Additional fields</span></span>

<span data-ttu-id="349e1-186">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-186">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="349e1-187">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-187">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="349e1-188">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="349e1-188">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="349e1-189">`AdditionalFields`, `"FirstName"` ve `"LastName"`dizeler için açıkça ayarlanabilir, ancak [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-189">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="349e1-190">Bu doğrulama için eylem yöntemi hem adı hem de soyadı bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="349e1-190">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="349e1-191">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="349e1-191">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="349e1-192">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="349e1-192">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="349e1-193">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="349e1-193">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="349e1-194">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-194">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="349e1-195">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-195">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="349e1-196">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="349e1-196">Alternatives to built-in attributes</span></span>

<span data-ttu-id="349e1-197">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-197">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="349e1-198">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="349e1-198">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="349e1-199">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="349e1-199">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="349e1-200">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="349e1-200">Custom attributes</span></span>

<span data-ttu-id="349e1-201">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-201">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="349e1-202"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="349e1-202">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="349e1-203">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-203">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="349e1-204">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-204">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="349e1-205">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-205">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="349e1-206">`[ClassicMovie2]` özniteliği önce tarzı denetler ve yalnızca *Klasik*ise devam eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-206">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="349e1-207">Classics olarak tanımlanan filmler için, öznitelik oluşturucusuna geçirilen sınırdan daha sonra olmadığından emin olmak için yayın tarihini denetler.)</span><span class="sxs-lookup"><span data-stu-id="349e1-207">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="349e1-208">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-208">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="349e1-209">`IsValid` yöntemi tarihi ve tarzı denetler.</span><span class="sxs-lookup"><span data-stu-id="349e1-209">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="349e1-210">Doğrulama başarıyla tamamlandığında, `IsValid` bir `ValidationResult.Success` kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="349e1-210">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="349e1-211">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="349e1-211">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="349e1-212">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="349e1-212">IValidatableObject</span></span>

<span data-ttu-id="349e1-213">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="349e1-213">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="349e1-214">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="349e1-214">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="349e1-215">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-215">Top-level node validation</span></span>

<span data-ttu-id="349e1-216">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="349e1-216">Top-level nodes include:</span></span>

* <span data-ttu-id="349e1-217">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="349e1-217">Action parameters</span></span>
* <span data-ttu-id="349e1-218">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-218">Controller properties</span></span>
* <span data-ttu-id="349e1-219">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="349e1-219">Page handler parameters</span></span>
* <span data-ttu-id="349e1-220">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-220">Page model properties</span></span>

<span data-ttu-id="349e1-221">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-221">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="349e1-222">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="349e1-222">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="349e1-223">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-223">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="349e1-224">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="349e1-224">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="349e1-225">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-225">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="349e1-226">İlk form bir `99` `Age` değerini bir sorgu dizesi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="349e1-226">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="349e1-227">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-227">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="349e1-228">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="349e1-228">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="349e1-229">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="349e1-229">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="349e1-230">`CompatibilityVersion.Version_2_1` veya sonraki sürümleriyle çalışırken, en üst düzey düğüm doğrulama varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="349e1-230">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="349e1-231">Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-231">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="349e1-232">Varsayılan seçenek, burada gösterildiği gibi, (`Startup.ConfigureServices`) içinde <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliği ayarlanarak geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="349e1-232">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="349e1-233">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="349e1-233">Maximum errors</span></span>

<span data-ttu-id="349e1-234">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="349e1-234">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="349e1-235">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-235">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="349e1-236">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="349e1-236">Maximum recursion</span></span>

<span data-ttu-id="349e1-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="349e1-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="349e1-238">Çok derin olan veya sonsuz özyinelemeli özyinelemeli modeller için, doğrulama yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-238">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="349e1-239">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="349e1-240">`MvcOptions.MaxValidationDepth` varsayılan değeri, `CompatibilityVersion.Version_2_2` veya sonraki sürümleriyle çalışırken 200 ' dir.</span><span class="sxs-lookup"><span data-stu-id="349e1-240">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="349e1-241">Önceki sürümler için değer null, bu da derinlemesine bir kısıtlama kısıtlaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-241">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="349e1-242">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="349e1-242">Automatic short-circuit</span></span>

<span data-ttu-id="349e1-243">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="349e1-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="349e1-244">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="349e1-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="349e1-245">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="349e1-245">Disable validation</span></span>

<span data-ttu-id="349e1-246">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="349e1-246">To disable validation:</span></span>

1. <span data-ttu-id="349e1-247">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="349e1-248">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="349e1-249">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="349e1-250">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-250">Client-side validation</span></span>

<span data-ttu-id="349e1-251">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="349e1-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="349e1-252">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="349e1-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="349e1-253">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="349e1-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="349e1-254">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="349e1-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="349e1-255">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="349e1-256">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="349e1-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="349e1-257">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="349e1-258">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="349e1-259">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="349e1-260">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler.</span><span class="sxs-lookup"><span data-stu-id="349e1-260">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="349e1-261">HTML çıkışındaki `data-` özniteliklerinin `ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="349e1-262">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="349e1-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="349e1-263">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve bu ileti, eşlik eden **\<span >** öğesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="349e1-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="349e1-264">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="349e1-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="349e1-265">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="349e1-266">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="349e1-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="349e1-267">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="349e1-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="349e1-268">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="349e1-269">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="349e1-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="349e1-270">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="349e1-271">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="349e1-272">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="349e1-273">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="349e1-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="349e1-274">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="349e1-275">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="349e1-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="349e1-276">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="349e1-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="349e1-277">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="349e1-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="349e1-278">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-278">Custom client-side validation</span></span>

<span data-ttu-id="349e1-279">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="349e1-280">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `ClassicMovie` ve `ClassicMovie2` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="349e1-280">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="349e1-281">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="349e1-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="349e1-282">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="349e1-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="349e1-283">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="349e1-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="349e1-284">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="349e1-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="349e1-285">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="349e1-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="349e1-286">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="349e1-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="349e1-287">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="349e1-288">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="349e1-289">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="349e1-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="349e1-290">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="349e1-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="349e1-291">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="349e1-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="349e1-292">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="349e1-293">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="349e1-294">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="349e1-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="349e1-295">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="349e1-296">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="349e1-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="349e1-297">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="349e1-298">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="349e1-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="349e1-299">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="349e1-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="349e1-300"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="349e1-301">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="349e1-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="349e1-302">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="349e1-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="349e1-303">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="349e1-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="349e1-304">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie2` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="349e1-305">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="349e1-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="349e1-306">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="349e1-307">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="349e1-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="349e1-308">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="349e1-308">Disable client-side validation</span></span>

<span data-ttu-id="349e1-309">Aşağıdaki kod, MVC görünümlerinde istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="349e1-309">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="349e1-310">Ve Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="349e1-310">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="349e1-311">İstemci doğrulamasını devre dışı bırakmaya yönelik başka bir seçenek de, *. cshtml* dosyanızdaki `_ValidationScriptsPartial` başvurusunu açıklamaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="349e1-311">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="349e1-312">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="349e1-312">Additional resources</span></span>

* [<span data-ttu-id="349e1-313">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="349e1-313">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="349e1-314">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="349e1-314">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="349e1-315">Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-315">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="349e1-316">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="349e1-316">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="349e1-317">Model durumu</span><span class="sxs-lookup"><span data-stu-id="349e1-317">Model state</span></span>

<span data-ttu-id="349e1-318">Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="349e1-318">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="349e1-319">[Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır (örneğin, bir tamsayı bekleyen bir alana bir "x" girilir).</span><span class="sxs-lookup"><span data-stu-id="349e1-319">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="349e1-320">Model bağlama ve verilerin iş kurallarına uygun olmadığı rapor hataları (örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana bir 0 girildiğinde) oluşturulduktan sonra model doğrulaması oluşur.</span><span class="sxs-lookup"><span data-stu-id="349e1-320">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="349e1-321">Hem model bağlama hem de doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="349e1-321">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="349e1-322">Web uygulamaları için, uygulamanın `ModelState.IsValid` İnceleme ve uygun şekilde tepki verme sorumluluğu vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-322">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="349e1-323">Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:</span><span class="sxs-lookup"><span data-stu-id="349e1-323">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="349e1-324">`[ApiController]` özniteliğine sahip olmaları durumunda Web API denetleyicilerinin `ModelState.IsValid` denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="349e1-324">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="349e1-325">Bu durumda, model durumu geçersiz olduğunda sorun ayrıntıları içeren bir otomatik HTTP 400 yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="349e1-325">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="349e1-326">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="349e1-326">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="349e1-327">Doğrulamayı yeniden çalıştır</span><span class="sxs-lookup"><span data-stu-id="349e1-327">Rerun validation</span></span>

<span data-ttu-id="349e1-328">Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-328">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="349e1-329">Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-329">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="349e1-330">Doğrulamayı yeniden çalıştırmak için, burada gösterildiği gibi `TryValidateModel` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="349e1-330">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="349e1-331">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-331">Validation attributes</span></span>

<span data-ttu-id="349e1-332">Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-332">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="349e1-333">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="349e1-333">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="349e1-334">`[ClassicMovie]` özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="349e1-334">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="349e1-335">(Gösterilmez `[ClassicMovie2]`, özel bir özniteliği uygulamak için alternatif bir yol gösterir.)</span><span class="sxs-lookup"><span data-stu-id="349e1-335">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="349e1-336">Yerleşik öznitelikler</span><span class="sxs-lookup"><span data-stu-id="349e1-336">Built-in attributes</span></span>

<span data-ttu-id="349e1-337">Yerleşik doğrulama özniteliklerinden bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-337">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="349e1-338">`[CreditCard]`: özelliğin kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-338">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="349e1-339">`[Compare]`: bir modeldeki iki özelliği eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-339">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="349e1-340">`[EmailAddress]`: özelliğin bir e-posta biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-340">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="349e1-341">`[Phone]`: özelliğin bir telefon numarası biçimi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-341">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="349e1-342">`[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-342">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="349e1-343">`[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-343">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="349e1-344">`[Required]`: alanın null olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-344">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="349e1-345">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Required] özniteliği](#required-attribute) .</span><span class="sxs-lookup"><span data-stu-id="349e1-345">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="349e1-346">`[StringLength]`: dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-346">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="349e1-347">`[Url]`: özelliğin bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-347">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="349e1-348">`[Remote]`: sunucuda bir eylem yöntemi çağırarak istemcide girişi doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-348">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="349e1-349">Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Remote] Attribute](#remote-attribute) .</span><span class="sxs-lookup"><span data-stu-id="349e1-349">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="349e1-350">Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-350">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="349e1-351">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="349e1-351">Error messages</span></span>

<span data-ttu-id="349e1-352">Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="349e1-352">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="349e1-353">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="349e1-353">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="349e1-354">Dahili olarak, öznitelikler çağrı, alan adı için bir yer tutucu ve bazen ek yer tutucular ile `String.Format`.</span><span class="sxs-lookup"><span data-stu-id="349e1-354">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="349e1-355">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="349e1-355">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="349e1-356">Bir `Name` özelliğine uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.</span><span class="sxs-lookup"><span data-stu-id="349e1-356">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="349e1-357">Belirli bir özniteliğin hata iletisinde hangi parametrelerin `String.Format` geçtiğini öğrenmek için bkz. [Datareek açıklamaları kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="349e1-357">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="349e1-358">[Zorunlu] özniteliği</span><span class="sxs-lookup"><span data-stu-id="349e1-358">[Required] attribute</span></span>

<span data-ttu-id="349e1-359">Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="349e1-359">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="349e1-360">`decimal` ve `int` gibi [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) null atanamaz.</span><span class="sxs-lookup"><span data-stu-id="349e1-360">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="349e1-361">[Zorunlu] sunucuda doğrulama</span><span class="sxs-lookup"><span data-stu-id="349e1-361">[Required] validation on the server</span></span>

<span data-ttu-id="349e1-362">Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-362">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="349e1-363">Null yapılamayan bir alan her zaman geçerlidir ve [gerekli] özniteliğinin hata mesajı hiçbir zaman gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="349e1-363">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="349e1-364">Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve `The value '' is invalid`gibi bir hata mesajı elde edebilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-364">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="349e1-365">Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-365">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="349e1-366">Alanı null yapılabilir yapın (örneğin, `decimal`yerine `decimal?`).</span><span class="sxs-lookup"><span data-stu-id="349e1-366">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="349e1-367">[Null yapılabilir\<t >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-367">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="349e1-368">Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:</span><span class="sxs-lookup"><span data-stu-id="349e1-368">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="349e1-369">İçin varsayılan iletileri ayarlayabilmeniz için model bağlama hataları hakkında daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="349e1-369">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="349e1-370">[Zorunlu] istemcide doğrulama</span><span class="sxs-lookup"><span data-stu-id="349e1-370">[Required] validation on the client</span></span>

<span data-ttu-id="349e1-371">Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="349e1-371">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="349e1-372">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="349e1-372">On the client:</span></span>

* <span data-ttu-id="349e1-373">Yalnızca girdi girildiğinde bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-373">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="349e1-374">Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="349e1-374">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="349e1-375">Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-375">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="349e1-376">Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-376">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="349e1-377">Daha önce belirtildiği gibi, null olamayan türler `[Required]` bir özniteliğe sahip olsa da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-377">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="349e1-378">Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-378">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="349e1-379">Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="349e1-379">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="349e1-380">Özel bir hata iletisi belirtmek için özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="349e1-380">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="349e1-381">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="349e1-381">[Remote] attribute</span></span>

<span data-ttu-id="349e1-382">`[Remote]` özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular.</span><span class="sxs-lookup"><span data-stu-id="349e1-382">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="349e1-383">Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-383">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="349e1-384">Uzaktan doğrulamayı uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="349e1-384">To implement remote validation:</span></span>

1. <span data-ttu-id="349e1-385">JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-385">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="349e1-386">JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="349e1-386">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="349e1-387">`"true"`, giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-387">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="349e1-388">`"false"`, `undefined`veya `null`, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-388">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="349e1-389">Varsayılan hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="349e1-389">Display the default error message.</span></span>
   * <span data-ttu-id="349e1-390">Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-390">Any other string means the input is invalid.</span></span> <span data-ttu-id="349e1-391">Dizeyi özel bir hata iletisi olarak görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-391">Display the string as a custom error message.</span></span>

   <span data-ttu-id="349e1-392">Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="349e1-392">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="349e1-393">Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi yöntemine işaret eden bir `[Remote]` özniteliğiyle özelliğe not ekleyin:</span><span class="sxs-lookup"><span data-stu-id="349e1-393">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="349e1-394">`[Remote]` özniteliği `Microsoft.AspNetCore.Mvc` ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-394">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="349e1-395">`Microsoft.AspNetCore.App` veya `Microsoft.AspNetCore.All` metapackage kullanmıyorsanız [Microsoft. AspNetCore. Mvc. ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-395">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="349e1-396">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="349e1-396">Additional fields</span></span>

<span data-ttu-id="349e1-397">`[Remote]` özniteliğinin `AdditionalFields` özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-397">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="349e1-398">Örneğin, `User` modelinde `FirstName` ve `LastName` özellikler varsa, mevcut hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-398">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="349e1-399">Aşağıdaki örnek `AdditionalFields`nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="349e1-399">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="349e1-400">`AdditionalFields`, `"FirstName"` ve `"LastName"`dizeler için açıkça ayarlanabilir, ancak [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kullanılması daha sonra yeniden düzenlemeyi basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-400">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="349e1-401">Bu doğrulama için eylem yöntemi hem adı hem de soyadı bağımsız değişkenlerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="349e1-401">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="349e1-402">Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="349e1-402">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="349e1-403">İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="349e1-403">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="349e1-404">Örneğin, modele bir `MiddleName` özelliği eklemek için, `[Remote]` özniteliğini aşağıdaki örnekte gösterildiği gibi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="349e1-404">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="349e1-405">Tüm öznitelik bağımsız değişkenleri gibi `AdditionalFields`, sabit bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-405">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="349e1-406">Bu nedenle, `AdditionalFields`başlatmak için, [enterpolasyonlu bir dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-406">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="349e1-407">Yerleşik özniteliklerin alternatifleri</span><span class="sxs-lookup"><span data-stu-id="349e1-407">Alternatives to built-in attributes</span></span>

<span data-ttu-id="349e1-408">Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-408">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="349e1-409">[Özel öznitelikler oluşturun](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="349e1-409">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="349e1-410">[IValidatableObject uygulayın](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="349e1-410">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="349e1-411">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="349e1-411">Custom attributes</span></span>

<span data-ttu-id="349e1-412">Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-412">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="349e1-413"><xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemini geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="349e1-413">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="349e1-414">`IsValid` yöntemi, doğrulanacak girdi olan *Value*adlı bir nesne kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-414">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="349e1-415">Aşırı yükleme, model bağlama tarafından oluşturulan model örneği gibi ek bilgiler sağlayan `ValidationContext` nesnesini de kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-415">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="349e1-416">Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-416">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="349e1-417">`[ClassicMovie2]` özniteliği önce tarzı denetler ve yalnızca *Klasik*ise devam eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-417">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="349e1-418">Classics olarak tanımlanan filmler için, öznitelik oluşturucusuna geçirilen sınırdan daha sonra olmadığından emin olmak için yayın tarihini denetler.)</span><span class="sxs-lookup"><span data-stu-id="349e1-418">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="349e1-419">Yukarıdaki örnekteki `movie` değişkeni, form gönderimden verileri içeren bir `Movie` nesnesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-419">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="349e1-420">`IsValid` yöntemi tarihi ve tarzı denetler.</span><span class="sxs-lookup"><span data-stu-id="349e1-420">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="349e1-421">Doğrulama başarıyla tamamlandığında, `IsValid` bir `ValidationResult.Success` kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="349e1-421">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="349e1-422">Doğrulama başarısız olduğunda, hata iletisi içeren bir `ValidationResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="349e1-422">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="349e1-423">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="349e1-423">IValidatableObject</span></span>

<span data-ttu-id="349e1-424">Yukarıdaki örnek yalnızca `Movie` türleriyle birlikte geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="349e1-424">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="349e1-425">Sınıf düzeyinde doğrulama için başka bir seçenek de, aşağıdaki örnekte gösterildiği gibi model sınıfında `IValidatableObject` uygulamaktır:</span><span class="sxs-lookup"><span data-stu-id="349e1-425">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="349e1-426">Üst düzey düğüm doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-426">Top-level node validation</span></span>

<span data-ttu-id="349e1-427">Üst düzey düğümler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="349e1-427">Top-level nodes include:</span></span>

* <span data-ttu-id="349e1-428">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="349e1-428">Action parameters</span></span>
* <span data-ttu-id="349e1-429">Denetleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-429">Controller properties</span></span>
* <span data-ttu-id="349e1-430">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="349e1-430">Page handler parameters</span></span>
* <span data-ttu-id="349e1-431">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="349e1-431">Page model properties</span></span>

<span data-ttu-id="349e1-432">Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-432">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="349e1-433">Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak için <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> kullanır:</span><span class="sxs-lookup"><span data-stu-id="349e1-433">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="349e1-434">Üst düzey düğümler, doğrulama öznitelikleri ile <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-434">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="349e1-435">Örnek uygulamadan aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametresinin sorgu dizesinden bağlanması gerektiğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="349e1-435">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="349e1-436">Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır.</span><span class="sxs-lookup"><span data-stu-id="349e1-436">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="349e1-437">İlk form bir `99` `Age` değerini bir sorgu dizesi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="349e1-437">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="349e1-438">Sorgu dizesinden düzgün şekilde biçimlendirilen bir `age` parametresi gönderildiğinde, form doğrular.</span><span class="sxs-lookup"><span data-stu-id="349e1-438">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="349e1-439">Denetim yaşı sayfasındaki ikinci form, isteğin gövdesinde `Age` değerini gönderir ve doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="349e1-439">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="349e1-440">`age` parametresi bir sorgu dizesinden gelmiş olması gerektiğinden bağlama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="349e1-440">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="349e1-441">`CompatibilityVersion.Version_2_1` veya sonraki sürümleriyle çalışırken, en üst düzey düğüm doğrulama varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="349e1-441">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="349e1-442">Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-442">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="349e1-443">Varsayılan seçenek, burada gösterildiği gibi, (`Startup.ConfigureServices`) içinde <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliği ayarlanarak geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="349e1-443">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="349e1-444">En fazla hata sayısı</span><span class="sxs-lookup"><span data-stu-id="349e1-444">Maximum errors</span></span>

<span data-ttu-id="349e1-445">En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200).</span><span class="sxs-lookup"><span data-stu-id="349e1-445">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="349e1-446">Bu numarayı `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-446">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="349e1-447">En yüksek özyineleme</span><span class="sxs-lookup"><span data-stu-id="349e1-447">Maximum recursion</span></span>

<span data-ttu-id="349e1-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>, doğrulanan modelin nesne grafiğinin altına geçer.</span><span class="sxs-lookup"><span data-stu-id="349e1-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="349e1-449">Çok derin olan veya sonsuz özyinelemeli özyinelemeli modeller için, doğrulama yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-449">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="349e1-450">[Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-450">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="349e1-451">`MvcOptions.MaxValidationDepth` varsayılan değeri, `CompatibilityVersion.Version_2_2` veya sonraki sürümleriyle çalışırken 200 ' dir.</span><span class="sxs-lookup"><span data-stu-id="349e1-451">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="349e1-452">Önceki sürümler için değer null, bu da derinlemesine bir kısıtlama kısıtlaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="349e1-452">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="349e1-453">Otomatik kısa devre</span><span class="sxs-lookup"><span data-stu-id="349e1-453">Automatic short-circuit</span></span>

<span data-ttu-id="349e1-454">Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı).</span><span class="sxs-lookup"><span data-stu-id="349e1-454">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="349e1-455">Çalışma zamanının, herhangi bir Doğrulayıcıları olmayan `byte[]`, `string[]`, `Dictionary<string, string>`) ve karmaşık nesne grafikleri dahil olmak üzere doğrulamayı atlayan nesneler.</span><span class="sxs-lookup"><span data-stu-id="349e1-455">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="349e1-456">Doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="349e1-456">Disable validation</span></span>

<span data-ttu-id="349e1-457">Doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="349e1-457">To disable validation:</span></span>

1. <span data-ttu-id="349e1-458">Hiçbir alanı geçersiz olarak işaretlememeyen bir `IObjectModelValidator` uygulamasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-458">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="349e1-459">Bağımlılık ekleme kapsayıcısında varsayılan `IObjectModelValidator` uygulamasını değiştirmek için `Startup.ConfigureServices` aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-459">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="349e1-460">Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="349e1-460">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="349e1-461">İstemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-461">Client-side validation</span></span>

<span data-ttu-id="349e1-462">İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler.</span><span class="sxs-lookup"><span data-stu-id="349e1-462">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="349e1-463">Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="349e1-463">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="349e1-464">Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="349e1-464">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="349e1-465">*_Layout. cshtml* ve *_ValidationScriptsPartial. cshtml* ' deki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:</span><span class="sxs-lookup"><span data-stu-id="349e1-465">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="349e1-466">[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="349e1-466">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="349e1-467">JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde.</span><span class="sxs-lookup"><span data-stu-id="349e1-467">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="349e1-468">Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama GEREKTIREN form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-468">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="349e1-469">jQuery unobtrusive doğrulaması `data-` özniteliklerini ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkin şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-469">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="349e1-470">Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="349e1-470">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="349e1-471">Önceki etiket yardımcıları aşağıdaki HTML 'yi işler.</span><span class="sxs-lookup"><span data-stu-id="349e1-471">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="349e1-472">HTML çıkışındaki `data-` özniteliklerinin `ReleaseDate` özelliğinin doğrulama özniteliklerine karşılık geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-472">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="349e1-473">`data-val-required` özniteliği, Kullanıcı Yayın tarihi alanını doldurmazsa görüntülenecek bir hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="349e1-473">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="349e1-474">jQuery unobtrusive doğrulaması bu değeri jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) yöntemine geçirir ve bu ileti, eşlik eden **\<span >** öğesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="349e1-474">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="349e1-475">Veri türü doğrulama, bir `[DataType]` özniteliği tarafından geçersiz kılınmadıkça, özelliğin .NET türünü temel alır.</span><span class="sxs-lookup"><span data-stu-id="349e1-475">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="349e1-476">Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-476">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="349e1-477">`[EmailAddress]` gibi öznitelikleri ve alt sınıfları `[DataType]`, hata iletisini belirtmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="349e1-477">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="349e1-478">Dinamik formlara doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="349e1-478">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="349e1-479">jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir.</span><span class="sxs-lookup"><span data-stu-id="349e1-479">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="349e1-480">Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="349e1-480">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="349e1-481">Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin.</span><span class="sxs-lookup"><span data-stu-id="349e1-481">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="349e1-482">Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="349e1-482">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="349e1-483">`$.validator.unobtrusive.parse()` yöntemi bir bağımsız değişkeni için jQuery seçiciyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="349e1-483">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="349e1-484">Bu yöntem, jQuery 'in bu seçicideki formların `data-` özniteliklerini ayrıştırmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="349e1-484">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="349e1-485">Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-485">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="349e1-486">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="349e1-486">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="349e1-487">`$.validator.unobtrusive.parse()` yöntemi, `<input>` ve `<select/>`gibi tek dinamik olarak oluşturulan denetimlerde değil, formun tamamına göre çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="349e1-487">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="349e1-488">Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:</span><span class="sxs-lookup"><span data-stu-id="349e1-488">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="349e1-489">Özel istemci tarafı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="349e1-489">Custom client-side validation</span></span>

<span data-ttu-id="349e1-490">Özel istemci tarafı doğrulama işlemi, özel bir jQuery Validate bağdaştırıcısıyla çalışan `data-` HTML öznitelikleri oluşturarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-490">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="349e1-491">Aşağıdaki örnek bağdaştırıcı kodu, bu makalede daha önce sunulan `ClassicMovie` ve `ClassicMovie2` öznitelikleri için yazılmıştır:</span><span class="sxs-lookup"><span data-stu-id="349e1-491">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="349e1-492">Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.</span><span class="sxs-lookup"><span data-stu-id="349e1-492">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="349e1-493">Belirli bir alan için bir bağdaştırıcının kullanımı, `data-` öznitelikleri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="349e1-493">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="349e1-494">Alana, doğrulamaya tabi olacak şekilde bayrak ekleyin (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="349e1-494">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="349e1-495">Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.</span><span class="sxs-lookup"><span data-stu-id="349e1-495">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="349e1-496">Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="349e1-496">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="349e1-497">Aşağıdaki örnek, örnek uygulamanın `ClassicMovie` özniteliği için `data-` özniteliklerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="349e1-497">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="349e1-498">Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) `data-` öznitelikleri işlemek için doğrulama özniteliklerinden bilgi kullanır.</span><span class="sxs-lookup"><span data-stu-id="349e1-498">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="349e1-499">Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="349e1-499">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="349e1-500">`AttributeAdapterBase<TAttribute>` ve `IValidationAttributeAdapterProvider`uygulayan bir sınıftan türeten bir sınıf oluşturun ve özniteinizi ve bağdaştırıcısını dı olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="349e1-500">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="349e1-501">Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler.</span><span class="sxs-lookup"><span data-stu-id="349e1-501">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="349e1-502">Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="349e1-502">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="349e1-503">`ValidationAttribute` sınıfınıza `IClientModelValidator` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="349e1-503">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="349e1-504">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="349e1-504">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="349e1-505">İstemci tarafı doğrulaması için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="349e1-505">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="349e1-506">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-506">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="349e1-507">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="349e1-507">To add client validation by using this method:</span></span>

1. <span data-ttu-id="349e1-508">Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-508">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="349e1-509">Özniteliği [Attributeadapterbase\<t >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'dan türeten.</span><span class="sxs-lookup"><span data-stu-id="349e1-509">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="349e1-510">Aşağıdaki örnekte gösterildiği gibi, işlenen çıktıya `data-` öznitelikleri ekleyen bir `AddValidation` yöntemi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="349e1-510">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="349e1-511"><xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>uygulayan bir bağdaştırıcı sağlayıcısı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-511">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="349e1-512">`GetAttributeAdapter` yöntemi, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:</span><span class="sxs-lookup"><span data-stu-id="349e1-512">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="349e1-513">Dı için bağdaştırıcı sağlayıcısını `Startup.ConfigureServices`Kaydet:</span><span class="sxs-lookup"><span data-stu-id="349e1-513">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="349e1-514">İstemci tarafı doğrulaması için ılientmodelvalidator</span><span class="sxs-lookup"><span data-stu-id="349e1-514">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="349e1-515">HTML 'de `data-` öznitelikleri işleme yöntemi, örnek uygulamadaki `ClassicMovie2` özniteliği tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="349e1-515">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="349e1-516">Bu yöntemi kullanarak istemci doğrulaması eklemek için:</span><span class="sxs-lookup"><span data-stu-id="349e1-516">To add client validation by using this method:</span></span>

* <span data-ttu-id="349e1-517">Özel doğrulama özniteliğinde `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="349e1-517">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="349e1-518">`AddValidation` yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için `data-` öznitelikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="349e1-518">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="349e1-519">İstemci tarafı doğrulamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="349e1-519">Disable client-side validation</span></span>

<span data-ttu-id="349e1-520">Aşağıdaki kod, MVC görünümlerinde istemci doğrulamasını devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="349e1-520">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="349e1-521">Ve Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="349e1-521">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="349e1-522">İstemci doğrulamasını devre dışı bırakmaya yönelik başka bir seçenek de, *. cshtml* dosyanızdaki `_ValidationScriptsPartial` başvurusunu açıklamaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="349e1-522">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="349e1-523">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="349e1-523">Additional resources</span></span>

* [<span data-ttu-id="349e1-524">System. ComponentModel. Dataaçıklamalarda ad alanı</span><span class="sxs-lookup"><span data-stu-id="349e1-524">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="349e1-525">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="349e1-525">Model Binding</span></span>](model-binding.md)

::: moniker-end
