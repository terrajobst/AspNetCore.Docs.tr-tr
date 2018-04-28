---
title: ASP.NET Core Web API'sinde özel biçimlendiricileri
author: tdykstra
description: Oluşturmayı ve web ASP.NET Core API'leri için özel biçimlendiricileri kullanmayı öğrenin.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 45d64feea9b3466d32088b5949b33d3fec3b3a41
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="2fe9b-103">ASP.NET Core Web API'sinde özel biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="2fe9b-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="2fe9b-104">Tarafından [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2fe9b-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2fe9b-105">ASP.NET Core MVC web API'leri JSON, XML veya düz metin biçimlerini kullanarak veri değişimi için yerleşik destek sahip olur.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="2fe9b-106">Bu makalede, özel biçimlendiricileri oluşturarak ek biçimleri için destek eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="2fe9b-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2fe9b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="2fe9b-108">Ne zaman özel biçimlendiricileri kullanılır</span><span class="sxs-lookup"><span data-stu-id="2fe9b-108">When to use custom formatters</span></span>

<span data-ttu-id="2fe9b-109">İstediğiniz zaman özel bir biçimlendirici kullanmak [içerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) yerleşik biçimlendiricileri (JSON, XML ve düz metin) tarafından desteklenmeyen bir içerik türü desteklemek için işlem.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="2fe9b-110">Örneğin, bazı web API için istemci işleyebiliyorsa [Protobuf](https://github.com/google/protobuf) biçimi, daha etkili olduğundan Protobuf istemcilerle kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="2fe9b-111">Veya kişi adları ve adresleri göndermek için web API isteyebilirsiniz [vCard](https://wikipedia.org/wiki/VCard) biçimi, kişi veri değişimi için sık kullanılan biçim.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="2fe9b-112">Bu makalede sağlanan örnek uygulamayı basit vCard biçimlendirici uygular.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="2fe9b-113">Özel bir biçimlendirici kullanma genel bakış</span><span class="sxs-lookup"><span data-stu-id="2fe9b-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="2fe9b-114">Özel bir biçimlendirici oluşturmak için adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2fe9b-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="2fe9b-115">İstemciye gönderilecek verilerini seri hale getirmek istiyorsanız, bir çıktı biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="2fe9b-116">İstemciden alınan verileri seri durumdan istiyorsanız, bir giriş biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="2fe9b-117">Örnek, biçimlendiricilerin eklemek `InputFormatters` ve `OutputFormatters` koleksiyonlarda [MvcOptions](/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="2fe9b-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="2fe9b-118">Aşağıdaki bölümler bu adımların her biri için yönergeler ve kod örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="2fe9b-119">Özel biçimlendirici sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="2fe9b-119">How to create a custom formatter class</span></span>

<span data-ttu-id="2fe9b-120">Bir biçimlendirici oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="2fe9b-120">To create a formatter:</span></span>

* <span data-ttu-id="2fe9b-121">Sınıf uygun temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="2fe9b-122">Geçerli bir medya türlerini ve Kodlamalar oluşturucuda belirtin.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="2fe9b-123">Geçersiz kılma `CanReadType` / `CanWriteType` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="2fe9b-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="2fe9b-124">Geçersiz kılma `ReadRequestBodyAsync` / `WriteResponseBodyAsync` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="2fe9b-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="2fe9b-125">Uygun temel sınıfından türetilir</span><span class="sxs-lookup"><span data-stu-id="2fe9b-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="2fe9b-126">Metin medya türleri için (örneğin, vCard) türetilen [TextInputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) veya [TextOutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="2fe9b-127">İkili türleri için türetilen [InputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) veya [OutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-127">For binary types, derive from the [InputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="2fe9b-128">Geçerli bir medya türlerini ve Kodlamalar belirtin</span><span class="sxs-lookup"><span data-stu-id="2fe9b-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="2fe9b-129">Ekleyerek oluşturucuda geçerli bir medya türleri ve Kodlamalar belirtin `SupportedMediaTypes` ve `SupportedEncodings` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> <span data-ttu-id="2fe9b-130">Bir biçimlendirici sınıf oluşturucu bağımlılık ekleme yapamazsınız.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="2fe9b-131">Örneğin, oluşturucuya bir Günlükçü parametresini ekleyerek bir Günlükçü alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="2fe9b-132">Hizmetlere erişmek için yönteme geçirilen context nesnesi kullanmak zorunda.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="2fe9b-133">Kod örneği [aşağıda](#read-write) bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="2fe9b-134">CanReadType/CanWriteType geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="2fe9b-134">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="2fe9b-135">İçine seri durumdan ya da geçersiz kılarak öğesinden seri türünü belirtmek `CanReadType` veya `CanWriteType` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="2fe9b-136">Örneğin, yalnızca vCard metinden oluşturmak mümkün olabilir bir `Contact` türü tersi.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="2fe9b-137">CanWriteResult yöntemi</span><span class="sxs-lookup"><span data-stu-id="2fe9b-137">The CanWriteResult method</span></span>

<span data-ttu-id="2fe9b-138">Bazı senaryolarda geçersiz kılmak zorunda `CanWriteResult` yerine `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="2fe9b-139">Kullanım `CanWriteResult` aşağıdaki koşullar doğruysa:</span><span class="sxs-lookup"><span data-stu-id="2fe9b-139">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="2fe9b-140">Eylem yöntemi bir model sınıfı döndürür.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-140">Your action method returns a model class.</span></span>
* <span data-ttu-id="2fe9b-141">Çalışma zamanında döndürülen türetilmiş sınıfları vardır.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-141">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="2fe9b-142">Sınıfı eylem tarafından döndürülen türetilmiş çalışma zamanında bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-142">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="2fe9b-143">Örneğin, eylem yöntemi imzası döndürür varsayalım bir `Person` türü, ancak döndürebilir bir `Student` veya `Instructor` türetilen tür `Person`.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="2fe9b-144">Yalnızca işlemek için biçimlendirici istiyorsanız `Student` nesneleri denetleyin türünü [nesne](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) için sağlanan context nesnesi içinde `CanWriteResult` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-144">If you want your formatter to handle only `Student` objects, check the type of [Object](/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="2fe9b-145">Kullanmak için gerekli olmadığını göz önünde bulundurun `CanWriteResult` eylem yönteminin döndüğünde `IActionResult`; bu durumda, `CanWriteType` yöntemi çalışma zamanı türü alır.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="2fe9b-146">ReadRequestBodyAsync/WriteResponseBodyAsync geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="2fe9b-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="2fe9b-147">Seri durumdan veya içinde seri hale getirme gerçek yapması `ReadRequestBodyAsync` veya `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="2fe9b-148">Aşağıdaki örnekte vurgulanmış satırlar (bunları Oluşturucusu parametrelerinden alınamıyor) bağımlılık ekleme kapsayıcısını Hizmetleri verimli şekilde nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="2fe9b-149">MVC özel bir biçimlendirici kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2fe9b-149">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="2fe9b-150">Özel bir biçimlendirici kullanmak için biçimlendirici sınıfının bir örneğini ekleme `InputFormatters` veya `OutputFormatters` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="2fe9b-151">Biçimlendiricileri ekledikten sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="2fe9b-152">Birinci öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-152">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fe9b-153">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2fe9b-153">Next steps</span></span>

<span data-ttu-id="2fe9b-154">Bkz: [örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), uygulayan basit vCard giriş ve çıkış biçimlendiricileri.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="2fe9b-155">Uygulama okur ve aşağıdaki gibi görünen vCard Yazar:</span><span class="sxs-lookup"><span data-stu-id="2fe9b-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="2fe9b-156">VCard çıktı, uygulamayı çalıştırın ve başlığı "metin/vcard" Get isteğini kabul et ile göndermek için görmek için `http://localhost:63313/api/contacts/` (Visual Studio'dan çalışırken) veya `http://localhost:5000/api/contacts/` (komut satırından çalışırken).</span><span class="sxs-lookup"><span data-stu-id="2fe9b-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="2fe9b-157">VCard kişiler bellek içi koleksiyona eklemek için yukarıdaki örnekteki gibi biçimlendirilmiş gövdesinde vCard metin ile Content-Type üstbilgisi "metin/vcard" ile aynı URL bir Post isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="2fe9b-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
