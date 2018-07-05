---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385706"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="b1f94-102">ASP.NET Web API 2.1 sürümündeki yenilikler</span><span class="sxs-lookup"><span data-stu-id="b1f94-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="b1f94-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b1f94-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="b1f94-104">Bu konuda, ASP.NET Web API 2.1 için Yenilikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b1f94-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="b1f94-105">İndir</span><span class="sxs-lookup"><span data-stu-id="b1f94-105">Download</span></span>](#download)
- [<span data-ttu-id="b1f94-106">Belgeler</span><span class="sxs-lookup"><span data-stu-id="b1f94-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="b1f94-107">ASP.NET Web API 2.1 sürümünde yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="b1f94-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="b1f94-108">Genel hata işleme</span><span class="sxs-lookup"><span data-stu-id="b1f94-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="b1f94-109">Öznitelik yönlendirme geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="b1f94-110">Yardım sayfası geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="b1f94-111">IgnoreRoute desteği</span><span class="sxs-lookup"><span data-stu-id="b1f94-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="b1f94-112">BSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="b1f94-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="b1f94-113">Zaman uyumsuz filtreleri için daha iyi destek</span><span class="sxs-lookup"><span data-stu-id="b1f94-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="b1f94-114">Sorgu için istemci kitaplığı biçimlendirme ayrıştırma</span><span class="sxs-lookup"><span data-stu-id="b1f94-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="b1f94-115">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="b1f94-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="b1f94-116">Hata düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="b1f94-117">İndir</span><span class="sxs-lookup"><span data-stu-id="b1f94-117">Download</span></span>

<span data-ttu-id="b1f94-118">Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="b1f94-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="b1f94-119">Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi.</span><span class="sxs-lookup"><span data-stu-id="b1f94-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="b1f94-120">ASP.NET Web API 2.1 RTM paketin en son sürümü var: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="b1f94-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="b1f94-121">Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="b1f94-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="b1f94-122">Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="b1f94-123">NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b1f94-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="b1f94-124">Belgeler</span><span class="sxs-lookup"><span data-stu-id="b1f94-124">Documentation</span></span>

<span data-ttu-id="b1f94-125">Öğreticiler ve ASP.NET Web API 2.1 RTM ile ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="b1f94-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="b1f94-126">ASP.NET Web API 2.1 sürümünde yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="b1f94-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="b1f94-127">Genel hata işleme</span><span class="sxs-lookup"><span data-stu-id="b1f94-127">Global Error Handling</span></span>

<span data-ttu-id="b1f94-128">Merkezi bir mekanizma aracılığıyla tüm işlenmemiş özel durumlar artık kaydedilebilecek ve işlenmeyen özel durumlar için davranış özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="b1f94-129">Framework işlenmeyen özel durum ve bu, örneğin sırada işlenmekte olan isteği gerçekleştiği bağlamı hakkında bilgi tüm, birden çok özel durum günlükçüleri destekler.</span><span class="sxs-lookup"><span data-stu-id="b1f94-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="b1f94-130">Örneğin, aşağıdaki kod, tüm işlenmemiş özel durumları günlüğe kaydetmek için System.Diagnostics.TraceSource kullanır:</span><span class="sxs-lookup"><span data-stu-id="b1f94-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="b1f94-131">Varsayılan özel durum işleyicisini de değiştirebilir, böylece işlenmeyen bir özel durum, gönderilen HTTP yanıt iletisi tam olarak özelleştirebilirsiniz gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="b1f94-132">Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , popüler ELMAH çerçevesi aracılığıyla tüm işlenmemiş özel durumları günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b1f94-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="b1f94-133">Öznitelik yönlendirme geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="b1f94-134">Öznitelik şimdi yönlendirme kısıtlamaları, sürüm oluşturma ve üst bilgi tabanlı yol seçimi etkinleştirme destekler.</span><span class="sxs-lookup"><span data-stu-id="b1f94-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="b1f94-135">Ayrıca, öznitelik rotaları birçok yönden artık özelleştirilebilir aracılığıyla **IDirectRouteFactory** arabirimi ve **RouteFactoryAttribute** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b1f94-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="b1f94-136">Rota öneki ile Genişletilebilir **IRoutePrefix** arabirimi ve **RoutePrefixAttribute** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b1f94-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="b1f94-137">Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) denetleyicileri bir 'api-version' HTTP üst bilgisi tarafından dinamik olarak filtrelemek için kısıtlamaları kullanan.</span><span class="sxs-lookup"><span data-stu-id="b1f94-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="b1f94-138">Yardım sayfası geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-138">Help Page Improvements</span></span>

<span data-ttu-id="b1f94-139">Web API 2.1 aşağıdaki geliştirmeleri içerir [API Yardım sayfaları](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="b1f94-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="b1f94-140">Parametre veya dönüş türleri eylemlerin özelliklerini bireysel belgeleri.</span><span class="sxs-lookup"><span data-stu-id="b1f94-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="b1f94-141">Veri modelini ek belgeleri.</span><span class="sxs-lookup"><span data-stu-id="b1f94-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="b1f94-142">Kullanıcı Arabirimi tasarımı Yardım sayfaları, ayrıca, bu değişiklikleri uyum sağlamak için güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="b1f94-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="b1f94-143">IgnoreRoute desteği</span><span class="sxs-lookup"><span data-stu-id="b1f94-143">IgnoreRoute Support</span></span>

<span data-ttu-id="b1f94-144">Web API 2.1 destekleyen Web API'si, bir dizi aracılığıyla akışında URL desenleri yoksayılıyor **IgnoreRoute** üzerinde genişletme yöntemleri **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="b1f94-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="b1f94-145">Bu yöntemler Web API'si, belirtilen bir şablonu eşleşen herhangi bir URL yok saymak neden ve uygun durumlarda ek işleme uygulamak konak izin verin.</span><span class="sxs-lookup"><span data-stu-id="b1f94-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="b1f94-146">Aşağıdaki örnek ile başlayan bir URI'leri yoksayar bir &quot;içeriği&quot; segment:</span><span class="sxs-lookup"><span data-stu-id="b1f94-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="b1f94-147">BSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="b1f94-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="b1f94-148">Web API destekler [BSON](http://bsonspec.org/) kablo biçimini, istemci ve sunucu.</span><span class="sxs-lookup"><span data-stu-id="b1f94-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="b1f94-149">Sunucu tarafında BSON etkinleştirmek için eklemeniz **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonu için:</span><span class="sxs-lookup"><span data-stu-id="b1f94-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="b1f94-150">.NET istemci BSON biçimi nasıl tüketebileceği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b1f94-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="b1f94-151">Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) istemci ve sunucu tarafı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="b1f94-152">Daha fazla bilgi için [Web API 2.1 sürümünde BSON desteği](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="b1f94-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="b1f94-153">Zaman uyumsuz filtreleri için daha iyi destek</span><span class="sxs-lookup"><span data-stu-id="b1f94-153">Better Support for Async Filters</span></span>

<span data-ttu-id="b1f94-154">Web API'si, zaman uyumsuz olarak yürütülen bir filtre oluşturmak için kolay bir yol artık desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="b1f94-155">Bu özellik yararlıdır filtrenizle veritabanı erişimi gibi bir zaman uyumsuz eylemi gerçekleştirmek gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="b1f94-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="b1f94-156">Filtre taban sınıfları yalnızca zaman uyumlu yöntemler açıkta olduğundan, zaman uyumsuz bir filtre oluşturmak için kendiniz filtre arabirim uygulamak vardı.</span><span class="sxs-lookup"><span data-stu-id="b1f94-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="b1f94-157">Şimdi sanal kılabilirsiniz `On*Async` filtrenin yöntemleri taban sınıf.</span><span class="sxs-lookup"><span data-stu-id="b1f94-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="b1f94-158">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b1f94-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="b1f94-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, ve **ExceptionFilterAttribute** sınıfları tüm Web API 2.1 içinde zaman uyumsuz destekler.</span><span class="sxs-lookup"><span data-stu-id="b1f94-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="b1f94-160">Sorgu için istemci kitaplığı biçimlendirme ayrıştırma</span><span class="sxs-lookup"><span data-stu-id="b1f94-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="b1f94-161">Daha önce **Microsoft.ASPNET.webapi.Client** ayrıştırma ve sunucu tarafı kodu için URI sorguları güncelleme desteklenir, ancak eşdeğer taşınabilir kitaplık bu özelliği eksik.</span><span class="sxs-lookup"><span data-stu-id="b1f94-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="b1f94-162">Web API 2.1 içinde bir istemci uygulaması artık kolayca ayrıştırma ve bir sorgu dizesi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b1f94-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="b1f94-163">Aşağıdaki örnekler ayrıştırma, değiştirebilir ve URI sorgu oluşturma işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="b1f94-164">(Örnekler basit bir konsol uygulaması gösterir.)</span><span class="sxs-lookup"><span data-stu-id="b1f94-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="b1f94-165">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="b1f94-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="b1f94-166">Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.1 RTM bozucu değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b1f94-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="b1f94-167">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="b1f94-167">Attribute Routing</span></span>

<span data-ttu-id="b1f94-168">Öznitelik yönlendirme eşleşme belirsizliklerden artık ilk eşleşme seçmek yerine bir hata bildirir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="b1f94-169">Öznitelik rotaları kullanmaları yasaktır *{denetleyici}* parametresi ve kullanarak *{action}* rota parametresi eylemleri yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="b1f94-170">Bu parametreler çok büyük olasılıkla belirsizliğe neden olur.</span><span class="sxs-lookup"><span data-stu-id="b1f94-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="b1f94-171">Yapı iskelesi MVC/Web API projesine 5.0 paketler için projede mevcut 5.1 paketleri sonuçları</span><span class="sxs-lookup"><span data-stu-id="b1f94-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="b1f94-172">ASP.NET Web API 2.1 RTM için NuGet paketlerini güncelleştirme, Visual Studio Araçları gibi ASP.NET iskeleti oluşturma veya ASP.NET Web uygulaması proje şablonu güncelleştirmez.</span><span class="sxs-lookup"><span data-stu-id="b1f94-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="b1f94-173">ASP.NET çalışma zamanı paketlerini (5.0.0.0) önceki sürümünü kullanırlar.</span><span class="sxs-lookup"><span data-stu-id="b1f94-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="b1f94-174">Sonuç olarak, zaten projelerinizde kullanılabilir yoksa ASP.NET iskeleti oluşturma önceki sürümünü (5.0.0.0) gerekli paketleri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b1f94-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="b1f94-175">Ancak, en son paketlerin projelerinizde te ASP.NET iskeleti oluşturma veya güncelleştirme 1'Visual Studio 2013 RTM üzerine yazılmaz.</span><span class="sxs-lookup"><span data-stu-id="b1f94-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="b1f94-176">Web API 2.1 veya ASP.NET MVC 5.1 paketleri güncelleştirdikten sonra ASP.NET iskeleti oluşturma kullanırsanız, Web API ve MVC sürümleri tutarlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="b1f94-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="b1f94-177">Tür yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="b1f94-177">Type Renames</span></span>

<span data-ttu-id="b1f94-178">Bazı öznitelik yönlendirme genişletilebilirliği için kullanılan türleri RC'den 2.1 RTM'ye değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="b1f94-179">Eski tür adı (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="b1f94-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="b1f94-180">Yeni tür adı (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="b1f94-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="b1f94-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="b1f94-181">IDirectRouteProvider</span></span> | <span data-ttu-id="b1f94-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="b1f94-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="b1f94-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="b1f94-183">RouteProviderAttribute</span></span> | <span data-ttu-id="b1f94-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="b1f94-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="b1f94-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="b1f94-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="b1f94-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="b1f94-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="b1f94-187">Özel durum filtreleri, zaman uyumsuz eylemleri oluşan toplam özel durum unwrap değil</span><span class="sxs-lookup"><span data-stu-id="b1f94-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="b1f94-188">Daha önce bir zaman uyumsuz eylem oluşturduysa bir **AggregateException**, özel durum filtresi özel durum sarmalamadan çıkarma ve **OnException** temel özel durum elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="b1f94-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="b1f94-189">2.1 içinde özel durum filtresi, unwrap değil ve **OnException** özgün alır **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="b1f94-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="b1f94-190">Hata Düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="b1f94-190">Bug Fixes</span></span>

<span data-ttu-id="b1f94-191">Bu sürüm ayrıca çeşitli hata düzeltmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="b1f94-192">Tam listesini burada bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b1f94-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="b1f94-193">5.1.0 paket</span><span class="sxs-lookup"><span data-stu-id="b1f94-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="b1f94-194">5.1.1 paket</span><span class="sxs-lookup"><span data-stu-id="b1f94-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="b1f94-195">5.1.2 paket IntelliSense güncelleştirme, ancak hiçbir hata düzeltmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b1f94-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
