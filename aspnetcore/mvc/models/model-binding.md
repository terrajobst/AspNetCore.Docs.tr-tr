---
title: ASP.NET core'da model bağlama
author: tdykstra
description: ASP.NET Core model bağlamanın nasıl çalıştığı ve davranışını özelleştirmek nasıl öğrenin.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 7d62ccecdacbd34a38a1fd8c58979a9b09cf86e8
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750207"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="7e6d6-103">ASP.NET core'da model bağlama</span><span class="sxs-lookup"><span data-stu-id="7e6d6-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="7e6d6-104">Bu makalede, hangi model bağlama, nasıl çalıştığını ve davranışını özelleştirmek nasıl açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="7e6d6-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="7e6d6-106">Model bağlama nedir</span><span class="sxs-lookup"><span data-stu-id="7e6d6-106">What is Model binding</span></span>

<span data-ttu-id="7e6d6-107">Denetleyicileri ve Razor sayfaları HTTP isteklerinden gelen verilerle çalışır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="7e6d6-108">Örneğin, bir kayıt anahtarı rota verilerini sağlayabilir ve gönderilen form alanlarını, modelin özellikleri için değer sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="7e6d6-109">Bu değerlerin her birini alıp bunları .NET türleri için dizeleri dönüştürmek için kod yazma, sıkıcı ve hataya açık olacaktır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="7e6d6-110">Model bağlama bu işlemini otomatikleştirir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-110">Model binding automates this process.</span></span> <span data-ttu-id="7e6d6-111">Model bağlama sistemi:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-111">The model binding system:</span></span>

* <span data-ttu-id="7e6d6-112">Rota verileri, form alanlarını ve sorgu dizeleri gibi çeşitli kaynaklardan gelen verileri alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="7e6d6-113">Veri denetleyicilerine ve Razor sayfalarında yöntem parametreleri ve ortak özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="7e6d6-114">.NET türlerine verileri dönüştürür dize.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="7e6d6-115">Karmaşık türler özelliklerini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="7e6d6-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="7e6d6-116">Example</span></span>

<span data-ttu-id="7e6d6-117">Aşağıdaki bir eylem yönteminin olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="7e6d6-118">Ve uygulama bu URL'yi bir istekle alır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="7e6d6-119">Eylem yönteminin yönlendirme sistem seçtikten sonra model bağlama aşağıdaki adımları yine de geçer:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-119">Model binding goes though the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="7e6d6-120">İlk parametresi bulur `GetByID`, adlandırılmış tamsayı `id`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="7e6d6-121">HTTP isteği kullanılabilir kaynakları arar ve bulduğu `id` "2" rota verilerindeki =.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="7e6d6-122">"2" dizesi 2 değeri tamsayıya dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="7e6d6-123">Sonraki parametresi bulur `GetByID`, adlı bir Boole değeri `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="7e6d6-124">Kaynakları aracılığıyla arar ve bulduğu "DogsOnly = true" sorgu dizesi içinde.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="7e6d6-125">Ad eşleştirme büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="7e6d6-126">"True" dizesi, Boole olarak dönüştürür `true`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="7e6d6-127">Framework sonra çağıran `GetById` yöntemi için 2'deki geçirme `id` parametresi ve `true` için `dogsOnly` parametresi.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="7e6d6-128">Önceki örnekte, model bağlama hedefleri basit türler yöntem parametreleri ' dir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="7e6d6-129">Hedefleri, bir karmaşık tür özelliklerini de olabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="7e6d6-130">Her bir özellik başarıyla bağlandıktan sonra [model doğrulama](xref:mvc/models/validation) bu özellik için gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="7e6d6-131">Hangi veri modeli ve bağlama veya doğrulama hataları, bağımlı kaydı depolanan [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="7e6d6-132">Bu işlem başarılı olduysa, uygulamanın denetler öğrenmek [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağı.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="7e6d6-133">Hedefler</span><span class="sxs-lookup"><span data-stu-id="7e6d6-133">Targets</span></span>

<span data-ttu-id="7e6d6-134">Model bağlama hedefleri aşağıdaki tür için değerleri bulmak çalışır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="7e6d6-135">Bir isteği yönlendirilir denetleyici eylem yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="7e6d6-136">Bir isteği yönlendirilir Razor sayfaları işleyici yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="7e6d6-137">Etki alanı denetleyicisinin genel özellikleri veya `PageModel` öznitelikleri tarafından belirtilen sınıf.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="7e6d6-138">[BindProperty] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-138">[BindProperty] attribute</span></span>

<span data-ttu-id="7e6d6-139">Bir denetleyici için bir ortak özellik uygulanabilir veya `PageModel` neden bu özellik hedeflemek, model bağlama sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="7e6d6-140">[BindProperties] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-140">[BindProperties] attribute</span></span>

<span data-ttu-id="7e6d6-141">ASP.NET Core 2.1 ve sonraki sürümlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="7e6d6-142">Bir denetleyiciye uygulanabilir veya `PageModel` sınıfın tüm ortak özellikleri hedeflemek için model bağlama bildirmek için sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="7e6d6-143">Model bağlama HTTP GET istekleri için</span><span class="sxs-lookup"><span data-stu-id="7e6d6-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="7e6d6-144">Varsayılan olarak, özellikler için HTTP GET isteklerini bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="7e6d6-145">Genellikle, bir GET isteği için ihtiyacınız olan bir kayıt kimliği parametresi.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="7e6d6-146">Kayıt kimliği, veritabanında öğeyi aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="7e6d6-147">Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamak için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="7e6d6-148">İstediğiniz özellikler GET isteklerinden verilere bağlı senaryolarda ayarlamak `SupportsGet` özelliğini `true`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="7e6d6-149">Kaynakları</span><span class="sxs-lookup"><span data-stu-id="7e6d6-149">Sources</span></span>

<span data-ttu-id="7e6d6-150">Varsayılan olarak, model bağlama bir HTTP isteğindeki aşağıdaki kaynaklardan veri anahtar-değer çiftleri biçiminde alır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="7e6d6-151">Form alanlarını</span><span class="sxs-lookup"><span data-stu-id="7e6d6-151">Form fields</span></span> 
1. <span data-ttu-id="7e6d6-152">İstek gövdesi (için [[ApiController] özniteliği olan denetleyicileri](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="7e6d6-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="7e6d6-153">Rota verileri</span><span class="sxs-lookup"><span data-stu-id="7e6d6-153">Route data</span></span>
1. <span data-ttu-id="7e6d6-154">Sorgu dizesi parametreleri</span><span class="sxs-lookup"><span data-stu-id="7e6d6-154">Query string parameters</span></span>
1. <span data-ttu-id="7e6d6-155">Karşıya yüklenen dosyalar</span><span class="sxs-lookup"><span data-stu-id="7e6d6-155">Uploaded files</span></span> 

<span data-ttu-id="7e6d6-156">Her hedef parametresi veya özelliği için kaynakları bu listede gösterilen sırayla taranır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="7e6d6-157">Birkaç özel durum vardır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-157">There are a few exceptions:</span></span>

* <span data-ttu-id="7e6d6-158">Veri ve sorgu dize değerleri yalnızca basit türleri için kullanılan yol.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="7e6d6-159">Karşıya yüklenen dosyalar uygulayan hedef türlerine bağlı `IFormFile` veya `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="7e6d6-160">Varsayılan davranışı doğru sonuçları alamazsanız, herhangi belirli bir hedef için kullanılacak kaynağı belirlemek için aşağıdaki özniteliklerden birini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="7e6d6-161">[[FromQuery] ](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -Sorgu dizesi değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="7e6d6-162">[[FromRoute] ](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -Değerler rota verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="7e6d6-163">[[FromForm] ](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -Gönderilen form alanının değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="7e6d6-164">[[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -Gövdeden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="7e6d6-165">[[FromHeader] ](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -Değerleri HTTP üst bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="7e6d6-166">Bu öznitelikler:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-166">These attributes:</span></span>

* <span data-ttu-id="7e6d6-167">Model özellikleri ayrı ayrı eklenir (değil için model sınıfı) aşağıdaki örnekte olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="7e6d6-168">İsteğe bağlı olarak bir oluşturucuda model adı değeri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="7e6d6-169">Özellik adı, istekteki değerle eşleşmeyen durumunda bu seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="7e6d6-170">Örneğin, istek değeri adında, aşağıdaki örnekte olduğu gibi bir tire sahip bir üstbilgi olabilir:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="7e6d6-171">[FromBody] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-171">[FromBody] attribute</span></span>

<span data-ttu-id="7e6d6-172">İstek gövdesi veri giriş biçimlendiricileri isteğin içerik türüyle belirli kullanılarak ayrıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="7e6d6-173">Giriş biçimlendiricileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="7e6d6-174">Uygulama `[FromBody]` eylem yönteminin başına birden fazla parametre.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="7e6d6-175">ASP.NET Core çalışma zamanı giriş biçimlendirici için istek akışı okuma sorumluluğunu atar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="7e6d6-176">İstek akışı okunduktan sonra artık diğer bağlama için yeniden okumak için hazır değil `[FromBody]` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="7e6d6-177">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7e6d6-177">Additional sources</span></span>

<span data-ttu-id="7e6d6-178">Kaynak veri modeline bağlama sistem tarafından sağlanan *değer sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="7e6d6-179">Yazma ve diğer kaynaklardan model bağlama için veri alma özel değer sağlayıcılarını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="7e6d6-180">Örneğin, tanımlama bilgisi veya oturum durumu verileri isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="7e6d6-181">Yeni bir kaynaktan veri almak için:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-181">To get data from a new source:</span></span>

* <span data-ttu-id="7e6d6-182">Uygulayan bir sınıf oluşturma `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="7e6d6-183">Uygulayan bir sınıf oluşturma `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="7e6d6-184">Fabrika sınıfında kaydetme `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="7e6d6-185">Örnek uygulamayı içeren bir [değer sağlayıcısındaki](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) ve [Fabrika](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) değerlerini tanımlama bilgilerini alır. örnek.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="7e6d6-186">Kayıt kodu işte `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="7e6d6-187">Özel değer sağlayıcı, gösterilen kod yerleşik değer sağlayıcıları tüm koyar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="7e6d6-188">Bunu yapmak için ilk çağrı listesinde `Insert(0, new CookieValueProviderFactory())` yerine `Add`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="7e6d6-189">Model bir özellik için bir kaynağı yok</span><span class="sxs-lookup"><span data-stu-id="7e6d6-189">No source for a model property</span></span>

<span data-ttu-id="7e6d6-190">Hiçbir değer için bir model özelliğine bulunması durumunda varsayılan olarak, model durumu hatası oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="7e6d6-191">Özelliği null veya varsayılan değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="7e6d6-192">Boş değer atanabilir basit türler ayarlandığında `null`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="7e6d6-193">Atanamayan değer türleri ayarlanmış `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="7e6d6-194">Örneğin, bir parametre `int id` 0 olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="7e6d6-195">Karmaşık türler için model bağlama özellikleri ayarlamadan varsayılan Oluşturucusu kullanarak örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="7e6d6-196">Diziler ayarlanmış `Array.Empty<T>()`dışında `byte[]` diziler ayarlandığında `null`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="7e6d6-197">Hiçbir şey için bir model özelliğine form alanlarını bulunduğunda, model durumu geçersiz, kullanın [[BindRequired] özniteliği](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="7e6d6-198">Not Bu `[BindRequired]` gönderilen formu verilerden model bağlama için değil, bir istek gövdesi JSON veya XML verilerinde davranış uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="7e6d6-199">İstek gövdesi verileri tarafından işlenir [giriş biçimlendiricileri](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="7e6d6-200">Tür dönüştürme hataları</span><span class="sxs-lookup"><span data-stu-id="7e6d6-200">Type conversion errors</span></span>

<span data-ttu-id="7e6d6-201">Bir kaynak bulunamadı, ancak hedef türüne dönüştürülemiyor, model durumu geçersiz olarak işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="7e6d6-202">Hedef parametre ya da özelliği null veya varsayılan değer, önceki bölümde belirtildiği gibi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="7e6d6-203">Sahip bir API denetleyicisi, `[ApiController]` özniteliği, otomatik bir HTTP 400 yanıt geçersiz model durumu sonuçları.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="7e6d6-204">Bir Razor sayfası bir hata iletisiyle sayfasını yeniden görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="7e6d6-205">İstemci tarafı doğrulama Razor sayfaları forma gönderilmesi çoğu bozuk veri yakalar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="7e6d6-206">Bu doğrulama önceki vurgulanmış kodu tetiklemek zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="7e6d6-207">Örnek uygulamayı içeren bir **geçersiz tarih ile Gönder** bozuk veri koyar, bir düğme **işe giriş tarihi** alan ve formu gönderir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="7e6d6-208">Bu düğme, veri dönüştürme hataları meydana geldiğinde sayfayı yeniden görüntülemek için kodun nasıl çalıştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="7e6d6-209">Önceki kodun sayfası görüntülendiğinde, form alanı geçersiz giriş gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="7e6d6-210">Model özelliği null veya varsayılan değere ayarlanmış olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="7e6d6-211">Geçersiz giriş bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="7e6d6-212">Ancak, form alanı bozuk verileri yeniden görüntülemek istiyorsanız, model özelliğine bir dize yapma ve veri dönüştürme el ile yapılması göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="7e6d6-213">Tür dönüştürme hatalarının model durumuna hatalara neden istemiyorsanız aynı stratejisi önerilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="7e6d6-214">Bu durumda, model özelliğine bir dize olun.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="7e6d6-215">Basit türler</span><span class="sxs-lookup"><span data-stu-id="7e6d6-215">Simple types</span></span>

<span data-ttu-id="7e6d6-216">Model bağlayıcı, kaynak dizeleri dönüştürebilir basit türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="7e6d6-217">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="7e6d6-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="7e6d6-218">[Bayt](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="7e6d6-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="7e6d6-219">Char</span><span class="sxs-lookup"><span data-stu-id="7e6d6-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="7e6d6-220">Tarih/saat</span><span class="sxs-lookup"><span data-stu-id="7e6d6-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="7e6d6-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="7e6d6-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="7e6d6-222">Ondalık</span><span class="sxs-lookup"><span data-stu-id="7e6d6-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="7e6d6-223">çift</span><span class="sxs-lookup"><span data-stu-id="7e6d6-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="7e6d6-224">Sabit listesi</span><span class="sxs-lookup"><span data-stu-id="7e6d6-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="7e6d6-225">GUID</span><span class="sxs-lookup"><span data-stu-id="7e6d6-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="7e6d6-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="7e6d6-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="7e6d6-227">Tek</span><span class="sxs-lookup"><span data-stu-id="7e6d6-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="7e6d6-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="7e6d6-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="7e6d6-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="7e6d6-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="7e6d6-230">URI</span><span class="sxs-lookup"><span data-stu-id="7e6d6-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="7e6d6-231">Sürüm</span><span class="sxs-lookup"><span data-stu-id="7e6d6-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="7e6d6-232">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="7e6d6-232">Complex types</span></span>

<span data-ttu-id="7e6d6-233">Karmaşık bir tür, genel bir varsayılan oluşturucu ve bağlamak için genel yazılabilir özellikleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="7e6d6-234">Model bağlama oluştuğunda genel varsayılan oluşturucu kullanılarak sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="7e6d6-235">Model bağlama için her bir özellik karmaşık türün adı deseni için kaynakları arar *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="7e6d6-236">Hiçbir şey bulunursa, yalnızca görünüyor *property_name* öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="7e6d6-237">Bağlama için bir parametre, parametre adı önekidir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="7e6d6-238">Bağlama için bir `PageModel` ortak özelliği, ön eki olan genel özellik adı.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="7e6d6-239">Bazı özniteliklere sahip bir `Prefix` olanak tanıyan özellik parametresi veya özellik adı varsayılan kullanımını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="7e6d6-240">Örneğin, karmaşık tür aşağıdaki olduğunu varsayın `Instructor` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="7e6d6-241">Ön ek parametre adı =</span><span class="sxs-lookup"><span data-stu-id="7e6d6-241">Prefix = parameter name</span></span>

<span data-ttu-id="7e6d6-242">Adlı bir parametre modelin bağlı olup olmadığını `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="7e6d6-243">Model bağlama başlatır kaynakları anahtarı için bakarak `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="7e6d6-244">Bulunamazsa, arar `ID` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="7e6d6-245">Ön ek özellik adı =</span><span class="sxs-lookup"><span data-stu-id="7e6d6-245">Prefix = property name</span></span>

<span data-ttu-id="7e6d6-246">Bağlanılacak modeli adlı bir özellik olup olmadığını `Instructor` denetleyicisinin veya `PageModel` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="7e6d6-247">Model bağlama başlatır kaynakları anahtarı için bakarak `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="7e6d6-248">Bulunamazsa, arar `ID` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="7e6d6-249">Özel ön eki</span><span class="sxs-lookup"><span data-stu-id="7e6d6-249">Custom prefix</span></span>

<span data-ttu-id="7e6d6-250">Adlı bir parametre modelin bağlı olup olmadığını `instructorToUpdate` ve `Bind` özniteliği belirtir `Instructor` ön ek olarak:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="7e6d6-251">Model bağlama başlatır kaynakları anahtarı için bakarak `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="7e6d6-252">Bulunamazsa, arar `ID` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="7e6d6-253">Karmaşık tür hedefleri için öznitelikler</span><span class="sxs-lookup"><span data-stu-id="7e6d6-253">Attributes for complex type targets</span></span>

<span data-ttu-id="7e6d6-254">Birkaç yerleşik öznitelikler, karmaşık türler, model bağlama denetlemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="7e6d6-255">Bu öznitelikler, model etkileyen değerleri kaynağına gönderilen veri formu bağlamayı.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="7e6d6-256">İstek gövdesi JSON ve XML işlemi gönderilen giriş biçimlendiricileri etkilemez.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="7e6d6-257">Giriş biçimlendiricileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="7e6d6-258">Ayrıca bkz `[Required]` özniteliğini [Model doğrulama](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="7e6d6-259">[BindRequired] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-259">[BindRequired] attribute</span></span>

<span data-ttu-id="7e6d6-260">Yalnızca model özellikleri, yöntem parametreleri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="7e6d6-261">Model durumu hatası bağlanıyorsa bu değeri eklemek için neden model bağlama için bir modelin özelliği oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="7e6d6-262">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="7e6d6-263">[BindNever] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-263">[BindNever] attribute</span></span>

<span data-ttu-id="7e6d6-264">Yalnızca model özellikleri, yöntem parametreleri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="7e6d6-265">Model bağlama bir modelin özelliği ayarı öğesinden engeller.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="7e6d6-266">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="7e6d6-267">[Bağlama] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-267">[Bind] attribute</span></span>

<span data-ttu-id="7e6d6-268">Bir sınıf ya da bir yöntem parametresi için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="7e6d6-269">Model bağlama bir modelin hangi özelliklerin eklenmesi gereken belirtir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="7e6d6-270">Aşağıdaki örnekte, yalnızca belirtilen özelliklerini `Instructor` modeli, herhangi bir işleyici veya eylem yöntemi çağrıldığında bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="7e6d6-271">Aşağıdaki örnekte, yalnızca belirtilen özelliklerini `Instructor` model zaman bağlı `OnPost` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="7e6d6-272">`[Bind]` Özniteliği içinde overposting karşı korumak için kullanılabilir *oluşturma* senaryoları.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="7e6d6-273">Dışlanan özellikler için null veya varsayılan değeri olan yerine değiştirilmedi ayarlandığından iyi düzenleme senaryolarda işe yaramaz.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="7e6d6-274">Görünüm modelleri overposting karşı savunma için önerilen yerine `[Bind]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="7e6d6-275">Daha fazla bilgi için [overposting hakkında güvenlik notu](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="7e6d6-276">Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="7e6d6-276">Collections</span></span>

<span data-ttu-id="7e6d6-277">Basit türler hedefler için model bağlama için eşleşme arar *parameter_name* veya *property_name*.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="7e6d6-278">Eşleşme bulunursa, desteklenen biçimlerden öneki olmadan arar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="7e6d6-279">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-279">For example:</span></span>

* <span data-ttu-id="7e6d6-280">Bağlanacak parametre adlı bir dizi olduğunu varsayın `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="7e6d6-281">Form veya sorgu dizesi verileri aşağıdaki biçimlerden birinde olabilir:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="7e6d6-282">Aşağıdaki biçimi yalnızca form verilerini desteklenir:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="7e6d6-283">Tüm önceki örnek biçimler için model bağlama için iki öğelerin bir dizisi geçirir `selectedCourses` parametresi:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="7e6d6-284">[0] selectedCourses 1050 =</span><span class="sxs-lookup"><span data-stu-id="7e6d6-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="7e6d6-285">[1] selectedCourses 2000 =</span><span class="sxs-lookup"><span data-stu-id="7e6d6-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="7e6d6-286">Bu alt simge numaraları kullan (...) veri biçimleri [0]... [1]...) Bunlar sırayla sıfırdan başlayarak numaralandırılır emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="7e6d6-287">Alt simge numaralandırmayı herhangi bir boşluk varsa, boşluğun sonra tüm öğeleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="7e6d6-288">Örneğin, alt simgeler 0 ve 0 ile 1 yerine 2 ise, ikinci öğesi göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="7e6d6-289">sözlükler</span><span class="sxs-lookup"><span data-stu-id="7e6d6-289">Dictionaries</span></span>

<span data-ttu-id="7e6d6-290">İçin `Dictionary` hedefler, model bağlama için bir eşleşme arar *parameter_name* veya *property_name*.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="7e6d6-291">Eşleşme bulunursa, desteklenen biçimlerden öneki olmadan arar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="7e6d6-292">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-292">For example:</span></span>

* <span data-ttu-id="7e6d6-293">Hedef parametre olduğunu varsayın bir `Dictionary<string, string>` adlı `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-293">Suppose the target parameter is a `Dictionary<string, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="7e6d6-294">Gönderilen formundaki veya sorgu dizesi verileri aşağıdaki örneklerden birini gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="7e6d6-295">Tüm önceki örnek biçimler için model bağlama için iki öğeyi sözlüğü geçirir `selectedCourses` parametresi:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="7e6d6-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="7e6d6-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="7e6d6-297">selectedCourses ["2000"] "Ekonomisinden" =</span><span class="sxs-lookup"><span data-stu-id="7e6d6-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="7e6d6-298">Özel veri türleri</span><span class="sxs-lookup"><span data-stu-id="7e6d6-298">Special data types</span></span>

<span data-ttu-id="7e6d6-299">Model bağlama işleyebilir bazı özel veri türleri vardır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="7e6d6-300">IFormFile ve IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="7e6d6-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="7e6d6-301">HTTP isteğinde karşıya yüklenen dosya.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="7e6d6-302">Ayrıca desteklenen olan `IEnumerable<IFormFile>` birden çok dosya için.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="7e6d6-303">cancellationToken</span><span class="sxs-lookup"><span data-stu-id="7e6d6-303">CancellationToken</span></span>

<span data-ttu-id="7e6d6-304">Zaman uyumsuz denetleyicileri etkinliğinde iptal etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="7e6d6-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="7e6d6-305">FormCollection</span></span>

<span data-ttu-id="7e6d6-306">Tüm değerleri gönderilen form verileri almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="7e6d6-307">Giriş biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="7e6d6-307">Input formatters</span></span>

<span data-ttu-id="7e6d6-308">İstek gövdesinde veri, JSON, XML veya başka bir biçime olabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="7e6d6-309">Bu verileri ayrıştırmak için model bağlama kullanan bir *giriş biçimlendirici* belirli bir içerik türünü işleyecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="7e6d6-310">Varsayılan olarak, ASP.NET Core, JSON verilerini işlemek için giriş JSON tabanlı biçimlendiricileri içerir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="7e6d6-311">Diğer içerik türleri için diğer biçimlendiricileri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="7e6d6-312">ASP.NET Core giriş biçimlendiricileri göre seçer [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="7e6d6-313">Hiçbir öznitelik varsa, bunu kullanan [Content-Type üstbilgisi](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="7e6d6-314">Yerleşik XML giriş biçimlendiricileri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="7e6d6-315">Yükleme `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="7e6d6-316">İçinde `Startup.ConfigureServices`, çağrı <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="7e6d6-317">Uygulama `Consumes` özniteliği denetleyici sınıflarına veya istek gövdesinde XML beklemelisiniz eylem yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="7e6d6-318">Daha fazla bilgi için [giriş XML serileştirme](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-318">For more information, see [Introducing XML Serialization](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="7e6d6-319">Model bağlamanın dışında belirtilen türlerini dışla</span><span class="sxs-lookup"><span data-stu-id="7e6d6-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="7e6d6-320">Model bağlama ve doğrulama systems davranış tarafından yönlendirilen [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="7e6d6-321">Özelleştirebileceğiniz `ModelMetadata` ayrıntıları sağlayıcıya ekleyerek [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="7e6d6-322">Yerleşik ayrıntıları sağlayıcıları, model bağlama veya belirtilen tür için doğrulama devre dışı bırakmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="7e6d6-323">Belirtilen bir türün tüm modeller üzerinde model bağlama devre dışı bırakmak için ekleme bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7e6d6-324">Örneğin, devre dışı bırakma türü tüm modeller üzerinde model bağlama için `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="7e6d6-325">Belirtilen türün özelliklerini doğrulamasını devre dışı bırakmak için ekleme bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7e6d6-326">Örneğin, türün özelliklerini doğrulama devre dışı bırakmak için `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="7e6d6-327">Özel model bağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="7e6d6-327">Custom model binders</span></span>

<span data-ttu-id="7e6d6-328">Model bağlama özel yazarak genişletebilir model bağlayıcısını ve kullanarak `[ModelBinder]` özniteliği için belirtilen bir hedef seçin.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="7e6d6-329">Daha fazla bilgi edinin [özel model bağlama](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="7e6d6-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="7e6d6-330">El ile model bağlama</span><span class="sxs-lookup"><span data-stu-id="7e6d6-330">Manual model binding</span></span>

<span data-ttu-id="7e6d6-331">Model bağlama çağrılacak el ile kullanarak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="7e6d6-332">Yöntemi, hem de tanımlanır `ControllerBase` ve `PageModel` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="7e6d6-333">Yöntem aşırı yüklemeleri kullanılacak öneki ve değer sağlayıcısı belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="7e6d6-334">Yöntem döndürür `false` model bağlama başarısız olursa.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="7e6d6-335">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="7e6d6-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="7e6d6-336">[FromServices] özniteliği</span><span class="sxs-lookup"><span data-stu-id="7e6d6-336">[FromServices] attribute</span></span>

<span data-ttu-id="7e6d6-337">Bu özniteliğin adı, bir veri kaynağını belirtin, model bağlama özniteliklerini desenini izler.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="7e6d6-338">Ancak bir değer sağlayıcısı veri bağlama hakkında değil.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="7e6d6-339">Bir türün bir örneği alır [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7e6d6-340">Amacı, bir hizmet, yalnızca belirli bir yöntemi çağrılırsa ihtiyacınız olduğunda Oluşturucu ekleme için alternatif sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="7e6d6-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e6d6-341">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7e6d6-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
