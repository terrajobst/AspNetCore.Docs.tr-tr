---
title: ASP.NET Core 'de model bağlama
author: rick-anderson
description: ASP.NET Core model bağlamasının nasıl çalıştığını ve davranışını nasıl özelleştireceğinizi öğrenin.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/21/2019
uid: mvc/models/model-binding
ms.openlocfilehash: da6cc25e0bbb1b2301529b34eab4c91f9ccb46eb
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944301"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="c6d5d-103">ASP.NET Core 'de model bağlama</span><span class="sxs-lookup"><span data-stu-id="c6d5d-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c6d5d-104">Bu makalede, model bağlamanın ne olduğu, nasıl çalıştığı ve davranışını nasıl özelleştireceğiniz açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="c6d5d-105">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="c6d5d-106">Model bağlama nedir?</span><span class="sxs-lookup"><span data-stu-id="c6d5d-106">What is Model binding</span></span>

<span data-ttu-id="c6d5d-107">Denetleyiciler ve Razor sayfaları, HTTP isteklerinden gelen verilerle çalışır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="c6d5d-108">Örneğin, rota verileri bir kayıt anahtarı sağlayabilir ve postalanan form alanları, modelin özelliklerine ilişkin değerler sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="c6d5d-109">Bu değerlerin her birini almak ve bunları dizelerden .NET türlerine dönüştürmek için kod yazma sıkıcı ve hata durumunda olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="c6d5d-110">Model bağlama bu işlemi otomatikleştirir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-110">Model binding automates this process.</span></span> <span data-ttu-id="c6d5d-111">Model bağlama sistemi:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-111">The model binding system:</span></span>

* <span data-ttu-id="c6d5d-112">Veri yolu, form alanları ve sorgu dizeleri gibi çeşitli kaynaklardan veri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="c6d5d-113">Yöntem parametrelerinde ve genel özelliklerde bulunan denetleyicilere ve Razor sayfalarına verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="c6d5d-114">Dize verilerini .NET türlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="c6d5d-115">Karmaşık türlerin özelliklerini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="c6d5d-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6d5d-116">Example</span></span>

<span data-ttu-id="c6d5d-117">Aşağıdaki eylem yöntemine sahip olduğunuzu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="c6d5d-118">Ve uygulama şu URL ile bir istek alıyor:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="c6d5d-119">Model bağlama, yönlendirme sistemi eylem yöntemini seçtikten sonra aşağıdaki adımlardan geçer:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="c6d5d-120">`id`adlı bir tamsayı olan `GetByID`ilk parametresini bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="c6d5d-121">HTTP isteğindeki kullanılabilir kaynakları arar ve yönlendirme verilerinde `id` = "2" bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="c6d5d-122">"2" dizesini tamsayı 2 ' ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="c6d5d-123">`dogsOnly`adlı bir Boole değeri olan `GetByID`sonraki parametresini bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="c6d5d-124">Kaynakları arar ve sorgu dizesinde "DogsOnly = true" bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="c6d5d-125">Ad eşleştirme, büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="c6d5d-126">"True" dizesini Boole `true`dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="c6d5d-127">Daha sonra Framework, `id` parametresi için 2 ' ye geçerek ve `dogsOnly` parametresi için `true` `GetById` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="c6d5d-128">Önceki örnekte, model bağlama hedefleri basit türler olan yöntem parametreleridir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="c6d5d-129">Hedefler, karmaşık bir türün özellikleri de olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="c6d5d-130">Her bir özellik başarıyla bağlandıktan sonra, bu özellik için [model doğrulaması](xref:mvc/models/validation) oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="c6d5d-131">Hangi verilerin modele bağladığına ve tüm bağlama veya doğrulama hatalarıyla ilgili kayıt, [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [Pagemodel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="c6d5d-132">Bu işlemin başarılı olup olmadığını öğrenmek için uygulama [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağını denetler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="c6d5d-133">Hedefler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-133">Targets</span></span>

<span data-ttu-id="c6d5d-134">Model bağlama, aşağıdaki tür hedeflerin değerlerini bulmayı dener:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="c6d5d-135">Bir isteğin yönlendirildiği denetleyici eylemi yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="c6d5d-136">Bir isteğin yönlendirildiği Razor Pages işleyicisi yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="c6d5d-137">Bir denetleyicinin veya `PageModel` sınıfının öznitelikler tarafından belirtilmişse ortak özellikleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="c6d5d-138">[BindProperty] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-138">[BindProperty] attribute</span></span>

<span data-ttu-id="c6d5d-139">Bir denetleyicinin veya `PageModel` sınıfın ortak özelliğine, model bağlamasının bu özelliği hedeflemesini sağlamak için uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="c6d5d-140">[BindProperties] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-140">[BindProperties] attribute</span></span>

<span data-ttu-id="c6d5d-141">ASP.NET Core 2,1 ve üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="c6d5d-142">Model bağlamaya, sınıfın tüm ortak özelliklerini hedeflemesini bildirmek için bir denetleyiciye veya `PageModel` sınıfa uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="c6d5d-143">HTTP GET istekleri için model bağlama</span><span class="sxs-lookup"><span data-stu-id="c6d5d-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="c6d5d-144">Varsayılan olarak, Özellikler HTTP GET istekleri için bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="c6d5d-145">Genellikle, bir GET isteği için tüm ihtiyacınız olan bir kayıt KIMLIĞI parametresidir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="c6d5d-146">Kayıt KIMLIĞI, veritabanındaki öğeyi aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="c6d5d-147">Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamaya gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="c6d5d-148">GET isteklerinden alınan özelliklerin verilerine bağlanmasını istediğiniz senaryolarda `SupportsGet` özelliğini `true`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="c6d5d-149">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-149">Sources</span></span>

<span data-ttu-id="c6d5d-150">Varsayılan olarak, model bağlama, bir HTTP isteğindeki aşağıdaki kaynaklardan gelen anahtar-değer çiftleri biçimindeki verileri alır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="c6d5d-151">Form alanları</span><span class="sxs-lookup"><span data-stu-id="c6d5d-151">Form fields</span></span>
1. <span data-ttu-id="c6d5d-152">İstek gövdesi ( [[ApiController] özniteliğine sahip denetleyiciler](xref:web-api/index#binding-source-parameter-inference)için.)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="c6d5d-153">Veri yönlendirme</span><span class="sxs-lookup"><span data-stu-id="c6d5d-153">Route data</span></span>
1. <span data-ttu-id="c6d5d-154">Sorgu dizesi parametreleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-154">Query string parameters</span></span>
1. <span data-ttu-id="c6d5d-155">Karşıya yüklenen dosyalar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-155">Uploaded files</span></span>

<span data-ttu-id="c6d5d-156">Her hedef parametresi veya özelliği için kaynaklar, önceki listede belirtilen sırada taranır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="c6d5d-157">Birkaç özel durum vardır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-157">There are a few exceptions:</span></span>

* <span data-ttu-id="c6d5d-158">Rota verileri ve sorgu dizesi değerleri yalnızca basit türler için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="c6d5d-159">Karşıya yüklenen dosyalar yalnızca `IFormFile` veya `IEnumerable<IFormFile>`uygulayan hedef türlere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="c6d5d-160">Varsayılan kaynak doğru değilse, kaynağı belirtmek için aşağıdaki özniteliklerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="c6d5d-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -sorgu dizesinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="c6d5d-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -rota verilerinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="c6d5d-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -postalanan Form alanlarındaki değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="c6d5d-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -istek gövdesinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="c6d5d-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -http başlıklarındaki değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="c6d5d-166">Bu öznitelikler:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-166">These attributes:</span></span>

* <span data-ttu-id="c6d5d-167">Model özelliklerine tek tek eklenir (model sınıfına değil), aşağıdaki örnekte olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="c6d5d-168">İsteğe bağlı olarak oluşturucuda bir model adı değeri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="c6d5d-169">Bu seçenek, özellik adının istekteki değerle eşleşmemesi durumunda sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="c6d5d-170">Örneğin, istekteki değer aşağıdaki örnekte olduğu gibi adında bir tire olan bir üstbilgi olabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="c6d5d-171">[FromBody] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-171">[FromBody] attribute</span></span>

<span data-ttu-id="c6d5d-172">Bir HTTP isteğinin gövdesinden özelliklerini doldurmak için `[FromBody]` özniteliğini bir parametreye uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="c6d5d-173">ASP.NET Core çalışma zamanı, gövdeyi bir giriş biçimlendirici 'ya okumaktan sorumlu olarak temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="c6d5d-174">Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="c6d5d-175">Karmaşık bir tür parametresine `[FromBody]` uygulandığında, özelliklerine uygulanan herhangi bir bağlama kaynak özniteliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="c6d5d-176">Örneğin, aşağıdaki `Create` eylemi `pet` parametresinin gövdeden doldurulduğunu belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="c6d5d-177">`Pet` sınıfı, `Breed` özelliğinin bir sorgu dizesi parametresinden doldurulduğunu belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="c6d5d-178">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-178">In the preceding example:</span></span>

* <span data-ttu-id="c6d5d-179">`[FromQuery]` özniteliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="c6d5d-180">`Breed` özelliği bir sorgu dizesi parametresinden doldurulmamış.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="c6d5d-181">Giriş biçimleri yalnızca gövdeyi okur ve bağlama kaynak özniteliklerini anlamayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="c6d5d-182">Gövdede uygun bir değer bulunursa, bu değer `Breed` özelliğini doldurmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="c6d5d-183">Eylem yöntemi başına birden fazla parametreye `[FromBody]` uygulamayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="c6d5d-184">İstek akışı bir giriş biçimlendirici tarafından okunduktan sonra, diğer `[FromBody]` parametrelerini bağlamak için artık bir daha okunamaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="c6d5d-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-185">Additional sources</span></span>

<span data-ttu-id="c6d5d-186">Kaynak verileri, model bağlama sistemine *değer sağlayıcılara*göre sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="c6d5d-187">Diğer kaynaklardan model bağlamaya yönelik verileri alan özel değer sağlayıcıları yazabilir ve kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="c6d5d-188">Örneğin, tanımlama bilgileri veya oturum durumu verilerini isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="c6d5d-189">Yeni bir kaynaktan veri almak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-189">To get data from a new source:</span></span>

* <span data-ttu-id="c6d5d-190">`IValueProvider`uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="c6d5d-191">`IValueProviderFactory`uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="c6d5d-192">Factory sınıfını `Startup.ConfigureServices`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="c6d5d-193">Örnek uygulama, tanımlama bilgilerinden değerler alan bir [değer sağlayıcısı](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) ve [Factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) örneği içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="c6d5d-194">`Startup.ConfigureServices`kayıt kodu aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="c6d5d-195">Gösterilen kod, tüm yerleşik değer sağlayıcılarından sonra özel değer sağlayıcısını koyar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="c6d5d-196">Listenin ilk olması için, `Add`yerine `Insert(0, new CookieValueProviderFactory())` çağırın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="c6d5d-197">Model özelliği için kaynak yok</span><span class="sxs-lookup"><span data-stu-id="c6d5d-197">No source for a model property</span></span>

<span data-ttu-id="c6d5d-198">Varsayılan olarak, model özelliği için bir değer bulunmazsa model durumu hatası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="c6d5d-199">Özelliği null veya varsayılan bir değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="c6d5d-200">Null yapılabilir basit türler `null`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="c6d5d-201">Null yapılamayan değer türleri `default(T)`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="c6d5d-202">Örneğin, `int id` parametresi 0 olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="c6d5d-203">Karmaşık türler için model bağlama, özellikleri ayarlamadan varsayılan oluşturucuyu kullanarak bir örnek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="c6d5d-204">Diziler, `byte[]` dizilerinin `null`olarak ayarlandığı durumlar dışında `Array.Empty<T>()`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="c6d5d-205">Model özelliği için form alanlarında hiçbir şey bulunamadığında model durumunun geçersiz kılınmalıdır, [`[BindRequired]`](#bindrequired-attribute) özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="c6d5d-206">Bu `[BindRequired]` davranışının, bir istek gövdesinde JSON veya XML verilerine değil, postalanan form verilerinden model bağlama için geçerli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="c6d5d-207">İstek gövdesi verileri, [giriş formatlayıcıları](#input-formatters)tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="c6d5d-208">Tür dönüştürme hataları</span><span class="sxs-lookup"><span data-stu-id="c6d5d-208">Type conversion errors</span></span>

<span data-ttu-id="c6d5d-209">Bir kaynak bulunursa ancak hedef türe dönüştürülemiyorsa, model durumu geçersiz olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="c6d5d-210">Hedef parametresi veya özelliği, önceki bölümde belirtildiği gibi null veya varsayılan değer olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="c6d5d-211">`[ApiController]` özniteliğine sahip bir API denetleyicisinde, geçersiz model durumu otomatik HTTP 400 yanıtına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="c6d5d-212">Razor sayfasında, sayfayı bir hata iletisiyle yeniden görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="c6d5d-213">İstemci tarafı doğrulama, aksi durumda Razor Pages bir forma gönderilemeyen hatalı verileri yakalar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="c6d5d-214">Bu doğrulama, önceki vurgulanmış kodu tetiklemeyi zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="c6d5d-215">Örnek uygulama, teslim **tarihi** alanına hatalı veri yerleştiren ve formu Gönderen **Geçersiz tarih içeren bir Gönder** düğmesi içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="c6d5d-216">Bu düğme, veri dönüştürme hataları oluştuğunda sayfanın yeniden görüntülenmesine yönelik kodun nasıl çalıştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="c6d5d-217">Sayfa önceki kodla yeniden görüntülendiğinde, form alanında geçersiz giriş gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="c6d5d-218">Bunun nedeni model özelliğinin null ya da varsayılan bir değer olarak ayarlanmış olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="c6d5d-219">Geçersiz giriş bir hata iletisinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="c6d5d-220">Ancak form alanındaki hatalı verileri yeniden görüntülemek istiyorsanız, model özelliğini bir dize haline getirmeyi ve veri dönüştürmeyi el ile gerçekleştirmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="c6d5d-221">Tür dönüştürme hatalarının model durumu hatalarına neden olmasını istemiyorsanız aynı strateji önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="c6d5d-222">Bu durumda model özelliğini bir dize yapın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="c6d5d-223">Basit türler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-223">Simple types</span></span>

<span data-ttu-id="c6d5d-224">Model cildin kaynak dizeleri dönüştürebileceğiniz basit türler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="c6d5d-225">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="c6d5d-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="c6d5d-227">Char</span><span class="sxs-lookup"><span data-stu-id="c6d5d-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="c6d5d-228">Hem</span><span class="sxs-lookup"><span data-stu-id="c6d5d-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="c6d5d-229">Türünde</span><span class="sxs-lookup"><span data-stu-id="c6d5d-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="c6d5d-230">Kategori</span><span class="sxs-lookup"><span data-stu-id="c6d5d-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="c6d5d-231">Çift</span><span class="sxs-lookup"><span data-stu-id="c6d5d-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="c6d5d-232">Yardımının</span><span class="sxs-lookup"><span data-stu-id="c6d5d-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="c6d5d-233">'İni</span><span class="sxs-lookup"><span data-stu-id="c6d5d-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="c6d5d-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="c6d5d-235">Sunuculu</span><span class="sxs-lookup"><span data-stu-id="c6d5d-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="c6d5d-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c6d5d-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="c6d5d-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="c6d5d-238">Uri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="c6d5d-239">Sürüm</span><span class="sxs-lookup"><span data-stu-id="c6d5d-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="c6d5d-240">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-240">Complex types</span></span>

<span data-ttu-id="c6d5d-241">Karmaşık bir türün bağlanması için ortak bir varsayılan Oluşturucusu ve ortak yazılabilir özellikleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="c6d5d-242">Model bağlama gerçekleştiğinde, sınıf ortak varsayılan Oluşturucu kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="c6d5d-243">Karmaşık türün her özelliği için model bağlama, ad modeli ön eki için kaynakları arar *. property_name*.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="c6d5d-244">Hiçbir şey bulunamazsa, ön ek olmadan yalnızca *property_name* arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="c6d5d-245">Bir parametreye bağlama için, önek parametre adıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="c6d5d-246">`PageModel` public özelliğine bağlama için, önek ortak özellik adıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="c6d5d-247">Bazı özniteliklerin, parametre veya özellik adının varsayılan kullanımını geçersiz kılabilmenizi sağlayan `Prefix` bir özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="c6d5d-248">Örneğin, karmaşık türün aşağıdaki `Instructor` sınıfı olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="c6d5d-249">Önek = parametre adı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-249">Prefix = parameter name</span></span>

<span data-ttu-id="c6d5d-250">Bağlanacak model `instructorToUpdate`adlı bir parametredir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="c6d5d-251">Model bağlama, anahtar `instructorToUpdate.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="c6d5d-252">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="c6d5d-253">Önek = Özellik adı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-253">Prefix = property name</span></span>

<span data-ttu-id="c6d5d-254">Bağlanacak model, denetleyicinin veya `PageModel` sınıfının `Instructor` adlı bir özelliktir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="c6d5d-255">Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="c6d5d-256">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="c6d5d-257">Özel ön ek</span><span class="sxs-lookup"><span data-stu-id="c6d5d-257">Custom prefix</span></span>

<span data-ttu-id="c6d5d-258">Bağlanacak model `instructorToUpdate` adlı bir parametredir ve `Bind` özniteliği önek olarak `Instructor` belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="c6d5d-259">Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="c6d5d-260">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="c6d5d-261">Karmaşık tür hedefleri için öznitelikler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-261">Attributes for complex type targets</span></span>

<span data-ttu-id="c6d5d-262">Karmaşık türlerin model bağlamasını denetlemek için birkaç yerleşik öznitelik mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="c6d5d-263">Bu öznitelikler, gönderilen form verileri değer kaynağı olduğunda model bağlamayı etkiler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="c6d5d-264">Bunlar, gönderilen JSON ve XML istek gövdelerini işleyen giriş formatlayıcıları 'nı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="c6d5d-265">Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="c6d5d-266">Ayrıca bkz. [model doğrulamasında](xref:mvc/models/validation#required-attribute)`[Required]` özniteliği tartışması.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="c6d5d-267">[BindRequired] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-267">[BindRequired] attribute</span></span>

<span data-ttu-id="c6d5d-268">, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="c6d5d-269">Modelin özelliği için bağlama gerçekleşmemişse model bağlamasının model durumu hatası eklemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="c6d5d-270">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="c6d5d-271">[Bindhiç] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-271">[BindNever] attribute</span></span>

<span data-ttu-id="c6d5d-272">, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="c6d5d-273">Model bağlamasının model özelliğini değiştirmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="c6d5d-274">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="c6d5d-275">[Bind] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-275">[Bind] attribute</span></span>

<span data-ttu-id="c6d5d-276">, Bir sınıfa veya yöntem parametresine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="c6d5d-277">Model bağlamasındaki bir modelin hangi özelliklerinin dahil edileceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="c6d5d-278">Aşağıdaki örnekte, herhangi bir işleyici veya eylem yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="c6d5d-279">Aşağıdaki örnekte, `OnPost` yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="c6d5d-280">`[Bind]` özniteliği, *oluşturma* senaryolarında fazla nakline karşı korumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="c6d5d-281">Dışlanan Özellikler null ya da boş değer olarak ayarlandığı için, düzenleme senaryolarında iyi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="c6d5d-282">Fazla nakline karşı savunma için, `[Bind]` özniteliği yerine, görüntüleme modelleri önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="c6d5d-283">Daha fazla bilgi için bkz. fazla [nakil hakkında güvenlik NOI](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="c6d5d-284">Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-284">Collections</span></span>

<span data-ttu-id="c6d5d-285">Basit türlerin koleksiyonları olan hedefler için model bağlama, *parameter_name* veya *property_name*ile eşleşmeleri arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="c6d5d-286">Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="c6d5d-287">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-287">For example:</span></span>

* <span data-ttu-id="c6d5d-288">Bağlanacak parametrenin `selectedCourses`adlı bir dizi olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="c6d5d-289">Form veya sorgu dizesi verileri aşağıdaki biçimlerden birinde olabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="c6d5d-290">Aşağıdaki biçim yalnızca form verilerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="c6d5d-291">Önceki örnek biçimlerinin hepsi için model bağlama iki öğe dizisini `selectedCourses` parametresine geçirir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="c6d5d-292">Selectedkurslar [0] = 1050</span><span class="sxs-lookup"><span data-stu-id="c6d5d-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="c6d5d-293">Selectedkurslar [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="c6d5d-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="c6d5d-294">Alt simge numaralarını kullanan veri biçimleri (... [0]... [1]...) sıfırdan başlayarak sıralı olarak numaralandırıldıklarından emin olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="c6d5d-295">Alt simge numaralandırmasında boşluk varsa, boşluklardan sonraki tüm öğeler yoksayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="c6d5d-296">Örneğin, alt simgeler 0 ve 1 yerine 0 ve 2 ise ikinci öğe yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="c6d5d-297">Sözlükler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-297">Dictionaries</span></span>

<span data-ttu-id="c6d5d-298">`Dictionary` hedefler için, model bağlama *parameter_name* veya *property_name*eşleşmelerini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="c6d5d-299">Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="c6d5d-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-300">For example:</span></span>

* <span data-ttu-id="c6d5d-301">Hedef parametrenin `selectedCourses`adlı bir `Dictionary<int, string>` olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="c6d5d-302">Postalanan form veya sorgu dizesi verileri aşağıdaki örneklerden birine benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="c6d5d-303">Önceki örnek biçimlerinin hepsi için model bağlama iki öğenin sözlüğünü `selectedCourses` parametresine geçirir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="c6d5d-304">Selectedkurslar ["1050"] = "Chemistry"</span><span class="sxs-lookup"><span data-stu-id="c6d5d-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="c6d5d-305">Selectedkurslar ["2000"] = "Ekonomiks"</span><span class="sxs-lookup"><span data-stu-id="c6d5d-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="c6d5d-306">Model bağlama yolu verileri ve sorgu dizeleri için Genelleştirme davranışı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="c6d5d-307">ASP.NET Core yol değeri sağlayıcısı ve sorgu dizesi değer sağlayıcısı:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="c6d5d-308">Değerleri sabit kültür olarak değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="c6d5d-309">URL 'Lerin kültür sabiti olmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="c6d5d-310">Buna karşılık, form verilerinden gelen değerler kültüre duyarlı bir dönüştürmeye gider.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="c6d5d-311">Bu, URL 'Lerin yerel ayarlarda paylaşılabilir olması için tasarımdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="c6d5d-312">ASP.NET Core yol değeri sağlayıcısını ve sorgu dizesi değeri sağlayıcısını, kültüre duyarlı bir dönüşüme dönüştürmek için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="c6d5d-313"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory> 'den devralma</span><span class="sxs-lookup"><span data-stu-id="c6d5d-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="c6d5d-314">[QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) veya [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs) adresinden kodu kopyalayın</span><span class="sxs-lookup"><span data-stu-id="c6d5d-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="c6d5d-315">Değer sağlayıcısı oluşturucusuna geçirilen [kültür değerini](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture) ile değiştirin</span><span class="sxs-lookup"><span data-stu-id="c6d5d-315">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="c6d5d-316">MVC seçeneklerinde varsayılan değer sağlayıcı fabrikasını yeni bir değerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="c6d5d-317">Özel veri türleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-317">Special data types</span></span>

<span data-ttu-id="c6d5d-318">Model bağlamanın işleyebileceği bazı özel veri türleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="c6d5d-319">Iformfile ve ıformfilecollection</span><span class="sxs-lookup"><span data-stu-id="c6d5d-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="c6d5d-320">HTTP isteğine eklenen karşıya yüklenen dosya.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="c6d5d-321">Ayrıca, birden çok dosya için `IEnumerable<IFormFile>` desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="c6d5d-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="c6d5d-322">CancellationToken</span></span>

<span data-ttu-id="c6d5d-323">Zaman uyumsuz denetleyicilerde etkinliği iptal etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="c6d5d-324">Form koleksiyonu</span><span class="sxs-lookup"><span data-stu-id="c6d5d-324">FormCollection</span></span>

<span data-ttu-id="c6d5d-325">Postalanan form verilerinden tüm değerleri almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="c6d5d-326">Giriş biçimleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-326">Input formatters</span></span>

<span data-ttu-id="c6d5d-327">İstek gövdesindeki veriler JSON, XML veya başka bir biçimde olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="c6d5d-328">Model bağlama, bu verileri ayrıştırmak için belirli bir içerik türünü işlemek üzere yapılandırılmış bir *giriş biçimlendiricisi* kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="c6d5d-329">Varsayılan olarak, ASP.NET Core JSON verilerini işlemek için JSON tabanlı giriş formatlayıcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="c6d5d-330">Diğer içerik türleri için başka biçim ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="c6d5d-331">ASP.NET Core, [tüketen](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliğine bağlı olarak giriş formatlayıcıları seçer.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="c6d5d-332">Hiçbir öznitelik yoksa, [Content-Type üst bilgisini](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="c6d5d-333">Yerleşik XML girişi formatlayıcıları 'nı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="c6d5d-334">`Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="c6d5d-335">`Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="c6d5d-336">`Consumes` özniteliğini, istek gövdesinde XML beklemeniz gereken denetleyici sınıflarına veya eylem yöntemlerine uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="c6d5d-337">Daha fazla bilgi için bkz. [XML serileştirme tanıtımı](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="c6d5d-338">Belirtilen türleri model bağlamalarından Dışla</span><span class="sxs-lookup"><span data-stu-id="c6d5d-338">Exclude specified types from model binding</span></span>

<span data-ttu-id="c6d5d-339">Model bağlama ve doğrulama sistemlerinin davranışı [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata)tarafından yönlendiriliyor.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-339">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="c6d5d-340">[Mvcoptions. Modelmetadatadetails sağlayıcılarına](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)bir ayrıntı sağlayıcısı ekleyerek `ModelMetadata` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-340">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="c6d5d-341">Yerleşik Ayrıntılar sağlayıcıları, belirtilen türler için model bağlamayı veya doğrulamayı devre dışı bırakmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-341">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="c6d5d-342">Belirtilen türdeki tüm modellerdeki model bağlamayı devre dışı bırakmak için `Startup.ConfigureServices`bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-342">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d5d-343">Örneğin, `System.Version`türündeki tüm modeller üzerinde model bağlamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-343">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="c6d5d-344">Belirtilen türdeki özelliklerde doğrulamayı devre dışı bırakmak için `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-344">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d5d-345">Örneğin, `System.Guid`türündeki özelliklerde doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-345">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="c6d5d-346">Özel model ciltleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-346">Custom model binders</span></span>

<span data-ttu-id="c6d5d-347">Özel bir model cildi yazarak ve belirli bir hedef için seçmek üzere `[ModelBinder]` özniteliğini kullanarak model bağlamayı genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-347">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="c6d5d-348">[Özel model bağlama](xref:mvc/advanced/custom-model-binding)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-348">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="c6d5d-349">El ile model bağlama</span><span class="sxs-lookup"><span data-stu-id="c6d5d-349">Manual model binding</span></span>

<span data-ttu-id="c6d5d-350">Model bağlama, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi kullanılarak el ile çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-350">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="c6d5d-351">Yöntemi hem `ControllerBase` hem de `PageModel` sınıflarında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-351">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="c6d5d-352">Yöntem aşırı yüklemeleri, kullanılacak öneki ve değer sağlayıcısını belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-352">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="c6d5d-353">Model bağlama başarısız olursa Yöntem `false` döndürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-353">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="c6d5d-354">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-354">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="c6d5d-355">[FromServices] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-355">[FromServices] attribute</span></span>

<span data-ttu-id="c6d5d-356">Bu özniteliğin adı, bir veri kaynağı belirten model bağlama özniteliklerinin düzeniyle uyar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-356">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="c6d5d-357">Ancak bir değer sağlayıcısından veri bağlama hakkında bilgi yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-357">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="c6d5d-358">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısından bir türün bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-358">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c6d5d-359">Amacı, yalnızca belirli bir yöntem çağrılırsa bir hizmete ihtiyacınız olduğunda Oluşturucu ekleme için bir alternatif sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-359">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6d5d-360">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-360">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c6d5d-361">Bu makalede, model bağlamanın ne olduğu, nasıl çalıştığı ve davranışını nasıl özelleştireceğiniz açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-361">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="c6d5d-362">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-362">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="c6d5d-363">Model bağlama nedir?</span><span class="sxs-lookup"><span data-stu-id="c6d5d-363">What is Model binding</span></span>

<span data-ttu-id="c6d5d-364">Denetleyiciler ve Razor sayfaları, HTTP isteklerinden gelen verilerle çalışır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-364">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="c6d5d-365">Örneğin, rota verileri bir kayıt anahtarı sağlayabilir ve postalanan form alanları, modelin özelliklerine ilişkin değerler sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-365">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="c6d5d-366">Bu değerlerin her birini almak ve bunları dizelerden .NET türlerine dönüştürmek için kod yazma sıkıcı ve hata durumunda olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-366">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="c6d5d-367">Model bağlama bu işlemi otomatikleştirir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-367">Model binding automates this process.</span></span> <span data-ttu-id="c6d5d-368">Model bağlama sistemi:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-368">The model binding system:</span></span>

* <span data-ttu-id="c6d5d-369">Veri yolu, form alanları ve sorgu dizeleri gibi çeşitli kaynaklardan veri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-369">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="c6d5d-370">Yöntem parametrelerinde ve genel özelliklerde bulunan denetleyicilere ve Razor sayfalarına verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-370">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="c6d5d-371">Dize verilerini .NET türlerine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-371">Converts string data to .NET types.</span></span>
* <span data-ttu-id="c6d5d-372">Karmaşık türlerin özelliklerini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-372">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="c6d5d-373">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6d5d-373">Example</span></span>

<span data-ttu-id="c6d5d-374">Aşağıdaki eylem yöntemine sahip olduğunuzu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-374">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="c6d5d-375">Ve uygulama şu URL ile bir istek alıyor:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-375">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="c6d5d-376">Model bağlama, yönlendirme sistemi eylem yöntemini seçtikten sonra aşağıdaki adımlardan geçer:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-376">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="c6d5d-377">`id`adlı bir tamsayı olan `GetByID`ilk parametresini bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-377">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="c6d5d-378">HTTP isteğindeki kullanılabilir kaynakları arar ve yönlendirme verilerinde `id` = "2" bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-378">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="c6d5d-379">"2" dizesini tamsayı 2 ' ye dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-379">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="c6d5d-380">`dogsOnly`adlı bir Boole değeri olan `GetByID`sonraki parametresini bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-380">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="c6d5d-381">Kaynakları arar ve sorgu dizesinde "DogsOnly = true" bulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-381">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="c6d5d-382">Ad eşleştirme, büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-382">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="c6d5d-383">"True" dizesini Boole `true`dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-383">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="c6d5d-384">Daha sonra Framework, `id` parametresi için 2 ' ye geçerek ve `dogsOnly` parametresi için `true` `GetById` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-384">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="c6d5d-385">Önceki örnekte, model bağlama hedefleri basit türler olan yöntem parametreleridir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-385">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="c6d5d-386">Hedefler, karmaşık bir türün özellikleri de olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-386">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="c6d5d-387">Her bir özellik başarıyla bağlandıktan sonra, bu özellik için [model doğrulaması](xref:mvc/models/validation) oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-387">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="c6d5d-388">Hangi verilerin modele bağladığına ve tüm bağlama veya doğrulama hatalarıyla ilgili kayıt, [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [Pagemodel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-388">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="c6d5d-389">Bu işlemin başarılı olup olmadığını öğrenmek için uygulama [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağını denetler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-389">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="c6d5d-390">Hedefler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-390">Targets</span></span>

<span data-ttu-id="c6d5d-391">Model bağlama, aşağıdaki tür hedeflerin değerlerini bulmayı dener:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-391">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="c6d5d-392">Bir isteğin yönlendirildiği denetleyici eylemi yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-392">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="c6d5d-393">Bir isteğin yönlendirildiği Razor Pages işleyicisi yönteminin parametreleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-393">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="c6d5d-394">Bir denetleyicinin veya `PageModel` sınıfının öznitelikler tarafından belirtilmişse ortak özellikleri.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-394">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="c6d5d-395">[BindProperty] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-395">[BindProperty] attribute</span></span>

<span data-ttu-id="c6d5d-396">Bir denetleyicinin veya `PageModel` sınıfın ortak özelliğine, model bağlamasının bu özelliği hedeflemesini sağlamak için uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-396">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="c6d5d-397">[BindProperties] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-397">[BindProperties] attribute</span></span>

<span data-ttu-id="c6d5d-398">ASP.NET Core 2,1 ve üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-398">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="c6d5d-399">Model bağlamaya, sınıfın tüm ortak özelliklerini hedeflemesini bildirmek için bir denetleyiciye veya `PageModel` sınıfa uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-399">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="c6d5d-400">HTTP GET istekleri için model bağlama</span><span class="sxs-lookup"><span data-stu-id="c6d5d-400">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="c6d5d-401">Varsayılan olarak, Özellikler HTTP GET istekleri için bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-401">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="c6d5d-402">Genellikle, bir GET isteği için tüm ihtiyacınız olan bir kayıt KIMLIĞI parametresidir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-402">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="c6d5d-403">Kayıt KIMLIĞI, veritabanındaki öğeyi aramak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-403">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="c6d5d-404">Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamaya gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-404">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="c6d5d-405">GET isteklerinden alınan özelliklerin verilerine bağlanmasını istediğiniz senaryolarda `SupportsGet` özelliğini `true`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-405">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="c6d5d-406">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-406">Sources</span></span>

<span data-ttu-id="c6d5d-407">Varsayılan olarak, model bağlama, bir HTTP isteğindeki aşağıdaki kaynaklardan gelen anahtar-değer çiftleri biçimindeki verileri alır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-407">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="c6d5d-408">Form alanları</span><span class="sxs-lookup"><span data-stu-id="c6d5d-408">Form fields</span></span>
1. <span data-ttu-id="c6d5d-409">İstek gövdesi ( [[ApiController] özniteliğine sahip denetleyiciler](xref:web-api/index#binding-source-parameter-inference)için.)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-409">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="c6d5d-410">Veri yönlendirme</span><span class="sxs-lookup"><span data-stu-id="c6d5d-410">Route data</span></span>
1. <span data-ttu-id="c6d5d-411">Sorgu dizesi parametreleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-411">Query string parameters</span></span>
1. <span data-ttu-id="c6d5d-412">Karşıya yüklenen dosyalar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-412">Uploaded files</span></span>

<span data-ttu-id="c6d5d-413">Her hedef parametresi veya özelliği için kaynaklar, önceki listede belirtilen sırada taranır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-413">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="c6d5d-414">Birkaç özel durum vardır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-414">There are a few exceptions:</span></span>

* <span data-ttu-id="c6d5d-415">Rota verileri ve sorgu dizesi değerleri yalnızca basit türler için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-415">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="c6d5d-416">Karşıya yüklenen dosyalar yalnızca `IFormFile` veya `IEnumerable<IFormFile>`uygulayan hedef türlere bağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-416">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="c6d5d-417">Varsayılan kaynak doğru değilse, kaynağı belirtmek için aşağıdaki özniteliklerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-417">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="c6d5d-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -sorgu dizesinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="c6d5d-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -rota verilerinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="c6d5d-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -postalanan Form alanlarındaki değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="c6d5d-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -istek gövdesinden değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="c6d5d-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -http başlıklarındaki değerleri alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="c6d5d-423">Bu öznitelikler:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-423">These attributes:</span></span>

* <span data-ttu-id="c6d5d-424">Model özelliklerine tek tek eklenir (model sınıfına değil), aşağıdaki örnekte olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-424">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="c6d5d-425">İsteğe bağlı olarak oluşturucuda bir model adı değeri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-425">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="c6d5d-426">Bu seçenek, özellik adının istekteki değerle eşleşmemesi durumunda sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-426">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="c6d5d-427">Örneğin, istekteki değer aşağıdaki örnekte olduğu gibi adında bir tire olan bir üstbilgi olabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-427">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="c6d5d-428">[FromBody] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-428">[FromBody] attribute</span></span>

<span data-ttu-id="c6d5d-429">Bir HTTP isteğinin gövdesinden özelliklerini doldurmak için `[FromBody]` özniteliğini bir parametreye uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-429">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="c6d5d-430">ASP.NET Core çalışma zamanı, gövdeyi bir giriş biçimlendirici 'ya okumaktan sorumlu olarak temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-430">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="c6d5d-431">Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-431">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="c6d5d-432">Karmaşık bir tür parametresine `[FromBody]` uygulandığında, özelliklerine uygulanan herhangi bir bağlama kaynak özniteliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-432">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="c6d5d-433">Örneğin, aşağıdaki `Create` eylemi `pet` parametresinin gövdeden doldurulduğunu belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-433">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="c6d5d-434">`Pet` sınıfı, `Breed` özelliğinin bir sorgu dizesi parametresinden doldurulduğunu belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-434">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="c6d5d-435">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-435">In the preceding example:</span></span>

* <span data-ttu-id="c6d5d-436">`[FromQuery]` özniteliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-436">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="c6d5d-437">`Breed` özelliği bir sorgu dizesi parametresinden doldurulmamış.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-437">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="c6d5d-438">Giriş biçimleri yalnızca gövdeyi okur ve bağlama kaynak özniteliklerini anlamayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-438">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="c6d5d-439">Gövdede uygun bir değer bulunursa, bu değer `Breed` özelliğini doldurmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-439">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="c6d5d-440">Eylem yöntemi başına birden fazla parametreye `[FromBody]` uygulamayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-440">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="c6d5d-441">İstek akışı bir giriş biçimlendirici tarafından okunduktan sonra, diğer `[FromBody]` parametrelerini bağlamak için artık bir daha okunamaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-441">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="c6d5d-442">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-442">Additional sources</span></span>

<span data-ttu-id="c6d5d-443">Kaynak verileri, model bağlama sistemine *değer sağlayıcılara*göre sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-443">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="c6d5d-444">Diğer kaynaklardan model bağlamaya yönelik verileri alan özel değer sağlayıcıları yazabilir ve kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-444">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="c6d5d-445">Örneğin, tanımlama bilgileri veya oturum durumu verilerini isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-445">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="c6d5d-446">Yeni bir kaynaktan veri almak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-446">To get data from a new source:</span></span>

* <span data-ttu-id="c6d5d-447">`IValueProvider`uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-447">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="c6d5d-448">`IValueProviderFactory`uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-448">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="c6d5d-449">Factory sınıfını `Startup.ConfigureServices`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-449">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="c6d5d-450">Örnek uygulama, tanımlama bilgilerinden değerler alan bir [değer sağlayıcısı](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) ve [Factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) örneği içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-450">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="c6d5d-451">`Startup.ConfigureServices`kayıt kodu aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-451">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="c6d5d-452">Gösterilen kod, tüm yerleşik değer sağlayıcılarından sonra özel değer sağlayıcısını koyar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-452">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="c6d5d-453">Listenin ilk olması için, `Add`yerine `Insert(0, new CookieValueProviderFactory())` çağırın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-453">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="c6d5d-454">Model özelliği için kaynak yok</span><span class="sxs-lookup"><span data-stu-id="c6d5d-454">No source for a model property</span></span>

<span data-ttu-id="c6d5d-455">Varsayılan olarak, model özelliği için bir değer bulunmazsa model durumu hatası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-455">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="c6d5d-456">Özelliği null veya varsayılan bir değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-456">The property is set to null or a default value:</span></span>

* <span data-ttu-id="c6d5d-457">Null yapılabilir basit türler `null`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-457">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="c6d5d-458">Null yapılamayan değer türleri `default(T)`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-458">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="c6d5d-459">Örneğin, `int id` parametresi 0 olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-459">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="c6d5d-460">Karmaşık türler için model bağlama, özellikleri ayarlamadan varsayılan oluşturucuyu kullanarak bir örnek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-460">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="c6d5d-461">Diziler, `byte[]` dizilerinin `null`olarak ayarlandığı durumlar dışında `Array.Empty<T>()`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-461">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="c6d5d-462">Model özelliği için form alanlarında hiçbir şey bulunamadığında model durumunun geçersiz kılınmalıdır, [`[BindRequired]`](#bindrequired-attribute) özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-462">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="c6d5d-463">Bu `[BindRequired]` davranışının, bir istek gövdesinde JSON veya XML verilerine değil, postalanan form verilerinden model bağlama için geçerli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-463">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="c6d5d-464">İstek gövdesi verileri, [giriş formatlayıcıları](#input-formatters)tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-464">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="c6d5d-465">Tür dönüştürme hataları</span><span class="sxs-lookup"><span data-stu-id="c6d5d-465">Type conversion errors</span></span>

<span data-ttu-id="c6d5d-466">Bir kaynak bulunursa ancak hedef türe dönüştürülemiyorsa, model durumu geçersiz olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-466">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="c6d5d-467">Hedef parametresi veya özelliği, önceki bölümde belirtildiği gibi null veya varsayılan değer olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-467">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="c6d5d-468">`[ApiController]` özniteliğine sahip bir API denetleyicisinde, geçersiz model durumu otomatik HTTP 400 yanıtına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-468">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="c6d5d-469">Razor sayfasında, sayfayı bir hata iletisiyle yeniden görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-469">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="c6d5d-470">İstemci tarafı doğrulama, aksi durumda Razor Pages bir forma gönderilemeyen hatalı verileri yakalar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-470">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="c6d5d-471">Bu doğrulama, önceki vurgulanmış kodu tetiklemeyi zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-471">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="c6d5d-472">Örnek uygulama, teslim **tarihi** alanına hatalı veri yerleştiren ve formu Gönderen **Geçersiz tarih içeren bir Gönder** düğmesi içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-472">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="c6d5d-473">Bu düğme, veri dönüştürme hataları oluştuğunda sayfanın yeniden görüntülenmesine yönelik kodun nasıl çalıştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-473">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="c6d5d-474">Sayfa önceki kodla yeniden görüntülendiğinde, form alanında geçersiz giriş gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-474">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="c6d5d-475">Bunun nedeni model özelliğinin null ya da varsayılan bir değer olarak ayarlanmış olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-475">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="c6d5d-476">Geçersiz giriş bir hata iletisinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-476">The invalid input does appear in an error message.</span></span> <span data-ttu-id="c6d5d-477">Ancak form alanındaki hatalı verileri yeniden görüntülemek istiyorsanız, model özelliğini bir dize haline getirmeyi ve veri dönüştürmeyi el ile gerçekleştirmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-477">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="c6d5d-478">Tür dönüştürme hatalarının model durumu hatalarına neden olmasını istemiyorsanız aynı strateji önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-478">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="c6d5d-479">Bu durumda model özelliğini bir dize yapın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-479">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="c6d5d-480">Basit türler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-480">Simple types</span></span>

<span data-ttu-id="c6d5d-481">Model cildin kaynak dizeleri dönüştürebileceğiniz basit türler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-481">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="c6d5d-482">Boole değeri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-482">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="c6d5d-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="c6d5d-484">Char</span><span class="sxs-lookup"><span data-stu-id="c6d5d-484">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="c6d5d-485">Hem</span><span class="sxs-lookup"><span data-stu-id="c6d5d-485">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="c6d5d-486">Türünde</span><span class="sxs-lookup"><span data-stu-id="c6d5d-486">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="c6d5d-487">Kategori</span><span class="sxs-lookup"><span data-stu-id="c6d5d-487">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="c6d5d-488">Çift</span><span class="sxs-lookup"><span data-stu-id="c6d5d-488">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="c6d5d-489">Yardımının</span><span class="sxs-lookup"><span data-stu-id="c6d5d-489">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="c6d5d-490">'İni</span><span class="sxs-lookup"><span data-stu-id="c6d5d-490">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="c6d5d-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="c6d5d-492">Sunuculu</span><span class="sxs-lookup"><span data-stu-id="c6d5d-492">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="c6d5d-493">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c6d5d-493">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="c6d5d-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="c6d5d-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="c6d5d-495">Uri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-495">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="c6d5d-496">Sürüm</span><span class="sxs-lookup"><span data-stu-id="c6d5d-496">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="c6d5d-497">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-497">Complex types</span></span>

<span data-ttu-id="c6d5d-498">Karmaşık bir türün bağlanması için ortak bir varsayılan Oluşturucusu ve ortak yazılabilir özellikleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-498">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="c6d5d-499">Model bağlama gerçekleştiğinde, sınıf ortak varsayılan Oluşturucu kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-499">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="c6d5d-500">Karmaşık türün her özelliği için model bağlama, ad modeli ön eki için kaynakları arar *. property_name*.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-500">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="c6d5d-501">Hiçbir şey bulunamazsa, ön ek olmadan yalnızca *property_name* arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-501">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="c6d5d-502">Bir parametreye bağlama için, önek parametre adıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-502">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="c6d5d-503">`PageModel` public özelliğine bağlama için, önek ortak özellik adıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-503">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="c6d5d-504">Bazı özniteliklerin, parametre veya özellik adının varsayılan kullanımını geçersiz kılabilmenizi sağlayan `Prefix` bir özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-504">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="c6d5d-505">Örneğin, karmaşık türün aşağıdaki `Instructor` sınıfı olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-505">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="c6d5d-506">Önek = parametre adı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-506">Prefix = parameter name</span></span>

<span data-ttu-id="c6d5d-507">Bağlanacak model `instructorToUpdate`adlı bir parametredir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-507">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="c6d5d-508">Model bağlama, anahtar `instructorToUpdate.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-508">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="c6d5d-509">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-509">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="c6d5d-510">Önek = Özellik adı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-510">Prefix = property name</span></span>

<span data-ttu-id="c6d5d-511">Bağlanacak model, denetleyicinin veya `PageModel` sınıfının `Instructor` adlı bir özelliktir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-511">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="c6d5d-512">Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-512">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="c6d5d-513">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-513">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="c6d5d-514">Özel ön ek</span><span class="sxs-lookup"><span data-stu-id="c6d5d-514">Custom prefix</span></span>

<span data-ttu-id="c6d5d-515">Bağlanacak model `instructorToUpdate` adlı bir parametredir ve `Bind` özniteliği önek olarak `Instructor` belirtir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-515">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="c6d5d-516">Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-516">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="c6d5d-517">Bu bulunamazsa, öneki olmayan `ID` arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-517">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="c6d5d-518">Karmaşık tür hedefleri için öznitelikler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-518">Attributes for complex type targets</span></span>

<span data-ttu-id="c6d5d-519">Karmaşık türlerin model bağlamasını denetlemek için birkaç yerleşik öznitelik mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-519">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="c6d5d-520">Bu öznitelikler, gönderilen form verileri değer kaynağı olduğunda model bağlamayı etkiler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-520">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="c6d5d-521">Bunlar, gönderilen JSON ve XML istek gövdelerini işleyen giriş formatlayıcıları 'nı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-521">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="c6d5d-522">Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-522">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="c6d5d-523">Ayrıca bkz. [model doğrulamasında](xref:mvc/models/validation#required-attribute)`[Required]` özniteliği tartışması.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-523">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="c6d5d-524">[BindRequired] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-524">[BindRequired] attribute</span></span>

<span data-ttu-id="c6d5d-525">, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-525">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="c6d5d-526">Modelin özelliği için bağlama gerçekleşmemişse model bağlamasının model durumu hatası eklemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-526">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="c6d5d-527">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-527">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="c6d5d-528">[Bindhiç] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-528">[BindNever] attribute</span></span>

<span data-ttu-id="c6d5d-529">, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-529">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="c6d5d-530">Model bağlamasının model özelliğini değiştirmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-530">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="c6d5d-531">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-531">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="c6d5d-532">[Bind] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-532">[Bind] attribute</span></span>

<span data-ttu-id="c6d5d-533">, Bir sınıfa veya yöntem parametresine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-533">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="c6d5d-534">Model bağlamasındaki bir modelin hangi özelliklerinin dahil edileceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-534">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="c6d5d-535">Aşağıdaki örnekte, herhangi bir işleyici veya eylem yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-535">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="c6d5d-536">Aşağıdaki örnekte, `OnPost` yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-536">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="c6d5d-537">`[Bind]` özniteliği, *oluşturma* senaryolarında fazla nakline karşı korumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-537">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="c6d5d-538">Dışlanan Özellikler null ya da boş değer olarak ayarlandığı için, düzenleme senaryolarında iyi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-538">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="c6d5d-539">Fazla nakline karşı savunma için, `[Bind]` özniteliği yerine, görüntüleme modelleri önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-539">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="c6d5d-540">Daha fazla bilgi için bkz. fazla [nakil hakkında güvenlik NOI](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-540">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="c6d5d-541">Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-541">Collections</span></span>

<span data-ttu-id="c6d5d-542">Basit türlerin koleksiyonları olan hedefler için model bağlama, *parameter_name* veya *property_name*ile eşleşmeleri arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-542">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="c6d5d-543">Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-543">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="c6d5d-544">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-544">For example:</span></span>

* <span data-ttu-id="c6d5d-545">Bağlanacak parametrenin `selectedCourses`adlı bir dizi olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-545">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="c6d5d-546">Form veya sorgu dizesi verileri aşağıdaki biçimlerden birinde olabilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-546">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="c6d5d-547">Aşağıdaki biçim yalnızca form verilerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-547">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="c6d5d-548">Önceki örnek biçimlerinin hepsi için model bağlama iki öğe dizisini `selectedCourses` parametresine geçirir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-548">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="c6d5d-549">Selectedkurslar [0] = 1050</span><span class="sxs-lookup"><span data-stu-id="c6d5d-549">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="c6d5d-550">Selectedkurslar [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="c6d5d-550">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="c6d5d-551">Alt simge numaralarını kullanan veri biçimleri (... [0]... [1]...) sıfırdan başlayarak sıralı olarak numaralandırıldıklarından emin olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-551">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="c6d5d-552">Alt simge numaralandırmasında boşluk varsa, boşluklardan sonraki tüm öğeler yoksayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-552">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="c6d5d-553">Örneğin, alt simgeler 0 ve 1 yerine 0 ve 2 ise ikinci öğe yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-553">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="c6d5d-554">Sözlükler</span><span class="sxs-lookup"><span data-stu-id="c6d5d-554">Dictionaries</span></span>

<span data-ttu-id="c6d5d-555">`Dictionary` hedefler için, model bağlama *parameter_name* veya *property_name*eşleşmelerini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-555">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="c6d5d-556">Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-556">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="c6d5d-557">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-557">For example:</span></span>

* <span data-ttu-id="c6d5d-558">Hedef parametrenin `selectedCourses`adlı bir `Dictionary<int, string>` olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-558">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="c6d5d-559">Postalanan form veya sorgu dizesi verileri aşağıdaki örneklerden birine benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-559">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="c6d5d-560">Önceki örnek biçimlerinin hepsi için model bağlama iki öğenin sözlüğünü `selectedCourses` parametresine geçirir:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-560">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="c6d5d-561">Selectedkurslar ["1050"] = "Chemistry"</span><span class="sxs-lookup"><span data-stu-id="c6d5d-561">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="c6d5d-562">Selectedkurslar ["2000"] = "Ekonomiks"</span><span class="sxs-lookup"><span data-stu-id="c6d5d-562">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="c6d5d-563">Model bağlama yolu verileri ve sorgu dizeleri için Genelleştirme davranışı</span><span class="sxs-lookup"><span data-stu-id="c6d5d-563">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="c6d5d-564">ASP.NET Core yol değeri sağlayıcısı ve sorgu dizesi değer sağlayıcısı:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-564">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="c6d5d-565">Değerleri sabit kültür olarak değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-565">Treat values as invariant culture.</span></span>
* <span data-ttu-id="c6d5d-566">URL 'Lerin kültür sabiti olmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-566">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="c6d5d-567">Buna karşılık, form verilerinden gelen değerler kültüre duyarlı bir dönüştürmeye gider.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-567">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="c6d5d-568">Bu, URL 'Lerin yerel ayarlarda paylaşılabilir olması için tasarımdır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-568">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="c6d5d-569">ASP.NET Core yol değeri sağlayıcısını ve sorgu dizesi değeri sağlayıcısını, kültüre duyarlı bir dönüşüme dönüştürmek için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-569">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="c6d5d-570"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory> 'den devralma</span><span class="sxs-lookup"><span data-stu-id="c6d5d-570">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="c6d5d-571">[QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) veya [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs) adresinden kodu kopyalayın</span><span class="sxs-lookup"><span data-stu-id="c6d5d-571">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="c6d5d-572">Değer sağlayıcısı oluşturucusuna geçirilen [kültür değerini](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture) ile değiştirin</span><span class="sxs-lookup"><span data-stu-id="c6d5d-572">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="c6d5d-573">MVC seçeneklerinde varsayılan değer sağlayıcı fabrikasını yeni bir değerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-573">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="c6d5d-574">Özel veri türleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-574">Special data types</span></span>

<span data-ttu-id="c6d5d-575">Model bağlamanın işleyebileceği bazı özel veri türleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-575">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="c6d5d-576">Iformfile ve ıformfilecollection</span><span class="sxs-lookup"><span data-stu-id="c6d5d-576">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="c6d5d-577">HTTP isteğine eklenen karşıya yüklenen dosya.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-577">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="c6d5d-578">Ayrıca, birden çok dosya için `IEnumerable<IFormFile>` desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-578">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="c6d5d-579">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="c6d5d-579">CancellationToken</span></span>

<span data-ttu-id="c6d5d-580">Zaman uyumsuz denetleyicilerde etkinliği iptal etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-580">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="c6d5d-581">Form koleksiyonu</span><span class="sxs-lookup"><span data-stu-id="c6d5d-581">FormCollection</span></span>

<span data-ttu-id="c6d5d-582">Postalanan form verilerinden tüm değerleri almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-582">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="c6d5d-583">Giriş biçimleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-583">Input formatters</span></span>

<span data-ttu-id="c6d5d-584">İstek gövdesindeki veriler JSON, XML veya başka bir biçimde olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-584">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="c6d5d-585">Model bağlama, bu verileri ayrıştırmak için belirli bir içerik türünü işlemek üzere yapılandırılmış bir *giriş biçimlendiricisi* kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-585">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="c6d5d-586">Varsayılan olarak, ASP.NET Core JSON verilerini işlemek için JSON tabanlı giriş formatlayıcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-586">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="c6d5d-587">Diğer içerik türleri için başka biçim ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-587">You can add other formatters for other content types.</span></span>

<span data-ttu-id="c6d5d-588">ASP.NET Core, [tüketen](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliğine bağlı olarak giriş formatlayıcıları seçer.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-588">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="c6d5d-589">Hiçbir öznitelik yoksa, [Content-Type üst bilgisini](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-589">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="c6d5d-590">Yerleşik XML girişi formatlayıcıları 'nı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-590">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="c6d5d-591">`Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-591">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="c6d5d-592">`Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-592">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="c6d5d-593">`Consumes` özniteliğini, istek gövdesinde XML beklemeniz gereken denetleyici sınıflarına veya eylem yöntemlerine uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-593">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="c6d5d-594">Daha fazla bilgi için bkz. [XML serileştirme tanıtımı](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="c6d5d-594">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="c6d5d-595">Belirtilen türleri model bağlamalarından Dışla</span><span class="sxs-lookup"><span data-stu-id="c6d5d-595">Exclude specified types from model binding</span></span>

<span data-ttu-id="c6d5d-596">Model bağlama ve doğrulama sistemlerinin davranışı [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata)tarafından yönlendiriliyor.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-596">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="c6d5d-597">[Mvcoptions. Modelmetadatadetails sağlayıcılarına](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)bir ayrıntı sağlayıcısı ekleyerek `ModelMetadata` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-597">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="c6d5d-598">Yerleşik Ayrıntılar sağlayıcıları, belirtilen türler için model bağlamayı veya doğrulamayı devre dışı bırakmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-598">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="c6d5d-599">Belirtilen türdeki tüm modellerdeki model bağlamayı devre dışı bırakmak için `Startup.ConfigureServices`bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-599">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d5d-600">Örneğin, `System.Version`türündeki tüm modeller üzerinde model bağlamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-600">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="c6d5d-601">Belirtilen türdeki özelliklerde doğrulamayı devre dışı bırakmak için `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-601">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d5d-602">Örneğin, `System.Guid`türündeki özelliklerde doğrulamayı devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-602">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="c6d5d-603">Özel model ciltleri</span><span class="sxs-lookup"><span data-stu-id="c6d5d-603">Custom model binders</span></span>

<span data-ttu-id="c6d5d-604">Özel bir model cildi yazarak ve belirli bir hedef için seçmek üzere `[ModelBinder]` özniteliğini kullanarak model bağlamayı genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-604">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="c6d5d-605">[Özel model bağlama](xref:mvc/advanced/custom-model-binding)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-605">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="c6d5d-606">El ile model bağlama</span><span class="sxs-lookup"><span data-stu-id="c6d5d-606">Manual model binding</span></span>

<span data-ttu-id="c6d5d-607">Model bağlama, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi kullanılarak el ile çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-607">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="c6d5d-608">Yöntemi hem `ControllerBase` hem de `PageModel` sınıflarında tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-608">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="c6d5d-609">Yöntem aşırı yüklemeleri, kullanılacak öneki ve değer sağlayıcısını belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-609">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="c6d5d-610">Model bağlama başarısız olursa Yöntem `false` döndürür.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-610">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="c6d5d-611">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="c6d5d-611">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="c6d5d-612">[FromServices] özniteliği</span><span class="sxs-lookup"><span data-stu-id="c6d5d-612">[FromServices] attribute</span></span>

<span data-ttu-id="c6d5d-613">Bu özniteliğin adı, bir veri kaynağı belirten model bağlama özniteliklerinin düzeniyle uyar.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-613">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="c6d5d-614">Ancak bir değer sağlayıcısından veri bağlama hakkında bilgi yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-614">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="c6d5d-615">[Bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısından bir türün bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-615">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c6d5d-616">Amacı, yalnızca belirli bir yöntem çağrılırsa bir hizmete ihtiyacınız olduğunda Oluşturucu ekleme için bir alternatif sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="c6d5d-616">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6d5d-617">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6d5d-617">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
