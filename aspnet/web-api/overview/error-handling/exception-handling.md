---
uid: web-api/overview/error-handling/exception-handling
title: Özel durum işleme ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566394"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="4b4ae-102">Özel durum ASP.NET Web API işleme</span><span class="sxs-lookup"><span data-stu-id="4b4ae-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4b4ae-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b4ae-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4b4ae-104">Bu makalede, hata ve özel durum işleme ASP.NET Web API'de açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="4b4ae-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="4b4ae-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="4b4ae-106">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="4b4ae-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="4b4ae-107">Özel durum filtreleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="4b4ae-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="4b4ae-108">HTTP hatası</span><span class="sxs-lookup"><span data-stu-id="4b4ae-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="4b4ae-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="4b4ae-109">HttpResponseException</span></span>

<span data-ttu-id="4b4ae-110">Web API denetleyicisi yakalanmayan bir özel durum oluşturursa ne olur?</span><span class="sxs-lookup"><span data-stu-id="4b4ae-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="4b4ae-111">Varsayılan olarak, bir HTTP yanıtı durum kodu 500, iç sunucu hatası ile içine çoğu özel durumlar çevrilir.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="4b4ae-112">**HttpResponseException** türü olan bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="4b4ae-113">Bu özel durum oluşturucuda belirttiğiniz tüm HTTP durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="4b4ae-114">Örneğin, aşağıdaki yöntemi 404, bulunamadı, döndürür *kimliği* parametresi geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="4b4ae-115">Yanıt üzerinde daha fazla denetim için ayrıca tüm yanıt iletisi oluşturmak ve onunla dahil **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="4b4ae-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="4b4ae-116">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="4b4ae-116">Exception Filters</span></span>

<span data-ttu-id="4b4ae-117">Web API yazarak özel durumları nasıl işler özelleştirebilirsiniz bir *özel durum filtresi*.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="4b4ae-118">Denetleyici yöntemi herhangi işlenmeyen özel durum oluşturduğunda bir özel durum filtresi yürütülür *değil* bir **HttpResponseException** özel durum.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="4b4ae-119">**HttpResponseException** türü olan bir özel durum çünkü özel olarak bir HTTP yanıtının döndürmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="4b4ae-120">Özel durum filtreleri uygulamak **System.Web.Http.Filters.IExceptionFilter** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="4b4ae-121">Öğesinden türetilen için bir özel durum filtresi yazma en basit yolu olan **System.Web.Http.Filters.ExceptionFilterAttribute** sınıfı ve geçersiz kılma **OnException** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="4b4ae-122">ASP.NET Web API özel durum filtrelerini ASP.NET mvc'de benzerdir.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="4b4ae-123">Ancak, bunlar bir ayrı ad alanı ve işlev ayrı olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="4b4ae-124">Özellikle, **HandleErrorAttribute** MVC uygulamasında kullanılan sınıf Web API denetleyicisi tarafından oluşturulan özel durumları işleme değil.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="4b4ae-125">Dönüştüren bir filtre işte **NotImplementedException** özel durumlar içine HTTP durum kodu 501, uygulanmadı:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="4b4ae-126">**Yanıt** özelliği **HttpActionExecutedContext** nesne istemciye gönderilen HTTP yanıt iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="4b4ae-127">Özel durum filtreleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="4b4ae-127">Registering Exception Filters</span></span>

<span data-ttu-id="4b4ae-128">Bir Web API özel durum filtresi kaydetmek için birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="4b4ae-129">Eylem tarafından</span><span class="sxs-lookup"><span data-stu-id="4b4ae-129">By action</span></span>
- <span data-ttu-id="4b4ae-130">Denetleyici tarafından</span><span class="sxs-lookup"><span data-stu-id="4b4ae-130">By controller</span></span>
- <span data-ttu-id="4b4ae-131">Genel</span><span class="sxs-lookup"><span data-stu-id="4b4ae-131">Globally</span></span>

<span data-ttu-id="4b4ae-132">Belirli bir eylem filtresi uygulamak için filtre eylemi için bir özniteliği olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="4b4ae-133">Tüm bir denetleyici eylemleri filtre uygulamak için filtre öznitelik olarak denetleyici sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="4b4ae-134">Tüm Web API denetleyicilerinin genel filtre uygulamak için filtre örneğini ekleme **GlobalConfiguration.Configuration.Filters** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="4b4ae-135">Bu koleksiyonda exeption filtreleri için herhangi bir Web API denetleyici eylemi uygular.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="4b4ae-136">Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonu kullanırsanız, Web API yapılandırması kodunuzu içinde put `WebApiConfig` uygulamada bulunan sınıfı\_başlangıç klasörü:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="4b4ae-137">HTTP hatası</span><span class="sxs-lookup"><span data-stu-id="4b4ae-137">HttpError</span></span>

<span data-ttu-id="4b4ae-138">**HTTP hatası** nesnesi, yanıt gövdesinde hata bilgilerini döndürmek için tutarlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="4b4ae-139">Aşağıdaki örnek, ile nasıl HTTP durum kodu 404 (bulunamadı) döndürüleceğini gösterir. bir **HTTP hatası** yanıt gövdesi içinde.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="4b4ae-140">**CreateErrorResponse** bir genişletme yöntemi tanımlanan **System.Net.Http.HttpRequestMessageExtensions** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="4b4ae-141">Dahili olarak, **CreateErrorResponse** oluşturur bir **HTTP hatası** örneği ve ardından oluşturan bir **httpresponsemessage öğesini** içeren **HTTP hatası**.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="4b4ae-142">Bu örnekte, yöntem başarılı olursa HTTP yanıt ürün döndürür.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="4b4ae-143">Ancak istenen ürün bulunmazsa, HTTP yanıtı içeren bir **HTTP hatası** istek gövdesindeki.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="4b4ae-144">Yanıt aşağıdakine benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="4b4ae-145">Dikkat **HTTP hatası** JSON olarak bu örnekte serileştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="4b4ae-146">Kullanmanın bir avantajı **HTTP hatası** aynı gider olan [içerik anlaşması](../formats-and-model-binding/content-negotiation.md) ve seri hale getirme işlemek gibi diğer kesin türü belirtilmiş modeli.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="4b4ae-147">HTTP hatası ve Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="4b4ae-147">HttpError and Model Validation</span></span>

<span data-ttu-id="4b4ae-148">Model doğrulama için model durumuna geçirebilirsiniz **CreateErrorResponse**, doğrulama hataları yanıta dahil etmek için:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="4b4ae-149">Bu örnekte aşağıdaki yanıtı döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="4b4ae-150">Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4b4ae-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="4b4ae-151">HTTP hatası HttpResponseException ile kullanma</span><span class="sxs-lookup"><span data-stu-id="4b4ae-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="4b4ae-152">Önceki örneklerde dönüş bir **httpresponsemessage öğesini** denetleyici eylemi, ancak ileti de kullanabilir **HttpResponseException** döndürmek için bir **HTTP hatası**.</span><span class="sxs-lookup"><span data-stu-id="4b4ae-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="4b4ae-153">Bu, hala döndürürken normal başarılı durumda kesin türü belirtilmiş bir model dönüş sağlar **HTTP hatası** bir hata varsa:</span><span class="sxs-lookup"><span data-stu-id="4b4ae-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
