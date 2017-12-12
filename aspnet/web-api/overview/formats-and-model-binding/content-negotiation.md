---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "İçerik anlaşması ASP.NET Web API'de | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API HTTP İçerik anlaşması nasıl uyguladığını açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="dfdf3-103">ASP.NET Web API'de içerik anlaşması</span><span class="sxs-lookup"><span data-stu-id="dfdf3-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="dfdf3-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dfdf3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dfdf3-105">Bu makalede, ASP.NET Web API içerik anlaşması nasıl uyguladığını açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="dfdf3-106">"Kullanılabilir birden çok Beyanları olduğunda belirtilen yanıt için en iyi gösterimi seçme işlemi." olarak içerik anlaşması HTTP belirtimi (RFC 2616) tanımlar</span><span class="sxs-lookup"><span data-stu-id="dfdf3-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="dfdf3-107">HTTP İçerik anlaşması için birincil mekanizma olan bu istek üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="dfdf3-108">**Kabul et:** hangi medya türü "application/json" gibi yanıtı için kabul edilebilir "application/xml" ya da özel ortam türü gibi &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="dfdf3-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="dfdf3-109">**Accept-Charset:** hangi karakter kümesi UTF-8 veya ISO 8859-1 gibi kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="dfdf3-110">**-Encoding kabul:** gzip gibi hangi içerik kodlamaları edilebilir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="dfdf3-111">**Kabul dil:** tercih edilen doğal dil gibi "en-us".</span><span class="sxs-lookup"><span data-stu-id="dfdf3-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="dfdf3-112">Sunucu HTTP isteği diğer kısımlarını de bakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="dfdf3-113">İsteği X-Requested-With üst bilgisi içeriyorsa, Accept üstbilgisi ise örneğin, bir AJAX isteği belirten JSON için varsayılan sunucu.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="dfdf3-114">Bu makalede, Web API kabul ediyor ve Accept-Charset üstbilgileri nasıl kullandığını adresindeki göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="dfdf3-115">(Şu anda yerleşik desteği yoktur Accept-Encoding veya Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="dfdf3-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="dfdf3-116">Serileştirme</span><span class="sxs-lookup"><span data-stu-id="dfdf3-116">Serialization</span></span>

<span data-ttu-id="dfdf3-117">Web API denetleyicisi bir kaynak olarak CLR türü döndürürse, ardışık düzen dönüş değeri olarak serileştirir ve HTTP yanıt gövdesi yazar.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="dfdf3-118">Örneğin, aşağıdaki denetleyici eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="dfdf3-119">Bir istemci bu HTTP istek gönderebilir:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="dfdf3-120">Yanıt olarak, sunucunun gönderebilir:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="dfdf3-121">Bu örnekte, istemci istenen JSON, Javascript veya "hiçbir şey" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="dfdf3-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="dfdf3-122">Sunucunun JSON temsili yanıtlanan `Product` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="dfdf3-123">Content-Type üstbilgisi yanıt kümesine bildirimi &quot;uygulama/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="dfdf3-124">Bir denetleyici ayrıca dönebilirsiniz bir **httpresponsemessage öğesini** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="dfdf3-125">Yanıt gövdesi için bir CLR nesnesi belirtmek için arama **CreateResponse** genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="dfdf3-126">Bu seçenek yanıt ayrıntılarını üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="dfdf3-127">Durum kodu ayarlamanız, HTTP üstbilgileri ekleme ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="dfdf3-128">Kaynak serileştiren nesnesi olarak adlandırılan bir *medya biçimlendiricisi*.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="dfdf3-129">Medya biçimlendiricileri türetilen **MediaTypeFormatter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="dfdf3-130">XML ve JSON için medya biçimlendiricileri Web API'si sağlar ve diğer medya türlerini desteklemek için özel biçimlendiricileri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="dfdf3-131">Özel bir biçimlendirici yazma hakkında daha fazla bilgi için bkz: [medya Biçimlendiricileri](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="dfdf3-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="dfdf3-132">Nasıl içerik anlaşması çalışır</span><span class="sxs-lookup"><span data-stu-id="dfdf3-132">How Content Negotiation Works</span></span>

<span data-ttu-id="dfdf3-133">İlk olarak, ardışık düzen alır **IContentNegotiator** gelen hizmet **HttpConfiguration** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="dfdf3-134">Da medya biçimlendiricileri listesini alır **HttpConfiguration.Formatters** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="dfdf3-135">Ardından, ardışık düzen çağırır **IContentNegotiatior.Negotiate**, geçen içinde:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="dfdf3-136">Serileştirilecek nesnenin türü</span><span class="sxs-lookup"><span data-stu-id="dfdf3-136">The type of object to serialize</span></span>
- <span data-ttu-id="dfdf3-137">Medya biçimlendiricileri koleksiyonu</span><span class="sxs-lookup"><span data-stu-id="dfdf3-137">The collection of media formatters</span></span>
- <span data-ttu-id="dfdf3-138">HTTP isteği</span><span class="sxs-lookup"><span data-stu-id="dfdf3-138">The HTTP request</span></span>

<span data-ttu-id="dfdf3-139">**Anlaş** yöntemi iki parça bilgi verir:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="dfdf3-140">Kullanmak için hangi biçimlendirici</span><span class="sxs-lookup"><span data-stu-id="dfdf3-140">Which formatter to use</span></span>
- <span data-ttu-id="dfdf3-141">Yanıt medya türü</span><span class="sxs-lookup"><span data-stu-id="dfdf3-141">The media type for the response</span></span>

<span data-ttu-id="dfdf3-142">Biçimlendirici bulunamazsa, **anlaş** yöntemi döndürür **null**ve istemci recevies HTTP Hatası 406 (kabul edilemez).</span><span class="sxs-lookup"><span data-stu-id="dfdf3-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="dfdf3-143">Aşağıdaki kod, nasıl bir denetleyici içerik anlaşması doğrudan çağırabileceği gösterir:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="dfdf3-144">Bu kod eşdeğerdir ardışık düzen otomatik olarak ne yapar.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="dfdf3-145">Varsayılan içerik Uzlaştırıcı</span><span class="sxs-lookup"><span data-stu-id="dfdf3-145">Default Content Negotiator</span></span>

<span data-ttu-id="dfdf3-146">**DefaultContentNegotiator** SAX varsayılan uygulaması **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="dfdf3-147">Bir biçimlendirici seçmek için çeşitli ölçütler kullanır.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="dfdf3-148">İlk olarak, biçimlendirici türü seri hale getiremiyor olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="dfdf3-149">Bu çağırarak doğrulanır **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="dfdf3-150">Ardından, içerik uzlaştırıcı her bir Biçimlendiricinin arar ve ne kadar iyi HTTP isteği eşleştiğini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="dfdf3-151">Eşleşme değerlendirmek için içerik uzlaştırıcı biçimlendirici üzerinde ikisinden görünür:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="dfdf3-152">**SupportedMediaTypes** desteklenen medya türlerinin bir listesini içeren koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="dfdf3-153">İçerik uzlaştırıcı bu listeyi Accept üstbilgisi isteği karşı eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="dfdf3-154">Accept üstbilgisi aralıkları dahil edebileceğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="dfdf3-155">Örneğin, "metin/düz" bir metin için eşleşmedir /\* veya \* / \*.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="dfdf3-156">**MediaTypeMappings** bir listesini içeren koleksiyon **MediaTypeMapping** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="dfdf3-157">**MediaTypeMapping** sınıfı HTTP isteklerini medya türleriyle eşleştirmek için genel bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="dfdf3-158">Örneğin, belirli bir medya türünün için özel bir HTTP üstbilgisi eşlemek.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="dfdf3-159">Varsa birden çok eşleşme yüksek kalite faktörü WINS ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="dfdf3-160">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="dfdf3-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="dfdf3-161">Bu örnekte, uygulama/json 1.0, bir kapsanan kalite faktörü sahiptir, böylece uygulama/xml tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="dfdf3-162">Herhangi bir eşleşme bulunmazsa, içerik uzlaştırıcı istek gövdesi medya türünü eşleştirmek varsa çalışır.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="dfdf3-163">Örneğin, istek JSON veri içeriyorsa, içerik uzlaştırıcı JSON biçimlendirici için arar.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="dfdf3-164">Hala herhangi bir eşleşme varsa, içerik uzlaştırıcı yalnızca türü serileştirebilen ilk biçimlendiriciyi alır.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="dfdf3-165">Karakter kodlamasını seçme</span><span class="sxs-lookup"><span data-stu-id="dfdf3-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="dfdf3-166">Bir biçimlendirici seçtikten sonra içerik uzlaştırıcı en iyi karakter bakarak kodlamasını seçer **SupportedEncodings** biçimlendirici ve bu isteği Accept-Charset üstbilgisinde (varsa) eşleştirme özelliği.</span><span class="sxs-lookup"><span data-stu-id="dfdf3-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
