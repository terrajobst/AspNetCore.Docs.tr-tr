---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: "Medya Biçimlendiricileri ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="fb761-102">ASP.NET Web API 2 medya Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="fb761-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fb761-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fb761-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fb761-104">Bu öğretici gösterir, ASP.NET Web API'de ek medya biçimlerinin nasıl destekler.</span><span class="sxs-lookup"><span data-stu-id="fb761-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="fb761-105">Internet medya türleri</span><span class="sxs-lookup"><span data-stu-id="fb761-105">Internet Media Types</span></span>

<span data-ttu-id="fb761-106">Bir MIME türü olarak da adlandırılan bir medya türünün bir parça veri biçimini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb761-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="fb761-107">HTTP, ileti gövdesinin biçimi medya türleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb761-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="fb761-108">Bir medya türünün türünü ve alt olmak üzere iki dizeyi oluşur.</span><span class="sxs-lookup"><span data-stu-id="fb761-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="fb761-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fb761-109">For example:</span></span>

- <span data-ttu-id="fb761-110">metin/html</span><span class="sxs-lookup"><span data-stu-id="fb761-110">text/html</span></span>
- <span data-ttu-id="fb761-111">image/png</span><span class="sxs-lookup"><span data-stu-id="fb761-111">image/png</span></span>
- <span data-ttu-id="fb761-112">Uygulama/json</span><span class="sxs-lookup"><span data-stu-id="fb761-112">application/json</span></span>

<span data-ttu-id="fb761-113">HTTP iletisinin bir varlık gövdesi içeriyorsa, Content-Type üstbilgisi ileti gövdesinin biçimi belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb761-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="fb761-114">Bu, ileti gövdesi içeriğini nasıl alıcı bildirir.</span><span class="sxs-lookup"><span data-stu-id="fb761-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="fb761-115">Örneğin, bir HTTP yanıtının bir PNG resim içeriyorsa, yanıt aşağıdaki üst bilgiler olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb761-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="fb761-116">İstemci bir istek iletisi gönderirken bir Accept üstbilgisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fb761-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="fb761-117">Accept üstbilgisi, sunucunun istemci hangi medya türde sunucudan istediği söyler.</span><span class="sxs-lookup"><span data-stu-id="fb761-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="fb761-118">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fb761-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="fb761-119">Bu üst istemci HTML, XHTML veya XML istediği sunucusuna söyler.</span><span class="sxs-lookup"><span data-stu-id="fb761-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="fb761-120">Medya türü nasıl Web API serileştirir ve HTTP ileti gövdesi seri durumdan çıkarır belirler.</span><span class="sxs-lookup"><span data-stu-id="fb761-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="fb761-121">Web API'si XML, JSON, BSON ve form urlencoded verileri için yerleşik destek vardır ve yazarak ek medya türlerini destekleyebilir bir *medya biçimlendiricisi*.</span><span class="sxs-lookup"><span data-stu-id="fb761-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="fb761-122">Medya biçimlendiricisi oluşturmak için bu sınıfların birinden türetilen:</span><span class="sxs-lookup"><span data-stu-id="fb761-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="fb761-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb761-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="fb761-124">Bu sınıfı kullanır zaman uyumsuz okuma ve yazma yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="fb761-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="fb761-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb761-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="fb761-126">Bu sınıfın türetildiği **MediaTypeFormatter** ancak sychronous okuma/yazma yöntemleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb761-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="fb761-127">Türetme **BufferedMediaTypeFormatter** zaman uyumsuz kodu yok, ancak aynı zamanda çağıran iş parçacığı g/ç sırasında engelleme gelir basittir.</span><span class="sxs-lookup"><span data-stu-id="fb761-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="fb761-128">Örnek: bir CSV medya biçimlendiricisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb761-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="fb761-129">Aşağıdaki örnek, virgülle ayrılmış değerler (CSV) biçiminde bir Product nesnesi seri hale getirebilir medya türü biçimlendiricisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb761-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="fb761-130">Bu örnek öğreticide tanımlanan ürün türünü kullanan [CRUD işlemleri destekler bir Web API oluşturma](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="fb761-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="fb761-131">Ürün nesnesinin tanımı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fb761-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="fb761-132">Bir CSV biçimlendirici uygulamak için türeyen bir sınıf tanımlama **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="fb761-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="fb761-133">Oluşturucuda Biçimlendiricinin desteklediği medya türleriyle ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fb761-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="fb761-134">Bu örnekte, bir tek medya türü biçimlendirici destekleyen &quot;metin/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="fb761-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="fb761-135">Geçersiz kılma **CanWriteType** serialize yöntemi, biçimlendirici türlerini belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="fb761-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="fb761-136">Bu örnekte, biçimlendirici tek serileştirebilen `Product` nesneleri yanı sıra koleksiyonları `Product` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="fb761-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="fb761-137">Benzer şekilde, geçersiz kılma **CanReadType** hangi biçimlendirici türlerini belirtmek için yöntemi çıkarabilen.</span><span class="sxs-lookup"><span data-stu-id="fb761-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="fb761-138">Yöntem yalnızca döndürecek şekilde bu örnekte, biçimlendirici seri durumdan çıkarma, desteklemiyor **false**.</span><span class="sxs-lookup"><span data-stu-id="fb761-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="fb761-139">Son olarak, geçersiz kılma **WriteToStream** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb761-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="fb761-140">Bu yöntem, bir akışa yazarak bir türü serileştirir.</span><span class="sxs-lookup"><span data-stu-id="fb761-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="fb761-141">Ayrıca, biçimlendirici seri durumdan çıkarma destekliyorsa, geçersiz kılma **ReadFromStream** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb761-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="fb761-142">Web API ardışık medya biçimlendiricisi ekleme</span><span class="sxs-lookup"><span data-stu-id="fb761-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="fb761-143">Bir medya türü biçimlendiricisi Web API ardışık düzenine eklemek için kullanın **Biçimlendiricileri** özelliği **HttpConfiguration** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fb761-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="fb761-144">Karakter kodlamaları</span><span class="sxs-lookup"><span data-stu-id="fb761-144">Character Encodings</span></span>

<span data-ttu-id="fb761-145">İsteğe bağlı olarak, bir medya biçimlendiricisi UTF-8 veya ISO 8859-1 gibi birden çok karakter kodlamaları destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="fb761-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="fb761-146">Bir veya daha fazla oluşturucuda eklemek [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) türlerine **SupportedEncodings** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="fb761-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="fb761-147">Varsayılan ilk kodlama yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="fb761-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="fb761-148">İçinde **WriteToStream** ve **ReadFromStream** yöntemlerini çağırın [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) tercih edilen karakter kodlamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="fb761-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="fb761-149">Bu yöntem Desteklenen kodlamalar listesine karşı istek üstbilgileri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="fb761-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="fb761-150">Döndürülen kullanmak **kodlama** zaman okuma veya yazma akıştan:</span><span class="sxs-lookup"><span data-stu-id="fb761-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
