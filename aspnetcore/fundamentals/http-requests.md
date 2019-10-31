---
title: ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın
author: stevejgordon
description: ASP.NET Core içindeki mantıksal HttpClient örneklerini yönetmek için ıhttpclientfactory arabirimini kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: a963833acfa12889c8ae3dac443962682e1cb931
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190587"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="50d2c-103">ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın</span><span class="sxs-lookup"><span data-stu-id="50d2c-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="50d2c-104">[Glenn CONDRON](https://github.com/glennc), [Ryan şimdi ak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Kirk larkabağı](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="50d2c-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="50d2c-105">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="50d2c-106">`IHttpClientFactory` aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="50d2c-107">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-108">Örneğin, *GitHub* adlı bir Istemci, [GitHub](https://github.com/)'a erişmek için kaydedilebilir ve yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="50d2c-109">Varsayılan istemci, genel erişim için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="50d2c-110">`HttpClient`, işleyiciler için temsilci atama yoluyla giden ara yazılım kavramını daha da artırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="50d2c-111">`HttpClient`' de işleyiciler temsilci seçme avantajlarından faydalanmak için, Polya tabanlı bir ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="50d2c-112">Temel alınan `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="50d2c-113">Otomatik yönetim, `HttpClient` yaşam sürelerini el ile yönetirken oluşan ortak DNS (etki alanı adı sistemi) sorunlarını önler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="50d2c-114">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="50d2c-115">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="50d2c-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="50d2c-116">Bu konu sürümündeki örnek kod, HTTP yanıtlarında döndürülen JSON içeriğinin serisini kaldırmak için <xref:System.Text.Json> kullanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="50d2c-117">`Json.NET` ve `ReadAsAsync<T>`kullanan örnekler için, bu konunun 2. x sürümünü seçmek üzere sürüm seçiciyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="50d2c-118">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="50d2c-118">Consumption patterns</span></span>

<span data-ttu-id="50d2c-119">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="50d2c-120">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="50d2c-121">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="50d2c-122">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="50d2c-123">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="50d2c-124">En iyi yaklaşım, uygulamanın gereksinimlerine bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="50d2c-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-125">Basic usage</span></span>

<span data-ttu-id="50d2c-126">`IHttpClientFactory`, `AddHttpClient`çağırarak kaydedilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="50d2c-127">`IHttpClientFactory`, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanılarak istenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50d2c-128">Aşağıdaki kod, bir `HttpClient` örneği oluşturmak için `IHttpClientFactory` kullanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="50d2c-129">Yukarıdaki örnekte olduğu gibi `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="50d2c-130">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="50d2c-131">Mevcut bir uygulamada `HttpClient` örneklerinin oluşturulduğu yerlerde, bu oluşumları <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="50d2c-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-132">Named clients</span></span>

<span data-ttu-id="50d2c-133">Adlandırılmış istemciler şu durumlarda iyi bir seçimdir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="50d2c-134">Uygulama birçok farklı `HttpClient`kullanımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="50d2c-135">Birçok `HttpClient`farklı yapılandırmaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="50d2c-136">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="50d2c-137">İstemcinin yapılandırıldığı önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="50d2c-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="50d2c-138">Temel adres `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="50d2c-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="50d2c-139">GitHub API 'SI ile çalışmak için iki üst bilgi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="50d2c-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="50d2c-140">CreateClient</span></span>

<span data-ttu-id="50d2c-141"><xref:System.Net.Http.IHttpClientFactory.CreateClient*> her çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="50d2c-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="50d2c-142">Yeni bir `HttpClient` örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="50d2c-143">Yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-143">The configuration action is called.</span></span>

<span data-ttu-id="50d2c-144">Adlandırılmış bir istemci oluşturmak için adını `CreateClient`geçirin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="50d2c-145">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="50d2c-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="50d2c-146">İstemci için yapılandırılan taban adresi kullanıldığından, kod yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="50d2c-147">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-147">Typed clients</span></span>

<span data-ttu-id="50d2c-148">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="50d2c-148">Typed clients:</span></span>

* <span data-ttu-id="50d2c-149">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="50d2c-150">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="50d2c-151">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="50d2c-152">Örneğin, tek bir türü belirtilmiş istemci kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="50d2c-153">Tek bir arka uç uç noktası için.</span><span class="sxs-lookup"><span data-stu-id="50d2c-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="50d2c-154">Uç nokta ile ilgili tüm mantığı kapsüllemek için.</span><span class="sxs-lookup"><span data-stu-id="50d2c-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="50d2c-155">DI ile birlikte çalışın ve uygulamada gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="50d2c-156">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="50d2c-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="50d2c-157">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="50d2c-157">In the preceding code:</span></span>

* <span data-ttu-id="50d2c-158">Yapılandırma, yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="50d2c-159">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="50d2c-160">`HttpClient` işlevselliği ortaya çıkaran API 'ye özgü Yöntemler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="50d2c-161">Örneğin, `GetAspNetDocsIssues` yöntemi açık sorunları almak için kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="50d2c-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="50d2c-162">Aşağıdaki kod, bir tür istemci sınıfını kaydetmek için `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="50d2c-163">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="50d2c-164">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="50d2c-165">Türü belirlenmiş bir istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="50d2c-166">`HttpClient`, türü belirlenmiş bir istemci içinde kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="50d2c-167">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran bir yöntem tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="50d2c-168">Yukarıdaki kodda `HttpClient` bir özel alanda depolanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="50d2c-169">`HttpClient` erişim, genel `GetRepos` yöntemine göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="50d2c-170">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-170">Generated clients</span></span>

<span data-ttu-id="50d2c-171">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="50d2c-172">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="50d2c-173">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="50d2c-174">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="50d2c-175">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="50d2c-176">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="50d2c-177">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="50d2c-178">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="50d2c-178">Outgoing request middleware</span></span>

<span data-ttu-id="50d2c-179">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="50d2c-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="50d2c-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="50d2c-181">Her bir adlandırılmış istemci için uygulanacak işleyiciler tanımlamayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="50d2c-182">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydedilmesini ve zincirleme kullanımını destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="50d2c-183">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="50d2c-184">Bu model:</span><span class="sxs-lookup"><span data-stu-id="50d2c-184">This pattern:</span></span>

  * <span data-ttu-id="50d2c-185">ASP.NET Core gelen ara yazılım ardışık düzenine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="50d2c-186">, HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar, örneğin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="50d2c-187">önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="50d2c-187">caching</span></span>
    * <span data-ttu-id="50d2c-188">hata işleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-188">error handling</span></span>
    * <span data-ttu-id="50d2c-189">serileştirme</span><span class="sxs-lookup"><span data-stu-id="50d2c-189">serialization</span></span>
    * <span data-ttu-id="50d2c-190">günlük kaydı</span><span class="sxs-lookup"><span data-stu-id="50d2c-190">logging</span></span>

<span data-ttu-id="50d2c-191">Temsilci seçme işleyicisi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="50d2c-191">To create a delegating handler:</span></span>

* <span data-ttu-id="50d2c-192"><xref:System.Net.Http.DelegatingHandler>türet.</span><span class="sxs-lookup"><span data-stu-id="50d2c-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="50d2c-193"><xref:System.Net.Http.DelegatingHandler.SendAsync*>geçersiz kıl.</span><span class="sxs-lookup"><span data-stu-id="50d2c-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="50d2c-194">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütün:</span><span class="sxs-lookup"><span data-stu-id="50d2c-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="50d2c-195">Yukarıdaki kod, `X-API-KEY` üst bilgisinin istekte olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="50d2c-196">`X-API-KEY` eksikse, <xref:System.Net.HttpStatusCode.BadRequest> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="50d2c-197"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>bir `HttpClient` yapılandırmasına birden fazla işleyici eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="50d2c-198">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="50d2c-199">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="50d2c-200">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="50d2c-201">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="50d2c-202">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="50d2c-203">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="50d2c-204">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="50d2c-205">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="50d2c-206">[HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties)kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="50d2c-207">Geçerli isteğe erişmek için <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> kullanın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="50d2c-208">Verileri geçirmek için özel bir <xref:System.Threading.AsyncLocal`1> depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50d2c-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="50d2c-209">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="50d2c-209">Use Polly-based handlers</span></span>

<span data-ttu-id="50d2c-210">`IHttpClientFactory`, üçüncü taraf kitaplığı [Polly](https://github.com/App-vNext/Polly)ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="50d2c-211">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="50d2c-212">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="50d2c-213">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-214">Polly uzantıları, istemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="50d2c-215">Polly, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="50d2c-216">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="50d2c-216">Handle transient faults</span></span>

<span data-ttu-id="50d2c-217">Hatalar genellikle dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="50d2c-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, geçici hataları işlemek için bir ilkenin tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="50d2c-219">`AddTransientHttpErrorPolicy` ile yapılandırılan ilkeler aşağıdaki yanıtları işleyecek şekilde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="50d2c-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="50d2c-220">HTTP 5xx</span></span>
* <span data-ttu-id="50d2c-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="50d2c-221">HTTP 408</span></span>

<span data-ttu-id="50d2c-222">`AddTransientHttpErrorPolicy`, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="50d2c-223">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="50d2c-224">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="50d2c-225">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="50d2c-225">Dynamically select policies</span></span>

<span data-ttu-id="50d2c-226">Uzantı yöntemleri, örneğin <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>, Polly tabanlı işleyiciler eklemek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="50d2c-227">Aşağıdaki `AddPolicyHandler` aşırı yüklemesi hangi ilkenin uygulanacağını belirlemek için isteği inceler:</span><span class="sxs-lookup"><span data-stu-id="50d2c-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="50d2c-228">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="50d2c-229">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="50d2c-230">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="50d2c-231">Polly ilkeleri iç içe almak yaygın bir şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="50d2c-232">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50d2c-232">In the preceding example:</span></span>

* <span data-ttu-id="50d2c-233">İki işleyici eklenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-233">Two handlers are added.</span></span>
* <span data-ttu-id="50d2c-234">İlk işleyici, yeniden deneme ilkesi eklemek için <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> kullanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="50d2c-235">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="50d2c-236">İkinci `AddTransientHttpErrorPolicy` çağrısı bir devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="50d2c-237">5 başarısız girişim sıralı olarak gerçekleşirse, daha fazla dış istek 30 saniye boyunca engellenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="50d2c-238">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="50d2c-239">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="50d2c-240">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="50d2c-241">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="50d2c-242">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="50d2c-242">In the following code:</span></span>

* <span data-ttu-id="50d2c-243">"Normal" ve "uzun" ilkeler eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="50d2c-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>, kayıt defterinden "normal" ve "uzun" ilkeleri ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="50d2c-245">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi için bkz. [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="50d2c-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="50d2c-246">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="50d2c-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="50d2c-247">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-248">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="50d2c-249">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="50d2c-250">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="50d2c-251">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="50d2c-252">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="50d2c-253">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="50d2c-254">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS (etki alanı adı sistemi) değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="50d2c-255">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="50d2c-256">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="50d2c-257">`HttpClient` örnekleri genellikle aktiften **çıkarma gerektirmeyen .NET nesneleri olarak** kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="50d2c-258">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="50d2c-259">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="50d2c-260">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-261">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="50d2c-262">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="50d2c-262">Logging</span></span>

<span data-ttu-id="50d2c-263">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-263">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="50d2c-264">Varsayılan günlük iletilerini görmek için günlük yapılandırmasında uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-264">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="50d2c-265">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-265">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="50d2c-266">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-266">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="50d2c-267">Örneğin, *Mynamedclient*adlı bir istemci, "System .net. http. HttpClient" kategorisine sahip iletileri günlüğe kaydeder. **Mynamedclient**. LogicalHandler ".</span><span class="sxs-lookup"><span data-stu-id="50d2c-267">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="50d2c-268">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-268">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-269">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-269">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="50d2c-270">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-270">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="50d2c-271">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-271">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-272">*Mynamedclient* örneğinde, bu Iletiler "System .net. http. HttpClient" günlük kategorisiyle günlüğe kaydedilir. **Mynamedclient**. ClientHandler ".</span><span class="sxs-lookup"><span data-stu-id="50d2c-272">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="50d2c-273">İstek için bu, tüm diğer işleyiciler çalıştırıldıktan sonra ve istek gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-273">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="50d2c-274">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-274">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="50d2c-275">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-275">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="50d2c-276">Bu, istek üst bilgilerinde veya yanıt durum kodunda yapılan değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-276">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="50d2c-277">İstemcinin adını log kategorisinde da içermek, belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-277">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="50d2c-278">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="50d2c-278">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="50d2c-279">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-279">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="50d2c-280">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-280">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="50d2c-281"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-281">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="50d2c-282">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-282">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="50d2c-283">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-283">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="50d2c-284">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-284">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="50d2c-285">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="50d2c-285">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="50d2c-286">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="50d2c-286">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="50d2c-287">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50d2c-287">In the following example:</span></span>

* <span data-ttu-id="50d2c-288"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-288"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="50d2c-289">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-289">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="50d2c-290">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-290">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="50d2c-291">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-291">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="50d2c-292">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="50d2c-292">Additional resources</span></span>

* [<span data-ttu-id="50d2c-293">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-293">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="50d2c-294">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="50d2c-294">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="50d2c-295">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="50d2c-295">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="50d2c-296">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="50d2c-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="50d2c-297">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-297">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="50d2c-298">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-298">It offers the following benefits:</span></span>

* <span data-ttu-id="50d2c-299">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-299">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-300">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-300">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="50d2c-301">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-301">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="50d2c-302">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-302">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="50d2c-303">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-303">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="50d2c-304">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-304">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="50d2c-305">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50d2c-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="50d2c-306">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="50d2c-306">Consumption patterns</span></span>

<span data-ttu-id="50d2c-307">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-307">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="50d2c-308">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-308">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="50d2c-309">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-309">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="50d2c-310">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-310">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="50d2c-311">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-311">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="50d2c-312">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-312">None of them are strictly superior to another.</span></span> <span data-ttu-id="50d2c-313">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-313">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="50d2c-314">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-314">Basic usage</span></span>

<span data-ttu-id="50d2c-315">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-315">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="50d2c-316">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-316">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50d2c-317">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-317">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="50d2c-318">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-318">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="50d2c-319">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-319">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="50d2c-320">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-320">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="50d2c-321">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-321">Named clients</span></span>

<span data-ttu-id="50d2c-322">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-322">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="50d2c-323">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-323">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="50d2c-324">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-324">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="50d2c-325">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-325">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="50d2c-326">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-326">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="50d2c-327">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-327">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="50d2c-328">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-328">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="50d2c-329">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="50d2c-329">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="50d2c-330">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-330">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="50d2c-331">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-331">Typed clients</span></span>

<span data-ttu-id="50d2c-332">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="50d2c-332">Typed clients:</span></span>

* <span data-ttu-id="50d2c-333">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-333">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="50d2c-334">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-334">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="50d2c-335">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-335">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="50d2c-336">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-336">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="50d2c-337">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-337">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="50d2c-338">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="50d2c-338">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="50d2c-339">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-339">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="50d2c-340">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-340">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="50d2c-341">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-341">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="50d2c-342">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="50d2c-342">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="50d2c-343">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-343">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="50d2c-344">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-344">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="50d2c-345">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-345">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="50d2c-346">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-346">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="50d2c-347">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-347">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="50d2c-348">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-348">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="50d2c-349">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-349">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="50d2c-350">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-350">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="50d2c-351">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-351">Generated clients</span></span>

<span data-ttu-id="50d2c-352">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-352">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="50d2c-353">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-353">Refit is a REST library for .NET.</span></span> <span data-ttu-id="50d2c-354">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-354">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="50d2c-355">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-355">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="50d2c-356">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-356">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="50d2c-357">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-357">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="50d2c-358">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-358">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="50d2c-359">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="50d2c-359">Outgoing request middleware</span></span>

<span data-ttu-id="50d2c-360">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-360">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="50d2c-361">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-361">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="50d2c-362">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-362">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="50d2c-363">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-363">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="50d2c-364">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-364">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="50d2c-365">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-365">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="50d2c-366">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-366">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="50d2c-367">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-367">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="50d2c-368">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-368">The preceding code defines a basic handler.</span></span> <span data-ttu-id="50d2c-369">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-369">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="50d2c-370">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-370">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="50d2c-371">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-371">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="50d2c-372">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-372">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="50d2c-373">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-373">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="50d2c-374">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-374">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="50d2c-375">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-375">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="50d2c-376">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-376">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="50d2c-377">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-377">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="50d2c-378">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-378">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="50d2c-379">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-379">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="50d2c-380">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-380">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="50d2c-381">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-381">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="50d2c-382">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-382">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="50d2c-383">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50d2c-383">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="50d2c-384">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="50d2c-384">Use Polly-based handlers</span></span>

<span data-ttu-id="50d2c-385">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-385">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="50d2c-386">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-386">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="50d2c-387">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-387">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="50d2c-388">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-388">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-389">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="50d2c-389">The Polly extensions:</span></span>

* <span data-ttu-id="50d2c-390">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-390">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="50d2c-391">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-391">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="50d2c-392">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-392">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="50d2c-393">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="50d2c-393">Handle transient faults</span></span>

<span data-ttu-id="50d2c-394">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-394">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="50d2c-395">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-395">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="50d2c-396">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="50d2c-396">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="50d2c-397">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-397">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="50d2c-398">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-398">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="50d2c-399">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-399">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="50d2c-400">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-400">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="50d2c-401">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="50d2c-401">Dynamically select policies</span></span>

<span data-ttu-id="50d2c-402">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-402">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="50d2c-403">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="50d2c-403">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="50d2c-404">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-404">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="50d2c-405">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-405">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="50d2c-406">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-406">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="50d2c-407">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-407">Add multiple Polly handlers</span></span>

<span data-ttu-id="50d2c-408">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-408">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="50d2c-409">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-409">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="50d2c-410">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-410">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="50d2c-411">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-411">Failed requests are retried up to three times.</span></span> <span data-ttu-id="50d2c-412">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-412">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="50d2c-413">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-413">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="50d2c-414">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-414">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="50d2c-415">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-415">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="50d2c-416">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-416">Add policies from the Polly registry</span></span>

<span data-ttu-id="50d2c-417">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-417">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="50d2c-418">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-418">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="50d2c-419">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-419">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="50d2c-420">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-420">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="50d2c-421">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-421">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="50d2c-422">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="50d2c-422">HttpClient and lifetime management</span></span>

<span data-ttu-id="50d2c-423">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-423">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-424">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-424">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="50d2c-425">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-425">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="50d2c-426">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-426">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="50d2c-427">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-427">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="50d2c-428">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-428">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="50d2c-429">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-429">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="50d2c-430">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-430">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="50d2c-431">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-431">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="50d2c-432">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-432">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="50d2c-433">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-433">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="50d2c-434">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-434">Disposal of the client isn't required.</span></span> <span data-ttu-id="50d2c-435">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-435">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="50d2c-436">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-436">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-437">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-437">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="50d2c-438">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-438">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-439">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-439">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="50d2c-440">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="50d2c-440">Logging</span></span>

<span data-ttu-id="50d2c-441">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-441">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="50d2c-442">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-442">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="50d2c-443">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-443">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="50d2c-444">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-444">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="50d2c-445">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50d2c-445">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="50d2c-446">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-446">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-447">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-447">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="50d2c-448">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-448">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="50d2c-449">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-449">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-450">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-450">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="50d2c-451">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-451">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="50d2c-452">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-452">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="50d2c-453">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-453">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="50d2c-454">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-454">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="50d2c-455">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-455">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="50d2c-456">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="50d2c-456">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="50d2c-457">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-457">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="50d2c-458">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-458">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="50d2c-459"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-459">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="50d2c-460">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-460">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="50d2c-461">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-461">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="50d2c-462">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-462">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="50d2c-463">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="50d2c-463">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="50d2c-464">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="50d2c-464">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="50d2c-465">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50d2c-465">In the following example:</span></span>

* <span data-ttu-id="50d2c-466"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-466"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="50d2c-467">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-467">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="50d2c-468">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-468">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="50d2c-469">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-469">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="50d2c-470">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="50d2c-470">Additional resources</span></span>

* [<span data-ttu-id="50d2c-471">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-471">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="50d2c-472">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="50d2c-472">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="50d2c-473">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="50d2c-473">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="50d2c-474">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="50d2c-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="50d2c-475">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-475">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="50d2c-476">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-476">It offers the following benefits:</span></span>

* <span data-ttu-id="50d2c-477">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-477">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-478">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-478">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="50d2c-479">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-479">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="50d2c-480">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-480">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="50d2c-481">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-481">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="50d2c-482">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-482">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="50d2c-483">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50d2c-483">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50d2c-484">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="50d2c-484">Prerequisites</span></span>

<span data-ttu-id="50d2c-485">.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-485">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="50d2c-486">.NET Core ile hedeflenen ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-486">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="50d2c-487">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="50d2c-487">Consumption patterns</span></span>

<span data-ttu-id="50d2c-488">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-488">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="50d2c-489">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-489">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="50d2c-490">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-490">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="50d2c-491">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-491">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="50d2c-492">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-492">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="50d2c-493">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-493">None of them are strictly superior to another.</span></span> <span data-ttu-id="50d2c-494">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-494">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="50d2c-495">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="50d2c-495">Basic usage</span></span>

<span data-ttu-id="50d2c-496">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-496">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="50d2c-497">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-497">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50d2c-498">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-498">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="50d2c-499">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-499">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="50d2c-500">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-500">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="50d2c-501">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-501">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="50d2c-502">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-502">Named clients</span></span>

<span data-ttu-id="50d2c-503">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-503">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="50d2c-504">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-504">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="50d2c-505">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-505">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="50d2c-506">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-506">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="50d2c-507">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-507">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="50d2c-508">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-508">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="50d2c-509">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-509">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="50d2c-510">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="50d2c-510">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="50d2c-511">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-511">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="50d2c-512">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-512">Typed clients</span></span>

<span data-ttu-id="50d2c-513">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="50d2c-513">Typed clients:</span></span>

* <span data-ttu-id="50d2c-514">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-514">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="50d2c-515">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-515">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="50d2c-516">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-516">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="50d2c-517">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-517">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="50d2c-518">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-518">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="50d2c-519">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="50d2c-519">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="50d2c-520">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-520">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="50d2c-521">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-521">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="50d2c-522">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-522">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="50d2c-523">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="50d2c-523">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="50d2c-524">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-524">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="50d2c-525">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-525">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="50d2c-526">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-526">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="50d2c-527">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-527">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="50d2c-528">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-528">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="50d2c-529">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-529">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="50d2c-530">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-530">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="50d2c-531">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-531">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="50d2c-532">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="50d2c-532">Generated clients</span></span>

<span data-ttu-id="50d2c-533">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-533">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="50d2c-534">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-534">Refit is a REST library for .NET.</span></span> <span data-ttu-id="50d2c-535">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-535">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="50d2c-536">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-536">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="50d2c-537">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-537">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="50d2c-538">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-538">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="50d2c-539">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-539">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="50d2c-540">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="50d2c-540">Outgoing request middleware</span></span>

<span data-ttu-id="50d2c-541">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-541">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="50d2c-542">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-542">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="50d2c-543">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-543">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="50d2c-544">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-544">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="50d2c-545">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-545">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="50d2c-546">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-546">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="50d2c-547">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-547">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="50d2c-548">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-548">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="50d2c-549">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-549">The preceding code defines a basic handler.</span></span> <span data-ttu-id="50d2c-550">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-550">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="50d2c-551">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-551">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="50d2c-552">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-552">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="50d2c-553">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-553">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="50d2c-554">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-554">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="50d2c-555">İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-555">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="50d2c-556">İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:</span><span class="sxs-lookup"><span data-stu-id="50d2c-556">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="50d2c-557">İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.</span><span class="sxs-lookup"><span data-stu-id="50d2c-557">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="50d2c-558">Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-558">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="50d2c-559">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, işleyici türünü geçirerek çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-559">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="50d2c-560">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-560">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="50d2c-561">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-561">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="50d2c-562">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-562">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="50d2c-563">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-563">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="50d2c-564">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="50d2c-564">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="50d2c-565">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50d2c-565">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="50d2c-566">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="50d2c-566">Use Polly-based handlers</span></span>

<span data-ttu-id="50d2c-567">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-567">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="50d2c-568">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-568">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="50d2c-569">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-569">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="50d2c-570">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-570">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-571">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="50d2c-571">The Polly extensions:</span></span>

* <span data-ttu-id="50d2c-572">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-572">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="50d2c-573">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-573">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="50d2c-574">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-574">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="50d2c-575">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="50d2c-575">Handle transient faults</span></span>

<span data-ttu-id="50d2c-576">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-576">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="50d2c-577">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-577">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="50d2c-578">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="50d2c-578">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="50d2c-579">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-579">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="50d2c-580">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-580">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="50d2c-581">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-581">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="50d2c-582">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-582">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="50d2c-583">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="50d2c-583">Dynamically select policies</span></span>

<span data-ttu-id="50d2c-584">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-584">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="50d2c-585">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="50d2c-585">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="50d2c-586">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="50d2c-586">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="50d2c-587">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-587">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="50d2c-588">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-588">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="50d2c-589">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-589">Add multiple Polly handlers</span></span>

<span data-ttu-id="50d2c-590">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="50d2c-590">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="50d2c-591">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-591">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="50d2c-592">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-592">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="50d2c-593">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-593">Failed requests are retried up to three times.</span></span> <span data-ttu-id="50d2c-594">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-594">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="50d2c-595">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-595">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="50d2c-596">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-596">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="50d2c-597">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-597">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="50d2c-598">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="50d2c-598">Add policies from the Polly registry</span></span>

<span data-ttu-id="50d2c-599">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-599">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="50d2c-600">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-600">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="50d2c-601">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-601">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="50d2c-602">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="50d2c-602">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="50d2c-603">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-603">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="50d2c-604">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="50d2c-604">HttpClient and lifetime management</span></span>

<span data-ttu-id="50d2c-605">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-605">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-606">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-606">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="50d2c-607">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-607">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="50d2c-608">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-608">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="50d2c-609">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-609">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="50d2c-610">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-610">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="50d2c-611">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-611">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="50d2c-612">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-612">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="50d2c-613">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-613">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="50d2c-614">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-614">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="50d2c-615">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="50d2c-615">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="50d2c-616">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-616">Disposal of the client isn't required.</span></span> <span data-ttu-id="50d2c-617">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-617">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="50d2c-618">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-618">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="50d2c-619">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-619">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="50d2c-620">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-620">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="50d2c-621">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-621">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="50d2c-622">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="50d2c-622">Logging</span></span>

<span data-ttu-id="50d2c-623">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="50d2c-623">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="50d2c-624">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="50d2c-624">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="50d2c-625">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-625">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="50d2c-626">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-626">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="50d2c-627">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50d2c-627">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="50d2c-628">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-628">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-629">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-629">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="50d2c-630">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-630">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="50d2c-631">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-631">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="50d2c-632">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-632">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="50d2c-633">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-633">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="50d2c-634">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-634">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="50d2c-635">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-635">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="50d2c-636">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-636">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="50d2c-637">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-637">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="50d2c-638">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="50d2c-638">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="50d2c-639">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-639">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="50d2c-640">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="50d2c-640">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="50d2c-641"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-641">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="50d2c-642">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="50d2c-642">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="50d2c-643">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-643">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="50d2c-644">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="50d2c-644">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="50d2c-645">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="50d2c-645">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="50d2c-646">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="50d2c-646">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="50d2c-647">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50d2c-647">In the following example:</span></span>

* <span data-ttu-id="50d2c-648"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50d2c-648"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="50d2c-649">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50d2c-649">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="50d2c-650">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50d2c-650">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="50d2c-651">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="50d2c-651">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="50d2c-652">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="50d2c-652">Additional resources</span></span>

* [<span data-ttu-id="50d2c-653">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="50d2c-653">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="50d2c-654">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="50d2c-654">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="50d2c-655">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="50d2c-655">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
