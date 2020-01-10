---
title: ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın
author: stevejgordon
description: ASP.NET Core içindeki mantıksal HttpClient örneklerini yönetmek için ıhttpclientfactory arabirimini kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 482f8e28c23c621cecaf9ce111d89e9166ea6d85
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722732"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="d5790-103">ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın</span><span class="sxs-lookup"><span data-stu-id="d5790-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d5790-104">[Glenn CONDRON](https://github.com/glennc), [Ryan şimdi ak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Kirk larkabağı](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="d5790-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="d5790-105">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d5790-106">`IHttpClientFactory` aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="d5790-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="d5790-107">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d5790-108">Örneğin, *GitHub* adlı bir Istemci, [GitHub](https://github.com/)'a erişmek için kaydedilebilir ve yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d5790-109">Varsayılan istemci, genel erişim için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="d5790-110">`HttpClient`, işleyiciler için temsilci atama yoluyla giden ara yazılım kavramını daha da artırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="d5790-111">`HttpClient`' de işleyiciler temsilci seçme avantajlarından faydalanmak için, Polya tabanlı bir ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="d5790-112">Temel alınan `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="d5790-113">Otomatik yönetim, `HttpClient` yaşam sürelerini el ile yönetirken oluşan ortak DNS (etki alanı adı sistemi) sorunlarını önler.</span><span class="sxs-lookup"><span data-stu-id="d5790-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d5790-114">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d5790-115">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d5790-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d5790-116">Bu konu sürümündeki örnek kod, HTTP yanıtlarında döndürülen JSON içeriğinin serisini kaldırmak için <xref:System.Text.Json> kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="d5790-117">`Json.NET` ve `ReadAsAsync<T>`kullanan örnekler için, bu konunun 2. x sürümünü seçmek üzere sürüm seçiciyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d5790-118">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="d5790-118">Consumption patterns</span></span>

<span data-ttu-id="d5790-119">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="d5790-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d5790-120">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d5790-121">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d5790-122">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d5790-123">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d5790-124">En iyi yaklaşım, uygulamanın gereksinimlerine bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="d5790-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d5790-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-125">Basic usage</span></span>

<span data-ttu-id="d5790-126">`IHttpClientFactory`, `AddHttpClient`çağırarak kaydedilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d5790-127">`IHttpClientFactory`, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanılarak istenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d5790-128">Aşağıdaki kod, bir `HttpClient` örneği oluşturmak için `IHttpClientFactory` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d5790-129">Yukarıdaki örnekte olduğu gibi `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="d5790-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="d5790-130">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d5790-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="d5790-131">Mevcut bir uygulamada `HttpClient` örneklerinin oluşturulduğu yerlerde, bu oluşumları <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrılarıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d5790-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-132">Named clients</span></span>

<span data-ttu-id="d5790-133">Adlandırılmış istemciler şu durumlarda iyi bir seçimdir:</span><span class="sxs-lookup"><span data-stu-id="d5790-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="d5790-134">Uygulama birçok farklı `HttpClient`kullanımı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="d5790-135">Birçok `HttpClient`farklı yapılandırmaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d5790-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="d5790-136">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d5790-137">İstemcinin yapılandırıldığı önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="d5790-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="d5790-138">Temel adres `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="d5790-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="d5790-139">GitHub API 'SI ile çalışmak için iki üst bilgi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d5790-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="d5790-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="d5790-140">CreateClient</span></span>

<span data-ttu-id="d5790-141"><xref:System.Net.Http.IHttpClientFactory.CreateClient*> her çağrıldığında:</span><span class="sxs-lookup"><span data-stu-id="d5790-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="d5790-142">Yeni bir `HttpClient` örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="d5790-143">Yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-143">The configuration action is called.</span></span>

<span data-ttu-id="d5790-144">Adlandırılmış bir istemci oluşturmak için adını `CreateClient`geçirin:</span><span class="sxs-lookup"><span data-stu-id="d5790-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d5790-145">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d5790-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d5790-146">İstemci için yapılandırılan taban adresi kullanıldığından, kod yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d5790-147">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-147">Typed clients</span></span>

<span data-ttu-id="d5790-148">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="d5790-148">Typed clients:</span></span>

* <span data-ttu-id="d5790-149">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d5790-150">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d5790-151">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d5790-152">Örneğin, tek bir türü belirtilmiş istemci kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="d5790-153">Tek bir arka uç uç noktası için.</span><span class="sxs-lookup"><span data-stu-id="d5790-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="d5790-154">Uç nokta ile ilgili tüm mantığı kapsüllemek için.</span><span class="sxs-lookup"><span data-stu-id="d5790-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="d5790-155">DI ile birlikte çalışın ve uygulamada gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="d5790-156">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="d5790-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d5790-157">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="d5790-157">In the preceding code:</span></span>

* <span data-ttu-id="d5790-158">Yapılandırma, yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="d5790-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="d5790-159">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="d5790-160">`HttpClient` işlevselliği ortaya çıkaran API 'ye özgü Yöntemler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d5790-161">Örneğin, `GetAspNetDocsIssues` yöntemi açık sorunları almak için kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="d5790-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="d5790-162">Aşağıdaki kod, bir tür istemci sınıfını kaydetmek için `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="d5790-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d5790-163">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d5790-164">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d5790-165">Türü belirlenmiş bir istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d5790-166">`HttpClient`, türü belirlenmiş bir istemci içinde kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="d5790-167">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran bir yöntem tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="d5790-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d5790-168">Yukarıdaki kodda `HttpClient` bir özel alanda depolanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="d5790-169">`HttpClient` erişim, genel `GetRepos` yöntemine göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d5790-170">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-170">Generated clients</span></span>

<span data-ttu-id="d5790-171">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d5790-172">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d5790-173">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d5790-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d5790-174">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d5790-175">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="d5790-176">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="d5790-177">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d5790-178">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="d5790-178">Outgoing request middleware</span></span>

<span data-ttu-id="d5790-179">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d5790-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="d5790-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="d5790-181">Her bir adlandırılmış istemci için uygulanacak işleyiciler tanımlamayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="d5790-182">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydedilmesini ve zincirleme kullanımını destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d5790-183">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d5790-184">Bu model:</span><span class="sxs-lookup"><span data-stu-id="d5790-184">This pattern:</span></span>

  * <span data-ttu-id="d5790-185">ASP.NET Core gelen ara yazılım ardışık düzenine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="d5790-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="d5790-186">, HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar, örneğin:</span><span class="sxs-lookup"><span data-stu-id="d5790-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="d5790-187">önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="d5790-187">caching</span></span>
    * <span data-ttu-id="d5790-188">hata işleme</span><span class="sxs-lookup"><span data-stu-id="d5790-188">error handling</span></span>
    * <span data-ttu-id="d5790-189">serileştirme</span><span class="sxs-lookup"><span data-stu-id="d5790-189">serialization</span></span>
    * <span data-ttu-id="d5790-190">günlük kaydı</span><span class="sxs-lookup"><span data-stu-id="d5790-190">logging</span></span>

<span data-ttu-id="d5790-191">Temsilci seçme işleyicisi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d5790-191">To create a delegating handler:</span></span>

* <span data-ttu-id="d5790-192"><xref:System.Net.Http.DelegatingHandler>türet.</span><span class="sxs-lookup"><span data-stu-id="d5790-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="d5790-193"><xref:System.Net.Http.DelegatingHandler.SendAsync*>geçersiz kıl.</span><span class="sxs-lookup"><span data-stu-id="d5790-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="d5790-194">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d5790-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d5790-195">Yukarıdaki kod, `X-API-KEY` üst bilgisinin istekte olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d5790-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="d5790-196">`X-API-KEY` eksikse, <xref:System.Net.HttpStatusCode.BadRequest> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="d5790-197"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>bir `HttpClient` yapılandırmaya birden fazla işleyici eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="d5790-198">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d5790-199">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5790-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="d5790-200">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="d5790-201">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="d5790-202">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="d5790-203">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d5790-204">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="d5790-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d5790-205">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d5790-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d5790-206">[HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties)kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="d5790-207">Geçerli isteğe erişmek için <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="d5790-208">Verileri geçirmek için özel bir <xref:System.Threading.AsyncLocal`1> depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d5790-209">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="d5790-209">Use Polly-based handlers</span></span>

<span data-ttu-id="d5790-210">`IHttpClientFactory`, üçüncü taraf kitaplığı [Polly](https://github.com/App-vNext/Polly)ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d5790-211">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d5790-212">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d5790-213">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d5790-214">Polly uzantıları, istemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="d5790-215">Polly, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d5790-216">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="d5790-216">Handle transient faults</span></span>

<span data-ttu-id="d5790-217">Hatalar genellikle dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d5790-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, geçici hataları işlemek için bir ilkenin tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d5790-219">`AddTransientHttpErrorPolicy` ile yapılandırılan ilkeler aşağıdaki yanıtları işleyecek şekilde yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="d5790-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="d5790-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="d5790-220">HTTP 5xx</span></span>
* <span data-ttu-id="d5790-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="d5790-221">HTTP 408</span></span>

<span data-ttu-id="d5790-222">`AddTransientHttpErrorPolicy`, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="d5790-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="d5790-223">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d5790-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d5790-224">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d5790-225">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="d5790-225">Dynamically select policies</span></span>

<span data-ttu-id="d5790-226">Uzantı yöntemleri, örneğin <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>, Polly tabanlı işleyiciler eklemek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="d5790-227">Aşağıdaki `AddPolicyHandler` aşırı yüklemesi hangi ilkenin uygulanacağını belirlemek için isteği inceler:</span><span class="sxs-lookup"><span data-stu-id="d5790-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d5790-228">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d5790-229">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d5790-230">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="d5790-231">Polly ilkeleri iç içe almak yaygın bir şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="d5790-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d5790-232">Yukarıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d5790-232">In the preceding example:</span></span>

* <span data-ttu-id="d5790-233">İki işleyici eklenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-233">Two handlers are added.</span></span>
* <span data-ttu-id="d5790-234">İlk işleyici, yeniden deneme ilkesi eklemek için <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="d5790-235">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="d5790-236">İkinci `AddTransientHttpErrorPolicy` çağrısı bir devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="d5790-237">5 başarısız girişim sıralı olarak gerçekleşirse, daha fazla dış istek 30 saniye boyunca engellenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="d5790-238">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d5790-239">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d5790-240">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="d5790-241">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="d5790-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="d5790-242">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="d5790-242">In the following code:</span></span>

* <span data-ttu-id="d5790-243">"Normal" ve "uzun" ilkeler eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="d5790-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="d5790-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>, kayıt defterinden "normal" ve "uzun" ilkeleri ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="d5790-245">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi için bkz. [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="d5790-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d5790-246">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="d5790-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="d5790-247">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-248">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="d5790-249">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d5790-250">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d5790-251">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d5790-252">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d5790-253">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d5790-254">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS (etki alanı adı sistemi) değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="d5790-255">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="d5790-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d5790-256">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="d5790-257">`HttpClient` örnekleri genellikle aktiften **çıkarma gerektirmeyen .NET nesneleri olarak** kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="d5790-258">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d5790-259">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="d5790-260">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="d5790-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-261">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d5790-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d5790-262">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="d5790-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d5790-263">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="d5790-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d5790-264">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d5790-265">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="d5790-266">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d5790-267">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d5790-268"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d5790-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d5790-269">Gerektiğinde `new HttpClient(handler, disposeHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="d5790-270">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="d5790-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d5790-271">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d5790-272">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d5790-273">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="d5790-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d5790-274">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="d5790-274">Cookies</span></span>

<span data-ttu-id="d5790-275">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d5790-276">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d5790-277">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d5790-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d5790-278">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="d5790-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d5790-279">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="d5790-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d5790-280">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="d5790-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d5790-281">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d5790-281">Logging</span></span>

<span data-ttu-id="d5790-282">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="d5790-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d5790-283">Varsayılan günlük iletilerini görmek için günlük yapılandırmasında uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="d5790-284">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="d5790-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d5790-285">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d5790-286">Örneğin, *Mynamedclient*adlı bir istemci, "System .net. http. HttpClient" kategorisine sahip iletileri günlüğe kaydeder. **Mynamedclient**. LogicalHandler ".</span><span class="sxs-lookup"><span data-stu-id="d5790-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="d5790-287">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d5790-288">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d5790-289">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d5790-290">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d5790-291">*Mynamedclient* örneğinde, bu Iletiler "System .net. http. HttpClient" günlük kategorisiyle günlüğe kaydedilir. **Mynamedclient**. ClientHandler ".</span><span class="sxs-lookup"><span data-stu-id="d5790-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="d5790-292">İstek için bu, tüm diğer işleyiciler çalıştırıldıktan sonra ve istek gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="d5790-293">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d5790-294">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d5790-295">Bu, istek üst bilgilerinde veya yanıt durum kodunda yapılan değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="d5790-296">İstemcinin adını log kategorisinde da içermek, belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="d5790-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d5790-297">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d5790-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d5790-298">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d5790-299">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d5790-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d5790-301">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d5790-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d5790-302">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d5790-303">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d5790-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d5790-304">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="d5790-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d5790-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="d5790-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d5790-306">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d5790-306">In the following example:</span></span>

* <span data-ttu-id="d5790-307"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d5790-308">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5790-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d5790-309">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d5790-310">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="d5790-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d5790-311">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d5790-311">Additional resources</span></span>

* [<span data-ttu-id="d5790-312">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d5790-313">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="d5790-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d5790-314">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="d5790-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="d5790-315">.NET 'te JSON serileştirme ve serisini kaldırma</span><span class="sxs-lookup"><span data-stu-id="d5790-315">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d5790-316">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="d5790-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="d5790-317">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-317">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d5790-318">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="d5790-318">It offers the following benefits:</span></span>

* <span data-ttu-id="d5790-319">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-319">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d5790-320">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-320">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d5790-321">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-321">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d5790-322">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-322">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="d5790-323">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-323">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d5790-324">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-324">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d5790-325">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d5790-325">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d5790-326">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="d5790-326">Consumption patterns</span></span>

<span data-ttu-id="d5790-327">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="d5790-327">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d5790-328">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-328">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d5790-329">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-329">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d5790-330">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-330">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d5790-331">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-331">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d5790-332">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-332">None of them are strictly superior to another.</span></span> <span data-ttu-id="d5790-333">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-333">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d5790-334">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-334">Basic usage</span></span>

<span data-ttu-id="d5790-335">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-335">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d5790-336">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-336">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d5790-337">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-337">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d5790-338">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="d5790-338">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="d5790-339">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d5790-339">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="d5790-340">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-340">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d5790-341">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-341">Named clients</span></span>

<span data-ttu-id="d5790-342">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-342">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="d5790-343">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-343">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d5790-344">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-344">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="d5790-345">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-345">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="d5790-346">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-346">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="d5790-347">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-347">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="d5790-348">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="d5790-348">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d5790-349">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d5790-349">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d5790-350">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-350">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d5790-351">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-351">Typed clients</span></span>

<span data-ttu-id="d5790-352">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="d5790-352">Typed clients:</span></span>

* <span data-ttu-id="d5790-353">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-353">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d5790-354">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-354">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d5790-355">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-355">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d5790-356">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-356">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="d5790-357">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-357">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="d5790-358">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="d5790-358">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d5790-359">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="d5790-359">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="d5790-360">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-360">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="d5790-361">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d5790-361">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d5790-362">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="d5790-362">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="d5790-363">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-363">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d5790-364">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-364">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d5790-365">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-365">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d5790-366">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-366">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d5790-367">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d5790-367">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="d5790-368">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-368">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d5790-369">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-369">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="d5790-370">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="d5790-370">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d5790-371">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-371">Generated clients</span></span>

<span data-ttu-id="d5790-372">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-372">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d5790-373">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-373">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d5790-374">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d5790-374">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d5790-375">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-375">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d5790-376">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-376">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="d5790-377">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-377">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="d5790-378">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-378">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d5790-379">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="d5790-379">Outgoing request middleware</span></span>

<span data-ttu-id="d5790-380">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-380">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d5790-381">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-381">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="d5790-382">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-382">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d5790-383">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-383">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d5790-384">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="d5790-384">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d5790-385">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-385">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="d5790-386">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-386">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="d5790-387">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="d5790-387">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d5790-388">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-388">The preceding code defines a basic handler.</span></span> <span data-ttu-id="d5790-389">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d5790-389">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="d5790-390">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-390">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="d5790-391">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-391">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="d5790-392">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-392">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="d5790-393">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-393">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d5790-394">`IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5790-394">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="d5790-395">İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="d5790-395">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="d5790-396">İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-396">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="d5790-397">Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-397">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="d5790-398">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-398">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d5790-399">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="d5790-399">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d5790-400">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d5790-400">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d5790-401">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-401">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="d5790-402">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-402">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="d5790-403">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-403">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d5790-404">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="d5790-404">Use Polly-based handlers</span></span>

<span data-ttu-id="d5790-405">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-405">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d5790-406">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-406">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d5790-407">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-407">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d5790-408">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-408">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d5790-409">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="d5790-409">The Polly extensions:</span></span>

* <span data-ttu-id="d5790-410">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-410">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="d5790-411">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-411">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="d5790-412">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-412">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d5790-413">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="d5790-413">Handle transient faults</span></span>

<span data-ttu-id="d5790-414">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-414">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d5790-415">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-415">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d5790-416">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="d5790-416">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="d5790-417">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-417">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d5790-418">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="d5790-418">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="d5790-419">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d5790-419">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d5790-420">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-420">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d5790-421">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="d5790-421">Dynamically select policies</span></span>

<span data-ttu-id="d5790-422">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-422">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="d5790-423">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="d5790-423">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="d5790-424">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="d5790-424">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d5790-425">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-425">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d5790-426">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-426">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d5790-427">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-427">Add multiple Polly handlers</span></span>

<span data-ttu-id="d5790-428">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="d5790-428">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d5790-429">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="d5790-429">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="d5790-430">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-430">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="d5790-431">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-431">Failed requests are retried up to three times.</span></span> <span data-ttu-id="d5790-432">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-432">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="d5790-433">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-433">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="d5790-434">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-434">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d5790-435">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-435">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d5790-436">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-436">Add policies from the Polly registry</span></span>

<span data-ttu-id="d5790-437">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="d5790-437">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="d5790-438">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-438">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="d5790-439">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-439">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="d5790-440">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="d5790-440">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="d5790-441">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-441">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d5790-442">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="d5790-442">HttpClient and lifetime management</span></span>

<span data-ttu-id="d5790-443">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-443">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-444">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-444">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="d5790-445">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-445">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d5790-446">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-446">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d5790-447">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-447">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d5790-448">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-448">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d5790-449">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-449">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d5790-450">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-450">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="d5790-451">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="d5790-451">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d5790-452">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-452">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="d5790-453">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="d5790-453">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="d5790-454">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-454">Disposal of the client isn't required.</span></span> <span data-ttu-id="d5790-455">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-455">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d5790-456">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-456">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="d5790-457">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-457">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="d5790-458">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="d5790-458">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-459">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d5790-459">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d5790-460">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="d5790-460">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d5790-461">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="d5790-461">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d5790-462">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-462">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d5790-463">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-463">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="d5790-464">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-464">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d5790-465">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-465">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d5790-466"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d5790-466">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d5790-467">Gerektiğinde `new HttpClient(handler, disposeHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-467">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="d5790-468">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="d5790-468">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d5790-469">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-469">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d5790-470">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-470">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d5790-471">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="d5790-471">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d5790-472">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="d5790-472">Cookies</span></span>

<span data-ttu-id="d5790-473">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-473">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d5790-474">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-474">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d5790-475">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d5790-475">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d5790-476">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="d5790-476">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d5790-477">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="d5790-477">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d5790-478">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="d5790-478">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d5790-479">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d5790-479">Logging</span></span>

<span data-ttu-id="d5790-480">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="d5790-480">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d5790-481">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-481">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="d5790-482">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="d5790-482">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d5790-483">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-483">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d5790-484">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="d5790-484">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="d5790-485">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-485">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d5790-486">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-486">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d5790-487">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-487">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d5790-488">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-488">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d5790-489">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-489">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="d5790-490">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-490">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="d5790-491">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-491">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d5790-492">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-492">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d5790-493">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-493">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="d5790-494">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="d5790-494">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d5790-495">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d5790-495">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d5790-496">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-496">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d5790-497">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-497">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d5790-498"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-498">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d5790-499">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d5790-499">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d5790-500">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-500">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d5790-501">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d5790-501">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d5790-502">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="d5790-502">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d5790-503">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="d5790-503">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d5790-504">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d5790-504">In the following example:</span></span>

* <span data-ttu-id="d5790-505"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-505"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d5790-506">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5790-506">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d5790-507">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-507">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d5790-508">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="d5790-508">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d5790-509">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d5790-509">Additional resources</span></span>

* [<span data-ttu-id="d5790-510">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-510">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d5790-511">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="d5790-511">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d5790-512">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="d5790-512">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="d5790-513">, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="d5790-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="d5790-514">Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-514">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d5790-515">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="d5790-515">It offers the following benefits:</span></span>

* <span data-ttu-id="d5790-516">, Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-516">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d5790-517">Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-517">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d5790-518">Varsayılan istemci, diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-518">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d5790-519">`HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-519">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="d5790-520">`HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-520">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d5790-521">Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-521">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d5790-522">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d5790-522">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5790-523">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="d5790-523">Prerequisites</span></span>

<span data-ttu-id="d5790-524">.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-524">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="d5790-525">.NET Core ile hedeflenen ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-525">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d5790-526">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="d5790-526">Consumption patterns</span></span>

<span data-ttu-id="d5790-527">Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:</span><span class="sxs-lookup"><span data-stu-id="d5790-527">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d5790-528">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-528">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d5790-529">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-529">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d5790-530">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-530">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d5790-531">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-531">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d5790-532">Hiçbiri diğerinden tamamen üst değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-532">None of them are strictly superior to another.</span></span> <span data-ttu-id="d5790-533">En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-533">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d5790-534">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="d5790-534">Basic usage</span></span>

<span data-ttu-id="d5790-535">`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-535">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d5790-536">Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-536">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d5790-537">`IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-537">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d5790-538">Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="d5790-538">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="d5790-539">`HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d5790-539">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="d5790-540">`HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-540">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d5790-541">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-541">Named clients</span></span>

<span data-ttu-id="d5790-542">Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-542">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="d5790-543">Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-543">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d5790-544">Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-544">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="d5790-545">Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-545">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="d5790-546">`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-546">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="d5790-547">Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-547">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="d5790-548">Oluşturulacak istemcinin adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="d5790-548">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d5790-549">Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d5790-549">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d5790-550">İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-550">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d5790-551">Yazılan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-551">Typed clients</span></span>

<span data-ttu-id="d5790-552">Yazılan istemciler:</span><span class="sxs-lookup"><span data-stu-id="d5790-552">Typed clients:</span></span>

* <span data-ttu-id="d5790-553">Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-553">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d5790-554">İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-554">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d5790-555">Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-555">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d5790-556">Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-556">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="d5790-557">DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-557">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="d5790-558">Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="d5790-558">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d5790-559">Önceki kodda, yapılandırma yazılan istemciye taşınır.</span><span class="sxs-lookup"><span data-stu-id="d5790-559">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="d5790-560">`HttpClient` nesnesi ortak bir özellik olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-560">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="d5790-561">`HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d5790-561">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d5790-562">`GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="d5790-562">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="d5790-563">Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-563">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d5790-564">Yazılan istemci, DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-564">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d5790-565">Yazılan istemci doğrudan eklenebilir ve tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-565">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d5790-566">Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-566">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d5790-567">Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d5790-567">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="d5790-568">Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-568">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d5790-569">Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-569">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="d5790-570">Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.</span><span class="sxs-lookup"><span data-stu-id="d5790-570">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d5790-571">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="d5790-571">Generated clients</span></span>

<span data-ttu-id="d5790-572">`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-572">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d5790-573">Yeniden sığdırma, .NET için bir REST kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-573">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d5790-574">REST API 'Leri canlı arabirimlere dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d5790-574">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d5790-575">Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d5790-575">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d5790-576">Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-576">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="d5790-577">Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d5790-577">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="d5790-578">Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-578">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d5790-579">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="d5790-579">Outgoing request middleware</span></span>

<span data-ttu-id="d5790-580">`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-580">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d5790-581">`IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-581">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="d5790-582">Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-582">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d5790-583">Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-583">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d5790-584">Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer.</span><span class="sxs-lookup"><span data-stu-id="d5790-584">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d5790-585">Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-585">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="d5790-586">Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d5790-586">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="d5790-587">İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="d5790-587">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d5790-588">Yukarıdaki kod, temel bir işleyiciyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-588">The preceding code defines a basic handler.</span></span> <span data-ttu-id="d5790-589">İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d5790-589">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="d5790-590">Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-590">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="d5790-591">Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-591">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="d5790-592">Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-592">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="d5790-593">Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-593">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d5790-594">İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="d5790-594">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="d5790-595">İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:</span><span class="sxs-lookup"><span data-stu-id="d5790-595">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="d5790-596">İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.</span><span class="sxs-lookup"><span data-stu-id="d5790-596">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="d5790-597">Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-597">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="d5790-598">Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, işleyici türünü geçirerek çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-598">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="d5790-599">Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-599">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d5790-600">Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:</span><span class="sxs-lookup"><span data-stu-id="d5790-600">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d5790-601">İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d5790-601">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d5790-602">`HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-602">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="d5790-603">Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-603">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="d5790-604">Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-604">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d5790-605">Polly tabanlı işleyiciler kullanın</span><span class="sxs-lookup"><span data-stu-id="d5790-605">Use Polly-based handlers</span></span>

<span data-ttu-id="d5790-606">`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-606">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d5790-607">Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="d5790-607">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d5790-608">Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-608">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d5790-609">Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-609">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d5790-610">Polly uzantıları:</span><span class="sxs-lookup"><span data-stu-id="d5790-610">The Polly extensions:</span></span>

* <span data-ttu-id="d5790-611">İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-611">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="d5790-612">, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-612">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="d5790-613">Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-613">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d5790-614">Geçici hataları işle</span><span class="sxs-lookup"><span data-stu-id="d5790-614">Handle transient faults</span></span>

<span data-ttu-id="d5790-615">Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-615">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d5790-616">`AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-616">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d5790-617">Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.</span><span class="sxs-lookup"><span data-stu-id="d5790-617">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="d5790-618">`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-618">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d5790-619">Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:</span><span class="sxs-lookup"><span data-stu-id="d5790-619">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="d5790-620">Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d5790-620">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d5790-621">Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-621">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d5790-622">Dinamik olarak ilke seçme</span><span class="sxs-lookup"><span data-stu-id="d5790-622">Dynamically select policies</span></span>

<span data-ttu-id="d5790-623">Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-623">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="d5790-624">Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`.</span><span class="sxs-lookup"><span data-stu-id="d5790-624">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="d5790-625">Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:</span><span class="sxs-lookup"><span data-stu-id="d5790-625">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d5790-626">Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-626">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d5790-627">Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-627">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d5790-628">Birden çok Polly işleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-628">Add multiple Polly handlers</span></span>

<span data-ttu-id="d5790-629">Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:</span><span class="sxs-lookup"><span data-stu-id="d5790-629">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d5790-630">Yukarıdaki örnekte, iki işleyici eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="d5790-630">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="d5790-631">İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-631">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="d5790-632">Başarısız istekler en fazla üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-632">Failed requests are retried up to three times.</span></span> <span data-ttu-id="d5790-633">`AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="d5790-633">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="d5790-634">Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir.</span><span class="sxs-lookup"><span data-stu-id="d5790-634">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="d5790-635">Devre kesici ilkeleri durum bilgisi vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-635">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d5790-636">Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-636">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d5790-637">Polly kayıt defterinden ilke ekleme</span><span class="sxs-lookup"><span data-stu-id="d5790-637">Add policies from the Polly registry</span></span>

<span data-ttu-id="d5790-638">Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.</span><span class="sxs-lookup"><span data-stu-id="d5790-638">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="d5790-639">Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="d5790-639">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="d5790-640">Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-640">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="d5790-641">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.</span><span class="sxs-lookup"><span data-stu-id="d5790-641">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="d5790-642">`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-642">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d5790-643">HttpClient ve ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="d5790-643">HttpClient and lifetime management</span></span>

<span data-ttu-id="d5790-644">`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-644">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-645">Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-645">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="d5790-646">Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.</span><span class="sxs-lookup"><span data-stu-id="d5790-646">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d5790-647">kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-647">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d5790-648">`HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-648">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d5790-649">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-649">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d5790-650">Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-650">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d5790-651">Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-651">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="d5790-652">Varsayılan işleyici ömrü iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="d5790-652">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d5790-653">Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-653">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="d5790-654">Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="d5790-654">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="d5790-655">İstemcinin çıkarılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d5790-655">Disposal of the client isn't required.</span></span> <span data-ttu-id="d5790-656">Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5790-656">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d5790-657">`IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d5790-657">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="d5790-658">`HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-658">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="d5790-659">Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="d5790-659">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d5790-660">Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d5790-660">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d5790-661">Ihttpclientfactory alternatifleri</span><span class="sxs-lookup"><span data-stu-id="d5790-661">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d5790-662">Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:</span><span class="sxs-lookup"><span data-stu-id="d5790-662">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d5790-663">`HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-663">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d5790-664">Düzenli aralıklarla `HttpMessageHandler` örnekleri arasında geçiş yaparak eski DNS sorunları.</span><span class="sxs-lookup"><span data-stu-id="d5790-664">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="d5790-665">Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.</span><span class="sxs-lookup"><span data-stu-id="d5790-665">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d5790-666">Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.</span><span class="sxs-lookup"><span data-stu-id="d5790-666">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d5790-667"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d5790-667">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d5790-668">Gerektiğinde `new HttpClient(handler, disposeHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d5790-668">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="d5790-669">Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.</span><span class="sxs-lookup"><span data-stu-id="d5790-669">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d5790-670">`SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır.</span><span class="sxs-lookup"><span data-stu-id="d5790-670">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d5790-671">Bu paylaşım, yuva azalmasına engel olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-671">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d5790-672">`SocketsHttpHandler`, eski DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.</span><span class="sxs-lookup"><span data-stu-id="d5790-672">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d5790-673">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="d5790-673">Cookies</span></span>

<span data-ttu-id="d5790-674">Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d5790-674">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d5790-675">Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="d5790-675">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d5790-676">Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d5790-676">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d5790-677">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="d5790-677">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d5790-678">`IHttpClientFactory` önleme</span><span class="sxs-lookup"><span data-stu-id="d5790-678">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d5790-679">Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="d5790-679">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d5790-680">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d5790-680">Logging</span></span>

<span data-ttu-id="d5790-681">Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler.</span><span class="sxs-lookup"><span data-stu-id="d5790-681">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d5790-682">Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d5790-682">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="d5790-683">İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="d5790-683">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d5790-684">Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-684">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d5790-685">Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="d5790-685">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="d5790-686">*Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5790-686">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d5790-687">İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-687">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d5790-688">Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-688">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d5790-689">Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-689">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d5790-690">*Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-690">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="d5790-691">İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="d5790-691">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="d5790-692">Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="d5790-692">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d5790-693">İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="d5790-693">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d5790-694">Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-694">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="d5790-695">İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.</span><span class="sxs-lookup"><span data-stu-id="d5790-695">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d5790-696">HttpMessageHandler 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d5790-696">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d5790-697">İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-697">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d5790-698">Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d5790-698">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d5790-699"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-699">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d5790-700">Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d5790-700">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d5790-701">Konsol uygulamasında ıhttpclientfactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-701">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d5790-702">Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d5790-702">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d5790-703">Microsoft. Extensions. Hosting</span><span class="sxs-lookup"><span data-stu-id="d5790-703">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d5790-704">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="d5790-704">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d5790-705">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d5790-705">In the following example:</span></span>

* <span data-ttu-id="d5790-706"><xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d5790-706"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d5790-707">`MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d5790-707">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d5790-708">`HttpClient`, bir Web sayfasını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d5790-708">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d5790-709">`Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="d5790-709">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d5790-710">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d5790-710">Additional resources</span></span>

* [<span data-ttu-id="d5790-711">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="d5790-711">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d5790-712">HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="d5790-712">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d5790-713">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="d5790-713">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
