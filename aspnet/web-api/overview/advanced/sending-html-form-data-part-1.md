---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "ASP.NET Web API HTML Form verileri gönderme: Form urlencoded veri | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="490ef-102">ASP.NET Web API HTML Form verileri gönderme: Form urlencoded veri</span><span class="sxs-lookup"><span data-stu-id="490ef-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="490ef-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="490ef-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="490ef-104">1. Kısım: Form urlencoded veri</span><span class="sxs-lookup"><span data-stu-id="490ef-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="490ef-105">Bu makalede bir Web API denetleyicisi form urlencoded veri göndermek nasıl gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="490ef-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="490ef-106">HTML formları genel bakış</span><span class="sxs-lookup"><span data-stu-id="490ef-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="490ef-107">Karmaşık türler gönderme</span><span class="sxs-lookup"><span data-stu-id="490ef-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="490ef-108">Form verileri AJAX aracılığıyla gönderme</span><span class="sxs-lookup"><span data-stu-id="490ef-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="490ef-109">Gönderen basit türler</span><span class="sxs-lookup"><span data-stu-id="490ef-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="490ef-110">[Tamamlanan projenizi indirin](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="490ef-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="490ef-111">HTML formları genel bakış</span><span class="sxs-lookup"><span data-stu-id="490ef-111">Overview of HTML Forms</span></span>

<span data-ttu-id="490ef-112">HTML formları kullanın alın veya sunucuya veri göndermek için gönderin.</span><span class="sxs-lookup"><span data-stu-id="490ef-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="490ef-113">**Yöntemi** özniteliği **form** öğesi HTTP yöntemini sunar:</span><span class="sxs-lookup"><span data-stu-id="490ef-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="490ef-114">GET için varsayılan yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="490ef-114">The default method is GET.</span></span> <span data-ttu-id="490ef-115">Formun kullanıyorsa, formun URI sorgu dizesi olarak kodlanmış verileri alın.</span><span class="sxs-lookup"><span data-stu-id="490ef-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="490ef-116">Form POST kullanıyorsa, form verilerini istek gövdesinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="490ef-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="490ef-117">Deftere nakledilen veriler için **enctype** özniteliği, istek gövdesini biçimi belirtir:</span><span class="sxs-lookup"><span data-stu-id="490ef-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="490ef-118">enctype</span><span class="sxs-lookup"><span data-stu-id="490ef-118">enctype</span></span> | <span data-ttu-id="490ef-119">Açıklama</span><span class="sxs-lookup"><span data-stu-id="490ef-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="490ef-120">Uygulama/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="490ef-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="490ef-121">Ad/değer çiftleri, bir URI sorgu dizesinde benzer olarak kodlanmış form verileri.</span><span class="sxs-lookup"><span data-stu-id="490ef-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="490ef-122">POST için varsayılan biçimi budur.</span><span class="sxs-lookup"><span data-stu-id="490ef-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="490ef-123">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="490ef-123">multipart/form-data</span></span> | <span data-ttu-id="490ef-124">Çok parçalı MIME ileti olarak kodlanmış form verileri.</span><span class="sxs-lookup"><span data-stu-id="490ef-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="490ef-125">Bir dosyayı sunucuya yüklüyorsanız bu biçimini kullanın.</span><span class="sxs-lookup"><span data-stu-id="490ef-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="490ef-126">Bu makalenin bölüm 1 x-www-form-urlencoded biçiminde görünür.</span><span class="sxs-lookup"><span data-stu-id="490ef-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="490ef-127">[2. bölüm](sending-html-form-data-part-2.md) çok parçalı MIME açıklar.</span><span class="sxs-lookup"><span data-stu-id="490ef-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="490ef-128">Karmaşık türler gönderme</span><span class="sxs-lookup"><span data-stu-id="490ef-128">Sending Complex Types</span></span>

<span data-ttu-id="490ef-129">Genellikle, birkaç formu denetimlerini gerçekleştirilecek değerleri oluşan bir karmaşık tür gönderir.</span><span class="sxs-lookup"><span data-stu-id="490ef-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="490ef-130">Bir durum güncelleştirmesi temsil eden aşağıdaki model göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="490ef-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="490ef-131">Kabul eden bir Web API denetleyicisi İşte bir `Update` POST ile nesne.</span><span class="sxs-lookup"><span data-stu-id="490ef-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="490ef-132">Bu denetleyici kullanan [eylem tabanlı yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)rota şablonu gelir &quot;API / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="490ef-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="490ef-133">İstemci verileri gönderecek &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="490ef-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="490ef-134">Şimdi bir durum güncelleştirmesi göndermek kullanıcılar için bir HTML formuna yazalım.</span><span class="sxs-lookup"><span data-stu-id="490ef-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="490ef-135">Dikkat **eylem** formdaki bir özniteliktir bizim denetleyici eylemi URI'si.</span><span class="sxs-lookup"><span data-stu-id="490ef-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="490ef-136">Girdiğiniz bazı değerler formla şöyledir:</span><span class="sxs-lookup"><span data-stu-id="490ef-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="490ef-137">Kullanıcı gönderme tıkladığında tarayıcı bir HTTP isteği aşağıdakine benzer gönderir:</span><span class="sxs-lookup"><span data-stu-id="490ef-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="490ef-138">İstek gövdesindeki ad/değer çiftleri olarak biçimlendirilmiş form verileri içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="490ef-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="490ef-139">Web API örneği otomatik olarak ad/değer çiftleri dönüştürür `Update` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="490ef-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="490ef-140">Form verileri AJAX aracılığıyla gönderme</span><span class="sxs-lookup"><span data-stu-id="490ef-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="490ef-141">Kullanıcı formu gönderdiğinde, tarayıcı geçerli sayfasından ayrılmayın gider ve yanıt iletisinin gövdesi işler.</span><span class="sxs-lookup"><span data-stu-id="490ef-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="490ef-142">Yanıt HTML sayfası olduğunda bu normaldir.</span><span class="sxs-lookup"><span data-stu-id="490ef-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="490ef-143">Bir web API ile ancak yanıt gövdesi genellikle bozulmuş boş veya JSON gibi yapılandırılmış veri içeriyor.</span><span class="sxs-lookup"><span data-stu-id="490ef-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="490ef-144">Bu durumda, istek bir AJAX kullanarak form verilerini, böylece sayfa yanıt işleyebilir göndermek için daha fazla mantıklıdır.</span><span class="sxs-lookup"><span data-stu-id="490ef-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="490ef-145">Aşağıdaki kodu kullanarak jQuery form veri göndermek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="490ef-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="490ef-146">JQuery **gönderme** işlevi ile yeni bir işlev form eylemi yerini alır.</span><span class="sxs-lookup"><span data-stu-id="490ef-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="490ef-147">Bu, Gönder düğmesine varsayılan davranışını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="490ef-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="490ef-148">**Seri** işlevi ad/değer çiftlerine form verilerini serileştirir.</span><span class="sxs-lookup"><span data-stu-id="490ef-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="490ef-149">Form verileri sunucuya göndermek için çağrı `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="490ef-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="490ef-150">İstek tamamlandığında `.success()` veya `.error()` işleyici kullanıcıya uygun bir mesaj görüntüler.</span><span class="sxs-lookup"><span data-stu-id="490ef-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="490ef-151">Gönderen basit türler</span><span class="sxs-lookup"><span data-stu-id="490ef-151">Sending Simple Types</span></span>

<span data-ttu-id="490ef-152">Önceki bölümlerde modeli sınıfının bir örneği için Web API serisi bir karmaşık tür gönderdi.</span><span class="sxs-lookup"><span data-stu-id="490ef-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="490ef-153">Dize gibi basit türler de gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="490ef-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="490ef-154">Basit bir tür göndermeden önce bunun yerine bir karmaşık tür değeri kaydırma göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="490ef-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="490ef-155">Bu sunucu tarafında model doğrulama avantajları sağlar ve gerekirse modelinizi genişletmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="490ef-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="490ef-156">Basit bir tür göndermek için temel adımlar aynıdır, ancak iki küçük farklılıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="490ef-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="490ef-157">İlk olarak, denetleyicisi, parametre adı ile tasarlamanız gerekir **FromBody** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="490ef-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="490ef-158">Varsayılan olarak, Web API istek URI'si basit türler almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="490ef-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="490ef-159">**FromBody** özniteliği söyler Web API isteği gövdesinden değeri okunamıyor.</span><span class="sxs-lookup"><span data-stu-id="490ef-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="490ef-160">Web API yanıt gövdesi en fazla bir kez, böylece yalnızca tek bir eylem parametresinin isteği gövdesinden gelebilir okur.</span><span class="sxs-lookup"><span data-stu-id="490ef-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="490ef-161">Birden çok değer isteği gövdesinden almanız gerekirse, karmaşık bir tür tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="490ef-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="490ef-162">İkinci olarak, istemci aşağıdaki biçimde değeri göndermesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="490ef-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="490ef-163">Özellikle, ad/değer çiftinin ad bölümünü basit bir tür için boş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="490ef-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="490ef-164">Tüm tarayıcılar bu için HTML formları desteklemez, ancak, bu biçim komut dosyasında aşağıdaki gibi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="490ef-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="490ef-165">Bir örnek form şöyledir:</span><span class="sxs-lookup"><span data-stu-id="490ef-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="490ef-166">Burada da form değer göndermek için komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="490ef-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="490ef-167">Yalnızca önceki komut dosyasından içine geçirilen bağımsız değişken farktır **sonrası** işlevi.</span><span class="sxs-lookup"><span data-stu-id="490ef-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="490ef-168">Basit türleri dizisi göndermek için aynı yaklaşımı kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="490ef-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="490ef-169">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="490ef-169">Additional Resources</span></span>

[<span data-ttu-id="490ef-170">2. Kısım: Karşıya dosya yükleme ve çok parçalı MIME</span><span class="sxs-lookup"><span data-stu-id="490ef-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
