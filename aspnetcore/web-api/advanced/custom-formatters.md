---
title: ASP.NET Core Web API'si, özel biçimlendiriciler
author: rick-anderson
description: Oluşturma ve web ASP.NET Core API'leri için özel biçimlendiriciler kullanma hakkında bilgi edinin.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: a038cd9c05950333fce9e72f67d6721198fae4d3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206321"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="16f2d-103">ASP.NET Core Web API'si, özel biçimlendiriciler</span><span class="sxs-lookup"><span data-stu-id="16f2d-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="16f2d-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="16f2d-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="16f2d-105">ASP.NET Core MVC, JSON, XML veya düz metin biçimleri kullanarak, web API'leri veri değişimi için yerleşik destek sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="16f2d-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="16f2d-106">Bu makalede, özel biçimlendiriciler oluşturarak ek biçimleri için destek eklenecek şekilde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="16f2d-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16f2d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="16f2d-108">Özel biçimlendiriciler kullanıldığı durumlar</span><span class="sxs-lookup"><span data-stu-id="16f2d-108">When to use custom formatters</span></span>

<span data-ttu-id="16f2d-109">İstediğiniz zaman bir özel biçimlendiricisini kullanmayı [içerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) yerleşik biçimlendiricileri (JSON, XML ve düz metin) tarafından desteklenmeyen bir içerik türünü desteklemek üzere işlem.</span><span class="sxs-lookup"><span data-stu-id="16f2d-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="16f2d-110">Örneğin, web API'niz için olan istemcilerin bazılarını işleyebilir [Protobuf](https://github.com/google/protobuf) biçimi daha etkilidir çünkü Protobuf istemcilerle kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16f2d-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="16f2d-111">Veya kişi adlarını ve adreslerini göndermek için web API'nizi isteyebileceğiniz [vCard](https://wikipedia.org/wiki/VCard) biçimi, kişi veri değişimi için yaygın olarak kullanılan bir biçimi.</span><span class="sxs-lookup"><span data-stu-id="16f2d-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="16f2d-112">Bu makalede sağlanan örnek uygulama basit vCard biçimlendirici uygular.</span><span class="sxs-lookup"><span data-stu-id="16f2d-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="16f2d-113">Özel bir biçimlendirici kullanmayı genel bakış</span><span class="sxs-lookup"><span data-stu-id="16f2d-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="16f2d-114">Özel bir biçimlendirici oluşturup kullanacağınızı adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="16f2d-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="16f2d-115">İstemciye gönderilecek verileri seri hale getirmek istiyorsanız, bir çıkış biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="16f2d-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="16f2d-116">İstemciden alınan verileri seri durumdan istiyorsanız bir giriş biçimlendirici sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="16f2d-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="16f2d-117">Eklemek için biçimlendiricileri örneklerini `InputFormatters` ve `OutputFormatters` koleksiyonlarında [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="16f2d-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="16f2d-118">Aşağıdaki bölümler bu adımların her biri için rehberlik ve kod örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="16f2d-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="16f2d-119">Özel biçimlendirici sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="16f2d-119">How to create a custom formatter class</span></span>

<span data-ttu-id="16f2d-120">Bir biçimlendirici oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="16f2d-120">To create a formatter:</span></span>

* <span data-ttu-id="16f2d-121">Sınıfı, uygun bir temel sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="16f2d-122">Geçerli bir medya türleri ve kodlamaları oluşturucuda belirtin.</span><span class="sxs-lookup"><span data-stu-id="16f2d-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="16f2d-123">Geçersiz kılma `CanReadType` / `CanWriteType` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="16f2d-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="16f2d-124">Geçersiz kılma `ReadRequestBodyAsync` / `WriteResponseBodyAsync` yöntemleri</span><span class="sxs-lookup"><span data-stu-id="16f2d-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="16f2d-125">Uygun temel sınıfından türetilir</span><span class="sxs-lookup"><span data-stu-id="16f2d-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="16f2d-126">Metin medya türleri için (örneğin, vCard) öğesinden türetilen [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) veya [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="16f2d-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="16f2d-127">İkili türleri için türetilen [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) veya [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="16f2d-127">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="16f2d-128">Geçerli bir medya türleri ve kodlamayı belirtin</span><span class="sxs-lookup"><span data-stu-id="16f2d-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="16f2d-129">Ekleyerek oluşturucusunun içinde geçerli bir medya türleri ve kodlamayı belirtin `SupportedMediaTypes` ve `SupportedEncodings` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="16f2d-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> <span data-ttu-id="16f2d-130">Bir biçimlendirici sınıftaki Oluşturucu bağımlılık ekleme yapamazsınız.</span><span class="sxs-lookup"><span data-stu-id="16f2d-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="16f2d-131">Örneğin, oluşturucuya bir Günlükçü parametre ekleyerek bir Günlükçü alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="16f2d-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="16f2d-132">Hizmetlere erişmek için yönteme geçirilen bağlam nesnesi kullanmak zorunda.</span><span class="sxs-lookup"><span data-stu-id="16f2d-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="16f2d-133">Bir kod örneği [aşağıda](#read-write) bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="16f2d-134">CanReadType/CanWriteType geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="16f2d-134">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="16f2d-135">İçine seri durumdan veya öğesinden serileştirme geçersiz kılarak türü belirtmek `CanReadType` veya `CanWriteType` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="16f2d-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="16f2d-136">Örneğin, yalnızca vCard metinden oluşturmak mümkün olabilir bir `Contact` türü geçme veya tam tersi.</span><span class="sxs-lookup"><span data-stu-id="16f2d-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="16f2d-137">CanWriteResult yöntemi</span><span class="sxs-lookup"><span data-stu-id="16f2d-137">The CanWriteResult method</span></span>

<span data-ttu-id="16f2d-138">Geçersiz kılmak sahip bazı senaryolarda `CanWriteResult` yerine `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="16f2d-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="16f2d-139">Kullanım `CanWriteResult` aşağıdaki koşullar geçerli olduğunda:</span><span class="sxs-lookup"><span data-stu-id="16f2d-139">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="16f2d-140">Bir model sınıfı, eylem yöntemi döndürür.</span><span class="sxs-lookup"><span data-stu-id="16f2d-140">Your action method returns a model class.</span></span>
* <span data-ttu-id="16f2d-141">Çalışma zamanında döndürülen türetilmiş sınıfları vardır.</span><span class="sxs-lookup"><span data-stu-id="16f2d-141">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="16f2d-142">Sınıfı eylem tarafından döndürülen türetilmiş çalışma zamanında bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-142">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="16f2d-143">Örneğin, eylem yöntemi imzanızı döndürür varsayalım. bir `Person` türü, ancak döndürebilir bir `Student` veya `Instructor` , türetilen tür `Person`.</span><span class="sxs-lookup"><span data-stu-id="16f2d-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="16f2d-144">Yalnızca işlemek için biçimlendirici istiyorsanız `Student` nesneleri kontrol türünü [nesne](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) sağlanan context nesnesi içinde `CanWriteResult` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16f2d-144">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="16f2d-145">Kullanmak için gerekli olmadığını göz önünde bulundurun `CanWriteResult` eylem yöntemi döndüğünde `IActionResult`; bu durumda, `CanWriteType` yöntem çalışma zamanı türü alır.</span><span class="sxs-lookup"><span data-stu-id="16f2d-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="16f2d-146">ReadRequestBodyAsync/WriteResponseBodyAsync geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="16f2d-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="16f2d-147">Seri durumdan çıkarılırken veya seri hale getirme gerçek kitaplıklarımızı `ReadRequestBodyAsync` veya `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="16f2d-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="16f2d-148">Vurgulanan satırları aşağıdaki örnekte, Hizmetleri (bunları Oluşturucu parametreler alınamıyor) bağımlılık ekleme kapsayıcısını alma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="16f2d-149">MVC özel bir biçimlendirici kullanmak için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="16f2d-149">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="16f2d-150">Özel bir biçimlendirici kullanılacak biçimlendirici sınıfı bir örneğini ekleme `InputFormatters` veya `OutputFormatters` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="16f2d-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="16f2d-151">Biçimlendiricileri ekledikten sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="16f2d-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="16f2d-152">Birinci öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="16f2d-152">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16f2d-153">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="16f2d-153">Next steps</span></span>

<span data-ttu-id="16f2d-154">Bkz: [örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), uygulayan basit vCard giriş ve çıkış biçimlendiricileri.</span><span class="sxs-lookup"><span data-stu-id="16f2d-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="16f2d-155">Uygulama okur ve aşağıdaki gibi görünen vCard Yazar:</span><span class="sxs-lookup"><span data-stu-id="16f2d-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="16f2d-156">VCard çıktı, uygulamayı çalıştırmak ve kabul etme ile bir Get isteği üst bilgisi "metin/vcard" gönderme için görmek için `http://localhost:63313/api/contacts/` (Visual Studio çalışırken) veya `http://localhost:5000/api/contacts/` (komut satırından çalışırken).</span><span class="sxs-lookup"><span data-stu-id="16f2d-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="16f2d-157">VCard kişiler bellek içi koleksiyonuna eklemek için aynı URL'yi vCard metin yukarıdaki örnekte olduğu gibi biçimlendirilmiş gövdesinde ve Content-Type üstbilgisi "metin/vcard" ile bir Post isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="16f2d-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
