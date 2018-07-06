---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2'deki medya Biçimlendiricileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9776adee19a62be4be38a4fb963c4978da3ed912
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803291"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="0fb06-102">ASP.NET Web API 2'deki medya Biçimlendiricileri</span><span class="sxs-lookup"><span data-stu-id="0fb06-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0fb06-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0fb06-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0fb06-104">Bu öğreticide, ASP.NET Web API'de ek medya biçimleri için destek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="0fb06-105">Internet medya türleri</span><span class="sxs-lookup"><span data-stu-id="0fb06-105">Internet Media Types</span></span>

<span data-ttu-id="0fb06-106">Bir MIME türü olarak da bilinen bir medya türü bir veri parçasını biçimini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0fb06-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="0fb06-107">HTTP, ileti gövdesinin biçimi medya türleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0fb06-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="0fb06-108">Medya türü, iki dizeyi, bir tür ve alt oluşur.</span><span class="sxs-lookup"><span data-stu-id="0fb06-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="0fb06-109">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0fb06-109">For example:</span></span>

- <span data-ttu-id="0fb06-110">metin/html</span><span class="sxs-lookup"><span data-stu-id="0fb06-110">text/html</span></span>
- <span data-ttu-id="0fb06-111">Görüntü/png</span><span class="sxs-lookup"><span data-stu-id="0fb06-111">image/png</span></span>
- <span data-ttu-id="0fb06-112">Uygulama/json</span><span class="sxs-lookup"><span data-stu-id="0fb06-112">application/json</span></span>

<span data-ttu-id="0fb06-113">HTTP iletisinin Varlık gövdesi içeriyorsa, Content-Type üst bilgisi, ileti gövdesinin biçimi belirtir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="0fb06-114">Bu, ileti gövdesi içeriğini ayrıştırmayı alıcı bildirir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="0fb06-115">Örneğin, bir HTTP yanıtı bir PNG görüntüsü içeriyorsa, yanıt aşağıdaki üst bilgileri olabilir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="0fb06-116">İstemci bir istek iletisi gönderirken bir Accept üst bilgisi dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0fb06-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="0fb06-117">Accept üst bilgisi, sunucunun istemci medya türde sunucudan istediği söyler.</span><span class="sxs-lookup"><span data-stu-id="0fb06-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="0fb06-118">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0fb06-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="0fb06-119">Bu üst bilgi, istemci HTML, XHTML veya XML istediğini sunucu söyler.</span><span class="sxs-lookup"><span data-stu-id="0fb06-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="0fb06-120">Web API'sini nasıl serileştirir ve HTTP ileti gövdesi seri durumdan çıkarır medya türünü belirler.</span><span class="sxs-lookup"><span data-stu-id="0fb06-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="0fb06-121">Web API'si, XML, JSON, BSON ve form-urlencoded verileri için yerleşik desteği vardır ve ek medya türleri yazarak destekleyebilir bir *medya biçimlendiricisi*.</span><span class="sxs-lookup"><span data-stu-id="0fb06-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="0fb06-122">Bir medya biçimlendiricisi oluşturmak için bu sınıflardan birine türetilir:</span><span class="sxs-lookup"><span data-stu-id="0fb06-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="0fb06-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb06-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="0fb06-124">Bu sınıfı kullanan zaman uyumsuz okuma ve yazma yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="0fb06-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="0fb06-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb06-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="0fb06-126">Bu sınıfın türetildiği **MediaTypeFormatter** ancak sychronous okuma/yazma yöntemleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="0fb06-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="0fb06-127">Öğesinden türetme **BufferedMediaTypeFormatter** çünkü zaman uyumsuz kod yok, ancak çağıran iş parçacığını g/ç sırasında engellemek geldiğini de kolaydır.</span><span class="sxs-lookup"><span data-stu-id="0fb06-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="0fb06-128">Örnek: bir CSV medya biçimlendiricisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0fb06-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="0fb06-129">Aşağıdaki örnek, bir virgülle ayrılmış değerler (CSV) biçiminde bir ürün nesnesine serileştirebilen bir medya türü biçimlendiricisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="0fb06-130">Bu örnekte bu öğreticide tanımlanan ürün türü [bu destekler CRUD işlemleri bir Web API'si oluşturma](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="0fb06-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="0fb06-131">Ürün nesnesinin tanımı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0fb06-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="0fb06-132">Bir CSV biçimlendirici uygulamak için türetilen bir sınıf tanımlama **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="0fb06-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="0fb06-133">Oluşturucuda Biçimlendiricinin desteklediği medya türleriyle ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0fb06-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="0fb06-134">Bu örnekte, bir tek bir medya türü biçimlendirici destekler &quot;metin/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="0fb06-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="0fb06-135">Geçersiz kılma **CanWriteType** serialize yöntemi, biçimlendirici türlerini belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="0fb06-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="0fb06-136">Bu örnekte, biçimlendirici tek serileştirebiliyorsa `Product` nesneleri koleksiyonlarının yanı sıra `Product` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="0fb06-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="0fb06-137">Benzer şekilde, geçersiz kılma **CanReadType** yöntemi, biçimlendirici türlerini belirtmek için seri durumdan çıkarabilen.</span><span class="sxs-lookup"><span data-stu-id="0fb06-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="0fb06-138">Yöntemi yalnızca döndürecek şekilde bu örnekte, biçimlendirici seri durumundan çıkarma desteklemiyor **false**.</span><span class="sxs-lookup"><span data-stu-id="0fb06-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="0fb06-139">Son olarak, geçersiz kılma **WriteToStream** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0fb06-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="0fb06-140">Bu yöntem bir tür yazarak bir akışa serileştirir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="0fb06-141">Ayrıca, biçimlendirici seri durumundan çıkarma destekliyorsa, geçersiz kılma **ReadFromStream** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0fb06-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="0fb06-142">Bir medya biçimlendiricisi Web API ardışık düzenine eklemek için</span><span class="sxs-lookup"><span data-stu-id="0fb06-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="0fb06-143">Bir medya türü biçimlendiricisi Web API ardışık düzenine eklemek için **Biçimlendiricileri** özelliği **HttpConfiguration** nesne.</span><span class="sxs-lookup"><span data-stu-id="0fb06-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="0fb06-144">Karakter kodlamaları</span><span class="sxs-lookup"><span data-stu-id="0fb06-144">Character Encodings</span></span>

<span data-ttu-id="0fb06-145">İsteğe bağlı olarak, bir medya biçimlendiricisi UTF-8 veya ISO 8859-1 gibi birden çok karakter kodlamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="0fb06-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="0fb06-146">Oluşturucusunun içinde bir veya daha fazla Ekle [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) türlerine **SupportedEncodings** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="0fb06-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="0fb06-147">Varsayılan ilk kodlama yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="0fb06-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="0fb06-148">İçinde **WriteToStream** ve **ReadFromStream** yöntemleri çağırmak [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) tercih edilen karakter kodlamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="0fb06-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="0fb06-149">Bu yöntem Desteklenen kodlamalar listesiyle istek üstbilgilerini eşleşir.</span><span class="sxs-lookup"><span data-stu-id="0fb06-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="0fb06-150">Döndürülen kullanın **kodlama** okuma veya yazma akıştan zaman:</span><span class="sxs-lookup"><span data-stu-id="0fb06-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
