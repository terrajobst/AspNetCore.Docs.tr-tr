---
title: Başlatma HTTP istekleri
author: stevejgordon
description: ASP.NET Core mantıksal HttpClient örnekleri yönetmek için IHttpClientFactory arabirimi kullanma hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 1f2c7522a10220cd9520d78846d2e897115447c2
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="initiate-http-requests"></a><span data-ttu-id="244cb-103">Başlatma HTTP istekleri</span><span class="sxs-lookup"><span data-stu-id="244cb-103">Initiate HTTP requests</span></span>

<span data-ttu-id="244cb-104">Tarafından [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="244cb-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="244cb-105">Bir `IHttpClientFactory` kayıtlı ve yapılandırmak ve oluşturmak için kullanılan [HttpClient](/dotnet/api/system.net.http.httpclient) bir uygulama örneği.</span><span class="sxs-lookup"><span data-stu-id="244cb-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="244cb-106">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="244cb-106">It offers the following benefits:</span></span>

* <span data-ttu-id="244cb-107">Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="244cb-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="244cb-108">Örneğin, "github" istemci kayıtlı ve değiştirebilirsiniz GitHub erişmek için yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="244cb-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="244cb-109">Varsayılan istemci diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="244cb-110">İşleyiciler için temsilci seçme aracılığıyla giden Ara kavramı kod oluşturur `HttpClient` ve, yararlanmak Polly tabanlı ara yazılımı için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="244cb-111">Havuz ve arka plandaki, yaşam süresi yönetir `HttpClientMessageHandler` el ile yönetirken oluşan Genel DNS sorunlarını önlemek için örnekler `HttpClient` yaşam süresi yok.</span><span class="sxs-lookup"><span data-stu-id="244cb-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="244cb-112">Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri üzerinden gönderilen tüm istekler için.</span><span class="sxs-lookup"><span data-stu-id="244cb-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="244cb-113">Tüketim desenleri</span><span class="sxs-lookup"><span data-stu-id="244cb-113">Consumption patterns</span></span>

<span data-ttu-id="244cb-114">Birkaç yolu vardır `IHttpClientFactory` bir uygulamada kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="244cb-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="244cb-115">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="244cb-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="244cb-116">Adlandırılmış istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="244cb-117">Yazılan istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="244cb-118">Oluşturulan istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="244cb-119">Bunların hiçbiri diğerine kesinlikle üstün.</span><span class="sxs-lookup"><span data-stu-id="244cb-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="244cb-120">En iyi yaklaşımı uygulamanın kısıtlamaları bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="244cb-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="244cb-121">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="244cb-121">Basic usage</span></span>

<span data-ttu-id="244cb-122">`IHttpClientFactory` Çağırarak kayıtlı `AddHttpClient` genişletme yöntemi `IServiceCollection`içine `ConfigureServices` haline yöntemi.</span><span class="sxs-lookup"><span data-stu-id="244cb-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="244cb-123">Kayıttan sonra kod kabul edebileceği bir `IHttpClientFactory` Hizmetleri ile herhangi bir yere yerleştirilebilir [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı).</span><span class="sxs-lookup"><span data-stu-id="244cb-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="244cb-124">`IHttpClientFactory` Oluşturmak için kullanılan bir `HttpClient` örneği:</span><span class="sxs-lookup"><span data-stu-id="244cb-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="244cb-125">Kullanarak `IHttpClientFactory` bu şekilde mevcut bir uygulamayı yeniden düzenlemeniz için harika bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="244cb-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="244cb-126">Yolu üzerinde hiçbir etkisi olmaz `HttpClient` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="244cb-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="244cb-127">Yerlerde nerede `HttpClient` örnekleri şu anda oluşturulur, bu oluşumları çağrısıyla yerine `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="244cb-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="244cb-128">Adlandırılmış istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-128">Named clients</span></span>

<span data-ttu-id="244cb-129">Bir uygulama birden çok farklı kullanımlarını gerektiriyorsa `HttpClient`, her farklı bir yapılandırma ile kullanmak için bir seçenek olan **istemciler adında**.</span><span class="sxs-lookup"><span data-stu-id="244cb-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="244cb-130">Yapılandırma için bir adlandırılmış `HttpClient` kaydı sırasında belirtilen `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="244cb-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="244cb-131">Önceki kod `AddHttpClient` , adı "github" sağlama denir.</span><span class="sxs-lookup"><span data-stu-id="244cb-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="244cb-132">Bu istemci uygulanan bazı varsayılan yapılandırmaya sahip&mdash;taban adresini ve GitHub API ile çalışmak için gereken iki üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="244cb-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="244cb-133">Her zaman `CreateClient` çağrılır, yeni bir örneğini `HttpClient` oluşturulur ve yapılandırma eylem olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="244cb-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="244cb-134">Adlandırılmış bir istemci kullanmak için bir dize parametresi için geçirilebilir `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="244cb-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="244cb-135">Oluşturulacak istemci adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="244cb-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="244cb-136">Önceki kod, istek bir ana bilgisayar adı belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="244cb-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="244cb-137">Bu istemci için yapılandırılan taban adresi kullanıldığından yolu yalnızca geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="244cb-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="244cb-138">Yazılan istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-138">Typed clients</span></span>

<span data-ttu-id="244cb-139">Yazılan istemcileri dizeleri anahtar olarak kullanmak zorunda kalmadan adlandırılmış istemcileri olarak aynı yetenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="244cb-140">Türü belirlenmiş istemci yaklaşım IntelliSense ve derleyici istemcileri kullanırken Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="244cb-141">Yapılandırma ve belirli bir ile etkileşim kurmak için tek bir konum sağlarlar `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="244cb-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="244cb-142">Örneğin, tek bir türü belirlenmiş istemci tek arka uç noktası için kullanılabilir ve bu uç tüm mantığı postalarla kapsüller.</span><span class="sxs-lookup"><span data-stu-id="244cb-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="244cb-143">Başka bir avantajı dı ile çalışır ve uygulamanızda gerektiğinde yerleştirilebilir sağlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="244cb-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="244cb-144">Türü belirlenmiş istemci kabul bir `HttpClient` kurucusu parametresinde:</span><span class="sxs-lookup"><span data-stu-id="244cb-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="244cb-145">Önceki kodda yapılandırma türü belirlenmiş istemci taşınır.</span><span class="sxs-lookup"><span data-stu-id="244cb-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="244cb-146">`HttpClient` Nesnesi ortak bir özellik olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="244cb-147">Kullanıma API özgü yöntemleri tanımlamak mümkündür `HttpClient` işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="244cb-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="244cb-148">`GetAspNetDocsIssues` Yöntemi sorgulamak ve en son açık sorunlar Github'da depodan dışarı ayrıştırmak için gereken kod yalıtır.</span><span class="sxs-lookup"><span data-stu-id="244cb-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="244cb-149">Türü belirlenmiş istemci, genel kaydetmek için `AddHttpClient` genişletme yöntemi, içinde kullanılabilir `ConfigureServices`, yazılan istemci sınıfı belirtme:</span><span class="sxs-lookup"><span data-stu-id="244cb-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="244cb-150">Türü belirlenmiş istemci dı ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="244cb-151">Türü belirlenmiş istemci eklenen ve doğrudan tüketilen:</span><span class="sxs-lookup"><span data-stu-id="244cb-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="244cb-152">Kaydı sırasında tercih edilen olduğunda yazılan istemci yapılandırması belirtilebilir `ConfigureServices`, yerine yazılan istemcinin Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="244cb-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="244cb-153">Tamamen kapsülleyen mümkündür `HttpClient` türü belirlenmiş istemci içinde.</span><span class="sxs-lookup"><span data-stu-id="244cb-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="244cb-154">Bir özellik olarak gösterme yerine genel yöntemler hangi çağrısı sağlanabilir `HttpClient` dahili örneği.</span><span class="sxs-lookup"><span data-stu-id="244cb-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="244cb-155">Önceki kod `HttpClient` özel bir alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="244cb-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="244cb-156">Dış çağrı yapmak için tüm erişim geçtiği `GetRepos` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="244cb-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="244cb-157">Oluşturulan istemcileri</span><span class="sxs-lookup"><span data-stu-id="244cb-157">Generated clients</span></span>

<span data-ttu-id="244cb-158">`IHttpClientFactory` diğer üçüncü taraf kitaplıkları ile birlikte gibi kullanılabilir [yeniden donatılması](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="244cb-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="244cb-159">Refit, .NET için bir REST kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="244cb-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="244cb-160">REST API'leri Canlı arabirimleri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="244cb-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="244cb-161">Arabirim uygulaması göre dinamik olarak üretilen `RestService`kullanarak `HttpClient` dış HTTP kurmayı çağırır.</span><span class="sxs-lookup"><span data-stu-id="244cb-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="244cb-162">Harici API ve yanıt temsil etmek için bir arabirim ve yanıt tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="244cb-162">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="244cb-163">Uygulama oluşturmak için Refit kullanılarak yazılan istemci eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="244cb-163">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="244cb-164">Tanımlanan arabirimi dı ve Refit tarafından sağlanan uygulama gerektiğinde tüketilebilir:</span><span class="sxs-lookup"><span data-stu-id="244cb-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="244cb-165">Giden istek Ara</span><span class="sxs-lookup"><span data-stu-id="244cb-165">Outgoing request middleware</span></span>

<span data-ttu-id="244cb-166">`HttpClient` zaten giden HTTP istekleri için birbirine işleyicileri için temsilci seçme kavramı vardır.</span><span class="sxs-lookup"><span data-stu-id="244cb-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="244cb-167">`IHttpClientFactory` Adlandırılmış her istemci için uygulamak için işleyiciler tanımlayın kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="244cb-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="244cb-168">Kaydı ve bir giden talep ara yazılım ardışık düzenini oluşturmak için birden çok işleyicileri zincirleme destekler.</span><span class="sxs-lookup"><span data-stu-id="244cb-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="244cb-169">Bu işleyiciler her iş önce ve sonra giden istek gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="244cb-170">Bu deseni, ASP.NET Core gelen ara yazılım ardışık benzerdir.</span><span class="sxs-lookup"><span data-stu-id="244cb-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="244cb-171">Desen arası kesme sorunları çevresinde önbelleğe alma, hata, serileştirme, işleme ve günlüğe kaydetme dahil olmak üzere HTTP isteklerini yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="244cb-172">Bir işleyici oluşturmak için bir sınıf türetme tanımlayın `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="244cb-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="244cb-173">Geçersiz kılma `SendAsync` ardışık düzende sonraki işleyicisi için isteği geçirmeden önce kod yürütmek için yöntemi:</span><span class="sxs-lookup"><span data-stu-id="244cb-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="244cb-174">Önceki kod temel işleyicisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="244cb-175">Bir X API anahtarı üstbilgisi isteği dahil edilmiş görmek için denetler.</span><span class="sxs-lookup"><span data-stu-id="244cb-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="244cb-176">Üstbilgi yoksa, bu HTTP çağrısıyla önlemek ve uygun bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="244cb-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="244cb-177">Kayıt sırasında bir veya daha fazla işleyicileri için yapılandırma eklenebilir bir `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="244cb-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="244cb-178">Bu görevin üzerinde genişletme yöntemleri yapıldığını `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="244cb-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="244cb-179">Önceki kod `ValidateHeaderHandler` dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="244cb-180">İşleyici **gerekir** dı geçici olarak kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="244cb-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="244cb-181">Bir kez kayıtlı `AddHttpMessageHandler` bulunan tür işleyicisi geçirme çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="244cb-182">Birden çok işleyici yürütme sırayla kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="244cb-183">Her işleyici sonraki işleyici kadar en son sarmalar `HttpClientHandler` isteği yürütür:</span><span class="sxs-lookup"><span data-stu-id="244cb-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="244cb-184">Polly tabanlı işleyicilerini kullanın</span><span class="sxs-lookup"><span data-stu-id="244cb-184">Use Polly-based handlers</span></span>

<span data-ttu-id="244cb-185">`IHttpClientFactory` adlı popüler bir üçüncü taraf kitaplığı ile tümleşir [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="244cb-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="244cb-186">Polly kapsamlı esnekliği ve .NET için geçici hata işleme Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="244cb-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="244cb-187">Geliştiricilerin fluent ve iş parçacığı açısından güvenli bir şekilde yeniden deneme devre kesici, zaman aşımı, Bulkhead yalıtım ve geri dönüş gibi ilkeleri express olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="244cb-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="244cb-188">Genişletme yöntemleri Polly ilkeleriyle kullanımını etkinleştirmek için yapılandırılmış sağlanan `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="244cb-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="244cb-189">Polly Uzantıları 'Microsoft.Extensions.Http.Polly' adlı bir NuGet paketi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="244cb-190">Bu paketi 'Microsoft.AspNetCore.App' metapackage tarafından varsayılan olarak dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="244cb-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="244cb-191">Uzantıları kullanmak için bir PackageReference açıkça projeye eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="244cb-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="244cb-192">Bu paket geri yükledikten sonra Polly tabanlı istemcilere işleyicileri desteklemek genişletme yöntemleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="244cb-193">Geçici hataları işleme</span><span class="sxs-lookup"><span data-stu-id="244cb-193">Handle transient faults</span></span>

<span data-ttu-id="244cb-194">Dış HTTP çağrıları yapma meydana gelmesini beklediğiniz en yaygın hataları geçici olacaktır.</span><span class="sxs-lookup"><span data-stu-id="244cb-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="244cb-195">Uygun genişletme yöntemi olarak adlandırılan `AddTransientHttpErrorPolicy` olan geçici hataları işlemek için tanımlanmış bir ilke sağlayan dahil.</span><span class="sxs-lookup"><span data-stu-id="244cb-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="244cb-196">Bu uzantı yöntemi tanıtıcıyla yapılandırılmış ilkeler `HttpRequestException`, HTTP 5xx yanıtlar ve HTTP 408 yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="244cb-197">`AddTransientHttpErrorPolicy` Uzantısı içinde kullanılabilir `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="244cb-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="244cb-198">Uzantı erişim sağlayan bir `PolicyBuilder` olası geçici bir hata temsil eden hataları işlemek için yapılandırılmış nesnesi:</span><span class="sxs-lookup"><span data-stu-id="244cb-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="244cb-199">Önceki kod içinde bir `WaitAndRetryAsync` İlkesi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="244cb-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="244cb-200">Başarısız istekler girişimleri arasındaki 600 MS gecikmeyle üç kez kadar denenir.</span><span class="sxs-lookup"><span data-stu-id="244cb-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="244cb-201">Dinamik olarak İlkeleri'ni seçin</span><span class="sxs-lookup"><span data-stu-id="244cb-201">Dynamically select policies</span></span>

<span data-ttu-id="244cb-202">Ek genişletme yöntemleri Polly tabanlı işleyicileri eklemek için kullanılabilecek mevcut.</span><span class="sxs-lookup"><span data-stu-id="244cb-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="244cb-203">Bu tür bir uzantısıdır `AddPolicyHandler`, birden çok aşırı sahiptir.</span><span class="sxs-lookup"><span data-stu-id="244cb-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="244cb-204">Aşırı yüklemesiyle uygulamak için hangi ilkeyi tanımlarken denetlenecek istek sağlar:</span><span class="sxs-lookup"><span data-stu-id="244cb-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="244cb-205">Giden istek bir GET ise önceki kodda 10 saniyelik zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="244cb-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="244cb-206">Diğer HTTP yöntemi için 30 saniyelik zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="244cb-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="244cb-207">Birden çok Polly işleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="244cb-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="244cb-208">Gelişmiş işlevselliği sağlamak için Polly ilkeleri yerleştirmek için ortaktır:</span><span class="sxs-lookup"><span data-stu-id="244cb-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="244cb-209">Önceki örnekte, iki işleyicileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="244cb-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="244cb-210">İlk kullandığı `AddTransientHttpErrorPolicy` bir yeniden deneme ilkesi eklemek için uzantı.</span><span class="sxs-lookup"><span data-stu-id="244cb-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="244cb-211">Başarısız istekler üç kereye kadar yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="244cb-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="244cb-212">İkinci çağrı `AddTransientHttpErrorPolicy` devre kesici ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="244cb-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="244cb-213">Daha fazla beş başarısız girişim sırayla oluşursa dış istekleri 30 saniye engellenir.</span><span class="sxs-lookup"><span data-stu-id="244cb-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="244cb-214">Devre kesici ilkeleri durum bilgisi yok.</span><span class="sxs-lookup"><span data-stu-id="244cb-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="244cb-215">Tüm çağrılar bu istemci aynı devre durumunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="244cb-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="244cb-216">İlkeleri Polly kayıt defterinden ekleme</span><span class="sxs-lookup"><span data-stu-id="244cb-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="244cb-217">Düzenli olarak kullanılan ilkelerini yönetmek için bir kez tanımlayın ve bunları kaydetmek için yaklaşımdır bir `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="244cb-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="244cb-218">Bir genişletme yöntemi, kayıt defterinden bir ilkeyi kullanarak eklemek bir işleyici sağlayan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="244cb-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="244cb-219">Önceki kodda bir PolicyRegistry eklenen `ServiceCollection` ve iki ilke kayıtlı ile.</span><span class="sxs-lookup"><span data-stu-id="244cb-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="244cb-220">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır, uygulanacak ilke adını geçirme.</span><span class="sxs-lookup"><span data-stu-id="244cb-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="244cb-221">Daha fazla bilgi hakkında `IHttpClientFactory` ve Polly tümleştirmeler bulunabilir [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="244cb-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="244cb-222">HttpClient ve ömür boyu Yönetimi</span><span class="sxs-lookup"><span data-stu-id="244cb-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="244cb-223">Her zaman `CreateClient` üzerinde adlı `IHttpClientFactory`, yeni bir örneğini bir `HttpClient` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="244cb-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="244cb-224">Olacaktır bir `HttpMessageHandler` adlandırılmış istemci başına.</span><span class="sxs-lookup"><span data-stu-id="244cb-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="244cb-225">`IHttpClientFactory` havuzunun `HttpMessageHandler` kaynak kullanımını azaltma fabrikası tarafından oluşturulan örnekleri.</span><span class="sxs-lookup"><span data-stu-id="244cb-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="244cb-226">A `HttpMessageHandler` örneği yeniden kullanılabilir havuzundan yeni bir oluştururken `HttpClient` Yaşam Süresi dolmamışsa örneği.</span><span class="sxs-lookup"><span data-stu-id="244cb-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="244cb-227">Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinde işleyicileri havuzu tercih edilir; gerekenden daha fazla işleyicileri oluşturma bağlantı gecikmelere yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="244cb-228">Bazı işleyicileri de bağlantıları açık süresiz olarak, DNS değişiklikleri tepki hangi işleyici engelleyebilirsiniz tutun.</span><span class="sxs-lookup"><span data-stu-id="244cb-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="244cb-229">Varsayılan işleyici yaşam iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="244cb-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="244cb-230">Varsayılan değer üzerinde geçersiz kılınabilir bir adlandırılmış istemci temelinde.</span><span class="sxs-lookup"><span data-stu-id="244cb-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="244cb-231">Geçersiz kılmak için arama `SetHandlerLifetime` üzerinde `IHttpClientBuilder` istemci oluştururken döndürdü:</span><span class="sxs-lookup"><span data-stu-id="244cb-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="244cb-232">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="244cb-232">Logging</span></span>

<span data-ttu-id="244cb-233">İstemciler ile oluşturulan `IHttpClientFactory` tüm istekler için günlük iletilerini kaydedin.</span><span class="sxs-lookup"><span data-stu-id="244cb-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="244cb-234">Varsayılan günlük iletilerini görmek için günlüğü yapılandırmanıza uygun bilgileri düzeyi etkinleştirmek için gerekir.</span><span class="sxs-lookup"><span data-stu-id="244cb-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="244cb-235">İstek üstbilgilerini günlüğe gibi ek günlük adresindeki dahil yalnızca izleme düzeyi.</span><span class="sxs-lookup"><span data-stu-id="244cb-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="244cb-236">Her istemci için kullanılan günlük kategorisi istemcinin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="244cb-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="244cb-237">Örneğin, "MyNamedClient" adlı bir istemci bir kategori iletilerini kaydeder `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="244cb-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="244cb-238">"LogicalHandler" soneki iletileri istek işleyicisi ardışık dışarıda oluşur.</span><span class="sxs-lookup"><span data-stu-id="244cb-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="244cb-239">Herhangi bir işleyicileri ardışık düzeninde, işlenmiş önce istekte, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="244cb-240">Başka bir işlem hattı işleyicileri yanıt aldıktan sonra yanıtta iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="244cb-241">Günlük kaydı ayrıca istek işleyicisi ardışık iç oluşur.</span><span class="sxs-lookup"><span data-stu-id="244cb-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="244cb-242">"MyNamedClient" örnek söz konusu olduğunda, bu iletileri karşı günlük kategori günlüğe kaydedilen `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="244cb-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="244cb-243">İstek için tüm işleyiciler çalıştırdıktan sonra ve istek ağda hemen gönderilmeden önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="244cb-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="244cb-244">Geri işleyici ardışık düzen üzerinden geçirmeden önce yanıtta bu günlüğü yanıt durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="244cb-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="244cb-245">Dış ve ardışık düzen içinde günlüğünü etkinleştirme bir ardışık düzen işleyiciler tarafından yapılan değişiklikleri incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="244cb-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="244cb-246">Bu örneğin veya yanıt durum kodu için istek üst bilgileri, değişiklik içerebilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="244cb-247">İstemci günlük kategorisinde da dahil olmak üzere, özel adlandırılmış istemciler için gerektiğinde filtreleme günlük sağlar.</span><span class="sxs-lookup"><span data-stu-id="244cb-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="244cb-248">HttpMessageHandler yapılandırın</span><span class="sxs-lookup"><span data-stu-id="244cb-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="244cb-249">İç yapılandırmasını denetlemek gerekli olabilir `HttpMessageHandler` bir istemci tarafından kullanılan.</span><span class="sxs-lookup"><span data-stu-id="244cb-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="244cb-250">Bir `IHttpClientBuilder` eklerken veya adlı yazılan istemcileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="244cb-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="244cb-251">`ConfigurePrimaryHttpMessageHandler` Genişletme yöntemi, bir temsilci tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="244cb-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="244cb-252">Temsilciyi oluşturmak ve birincil yapılandırmak için kullanılan `HttpMessageHandler` , istemci tarafından kullanılan:</span><span class="sxs-lookup"><span data-stu-id="244cb-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
