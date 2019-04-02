---
title: ASP.NET Core IHttpClientFactory kullanarak HTTP istekleri
author: stevejgordon
description: ASP.NET core'da mantıksal HttpClient örneğini yönetmek için IHttpClientFactory arabirimi kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 270727443f091306ac3e4ce4e2ceb99b88bbc609
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809214"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="b0d1a-103">ASP.NET Core IHttpClientFactory kullanarak HTTP istekleri</span><span class="sxs-lookup"><span data-stu-id="b0d1a-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

<span data-ttu-id="b0d1a-104">Tarafından [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="b0d1a-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="b0d1a-105">Bir <xref:System.Net.Http.IHttpClientFactory> kayıtlı ve oluşturma ve yapılandırma için kullanılan <xref:System.Net.Http.HttpClient> uygulama örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="b0d1a-106">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-106">It offers the following benefits:</span></span>

* <span data-ttu-id="b0d1a-107">Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="b0d1a-108">Örneğin, bir *github* istemci kayıtlı ve GitHub erişim sağlamak için yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="b0d1a-109">Varsayılan istemci diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="b0d1a-110">İşleyicileri temsilci aracılığıyla giden ara yazılım kavramı'ı kodlar `HttpClient` ve, yararlanmak Polly tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="b0d1a-111">Havuzu ve arka plandaki, yaşam süresini yöneten `HttpClientMessageHandler` el ile yönetilmesi sırasında oluşan Genel DNS sorunları önlemek için örnekleri `HttpClient` yaşam süresi yok.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="b0d1a-112">Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri aracılığıyla gönderilen tüm istekler için.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="b0d1a-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b0d1a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0d1a-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b0d1a-114">Prerequisites</span></span>

<span data-ttu-id="b0d1a-115">.NET Framework'ü hedefleyen projeleri yüklenmesini gerektiren [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="b0d1a-116">.NET Core ve başvuru hedefleyen projeler [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) zaten dahil `Microsoft.Extensions.Http` paket.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="b0d1a-117">Tüketim modelleri</span><span class="sxs-lookup"><span data-stu-id="b0d1a-117">Consumption patterns</span></span>

<span data-ttu-id="b0d1a-118">Birkaç şekilde `IHttpClientFactory` bir uygulamada kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="b0d1a-119">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="b0d1a-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="b0d1a-120">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="b0d1a-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="b0d1a-121">Türü belirlenmiş istemci</span><span class="sxs-lookup"><span data-stu-id="b0d1a-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="b0d1a-122">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="b0d1a-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="b0d1a-123">Bunların hiçbiri diğerine kesinlikle üst.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="b0d1a-124">En iyi yaklaşım, uygulamanın kısıtlamaları bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="b0d1a-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="b0d1a-125">Basic usage</span></span>

<span data-ttu-id="b0d1a-126">`IHttpClientFactory` Çağırarak kayıtlı `AddHttpClient` genişletme yöntemini `IServiceCollection`içine `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="b0d1a-127">Kaydedildikten sonra kod kabul edebilen bir `IHttpClientFactory` Hizmetleri ile her yerde yerleştirilebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b0d1a-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b0d1a-128">`IHttpClientFactory` Oluşturmak için kullanılan bir `HttpClient` örneği:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="b0d1a-129">Kullanarak `IHttpClientFactory` bu şekilde var olan bir uygulamayı yeniden düzenlenmesi için iyi bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-129">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="b0d1a-130">Yolda hiçbir etkisi olmaz `HttpClient` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="b0d1a-131">Yerde nerede `HttpClient` örnekleri şu anda oluşturulur, bu oluşumları çağrısı ile Değiştir <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="b0d1a-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="b0d1a-132">Named clients</span></span>

<span data-ttu-id="b0d1a-133">Bir uygulama birçok farklı kullanımlarını gerektiriyorsa `HttpClient`, her farklı bir yapılandırma ile bir seçenek kullanmaktır **istemcileri adlı**.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="b0d1a-134">Yapılandırma için bir adlandırılmış `HttpClient` kaydı sırasında belirtilen `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="b0d1a-135">Önceki kodda, `AddHttpClient` olarak adlandırılan, bir ad sağlamayı *github*.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="b0d1a-136">Bu istemci bazı varsayılan yapılandırma uygulandı sahip&mdash;taban adresini ve GitHub API ile çalışması için gereken iki üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="b0d1a-137">Her zaman `CreateClient` çağrılır, yeni bir örneğini `HttpClient` oluşturulur ve yapılandırma eylem olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="b0d1a-138">Adlandırılmış bir istemcinin kullanılacağı bir dize parametresi geçirilebilir `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="b0d1a-139">Oluşturulacak istemci adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="b0d1a-140">Önceki kodda, istek bir ana bilgisayar adı belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="b0d1a-141">İstemcisi için yapılandırılan taban adresi kullanıldığından, yol yalnızca geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="b0d1a-142">Türü belirlenmiş istemci</span><span class="sxs-lookup"><span data-stu-id="b0d1a-142">Typed clients</span></span>

<span data-ttu-id="b0d1a-143">Türü belirlenmiş istemci anahtarı olarak dizeleri kullanmak zorunda kalmadan adlandırılmış istemcileri ile aynı özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="b0d1a-144">Türü belirlenmiş istemci yaklaşım, IntelliSense ve derleyici istemciler tüketildiğinde Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="b0d1a-145">Yapılandırma ve belirli bir ile etkileşim kurmak için tek bir konum sağladıkları `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="b0d1a-146">Örneğin, tek bir türü belirlenmiş istemci tek bir arka uç noktası için kullanılabilir ve bu uç nokta tüm mantığı uğraşmanızı kapsüller.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="b0d1a-147">Başka bir avantajı DI, iş ve uygulamanızı gerekli olduğu durumlarda yerleştirilebilir ' dir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="b0d1a-148">Türü belirlenmiş istemci kabul eden bir `HttpClient` oluşturucusuna parametre:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b0d1a-149">Önceki kodda, türü belirlenmiş istemci yapılandırma taşınır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="b0d1a-150">`HttpClient` Nesne, ortak bir özellik olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="b0d1a-151">Kullanıma sunan bir özel API yöntemleri tanımlamak mümkündür `HttpClient` işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="b0d1a-152">`GetAspNetDocsIssues` Yöntemi en son açık sorunlar bir GitHub deposundan ayrıştırabilir ve sorgulamak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="b0d1a-153">Türü belirlenmiş bir istemci, genel kaydedilecek <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> genişletme yöntemi içinde kullanılabilir `Startup.ConfigureServices`, türü belirlenmiş istemci sınıfı belirtme:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-153">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="b0d1a-154">Türü belirlenmiş istemci DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="b0d1a-155">Türü belirlenmiş istemci eklenen ve doğrudan tüketilen:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="b0d1a-156">Kaydı sırasında tercih etmeleri durumunda, türü belirlenmiş istemci yapılandırmasını belirtilebilir `Startup.ConfigureServices`, yerine belirlenmiş istemcinin Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="b0d1a-157">Tamamen yalıtılacak mümkündür `HttpClient` türü belirlenmiş istemci içinde.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="b0d1a-158">Bir özellik olarak gösterme yerine genel yöntemleri arama sağlanabilir `HttpClient` dahili olarak örneği.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="b0d1a-159">Önceki kodda, `HttpClient` özel bir alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="b0d1a-160">Dış çağrı yapmak için tüm erişim geçtiği `GetRepos` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="b0d1a-161">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="b0d1a-161">Generated clients</span></span>

<span data-ttu-id="b0d1a-162">`IHttpClientFactory` diğer üçüncü taraf kitaplıklar ile birlikte gibi kullanılabilir [yeniden donatılması](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="b0d1a-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="b0d1a-163">Refit, .NET için bir REST kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="b0d1a-164">Bu REST API Canlı arabirimleri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="b0d1a-165">Arabiriminin göre dinamik olarak oluşturulan `RestService`kullanarak `HttpClient` dış HTTP çağrısı.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="b0d1a-166">Dış API ve yanıtını temsil etmek için bir arabirim ve bir yanıt tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="b0d1a-167">Uygulama oluşturmak için Refit kullanarak bir türü belirlenmiş istemci eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="b0d1a-168">Tanımlanan arabirimi gerektiğinde DI ve Refit tarafından sağlanan uygulama ile kullanılabilecek:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="b0d1a-169">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="b0d1a-169">Outgoing request middleware</span></span>

<span data-ttu-id="b0d1a-170">`HttpClient` Giden HTTP istekleri için birbirine işleyicileri temsilci olarak görevlendirme kavramı zaten sahip.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="b0d1a-171">`IHttpClientFactory` Adlandırılmış her istemci için uygulanacak işleyicilerini tanımlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="b0d1a-172">Bu, kayıt ve giden bir istek ara yazılım ardışık düzenini oluşturmak için birden çok işleyicileri zincirleme destekler.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="b0d1a-173">Bu işleyiciler, her giden istekten önce ve sonra çalışma gerçekleştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="b0d1a-174">Bu düzen, ASP.NET Core gelen ara yazılım ardışık benzerdir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="b0d1a-175">Desen etrafında HTTP isteklerini, önbelleğe alma, hata, seri hale getirme, işleme ve günlüğe kaydetme gibi çapraz kesme konuları yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="b0d1a-176">Bir işleyici oluşturmak için türetilen bir sınıf tanımlama <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-176">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="b0d1a-177">Geçersiz kılma `SendAsync` yöntemi istek ardışık düzende sonraki işleyici geçirmeden önce kodu çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="b0d1a-178">Yukarıdaki kod, bir temel işleyicisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="b0d1a-179">Olup olmadığını denetler bir `X-API-KEY` üst bilgi, istek dahil edilmemiş.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="b0d1a-180">Üst bilgisi eksik, bu HTTP çağrısı kaçınmak ve uygun bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="b0d1a-181">Kayıt sırasında bir veya daha fazla işleyicileri için yapılandırmasına eklenebilir bir `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="b0d1a-182">Bu görev üzerinde genişletme yöntemleri gerçekleştirilir <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-182">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b0d1a-183">Önceki kodda, `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="b0d1a-184">`IHttpClientFactory` Her işleyicisi için ayrı bir DI kapsamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-184">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="b0d1a-185">Bağımlı hizmetleri herhangi bir kapsamın işleyicileri ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-185">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="b0d1a-186">İşleyici işleyicileri bağlı Hizmetleri silinmediğinde.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-186">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="b0d1a-187">Bir kez kayıtlı <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> türü için işleyici geçirme çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-187">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b0d1a-188">Önceki kodda, `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-188">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="b0d1a-189">İşleyici **gerekir** hiçbir zaman kapsamlı bir geçici hizmet kayıtlı DI içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-189">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="b0d1a-190">İşleyici işleyici başarısız olmasına neden kapsam dışına çıkmadan önce işleyici kapsamlı bir hizmet olarak kayıtlı ve işleyici bağımlı olan hizmetler atılabilir, işleyicinin Hizmetleri elden.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-190">If the handler is registered as a scoped service and any services that the handler depends upon are disposable, the handler's services could be disposed before the handler goes out of scope, which would cause the handler to fail.</span></span>

<span data-ttu-id="b0d1a-191">Bir kez kayıtlı <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> işleyici türü çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-191">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

::: moniker-end

<span data-ttu-id="b0d1a-192">Birden fazla işleyici sırayla yürütülmesi gerektiğini kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-192">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="b0d1a-193">Her işleyici son kadar bir sonraki işleyici sarmalar `HttpClientHandler` isteği yürütür:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-193">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="b0d1a-194">İstek başına durum ileti işleyicileri ile paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-194">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="b0d1a-195">İşleyici kullanarak veri iletmek `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-195">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="b0d1a-196">Kullanım `IHttpContextAccessor` geçerli istek erişmek için.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-196">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="b0d1a-197">Bir özel Oluştur `AsyncLocal` veri iletmek için depolama nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-197">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="b0d1a-198">Polly tabanlı işleyicileri kullanın</span><span class="sxs-lookup"><span data-stu-id="b0d1a-198">Use Polly-based handlers</span></span>

<span data-ttu-id="b0d1a-199">`IHttpClientFactory` adlı bir popüler üçüncü taraf kitaplığı ile tümleştirilir [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="b0d1a-199">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="b0d1a-200">Polly kapsamlı esnekliği ve .NET için geçici hata işleme Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-200">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="b0d1a-201">Bu hizmet sayesinde geliştiriciler, yeniden deneme, devre kesici, zaman aşımı, bölme perdesi yalıtım ve geri dönüş gibi ilkeleri fluent ve iş parçacığı açısından güvenli bir şekilde ifade etmek.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-201">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="b0d1a-202">Genişletme yöntemleri, Polly ilkeleriyle kullanımını etkinleştirmek için yapılandırılmış sağlanan `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-202">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="b0d1a-203">Polly uzantıları kullanılabilir [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-203">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="b0d1a-204">Bu paket bulunup bulunmadığına [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b0d1a-204">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b0d1a-205">Açık bir uzantıları kullanmak için `<PackageReference />` projeye eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-205">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=10)]

<span data-ttu-id="b0d1a-206">Bu paket geri yükledikten sonra istemcileri Polly tabanlı işleyicileri ekleme desteklemek genişletme yöntemleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-206">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="b0d1a-207">Geçici hataları işleme</span><span class="sxs-lookup"><span data-stu-id="b0d1a-207">Handle transient faults</span></span>

<span data-ttu-id="b0d1a-208">Dış HTTP çağrıları geçici en yaygın hataları ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-208">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="b0d1a-209">Bir uzantı yöntemi `AddTransientHttpErrorPolicy` olduğundan geçici hataları işlemek için tanımladığı bir ilke sağlayan dahil.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-209">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="b0d1a-210">Bu uzantı metot tanıtıcısı ile yapılandırılmış ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-210">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="b0d1a-211">`AddTransientHttpErrorPolicy` Uzantı içinde kullanılabilir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-211">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b0d1a-212">Uzantı erişim sağlayan bir `PolicyBuilder` olası bir geçici hata temsil eden hataları işlemek için yapılandırılmış nesne:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-212">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="b0d1a-213">Önceki kodda, bir `WaitAndRetryAsync` İlkesi tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-213">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="b0d1a-214">Başarısız istekler en fazla 600 ms denemeler arasındaki gecikme ile üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-214">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="b0d1a-215">Dinamik olarak ilkelerini seçin</span><span class="sxs-lookup"><span data-stu-id="b0d1a-215">Dynamically select policies</span></span>

<span data-ttu-id="b0d1a-216">Polly tabanlı işleyicileri eklemek için kullanılabilecek ek genişletme yöntemleri mevcut.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-216">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="b0d1a-217">Böyle bir uzantısıdır `AddPolicyHandler`, birden çok aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-217">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="b0d1a-218">Aşırı yüklemelerden birine uygulamak için ilkeyi tanımlarken denetlenecek istek sağlar:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-218">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="b0d1a-219">Önceki kodda, giden istek bir GET ise 10 saniyelik zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-219">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="b0d1a-220">Diğer HTTP yöntemi için 30 saniyelik zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-220">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="b0d1a-221">Birden çok Polly işleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="b0d1a-221">Add multiple Polly handlers</span></span>

<span data-ttu-id="b0d1a-222">Polly ilkeleri gelişmiş işlevsellik sağlamak için iç içe yaygın bir uygulamadır:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-222">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="b0d1a-223">Önceki örnekte, iki işleyicisi eklenir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-223">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="b0d1a-224">İlk kullandığı `AddTransientHttpErrorPolicy` bir yeniden deneme ilkesi eklemek için uzantı.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-224">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="b0d1a-225">En fazla üç kez başarısız istek yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-225">Failed requests are retried up to three times.</span></span> <span data-ttu-id="b0d1a-226">İçin yapılan ikinci çağrı `AddTransientHttpErrorPolicy` devre kesici ilke ekler.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-226">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="b0d1a-227">Daha fazla ardışık olarak beş başarısız girişim meydana gelirse dış istekleri 30 saniye engellenir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-227">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="b0d1a-228">Devre kesici ilkeleri bilgisi yok.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-228">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="b0d1a-229">Tüm çağrılar bu istemciyi aynı bağlantı hattı durumu paylaşın.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-229">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="b0d1a-230">Polly kayıt defterinden ilkeleri ekleme</span><span class="sxs-lookup"><span data-stu-id="b0d1a-230">Add policies from the Polly registry</span></span>

<span data-ttu-id="b0d1a-231">Bir kez tanımlayın ve bunları kaydetmek için düzenli olarak kullanılan ilkeleri yönetme yaklaşım, bir `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-231">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="b0d1a-232">Kayıt defterinden bir ilke kullanarak eklenecek bir işleyici olanak tanıyan bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-232">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="b0d1a-233">Önceki kodda, iki ilke kayıtlı olduğunda `PolicyRegistry` eklenir `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-233">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="b0d1a-234">Kayıt defterinden bir ilkeyi kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır, uygulanacak ilke adını geçirerek.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-234">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="b0d1a-235">Daha fazla bilgi hakkında `IHttpClientFactory` ve Polly tümleştirmeler bulunabilir [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="b0d1a-235">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="b0d1a-236">HttpClient ve ömür boyu Yönetimi</span><span class="sxs-lookup"><span data-stu-id="b0d1a-236">HttpClient and lifetime management</span></span>

<span data-ttu-id="b0d1a-237">Yeni bir `HttpClient` örneği her döndürülen `CreateClient` üzerinde çağrılır `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-237">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="b0d1a-238">Var olan bir <xref:System.Net.Http.HttpMessageHandler> adlandırılmış istemci başına.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-238">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="b0d1a-239">Eklentilerin ömrü üretecini yöneten `HttpMessageHandler` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-239">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="b0d1a-240">`IHttpClientFactory` havuzları `HttpMessageHandler` Fabrika kaynak tüketimini azaltmak için oluşturulan örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-240">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="b0d1a-241">Bir `HttpMessageHandler` örneği yeniden kullanılabilir havuzundan yeni bir oluştururken `HttpClient` yaşam süresi dolmadıysa örneği.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-241">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="b0d1a-242">Her işleyicisi genellikle kendi temel alınan HTTP bağlantıları yöneten işleyicileri havuzu tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-242">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="b0d1a-243">Gerekenden daha fazla işleyicileri oluşturma bağlantı gecikmelerine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-243">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="b0d1a-244">Bazı işleyiciler da bağlantıları açık süresiz olarak, DNS değişiklikleri tepki gelen, işleyici engelleyebilir tutun.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-244">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="b0d1a-245">Varsayılan işleyici yaşam süresi iki dakika olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-245">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="b0d1a-246">Varsayılan değer üzerinde geçersiz kılınabilir bir adlandırılmış istemci temelinde.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-246">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="b0d1a-247">Geçersiz kılmak için çağrı <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> üzerinde `IHttpClientBuilder` istemci oluştururken döndürülür:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-247">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="b0d1a-248">İstemci bir şekilde elden gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-248">Disposal of the client isn't required.</span></span> <span data-ttu-id="b0d1a-249">Giden istekleri ve garanti elden iptal verilen `HttpClient` örneği çağırdıktan sonra kullanılamaz <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-249">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="b0d1a-250">`IHttpClientFactory` tarafından kullanılan kaynakları siler ve izler `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-250">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="b0d1a-251">`HttpClient` Örnekleri genellikle kabul elden gerektirmeyen .NET nesneleri olarak.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-251">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="b0d1a-252">Tek bir tutma `HttpClient` örneği uzun bir süre için etkin tutulan bağlantıyı destekliyorsa yeni önce kullanılan yaygın bir düzen olduğunu `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-252">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="b0d1a-253">Bu düzen geçtikten sonra gereksiz olur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-253">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="b0d1a-254">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="b0d1a-254">Logging</span></span>

<span data-ttu-id="b0d1a-255">İstemcileri aracılığıyla oluşturulan `IHttpClientFactory` tüm isteklere ait günlük iletilerini kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-255">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="b0d1a-256">Uygun bilgi düzeyini varsayılan günlük iletilerini görmek için günlüğü yapılandırmanızda etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-256">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="b0d1a-257">İstek üst bilgilerinin günlüğe kaydetme gibi ek günlük kaydı sırasında dahil yalnızca izleme düzeyi.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-257">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="b0d1a-258">Her istemci için kullanılan günlük kategorisi istemci adını içerir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-258">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="b0d1a-259">Adlı bir istemci *MyNamedClient*, örneğin, bir kategorisiyle iletileri günlüğe kaydeder `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-259">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="b0d1a-260">İletileri soneki ile *LogicalHandler* istek işleyicisi ardışık düzenini dışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-260">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="b0d1a-261">Herhangi bir işlem hattı işleyicilerindeki işlenen önce istekte, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-261">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="b0d1a-262">Yanıtta, herhangi bir işlem hattı işleyicileri yanıt aldığınız sonra iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-262">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="b0d1a-263">Günlük kaydı ayrıca istek işleyicisi ardışık düzenini içinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-263">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="b0d1a-264">İçinde *MyNamedClient* örnek, bu iletileri karşı günlük kategorisi kaydedilir `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-264">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="b0d1a-265">İstek için tüm işleyicileri çalıştırdıktan sonra ve istek ağda hemen gönderilmeden önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-265">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="b0d1a-266">Geri işleyici işlem hattı geçirmeden önce yanıtta yanıt durumu bu günlük kaydı içerir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-266">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="b0d1a-267">Günlük kaydı dışında olan ve işlem hattı içindeki etkinleştirilmesi, bir işlem hattı işleyicileri tarafından yapılan değişiklikleri İnceleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-267">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="b0d1a-268">Bu örneğin veya yanıt durum kodu istek üst bilgileri, değişiklik içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-268">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="b0d1a-269">Günlük kategorisinde istemci adı dahil olmak üzere, gerektiğinde belirli adlandırılmış istemciler için filtreleme günlük sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-269">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="b0d1a-270">HttpMessageHandler yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b0d1a-270">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="b0d1a-271">İç'ın yapılandırmasını kontrol gerekebilir `HttpMessageHandler` bir istemci tarafından kullanılmış.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-271">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="b0d1a-272">Bir `IHttpClientBuilder` adlı eklerken, veya yazılan istemciler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-272">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="b0d1a-273"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> Genişletme yöntemi, bir temsilci tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0d1a-273">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="b0d1a-274">Temsilci oluşturmak ve birincil yapılandırmak için kullanılan `HttpMessageHandler` istemci tarafından kullanılan:</span><span class="sxs-lookup"><span data-stu-id="b0d1a-274">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a><span data-ttu-id="b0d1a-275">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0d1a-275">Additional resources</span></span>

* [<span data-ttu-id="b0d1a-276">Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma</span><span class="sxs-lookup"><span data-stu-id="b0d1a-276">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="b0d1a-277">HttpClientFactory ve Polly üstel geri alma ile HTTP çağrı yeniden uygulayın</span><span class="sxs-lookup"><span data-stu-id="b0d1a-277">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="b0d1a-278">Devre Kesici desenini uygulama</span><span class="sxs-lookup"><span data-stu-id="b0d1a-278">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)