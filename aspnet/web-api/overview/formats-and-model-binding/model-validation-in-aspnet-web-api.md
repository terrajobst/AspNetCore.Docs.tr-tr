---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "Model doğrulama ASP.NET Web API'de | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="ffb53-102">ASP.NET Web API'de model doğrulama</span><span class="sxs-lookup"><span data-stu-id="ffb53-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ffb53-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ffb53-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ffb53-104">Bir istemci web API'nize veri gönderdiğinde, genellikle verileri herhangi bir işlem yapmadan önce doğrulamak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="ffb53-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="ffb53-105">Bu makalede, Modellerinizi açıklama, veri doğrulama için ek açıklamalar kullanmak ve web API doğrulama hataları işlemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ffb53-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ffb53-106">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ffb53-106">Data Annotations</span></span>

<span data-ttu-id="ffb53-107">ASP.NET Web API'de özniteliklerini kullanabilirsiniz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) modelinize göre özellikleri için doğrulama kuralları ayarlamak için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="ffb53-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="ffb53-108">Aşağıdaki model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ffb53-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="ffb53-109">ASP.NET MVC model doğrulama kullandıysanız, bu tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="ffb53-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="ffb53-110">**Gerekli** özniteliği diyor `Name` özelliği null olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="ffb53-111">**Aralığı** özniteliği diyor `Weight` sıfır ile 999 arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="ffb53-112">Bir istemci bir POST isteği aşağıdaki JSON gösterimi ile gönderir varsayın:</span><span class="sxs-lookup"><span data-stu-id="ffb53-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="ffb53-113">İstemci içermediği görebilirsiniz `Name` olarak işaretlenmiş özelliği gerekli.</span><span class="sxs-lookup"><span data-stu-id="ffb53-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="ffb53-114">Web API dönüştürür JSON içinde ne zaman bir `Product` örneği doğruladığı `Product` karşı doğrulama öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="ffb53-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="ffb53-115">Denetleyici eyleminizi modelin geçerli olup olmadığını kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="ffb53-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="ffb53-116">Model doğrulama istemci verilerini güvenli olduğu garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="ffb53-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="ffb53-117">Ek doğrulama diğer uygulama katmanında gerekli.</span><span class="sxs-lookup"><span data-stu-id="ffb53-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="ffb53-118">(Örneğin, veri katmanı yabancı anahtar kısıtlamaları zorlayabilir.) Öğretici [Entity Framework ile Web API kullanarak](../data/using-web-api-with-entity-framework/part-1.md) bu sorunlardan bazıları araştırır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="ffb53-119">**"Eksik nakil"**: eksik nakil istemci bazı özellikleri ayrıldığında olur.</span><span class="sxs-lookup"><span data-stu-id="ffb53-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="ffb53-120">Örneğin, istemci aşağıdaki gönderir varsayın:</span><span class="sxs-lookup"><span data-stu-id="ffb53-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="ffb53-121">Burada, istemci için değerler belirtmedi `Price` veya `Weight`.</span><span class="sxs-lookup"><span data-stu-id="ffb53-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="ffb53-122">JSON formatter sıfır eksik özellikleri için varsayılan değeri atar.</span><span class="sxs-lookup"><span data-stu-id="ffb53-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="ffb53-123">Bu özellikler için geçerli bir değer sıfır olduğundan model durumu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="ffb53-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="ffb53-124">Bu bir sorun olup olmadığını, senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="ffb53-125">Örneğin, bir güncelleştirme işleminde, "sıfır" ve "ayarlı değil." ayırt etmek isteyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="ffb53-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="ffb53-126">Bir değer ayarlamak için istemcileri zorlamak üzere özelliği boş değer atanabilir ayarlanmış yapıp **gerekli** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="ffb53-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="ffb53-127">**"Aşırı gönderim"**: bir istemci de gönderebilirsiniz *daha fazla* , beklenen veri.</span><span class="sxs-lookup"><span data-stu-id="ffb53-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="ffb53-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffb53-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="ffb53-129">Burada, JSON bulunmayan bir özelliği ("renk") içeren `Product` modeli.</span><span class="sxs-lookup"><span data-stu-id="ffb53-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="ffb53-130">Bu durumda, JSON biçimlendirici yalnızca bu değer yok sayar.</span><span class="sxs-lookup"><span data-stu-id="ffb53-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="ffb53-131">(Aynı XML biçimlendiricisi desteklemez.) Model salt okunur olarak kullanılması özelliklere sahipse atlayarak nakil sorunlara neden olur.</span><span class="sxs-lookup"><span data-stu-id="ffb53-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="ffb53-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ffb53-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="ffb53-133">Güncelleştirmek için kullanıcıların istemediğiniz `IsAdmin` özelliği ve kendilerini yöneticilere!</span><span class="sxs-lookup"><span data-stu-id="ffb53-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="ffb53-134">Güvenli stratejisi istemci ne göndermesine izin verildiğinden tam olarak eşleşen bir model sınıfı kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="ffb53-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="ffb53-135">Brad Wilson'ın blog gönderisi "[giriş doğrulama vs. ASP.NET MVC doğrulama model](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"eksik nakil ve atlayarak nakil iyi bir tartışma vardır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="ffb53-136">Post ASP.NET MVC 2 hakkında olsa da, Web API için hala ilgili bir sorunlardır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="ffb53-137">Doğrulama hataları işleme</span><span class="sxs-lookup"><span data-stu-id="ffb53-137">Handling Validation Errors</span></span>

<span data-ttu-id="ffb53-138">Doğrulama başarısız olduğunda web API otomatik olarak bir hata istemciye döndürmez.</span><span class="sxs-lookup"><span data-stu-id="ffb53-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="ffb53-139">Model durumunu denetleyin ve uygun şekilde yanıt vermek için en fazla denetleyici eylemi var.</span><span class="sxs-lookup"><span data-stu-id="ffb53-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="ffb53-140">Denetleyici eylemini çağrılmadan önce model durumunu denetlemek için bir eylem filtresi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffb53-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="ffb53-141">Aşağıdaki kod örneği gösterir:</span><span class="sxs-lookup"><span data-stu-id="ffb53-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="ffb53-142">Model doğrulama başarısız olursa, bu filtre doğrulama hatalarını içeren bir HTTP yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="ffb53-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="ffb53-143">Bu durumda, denetleyici eylem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ffb53-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="ffb53-144">Tüm Web API denetleyicilerinin bu filtre uygulamak için filtre örneğini ekleme **HttpConfiguration.Filters** yapılandırma sırasında koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="ffb53-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="ffb53-145">Başka bir seçenek filtresi öznitelik olarak tek tek denetleyicileri veya denetleyici eylemleri ayarlamaktır:</span><span class="sxs-lookup"><span data-stu-id="ffb53-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
