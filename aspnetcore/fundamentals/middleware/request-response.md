---
title: ASP.NET Core 'de istek ve yanıt işlemleri
author: jkotalik
description: İstek gövdesini okumayı ve yanıt gövdesini ASP.NET Core yazmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667218"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="b0a95-103">ASP.NET Core 'de istek ve yanıt işlemleri</span><span class="sxs-lookup"><span data-stu-id="b0a95-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="b0a95-104">, [Kotin Kotalik](https://github.com/jkotalik) tarafından</span><span class="sxs-lookup"><span data-stu-id="b0a95-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="b0a95-105">Bu makalede, istek gövdesinden okuma ve yanıt gövdesine yazma işlemleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b0a95-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="b0a95-106">Bu işlemler için kod, ara yazılım yazılırken gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="b0a95-107">İşlemler MVC ve Razor Pages tarafından işlendiği için, yazma ara yazılımı dışında, özel kod genellikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="b0a95-108">İstek ve yanıt gövdelerinde iki soyut vardır: <xref:System.IO.Stream> ve <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="b0a95-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="b0a95-109">İstek okuma için, [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) bir <xref:System.IO.Stream>ve `HttpRequest.BodyReader` bir <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="b0a95-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="b0a95-110">Yanıt yazma için [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) bir <xref:System.IO.Stream>ve `HttpResponse.BodyWriter` bir <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="b0a95-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="b0a95-111">İşlem [hatları](/dotnet/standard/io/pipelines) akışlar üzerinde önerilir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-111">[Pipelines](/dotnet/standard/io/pipelines) are recommended over streams.</span></span> <span data-ttu-id="b0a95-112">Akışlar bazı basit işlemler için daha kolay olabilir, ancak işlem hatları performans avantajına sahiptir ve çoğu senaryoda daha kolay kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0a95-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="b0a95-113">ASP.NET Core dahili akışlar yerine işlem hatlarını kullanmaya başlıyor.</span><span class="sxs-lookup"><span data-stu-id="b0a95-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="b0a95-114">Örneklere şunlar dahildir:</span><span class="sxs-lookup"><span data-stu-id="b0a95-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="b0a95-115">Akışlar çerçeveden kaldırılmıyor.</span><span class="sxs-lookup"><span data-stu-id="b0a95-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="b0a95-116">Akışlar .NET genelinde çalışmaya devam eder ve birçok akış türü `FileStreams` ve `ResponseCompression`gibi kanal eşdeğerlerine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="b0a95-117">Stream örnekleri</span><span class="sxs-lookup"><span data-stu-id="b0a95-117">Stream examples</span></span>

<span data-ttu-id="b0a95-118">Hedefin, yeni satırları bölerek tüm istek gövdesini bir dizeler listesi olarak okuyan bir ara yazılım oluşturduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="b0a95-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="b0a95-119">Basit bir akış uygulamasının aşağıdaki örnekteki gibi görünebilir:</span><span class="sxs-lookup"><span data-stu-id="b0a95-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="b0a95-120">Bu kod işe yarar ancak bazı sorunlar vardır:</span><span class="sxs-lookup"><span data-stu-id="b0a95-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="b0a95-121">`StringBuilder`eklemeden önce, örnek hemen oluşturulan başka bir dize (`encodedString`) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b0a95-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="b0a95-122">Bu işlem akıştaki tüm baytlar için gerçekleşir, bu nedenle sonuç, tüm istek gövdesinin boyutunun fazladan bellek ayırmasından oluşur.</span><span class="sxs-lookup"><span data-stu-id="b0a95-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="b0a95-123">Örnek, yeni satırlara bölmeden önce tüm dizeyi okur.</span><span class="sxs-lookup"><span data-stu-id="b0a95-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="b0a95-124">Bayt dizisindeki yeni satırları denetlemek daha etkilidir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="b0a95-125">Yukarıdaki sorunlardan bazılarını düzelten bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b0a95-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="b0a95-126">Bu önceki örnek:</span><span class="sxs-lookup"><span data-stu-id="b0a95-126">This preceding example:</span></span>

* <span data-ttu-id="b0a95-127">Herhangi bir yeni satır karakteri olmadıkça tüm istek gövdesini bir `StringBuilder` arabelleğe almaz.</span><span class="sxs-lookup"><span data-stu-id="b0a95-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="b0a95-128">Dizedeki `Split` çağırmaz.</span><span class="sxs-lookup"><span data-stu-id="b0a95-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="b0a95-129">Ancak, yine de bazı sorunlar vardır:</span><span class="sxs-lookup"><span data-stu-id="b0a95-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="b0a95-130">Yeni satır karakterleri seyrek ise, dize içinde istek gövdesinin büyük bir bölümü arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="b0a95-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="b0a95-131">Kod, dizeler oluşturmaya (`remainingString`) devam eder ve bunları dize arabelleğine ekler ve bu da ek bir ayırmaya neden olur.</span><span class="sxs-lookup"><span data-stu-id="b0a95-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="b0a95-132">Bu sorunlar düzeltilebilir, ancak kod çok daha karmaşık bir geliştirme sayesinde daha karmaşık hale geliyor.</span><span class="sxs-lookup"><span data-stu-id="b0a95-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="b0a95-133">İşlem hatları, en az kod karmaşıklığı ile bu sorunları çözmenin bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0a95-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="b0a95-134">İşlem hatları</span><span class="sxs-lookup"><span data-stu-id="b0a95-134">Pipelines</span></span>

<span data-ttu-id="b0a95-135">Aşağıdaki örnek, `PipeReader`kullanarak aynı senaryonun nasıl işlenebileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="b0a95-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="b0a95-136">Bu örnek, akışlar uygulamalarında bulunan birçok sorunu düzeltir:</span><span class="sxs-lookup"><span data-stu-id="b0a95-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="b0a95-137">`PipeReader` kullanılmayan baytları işlediği için bir dize arabelleği gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b0a95-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="b0a95-138">Kodlanmış dizeler doğrudan döndürülen dizeler listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="b0a95-139">Dize oluşturma, dize tarafından kullanılan belleğin yanı sıra (`ToArray()` çağrısı dışında) ayırma ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="b0a95-140">Örünü</span><span class="sxs-lookup"><span data-stu-id="b0a95-140">Adapters</span></span>

<span data-ttu-id="b0a95-141">Hem `Body` hem de `BodyReader/BodyWriter` özellikleri `HttpRequest` ve `HttpResponse`için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0a95-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="b0a95-142">Farklı bir akışa `Body` ayarladığınızda, yeni bir bağdaştırıcılar kümesi otomatik olarak her türü birbirlerine uyarlar.</span><span class="sxs-lookup"><span data-stu-id="b0a95-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="b0a95-143">`HttpRequest.Body` yeni bir akışa ayarlarsanız, `HttpRequest.BodyReader` otomatik olarak `HttpRequest.Body`sarmalanmış yeni bir `PipeReader` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b0a95-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="b0a95-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="b0a95-144">StartAsync</span></span>

<span data-ttu-id="b0a95-145">`HttpResponse.StartAsync`, üst bilgilerin değiştirilemeyen ve `OnStarting` geri çağırmaların çalıştırılacağı olduğunu göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0a95-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="b0a95-146">Sunucu olarak Kestrel kullanırken, `PipeReader` kullanmadan önce `StartAsync` çağırmak, `GetMemory` tarafından döndürülen belleğin dış bir arabellek yerine Kestrel 'in iç <xref:System.IO.Pipelines.Pipe> ait olduğunu garanti eder.</span><span class="sxs-lookup"><span data-stu-id="b0a95-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0a95-147">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0a95-147">Additional resources</span></span>

* [<span data-ttu-id="b0a95-148">System. ıO. işlem hatlarını tanıtma</span><span class="sxs-lookup"><span data-stu-id="b0a95-148">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
