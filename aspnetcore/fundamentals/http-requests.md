---
title: ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın
author: stevejgordon
description: ASP.NET Core içindeki mantıksal HttpClient örneklerini yönetmek için ıhttpclientfactory arabirimini kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 7a5b5c84775ea2482034ef9f3e8a2376036e66cb
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478733"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="98838-103">ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın</span><span class="sxs-lookup"><span data-stu-id="98838-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98838-104">[Glenn CONDRON](https://github.com/glennc), [Ryan şimdi ak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Kirk larkabağı](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="98838-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="98838-105">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="98838-106">`IHttpClientFactory` aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="98838-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="98838-107">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="98838-108">Örneğin, *GitHub* adlı bir Istemci, [GitHub](https://github.com/)'a erişmek için kaydedilebilir ve yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="98838-109">Varsayılan istemci, genel erişim için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="98838-110">`HttpClient`, işleyiciler için temsilci atama yoluyla giden ara yazılım kavramını daha da artırır.</span><span class="sxs-lookup"><span data-stu-id="98838-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="98838-111">`HttpClient`' de işleyiciler temsilci seçme avantajlarından faydalanmak için, Polya tabanlı bir ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="98838-112">Temel alınan `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="98838-113">Otomatik yönetim, `HttpClient` yaşam sürelerini el ile yönetirken oluşan ortak DNS (etki alanı adı sistemi) sorunlarını önler.</span><span class="sxs-lookup"><span data-stu-id="98838-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="98838-114">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="98838-115">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="98838-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="98838-116">Bu konu sürümündeki örnek kod, HTTP yanıtlarında döndürülen JSON içeriğinin serisini kaldırmak için <xref:System.Text.Json> kullanır.</span><span class="sxs-lookup"><span data-stu-id="98838-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="98838-117">`Json.NET` ve `ReadAsAsync<T>`kullanan örnekler için, bu konunun 2. x sürümünü seçmek üzere sürüm seçiciyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="98838-118">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="98838-118">Consumption patterns</span></span>

<span data-ttu-id="98838-119">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="98838-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="98838-120">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="98838-121">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="98838-122">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="98838-123">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="98838-124">En iyi yaklaşım, uygulamanın gereksinimlerine bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="98838-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="98838-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-125">Basic usage</span></span>

<span data-ttu-id="98838-126">`IHttpClientFactory`, `AddHttpClient`çağırarak kaydedilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="98838-127">`IHttpClientFactory`, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanılarak istenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="98838-128">Aşağıdaki kod, bir `HttpClient` örneği oluşturmak için `IHttpClientFactory` kullanır:</span><span class="sxs-lookup"><span data-stu-id="98838-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="98838-129">Yukarıdaki örnekte olduğu gibi `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="98838-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="98838-130">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="98838-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="98838-131">Mevcut bir uygulamada `HttpClient` örneklerinin oluşturulduğu yerlerde, bu oluşumları <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="98838-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-132">Named clients</span></span>

<span data-ttu-id="98838-133">Adlandırılmış istemciler şu durumlarda iyi bir seçimdir:</span><span class="sxs-lookup"><span data-stu-id="98838-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="98838-134">Uygulama birçok farklı `HttpClient`kullanımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98838-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="98838-135">Birçok `HttpClient`farklı yapılandırmaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="98838-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="98838-136">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="98838-137">İstemcinin yapılandırıldığı önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="98838-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="98838-138">Temel adres `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="98838-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="98838-139">GitHub API 'SI ile çalışmak için iki üst bilgi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="98838-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="98838-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="98838-140">CreateClient</span></span>

<span data-ttu-id="98838-141"><xref:System.Net.Http.IHttpClientFactory.CreateClient*> her çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="98838-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="98838-142">Yeni bir `HttpClient` örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98838-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="98838-143">Yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="98838-143">The configuration action is called.</span></span>

<span data-ttu-id="98838-144">Adlandırılmış bir istemci oluşturmak için adını `CreateClient`geçirin:</span><span class="sxs-lookup"><span data-stu-id="98838-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="98838-145">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="98838-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="98838-146">İstemci için yapılandırılan taban adresi kullanıldığından, kod yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="98838-147">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-147">Typed clients</span></span>

<span data-ttu-id="98838-148">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="98838-148">Typed clients:</span></span>

* <span data-ttu-id="98838-149">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="98838-150">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="98838-151">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="98838-152">Örneğin, tek bir türü belirtilmiş istemci kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="98838-153">Tek bir arka uç uç noktası için.</span><span class="sxs-lookup"><span data-stu-id="98838-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="98838-154">Uç nokta ile ilgili tüm mantığı kapsüllemek için.</span><span class="sxs-lookup"><span data-stu-id="98838-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="98838-155">DI ile birlikte çalışın ve uygulamada gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="98838-156">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="98838-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="98838-157">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="98838-157">In the preceding code:</span></span>

* <span data-ttu-id="98838-158">Yapılandırma, yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="98838-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="98838-159">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="98838-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="98838-160">`HttpClient` işlevselliği ortaya çıkaran API 'ye özgü Yöntemler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="98838-161">Örneğin, `GetAspNetDocsIssues` yöntemi açık sorunları almak için kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="98838-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="98838-162">Aşağıdaki kod, bir tür istemci sınıfını kaydetmek için `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="98838-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="98838-163">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="98838-164">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="98838-165">Türü belirlenmiş bir istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="98838-166">`HttpClient`, türü belirlenmiş bir istemci içinde kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="98838-167">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran bir yöntem tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="98838-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="98838-168">Yukarıdaki kodda `HttpClient` bir özel alanda depolanır.</span><span class="sxs-lookup"><span data-stu-id="98838-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="98838-169">`HttpClient` erişim, genel `GetRepos` yöntemine göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="98838-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="98838-170">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-170">Generated clients</span></span>

<span data-ttu-id="98838-171">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="98838-172">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="98838-173">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="98838-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="98838-174">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98838-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="98838-175">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="98838-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="98838-176">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="98838-177">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="98838-178">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="98838-178">Outgoing request middleware</span></span>

<span data-ttu-id="98838-179">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="98838-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="98838-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="98838-181">Her bir adlandırılmış istemci için uygulanacak işleyiciler tanımlamayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="98838-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="98838-182">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydedilmesini ve zincirleme kullanımını destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="98838-183">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="98838-184">Bu model:</span><span class="sxs-lookup"><span data-stu-id="98838-184">This pattern:</span></span>

  * <span data-ttu-id="98838-185">ASP.NET Core gelen ara yazılım ardışık düzenine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="98838-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="98838-186">, HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar, örneğin:</span><span class="sxs-lookup"><span data-stu-id="98838-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="98838-187">önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="98838-187">caching</span></span>
    * <span data-ttu-id="98838-188">hata işleme</span><span class="sxs-lookup"><span data-stu-id="98838-188">error handling</span></span>
    * <span data-ttu-id="98838-189">serileştirme</span><span class="sxs-lookup"><span data-stu-id="98838-189">serialization</span></span>
    * <span data-ttu-id="98838-190">günlük kaydı</span><span class="sxs-lookup"><span data-stu-id="98838-190">logging</span></span>

<span data-ttu-id="98838-191">Temsilci seçme işleyicisi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="98838-191">To create a delegating handler:</span></span>

* <span data-ttu-id="98838-192"><xref:System.Net.Http.DelegatingHandler>türet.</span><span class="sxs-lookup"><span data-stu-id="98838-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="98838-193"><xref:System.Net.Http.DelegatingHandler.SendAsync*>geçersiz kıl.</span><span class="sxs-lookup"><span data-stu-id="98838-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="98838-194">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütün:</span><span class="sxs-lookup"><span data-stu-id="98838-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="98838-195">Yukarıdaki kod, `X-API-KEY` üst bilgisinin istekte olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="98838-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="98838-196">`X-API-KEY` eksikse, <xref:System.Net.HttpStatusCode.BadRequest> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="98838-197"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>bir `HttpClient` yapılandırmasına birden fazla işleyici eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="98838-198">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="98838-199">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98838-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="98838-200">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="98838-201">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="98838-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="98838-202">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="98838-203">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="98838-204">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="98838-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="98838-205">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="98838-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="98838-206">[HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties)kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="98838-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="98838-207">Geçerli isteğe erişmek için <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="98838-208">Verileri geçirmek için özel bir <xref:System.Threading.AsyncLocal`1> depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="98838-209">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="98838-209">Use Polly-based handlers</span></span>

<span data-ttu-id="98838-210">`IHttpClientFactory`, üçüncü taraf kitaplığı [Polly](https://github.com/App-vNext/Polly)ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="98838-211">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="98838-212">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="98838-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="98838-213">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="98838-214">Polly uzantıları, istemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="98838-215">Polly, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98838-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="98838-216">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="98838-216">Handle transient faults</span></span>

<span data-ttu-id="98838-217">Hatalar genellikle dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="98838-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, geçici hataları işlemek için bir ilkenin tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="98838-219">`AddTransientHttpErrorPolicy` ile yapılandırılan ilkeler aşağıdaki yanıtları işleyecek şekilde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="98838-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="98838-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="98838-220">HTTP 5xx</span></span>
* <span data-ttu-id="98838-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="98838-221">HTTP 408</span></span>

<span data-ttu-id="98838-222">`AddTransientHttpErrorPolicy`, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="98838-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="98838-223">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98838-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="98838-224">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="98838-225">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="98838-225">Dynamically select policies</span></span>

<span data-ttu-id="98838-226">Uzantı yöntemleri, örneğin <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>, Polly tabanlı işleyiciler eklemek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="98838-227">Aşağıdaki `AddPolicyHandler` aşırı yüklemesi hangi ilkenin uygulanacağını belirlemek için isteği inceler:</span><span class="sxs-lookup"><span data-stu-id="98838-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="98838-228">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="98838-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="98838-229">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="98838-230">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="98838-231">Polly ilkeleri iç içe almak yaygın bir şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="98838-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="98838-232">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="98838-232">In the preceding example:</span></span>

* <span data-ttu-id="98838-233">İki işleyici eklenir.</span><span class="sxs-lookup"><span data-stu-id="98838-233">Two handlers are added.</span></span>
* <span data-ttu-id="98838-234">İlk işleyici, yeniden deneme ilkesi eklemek için <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> kullanır.</span><span class="sxs-lookup"><span data-stu-id="98838-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="98838-235">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="98838-236">İkinci `AddTransientHttpErrorPolicy` çağrısı bir devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="98838-237">5 başarısız girişim sıralı olarak gerçekleşirse, daha fazla dış istek 30 saniye boyunca engellenir.</span><span class="sxs-lookup"><span data-stu-id="98838-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="98838-238">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="98838-239">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="98838-240">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="98838-241">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="98838-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="98838-242">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="98838-242">In the following code:</span></span>

* <span data-ttu-id="98838-243">"Normal" ve "uzun" ilkeler eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="98838-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="98838-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>, kayıt defterinden "normal" ve "uzun" ilkeleri ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="98838-245">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi için bkz. [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="98838-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="98838-246">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="98838-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="98838-247">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-248">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98838-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="98838-249">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="98838-250">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="98838-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="98838-251">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="98838-252">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="98838-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="98838-253">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="98838-254">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS (etki alanı adı sistemi) değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="98838-255">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="98838-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="98838-256">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="98838-257">`HttpClient` örnekleri genellikle aktiften **çıkarma gerektirmeyen .NET nesneleri olarak** kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="98838-258">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="98838-259">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="98838-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="98838-260">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="98838-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-261">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98838-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="98838-262">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="98838-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="98838-263">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="98838-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="98838-264">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="98838-265">Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="98838-266">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="98838-267">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="98838-268"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="98838-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="98838-269">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="98838-270">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="98838-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="98838-271">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="98838-272">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="98838-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="98838-273">`SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="98838-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="98838-274">Özgü</span><span class="sxs-lookup"><span data-stu-id="98838-274">Cookies</span></span>

<span data-ttu-id="98838-275">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98838-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="98838-276">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="98838-277">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="98838-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="98838-278">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="98838-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="98838-279">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="98838-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="98838-280">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="98838-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="98838-281">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="98838-281">Logging</span></span>

<span data-ttu-id="98838-282">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="98838-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="98838-283">Varsayılan günlük iletilerini görmek için günlük yapılandırmasında uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="98838-284">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="98838-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="98838-285">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="98838-286">Örneğin, *Mynamedclient*adlı bir istemci, "System .net. http. HttpClient" kategorisine sahip iletileri günlüğe kaydeder. **Mynamedclient**. LogicalHandler ".</span><span class="sxs-lookup"><span data-stu-id="98838-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="98838-287">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="98838-288">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="98838-289">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="98838-290">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="98838-291">*Mynamedclient* örneğinde, bu Iletiler "System .net. http. HttpClient" günlük kategorisiyle günlüğe kaydedilir. **Mynamedclient**. ClientHandler ".</span><span class="sxs-lookup"><span data-stu-id="98838-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="98838-292">İstek için bu, tüm diğer işleyiciler çalıştırıldıktan sonra ve istek gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="98838-293">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="98838-294">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="98838-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="98838-295">Bu, istek üst bilgilerinde veya yanıt durum kodunda yapılan değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="98838-296">İstemcinin adını log kategorisinde da içermek, belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="98838-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="98838-297">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="98838-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="98838-298">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="98838-299">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="98838-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="98838-301">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="98838-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="98838-302">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="98838-303">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="98838-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="98838-304">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="98838-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="98838-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="98838-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="98838-306">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="98838-306">In the following example:</span></span>

* <span data-ttu-id="98838-307"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="98838-308">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98838-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="98838-309">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="98838-310">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="98838-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="98838-311">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="98838-311">Additional resources</span></span>

* [<span data-ttu-id="98838-312">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="98838-313">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="98838-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="98838-314">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="98838-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="98838-315">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="98838-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="98838-316">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="98838-317">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="98838-317">It offers the following benefits:</span></span>

* <span data-ttu-id="98838-318">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="98838-319">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="98838-320">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="98838-321">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="98838-322">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="98838-323">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="98838-324">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98838-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="98838-325">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="98838-325">Consumption patterns</span></span>

<span data-ttu-id="98838-326">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="98838-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="98838-327">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="98838-328">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="98838-329">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="98838-330">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="98838-331">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="98838-332">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="98838-333">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-333">Basic usage</span></span>

<span data-ttu-id="98838-334">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="98838-335">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="98838-336">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="98838-337">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="98838-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="98838-338">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="98838-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="98838-339">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="98838-340">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-340">Named clients</span></span>

<span data-ttu-id="98838-341">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="98838-342">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="98838-343">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="98838-344">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="98838-345">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="98838-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="98838-346">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="98838-347">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="98838-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="98838-348">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="98838-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="98838-349">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="98838-350">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-350">Typed clients</span></span>

<span data-ttu-id="98838-351">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="98838-351">Typed clients:</span></span>

* <span data-ttu-id="98838-352">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="98838-353">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="98838-354">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="98838-355">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="98838-356">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="98838-357">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="98838-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="98838-358">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="98838-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="98838-359">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="98838-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="98838-360">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98838-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="98838-361">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="98838-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="98838-362">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="98838-363">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="98838-364">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="98838-365">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="98838-366">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98838-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="98838-367">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="98838-368">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="98838-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="98838-369">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="98838-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="98838-370">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-370">Generated clients</span></span>

<span data-ttu-id="98838-371">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="98838-372">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="98838-373">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="98838-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="98838-374">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98838-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="98838-375">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="98838-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="98838-376">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-376">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="98838-377">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="98838-378">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="98838-378">Outgoing request middleware</span></span>

<span data-ttu-id="98838-379">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="98838-380">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="98838-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="98838-381">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="98838-382">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="98838-383">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="98838-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="98838-384">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="98838-385">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="98838-386">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="98838-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="98838-387">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="98838-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="98838-388">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="98838-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="98838-389">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="98838-390">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="98838-391">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="98838-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="98838-392">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="98838-393">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98838-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="98838-394">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="98838-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="98838-395">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="98838-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="98838-396">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="98838-397">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="98838-398">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="98838-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="98838-399">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="98838-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="98838-400">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="98838-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="98838-401">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="98838-402">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="98838-403">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="98838-403">Use Polly-based handlers</span></span>

<span data-ttu-id="98838-404">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="98838-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="98838-405">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="98838-406">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="98838-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="98838-407">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="98838-408">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="98838-408">The Polly extensions:</span></span>

* <span data-ttu-id="98838-409">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="98838-410">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="98838-411">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="98838-412">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="98838-412">Handle transient faults</span></span>

<span data-ttu-id="98838-413">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="98838-414">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="98838-415">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="98838-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="98838-416">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="98838-417">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="98838-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="98838-418">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98838-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="98838-419">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="98838-420">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="98838-420">Dynamically select policies</span></span>

<span data-ttu-id="98838-421">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="98838-422">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="98838-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="98838-423">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="98838-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="98838-424">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="98838-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="98838-425">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="98838-426">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="98838-427">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="98838-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="98838-428">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="98838-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="98838-429">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="98838-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="98838-430">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="98838-431">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="98838-432">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="98838-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="98838-433">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="98838-434">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="98838-435">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="98838-436">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="98838-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="98838-437">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="98838-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="98838-438">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="98838-439">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="98838-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="98838-440">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="98838-441">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="98838-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="98838-442">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-443">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="98838-444">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="98838-445">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="98838-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="98838-446">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="98838-447">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="98838-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="98838-448">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="98838-449">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="98838-450">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="98838-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="98838-451">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="98838-452">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="98838-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="98838-453">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="98838-454">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="98838-455">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="98838-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="98838-456">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="98838-457">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="98838-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-458">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98838-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="98838-459">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="98838-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="98838-460">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="98838-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="98838-461">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="98838-462">Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="98838-463">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="98838-464">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="98838-465"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="98838-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="98838-466">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="98838-467">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="98838-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="98838-468">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="98838-469">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="98838-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="98838-470">`SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="98838-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="98838-471">Özgü</span><span class="sxs-lookup"><span data-stu-id="98838-471">Cookies</span></span>

<span data-ttu-id="98838-472">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98838-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="98838-473">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="98838-474">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="98838-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="98838-475">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="98838-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="98838-476">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="98838-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="98838-477">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="98838-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="98838-478">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="98838-478">Logging</span></span>

<span data-ttu-id="98838-479">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="98838-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="98838-480">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="98838-481">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="98838-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="98838-482">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="98838-483">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="98838-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="98838-484">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="98838-485">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="98838-486">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="98838-487">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="98838-488">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="98838-489">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="98838-490">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="98838-491">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="98838-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="98838-492">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="98838-493">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="98838-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="98838-494">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="98838-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="98838-495">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="98838-496">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="98838-497"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="98838-498">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="98838-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="98838-499">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="98838-500">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="98838-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="98838-501">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="98838-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="98838-502">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="98838-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="98838-503">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="98838-503">In the following example:</span></span>

* <span data-ttu-id="98838-504"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="98838-505">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98838-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="98838-506">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="98838-507">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="98838-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="98838-508">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="98838-508">Additional resources</span></span>

* [<span data-ttu-id="98838-509">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="98838-510">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="98838-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="98838-511">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="98838-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="98838-512">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="98838-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="98838-513">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="98838-514">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="98838-514">It offers the following benefits:</span></span>

* <span data-ttu-id="98838-515">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="98838-516">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="98838-517">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="98838-518">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="98838-519">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="98838-520">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="98838-521">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98838-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98838-522">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="98838-522">Prerequisites</span></span>

<span data-ttu-id="98838-523">.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98838-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="98838-524">.NET Core ile hedeflenen ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="98838-525">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="98838-525">Consumption patterns</span></span>

<span data-ttu-id="98838-526">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="98838-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="98838-527">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="98838-528">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="98838-529">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="98838-530">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="98838-531">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="98838-532">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="98838-533">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="98838-533">Basic usage</span></span>

<span data-ttu-id="98838-534">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="98838-535">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="98838-536">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="98838-537">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="98838-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="98838-538">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="98838-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="98838-539">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="98838-540">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-540">Named clients</span></span>

<span data-ttu-id="98838-541">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="98838-542">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="98838-543">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="98838-544">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="98838-545">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="98838-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="98838-546">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="98838-547">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="98838-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="98838-548">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="98838-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="98838-549">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="98838-550">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-550">Typed clients</span></span>

<span data-ttu-id="98838-551">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="98838-551">Typed clients:</span></span>

* <span data-ttu-id="98838-552">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="98838-553">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="98838-554">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="98838-555">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="98838-556">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="98838-557">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="98838-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="98838-558">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="98838-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="98838-559">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="98838-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="98838-560">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98838-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="98838-561">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="98838-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="98838-562">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="98838-563">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="98838-564">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="98838-565">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="98838-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="98838-566">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98838-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="98838-567">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="98838-568">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="98838-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="98838-569">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="98838-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="98838-570">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="98838-570">Generated clients</span></span>

<span data-ttu-id="98838-571">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="98838-572">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="98838-573">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="98838-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="98838-574">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98838-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="98838-575">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="98838-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="98838-576">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="98838-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="98838-577">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="98838-578">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="98838-578">Outgoing request middleware</span></span>

<span data-ttu-id="98838-579">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="98838-580">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="98838-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="98838-581">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="98838-582">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="98838-583">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="98838-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="98838-584">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="98838-585">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="98838-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="98838-586">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="98838-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="98838-587">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="98838-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="98838-588">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="98838-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="98838-589">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="98838-590">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="98838-591">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="98838-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="98838-592">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="98838-593">İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="98838-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="98838-594">İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:</span><span class="sxs-lookup"><span data-stu-id="98838-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="98838-595">İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.</span><span class="sxs-lookup"><span data-stu-id="98838-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="98838-596">Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98838-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="98838-597">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, işleyici türünü geçirerek çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="98838-598">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="98838-599">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="98838-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="98838-600">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="98838-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="98838-601">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="98838-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="98838-602">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="98838-603">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="98838-604">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="98838-604">Use Polly-based handlers</span></span>

<span data-ttu-id="98838-605">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="98838-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="98838-606">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="98838-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="98838-607">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="98838-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="98838-608">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="98838-609">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="98838-609">The Polly extensions:</span></span>

* <span data-ttu-id="98838-610">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="98838-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="98838-611">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="98838-612">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="98838-613">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="98838-613">Handle transient faults</span></span>

<span data-ttu-id="98838-614">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="98838-615">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="98838-616">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="98838-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="98838-617">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="98838-618">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="98838-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="98838-619">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98838-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="98838-620">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="98838-621">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="98838-621">Dynamically select policies</span></span>

<span data-ttu-id="98838-622">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="98838-623">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="98838-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="98838-624">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="98838-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="98838-625">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="98838-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="98838-626">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="98838-627">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="98838-628">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="98838-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="98838-629">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="98838-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="98838-630">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="98838-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="98838-631">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="98838-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="98838-632">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="98838-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="98838-633">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="98838-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="98838-634">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="98838-635">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="98838-636">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="98838-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="98838-637">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="98838-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="98838-638">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="98838-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="98838-639">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="98838-640">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="98838-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="98838-641">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="98838-642">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="98838-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="98838-643">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-644">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="98838-645">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="98838-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="98838-646">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="98838-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="98838-647">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="98838-648">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="98838-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="98838-649">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="98838-650">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="98838-651">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="98838-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="98838-652">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="98838-653">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="98838-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="98838-654">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="98838-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="98838-655">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="98838-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="98838-656">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="98838-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="98838-657">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="98838-658">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="98838-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="98838-659">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98838-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="98838-660">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="98838-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="98838-661">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="98838-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="98838-662">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="98838-663">Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="98838-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="98838-664">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="98838-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="98838-665">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="98838-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="98838-666"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="98838-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="98838-667">Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98838-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="98838-668">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="98838-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="98838-669">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="98838-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="98838-670">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="98838-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="98838-671">`SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="98838-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="98838-672">Özgü</span><span class="sxs-lookup"><span data-stu-id="98838-672">Cookies</span></span>

<span data-ttu-id="98838-673">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98838-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="98838-674">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="98838-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="98838-675">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="98838-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="98838-676">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="98838-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="98838-677">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="98838-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="98838-678">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="98838-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="98838-679">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="98838-679">Logging</span></span>

<span data-ttu-id="98838-680">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="98838-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="98838-681">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="98838-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="98838-682">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="98838-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="98838-683">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="98838-684">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="98838-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="98838-685">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="98838-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="98838-686">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="98838-687">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="98838-688">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="98838-689">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="98838-690">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="98838-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="98838-691">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="98838-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="98838-692">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="98838-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="98838-693">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98838-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="98838-694">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="98838-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="98838-695">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="98838-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="98838-696">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="98838-697">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="98838-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="98838-698"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98838-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="98838-699">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="98838-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="98838-700">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="98838-701">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="98838-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="98838-702">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="98838-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="98838-703">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="98838-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="98838-704">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="98838-704">In the following example:</span></span>

* <span data-ttu-id="98838-705"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98838-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="98838-706">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98838-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="98838-707">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98838-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="98838-708">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="98838-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="98838-709">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="98838-709">Additional resources</span></span>

* [<span data-ttu-id="98838-710">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="98838-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="98838-711">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="98838-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="98838-712">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="98838-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
