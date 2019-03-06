---
title: ASP.NET Core istek ve yanıt işlemlerinde
author: jkotalik
description: İstek gövdesini okuyup yanıt gövdesi içinde ASP.NET Core hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: f29c0aff95887d639cf3d1a8c8fe9f9228159f7a
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400997"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="95b9a-103">ASP.NET Core istek ve yanıt işlemlerinde</span><span class="sxs-lookup"><span data-stu-id="95b9a-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="95b9a-104">Tarafından [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="95b9a-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="95b9a-105">Bu makalede, gövdeden okuma ve yazma için yanıt gövdesini açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="95b9a-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="95b9a-106">Ara yazılım yazarken bu işlemler için kod yazmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="95b9a-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="95b9a-107">Aksi takdirde, genellikle işlemleri, MVC ve Razor sayfaları tarafından işlenir. çünkü bu kodu yazmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="95b9a-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="95b9a-108">ASP.NET Core 3. 0 ', istek ve yanıt gövdeleri için iki soyutlama vardır: <xref:System.IO.Stream> ve <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="95b9a-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="95b9a-109">İsteği okumak için [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) olduğu bir <xref:System.IO.Stream>, ve `HttpRequest.BodyPipe` olduğu bir <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="95b9a-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="95b9a-110">Yanıt yazmak için [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) olduğu bir `HttpResponse.BodyPipe` olduğu bir <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="95b9a-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="95b9a-111">İşlem hatları üzerinde akışları öneririz.</span><span class="sxs-lookup"><span data-stu-id="95b9a-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="95b9a-112">Akışları kullanmak için bazı basit işlemler daha kolay olabilir, ancak işlem hatları performans avantajı vardır ve çoğu senaryoda kullanmak daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="95b9a-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="95b9a-113">3. 0 ', işlem hatları akışları yerine dahili olarak kullanmak üzere ASP.NET Core başlatıyor.</span><span class="sxs-lookup"><span data-stu-id="95b9a-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="95b9a-114">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="95b9a-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="95b9a-115">Akışları kullanımdan kaldırılıyor değildir.</span><span class="sxs-lookup"><span data-stu-id="95b9a-115">Streams aren't going away.</span></span> <span data-ttu-id="95b9a-116">.NET içinde kullanılacak devam etmek ve birçok akış türleri gibi kanal eşdeğerleri olmayan `FileStreams` ve `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="95b9a-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="95b9a-117">Stream örnekleri</span><span class="sxs-lookup"><span data-stu-id="95b9a-117">Stream examples</span></span>

<span data-ttu-id="95b9a-118">Yeni satırlara bölme dizelerden oluşan bir liste olarak tüm istek gövdesini okuyan bir ara yazılım oluşturmak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="95b9a-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="95b9a-119">Basit bir akış uygulaması aşağıdaki örnekteki gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="95b9a-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="95b9a-120">Bu kod çalışır, ancak bazı sorunlar vardır:</span><span class="sxs-lookup"><span data-stu-id="95b9a-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="95b9a-121">Sonuna ekleme önce `StringBuilder`, başka bir dize örneği oluşturur (`encodedString`), harekete hemen hemen.</span><span class="sxs-lookup"><span data-stu-id="95b9a-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="95b9a-122">Sonucu ek bellek ayırma boyutu tüm istek gövdesi, bu nedenle bu işlem, tüm bayt akışı olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="95b9a-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="95b9a-123">Örneğin, yeni satırlara bölme önce dizenin tamamını okur.</span><span class="sxs-lookup"><span data-stu-id="95b9a-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="95b9a-124">Bayt dizisinde yeni satırları denetlemek için daha verimli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="95b9a-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="95b9a-125">Bu sorunlardan bazıları düzelten bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="95b9a-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="95b9a-126">Bu örnekte:</span><span class="sxs-lookup"><span data-stu-id="95b9a-126">This example:</span></span>

- <span data-ttu-id="95b9a-127">Tüm istek gövdesinde arabelleğe almayan bir `StringBuilder` olmadığı sürece herhangi bir yeni satır karakteri.</span><span class="sxs-lookup"><span data-stu-id="95b9a-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="95b9a-128">Değil çağırma `Split` dizesini.</span><span class="sxs-lookup"><span data-stu-id="95b9a-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="95b9a-129">Ancak, yine de bazı sorunlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="95b9a-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="95b9a-130">Yeni satır karakterleri seyrek, istek gövdesi çoğunu dizesini arabelleğe alınıp.</span><span class="sxs-lookup"><span data-stu-id="95b9a-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="95b9a-131">Dizeleri hala oluşturur (`remainingString`) ve bunları bir ek ayırmayı sonuçları dize arabelleğine ekler.</span><span class="sxs-lookup"><span data-stu-id="95b9a-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="95b9a-132">Bu sorun düzeltilebilir, ancak kod ile küçük geliştirme ve daha karmaşık hale.</span><span class="sxs-lookup"><span data-stu-id="95b9a-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="95b9a-133">İşlem hatları, çok az kod karmaşıklığı ile ilgili sorunları çözmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="95b9a-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="95b9a-134">İşlem hatları</span><span class="sxs-lookup"><span data-stu-id="95b9a-134">Pipelines</span></span>

<span data-ttu-id="95b9a-135">Aşağıdaki örnek, aynı senaryoyu nasıl olabileceğini gösterir kullanarak işlenen bir `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="95b9a-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="95b9a-136">Bu örnekte akışları uygulamaları olan çok sayıda sorunu giderir:</span><span class="sxs-lookup"><span data-stu-id="95b9a-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="95b9a-137">Gerek yoktur dize arabellek için çünkü `PipeReader` kullanılmamış bayt işler.</span><span class="sxs-lookup"><span data-stu-id="95b9a-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="95b9a-138">Kodlanmış dizeleri doğrudan döndürülen dizelerin listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="95b9a-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="95b9a-139">Dize oluşturulması dizesi tarafından kullanılan bellek yanı sıra ayırma ücretsiz (dışında `ToArray()` arayın).</span><span class="sxs-lookup"><span data-stu-id="95b9a-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="95b9a-140">Bağdaştırıcıları</span><span class="sxs-lookup"><span data-stu-id="95b9a-140">Adapters</span></span>

<span data-ttu-id="95b9a-141">Artık hem `Body` ve `BodyPipe` özellikler için kullanılabilir `HttpRequest` ve `HttpResponse`, ayarladığınızda ne `Body` farklı bir akışa?</span><span class="sxs-lookup"><span data-stu-id="95b9a-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="95b9a-142">3. 0'bağdaştırıcıları yeni bir dizi otomatik olarak diğer her tür uyarlayın.</span><span class="sxs-lookup"><span data-stu-id="95b9a-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="95b9a-143">Örneğin, ayarlarsanız `HttpRequest.Body` yeni bir akışa `HttpRequest.BodyPipe` otomatik olarak yeni bir için ayarlanır `PipeReader` sonuna geldik `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="95b9a-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="95b9a-144">Aynı davranışı ayarı için geçerli `BodyPipe` özelliği.</span><span class="sxs-lookup"><span data-stu-id="95b9a-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="95b9a-145">Varsa `HttpResponse.BodyPipe` yeni bir için ayarlanır `PipeWriter`, `HttpResponse.Body` sarmalayan yeni bir akış için otomatik olarak ayarlanır `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="95b9a-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="95b9a-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="95b9a-146">StartAsync</span></span>

<span data-ttu-id="95b9a-147">`HttpResponse.StartAsync` 3. 0'da yenidir.</span><span class="sxs-lookup"><span data-stu-id="95b9a-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="95b9a-148">Üstbilgileri değiştirilemeyen olduğunu gösterir ve çalıştırmak için kullanılan `OnStarting` geri çağırmalar.</span><span class="sxs-lookup"><span data-stu-id="95b9a-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="95b9a-149">Çağırmak zorunda 3.0-preview3, `StartAsync` kullanmadan önce `HttpRequest.BodyPipe`, gelecek sürümlerde bir öneri olacaktır.</span><span class="sxs-lookup"><span data-stu-id="95b9a-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="95b9a-150">Kullanmadan önce StartAsync Kestrel sunucusu olarak kullanırken, çağırma `PipeReader` tarafından döndürülen belleğin garanti `GetMemory` Kestrel için ait iç ait <xref:System.IO.Pipelines.Pipe> yerine bir dış arabellek.</span><span class="sxs-lookup"><span data-stu-id="95b9a-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95b9a-151">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="95b9a-151">Additional resources</span></span>

* [<span data-ttu-id="95b9a-152">System.IO.Pipelines ile tanışın</span><span class="sxs-lookup"><span data-stu-id="95b9a-152">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>