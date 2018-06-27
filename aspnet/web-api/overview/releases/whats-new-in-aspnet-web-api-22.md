---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961305"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="287a2-102">ASP.NET Web API 2.2 yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="287a2-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="287a2-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="287a2-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="287a2-104">Bu konuda, ASP.NET Web API 2.2 yenilikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="287a2-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="287a2-105">İndir</span><span class="sxs-lookup"><span data-stu-id="287a2-105">Download</span></span>](#download)
- [<span data-ttu-id="287a2-106">Belgeler</span><span class="sxs-lookup"><span data-stu-id="287a2-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="287a2-107">ASP.NET Web API 2.2 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="287a2-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="287a2-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="287a2-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="287a2-109">Yönlendirme geliştirmeleri özniteliği</span><span class="sxs-lookup"><span data-stu-id="287a2-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="287a2-110">Windows Phone 8.1 için Web API İstemci Desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="287a2-111">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="287a2-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="287a2-112">Hata düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="287a2-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="287a2-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="287a2-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="287a2-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="287a2-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="287a2-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="287a2-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="287a2-116">İndir</span><span class="sxs-lookup"><span data-stu-id="287a2-116">Download</span></span>

<span data-ttu-id="287a2-117">Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="287a2-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="287a2-118">Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi.</span><span class="sxs-lookup"><span data-stu-id="287a2-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="287a2-119">En son ASP.NET Web API 2.2 paketini aşağıdaki sürümü vardır: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="287a2-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="287a2-120">Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="287a2-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="287a2-121">Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.</span><span class="sxs-lookup"><span data-stu-id="287a2-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="287a2-122">Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="287a2-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="287a2-123">Belgeler</span><span class="sxs-lookup"><span data-stu-id="287a2-123">Documentation</span></span>

<span data-ttu-id="287a2-124">Öğreticiler ve ASP.NET Web API 2.2 ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="287a2-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="287a2-125">ASP.NET Web API 2.2 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="287a2-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="287a2-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="287a2-126">OData v4</span></span>

<span data-ttu-id="287a2-127">Bu sürüm OData v4 protokolü için desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="287a2-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="287a2-128">Daha fazla bilgi için bkz: [Web API OData v4 belgeleri.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="287a2-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="287a2-129">Anahtar özellikleri ve değişiklikleri OData v4 için bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="287a2-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="287a2-130">OData modelinde yumuşatma özellikleri için destek</span><span class="sxs-lookup"><span data-stu-id="287a2-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="287a2-131">ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute ve ODataConventionModelBuilder ConcurrencyCheckAttribute desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="287a2-132">Eylemler için kolay başlık sağlamak için olanağı sunar.</span><span class="sxs-lookup"><span data-stu-id="287a2-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="287a2-133">ODL UriParser ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="287a2-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="287a2-134">Desteği [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [kapsama](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) ve [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="287a2-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="287a2-135">İlkel türler için cast desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="287a2-136">OData işlevi desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="287a2-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="287a2-137">İşlev çağrıları için destek parametre diğer adlar</span><span class="sxs-lookup"><span data-stu-id="287a2-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="287a2-138">Ortası büyük küçük harf adlandırma kuralını modelde desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="287a2-139">$Filter öğesinde cast() desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="287a2-140">Açık karmaşık tür için destek</span><span class="sxs-lookup"><span data-stu-id="287a2-140">Support for open complex type</span></span>
- <span data-ttu-id="287a2-141">Kaldırılan EntitySetController ve AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="287a2-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="287a2-142">Değiştirilen $link için $ref</span><span class="sxs-lookup"><span data-stu-id="287a2-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="287a2-143">Öznitelik yönlendirme desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="287a2-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="287a2-144">OData çekirdek kitaplıkları 6.4.0 kullanır</span><span class="sxs-lookup"><span data-stu-id="287a2-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="287a2-145">Yönlendirme geliştirmeleri özniteliği</span><span class="sxs-lookup"><span data-stu-id="287a2-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="287a2-146">Öznitelik şimdi yönlendirme öznitelik rotaları nasıl bulunan ve yapılandırılmış üzerinde tam denetim verir IDirectRouteProvider adlı bir genişletilebilirlik noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="287a2-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="287a2-147">Bir IDirectRouteProvider eylemlerin ve denetleyicilerin tam olarak hangi yönlendirme yapılandırması için bu eylemleri istendiğini belirtmek için ilişkili rota bilgilerle birlikte listesini sağlamaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="287a2-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="287a2-148">IDirectRouteProvider uygulaması MapAttributes/MapHttpAttributeRoutes çağrılırken belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="287a2-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="287a2-149">IDirectRouteProvider özelleştirme DefaultDirectRouteProvider bizim varsayılan uygulaması genişleterek kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="287a2-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="287a2-150">Bu sınıf öznitelikleri bulma, rota girişleri oluşturmak ve rota öneki ve alan öneki keşfetmek için mantığı değiştirmek için ayrı geçersiz kılınabilir sanal yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="287a2-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="287a2-151">Bu yeni genişletilebilirlik noktası ile ne yapabilir ilişkin bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="287a2-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="287a2-152">Rota özniteliklerin destek devralma</span><span class="sxs-lookup"><span data-stu-id="287a2-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="287a2-153">Örnek:</span><span class="sxs-lookup"><span data-stu-id="287a2-153">Example:</span></span>

    <span data-ttu-id="287a2-154">Bir istek LIKE "/ değerleri/api/10" "Başarı: 10" başarıyla burada döndürür</span><span class="sxs-lookup"><span data-stu-id="287a2-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="287a2-155">İstediğiniz gibi bazı kuralı izleyerek, öznitelik rotaları için varsayılan bir rota adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="287a2-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="287a2-156">Varsayılan olarak, öznitelik yönlendirme öznitelik rotaları için adları otomatik olarak oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="287a2-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="287a2-157">Bunlar rota tablosunda şunun önce öznitelik rotaları rota şablonu merkezi bir yerde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="287a2-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="287a2-158">Windows Phone 8.1 için Web API İstemci Desteği</span><span class="sxs-lookup"><span data-stu-id="287a2-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="287a2-159">Windows Phone 8.1 hedeflerken veya gelen Web API İstemci mantığınızı uygulamak için Web API İstemci NuGet paketi şimdi kullanabileceğiniz bir evrensel uygulama içinde.</span><span class="sxs-lookup"><span data-stu-id="287a2-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="287a2-160">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="287a2-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="287a2-161">Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.2 önemli değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="287a2-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="287a2-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="287a2-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="287a2-163">Model Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="287a2-163">Model builder</span></span>

<span data-ttu-id="287a2-164">Sorun: Aşırı yüklenmiş işlevlerin Functionımport olarak açığa çıkabileceği değil</span><span class="sxs-lookup"><span data-stu-id="287a2-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="287a2-165">2 aşırı yüklenmiş işlevlerin vardır ve bunlar da System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) sonuçlarında isteyen aşağıda gösterildiği gibi Functionımport durumunda.</span><span class="sxs-lookup"><span data-stu-id="287a2-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="287a2-166">Geçici çözüm: Bu sorun için geçici çözüm hem de, bir işlev aşırı yüklemelerinin FunctionImports eklemektir.</span><span class="sxs-lookup"><span data-stu-id="287a2-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="287a2-167">OData yönlendirme</span><span class="sxs-lookup"><span data-stu-id="287a2-167">OData Routing</span></span>

<span data-ttu-id="287a2-168">Eğik çizgi (% 2F) arasında URL dize değişmez değerleri kodlanmış ve OData kaynak yollarında kullanıldığında backslash(%5C) neden 404 hatası.</span><span class="sxs-lookup"><span data-stu-id="287a2-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="287a2-169">Örneğin, dize değişmez değerleri OData kaynak yollarında işlevlerin parametreleri ya da varlık kümelerinin anahtar değerleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="287a2-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="287a2-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="287a2-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="287a2-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="287a2-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="287a2-172">Bu tür istekleri konakları kaydını kaçış Hizmetleri aldığınızda, bu Web API çalışma zamanı için geçirmeden önce kaçış dizilerine.</span><span class="sxs-lookup"><span data-stu-id="287a2-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="287a2-173">Bu, aşağıdaki gibi saldırılarına karşı korur:</span><span class="sxs-lookup"><span data-stu-id="287a2-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="287a2-174">Bu, 404 hatası (bulunamadı) dönmek Web API OData yığını neden olur.</span><span class="sxs-lookup"><span data-stu-id="287a2-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="287a2-175">Bu hatayı önlemek için istemci ters eğik çizgi (% 255 C) ve çift kaçış sıralarına eğik çizgi işareti (% 252F) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="287a2-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="287a2-176">Bu sorgu dizelerine /Employees gibi gerçekleşmez? $filter adı eq 'Adı % 2F' =</span><span class="sxs-lookup"><span data-stu-id="287a2-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="287a2-177">**Atlanmamış eğik çizgi ('/') not edin ve ters eğik çizgi (") içinde OData kaynak yolu dize değişmez değerleri geçerli değildir. Eğik çizgi yalnızca yolu ayırıcısı olarak görüntülenmelidir ve ters eğik çizgi OData kaynak yolu hiç görünmemesi gerekir. (Her ikisi de bir OData sorgu dizesi bazı kısımları kullanılabilir.)**</span><span class="sxs-lookup"><span data-stu-id="287a2-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="287a2-178">Geçici çözüm: Gerçekte bunları ayrıştırmadan önce eğik çizgi ve dize değişmez değerleri ters eğik çizgiyi kaçınmak için DefaultODataPathHandler ayrıştırma yöntemi geçersiz.</span><span class="sxs-lookup"><span data-stu-id="287a2-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="287a2-179">Bu yaklaşımın burada bir örnek bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="287a2-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="287a2-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="287a2-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="287a2-181">[Sorgulanabilir]</span><span class="sxs-lookup"><span data-stu-id="287a2-181">[Queryable]</span></span>

<span data-ttu-id="287a2-182">[Queryable] özniteliği kullanım dışı bırakılmıştır.</span><span class="sxs-lookup"><span data-stu-id="287a2-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="287a2-183">Yeni OData v3 uygulamaları kullanması gereken **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="287a2-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="287a2-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** genişletme yöntemi şimdi ekler bir **EnableQueryAttribute** genel filtre koleksiyonuna.</span><span class="sxs-lookup"><span data-stu-id="287a2-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="287a2-185">Tüm denetleyiciniz varsa **[Queryable]** özniteliği, çağırma `config.EnableQuerySupport()` neden olacak **[Queryable]** başarısız özniteliği</span><span class="sxs-lookup"><span data-stu-id="287a2-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="287a2-186">Bu sorunu çözmek için önerilen yöntem, tüm örneklerini değiştirmektir **QueryableAttribute** ile **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="287a2-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="287a2-187">Aşağıdaki kod, Web API yapılandırmada kullanmak için alternatif bir geçici çözüm şöyledir:</span><span class="sxs-lookup"><span data-stu-id="287a2-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="287a2-188">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="287a2-188">Attribute Routing</span></span>

<span data-ttu-id="287a2-189">Sorun: FromUri özniteliği ile donatılmış karmaşık türünün Model bağlama özniteliği yönlendirme kullanırken farklı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="287a2-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="287a2-190">Aşağıdaki bağlantı sorun izleme ve geçici bir çözüm ayrıntılarını da sahiptir.</span><span class="sxs-lookup"><span data-stu-id="287a2-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="287a2-191">Sorun: Yapı İskelesi MVC/Web API projesine 5.2.0 ile 5.1.2 paketleri sonuçlarında paketler için olanları projede zaten mevcut değil</span><span class="sxs-lookup"><span data-stu-id="287a2-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="287a2-192">ASP.NET MVC 5.2 için NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio araçlarını veya ASP.NET Web uygulaması proje şablonu güncelleştirmez.</span><span class="sxs-lookup"><span data-stu-id="287a2-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="287a2-193">ASP.NET çalışma zamanı paketleri (örneğin 5.1.2 güncelleştirme 2 ')'ın önceki sürümünü kullanırlar.</span><span class="sxs-lookup"><span data-stu-id="287a2-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="287a2-194">Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümünü (örneğin güncelleştirme 2'deki 5.1.2) yükleyin.</span><span class="sxs-lookup"><span data-stu-id="287a2-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="287a2-195">Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz.</span><span class="sxs-lookup"><span data-stu-id="287a2-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="287a2-196">Web API 2.2 veya ASP.NET MVC 5.2 projelerinizi paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="287a2-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="287a2-197">Hata düzeltmeleri ve küçük özellik güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="287a2-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="287a2-198">Bu sürüm çeşitli hata düzeltmeleri ve küçük özellik içerir güncelleştirmeler.</span><span class="sxs-lookup"><span data-stu-id="287a2-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="287a2-199">Tam listesini burada bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="287a2-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="287a2-200">5.2 paketi</span><span class="sxs-lookup"><span data-stu-id="287a2-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="287a2-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="287a2-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="287a2-202">Microsoft.AspNet.OData 5.2.1 paketi NuGet bağımlılık güncelleştirmeler ancak hiçbir hata düzeltmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="287a2-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="287a2-203">Bu güncelleştirme ile 6.4.0 Microsoft.OData.Core üzerinde artık katı bir bağımlılık yoktur, ancak bir 6.4.0 7.0.0 arasındaki herhangi bir sürümüne yükseltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="287a2-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="287a2-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="287a2-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="287a2-205">Bu sürümde biz değiştirmek bağımlılık yapmış olduğunuz `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="287a2-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="287a2-206">Bu sürümündeki yenilikler hakkında daha fazla bilgi için `Json.NET`, bkz: [Json.NET 6.0 sürüm 4 - JSON birleştirme, bağımlılık ekleme](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="287a2-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="287a2-207">Bu sürümdeki diğer yeni özellikleri ve hata düzeltmeleri Web API'si sahip değil.</span><span class="sxs-lookup"><span data-stu-id="287a2-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="287a2-208">Biz, biz Web API'sini yeni bu sürümüne bağlıdır kendine ait diğer tüm bağımlı paketler sonradan güncelleştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="287a2-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="287a2-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="287a2-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="287a2-210">Yayın hakkında bilgi edinebilirsiniz [burada](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="287a2-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="287a2-211">Bu sürüm yalnızca hata düzeltmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="287a2-211">This release contains only bug fixes.</span></span> <span data-ttu-id="287a2-212">Kullanabileceğiniz [bu sorguyu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesini görmek için.</span><span class="sxs-lookup"><span data-stu-id="287a2-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
