---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API bağlama parametresi | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377815"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="0e197-102">ASP.NET Web API bağlama parametresi</span><span class="sxs-lookup"><span data-stu-id="0e197-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="0e197-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0e197-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0e197-104">Web API denetleyicisi üzerinde bir yöntemi çağırdığında, adlı bir işlem parametreleri için değer ayarlamalısınız *bağlama*.</span><span class="sxs-lookup"><span data-stu-id="0e197-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="0e197-105">Bu makalede, Web API'si parametreleri nasıl bağlar ve bağlama işlemi nasıl özelleştirebileceğiniz açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0e197-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="0e197-106">Varsayılan olarak, Web API'si parametleri bağlamak için aşağıdaki kuralları kullanır:</span><span class="sxs-lookup"><span data-stu-id="0e197-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="0e197-107">"Basit" bir tür parametresi olduğundan, Web API'si URI'SİNDEN değerini almak çalışır.</span><span class="sxs-lookup"><span data-stu-id="0e197-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="0e197-108">Basit türler .NET [ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **çift**, vb.), artı **TimeSpan**, **DateTime**, **GUID**, **ondalık**, ve **dize**, *artı* herhangi bir dizeden dönüştürebilirsiniz bir tür dönüştürücüsü yazın.</span><span class="sxs-lookup"><span data-stu-id="0e197-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="0e197-109">(Hakkında daha fazla daha sonra tür dönüştürücüleri.)</span><span class="sxs-lookup"><span data-stu-id="0e197-109">(More about type converters later.)</span></span>
- <span data-ttu-id="0e197-110">Karmaşık türler, Web API'si çalıştığı için ileti gövdesi okunamadı kullanarak bir [medya türü biçimlendiricisi](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="0e197-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="0e197-111">Örneğin, tipik bir Web API denetleyicisi yöntemi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="0e197-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="0e197-112">*Kimliği* parametresi bir &quot;basit&quot; yazın, böylece Web API'si istek URI'SİNDEN değeri almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="0e197-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="0e197-113">*Öğesi* parametre değeri gövdeden okunacak medya türü biçimlendiricisi Web API'si kullanan karmaşık bir tür olduğundan.</span><span class="sxs-lookup"><span data-stu-id="0e197-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="0e197-114">Web API URI'SİNDEN bir değer almak için rota verilerini ve URI sorgu dizesi içinde arar.</span><span class="sxs-lookup"><span data-stu-id="0e197-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="0e197-115">Yönlendirme sistem URI ayrıştırırken bir rota için eşleşen bir rota verilerini doldurulur.</span><span class="sxs-lookup"><span data-stu-id="0e197-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="0e197-116">Daha fazla bilgi için [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="0e197-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="0e197-117">Bu makalenin kalanında miyim model bağlama işleminden nasıl özelleştirebileceğiniz göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="0e197-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="0e197-118">Karmaşık türler için ancak, mümkün olduğunda medya türü biçimlendiricileri kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="0e197-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="0e197-119">Bir anahtar HTTP kaynakları kaynağın gösterimini belirtmek için içerik anlaşması kullanılarak ileti gövdesinde gönderildiğinden emin ilkesidir.</span><span class="sxs-lookup"><span data-stu-id="0e197-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="0e197-120">Medya türü biçimlendiricileri tam olarak bu amaç için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0e197-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="0e197-121">[FromUri] kullanma</span><span class="sxs-lookup"><span data-stu-id="0e197-121">Using [FromUri]</span></span>

<span data-ttu-id="0e197-122">Web API'si, bir karmaşık tür URI'SİNDEN okumaya zorlamak için ekleme **[FromUri]** özniteliği için parametre.</span><span class="sxs-lookup"><span data-stu-id="0e197-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="0e197-123">Aşağıdaki örnekte tanımlayan bir `GeoPoint` birlikte alır bir denetleyici yöntemi türü `GeoPoint` URI.</span><span class="sxs-lookup"><span data-stu-id="0e197-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="0e197-124">İstemci enlem ve boylam değerleri sorgu dizesinde yerleştirebilir ve Web API bunları oluşturmak için kullanacağı bir `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="0e197-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="0e197-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0e197-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="0e197-126">[FromBody] kullanma</span><span class="sxs-lookup"><span data-stu-id="0e197-126">Using [FromBody]</span></span>

<span data-ttu-id="0e197-127">Web API'si, bir basit türü gövdeden okunacak zorlamak için ekleme **[FromBody]** özniteliği için parametre:</span><span class="sxs-lookup"><span data-stu-id="0e197-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="0e197-128">Bu örnekte, değerini okumak için bir medya türü biçimlendiricisi Web API kullanacağı *adı* istek gövdesinden.</span><span class="sxs-lookup"><span data-stu-id="0e197-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="0e197-129">İşte bir örnek istemci isteği.</span><span class="sxs-lookup"><span data-stu-id="0e197-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="0e197-130">Web API'si Content-Type üstbilgisi [FromBody] bir parametreye sahip olduğunda, bir biçimlendirici seçmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0e197-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="0e197-131">Bu örnekte, içerik türü olduğundan &quot;application/json&quot; ve istek gövdesi ham bir JSON dizesi (JSON nesnesi değil).</span><span class="sxs-lookup"><span data-stu-id="0e197-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="0e197-132">En fazla bir parametre, ileti gövdeden okuma izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0e197-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="0e197-133">Bu nedenle bu çalışmaz:</span><span class="sxs-lookup"><span data-stu-id="0e197-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="0e197-134">Bu kural için istek gövdesi yalnızca bir kez okunabilir olmayan arabelleğe alınan bir akış içinde depolanabilir nedenidir.</span><span class="sxs-lookup"><span data-stu-id="0e197-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="0e197-135">Tür dönüştürücüleri</span><span class="sxs-lookup"><span data-stu-id="0e197-135">Type Converters</span></span>

<span data-ttu-id="0e197-136">Web API (Web API URI'SİNDEN bağlama dener böylece) bir sınıfı basit bir tür olarak davranmak oluşturarak yapabileceğiniz bir **TypeConverter** ve dize dönüştürme sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e197-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="0e197-137">Aşağıdaki kodda gösterildiği bir `GeoPoint` coğrafi bir noktayı temsil eden sınıf yanı sıra **TypeConverter** dönüştüren için dizelerden `GeoPoint` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="0e197-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="0e197-138">`GeoPoint` Sınıfı ile donatılmış bir **[TypeConverter]** tür dönüştürücüsünü belirtmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0e197-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="0e197-139">(Bu örnekte Mike kabini'nın blog gönderisi tarafından ilham alan [MVC/Webapı eylem imzaları özel nesneler bağlama nasıl](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="0e197-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="0e197-140">Web API değerlendirir artık `GeoPoint` basit bir tür bağlamak çalışır, yani `GeoPoint` URI parametreleri.</span><span class="sxs-lookup"><span data-stu-id="0e197-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="0e197-141">Eklemeniz gerekmez **[FromUri]** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="0e197-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="0e197-142">İstemci, şöyle bir URI yöntemi çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0e197-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="0e197-143">Model bağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="0e197-143">Model Binders</span></span>

<span data-ttu-id="0e197-144">Özel bir model bağlayıcısını oluşturmak için bir tür dönüştürücüsü olandan daha esnek bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="0e197-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="0e197-145">Bir model Bağlayıcısı ile HTTP isteği, eylem açıklaması ve ham değerler gibi rota verileri erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e197-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="0e197-146">Bir model bağlayıcı oluşturmak için uygulaması **IModelBinder** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="0e197-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="0e197-147">Bu arabirim, tek bir yöntemi tanımlar **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="0e197-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="0e197-148">Bir model bağlayıcı için işte `GeoPoint` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="0e197-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="0e197-149">Ham giriş değerlerini bir model bağlayıcı alır bir *değer sağlayıcısındaki*.</span><span class="sxs-lookup"><span data-stu-id="0e197-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="0e197-150">Bu tasarım, iki ayrı işlevlerin ayırır:</span><span class="sxs-lookup"><span data-stu-id="0e197-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="0e197-151">Değer sağlayıcı, HTTP isteği alır ve anahtar-değer çiftlerinin dictionary'si doldurur.</span><span class="sxs-lookup"><span data-stu-id="0e197-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="0e197-152">Model bağlayıcı Bu sözlük model doldurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0e197-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="0e197-153">Varsayılan değer sağlayıcı Web API'si, rota verilerini ve sorgu dizesi değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="0e197-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="0e197-154">Örneğin, URI ise `http://localhost/api/values/1?location=48,-122`, aşağıdaki anahtar-değer çiftleri değer sağlayıcı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0e197-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="0e197-155">kimlik = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="0e197-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="0e197-156">konumu = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="0e197-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="0e197-157">(Olan varsayılan rota şablonu varsayılarak &quot;API / {denetleyici} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="0e197-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="0e197-158">Bağlanacak parametre adını depolanan **ModelBindingContext.ModelName** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0e197-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="0e197-159">Model Bağlayıcısı sözlüğündeki bu değere sahip bir anahtar arar.</span><span class="sxs-lookup"><span data-stu-id="0e197-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="0e197-160">Değer varsa ve metne dönüştürülecek bir `GeoPoint`, model bağlayıcı ilişkili değeri atar **ModelBindingContext.Model** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0e197-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="0e197-161">Model bağlayıcı için bir basit tür dönüştürme sınırlı olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="0e197-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="0e197-162">Bu örnekte, model bağlayıcı önce bir tabloda bilinen konumları arar ve başarısız olursa, tür dönüştürme kullanır.</span><span class="sxs-lookup"><span data-stu-id="0e197-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="0e197-163">**Model bağlayıcı ayarlama**</span><span class="sxs-lookup"><span data-stu-id="0e197-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="0e197-164">Bir model bağlayıcısını ayarlamak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="0e197-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="0e197-165">İlk olarak ekleyebileceğiniz bir **[çoğaltan ModelBinder]** özniteliği için parametre.</span><span class="sxs-lookup"><span data-stu-id="0e197-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="0e197-166">Ayrıca bir **[çoğaltan ModelBinder]** öznitelik türü için.</span><span class="sxs-lookup"><span data-stu-id="0e197-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="0e197-167">Web API'si, bu türdeki tüm parametreler için belirtilen bir model bağlayıcı kullanır.</span><span class="sxs-lookup"><span data-stu-id="0e197-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="0e197-168">Son olarak, bir model Bağlayıcısı sağlayıcısına ekleyebilirsiniz **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="0e197-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="0e197-169">Yalnızca bir model bağlayıcı oluşturan bir Üreteç sınıfını bir model bağlayıcı sağlayıcısıdır.</span><span class="sxs-lookup"><span data-stu-id="0e197-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="0e197-170">Türetilen bir sağlayıcı oluşturabilirsiniz [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0e197-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="0e197-171">Tek bir tür, model bağlayıcı işler, ancak, yerleşik kullanımı daha kolay olur **SimpleModelBinderProvider**, bu amaç için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0e197-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="0e197-172">Aşağıdaki kod, bunun nasıl yapılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0e197-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="0e197-173">Bir model bağlama sağlayıcısı ile eklemek yine **[çoğaltan ModelBinder]** özniteliği için parametre Web API'si, bir model bağlayıcı ve medya türü biçimlendiricisi kullanmalısınız bildirmek için.</span><span class="sxs-lookup"><span data-stu-id="0e197-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="0e197-174">Ancak artık öznitelik, model bağlayıcı türünü belirtmeniz gerekmez:</span><span class="sxs-lookup"><span data-stu-id="0e197-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="0e197-175">Değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="0e197-175">Value Providers</span></span>

<span data-ttu-id="0e197-176">Bir model bağlayıcı değerlerini sağlayıcıdan bir değer alır bahsetmiştim.</span><span class="sxs-lookup"><span data-stu-id="0e197-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="0e197-177">Özel değer sağlayıcı yazma için uygulayan **IValueProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="0e197-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="0e197-178">İstekte tanımlama bilgisi değerleri çeken bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0e197-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="0e197-179">Ayrıca bir değer sağlayıcı üreteci türeterek oluşturmak için ihtiyacınız **ValueProviderFactory** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0e197-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="0e197-180">Değer sağlayıcı üreteci için ekleme **HttpConfiguration** gibi.</span><span class="sxs-lookup"><span data-stu-id="0e197-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="0e197-181">Web API ölçeklemesini tüm değer sağlayıcıları, bu nedenle bir model bağlayıcı çağırdığında **ValueProvider.GetValue**, model bağlayıcı bunu oluşturabildiği ilk değer sağlayıcısında değerini alır.</span><span class="sxs-lookup"><span data-stu-id="0e197-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="0e197-182">Parametre düzeyinde kullanarak değer sağlayıcı üreteci alternatif olarak, ayarlayabilirsiniz **valueprovider'ın** özniteliğini aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="0e197-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="0e197-183">Bu, Web API ile belirtilen değer sağlayıcı üreteci model bağlama kullanın ve herhangi bir kayıtlı değer sağlayıcıları kullanmayacak şekilde bildirir.</span><span class="sxs-lookup"><span data-stu-id="0e197-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="0e197-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="0e197-184">HttpParameterBinding</span></span>

<span data-ttu-id="0e197-185">Model bağlayıcıları daha genel bir mekanizması belirli bir örneğini ' dir.</span><span class="sxs-lookup"><span data-stu-id="0e197-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="0e197-186">Bakarsanız **[çoğaltan ModelBinder]** özniteliği, Özet türetilen görürsünüz **ParameterBindingAttribute** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0e197-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="0e197-187">Bu sınıf, tek bir yöntemi tanımlar **GetBinding**, döndüren bir **HttpParameterBinding** nesnesi:</span><span class="sxs-lookup"><span data-stu-id="0e197-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="0e197-188">Bir **HttpParameterBinding** bir değere bir parametre bağlama için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="0e197-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="0e197-189">Durumunda, **[çoğaltan ModelBinder]**, öznitelik döndürür bir **HttpParameterBinding** kullanan uygulaması bir **IModelBinder** gerçek bağlama gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="0e197-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="0e197-190">Ayrıca kendi uygulayabilirsiniz **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="0e197-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="0e197-191">Örneğin, gelen Etag'ler almak istediğiniz varsayalım `if-match` ve `if-none-match` istekteki üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0e197-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="0e197-192">Etag'ler temsil etmek için bir sınıf tanımlayarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="0e197-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="0e197-193">Biz de Etag'den almak gösteren numaralandırmaya tanımlarsınız `if-match` üst bilgi veya `if-none-match` başlığı.</span><span class="sxs-lookup"><span data-stu-id="0e197-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="0e197-194">İşte bir **HttpParameterBinding** ETag istenen başlığından alır ve ETag türünde bir parametre için bağlar:</span><span class="sxs-lookup"><span data-stu-id="0e197-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="0e197-195">**ExecuteBindingAsync** yöntemi bağlama yapar.</span><span class="sxs-lookup"><span data-stu-id="0e197-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="0e197-196">Bu yöntem içinde bağlı bir parametre değerine ekleyin **ActionArgument** sözlüğünde **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="0e197-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="0e197-197">Varsa, **ExecuteBindingAsync** yöntemi okur istek iletisinin gövdesi, geçersiz kılma **WillReadBody** özelliği true döndürür.</span><span class="sxs-lookup"><span data-stu-id="0e197-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="0e197-198">İstek gövdesi, Web API'sini bir kural, en fazla bir bağlama uygular, böylece yalnızca bir kez okunabilir arabellekten çıkarılan bir akışı, ileti gövdesi okuyabilirsiniz olabilir.</span><span class="sxs-lookup"><span data-stu-id="0e197-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="0e197-199">Özel bir uygulamaya **HttpParameterBinding**, türetilen bir öznitelik tanımlayabilirsiniz **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="0e197-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="0e197-200">İçin `ETagParameterBinding`, için iki öznitelik tanımlarız `if-match` üstbilgileri, diğeri de `if-none-match` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="0e197-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="0e197-201">Her ikisi de, soyut bir temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="0e197-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="0e197-202">İşte kullanan bir denetleyici yöntem `[IfNoneMatch]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0e197-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="0e197-203">Yanında **ParameterBindingAttribute**, özel bir eklemek için başka bir kanca yoktur **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="0e197-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="0e197-204">Üzerinde **HttpConfiguration** nesnesi **ParameterBindingRules** özelliği, türü anomymous işlevleri koleksiyonudur (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="0e197-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="0e197-205">Örneğin, herhangi bir ETag parametre GET yöntemini kullanan bir kural ekleyebilirsiniz `ETagParameterBinding` ile `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="0e197-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="0e197-206">İşlev döndürmelidir `null` parametrelerinin nereye bağlama uygulanabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="0e197-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="0e197-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="0e197-207">IActionValueBinder</span></span>

<span data-ttu-id="0e197-208">Parametre bağlamasını sürecinin tamamını takılabilir bir hizmet tarafından denetlenir **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="0e197-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="0e197-209">Varsayılan uygulaması **IActionValueBinder** şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="0e197-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="0e197-210">Aranan bir **ParameterBindingAttribute** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="0e197-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="0e197-211">Bu içerir **[FromBody]**, **[FromUri]**, ve **[çoğaltan ModelBinder]**, ya da özel öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="0e197-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="0e197-212">Aksi halde, konum **HttpConfiguration.ParameterBindingRules** null olmayan bir döndüren bir işlev için **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="0e197-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="0e197-213">Aksi takdirde, daha önce açıklanan varsayılan kuralları kullanın.</span><span class="sxs-lookup"><span data-stu-id="0e197-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="0e197-214">Parametre türü "Basit" veya bir tür dönüştürücüsü varsa URI'SİNDEN bağlayın.</span><span class="sxs-lookup"><span data-stu-id="0e197-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="0e197-215">Bu almaya eşdeğerdir **[FromUri]** parametre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0e197-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="0e197-216">Aksi takdirde, iletinin gövdesinden parametresi okunacak deneyin.</span><span class="sxs-lookup"><span data-stu-id="0e197-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="0e197-217">Bu almaya eşdeğerdir **[FromBody]** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="0e197-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="0e197-218">İsterseniz, tüm yerini alabilir **IActionValueBinder** hizmeti ile özel bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="0e197-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e197-219">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0e197-219">Additional Resources</span></span>

[<span data-ttu-id="0e197-220">Özel parametre bağlama örneği</span><span class="sxs-lookup"><span data-stu-id="0e197-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="0e197-221">Mike kabini Web API'si parametre bağlama hakkında blog gönderilerini iyi bir dizi yazdı:</span><span class="sxs-lookup"><span data-stu-id="0e197-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="0e197-222">Parametre bağlama Web API'sini nasıl yaptığını</span><span class="sxs-lookup"><span data-stu-id="0e197-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="0e197-223">Web API'si için MVC stili parametre bağlaması</span><span class="sxs-lookup"><span data-stu-id="0e197-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="0e197-224">Eylem imzalarında MVC/Web API'de özel nesnelere bağlama</span><span class="sxs-lookup"><span data-stu-id="0e197-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="0e197-225">Web API'de özel değer sağlayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="0e197-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="0e197-226">Web API'de parametre bağlama başlık altında</span><span class="sxs-lookup"><span data-stu-id="0e197-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
