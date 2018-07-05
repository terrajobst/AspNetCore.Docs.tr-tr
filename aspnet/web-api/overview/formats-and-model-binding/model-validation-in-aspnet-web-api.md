---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model doğrulama ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393102"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="950dd-102">ASP.NET Web API'de model doğrulama</span><span class="sxs-lookup"><span data-stu-id="950dd-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="950dd-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="950dd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="950dd-104">İstemci web API'nize veri gönderdiğinde, genellikle herhangi bir işlem gerçekleştirmeden önce verileri doğrulamak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="950dd-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="950dd-105">Bu makalede, web API'NİZİN doğrulama hataları işlemek modellerinize eklemek ve veri doğrulama için ek açıklamalarını kullanma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="950dd-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="950dd-106">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="950dd-106">Data Annotations</span></span>

<span data-ttu-id="950dd-107">ASP.NET Web API'de öznitelikleri kullanabilirsiniz [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) modelinize göre özellikler için doğrulama kurallarını ayarlamak için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="950dd-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="950dd-108">Şu model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="950dd-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="950dd-109">ASP.NET MVC'de model doğrulama kullandıysanız, bu tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="950dd-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="950dd-110">**Gerekli** öznitelik yazan `Name` özelliği null olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="950dd-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="950dd-111">**Aralığı** öznitelik yazan `Weight` sıfır ile 999 arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="950dd-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="950dd-112">Bir istemci aşağıdaki JSON gösterimine sahip bir POST isteği gönderir varsayalım:</span><span class="sxs-lookup"><span data-stu-id="950dd-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="950dd-113">İstemci içermediği gördüğünüz `Name` olarak işaretlenmiş özelliği gerekli.</span><span class="sxs-lookup"><span data-stu-id="950dd-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="950dd-114">Web API dönüştürür JSON'a ne zaman bir `Product` örneği, nasıl doğruladığı `Product` karşı doğrulama öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="950dd-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="950dd-115">Denetleyici eyleminizi model geçerli olup olmadığını kontrol edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="950dd-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="950dd-116">Model doğrulaması istemci verilerini güvenli olduğu garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="950dd-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="950dd-117">Ek doğrulama diğer uygulama katmanında gerekli.</span><span class="sxs-lookup"><span data-stu-id="950dd-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="950dd-118">(Örneğin, veri katmanı yabancı anahtar kısıtlamalarını zorlayabilir.) Öğreticiyi [Entity Framework ile Web API kullanarak](../data/using-web-api-with-entity-framework/part-1.md) bu sorunlardan bazıları keşfediyor.</span><span class="sxs-lookup"><span data-stu-id="950dd-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="950dd-119">**"Eksik posta"**: eksik posta istemci bazı özellikleri dışına çıktığında gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="950dd-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="950dd-120">Örneğin, aşağıdaki istemcinin gönderdiği varsayalım:</span><span class="sxs-lookup"><span data-stu-id="950dd-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="950dd-121">Burada, istemci için değerleri belirtmediniz `Price` veya `Weight`.</span><span class="sxs-lookup"><span data-stu-id="950dd-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="950dd-122">JSON biçimlendirici sıfır eksik özellikleri için varsayılan değeri atar.</span><span class="sxs-lookup"><span data-stu-id="950dd-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="950dd-123">Bu özellikler için geçerli bir değer sıfır olduğundan model durumu geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="950dd-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="950dd-124">Bu bir sorun olup olmadığını, senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="950dd-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="950dd-125">Örneğin, bir update işleminde "sıfır" ve "ayarlı değil." ayırt etmek isteyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="950dd-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="950dd-126">Bir değer ayarlamak için istemcileri zorlamak için özelliği boş değer atanabilir ve ayarlanmış olun **gerekli** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="950dd-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="950dd-127">**"Aşırı gönderme"**: bir istemci de gönderebilirsiniz *daha fazla* beklediğinizden daha veri.</span><span class="sxs-lookup"><span data-stu-id="950dd-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="950dd-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="950dd-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="950dd-129">Burada, JSON yok ("rengi") bir özellik içeren `Product` modeli.</span><span class="sxs-lookup"><span data-stu-id="950dd-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="950dd-130">Bu durumda, JSON biçimlendirici, yalnızca bu değeri yoksayar.</span><span class="sxs-lookup"><span data-stu-id="950dd-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="950dd-131">(Aynı XML biçimlendiricisi desteklemez.) Model salt okunur olmasını istediğiniz özellikler varsa fazla posta sorunlara neden olur.</span><span class="sxs-lookup"><span data-stu-id="950dd-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="950dd-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="950dd-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="950dd-133">Güncelleştirilecek kullanıcı istemediğiniz `IsAdmin` özelliği ve kendileri için yöneticileri Yükselt!</span><span class="sxs-lookup"><span data-stu-id="950dd-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="950dd-134">Güvenli stratejisi, istemcinin göndermek için izin verilenden tam olarak eşleşen bir model sınıfı kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="950dd-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="950dd-135">Brad Wilson'ın blog gönderisi "[giriş doğrulaması vs. Model doğrulama ASP.NET mvc'de](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"eksik posta ve fazla posta iyi bir tartışma sahiptir.</span><span class="sxs-lookup"><span data-stu-id="950dd-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="950dd-136">ASP.NET MVC 2 hakkında posta olsa da, sorunları Web API'sine yine de ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="950dd-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="950dd-137">Doğrulama hataları işleme</span><span class="sxs-lookup"><span data-stu-id="950dd-137">Handling Validation Errors</span></span>

<span data-ttu-id="950dd-138">Doğrulama başarısız olduğunda web API'si otomatik olarak bir hata istemciye döndürmez.</span><span class="sxs-lookup"><span data-stu-id="950dd-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="950dd-139">Model durumu denetlemek ve uygun şekilde yanıtlanması için en fazla denetleyici eylemi var.</span><span class="sxs-lookup"><span data-stu-id="950dd-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="950dd-140">Denetleyici eylemini çağrılmadan önce model durumunu denetlemek için bir eyleme eylem filtresi de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="950dd-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="950dd-141">Aşağıdaki kod, bir örnek göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="950dd-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="950dd-142">Bu filtre, model doğrulama başarısız olursa doğrulama hatalarını içeren bir HTTP yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="950dd-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="950dd-143">Bu durumda, denetleyici eylem çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="950dd-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="950dd-144">Tüm Web APİ'si denetleyicilerinin için bu filtre uygulamak için filtre bir örneğini ekleme **HttpConfiguration.Filters** yapılandırma sırasında koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="950dd-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="950dd-145">Filtre bir özniteliği olarak tek tek denetleyicileri veya denetleyici eylemlerini ayarlamak başka bir seçenektir:</span><span class="sxs-lookup"><span data-stu-id="950dd-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
