---
title: "Özel biçimlendiricileri ASP.NET Core MVC Web API'leri"
author: tdykstra
description: "Oluşturmayı ve web ASP.NET Core API'leri için özel biçimlendiricileri kullanmayı öğrenin."
keywords: "ASP.NET Core, web API, özel biçimlendiricileri"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="31f8b-104">Özel biçimlendiricileri ASP.NET Core MVC Web API'leri</span><span class="sxs-lookup"><span data-stu-id="31f8b-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="31f8b-105">tarafından [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="31f8b-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="31f8b-106">ASP.NET Core MVC web API'leri JSON, XML veya düz metin biçimlerini kullanarak veri değişimi için yerleşik destek sahip olur.</span><span class="sxs-lookup"><span data-stu-id="31f8b-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="31f8b-107">Bu makalede, özel biçimlendiricileri oluşturarak ek biçimleri için destek eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="31f8b-108">[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="31f8b-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="31f8b-109">Ne zaman özel biçimlendiricileri kullanılır</span><span class="sxs-lookup"><span data-stu-id="31f8b-109">When to use custom formatters</span></span>

<span data-ttu-id="31f8b-110">İstediğiniz zaman özel bir biçimlendirici kullanmak [içerik anlaşması](xref:mvc/models/formatting) yerleşik biçimlendiricileri (JSON, XML ve düz metin) tarafından desteklenmeyen bir içerik türü desteklemek için işlem.</span><span class="sxs-lookup"><span data-stu-id="31f8b-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="31f8b-111">Örneğin, bazı web API için istemci işleyebiliyorsa [Protobuf](https://github.com/google/protobuf) biçimi, daha etkili olduğundan Protobuf istemcilerle kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="31f8b-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="31f8b-112">Veya kişi adları ve adresleri göndermek için web API isteyebilirsiniz [vCard](https://wikipedia.org/wiki/VCard) biçimi, kişi veri değişimi için sık kullanılan biçim.</span><span class="sxs-lookup"><span data-stu-id="31f8b-112">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="31f8b-113">Bu makalede sağlanan örnek uygulamayı basit vCard biçimlendirici uygular.</span><span class="sxs-lookup"><span data-stu-id="31f8b-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="31f8b-114">Özel bir biçimlendirici kullanma genel bakış</span><span class="sxs-lookup"><span data-stu-id="31f8b-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="31f8b-115">Özel bir biçimlendirici oluşturmak için adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="31f8b-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="31f8b-116">İstemciye gönderilecek verilerini seri hale getirmek istiyorsanız, bir çıktı biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="31f8b-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="31f8b-117">İstemciden alınan verileri seri durumdan istiyorsanız, bir giriş biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="31f8b-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="31f8b-118">Örnek, biçimlendiricilerin eklemek `InputFormatters` ve `OutputFormatters` koleksiyonlarda [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="31f8b-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="31f8b-119">Aşağıdaki bölümler bu adımların her biri için yönergeler ve kod örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="31f8b-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="31f8b-120">Özel biçimlendirici sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="31f8b-120">How to create a custom formatter class</span></span>

<span data-ttu-id="31f8b-121">Bir biçimlendirici oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="31f8b-121">To create a formatter:</span></span>

* <span data-ttu-id="31f8b-122">Sınıf uygun temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="31f8b-123">Geçerli bir medya türlerini ve Kodlamalar oluşturucuda belirtin.</span><span class="sxs-lookup"><span data-stu-id="31f8b-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="31f8b-124">Geçersiz kılma `CanReadType` / `CanWriteType` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="31f8b-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="31f8b-125">Geçersiz kılma `ReadRequestBodyAsync` / `WriteResponseBodyAsync` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="31f8b-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="31f8b-126">Uygun temel sınıfından türetilir</span><span class="sxs-lookup"><span data-stu-id="31f8b-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="31f8b-127">Metin medya türleri için (örneğin, vCard) türetilen [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) veya [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="31f8b-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="31f8b-128">İkili türleri için türetilen [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) veya [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="31f8b-128">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="31f8b-129">Geçerli bir medya türlerini ve Kodlamalar belirtin</span><span class="sxs-lookup"><span data-stu-id="31f8b-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="31f8b-130">Ekleyerek oluşturucuda geçerli bir medya türleri ve Kodlamalar belirtin `SupportedMediaTypes` ve `SupportedEncodings` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="31f8b-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="31f8b-131">Bir biçimlendirici sınıf oluşturucu bağımlılık ekleme yapamazsınız.</span><span class="sxs-lookup"><span data-stu-id="31f8b-131">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="31f8b-132">Örneğin, oluşturucuya bir Günlükçü parametresini ekleyerek bir Günlükçü alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="31f8b-132">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="31f8b-133">Hizmetlere erişmek için yönteme geçirilen context nesnesi kullanmak zorunda.</span><span class="sxs-lookup"><span data-stu-id="31f8b-133">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="31f8b-134">Kod örneği [aşağıda](#read-write) bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-134">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="31f8b-135">CanReadType/CanWriteType geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="31f8b-135">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="31f8b-136">İçine seri durumdan ya da geçersiz kılarak öğesinden seri türünü belirtmek `CanReadType` veya `CanWriteType` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="31f8b-136">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="31f8b-137">Örneğin, yalnızca vCard metinden oluşturmak mümkün olabilir bir `Contact` türü tersi.</span><span class="sxs-lookup"><span data-stu-id="31f8b-137">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="31f8b-138">CanWriteResult yöntemi</span><span class="sxs-lookup"><span data-stu-id="31f8b-138">The CanWriteResult method</span></span>

<span data-ttu-id="31f8b-139">Bazı senaryolarda geçersiz kılmak zorunda `CanWriteResult` yerine `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="31f8b-139">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="31f8b-140">Kullanım `CanWriteResult` aşağıdaki koşullar doğruysa:</span><span class="sxs-lookup"><span data-stu-id="31f8b-140">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="31f8b-141">Eylem yöntemi bir model sınıfı döndürür.</span><span class="sxs-lookup"><span data-stu-id="31f8b-141">Your action method returns a model class.</span></span>
  * <span data-ttu-id="31f8b-142">Çalışma zamanında döndürülen türetilmiş sınıfları vardır.</span><span class="sxs-lookup"><span data-stu-id="31f8b-142">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="31f8b-143">Sınıfı eylem tarafından döndürülen türetilmiş çalışma zamanında bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-143">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="31f8b-144">Örneğin, eylem yöntemi imzası döndürür varsayalım bir `Person` türü, ancak döndürebilir bir `Student` veya `Instructor` türetilen tür `Person`.</span><span class="sxs-lookup"><span data-stu-id="31f8b-144">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="31f8b-145">Yalnızca işlemek için biçimlendirici istiyorsanız `Student` nesneleri denetleyin türünü [nesne](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) için sağlanan context nesnesi içinde `CanWriteResult` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="31f8b-145">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="31f8b-146">Kullanmak için gerekli olmadığını göz önünde bulundurun `CanWriteResult` eylem yönteminin döndüğünde `IActionResult`; bu durumda, `CanWriteType` yöntemi çalışma zamanı türü alır.</span><span class="sxs-lookup"><span data-stu-id="31f8b-146">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="31f8b-147">ReadRequestBodyAsync/WriteResponseBodyAsync geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="31f8b-147">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="31f8b-148">Seri durumdan veya içinde seri hale getirme gerçek yapması `ReadRequestBodyAsync` veya `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="31f8b-148">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="31f8b-149">Aşağıdaki örnekte vurgulanmış satırlar (bunları Oluşturucusu parametrelerinden alınamıyor) bağımlılık ekleme kapsayıcısını Hizmetleri verimli şekilde nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-149">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="31f8b-150">MVC özel bir biçimlendirici kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="31f8b-150">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="31f8b-151">Özel bir biçimlendirici kullanmak için biçimlendirici sınıfının bir örneğini ekleme `InputFormatters` veya `OutputFormatters` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="31f8b-151">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="31f8b-152">Biçimlendiricileri ekledikten sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="31f8b-152">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="31f8b-153">Birinci öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="31f8b-153">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="31f8b-154">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="31f8b-154">Next steps</span></span>

<span data-ttu-id="31f8b-155">Bkz: [örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), uygulayan basit vCard giriş ve çıkış biçimlendiricileri.</span><span class="sxs-lookup"><span data-stu-id="31f8b-155">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="31f8b-156">Uygulama okur ve aşağıdaki gibi görünen vCard Yazar:</span><span class="sxs-lookup"><span data-stu-id="31f8b-156">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="31f8b-157">VCard çıktı, uygulamayı çalıştırın ve başlığı "metin/vcard" Get isteğini kabul et ile göndermek için görmek için `http://localhost:63313/api/contacts/` (Visual Studio'dan çalışırken) veya `http://localhost:5000/api/contacts/` (komut satırından çalışırken).</span><span class="sxs-lookup"><span data-stu-id="31f8b-157">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="31f8b-158">VCard kişiler bellek içi koleksiyona eklemek için yukarıdaki örnekteki gibi biçimlendirilmiş gövdesinde vCard metin ile Content-Type üstbilgisi "metin/vcard" ile aynı URL bir Post isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="31f8b-158">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
