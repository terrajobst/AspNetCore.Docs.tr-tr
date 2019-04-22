---
title: ASP.NET Core MVC model doğrulama
author: tdykstra
description: Razor sayfaları ile ASP.NET Core MVC, model doğrulama hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: 1ae3c20478b02d6f654e65fdf34c88e1ffb837f8
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468743"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="1d4e2-103">Razor sayfaları ile ASP.NET Core MVC, model doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="1d4e2-104">Bu makalede, bir ASP.NET Core MVC veya Razor sayfaları uygulamada kullanıcı girişini doğrulama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="1d4e2-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="1d4e2-106">Model durumu</span><span class="sxs-lookup"><span data-stu-id="1d4e2-106">Model state</span></span>

<span data-ttu-id="1d4e2-107">Model durumu gösteren iki alt sistemlerin gelen hataları: model bağlama ve model doğrulama.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="1d4e2-108">Kaynaklanan hatalar [model bağlama](model-binding.md) genellikle veri dönüştürme hataları (örneğin, "x" girilen bir tamsayı bekliyor. bir alanda) olan.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="1d4e2-109">Model doğrulama gerçekleşir model bağlama ve raporları hataları sonra verileri nerede iş kuralları için uygun değil (örneğin, 0, 1 ile 5 arasında bir derecelendirme bekliyor alanındaki girilir).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="1d4e2-110">Model bağlama hem doğrulamayı bir denetleyici eylemi ya da bir Razor sayfaları işleyicisi yöntem yürütmeden önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="1d4e2-111">Web apps için bunu denetlemek için uygulamanın sorumluluğudur `ModelState.IsValid` ve uygun şekilde tepki verin.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="1d4e2-112">Web uygulamaları, genellikle sayfanın bir hata iletisi ile yeniden:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="1d4e2-113">Web API denetleyicisi, denetlenecek yok `ModelState.IsValid` oluşturulduysa `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="1d4e2-114">Bu durumda, bir otomatik HTTP 400 yanıt içeren sorun ayrıntıları döndürülür model durumu geçersiz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="1d4e2-115">Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="1d4e2-116">Doğrulama yeniden çalıştırın</span><span class="sxs-lookup"><span data-stu-id="1d4e2-116">Rerun validation</span></span>

<span data-ttu-id="1d4e2-117">Doğrulama işlemi otomatiktir ancak el ile yinelemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="1d4e2-118">Örneğin, bir özellik için bir değer hesaplamak ve hesaplanan değere özelliği ayarlandıktan sonra doğrulama yeniden çalıştırmak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="1d4e2-119">Doğrulama yeniden çalıştırmak için çağrı `TryValidateModel` burada gösterildiği gibi yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="1d4e2-120">Doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-120">Validation attributes</span></span>

<span data-ttu-id="1d4e2-121">Doğrulama özniteliklerinin model özelliklerini doğrulama kurallarını belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="1d4e2-122">Aşağıdaki örnekte [örnek uygulamasını](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) doğrulama özniteliklerle ek açıklama bir model sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-122">The following example from [the sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="1d4e2-123">`[ClassicMovie]` Diğer yerleşik ve özel doğrulama özniteliği bir özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="1d4e2-124">(Gösterilmemiştir olan `[ClassicMovie2]`, özel bir öznitelik uygulamak için alternatif bir yolu gösterir.)</span><span class="sxs-lookup"><span data-stu-id="1d4e2-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="1d4e2-125">Yerleşik öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-125">Built-in attributes</span></span>

<span data-ttu-id="1d4e2-126">Yerleşik doğrulama özniteliklerinin bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="1d4e2-127">`[CreditCard]`: Özelliği bir kredi kartı biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="1d4e2-128">`[Compare]`: Bu iki modeli eşleşme özelliklerinde doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="1d4e2-129">`[EmailAddress]`: Özelliği bir e-posta biçiminde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="1d4e2-130">`[Phone]`: Özelliği biçimlendirme bir telefon numarasını sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="1d4e2-131">`[Range]`: Özellik değeri belirtilen bir aralığa denk gelen olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="1d4e2-132">`[RegularExpression]`: Özellik değeri, belirtilen normal ifadeyle eşleştiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="1d4e2-133">`[Required]`: Alanın boş olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="1d4e2-134">Bkz: [[gerekli] özniteliği](#required-attribute) bu özniteliğin davranış hakkında ayrıntılı bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="1d4e2-135">`[StringLength]`: Bir dize özelliği değerinde belirtilen uzunluk sınırını aşmaması doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="1d4e2-136">`[Url]`: Özelliği bir URL biçimine sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="1d4e2-137">`[Remote]`: Sunucuda bir eylem yöntemini çağırarak giriş istemcide doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="1d4e2-138">Bkz: [[uzak] özniteliği](#remote-attribute) bu özniteliğin davranış hakkında ayrıntılı bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="1d4e2-139">Doğrulama özniteliklerinin tam listesini bulabilirsiniz [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="1d4e2-140">Hata iletileri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-140">Error messages</span></span>

<span data-ttu-id="1d4e2-141">Doğrulama öznitelikleri için geçersiz giriş görüntülenecek hata iletisi belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="1d4e2-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="1d4e2-143">Dahili olarak, öznitelikleri çağrı `String.Format` bazen ek yer tutucular ve alan adı için bir yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="1d4e2-144">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="1d4e2-145">Uygulandığında bir `Name` özelliği, önceki kod tarafından oluşturulan hata iletisi olacak "adı uzunluğu 6 ile 8 arasında olmalıdır.".</span><span class="sxs-lookup"><span data-stu-id="1d4e2-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="1d4e2-146">Hangi parametreler geçirilen bulmak için `String.Format` belirli bir özniteliğin hata iletisi için [DataAnnotations kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="1d4e2-147">[Gerekli] özniteliği</span><span class="sxs-lookup"><span data-stu-id="1d4e2-147">[Required] attribute</span></span>

<span data-ttu-id="1d4e2-148">Bunlar varmış gibi varsayılan olarak, doğrulama sistemi NULL olmayan parametreleri veya özellikleri değerlendirir bir `[Required]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="1d4e2-149">[Değer türleri](/dotnet/csharp/language-reference/keywords/value-types) gibi `decimal` ve `int` NULL olmayan atanabilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="1d4e2-150">Sunucuda [gerekli] doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-150">[Required] validation on the server</span></span>

<span data-ttu-id="1d4e2-151">Özellik null ise sunucu üzerinde gerekli değer eksik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="1d4e2-152">Her zaman NULL olmayan bir alan geçerli ve [gerekli] özniteliğin hata iletisi hiçbir zaman görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="1d4e2-153">Ancak, model bağlama için alamayan bir özellik gibi bir hata iletisi kaynaklanan başarısız olabilir `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="1d4e2-154">Sunucu tarafı doğrulama atanamayan tür için bir özel hata iletisi belirtmek için aşağıdaki seçenekleriniz vardır:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="1d4e2-155">Alanın boş değer atanabilir olun (örneğin, `decimal?` yerine `decimal`).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="1d4e2-156">[Boş değer atanabilir\<T >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri gibi standart boş değer atanabilir türler kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="1d4e2-157">Model bağlama tarafından kullanılan varsayılan hata iletisi, aşağıdaki örnekte gösterildiği gibi belirtin:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="1d4e2-158">Model bağlama hataları hakkında daha fazla bilgi varsayılan ayarlayabilirsiniz için iletileri için bkz: <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="1d4e2-159">İstemcide [gerekli] doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-159">[Required] validation on the client</span></span>

<span data-ttu-id="1d4e2-160">Olmayan boş değer atanabilir türler ile dizeler sunucuya karşılaştırıldığında istemcide farklı şekilde ele alınır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="1d4e2-161">İstemcide:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-161">On the client:</span></span>

* <span data-ttu-id="1d4e2-162">Bir değer girdi için yalnızca girilen mevcut olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="1d4e2-163">Bu nedenle, istemci tarafı doğrulama atanamayan türleri aynı boş değer atanabilir türler olarak işler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="1d4e2-164">JQuery doğrulaması dize alanı boş alanlarda geçerli giriş değerlendirilir [gerekli](https://jqueryvalidation.org/required-method/) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="1d4e2-165">Sunucu tarafı doğrulama yalnızca boşluk girilirse gerekli dize alanı geçersiz olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="1d4e2-166">Daha önce belirtildiği gibi sahip oldukları gibi sorgulamanıza atanamaz türler değerlendirilir bir `[Required]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="1d4e2-167">Size istemci tarafı doğrulama bile uygulamazsanız anlamına `[Required]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="1d4e2-168">Ancak öznitelik kullanmayın, varsayılan bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="1d4e2-169">Özel hata iletisi belirtmek için özniteliği kullanın.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="1d4e2-170">[Uzak] özniteliği</span><span class="sxs-lookup"><span data-stu-id="1d4e2-170">[Remote] attribute</span></span>

<span data-ttu-id="1d4e2-171">`[Remote]` Öznitelik alanı girişinin geçerli olup olmadığını belirlemek için sunucu üzerinde bir yöntemi çağırmak gerektiren bir istemci tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="1d4e2-172">Örneğin, bir kullanıcı adı zaten kullanımda olup olmadığını doğrulamak uygulamayı gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="1d4e2-173">Uzak doğrulama uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-173">To implement remote validation:</span></span>

1. <span data-ttu-id="1d4e2-174">Çağrılacak JavaScript için bir eylem yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="1d4e2-175">JQuery doğrulama [uzak](https://jqueryvalidation.org/remote-method/) yöntemi bir JSON yanıtı bekliyor:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="1d4e2-176">`"true"` Giriş verilerinin geçerli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="1d4e2-177">`"false"`, `undefined`, veya `null` giriş geçersiz olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="1d4e2-178">Varsayılan hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-178">Display the default error message.</span></span>
   * <span data-ttu-id="1d4e2-179">Herhangi bir dize girişi geçersiz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="1d4e2-180">Dize bir özel hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="1d4e2-181">Özel hata iletisi döndüren bir eylem yöntemi bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="1d4e2-182">Model sınıfında, ek açıklama özelliğiyle bir `[Remote]` doğrulama eylem yöntemine, aşağıdaki örnekte gösterildiği gibi işaret eden bir öznitelik:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a><span data-ttu-id="1d4e2-183">Ek alanlar</span><span class="sxs-lookup"><span data-stu-id="1d4e2-183">Additional fields</span></span>

<span data-ttu-id="1d4e2-184">`AdditionalFields` Özelliği `[Remote]` özniteliği karşı sunucuda veri alanlarının kombinasyonları doğrulamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-184">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="1d4e2-185">Örneğin, varsa `User` modeli olan `FirstName` ve `LastName` özellikleri, mevcut hiçbir kullanıcı adları bu çifti zaten sahip olduğunuzu doğrulayın isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-185">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="1d4e2-186">Aşağıdaki örnek nasıl kullanılacağını gösterir `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-186">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="1d4e2-187">`AdditionalFields` dizeler için açıkça ayarlanmış `"FirstName"` ve `"LastName"`, ancak kullanarak [ `nameof` ](/dotnet/csharp/language-reference/keywords/nameof) işleci, daha sonra yeniden düzenleme basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-187">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="1d4e2-188">Bu doğrulama için eylem yöntemi, hem ad hem de son adı bağımsız değişkenleri kabul etmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-188">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="1d4e2-189">Kullanıcı ilk veya son adı girdiğinde, JavaScript adları bu çift geçen görmek için bir uzak çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-189">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="1d4e2-190">İki veya daha fazla ek alanlar doğrulamak için virgülle ayrılmış bir liste olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-190">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="1d4e2-191">Örneğin, eklemek için bir `MiddleName` model özelliğinin ayarlandığı `[Remote]` özniteliği aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-191">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="1d4e2-192">`AdditionalFields`, tüm öznitelik bağımsız değişkenleri gibi bir sabit ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-192">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="1d4e2-193">Bu nedenle, kullanmadığınız bir [ilişkilendirilmiş bir dizedir](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> başlatmak için `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-193">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="1d4e2-194">Yerleşik özniteliklerini alternatifleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-194">Alternatives to built-in attributes</span></span>

<span data-ttu-id="1d4e2-195">Yerleşik öznitelikleri tarafından sağlanmayan doğrulama gerekiyorsa, şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-195">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="1d4e2-196">[Özel öznitelikler oluşturma](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-196">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="1d4e2-197">[IValidatableObject uygulamak](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-197">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="1d4e2-198">Özel öznitelikler</span><span class="sxs-lookup"><span data-stu-id="1d4e2-198">Custom attributes</span></span>

<span data-ttu-id="1d4e2-199">Yerleşik doğrulama özniteliklerinin işlemez senaryoları için özel doğrulama öznitelikleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-199">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="1d4e2-200">Devralınan bir sınıf oluşturmanız <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>ve geçersiz kılma <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-200">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="1d4e2-201">`IsValid` Yöntemi kabul adlı bir nesne *değer*, doğrulanacak giriş olduğu.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-201">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="1d4e2-202">Bir aşırı yüklemeyi de kabul eden bir `ValidationContext` model bağlama tarafından oluşturulan model örneği gibi ek bilgileri sağlayan nesne.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-202">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="1d4e2-203">Aşağıdaki örnek, yayın tarihi için içinde bir film doğrular *Klasik* Tarz belirtilen bir yıldan daha yeni değil.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-203">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="1d4e2-204">`[ClassicMovie2]` Özniteliği Tarz ilk denetler ve yalnızca olup olmadığını devam *Klasik*.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-204">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="1d4e2-205">Klasikler tanımlanan film için daha sonraki bir öznitelik oluşturucuya geçirilen sınırı olmadığından emin olmak için yayın tarihi denetler.)</span><span class="sxs-lookup"><span data-stu-id="1d4e2-205">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="1d4e2-206">`movie` Önceki örnekte değişken temsil eden bir `Movie` form gönderme verilerini içeren nesne.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-206">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="1d4e2-207">`IsValid` Yöntemi, tarih ve Tarz denetler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-207">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="1d4e2-208">Doğrulama başarılı bağlı `IsValid` döndürür bir `ValidationResult.Success` kod.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-208">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="1d4e2-209">Doğrulama başarısız olduğunda bir `ValidationResult` ile ilgili bir hata iletisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-209">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="1d4e2-210">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="1d4e2-210">IValidatableObject</span></span>

<span data-ttu-id="1d4e2-211">Yukarıdaki örnekte, yalnızca çalışır `Movie` türleri.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-211">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="1d4e2-212">Sınıf düzeyindeki doğrulama için başka bir seçenek uygulamaktır `IValidatableObject` model sınıfında, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-212">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="1d4e2-213">Üst düzey düğüm doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-213">Top-level node validation</span></span>

<span data-ttu-id="1d4e2-214">Üst düzey düğümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-214">Top-level nodes include:</span></span>

* <span data-ttu-id="1d4e2-215">Eylem parametreleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-215">Action parameters</span></span>
* <span data-ttu-id="1d4e2-216">Denetleyicisi Özellikleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-216">Controller properties</span></span>
* <span data-ttu-id="1d4e2-217">Sayfa işleyici parametreleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-217">Page handler parameters</span></span>
* <span data-ttu-id="1d4e2-218">Sayfa modeli özellikleri</span><span class="sxs-lookup"><span data-stu-id="1d4e2-218">Page model properties</span></span>

<span data-ttu-id="1d4e2-219">Model bağlama en üst düzey düğümlerin yanı sıra model özelliklerini doğrulama doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-219">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="1d4e2-220">Aşağıdaki örnekte, örnek uygulamadan `VerifyPhone` yöntemi kullanan <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> doğrulamak için `phone` eylem parametresi:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-220">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="1d4e2-221">En üst düzey düğümlerini kullanabileceğiniz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> doğrulama özniteliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-221">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="1d4e2-222">Aşağıdaki örnekte, örnek uygulamadan `CheckAge` yöntemini belirtir `age` parametre gerekir bağlı Sorgu dizesinden bir form gönderildiğinde:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-222">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="1d4e2-223">Yaş kontrol edin sayfasında (*CheckAge.cshtml*), iki biçimi vardır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-223">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="1d4e2-224">İlk formu gönderdiği bir `Age` değerini `99` sorgu dizesi olarak: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-224">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="1d4e2-225">Düzgün şekilde biçimlendirilmiş olduğunda `age` sorgu dizesi parametresi gönderildi, formun doğrular.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-225">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="1d4e2-226">Yaş kontrol edin sayfasında ikinci formu gönderdiği `Age` doğrulama başarısız olur ve istek gövdesinde bir değer.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-226">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="1d4e2-227">Başarısız olduğundan bağlama `age` parametresi, bir sorgu dizesinden gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-227">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="1d4e2-228">İle çalıştırırken `CompatibilityVersion.Version_2_1` ya da daha üst düzey düğümü doğrulaması varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-228">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="1d4e2-229">Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-229">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="1d4e2-230">Varsayılan seçenek ayarlayarak geçersiz kılınabilir <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliğinde (`Startup.ConfigureServices`), burada gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-230">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="1d4e2-231">En yüksek hata sayısı</span><span class="sxs-lookup"><span data-stu-id="1d4e2-231">Maximum errors</span></span>

<span data-ttu-id="1d4e2-232">Doğrulama hataları sayısı (varsayılan olarak, 200) ulaşıldığında durur.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-232">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="1d4e2-233">Bu sayı aşağıdaki kod ile yapılandırabileceğiniz `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-233">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="1d4e2-234">Özyineleme sayısı üst sınırı</span><span class="sxs-lookup"><span data-stu-id="1d4e2-234">Maximum recursion</span></span>

<span data-ttu-id="1d4e2-235"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> Doğrulanmakta olan model Nesne grafiği erişir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-235"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="1d4e2-236">Çok derin veya sonsuz yinelemelidir modelleri için doğrulama, yığın taşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-236">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="1d4e2-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) ziyaretçi özyineleme yapılandırılan derinlik aşarsa erken doğrulama durdurmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="1d4e2-238">Varsayılan değer olan `MvcOptions.MaxValidationDepth` 200 ile çalışırken olduğu `CompatibilityVersion.Version_2_2` veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-238">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="1d4e2-239">Önceki sürümleri için derinliği kısıtlama yok anlamına gelir, değeri null.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-239">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="1d4e2-240">Otomatik kısa devre oluşturur</span><span class="sxs-lookup"><span data-stu-id="1d4e2-240">Automatic short-circuit</span></span>

<span data-ttu-id="1d4e2-241">Model graf doğrulama gerektirmiyorsa doğrulama otomatik olarak (atlandı) kısa devre yapılma.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="1d4e2-242">Çalışma zamanı için doğrulamayı atlar nesneler temel elemanlar koleksiyonları içerir (gibi `byte[]`, `string[]`, `Dictionary<string, string>`) ve tüm doğrulayıcıları yoksa karmaşık nesne grafikler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="1d4e2-243">Doğrulama devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="1d4e2-243">Disable validation</span></span>

<span data-ttu-id="1d4e2-244">Doğrulama devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-244">To disable validation:</span></span>

1. <span data-ttu-id="1d4e2-245">Bir uygulamasını oluşturun `IObjectModelValidator` alanları geçersiz olarak işaretlemiyor.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="1d4e2-246">Aşağıdaki kodu ekleyin `Startup.ConfigureServices` varsayılan değiştirilecek `IObjectModelValidator` bağımlılık ekleme kapsayıcısını uygulamasında.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="1d4e2-247">Model bağlamanın dışında kaynaklanan model durumu hatalarının yine de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="1d4e2-248">İstemci tarafı doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-248">Client-side validation</span></span>

<span data-ttu-id="1d4e2-249">Formun geçerli olana kadar istemci tarafı doğrulama gönderimi engeller.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="1d4e2-250">Gönder düğmesine formu gönderdiği veya hata iletilerini görüntüler JavaScript çalışır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="1d4e2-251">Bir form üzerinde giriş hataları olduğunda istemci tarafı doğrulama sunucusuna gereksiz bir gidiş dönüş önler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="1d4e2-252">Aşağıdaki betiği başvurularının *_Layout.cshtml* ve *_ValidationScriptsPartial.cshtml* istemci tarafı doğrulama desteği:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="1d4e2-253">[JQuery örtük doğrulaması](https://github.com/aspnet/jquery-validation-unobtrusive) betiğidir popüler üzerinde oluşturan özel bir ön uç Microsoft Kitaplığı [jQuery doğrulama](https://jqueryvalidation.org/) eklentisi.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="1d4e2-254">JQuery örtük doğrulaması aynı doğrulama mantığı iki yerde kod gerekirdi: model özellikleri, sunucu tarafı doğrulama öznitelikleri içinde bir kez ve ardından tekrar istemci tarafı betikler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="1d4e2-255">Bunun yerine, [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) doğrulama öznitelikleri kullanın ve türü meta verileri HTML 5 işlemek için model özelliklerinden `data-` doğrulama gerekli form öğeleri için öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="1d4e2-256">jQuery doğrulama örtük ayrıştırır `data-` öznitelikleri ve jQuery doğrulama, etkili bir şekilde "sunucu tarafı doğrulama mantığını istemciye kopyalama" mantığı geçirir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="1d4e2-257">Etiket Yardımcıları burada gösterildiği gibi kullanarak istemci üzerinde doğrulama hataları görüntüleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="1d4e2-258">Önceki etiket Yardımcıları aşağıdaki HTML'yi işleyin.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-258">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="1d4e2-259">Dikkat `data-` HTML özniteliklerinde çıkış için doğrulama öznitelikleri karşılık `ReleaseDate` özelliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="1d4e2-260">`data-val-required` Özniteliği, kullanıcı Yayın Tarihi alanını doldurmazsa görüntülenecek hata iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="1d4e2-261">jQuery doğrulaması örtük jQuery doğrulama için bu değer geçirir [ `required()` ](https://jqueryvalidation.org/required-method/) sonra bu iletiyi eşlik yöntemini  **\<span >** öğesi.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="1d4e2-262">Veri türü doğrulama, tarafından geçersiz kılınmadığı sürece bir özelliği .NET türüne dayalı bir `[DataType]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="1d4e2-263">Kendi varsayılan hata iletileri tarayıcılar var ancak bu iletileri jQuery doğrulama örtük doğrulama paketi geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="1d4e2-264">`[DataType]` öznitelikleri ve sınıfları gibi `[EmailAddress]` hata iletisi belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="1d4e2-265">Doğrulama için dinamik formlar ekleyin</span><span class="sxs-lookup"><span data-stu-id="1d4e2-265">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="1d4e2-266">Sayfa ilk yüklendiğinde jQuery örtük doğrulaması jQuery doğrulama Doğrulama mantığı ve parametrelerini geçirir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-266">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="1d4e2-267">Bu nedenle, doğrulama otomatik olarak dinamik olarak üretilen formlar üzerinde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-267">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="1d4e2-268">Doğrulamayı etkinleştirmek için jQuery oluşturduktan hemen sonra dinamik biçimi ayrıştırmak için örtük doğrulaması söyleyin.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-268">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="1d4e2-269">Örneğin, aşağıdaki kod AJAX eklenen bir form üzerinde istemci tarafı doğrulama ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-269">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="1d4e2-270">`$.validator.unobtrusive.parse()` Yöntemi jQuery Seçicisi kendi bir bağımsız değişkeni kabul eder.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-270">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="1d4e2-271">Bu yöntem jQuery ayrıştırmak için örtük doğrulaması söyler `data-` formları içinde seçicinin öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-271">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="1d4e2-272">Bu öznitelik değerleri, ardından için jQuery doğrulama eklentisi geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-272">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="1d4e2-273">Dinamik denetimlere doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="1d4e2-273">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="1d4e2-274">`$.validator.unobtrusive.parse()` Gibi çalışır tek dinamik olarak üretilen denetimleri değil, tüm bir form üzerinde yöntemi `<input>` ve `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-274">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="1d4e2-275">Yeniden ayrıştırma form için form daha önce ayrıştırıldığında aşağıdaki örnekte gösterildiği gibi eklendikten sonra doğrulama verileri kaldırın:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-275">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="1d4e2-276">Özel istemci tarafı doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d4e2-276">Custom client-side validation</span></span>

<span data-ttu-id="1d4e2-277">Özel istemci tarafı doğrulama oluşturarak yapılır `data-` özel jQuery doğrulama bağdaştırıcısı birlikte çalışan HTML öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-277">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="1d4e2-278">Aşağıdaki örnek bağdaştırıcısı kod için yazılmıştır `ClassicMovie` ve `ClassicMovie2` bu makalenin önceki bölümlerinde sunulan öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-278">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="1d4e2-279">Bağdaştırıcıları yazma hakkında daha fazla bilgi için bkz: [jQuery doğrulama belgeleri](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-279">For information about how to write adapters, see the [jQuery Validate documentation](http://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="1d4e2-280">Belirli bir alan için bir bağdaştırıcı kullanımını tarafından tetiklenen `data-` öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-280">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="1d4e2-281">Alan doğrulama işlemine tabi olarak bayrak (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-281">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="1d4e2-282">Doğrulama kuralı adı ve hata ileti metnini belirleyin (örneğin, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-282">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="1d4e2-283">Ek parametreler Doğrulayıcı sağlamak gerekir (örneğin, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-283">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="1d4e2-284">Aşağıdaki örnekte gösterildiği `data-` örnek uygulamanın özniteliklerini `ClassicMovie` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-284">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="1d4e2-285">Daha önce belirtildiği gibi [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) işlemek için doğrulama öznitelikleri bilgileri kullanmak `data-` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-285">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="1d4e2-286">Özel oluşturulmasında Sonuç kodu yazmak için iki seçenek `data-` HTML öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-286">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="1d4e2-287">Türetilen bir sınıf oluşturmanız `AttributeAdapterBase<TAttribute>` ve uygulayan bir sınıf `IValidationAttributeAdapterProvider`ve, öznitelik ve onun bağdaştırıcısı DI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-287">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="1d4e2-288">Bu yöntemi izleyen [tek sorumluluk sorumlusu](https://wikipedia.org/wiki/Single_responsibility_principle) ayrı sınıflarda sunucu ve istemci ilgili doğrulama kodu olması.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-288">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="1d4e2-289">Bağdaştırıcıyı ayrıca DI kaydedildiğinden, diğer hizmetlerde DI gerekirse hazır olduğunu avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-289">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="1d4e2-290">Uygulama `IClientModelValidator` içinde `ValidationAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-290">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="1d4e2-291">Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulama yapmaz ve her DI hizmetlerinden ihtiyaç duymadığı uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-291">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="1d4e2-292">İstemci tarafı doğrulama için AttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="1d4e2-292">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="1d4e2-293">İşleme Bu yöntem `data-` HTML öznitelikleri tarafından kullanılan `ClassicMovie` örnek uygulamada özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-293">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="1d4e2-294">Bu yöntemi kullanarak istemci doğrulama eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-294">To add client validation by using this method:</span></span>

1. <span data-ttu-id="1d4e2-295">Özel doğrulama özniteliği için öznitelik bağdaştırıcı sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-295">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="1d4e2-296">Türetin [AttributeAdapterBase\<T >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="1d4e2-296">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="1d4e2-297">Oluşturma bir `AddValidation` ekler yöntemi `data-` işlenen çıkışı için bu örnekte gösterildiği gibi öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-297">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="1d4e2-298">Uygulayan bir bağdaştırıcı sağlayıcı sınıfı oluşturma <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-298">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="1d4e2-299">İçinde `GetAttributeAdapter` yöntemi bu örnekte gösterildiği gibi bu özel öznitelik bağdaştırıcının yapıcısına geçirin:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-299">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="1d4e2-300">DI içinde için bağdaştırıcı sağlayıcısını kaydetme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-300">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="1d4e2-301">İstemci tarafı doğrulama için IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="1d4e2-301">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="1d4e2-302">İşleme Bu yöntem `data-` HTML öznitelikleri tarafından kullanılan `ClassicMovie2` örnek uygulamada özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-302">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="1d4e2-303">Bu yöntemi kullanarak istemci doğrulama eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-303">To add client validation by using this method:</span></span>

* <span data-ttu-id="1d4e2-304">Özel doğrulama özniteliği uygulamak `IClientModelValidator` arabirim ve oluşturma bir `AddValidation` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-304">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="1d4e2-305">İçinde `AddValidation` yöntemi ekleme `data-` doğrulama için aşağıdaki örnekte gösterildiği gibi öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-305">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="1d4e2-306">İstemci tarafı doğrulama devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="1d4e2-306">Disable client-side validation</span></span>

<span data-ttu-id="1d4e2-307">Aşağıdaki kod, istemci doğrulama MVC görünümleri devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="1d4e2-307">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="1d4e2-308">Bu MVC görünümleri, Razor sayfaları içinde değil, yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-308">This works only in MVC views, not in Razor Pages.</span></span> <span data-ttu-id="1d4e2-309">İstemci doğrulamasını devre dışı bırakmak için başka bir başvuru açıklama seçenektir `_ValidationScriptsPartial` içinde *.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="1d4e2-309">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d4e2-310">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1d4e2-310">Additional resources</span></span>

* [<span data-ttu-id="1d4e2-311">System.ComponentModel.DataAnnotations ad alanı</span><span class="sxs-lookup"><span data-stu-id="1d4e2-311">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="1d4e2-312">Model Bağlamaları</span><span class="sxs-lookup"><span data-stu-id="1d4e2-312">Model Binding</span></span>](model-binding.md)
