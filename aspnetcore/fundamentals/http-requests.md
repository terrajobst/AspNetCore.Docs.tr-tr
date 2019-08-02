---
title: ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın
author: stevejgordon
description: ASP.NET Core içindeki mantıksal HttpClient örneklerini yönetmek için ıhttpclientfactory arabirimini kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/01/2019
uid: fundamentals/http-requests
ms.openlocfilehash: bcf2a2eaf6910222d274c38bac343c92fab9cb5b
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739530"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="5e6b9-103">ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın</span><span class="sxs-lookup"><span data-stu-id="5e6b9-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

<span data-ttu-id="5e6b9-104">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5e6b9-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5e6b9-105">Bir <xref:System.Net.Http.IHttpClientFactory> uygulamadaki örnekleri yapılandırmak ve oluşturmak <xref:System.Net.Http.HttpClient> için kayıt yapılabilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5e6b9-106">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-106">It offers the following benefits:</span></span>

* <span data-ttu-id="5e6b9-107">, Mantıksal `HttpClient` örnekleri adlandırmak ve yapılandırmak için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5e6b9-108">Örneğin, GitHub istemcisi kayıtlı ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-108">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5e6b9-109">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5e6b9-110">' De `HttpClient` işleyiciler için temsilci atama ile giden ara yazılım kavramı ve bundan faydalanmak için, Polly tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5e6b9-111">Yaşam sürelerini el ile yönetirken `HttpClientMessageHandler` `HttpClient` gerçekleşen yaygın DNS sorunlarından kaçınmak için temeldeki örneklerin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5e6b9-112">Fabrika tarafından oluşturulan istemciler aracılığıyla gönderilen tüm `ILogger`istekler için yapılandırılabilir bir günlüğe kaydetme deneyimi ekler (aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="5e6b9-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5e6b9-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e6b9-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="5e6b9-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5e6b9-114">Prerequisites</span></span>

<span data-ttu-id="5e6b9-115">.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="5e6b9-116">.NET Core ile hedeflenen ve [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

::: moniker-end

## <a name="consumption-patterns"></a><span data-ttu-id="5e6b9-117">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="5e6b9-117">Consumption patterns</span></span>

<span data-ttu-id="5e6b9-118">Bir uygulamada çeşitli yollar `IHttpClientFactory` kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5e6b9-119">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="5e6b9-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5e6b9-120">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5e6b9-121">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5e6b9-122">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5e6b9-123">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="5e6b9-124">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5e6b9-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="5e6b9-125">Basic usage</span></span>

<span data-ttu-id="5e6b9-126">, Yöntemi içindeki `AddHttpClient` içindeki genişletme yöntemi `IServiceCollection`çağırarak kaydedilebilir. `Startup.ConfigureServices` `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="5e6b9-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5e6b9-127">Kaydedildikten sonra kod, `IHttpClientFactory` [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden bir hizmeti kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5e6b9-128">`IHttpClientFactory` Bir`HttpClient` örnek oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5e6b9-129">Bu `IHttpClientFactory` biçimde kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-129">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="5e6b9-130">Kullanım şekli `HttpClient` üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5e6b9-131">`HttpClient` Örneklerin Şu anda oluşturulduğu yerlerde, bu tekrarlamaları ' a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5e6b9-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-132">Named clients</span></span>

<span data-ttu-id="5e6b9-133">Bir uygulama `HttpClient`, her biri farklı bir yapılandırmaya sahip birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5e6b9-134">Adlandırılmış `HttpClient` için yapılandırma, içinde `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5e6b9-135">Yukarıdaki kodda, `AddHttpClient` *GitHub*adının sağlanması denir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5e6b9-136">Bu istemci, GitHub API 'siyle birlikte&mdash;çalışmak için gerekli olan temel adres ve iki üst bilgi olan bazı varsayılan yapılandırma uygulanmış.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5e6b9-137">Her seferinde `CreateClient` her çağrıldığında yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5e6b9-138">Adlandırılmış bir istemciyi kullanmak için, bir dize parametresi iletilebilir `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5e6b9-139">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5e6b9-140">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5e6b9-141">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5e6b9-142">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-142">Typed clients</span></span>

<span data-ttu-id="5e6b9-143">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-143">Typed clients:</span></span>

* <span data-ttu-id="5e6b9-144">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-144">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5e6b9-145">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-145">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5e6b9-146">Yapılandırmak ve belirli `HttpClient`bir ile etkileşimde bulunmak için tek bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-146">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5e6b9-147">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-147">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="5e6b9-148">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-148">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5e6b9-149">Türü belirtilmiş istemci, oluşturucusunda `HttpClient` bir parametreyi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-149">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5e6b9-150">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-150">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5e6b9-151">`HttpClient` Nesne bir ortak özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-151">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5e6b9-152">İşlevselliği kullanıma `HttpClient` sunan API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-152">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5e6b9-153">Yöntemi `GetAspNetDocsIssues` , GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu saklar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-153">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5e6b9-154">Türü belirtilmiş bir istemciyi kaydettirmek için, genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> genişletme yöntemi içinde `Startup.ConfigureServices`kullanılabilir istemci sınıfını belirterek kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-154">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5e6b9-155">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-155">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5e6b9-156">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-156">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5e6b9-157">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine kayıt `Startup.ConfigureServices`sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-157">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5e6b9-158">Türü belirtilmiş bir istemci `HttpClient` içinde tamamen kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-158">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5e6b9-159">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneği dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-159">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5e6b9-160">Önceki kodda `HttpClient` , bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-160">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5e6b9-161">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-161">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5e6b9-162">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="5e6b9-162">Generated clients</span></span>

<span data-ttu-id="5e6b9-163">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarıyla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-163">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5e6b9-164">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-164">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5e6b9-165">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-165">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5e6b9-166">Arabirim bir uygulama, dış http çağrıları yapmak için kullanılarak `RestService` `HttpClient` tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-166">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5e6b9-167">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-167">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="5e6b9-168">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-168">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="5e6b9-169">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-169">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5e6b9-170">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="5e6b9-170">Outgoing request middleware</span></span>

<span data-ttu-id="5e6b9-171">`HttpClient`, giden HTTP istekleri için birlikte bağlanabilen işleyicileri temsilci seçme kavramı zaten var.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-171">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5e6b9-172">, `IHttpClientFactory` Her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-172">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5e6b9-173">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-173">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5e6b9-174">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-174">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5e6b9-175">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-175">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5e6b9-176">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-176">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5e6b9-177">Bir işleyici oluşturmak için, öğesinden türeten <xref:System.Net.Http.DelegatingHandler>bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-177">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="5e6b9-178">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için yöntemigeçersizkılın:`SendAsync`</span><span class="sxs-lookup"><span data-stu-id="5e6b9-178">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5e6b9-179">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-179">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5e6b9-180">İsteğe bağlı bir `X-API-KEY` başlık olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-180">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5e6b9-181">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-181">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5e6b9-182">Kayıt sırasında bir veya daha fazla işleyici bir `HttpClient`için yapılandırmasına eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-182">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5e6b9-183">Bu görev, <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>üzerindeki genişletme yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-183">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5e6b9-184">Yukarıdaki kodda `ValidateHeaderHandler` ,, dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-184">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5e6b9-185">Her işleyici için ayrı bir dı kapsamı oluşturur.`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="5e6b9-185">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="5e6b9-186">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-186">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="5e6b9-187">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-187">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="5e6b9-188">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> işleyicinin türü geçirerek, çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-188">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5e6b9-189">Yukarıdaki kodda `ValidateHeaderHandler` ,, dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-189">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5e6b9-190">İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-190">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="5e6b9-191">İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-191">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="5e6b9-192">İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-192">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="5e6b9-193">Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-193">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="5e6b9-194">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> işleyici türünü geçirerek çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-194">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

::: moniker-end

<span data-ttu-id="5e6b9-195">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-195">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5e6b9-196">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-196">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5e6b9-197">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-197">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5e6b9-198">Kullanarak `HttpRequestMessage.Properties`işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-198">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="5e6b9-199">Geçerli `IHttpContextAccessor` isteğe erişmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-199">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="5e6b9-200">Verileri geçirmek için `AsyncLocal` özel bir depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-200">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5e6b9-201">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="5e6b9-201">Use Polly-based handlers</span></span>

<span data-ttu-id="5e6b9-202">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplığı ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-202">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5e6b9-203">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-203">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5e6b9-204">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-204">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5e6b9-205">Uzantı yöntemleri, yapılandırılmış `HttpClient` örneklerle Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-205">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5e6b9-206">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-206">The Polly extensions:</span></span>

* <span data-ttu-id="5e6b9-207">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-207">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="5e6b9-208">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-208">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5e6b9-209">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-209">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5e6b9-210">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="5e6b9-210">Handle transient faults</span></span>

<span data-ttu-id="5e6b9-211">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-211">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5e6b9-212">Geçici hataları işlemek üzere bir `AddTransientHttpErrorPolicy` ilkenin tanımlanmasını sağlayan, çağrılan bir uygun genişletme yöntemi eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-212">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5e6b9-213">Bu uzantı yöntemi tanıtıcısıyla `HttpRequestException`yapılandırılmış ilkeler, http 5xx yanıtları ve http 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-213">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5e6b9-214">`AddTransientHttpErrorPolicy` Uzantı içinde`Startup.ConfigureServices`kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-214">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5e6b9-215">Uzantı, olası bir geçici hatayı `PolicyBuilder` temsil eden hataları işlemek için yapılandırılmış bir nesneye erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-215">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5e6b9-216">Yukarıdaki kodda bir `WaitAndRetryAsync` ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-216">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5e6b9-217">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-217">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5e6b9-218">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="5e6b9-218">Dynamically select policies</span></span>

<span data-ttu-id="5e6b9-219">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-219">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5e6b9-220">Bu tür bir uzantının `AddPolicyHandler`birden çok aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-220">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5e6b9-221">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-221">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5e6b9-222">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-222">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5e6b9-223">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-223">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5e6b9-224">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="5e6b9-224">Add multiple Polly handlers</span></span>

<span data-ttu-id="5e6b9-225">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-225">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5e6b9-226">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-226">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5e6b9-227">İlki, yeniden deneme `AddTransientHttpErrorPolicy` ilkesi eklemek için uzantıyı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-227">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5e6b9-228">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-228">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5e6b9-229">İçin `AddTransientHttpErrorPolicy` ikinci çağrı, bir devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-229">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5e6b9-230">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-230">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5e6b9-231">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-231">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5e6b9-232">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-232">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5e6b9-233">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="5e6b9-233">Add policies from the Polly registry</span></span>

<span data-ttu-id="5e6b9-234">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlayıp bir `PolicyRegistry`ile kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-234">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5e6b9-235">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-235">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5e6b9-236">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`öğesine eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-236">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5e6b9-237">Kayıt defterinden `AddPolicyHandlerFromRegistry` bir ilke kullanmak için yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-237">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5e6b9-238">Ve daha fazla `IHttpClientFactory` tümleştirme hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)' de bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-238">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5e6b9-239">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="5e6b9-239">HttpClient and lifetime management</span></span>

<span data-ttu-id="5e6b9-240">Her çağrıldığında `HttpClient`Yenibir örnek döndürülür. `IHttpClientFactory` `CreateClient`</span><span class="sxs-lookup"><span data-stu-id="5e6b9-240">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5e6b9-241">Adlandırılmış istemci başına <xref:System.Net.Http.HttpMessageHandler> .</span><span class="sxs-lookup"><span data-stu-id="5e6b9-241">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="5e6b9-242">Fabrika, `HttpMessageHandler` örneklerin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-242">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5e6b9-243">`IHttpClientFactory`kaynak tüketimini azaltmak için fabrika tarafından oluşturulan örneklerihavuzlar.`HttpMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="5e6b9-243">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5e6b9-244">Bir `HttpMessageHandler` örnek, süresi dolmamışsa yeni `HttpClient` bir örnek oluştururken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-244">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5e6b9-245">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-245">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5e6b9-246">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-246">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5e6b9-247">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-247">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5e6b9-248">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-248">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5e6b9-249">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-249">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5e6b9-250">Bunu geçersiz kılmak için, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> istemcisini oluştururken `IHttpClientBuilder` döndürülen öğesini çağırın:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-250">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5e6b9-251">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-251">Disposal of the client isn't required.</span></span> <span data-ttu-id="5e6b9-252">Çıkarma giden istekleri iptal eder ve çağırma `HttpClient` <xref:System.IDisposable.Dispose*>sonrasında verilen örneğin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-252">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5e6b9-253">`IHttpClientFactory`örnekler tarafından `HttpClient` kullanılan kaynakları izler ve ortadan kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-253">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5e6b9-254">`HttpClient` Örnekler genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-254">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5e6b9-255">Tek `HttpClient` bir örneğinin uzun süre canlı tutulması, önünde `IHttpClientFactory`kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-255">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5e6b9-256">Bu model, ' a geçtikten sonra `IHttpClientFactory`gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-256">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="5e6b9-257">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="5e6b9-257">Logging</span></span>

<span data-ttu-id="5e6b9-258">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-258">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5e6b9-259">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-259">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5e6b9-260">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-260">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5e6b9-261">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-261">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5e6b9-262">Örneğin, *Mynamedclient*adlı bir istemci, bir kategorisine `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`sahip iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-262">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5e6b9-263">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-263">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5e6b9-264">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-264">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5e6b9-265">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-265">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5e6b9-266">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-266">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5e6b9-267">*Mynamedclient* örneğinde, bu iletiler günlük kategorisine `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-267">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5e6b9-268">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-268">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5e6b9-269">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-269">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5e6b9-270">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-270">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5e6b9-271">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-271">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5e6b9-272">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-272">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5e6b9-273">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5e6b9-273">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5e6b9-274">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmayı denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-274">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5e6b9-275">Adlandırılmış `IHttpClientBuilder` veya yazılan istemciler eklenirken bir döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-275">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5e6b9-276"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> Genişletme yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-276">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5e6b9-277">Temsilci, bu istemci tarafından kullanılan birincili `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-277">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5e6b9-278">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="5e6b9-278">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5e6b9-279">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-279">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5e6b9-280">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="5e6b9-280">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5e6b9-281">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="5e6b9-281">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5e6b9-282">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="5e6b9-282">In the following example:</span></span>

* <span data-ttu-id="5e6b9-283"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-283"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5e6b9-284">`MyService`hizmetinden bir `HttpClient`istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-284">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5e6b9-285">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-285">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5e6b9-286">`Main`Hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="5e6b9-286">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5e6b9-287">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5e6b9-287">Additional resources</span></span>

* [<span data-ttu-id="5e6b9-288">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="5e6b9-288">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5e6b9-289">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="5e6b9-289">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5e6b9-290">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="5e6b9-290">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
