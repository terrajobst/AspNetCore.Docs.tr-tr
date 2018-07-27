---
title: HTTP isteklerini başlatma
author: stevejgordon
description: ASP.NET core'da mantıksal HttpClient örneğini yönetmek için IHttpClientFactory arabirimi kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 87424eaea499ba7ece1e5ef88649fcbb2e297635
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320661"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="8d8e6-103">HTTP isteklerini başlatma</span><span class="sxs-lookup"><span data-stu-id="8d8e6-103">Initiate HTTP requests</span></span>

<span data-ttu-id="8d8e6-104">Tarafından [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), ve [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="8d8e6-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="8d8e6-105">Bir [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) kayıtlı ve oluşturma ve yapılandırma için kullanılan [HttpClient](/dotnet/api/system.net.http.httpclient) uygulama örnekleri.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="8d8e6-106">Aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-106">It offers the following benefits:</span></span>

* <span data-ttu-id="8d8e6-107">Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="8d8e6-108">Örneğin, "github" istemci kaydettirilmeli ve GitHub erişim sağlamak için yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="8d8e6-109">Varsayılan istemci diğer amaçlar için kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="8d8e6-110">İşleyicileri temsilci aracılığıyla giden ara yazılım kavramı'ı kodlar `HttpClient` ve, yararlanmak Polly tabanlı ara yazılım için uzantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="8d8e6-111">Havuzu ve arka plandaki, yaşam süresini yöneten `HttpClientMessageHandler` el ile yönetilmesi sırasında oluşan Genel DNS sorunları önlemek için örnekleri `HttpClient` yaşam süresi yok.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="8d8e6-112">Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri aracılığıyla gönderilen tüm istekler için.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="8d8e6-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d8e6-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d8e6-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8d8e6-114">Prerequisites</span></span>

<span data-ttu-id="8d8e6-115">.NET Framework'ü hedefleyen projeleri yüklenmesini gerektiren [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="8d8e6-116">.NET Core ve başvuru hedefleyen projeler [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) zaten dahil `Microsoft.Extensions.Http` paket.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="8d8e6-117">Tüketim modelleri</span><span class="sxs-lookup"><span data-stu-id="8d8e6-117">Consumption patterns</span></span>

<span data-ttu-id="8d8e6-118">Birkaç şekilde `IHttpClientFactory` bir uygulamada kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="8d8e6-119">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="8d8e6-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="8d8e6-120">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="8d8e6-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="8d8e6-121">Türü belirlenmiş istemci</span><span class="sxs-lookup"><span data-stu-id="8d8e6-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="8d8e6-122">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="8d8e6-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="8d8e6-123">Bunların hiçbiri diğerine kesinlikle üst.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="8d8e6-124">En iyi yaklaşım, uygulamanın kısıtlamaları bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="8d8e6-125">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="8d8e6-125">Basic usage</span></span>

<span data-ttu-id="8d8e6-126">`IHttpClientFactory` Çağırarak kayıtlı `AddHttpClient` genişletme yöntemini `IServiceCollection`içine `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="8d8e6-127">Kaydedildikten sonra kod kabul edebilen bir `IHttpClientFactory` Hizmetleri ile her yerde yerleştirilebilir [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı).</span><span class="sxs-lookup"><span data-stu-id="8d8e6-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="8d8e6-128">`IHttpClientFactory` Oluşturmak için kullanılan bir `HttpClient` örneği:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="8d8e6-129">Kullanarak `IHttpClientFactory` bu şekilde var olan bir uygulamayı yeniden düzenleme için harika bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="8d8e6-130">Yolda hiçbir etkisi olmaz `HttpClient` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="8d8e6-131">Yerde nerede `HttpClient` örnekleri şu anda oluşturulur, bu oluşumları çağrısı ile Değiştir `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="8d8e6-132">Adlandırılmış istemciler</span><span class="sxs-lookup"><span data-stu-id="8d8e6-132">Named clients</span></span>

<span data-ttu-id="8d8e6-133">Bir uygulama birden çok farklı kullanımlarını gerektiriyorsa `HttpClient`, her farklı bir yapılandırma ile bir seçenek kullanmaktır **istemcileri adlı**.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-133">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="8d8e6-134">Yapılandırma için bir adlandırılmış `HttpClient` kaydı sırasında belirtilen `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="8d8e6-135">Önceki kodda, `AddHttpClient` , adı "github" sağlama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-135">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="8d8e6-136">Bu istemci bazı varsayılan yapılandırma uygulandı sahip&mdash;taban adresini ve GitHub API ile çalışması için gereken iki üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="8d8e6-137">Her zaman `CreateClient` çağrılır, yeni bir örneğini `HttpClient` oluşturulur ve yapılandırma eylem olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="8d8e6-138">Adlandırılmış bir istemcinin kullanılacağı bir dize parametresi geçirilebilir `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="8d8e6-139">Oluşturulacak istemci adını belirtin:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="8d8e6-140">Önceki kodda, istek bir ana bilgisayar adı belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="8d8e6-141">İstemcisi için yapılandırılan taban adresi kullanıldığından, yol yalnızca geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="8d8e6-142">Türü belirlenmiş istemci</span><span class="sxs-lookup"><span data-stu-id="8d8e6-142">Typed clients</span></span>

<span data-ttu-id="8d8e6-143">Türü belirlenmiş istemci anahtarı olarak dizeleri kullanmak zorunda kalmadan adlandırılmış istemcileri ile aynı özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="8d8e6-144">Türü belirlenmiş istemci yaklaşım, IntelliSense ve derleyici istemciler tüketildiğinde Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="8d8e6-145">Yapılandırma ve belirli bir ile etkileşim kurmak için tek bir konum sağladıkları `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="8d8e6-146">Örneğin, tek bir türü belirlenmiş istemci tek bir arka uç noktası için kullanılabilir ve bu uç nokta tüm mantığı uğraşmanızı kapsüller.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="8d8e6-147">Başka bir avantajı DI, iş ve uygulamanızı gerekli olduğu durumlarda yerleştirilebilir ' dir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="8d8e6-148">Türü belirlenmiş istemci kabul eden bir `HttpClient` oluşturucusuna parametre:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="8d8e6-149">Önceki kodda, türü belirlenmiş istemci yapılandırma taşınır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="8d8e6-150">`HttpClient` Nesne, ortak bir özellik olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="8d8e6-151">Kullanıma sunan bir özel API yöntemleri tanımlamak mümkündür `HttpClient` işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="8d8e6-152">`GetAspNetDocsIssues` Yöntemi en son açık sorunlar bir GitHub deposundan ayrıştırabilir ve sorgulamak için gereken kodu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="8d8e6-153">Türü belirlenmiş bir istemci, genel kaydedilecek `AddHttpClient` genişletme yöntemi içinde kullanılabilir `Startup.ConfigureServices`, türü belirlenmiş istemci sınıfı belirtme:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="8d8e6-154">Türü belirlenmiş istemci DI ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="8d8e6-155">Türü belirlenmiş istemci eklenen ve doğrudan tüketilen:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="8d8e6-156">Kaydı sırasında tercih etmeleri durumunda, türü belirlenmiş istemci yapılandırmasını belirtilebilir `Startup.ConfigureServices`, yerine belirlenmiş istemcinin Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="8d8e6-157">Tamamen yalıtılacak mümkündür `HttpClient` türü belirlenmiş istemci içinde.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="8d8e6-158">Bir özellik olarak gösterme yerine genel yöntemleri arama sağlanabilir `HttpClient` dahili olarak örneği.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8d8e6-159">Önceki kodda, `HttpClient` özel bir alan olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="8d8e6-160">Dış çağrı yapmak için tüm erişim geçtiği `GetRepos` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="8d8e6-161">Oluşturulan istemciler</span><span class="sxs-lookup"><span data-stu-id="8d8e6-161">Generated clients</span></span>

<span data-ttu-id="8d8e6-162">`IHttpClientFactory` diğer üçüncü taraf kitaplıklar ile birlikte gibi kullanılabilir [yeniden donatılması](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="8d8e6-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="8d8e6-163">Refit, .NET için bir REST kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="8d8e6-164">Bu REST API Canlı arabirimleri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="8d8e6-165">Arabiriminin göre dinamik olarak oluşturulan `RestService`kullanarak `HttpClient` dış HTTP çağrısı.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="8d8e6-166">Dış API ve yanıtını temsil etmek için bir arabirim ve bir yanıt tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="8d8e6-167">Uygulama oluşturmak için Refit kullanarak bir türü belirlenmiş istemci eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="8d8e6-168">Tanımlanan arabirimi gerektiğinde DI ve Refit tarafından sağlanan uygulama ile kullanılabilecek:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="8d8e6-169">Giden istek ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="8d8e6-169">Outgoing request middleware</span></span>

<span data-ttu-id="8d8e6-170">`HttpClient` Giden HTTP istekleri için birbirine işleyicileri temsilci olarak görevlendirme kavramı zaten sahip.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="8d8e6-171">`IHttpClientFactory` Adlandırılmış her istemci için uygulanacak işleyicilerini tanımlamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="8d8e6-172">Bu, kayıt ve giden bir istek ara yazılım ardışık düzenini oluşturmak için birden çok işleyicileri zincirleme destekler.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="8d8e6-173">Bu işleyiciler, her giden istekten önce ve sonra çalışma gerçekleştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="8d8e6-174">Bu düzen, ASP.NET Core gelen ara yazılım ardışık benzerdir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="8d8e6-175">Desen etrafında HTTP isteklerini, önbelleğe alma, hata, seri hale getirme, işleme ve günlüğe kaydetme gibi çapraz kesme konuları yönetmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="8d8e6-176">Bir işleyici oluşturmak için türetilen bir sınıf tanımlama `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="8d8e6-177">Geçersiz kılma `SendAsync` yöntemi istek ardışık düzende sonraki işleyici geçirmeden önce kodu çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="8d8e6-178">Yukarıdaki kod, bir temel işleyicisini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="8d8e6-179">Bir X-API-KEY üst bilgi isteği dahil edilmiş görmek için denetler.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-179">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="8d8e6-180">Üst bilgisi eksik, bu HTTP çağrısı kaçınmak ve uygun bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="8d8e6-181">Kayıt sırasında bir veya daha fazla işleyicileri için yapılandırmasına eklenebilir bir `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="8d8e6-182">Bu görev üzerinde genişletme yöntemleri gerçekleştirilir `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-182">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="8d8e6-183">Önceki kodda, `ValidateHeaderHandler` DI ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="8d8e6-184">İşleyici **gerekir** DI geçici olarak kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="8d8e6-185">Bir kez kayıtlı `AddHttpMessageHandler` türü için işleyici geçirme çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-185">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="8d8e6-186">Birden fazla işleyici sırayla yürütülmesi gerektiğini kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="8d8e6-187">Her işleyici son kadar bir sonraki işleyici sarmalar `HttpClientHandler` isteği yürütür:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="8d8e6-188">Polly tabanlı işleyicileri kullanın</span><span class="sxs-lookup"><span data-stu-id="8d8e6-188">Use Polly-based handlers</span></span>

<span data-ttu-id="8d8e6-189">`IHttpClientFactory` adlı bir popüler üçüncü taraf kitaplığı ile tümleştirilir [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="8d8e6-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="8d8e6-190">Polly kapsamlı esnekliği ve .NET için geçici hata işleme Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="8d8e6-191">Bu hizmet sayesinde geliştiriciler, yeniden deneme, devre kesici, zaman aşımı, bölme perdesi yalıtım ve geri dönüş gibi ilkeleri fluent ve iş parçacığı açısından güvenli bir şekilde ifade etmek.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="8d8e6-192">Genişletme yöntemleri, Polly ilkeleriyle kullanımını etkinleştirmek için yapılandırılmış sağlanan `HttpClient` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="8d8e6-193">Polly uzantıları kullanılabilir [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="8d8e6-194">Bu paket bulunup bulunmadığına [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8d8e6-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8d8e6-195">Açık bir uzantıları kullanmak için `<PackageReference />` projeye eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="8d8e6-196">Bu paket geri yükledikten sonra istemcileri Polly tabanlı işleyicileri ekleme desteklemek genişletme yöntemleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="8d8e6-197">Geçici hataları işleme</span><span class="sxs-lookup"><span data-stu-id="8d8e6-197">Handle transient faults</span></span>

<span data-ttu-id="8d8e6-198">Dış HTTP çağrıları yapma meydana gelmesini beklediğiniz en yaygın hataların geçici olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-198">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="8d8e6-199">Bir uzantı yöntemi `AddTransientHttpErrorPolicy` olduğundan geçici hataları işlemek için tanımladığı bir ilke sağlayan dahil.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="8d8e6-200">Bu uzantı metot tanıtıcısı ile yapılandırılmış ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="8d8e6-201">`AddTransientHttpErrorPolicy` Uzantı içinde kullanılabilir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8d8e6-202">Uzantı erişim sağlayan bir `PolicyBuilder` olası bir geçici hata temsil eden hataları işlemek için yapılandırılmış nesne:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="8d8e6-203">Önceki kodda, bir `WaitAndRetryAsync` İlkesi tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="8d8e6-204">Başarısız istekler en fazla 600 ms denemeler arasındaki gecikme ile üç kez yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="8d8e6-205">Dinamik olarak ilkelerini seçin</span><span class="sxs-lookup"><span data-stu-id="8d8e6-205">Dynamically select policies</span></span>

<span data-ttu-id="8d8e6-206">Polly tabanlı işleyicileri eklemek için kullanılabilecek ek genişletme yöntemleri mevcut.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="8d8e6-207">Böyle bir uzantısıdır `AddPolicyHandler`, birden çok aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="8d8e6-208">Aşırı yüklemelerden birine uygulamak için ilkeyi tanımlarken denetlenecek istek sağlar:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="8d8e6-209">Önceki kodda, giden istek bir GET ise 10 saniyelik zaman aşımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="8d8e6-210">Diğer HTTP yöntemi için 30 saniyelik zaman aşımı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="8d8e6-211">Birden çok Polly işleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="8d8e6-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="8d8e6-212">Polly ilkeleri gelişmiş işlevsellik sağlamak için iç içe yaygın bir uygulamadır:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="8d8e6-213">Önceki örnekte, iki işleyicisi eklenir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="8d8e6-214">İlk kullandığı `AddTransientHttpErrorPolicy` bir yeniden deneme ilkesi eklemek için uzantı.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="8d8e6-215">En fazla üç kez başarısız istek yeniden denenir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="8d8e6-216">İçin yapılan ikinci çağrı `AddTransientHttpErrorPolicy` devre kesici ilke ekler.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="8d8e6-217">Daha fazla ardışık olarak beş başarısız girişim meydana gelirse dış istekleri 30 saniye engellenir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="8d8e6-218">Devre kesici ilkeleri bilgisi yok.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="8d8e6-219">Tüm çağrılar bu istemciyi aynı bağlantı hattı durumu paylaşın.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="8d8e6-220">Polly kayıt defterinden ilkeleri ekleme</span><span class="sxs-lookup"><span data-stu-id="8d8e6-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="8d8e6-221">Bir kez tanımlayın ve bunları kaydetmek için düzenli olarak kullanılan ilkeleri yönetme yaklaşım, bir `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="8d8e6-222">Kayıt defterinden bir ilke kullanarak eklenecek bir işleyici olanak tanıyan bir genişletme yöntemi sağlanır:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="8d8e6-223">Önceki kodda, bir PolicyRegistry eklenen `ServiceCollection` ve iki ilke ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-223">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="8d8e6-224">Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır, uygulanacak ilke adını geçirerek.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-224">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="8d8e6-225">Daha fazla bilgi hakkında `IHttpClientFactory` ve Polly tümleştirmeler bulunabilir [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="8d8e6-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="8d8e6-226">HttpClient ve ömür boyu Yönetimi</span><span class="sxs-lookup"><span data-stu-id="8d8e6-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="8d8e6-227">Her zaman `CreateClient` üzerinde çağrılır `IHttpClientFactory`, yeni bir örneğini bir `HttpClient` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-227">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="8d8e6-228">Olacaktır bir `HttpMessageHandler` adlandırılmış istemci başına.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-228">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="8d8e6-229">`IHttpClientFactory` havuzunun `HttpMessageHandler` Fabrika kaynak tüketimini azaltmak için oluşturulan örnekleri.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-229">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="8d8e6-230">A `HttpMessageHandler` örneği yeniden kullanılabilir havuzundan yeni bir oluştururken `HttpClient` yaşam süresi dolmadıysa örneği.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-230">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="8d8e6-231">Her işleyicisi genellikle kendi temel alınan HTTP bağlantıları yöneten işleyicileri havuzu tercih edilir; gerekenden daha fazla işleyicileri oluşturma bağlantı gecikmelerine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="8d8e6-232">Bazı işleyiciler da bağlantıları açık süresiz olarak, DNS değişiklikleri tepki gelen, işleyici engelleyebilir tutun.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-232">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="8d8e6-233">Varsayılan işleyici yaşam süresi iki dakika olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-233">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="8d8e6-234">Varsayılan değer üzerinde geçersiz kılınabilir bir adlandırılmış istemci temelinde.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-234">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="8d8e6-235">Geçersiz kılmak için çağrı `SetHandlerLifetime` üzerinde `IHttpClientBuilder` istemci oluştururken döndürülür:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-235">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="8d8e6-236">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="8d8e6-236">Logging</span></span>

<span data-ttu-id="8d8e6-237">İstemcileri aracılığıyla oluşturulan `IHttpClientFactory` tüm isteklere ait günlük iletilerini kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-237">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="8d8e6-238">Varsayılan günlük iletilerini görmek için günlüğü yapılandırmanızda uygun bilgi düzeyi sağlamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-238">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="8d8e6-239">İstek üst bilgilerinin günlüğe kaydetme gibi ek günlük kaydı sırasında dahil yalnızca izleme düzeyi.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-239">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="8d8e6-240">Her istemci için kullanılan günlük kategorisi istemci adını içerir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-240">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="8d8e6-241">Örneğin, "MyNamedClient" adlı bir istemci ile kategorisi iletileri günlüğe kaydeder `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-241">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="8d8e6-242">İletileri "LogicalHandler" soneki ile istek işleyicisi işlem hattının dış oluşur.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-242">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="8d8e6-243">Herhangi bir işlem hattı işleyicilerindeki işlenen önce istekte, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-243">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="8d8e6-244">Yanıtta, herhangi bir işlem hattı işleyicileri yanıt aldığınız sonra iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-244">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="8d8e6-245">Günlüğe kaydetme, istek işleyicisi Ardışık düzenin iç de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-245">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="8d8e6-246">Karşı günlük kategorisi "MyNamedClient" örnek söz konusu olduğunda, bu iletileri günlüğe kaydedilen `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-246">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="8d8e6-247">İstek için tüm işleyicileri çalıştırdıktan sonra ve istek ağda hemen gönderilmeden önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-247">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="8d8e6-248">Geri işleyici işlem hattı geçirmeden önce yanıtta yanıt durumu bu günlük kaydı içerir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-248">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="8d8e6-249">Dış ve işlem hattı içinde günlük kaydını etkinleştirme, bir işlem hattı işleyicileri tarafından yapılan değişiklikleri incelemesini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-249">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="8d8e6-250">Bu örneğin veya yanıt durum kodu istek üst bilgileri, değişiklik içerebilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-250">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="8d8e6-251">Günlük kategorisinde istemci adı dahil olmak üzere, gerektiğinde belirli adlandırılmış istemciler için filtreleme günlük sağlar.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-251">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="8d8e6-252">HttpMessageHandler yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8d8e6-252">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="8d8e6-253">İç'ın yapılandırmasını kontrol gerekebilir `HttpMessageHandler` bir istemci tarafından kullanılmış.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-253">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="8d8e6-254">Bir `IHttpClientBuilder` adlı eklerken, veya yazılan istemciler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-254">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="8d8e6-255">`ConfigurePrimaryHttpMessageHandler` Genişletme yöntemi, bir temsilci tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8d8e6-255">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="8d8e6-256">Temsilci oluşturmak ve birincil yapılandırmak için kullanılan `HttpMessageHandler` istemci tarafından kullanılan:</span><span class="sxs-lookup"><span data-stu-id="8d8e6-256">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
