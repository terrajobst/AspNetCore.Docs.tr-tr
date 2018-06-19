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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042436"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="b0f8d-102">ASP.NET Web API bağlama parametresi</span><span class="sxs-lookup"><span data-stu-id="b0f8d-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b0f8d-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0f8d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b0f8d-104">Web API denetleyicisi üzerinde bir yöntem çağırdığında adlı bir işlem parametre değerlerini ayarlamalısınız *bağlama*.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="b0f8d-105">Bu makalede, Web API parametreleri nasıl bağlar ve bağlama işlemi nasıl özelleştirebileceğiniz açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="b0f8d-106">Varsayılan olarak, Web API parametreleri bağlamak için aşağıdaki kuralları kullanır:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="b0f8d-107">Parametre "Basit" bir tür ise, Web API URI'den değeri almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="b0f8d-108">Basit türler .NET [ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **çift**, vb.), artı **TimeSpan**, **DateTime**, **GUID**, **ondalık**, ve **dize**, *artı* herhangi bir dizeden dönüştürebilirsiniz türü dönüştürücü ile yazın.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="b0f8d-109">(Hakkında daha fazla daha sonra tür dönüştürücüleri.)</span><span class="sxs-lookup"><span data-stu-id="b0f8d-109">(More about type converters later.)</span></span>
- <span data-ttu-id="b0f8d-110">Karmaşık türler, Web API çalışır için ileti gövdesinden değerini okumaya kullanarak bir [medya türü biçimlendiricisi](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="b0f8d-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="b0f8d-111">Örneğin, tipik bir Web API denetleyicisi yöntemi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="b0f8d-112">*Kimliği* parametresi bir &quot;basit&quot; Web API isteğin URI değeri almaya çalıştığında şekilde yazın.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="b0f8d-113">*Öğesi* parametredir karmaşık bir tür böylece Web API isteği gövdesinden değerini okumak için bir medya türü biçimlendiricisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="b0f8d-114">URI'den bir değer almak için Web API rota verilerini ve URI sorgu dizesi arar.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="b0f8d-115">Yönlendirme sistem URI ayrıştırırken bir rotayla eşleşen rota verilerini doldurulur.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="b0f8d-116">Daha fazla bilgi için bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="b0f8d-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="b0f8d-117">Bu makalede kalan ı model bağlama işleminden nasıl özelleştirebileceğiniz göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="b0f8d-118">Karmaşık türler için Bununla birlikte, medya türü biçimlendiricilerini mümkün olduğunca kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="b0f8d-119">Bir anahtar HTTP kaynakları kaynak gösterimini belirtmek için içerik anlaşması kullanarak ileti gövdesini gönderilir ilkesidir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="b0f8d-120">Medya türü biçimlendiricilerini tam olarak bu amaç için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="b0f8d-121">[FromUri] kullanma</span><span class="sxs-lookup"><span data-stu-id="b0f8d-121">Using [FromUri]</span></span>

<span data-ttu-id="b0f8d-122">Karmaşık bir tür URI'den okumak için Web API zorlamak için add **[FromUri]** özniteliği parametresi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="b0f8d-123">Aşağıdaki örnek tanımlayan bir `GeoPoint` alır denetleyici yöntemi birlikte türü `GeoPoint` uri'den.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="b0f8d-124">İstemci sorgu dizesinde enlem ve boylam değerlerini koyabilirsiniz ve Web API kullanılacağını bunları oluşturmak için bir `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="b0f8d-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="b0f8d-126">[FromBody] kullanma</span><span class="sxs-lookup"><span data-stu-id="b0f8d-126">Using [FromBody]</span></span>

<span data-ttu-id="b0f8d-127">Web API isteği gövdesinden basit bir tür okumaya zorlamak için add **[FromBody]** özniteliği için parametre:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="b0f8d-128">Bu örnekte, değerini okumak için bir medya türü biçimlendiricisi Web API kullanacağı *adı* isteği gövdesinden.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="b0f8d-129">Burada, örnek istemci isteği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="b0f8d-130">Web API Content-Type üstbilgisi [FromBody] bir parametreye sahip olduğunda, bir biçimlendirici seçmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="b0f8d-131">Bu örnekte, içerik türü olduğundan &quot;uygulama/json&quot; ve istek gövdesi ham JSON dizesi (JSON nesnesi değil).</span><span class="sxs-lookup"><span data-stu-id="b0f8d-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="b0f8d-132">En fazla bir parametre ileti gövdeden okuma izni yok.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="b0f8d-133">Bu nedenle bu çalışmaz:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="b0f8d-134">Bu kural için istek gövdesini yalnızca bir kez okunabilir olmayan arabelleğe alınan bir akışta depolanabilir nedenidir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="b0f8d-135">Tür dönüştürücüleri</span><span class="sxs-lookup"><span data-stu-id="b0f8d-135">Type Converters</span></span>

<span data-ttu-id="b0f8d-136">Web API (Web API URI'den bağlamak deneyecek böylece) bir sınıfı basit bir tür olarak davranmak oluşturarak yapabileceğiniz bir **Typeconverter'a** ve dize dönüştürme sağlama.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="b0f8d-137">Aşağıdaki kodda gösterildiği bir `GeoPoint` coğrafi noktasını temsil eden sınıf artı bir **Typeconverter'a** dönüştüren için dizelerden `GeoPoint` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="b0f8d-138">`GeoPoint` Sınıfı donatılmış ile bir **[Typeconverter'a]** öznitelik türü dönüştürücü belirtin.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="b0f8d-139">(Bu örnek tarafından CAN kabini'nın blog gönderisi yaratıcı [MVC/Webapı eylem imzalarında özel nesneleri bağlamak nasıl](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="b0f8d-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="b0f8d-140">Web API işler artık `GeoPoint` basit bir tür olarak bağlamak çalışır yani `GeoPoint` URI parametrelerinden.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="b0f8d-141">Eklenecek gerekmeyen **[FromUri]** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="b0f8d-142">İstemci şöyle bir URI ile yöntemi çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="b0f8d-143">Model Binders</span><span class="sxs-lookup"><span data-stu-id="b0f8d-143">Model Binders</span></span>

<span data-ttu-id="b0f8d-144">Özel bir model bağlayıcısını oluşturmak için tür dönüştürücüsünü olandan daha esnek bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="b0f8d-145">Bir model bağlayıcı ile HTTP isteği, eylem açıklaması ve ham değerler gibi rota verilerinden erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="b0f8d-146">Bir model bağlayıcısı oluşturmak için uygulaması **IModelBinder** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="b0f8d-147">Bu arabirim tek bir yöntemi tanımlayan **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="b0f8d-148">İçin bir model bağlayıcı işte `GeoPoint` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="b0f8d-149">Ham giriş değerleri bir model bağlayıcı alır bir *değer sağlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="b0f8d-150">Bu tasarım, iki farklı işlevler ayırır:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="b0f8d-151">Değer sağlayıcı HTTP isteğini alır ve anahtar-değer çiftleri sözlüğü doldurur.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="b0f8d-152">Model bağlayıcı Bu sözlük model doldurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="b0f8d-153">Web API varsayılan değer sağlayıcısında rota verilerini ve sorgu dizesi değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="b0f8d-154">Örneğin, URI ise `http://localhost/api/values/1?location=48,-122`, aşağıdaki anahtar-değer çiftleri değer sağlayıcısı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="b0f8d-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="b0f8d-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="b0f8d-156">konumu = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="b0f8d-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="b0f8d-157">(Olan varsayılan rota şablonu varsayılarak &quot;API / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="b0f8d-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="b0f8d-158">Bağlanacak parametre adını depolanan **ModelBindingContext.ModelName** özelliği.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="b0f8d-159">Model Bağlayıcısı sözlüğünde bu değer bir anahtarla arar.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="b0f8d-160">Değeri varsa ve metne dönüştürülecek bir `GeoPoint`, model bağlayıcı bağımlı değere atar **ModelBindingContext.Model** özelliği.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="b0f8d-161">Model bağlayıcı için bir basit tür dönüştürme sınırlı olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="b0f8d-162">Bu örnekte, model bağlayıcı ilk bilinen konumları tablosunda arar ve başarısız olursa, tür dönüştürme kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="b0f8d-163">**Model bağlayıcı ayarlama**</span><span class="sxs-lookup"><span data-stu-id="b0f8d-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="b0f8d-164">Bir model bağlayıcısını ayarlamak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="b0f8d-165">İlk olarak, ekleyebileceğiniz bir **[çoğaltan ModelBinder]** özniteliği parametresi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="b0f8d-166">Ayrıca ekleyebileceğiniz bir **[çoğaltan ModelBinder]** öznitelik türü.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="b0f8d-167">Web API, bu türdeki tüm parametreler için belirtilen model bağlayıcı kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="b0f8d-168">Son olarak, bir model bağlayıcı sağlayıcısı ekleyebilirsiniz **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="b0f8d-169">Bir model bağlayıcı sağlayıcısı bir model bağlayıcı oluşturan yalnızca bir Fabrika sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="b0f8d-170">Türetilen bir sağlayıcı oluşturabilirsiniz [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="b0f8d-171">Tek bir tür, model bağlayıcı işler, ancak bu yerleşik kullanmak daha kolay olur **SimpleModelBinderProvider**, bu amaç için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="b0f8d-172">Aşağıdaki kod bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="b0f8d-173">Bir model bağlama sağlayıcıyla hala eklemek istediğiniz **[çoğaltan ModelBinder]** parametresi, bir model bağlayıcı ve medya türü biçimlendiricisi kullanmalıdır Web API bildirmek için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="b0f8d-174">Ancak şimdi özniteliğinde model bağlayıcı türünü belirtmenize gerek yoktur:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="b0f8d-175">Değer sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b0f8d-175">Value Providers</span></span>

<span data-ttu-id="b0f8d-176">I bir model bağlayıcı değerleri sağlayıcıdan bir değer alır belirtiliyor.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="b0f8d-177">Bir özel değer sağlayıcı yazmak için uygulama **IValueProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="b0f8d-178">İstekte tanımlama bilgisi değerleri çeken örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="b0f8d-179">Ayrıca bir değer sağlayıcı üreteci türetme tarafından oluşturmanıza gerek **ValueProviderFactory** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="b0f8d-180">Değer sağlayıcı üreteci eklemek **HttpConfiguration** gibi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="b0f8d-181">Web API oluşturur tüm değer sağlayıcıları, böylece bir model bağlayıcı çağırdığında **ValueProvider.GetValue**, onu oluşturabildiği ilk değer sağlayıcıdan değeri model bağlayıcı alır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="b0f8d-182">Alternatif olarak, değer sağlayıcı üreteci parametre düzeyinde kullanarak ayarlayabileceğiniz **valueprovider'ın** özniteliğini aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="b0f8d-183">Bu Web API ile belirtilen değer sağlayıcı üreteci model bağlama kullanın ve herhangi bir kayıtlı değer sağlayıcıları kullanmayacak şekilde söyler.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="b0f8d-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="b0f8d-184">HttpParameterBinding</span></span>

<span data-ttu-id="b0f8d-185">Model bağlayıcıları daha genel bir mekanizması belirli bir örneği var.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="b0f8d-186">Bakarsanız **[çoğaltan ModelBinder]** özniteliği, Özet türetilen görürsünüz **ParameterBindingAttribute** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="b0f8d-187">Tek bir yöntem bu sınıfı tanımlayan **GetBinding**, döndüren bir **HttpParameterBinding** nesnesi:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="b0f8d-188">Bir **HttpParameterBinding** bir değere bir parametre bağlama için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="b0f8d-189">Durumunda **[çoğaltan ModelBinder]**, öznitelik döndüren bir **HttpParameterBinding** kullanan uygulaması bir **IModelBinder** gerçek bağlama gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="b0f8d-190">Ayrıca kendi uygulayabileceğiniz **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="b0f8d-191">Örneğin, gelen Etag'ler almak istediğinizi varsayalım `if-match` ve `if-none-match` istekteki üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="b0f8d-192">Etag'ler temsil etmek için bir sınıf tanımlayarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="b0f8d-193">Biz de Etag'den alma atlanmayacağını belirtmek için bir numaralandırma tanımlarsınız `if-match` üstbilgisi veya `if-none-match` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="b0f8d-194">Burada bir **HttpParameterBinding** , istenen başlığından ETag alır ve ETag türünde bir parametre için bağlar:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="b0f8d-195">**ExecuteBindingAsync** yöntemi bağlama yapar.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="b0f8d-196">İçinde bu yöntem, ilişkili parametre değerine eklemek **ActionArgument** sözlükte **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="b0f8d-197">Varsa, **ExecuteBindingAsync** yöntemi Okuma isteği ileti gövdesi, geçersiz kılma **WillReadBody** özelliği true döndürür.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="b0f8d-198">İstek gövdesini Web API'sini bir kural, en çok bir bağlama zorlar, böylece yalnızca bir kez okunabilir bir arabellekten çıkarılan akışı ileti gövdesi okuyabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="b0f8d-199">Özel bir uygulamak için **HttpParameterBinding**, türetilen bir öznitelik tanımlayabilirsiniz **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="b0f8d-200">İçin `ETagParameterBinding`, için iki öznitelik tanımlama olasılığınız yüksektir `if-match` üstbilgileri, diğeri de `if-none-match` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="b0f8d-201">Her ikisi de soyut taban sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="b0f8d-202">Kullanan bir denetleyici yöntem işte `[IfNoneMatch]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="b0f8d-203">Yanında **ParameterBindingAttribute**, özel bir eklemek için başka bir kanca yoktur **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="b0f8d-204">Üzerinde **HttpConfiguration** nesnesi **ParameterBindingRules** özellik türünün anomymous işlevlerinin koleksiyonudur (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="b0f8d-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="b0f8d-205">Örneğin, herhangi bir ETag parametre GET yöntemini kullanan bir kural ekleyebilirsiniz `ETagParameterBinding` ile `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="b0f8d-206">İşlev döndürmelidir `null` bağlama olduğu geçerli parametreler için.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="b0f8d-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="b0f8d-207">IActionValueBinder</span></span>

<span data-ttu-id="b0f8d-208">Tüm parametre bağlama işlemi takılabilir bir hizmet tarafından denetlenen **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="b0f8d-209">Varsayılan uygulaması **IActionValueBinder** şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="b0f8d-210">Aramak bir **ParameterBindingAttribute** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="b0f8d-211">Bu içerir **[FromBody]**, **[FromUri]**, ve **[çoğaltan ModelBinder]**, ya da özel öznitelikler.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="b0f8d-212">Aksi takdirde, konum **HttpConfiguration.ParameterBindingRules** null olmayan bir döndüren bir işlev için **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="b0f8d-213">Aksi takdirde, daha önce açıklanan varsayılan kuralları kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="b0f8d-214">Parametre türü "Basit" veya bir tür dönüştürücüsünü varsa, URI'den bağlayın.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="b0f8d-215">Bu koymak eşdeğerdir **[FromUri]** parametre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="b0f8d-216">Aksi takdirde, ileti gövdesinden parametresi okumaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="b0f8d-217">Bu koymak eşdeğerdir **[FromBody]** parametresi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="b0f8d-218">İsterseniz, tüm koyabilirsiniz **IActionValueBinder** özel bir uygulama hizmet.</span><span class="sxs-lookup"><span data-stu-id="b0f8d-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0f8d-219">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0f8d-219">Additional Resources</span></span>

[<span data-ttu-id="b0f8d-220">Özel parametre bağlama örneği</span><span class="sxs-lookup"><span data-stu-id="b0f8d-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="b0f8d-221">Can kabini blog gönderileri iyi bir dizi Web API parametre bağlama hakkında yazdı:</span><span class="sxs-lookup"><span data-stu-id="b0f8d-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="b0f8d-222">Parametre bağlama Web API'sini nasıl yapar</span><span class="sxs-lookup"><span data-stu-id="b0f8d-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="b0f8d-223">Web API için MVC stili parametre bağlama</span><span class="sxs-lookup"><span data-stu-id="b0f8d-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="b0f8d-224">MVC/Web API eylem imzalarında özel nesneleri bağlamak nasıl</span><span class="sxs-lookup"><span data-stu-id="b0f8d-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="b0f8d-225">Bir özel değer sağlayıcı Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="b0f8d-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="b0f8d-226">Başlık altında Web API parametre bağlama</span><span class="sxs-lookup"><span data-stu-id="b0f8d-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
