---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 BSON desteği | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984589"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="f3bd7-102">ASP.NET Web API 2.1 BSON desteği</span><span class="sxs-lookup"><span data-stu-id="f3bd7-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="f3bd7-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f3bd7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f3bd7-104">Web API 2.1 BSON için destek sunar.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="f3bd7-105">Bu konuda, Web API denetleyicisi (sunucu tarafı) ve .NET istemci uygulaması BSON kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="f3bd7-106">BSON nedir?</span><span class="sxs-lookup"><span data-stu-id="f3bd7-106">What is BSON?</span></span>

<span data-ttu-id="f3bd7-107">[BSON](http://bsonspec.org/) ikili seri hale getirme biçimi.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="f3bd7-108">"BSON" "İçin ikili JSON" anlamına gelir, ancak BSON ve JSON çok farklı serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="f3bd7-109">BSON olan "JSON benzeri", nesneleri ad-değer çiftleri, JSON olarak benzer olarak temsil edilir çünkü.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="f3bd7-110">JSON, sayısal veri türleri değil dizeleri bayt olarak depolanır</span><span class="sxs-lookup"><span data-stu-id="f3bd7-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="f3bd7-111">BSON basit, tarama kolay ve hızlı kodlamak ve çözmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="f3bd7-112">JSON olarak boyutunda BSON karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="f3bd7-113">Verilere bağlı olarak, daha küçük veya daha büyük bir JSON yükü BSON yükü olabilir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="f3bd7-114">Base64 ile kodlanmış ikili veri olmadığı için bir görüntü dosyası gibi ikili verileri seri hale getirme için BSON JSON küçüktür.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="f3bd7-115">BSON belgeleri tarama bir Ayrıştırıcıyı kod çözme olmadan öğeleri atlayabilirsiniz öğeleri uzunluk alanı ile önek çünkü kolaydır.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="f3bd7-116">Sayısal veri türleri değil dizeleri sayı olarak depolandığından kodlama ve kod çözme, verimlidir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="f3bd7-117">.NET istemci uygulamaları gibi yerel istemcilerde JSON veya XML gibi metin tabanlı biçimler yerine BSON kullanarak yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="f3bd7-118">JavaScript doğrudan JSON yükü dönüştürebilirsiniz çünkü tarayıcı istemcileri için büyük olasılıkla JSON ile takılıyor istersiniz.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="f3bd7-119">Neyse ki, Web API'sini kullanan [içerik anlaşması](content-negotiation.md), API'nizi hem biçimleri için destek ve seçin istemci olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="f3bd7-120">BSON sunucu üzerindeki etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f3bd7-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="f3bd7-121">Web API yapılandırmanızda eklemek **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonuna.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="f3bd7-122">Şimdi istemci "uygulama/bson" isterse, Web API BSON biçimlendirici kullanın.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="f3bd7-123">BSON diğer medya türleri ile ilişkilendirmek için bunları SupportedMediaTypes koleksiyonuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="f3bd7-124">Aşağıdaki kod, desteklenen medya türleri için "application/vnd.contoso" ekler:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="f3bd7-125">Örnek HTTP oturumu</span><span class="sxs-lookup"><span data-stu-id="f3bd7-125">Example HTTP Session</span></span>

<span data-ttu-id="f3bd7-126">Bu örnek için aşağıdaki model sınıfı artı basit bir Web API denetleyicisi kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="f3bd7-127">Bir istemci aşağıdaki HTTP istek gönderebilir:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="f3bd7-128">Yanıt şöyledir:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="f3bd7-129">Burada t ikili verilerle değiştirilen &quot;.&quot; karakter.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="f3bd7-130">Aşağıdaki ekran Fiddler gösterir ham onaltılık değerler görüntüsü.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="f3bd7-131">BSON HttpClient ile kullanma</span><span class="sxs-lookup"><span data-stu-id="f3bd7-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="f3bd7-132">.NET istemci uygulamaları ile BSON biçimlendirici kullanabilir **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="f3bd7-133">Hakkında daha fazla bilgi için **HttpClient**, bkz: [bir Web API öğesinden bir .NET istemci çağırma](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="f3bd7-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="f3bd7-134">Aşağıdaki kod BSON kabul edip yanıt BSON yükünde seri durumdan çıkarır GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="f3bd7-135">BSON sunucudan istemek için "uygulama/bson" için Accept üstbilgisi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="f3bd7-136">Yanıt gövdesi seri durumdan çıkarılacak kullanmak **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="f3bd7-137">Yanıt gövdesi okurken belirtmeniz gerekir böylece bu Biçimlendiricinin varsayılan biçimlendiricileri koleksiyonunda değil:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="f3bd7-138">Sonraki örnek BSON içeren bir POST isteği göndermek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="f3bd7-139">Bu kodu çoğunu önceki örnekle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="f3bd7-140">Ancak **PostAsync** yöntemini belirtin **BsonMediaTypeFormatter** biçimlendirici olarak:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="f3bd7-141">Üst düzey ilkel türler seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="f3bd7-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="f3bd7-142">Anahtar/değer çiftlerinin listesini her BSON belgedir. BSON belirtimi tam sayı veya dize gibi tek bir ham değer serileştirmek için bir söz dizimi tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="f3bd7-143">Bu sınırlamaya geçici bir çözüm için **BsonMediaTypeFormatter** ilkel türler özel bir durum olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="f3bd7-144">Seri hale getirme önce bu değer bir anahtar/değer çifti "Value" anahtarla dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="f3bd7-145">Örneğin, API denetleyicisi bir tamsayı döndürür varsayın:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="f3bd7-146">Seri hale getirme önce BSON biçimlendirici bu aşağıdaki anahtar/değer çifti dönüştürür:</span><span class="sxs-lookup"><span data-stu-id="f3bd7-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="f3bd7-147">Seri durumdan aktarıldığında, biçimlendirici verileri özgün değere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="f3bd7-148">Ancak, web API ham değerler döndürürse farklı BSON ayrıştırıcı kullanan istemciler bu durumun üstesinden gerekir.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="f3bd7-149">Genel olarak, ham değerler yerine yapılandırılmış veri döndüren göz önünde bulundurmalısınız.</span><span class="sxs-lookup"><span data-stu-id="f3bd7-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3bd7-150">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f3bd7-150">Additional Resources</span></span>

[<span data-ttu-id="f3bd7-151">Web API BSON örneği</span><span class="sxs-lookup"><span data-stu-id="f3bd7-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="f3bd7-152">Medya Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="f3bd7-152">Media Formatters</span></span>](media-formatters.md)
