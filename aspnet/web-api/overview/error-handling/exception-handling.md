---
uid: web-api/overview/error-handling/exception-handling
title: Özel durum işleme ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756962"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="31509-102">Özel durum işleme ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="31509-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="31509-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31509-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="31509-104">Bu makalede, hata ve özel durum işleme ASP.NET Web API'de açıklanır.</span><span class="sxs-lookup"><span data-stu-id="31509-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="31509-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="31509-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="31509-106">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="31509-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="31509-107">Özel durum filtreleri kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="31509-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="31509-108">HTTP hatası</span><span class="sxs-lookup"><span data-stu-id="31509-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="31509-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="31509-109">HttpResponseException</span></span>

<span data-ttu-id="31509-110">Bir Web API denetleyicisi yakalanmayan bir özel durum oluşturursa ne olur?</span><span class="sxs-lookup"><span data-stu-id="31509-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="31509-111">Varsayılan olarak, çoğu özel durumların bir HTTP yanıtının durum kodu, 500 İç sunucu hatası ile çevrilir.</span><span class="sxs-lookup"><span data-stu-id="31509-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="31509-112">**HttpResponseException** bir özel durum türüdür.</span><span class="sxs-lookup"><span data-stu-id="31509-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="31509-113">Bu durum, özel durum oluşturucuda belirttiğiniz herhangi bir HTTP durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="31509-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="31509-114">Örneğin, aşağıdaki yöntem, 404, bulunamadı, döndürür *kimliği* parametresi geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="31509-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="31509-115">Yanıt üzerinde daha fazla denetim için ayrıca tüm yanıt iletisi oluşturmak ve bunu ile dahil **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="31509-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="31509-116">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="31509-116">Exception Filters</span></span>

<span data-ttu-id="31509-117">Web API yazarak özel durumları nasıl işlediğini özelleştirebileceğiniz bir *özel durum filtresi*.</span><span class="sxs-lookup"><span data-stu-id="31509-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="31509-118">Bir denetleyici yöntemi olan bir işlenmeyen özel durum oluşturduğunda, özel durum filtresi yürütülür *değil* bir **HttpResponseException** özel durum.</span><span class="sxs-lookup"><span data-stu-id="31509-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="31509-119">**HttpResponseException** özellikle bir HTTP yanıtı döndüren için tasarlandığından türü olan bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="31509-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="31509-120">Özel durum filtreleri uygulamak **System.Web.Http.Filters.IExceptionFilter** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="31509-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="31509-121">Türetilmesi için bir özel durum filtresi yazma en kolay yolu olan **System.Web.Http.Filters.ExceptionFilterAttribute** sınıf ve geçersiz kılma **OnException** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="31509-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="31509-122">ASP.NET Web API'de özel durum filtreleri ASP.NET mvc'de benzerdir.</span><span class="sxs-lookup"><span data-stu-id="31509-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="31509-123">Ancak, bunlar ayrı ad alanı ve işlevi ayrı olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="31509-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="31509-124">Özellikle, **HandleErrorAttribute** MVC'de kullanılan sınıf Web APİ'si denetleyicilerinin tarafından oluşturulan özel durumları işlemez.</span><span class="sxs-lookup"><span data-stu-id="31509-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="31509-125">İşte dönüştüren bir filtre **NotImplementedException** 501, uygulanmadı özel durumları HTTP durum kodu:</span><span class="sxs-lookup"><span data-stu-id="31509-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="31509-126">**Yanıt** özelliği **HttpActionExecutedContext** nesne istemciye gönderilecek HTTP yanıt iletisi içerir.</span><span class="sxs-lookup"><span data-stu-id="31509-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="31509-127">Özel durum filtreleri kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="31509-127">Registering Exception Filters</span></span>

<span data-ttu-id="31509-128">Bir Web API özel durum filtre kaydetmek için birkaç yol vardır:</span><span class="sxs-lookup"><span data-stu-id="31509-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="31509-129">Eylem tarafından</span><span class="sxs-lookup"><span data-stu-id="31509-129">By action</span></span>
- <span data-ttu-id="31509-130">Denetleyici tarafından</span><span class="sxs-lookup"><span data-stu-id="31509-130">By controller</span></span>
- <span data-ttu-id="31509-131">Genel olarak</span><span class="sxs-lookup"><span data-stu-id="31509-131">Globally</span></span>

<span data-ttu-id="31509-132">Belirli bir eylem için filtre uygulamak için filtre eylemi için bir özniteliği olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="31509-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="31509-133">Tüm bir denetleyici eylemleri için filtre uygulamak için bir özniteliği olarak filtre denetleyici sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="31509-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="31509-134">Filtre için tüm Web APİ'si denetleyicilerinin genel olarak uygulamak için filtre bir örneğini ekleme **GlobalConfiguration.Configuration.Filters** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="31509-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="31509-135">Özel durum filtreleri bu koleksiyondaki herhangi bir Web API denetleyici eylemi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="31509-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="31509-136">Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonunu kullanıyorsanız, Web API configuration kodunuzda gezebilirsiniz put `WebApiConfig` uygulamada bulunan sınıf\_başlangıç klasörü:</span><span class="sxs-lookup"><span data-stu-id="31509-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="31509-137">HTTP hatası</span><span class="sxs-lookup"><span data-stu-id="31509-137">HttpError</span></span>

<span data-ttu-id="31509-138">**HTTP hatası** nesnesi, yanıt gövdesi içinde hata bilgilerini döndürmek için tutarlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="31509-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="31509-139">Aşağıdaki örnek, ile nasıl HTTP durum kodu 404 (bulunamadı) döndürüleceğini gösterir. bir **HTTP hatası** yanıt gövdesi içinde.</span><span class="sxs-lookup"><span data-stu-id="31509-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="31509-140">**CreateErrorResponse** içinde bir uzantı yönteminin tanımlandığı **System.Net.Http.HttpRequestMessageExtensions** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="31509-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="31509-141">Dahili olarak **CreateErrorResponse** oluşturur bir **HTTP hatası** örnek ve ardından oluşturan bir **HttpResponseMessage** içeren **HTTPhatası**.</span><span class="sxs-lookup"><span data-stu-id="31509-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="31509-142">Bu örnekte, yöntem başarılı olursa üründe bir HTTP yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="31509-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="31509-143">Ancak istenen ürüne bulunamazsa, HTTP yanıtı içeren bir **HTTP hatası** istek gövdesindeki.</span><span class="sxs-lookup"><span data-stu-id="31509-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="31509-144">Yanıt aşağıdaki gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="31509-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="31509-145">Dikkat **HTTP hatası** Bu örnekte, JSON için seri duruma.</span><span class="sxs-lookup"><span data-stu-id="31509-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="31509-146">Kullanmanın bir avantajı **HTTP hatası** aynı gittiği yerdir [içerik anlaşması](../formats-and-model-binding/content-negotiation.md) ve seri hale getirme işlem diğer kesin olarak belirlenmiş model.</span><span class="sxs-lookup"><span data-stu-id="31509-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="31509-147">HTTP hatası ve Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="31509-147">HttpError and Model Validation</span></span>

<span data-ttu-id="31509-148">Model doğrulama için model durumuna geçirebilirsiniz **CreateErrorResponse**, yanıtta doğrulama hataları dahil etmek için:</span><span class="sxs-lookup"><span data-stu-id="31509-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="31509-149">Bu örnekte şu yanıtı döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="31509-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="31509-150">Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="31509-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="31509-151">HTTP hatası HttpResponseException ile kullanma</span><span class="sxs-lookup"><span data-stu-id="31509-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="31509-152">Önceki örneklerde dönüş bir **HttpResponseMessage** denetleyici eylemi, ancak ileti de kullanabilir **HttpResponseException** döndürülecek bir **HTTP hatası**.</span><span class="sxs-lookup"><span data-stu-id="31509-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="31509-153">Bu, kesin türü belirtilmiş bir model hala döndürürken normal başarı durumunda iade sağlar **HTTP hatası** bir hata varsa:</span><span class="sxs-lookup"><span data-stu-id="31509-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
