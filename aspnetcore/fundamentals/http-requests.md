---
title: ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın
author: stevejgordon
description: ASP.NET Core içindeki mantıksal HttpClient örneklerini yönetmek için ıhttpclientfactory arabirimini kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: f2494a5815396e693f6fd2a45ad78ebffe4d54a3
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358088"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="22126-103">ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın</span><span class="sxs-lookup"><span data-stu-id="22126-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="22126-104">[Glenn CONDRON](https://github.com/glennc), [Ryan şimdi ak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Kirk larkabağı](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="22126-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="22126-105">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="22126-106">`IHttpClientFactory` aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="22126-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="22126-107">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="22126-108">Örneğin, *GitHub* adlı bir Istemci, [GitHub](https://github.com/)'a erişmek için kaydedilebilir ve yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="22126-109">Varsayılan istemci, genel erişim için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="22126-110">`HttpClient`, işleyiciler için temsilci atama yoluyla giden ara yazılım kavramını daha da artırır.</span><span class="sxs-lookup"><span data-stu-id="22126-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="22126-111">`HttpClient`' de işleyiciler temsilci seçme avantajlarından faydalanmak için, Polya tabanlı bir ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="22126-112">Temel alınan `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="22126-113">Otomatik yönetim, `HttpClient` yaşam sürelerini el ile yönetirken oluşan ortak DNS (etki alanı adı sistemi) sorunlarını önler.</span><span class="sxs-lookup"><span data-stu-id="22126-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="22126-114">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="22126-115">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="22126-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="22126-116">Bu konu sürümündeki örnek kod, HTTP yanıtlarında döndürülen JSON içeriğinin serisini kaldırmak için <xref:System.Text.Json> kullanır.</span><span class="sxs-lookup"><span data-stu-id="22126-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="22126-117">`Json.NET` ve `ReadAsAsync<T>`kullanan örnekler için, bu konunun 2. x sürümünü seçmek üzere sürüm seçiciyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="22126-118">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="22126-118">Consumption patterns</span></span>

<span data-ttu-id="22126-119">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="22126-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="22126-120">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="22126-121">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="22126-122">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="22126-123">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="22126-124">En iyi yaklaşım, uygulamanın gereksinimlerine bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="22126-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="22126-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-125">Basic usage</span></span>

<span data-ttu-id="22126-126">`IHttpClientFactory`, `AddHttpClient`çağırarak kaydedilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="22126-127">`IHttpClientFactory`, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanılarak istenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="22126-128">Aşağıdaki kod, bir `HttpClient` örneği oluşturmak için `IHttpClientFactory` kullanır:</span><span class="sxs-lookup"><span data-stu-id="22126-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="22126-129">Yukarıdaki örnekte olduğu gibi `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="22126-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="22126-130">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="22126-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="22126-131">Mevcut bir uygulamada `HttpClient` örneklerinin oluşturulduğu yerlerde, bu oluşumları <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="22126-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-132">Named clients</span></span>

<span data-ttu-id="22126-133">Adlandırılmış istemciler şu durumlarda iyi bir seçimdir:</span><span class="sxs-lookup"><span data-stu-id="22126-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="22126-134">Uygulama birçok farklı `HttpClient`kullanımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="22126-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="22126-135">Birçok `HttpClient`farklı yapılandırmaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22126-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="22126-136">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="22126-137">İstemcinin yapılandırıldığı önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="22126-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="22126-138">Temel adres `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="22126-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="22126-139">GitHub API 'SI ile çalışmak için iki üst bilgi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="22126-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="22126-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="22126-140">CreateClient</span></span>

<span data-ttu-id="22126-141"><xref:System.Net.Http.IHttpClientFactory.CreateClient*> her çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="22126-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="22126-142">Yeni bir `HttpClient` örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22126-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="22126-143">Yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22126-143">The configuration action is called.</span></span>

<span data-ttu-id="22126-144">Adlandırılmış bir istemci oluşturmak için adını `CreateClient`geçirin:</span><span class="sxs-lookup"><span data-stu-id="22126-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="22126-145">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22126-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="22126-146">İstemci için yapılandırılan taban adresi kullanıldığından, kod yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="22126-147">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-147">Typed clients</span></span>

<span data-ttu-id="22126-148">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="22126-148">Typed clients:</span></span>

* <span data-ttu-id="22126-149">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="22126-150">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="22126-151">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="22126-152">Örneğin, tek bir türü belirtilmiş istemci kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="22126-153">Tek bir arka uç uç noktası için.</span><span class="sxs-lookup"><span data-stu-id="22126-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="22126-154">Uç nokta ile ilgili tüm mantığı kapsüllemek için.</span><span class="sxs-lookup"><span data-stu-id="22126-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="22126-155">DI ile birlikte çalışın ve uygulamada gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="22126-156">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="22126-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="22126-157">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="22126-157">In the preceding code:</span></span>

* <span data-ttu-id="22126-158">Yapılandırma, yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="22126-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="22126-159">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="22126-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="22126-160">`HttpClient` işlevselliği ortaya çıkaran API 'ye özgü Yöntemler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="22126-161">Örneğin, `GetAspNetDocsIssues` yöntemi açık sorunları almak için kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="22126-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="22126-162">Aşağıdaki kod, bir tür istemci sınıfını kaydetmek için `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="22126-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="22126-163">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="22126-164">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="22126-165">Türü belirlenmiş bir istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="22126-166">`HttpClient`, türü belirlenmiş bir istemci içinde kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="22126-167">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran bir yöntem tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="22126-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="22126-168">Yukarıdaki kodda `HttpClient` bir özel alanda depolanır.</span><span class="sxs-lookup"><span data-stu-id="22126-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="22126-169">`HttpClient` erişim, genel `GetRepos` yöntemine göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="22126-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="22126-170">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-170">Generated clients</span></span>

<span data-ttu-id="22126-171">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="22126-172">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="22126-173">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="22126-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="22126-174">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22126-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="22126-175">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="22126-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="22126-176">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="22126-177">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="22126-178">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="22126-178">Outgoing request middleware</span></span>

<span data-ttu-id="22126-179">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="22126-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="22126-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="22126-181">Her bir adlandırılmış istemci için uygulanacak işleyiciler tanımlamayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="22126-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="22126-182">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydedilmesini ve zincirleme kullanımını destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="22126-183">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="22126-184">Bu model:</span><span class="sxs-lookup"><span data-stu-id="22126-184">This pattern:</span></span>

  * <span data-ttu-id="22126-185">ASP.NET Core gelen ara yazılım ardışık düzenine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="22126-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="22126-186">, HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar, örneğin:</span><span class="sxs-lookup"><span data-stu-id="22126-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="22126-187">önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="22126-187">caching</span></span>
    * <span data-ttu-id="22126-188">hata işleme</span><span class="sxs-lookup"><span data-stu-id="22126-188">error handling</span></span>
    * <span data-ttu-id="22126-189">serileştirme</span><span class="sxs-lookup"><span data-stu-id="22126-189">serialization</span></span>
    * <span data-ttu-id="22126-190">günlük kaydı</span><span class="sxs-lookup"><span data-stu-id="22126-190">logging</span></span>

<span data-ttu-id="22126-191">Temsilci seçme işleyicisi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="22126-191">To create a delegating handler:</span></span>

* <span data-ttu-id="22126-192"><xref:System.Net.Http.DelegatingHandler>türet.</span><span class="sxs-lookup"><span data-stu-id="22126-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="22126-193"><xref:System.Net.Http.DelegatingHandler.SendAsync*>geçersiz kıl.</span><span class="sxs-lookup"><span data-stu-id="22126-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="22126-194">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütün:</span><span class="sxs-lookup"><span data-stu-id="22126-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="22126-195">Yukarıdaki kod, `X-API-KEY` üst bilgisinin istekte olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="22126-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="22126-196">`X-API-KEY` eksikse, <xref:System.Net.HttpStatusCode.BadRequest> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="22126-197"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>bir `HttpClient` yapılandırmaya birden fazla işleyici eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="22126-198">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="22126-199">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22126-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="22126-200">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="22126-201">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="22126-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="22126-202">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="22126-203">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="22126-204">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="22126-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="22126-205">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="22126-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="22126-206">[HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties)kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="22126-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="22126-207">Geçerli isteğe erişmek için <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="22126-208">Verileri geçirmek için özel bir <xref:System.Threading.AsyncLocal`1> depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="22126-209">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="22126-209">Use Polly-based handlers</span></span>

<span data-ttu-id="22126-210">`IHttpClientFactory`, üçüncü taraf kitaplığı [Polly](https://github.com/App-vNext/Polly)ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="22126-211">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="22126-212">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="22126-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="22126-213">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="22126-214">Polly uzantıları, istemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="22126-215">Polly, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="22126-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="22126-216">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="22126-216">Handle transient faults</span></span>

<span data-ttu-id="22126-217">Hatalar genellikle dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="22126-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, geçici hataları işlemek için bir ilkenin tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="22126-219">`AddTransientHttpErrorPolicy` ile yapılandırılan ilkeler aşağıdaki yanıtları işleyecek şekilde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="22126-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="22126-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="22126-220">HTTP 5xx</span></span>
* <span data-ttu-id="22126-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="22126-221">HTTP 408</span></span>

<span data-ttu-id="22126-222">`AddTransientHttpErrorPolicy`, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="22126-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="22126-223">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="22126-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="22126-224">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="22126-225">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="22126-225">Dynamically select policies</span></span>

<span data-ttu-id="22126-226">Uzantı yöntemleri, örneğin <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>, Polly tabanlı işleyiciler eklemek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="22126-227">Aşağıdaki `AddPolicyHandler` aşırı yüklemesi hangi ilkenin uygulanacağını belirlemek için isteği inceler:</span><span class="sxs-lookup"><span data-stu-id="22126-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="22126-228">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="22126-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="22126-229">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="22126-230">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="22126-231">Polly ilkeleri iç içe almak yaygın bir şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="22126-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="22126-232">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="22126-232">In the preceding example:</span></span>

* <span data-ttu-id="22126-233">İki işleyici eklenir.</span><span class="sxs-lookup"><span data-stu-id="22126-233">Two handlers are added.</span></span>
* <span data-ttu-id="22126-234">İlk işleyici, yeniden deneme ilkesi eklemek için <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> kullanır.</span><span class="sxs-lookup"><span data-stu-id="22126-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="22126-235">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="22126-236">İkinci `AddTransientHttpErrorPolicy` çağrısı bir devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="22126-237">5 başarısız girişim sıralı olarak gerçekleşirse, daha fazla dış istek 30 saniye boyunca engellenir.</span><span class="sxs-lookup"><span data-stu-id="22126-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="22126-238">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="22126-239">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="22126-240">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="22126-241">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="22126-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="22126-242">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="22126-242">In the following code:</span></span>

* <span data-ttu-id="22126-243">"Normal" ve "uzun" ilkeler eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="22126-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="22126-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>, kayıt defterinden "normal" ve "uzun" ilkeleri ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="22126-245">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi için bkz. [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="22126-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="22126-246">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="22126-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="22126-247">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-248">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22126-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="22126-249">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="22126-250">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="22126-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="22126-251">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="22126-252">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="22126-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="22126-253">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="22126-254">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS (etki alanı adı sistemi) değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="22126-255">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="22126-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="22126-256">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="22126-257">`HttpClient` örnekleri genellikle aktiften **çıkarma gerektirmeyen .NET nesneleri olarak** kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="22126-258">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="22126-259">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="22126-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="22126-260">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="22126-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-261">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="22126-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="22126-262">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="22126-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="22126-263">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="22126-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="22126-264">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="22126-265">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="22126-266">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="22126-267">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="22126-268"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22126-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="22126-269">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="22126-270">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="22126-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="22126-271">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="22126-272">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="22126-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="22126-273">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="22126-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="22126-274">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="22126-274">Cookies</span></span>

<span data-ttu-id="22126-275">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="22126-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="22126-276">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="22126-277">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="22126-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="22126-278">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="22126-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="22126-279">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="22126-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="22126-280">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="22126-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="22126-281">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="22126-281">Logging</span></span>

<span data-ttu-id="22126-282">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="22126-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="22126-283">Varsayılan günlük iletilerini görmek için günlük yapılandırmasında uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="22126-284">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="22126-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="22126-285">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="22126-286">Örneğin, *Mynamedclient*adlı bir istemci, "System .net. http. HttpClient" kategorisine sahip iletileri günlüğe kaydeder. **Mynamedclient**. LogicalHandler ".</span><span class="sxs-lookup"><span data-stu-id="22126-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="22126-287">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="22126-288">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="22126-289">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="22126-290">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="22126-291">*Mynamedclient* örneğinde, bu Iletiler "System .net. http. HttpClient" günlük kategorisiyle günlüğe kaydedilir. **Mynamedclient**. ClientHandler ".</span><span class="sxs-lookup"><span data-stu-id="22126-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="22126-292">İstek için bu, tüm diğer işleyiciler çalıştırıldıktan sonra ve istek gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="22126-293">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="22126-294">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="22126-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="22126-295">Bu, istek üst bilgilerinde veya yanıt durum kodunda yapılan değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="22126-296">İstemcinin adını log kategorisinde da içermek, belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="22126-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="22126-297">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22126-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="22126-298">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="22126-299">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="22126-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="22126-301">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="22126-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="22126-302">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="22126-303">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="22126-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="22126-304">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="22126-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="22126-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="22126-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="22126-306">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="22126-306">In the following example:</span></span>

* <span data-ttu-id="22126-307"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="22126-308">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22126-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="22126-309">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="22126-310">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="22126-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="22126-311">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="22126-311">Additional resources</span></span>

* [<span data-ttu-id="22126-312">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="22126-313">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="22126-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="22126-314">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="22126-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="22126-315">.NET 'te JSON serileştirme ve serisini kaldırma</span><span class="sxs-lookup"><span data-stu-id="22126-315">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="22126-316">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="22126-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="22126-317">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-317">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="22126-318">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="22126-318">It offers the following benefits:</span></span>

* <span data-ttu-id="22126-319">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-319">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="22126-320">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-320">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="22126-321">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-321">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="22126-322">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-322">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="22126-323">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-323">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="22126-324">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-324">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="22126-325">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22126-325">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="22126-326">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="22126-326">Consumption patterns</span></span>

<span data-ttu-id="22126-327">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="22126-327">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="22126-328">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-328">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="22126-329">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-329">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="22126-330">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-330">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="22126-331">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-331">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="22126-332">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-332">None of them are strictly superior to another.</span></span> <span data-ttu-id="22126-333">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-333">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="22126-334">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-334">Basic usage</span></span>

<span data-ttu-id="22126-335">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-335">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="22126-336">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-336">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="22126-337">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-337">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="22126-338">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="22126-338">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="22126-339">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="22126-339">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="22126-340">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-340">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="22126-341">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-341">Named clients</span></span>

<span data-ttu-id="22126-342">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-342">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="22126-343">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-343">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="22126-344">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-344">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="22126-345">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-345">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="22126-346">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22126-346">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="22126-347">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-347">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="22126-348">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="22126-348">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="22126-349">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22126-349">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="22126-350">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-350">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="22126-351">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-351">Typed clients</span></span>

<span data-ttu-id="22126-352">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="22126-352">Typed clients:</span></span>

* <span data-ttu-id="22126-353">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-353">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="22126-354">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-354">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="22126-355">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-355">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="22126-356">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-356">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="22126-357">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-357">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="22126-358">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="22126-358">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="22126-359">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="22126-359">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="22126-360">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="22126-360">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="22126-361">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="22126-361">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="22126-362">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="22126-362">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="22126-363">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-363">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="22126-364">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-364">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="22126-365">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-365">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="22126-366">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-366">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="22126-367">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="22126-367">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="22126-368">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-368">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="22126-369">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="22126-369">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="22126-370">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="22126-370">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="22126-371">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-371">Generated clients</span></span>

<span data-ttu-id="22126-372">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-372">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="22126-373">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-373">Refit is a REST library for .NET.</span></span> <span data-ttu-id="22126-374">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="22126-374">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="22126-375">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22126-375">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="22126-376">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="22126-376">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="22126-377">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-377">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="22126-378">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-378">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="22126-379">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="22126-379">Outgoing request middleware</span></span>

<span data-ttu-id="22126-380">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-380">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="22126-381">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="22126-381">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="22126-382">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-382">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="22126-383">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-383">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="22126-384">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="22126-384">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="22126-385">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-385">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="22126-386">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-386">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="22126-387">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="22126-387">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="22126-388">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="22126-388">The preceding code defines a basic handler.</span></span> <span data-ttu-id="22126-389">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="22126-389">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="22126-390">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-390">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="22126-391">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-391">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="22126-392">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22126-392">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="22126-393">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-393">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="22126-394">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22126-394">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="22126-395">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="22126-395">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="22126-396">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="22126-396">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="22126-397">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-397">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="22126-398">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-398">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="22126-399">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="22126-399">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="22126-400">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="22126-400">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="22126-401">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="22126-401">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="22126-402">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-402">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="22126-403">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-403">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="22126-404">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="22126-404">Use Polly-based handlers</span></span>

<span data-ttu-id="22126-405">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22126-405">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="22126-406">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-406">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="22126-407">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="22126-407">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="22126-408">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-408">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="22126-409">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="22126-409">The Polly extensions:</span></span>

* <span data-ttu-id="22126-410">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-410">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="22126-411">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-411">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="22126-412">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-412">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="22126-413">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="22126-413">Handle transient faults</span></span>

<span data-ttu-id="22126-414">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-414">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="22126-415">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-415">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="22126-416">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="22126-416">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="22126-417">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-417">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="22126-418">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="22126-418">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="22126-419">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="22126-419">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="22126-420">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-420">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="22126-421">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="22126-421">Dynamically select policies</span></span>

<span data-ttu-id="22126-422">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-422">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="22126-423">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="22126-423">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="22126-424">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="22126-424">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="22126-425">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="22126-425">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="22126-426">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-426">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="22126-427">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-427">Add multiple Polly handlers</span></span>

<span data-ttu-id="22126-428">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="22126-428">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="22126-429">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="22126-429">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="22126-430">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="22126-430">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="22126-431">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-431">Failed requests are retried up to three times.</span></span> <span data-ttu-id="22126-432">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-432">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="22126-433">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="22126-433">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="22126-434">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-434">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="22126-435">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-435">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="22126-436">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-436">Add policies from the Polly registry</span></span>

<span data-ttu-id="22126-437">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="22126-437">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="22126-438">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="22126-438">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="22126-439">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-439">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="22126-440">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="22126-440">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="22126-441">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-441">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="22126-442">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="22126-442">HttpClient and lifetime management</span></span>

<span data-ttu-id="22126-443">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-443">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-444">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-444">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="22126-445">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-445">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="22126-446">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="22126-446">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="22126-447">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-447">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="22126-448">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="22126-448">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="22126-449">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-449">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="22126-450">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-450">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="22126-451">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="22126-451">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="22126-452">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-452">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="22126-453">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="22126-453">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="22126-454">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-454">Disposal of the client isn't required.</span></span> <span data-ttu-id="22126-455">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-455">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="22126-456">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="22126-456">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="22126-457">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-457">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="22126-458">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="22126-458">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-459">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="22126-459">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="22126-460">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="22126-460">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="22126-461">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="22126-461">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="22126-462">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-462">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="22126-463">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-463">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="22126-464">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-464">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="22126-465">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-465">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="22126-466"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22126-466">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="22126-467">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-467">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="22126-468">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="22126-468">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="22126-469">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-469">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="22126-470">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="22126-470">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="22126-471">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="22126-471">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="22126-472">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="22126-472">Cookies</span></span>

<span data-ttu-id="22126-473">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="22126-473">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="22126-474">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-474">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="22126-475">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="22126-475">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="22126-476">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="22126-476">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="22126-477">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="22126-477">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="22126-478">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="22126-478">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="22126-479">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="22126-479">Logging</span></span>

<span data-ttu-id="22126-480">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="22126-480">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="22126-481">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-481">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="22126-482">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="22126-482">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="22126-483">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-483">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="22126-484">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="22126-484">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="22126-485">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-485">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="22126-486">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-486">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="22126-487">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-487">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="22126-488">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-488">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="22126-489">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-489">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="22126-490">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-490">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="22126-491">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-491">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="22126-492">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="22126-492">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="22126-493">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-493">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="22126-494">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="22126-494">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="22126-495">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22126-495">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="22126-496">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-496">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="22126-497">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-497">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="22126-498"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-498">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="22126-499">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="22126-499">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="22126-500">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-500">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="22126-501">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="22126-501">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="22126-502">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="22126-502">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="22126-503">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="22126-503">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="22126-504">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="22126-504">In the following example:</span></span>

* <span data-ttu-id="22126-505"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-505"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="22126-506">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22126-506">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="22126-507">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-507">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="22126-508">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="22126-508">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="22126-509">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="22126-509">Additional resources</span></span>

* [<span data-ttu-id="22126-510">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-510">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="22126-511">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="22126-511">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="22126-512">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="22126-512">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="22126-513">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="22126-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="22126-514">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-514">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="22126-515">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="22126-515">It offers the following benefits:</span></span>

* <span data-ttu-id="22126-516">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-516">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="22126-517">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-517">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="22126-518">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-518">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="22126-519">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-519">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="22126-520">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-520">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="22126-521">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-521">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="22126-522">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22126-522">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22126-523">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="22126-523">Prerequisites</span></span>

<span data-ttu-id="22126-524">.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="22126-524">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="22126-525">.NET Core ile hedeflenen ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-525">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="22126-526">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="22126-526">Consumption patterns</span></span>

<span data-ttu-id="22126-527">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="22126-527">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="22126-528">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-528">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="22126-529">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-529">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="22126-530">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-530">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="22126-531">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-531">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="22126-532">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-532">None of them are strictly superior to another.</span></span> <span data-ttu-id="22126-533">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-533">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="22126-534">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="22126-534">Basic usage</span></span>

<span data-ttu-id="22126-535">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-535">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="22126-536">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-536">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="22126-537">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-537">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="22126-538">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="22126-538">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="22126-539">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="22126-539">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="22126-540">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-540">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="22126-541">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-541">Named clients</span></span>

<span data-ttu-id="22126-542">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-542">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="22126-543">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-543">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="22126-544">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-544">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="22126-545">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-545">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="22126-546">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="22126-546">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="22126-547">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-547">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="22126-548">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="22126-548">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="22126-549">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="22126-549">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="22126-550">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-550">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="22126-551">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-551">Typed clients</span></span>

<span data-ttu-id="22126-552">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="22126-552">Typed clients:</span></span>

* <span data-ttu-id="22126-553">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-553">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="22126-554">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-554">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="22126-555">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-555">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="22126-556">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-556">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="22126-557">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-557">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="22126-558">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="22126-558">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="22126-559">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="22126-559">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="22126-560">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="22126-560">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="22126-561">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="22126-561">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="22126-562">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="22126-562">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="22126-563">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-563">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="22126-564">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-564">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="22126-565">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-565">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="22126-566">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="22126-566">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="22126-567">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="22126-567">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="22126-568">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-568">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="22126-569">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="22126-569">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="22126-570">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="22126-570">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="22126-571">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="22126-571">Generated clients</span></span>

<span data-ttu-id="22126-572">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-572">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="22126-573">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-573">Refit is a REST library for .NET.</span></span> <span data-ttu-id="22126-574">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="22126-574">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="22126-575">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="22126-575">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="22126-576">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="22126-576">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="22126-577">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="22126-577">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="22126-578">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-578">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="22126-579">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="22126-579">Outgoing request middleware</span></span>

<span data-ttu-id="22126-580">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-580">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="22126-581">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="22126-581">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="22126-582">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-582">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="22126-583">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-583">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="22126-584">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="22126-584">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="22126-585">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-585">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="22126-586">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="22126-586">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="22126-587">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="22126-587">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="22126-588">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="22126-588">The preceding code defines a basic handler.</span></span> <span data-ttu-id="22126-589">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="22126-589">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="22126-590">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-590">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="22126-591">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-591">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="22126-592">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22126-592">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="22126-593">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-593">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="22126-594">İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="22126-594">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="22126-595">İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:</span><span class="sxs-lookup"><span data-stu-id="22126-595">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="22126-596">İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.</span><span class="sxs-lookup"><span data-stu-id="22126-596">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="22126-597">Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="22126-597">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="22126-598">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, işleyici türünü geçirerek çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-598">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="22126-599">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-599">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="22126-600">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="22126-600">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="22126-601">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="22126-601">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="22126-602">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="22126-602">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="22126-603">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-603">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="22126-604">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-604">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="22126-605">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="22126-605">Use Polly-based handlers</span></span>

<span data-ttu-id="22126-606">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="22126-606">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="22126-607">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="22126-607">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="22126-608">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="22126-608">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="22126-609">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-609">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="22126-610">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="22126-610">The Polly extensions:</span></span>

* <span data-ttu-id="22126-611">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="22126-611">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="22126-612">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-612">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="22126-613">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-613">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="22126-614">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="22126-614">Handle transient faults</span></span>

<span data-ttu-id="22126-615">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-615">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="22126-616">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-616">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="22126-617">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="22126-617">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="22126-618">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-618">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="22126-619">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="22126-619">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="22126-620">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="22126-620">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="22126-621">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-621">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="22126-622">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="22126-622">Dynamically select policies</span></span>

<span data-ttu-id="22126-623">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-623">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="22126-624">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="22126-624">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="22126-625">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="22126-625">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="22126-626">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="22126-626">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="22126-627">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-627">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="22126-628">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-628">Add multiple Polly handlers</span></span>

<span data-ttu-id="22126-629">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="22126-629">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="22126-630">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="22126-630">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="22126-631">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="22126-631">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="22126-632">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="22126-632">Failed requests are retried up to three times.</span></span> <span data-ttu-id="22126-633">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="22126-633">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="22126-634">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="22126-634">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="22126-635">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-635">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="22126-636">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-636">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="22126-637">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="22126-637">Add policies from the Polly registry</span></span>

<span data-ttu-id="22126-638">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="22126-638">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="22126-639">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="22126-639">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="22126-640">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-640">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="22126-641">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="22126-641">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="22126-642">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-642">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="22126-643">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="22126-643">HttpClient and lifetime management</span></span>

<span data-ttu-id="22126-644">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-644">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-645">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-645">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="22126-646">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="22126-646">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="22126-647">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="22126-647">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="22126-648">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-648">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="22126-649">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="22126-649">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="22126-650">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-650">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="22126-651">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-651">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="22126-652">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="22126-652">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="22126-653">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-653">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="22126-654">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="22126-654">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="22126-655">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="22126-655">Disposal of the client isn't required.</span></span> <span data-ttu-id="22126-656">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="22126-656">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="22126-657">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="22126-657">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="22126-658">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-658">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="22126-659">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="22126-659">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="22126-660">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="22126-660">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="22126-661">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="22126-661">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="22126-662">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="22126-662">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="22126-663">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-663">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="22126-664">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="22126-664">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="22126-665">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="22126-665">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="22126-666">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="22126-666">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="22126-667"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="22126-667">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="22126-668">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22126-668">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="22126-669">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="22126-669">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="22126-670">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="22126-670">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="22126-671">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="22126-671">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="22126-672">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="22126-672">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="22126-673">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="22126-673">Cookies</span></span>

<span data-ttu-id="22126-674">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="22126-674">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="22126-675">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="22126-675">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="22126-676">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="22126-676">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="22126-677">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="22126-677">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="22126-678">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="22126-678">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="22126-679">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="22126-679">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="22126-680">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="22126-680">Logging</span></span>

<span data-ttu-id="22126-681">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="22126-681">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="22126-682">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="22126-682">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="22126-683">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="22126-683">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="22126-684">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-684">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="22126-685">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="22126-685">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="22126-686">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="22126-686">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="22126-687">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-687">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="22126-688">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-688">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="22126-689">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-689">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="22126-690">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-690">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="22126-691">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22126-691">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="22126-692">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="22126-692">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="22126-693">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="22126-693">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="22126-694">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="22126-694">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="22126-695">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="22126-695">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="22126-696">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="22126-696">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="22126-697">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-697">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="22126-698">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="22126-698">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="22126-699"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="22126-699">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="22126-700">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="22126-700">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="22126-701">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-701">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="22126-702">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="22126-702">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="22126-703">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="22126-703">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="22126-704">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="22126-704">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="22126-705">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="22126-705">In the following example:</span></span>

* <span data-ttu-id="22126-706"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="22126-706"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="22126-707">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22126-707">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="22126-708">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="22126-708">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="22126-709">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="22126-709">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="22126-710">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="22126-710">Additional resources</span></span>

* [<span data-ttu-id="22126-711">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="22126-711">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="22126-712">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="22126-712">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="22126-713">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="22126-713">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
