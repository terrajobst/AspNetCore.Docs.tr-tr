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
ms.openlocfilehash: 30ac239a38376feecffc3010387ec5e0009b6db6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="initiate-http-requests"></a>Başlatma HTTP istekleri

Tarafından [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), ve [Steve Gordon](https://github.com/stevejgordon)

[!INCLUDE[](~/includes/2.1.md)]

Bir `IHttpClientFactory` kayıtlı ve yapılandırmak ve oluşturmak için kullanılan [HttpClient](/dotnet/api/system.net.http.httpclient) bir uygulama örneği. Aşağıdaki avantajları sunar:

* Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri. Örneğin, "github" istemci kayıtlı ve değiştirebilirsiniz GitHub erişmek için yapılandırılmış. Varsayılan istemci diğer amaçlar için kaydedilebilir.
* İşleyiciler için temsilci seçme aracılığıyla giden Ara kavramı kod oluşturur `HttpClient` ve, yararlanmak Polly tabanlı ara yazılımı için uzantılar sağlar.
* Havuz ve arka plandaki, yaşam süresi yönetir `HttpClientMessageHandler` el ile yönetirken oluşan Genel DNS sorunlarını önlemek için örnekler `HttpClient` yaşam süresi yok.
* Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri üzerinden gönderilen tüm istekler için.

## <a name="consumption-patterns"></a>Tüketim desenleri

Birkaç yolu vardır `IHttpClientFactory` bir uygulamada kullanılabilir:

* [Temel kullanım](#basic-usage)
* [Adlandırılmış istemcileri](#named-clients)
* [Yazılan istemcileri](#typed-clients)
* [Oluşturulan istemcileri](#generated-clients)

Bunların hiçbiri diğerine kesinlikle üstün. En iyi yaklaşımı uygulamanın kısıtlamaları bağlıdır.

### <a name="basic-usage"></a>Temel kullanım

`IHttpClientFactory` Çağırarak kayıtlı `AddHttpClient` genişletme yöntemi `IServiceCollection`içine `ConfigureServices` haline yöntemi.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

Kayıttan sonra kod kabul edebileceği bir `IHttpClientFactory` Hizmetleri ile herhangi bir yere yerleştirilebilir [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı). `IHttpClientFactory` Oluşturmak için kullanılan bir `HttpClient` örneği:

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

Kullanarak `IHttpClientFactory` bu şekilde mevcut bir uygulamayı yeniden düzenlemeniz için harika bir yoludur. Yolu üzerinde hiçbir etkisi olmaz `HttpClient` kullanılır. Yerlerde nerede `HttpClient` örnekleri şu anda oluşturulur, bu oluşumları çağrısıyla yerine `CreateClient`.

### <a name="named-clients"></a>Adlandırılmış istemcileri

Bir uygulama birden çok farklı kullanımlarını gerektiriyorsa `HttpClient`, her farklı bir yapılandırma ile kullanmak için bir seçenek olan **istemciler adında**. Yapılandırma için bir adlandırılmış `HttpClient` kaydı sırasında belirtilen `ConfigureServices`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

Önceki kod `AddHttpClient` , adı "github" sağlama denir. Bu istemci uygulanan bazı varsayılan yapılandırmaya sahip&mdash;taban adresini ve GitHub API ile çalışmak için gereken iki üstbilgi.

Her zaman `CreateClient` çağrılır, yeni bir örneğini `HttpClient` oluşturulur ve yapılandırma eylem olarak adlandırılır.

Adlandırılmış bir istemci kullanmak için bir dize parametresi için geçirilebilir `CreateClient`. Oluşturulacak istemci adını belirtin:

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

Önceki kod, istek bir ana bilgisayar adı belirtmeniz gerekmez. Bu istemci için yapılandırılan taban adresi kullanıldığından yolu yalnızca geçirebilirsiniz.

### <a name="typed-clients"></a>Yazılan istemcileri

Yazılan istemcileri dizeleri anahtar olarak kullanmak zorunda kalmadan adlandırılmış istemcileri olarak aynı yetenekleri sağlar. Türü belirlenmiş istemci yaklaşım IntelliSense ve derleyici istemcileri kullanırken Yardım sağlar. Yapılandırma ve belirli bir ile etkileşim kurmak için tek bir konum sağlarlar `HttpClient`. Örneğin, tek bir türü belirlenmiş istemci tek arka uç noktası için kullanılabilir ve bu uç tüm mantığı postalarla kapsüller. Başka bir avantajı dı ile çalışır ve uygulamanızda gerektiğinde yerleştirilebilir sağlamasıdır.

Türü belirlenmiş istemci kabul bir `HttpClient` kurucusu parametresinde:

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Önceki kodda yapılandırma türü belirlenmiş istemci taşınır. `HttpClient` Nesnesi ortak bir özellik olarak gösterilir. Kullanıma API özgü yöntemleri tanımlamak mümkündür `HttpClient` işlevselliği. `GetLatestDocsIssue` Yöntemi sorgulamak ve en son sorun Github'da depodan dışarı ayrıştırmak için gereken kod yalıtır.

Türü belirlenmiş istemci, genel kaydetmek için `AddHttpClient` genişletme yöntemi, içinde kullanılabilir `ConfigureServices`, yazılan istemci sınıfı belirtme:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

Türü belirlenmiş istemci dı ile geçici olarak kaydedilir. Türü belirlenmiş istemci eklenen ve doğrudan tüketilen:

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Kaydı sırasında tercih edilen olduğunda yazılan istemci yapılandırması belirtilebilir `ConfigureServices`, yerine yazılan istemcinin Oluşturucusu:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

Tamamen kapsülleyen mümkündür `HttpClient` türü belirlenmiş istemci içinde. Bir özellik olarak gösterme yerine genel yöntemler hangi çağrısı sağlanabilir `HttpClient` dahili örneği.

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

Önceki kod `HttpClient` özel bir alan olarak depolanır. Dış çağrı yapmak için tüm erişim geçtiği `GetRepos` yöntemi.

### <a name="generated-clients"></a>Oluşturulan istemcileri

`IHttpClientFactory` diğer üçüncü taraf kitaplıkları ile birlikte gibi kullanılabilir [yeniden donatılması](https://github.com/paulcbetts/refit). Refit, .NET için bir REST kitaplıktır. REST API'leri Canlı arabirimleri dönüştürür. Arabirim uygulaması göre dinamik olarak üretilen `RestService`kullanarak `HttpClient` dış HTTP kurmayı çağırır.

Harici API ve yanıt temsil etmek için bir arabirim ve yanıt tanımlanmıştır:

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

Uygulama oluşturmak için Refit kullanılarak yazılan istemci eklenebilir:

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

Tanımlanan arabirimi dı ve Refit tarafından sağlanan uygulama gerektiğinde tüketilebilir:

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

## <a name="outgoing-request-middleware"></a>Giden istek Ara

`HttpClient` zaten giden HTTP istekleri için birbirine işleyicileri için temsilci seçme kavramı vardır. `IHttpClientFactory` Adlandırılmış her istemci için uygulamak için işleyiciler tanımlayın kolay hale getirir. Kaydı ve bir giden talep ara yazılım ardışık düzenini oluşturmak için birden çok işleyicileri zincirleme destekler. Bu işleyiciler her iş önce ve sonra giden istek gerçekleştirebilir. Bu deseni, ASP.NET Core gelen ara yazılım ardışık benzerdir. Desen arası kesme sorunları çevresinde önbelleğe alma, hata, serileştirme, işleme ve günlüğe kaydetme dahil olmak üzere HTTP isteklerini yönetmek için bir mekanizma sağlar.

Bir işleyici oluşturmak için bir sınıf türetme tanımlayın `DelegatingHandler`. Geçersiz kılma `SendAsync` ardışık düzende sonraki işleyicisi için isteği geçirmeden önce kod yürütmek için yöntemi:

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Önceki kod temel işleyicisini tanımlar. Bir X API anahtarı üstbilgisi isteği dahil edilmiş görmek için denetler. Üstbilgi yoksa, bu HTTP çağrısıyla önlemek ve uygun bir yanıt döndürür.

Kayıt sırasında bir veya daha fazla işleyicileri için yapılandırma eklenebilir bir `HttpClient`. Bu görevin üzerinde genişletme yöntemleri yapıldığını `IHttpClientBuilder`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

Önceki kod `ValidateHeaderHandler` dı ile kaydedilir. İşleyici **gerekir** dı geçici olarak kayıtlı olmalıdır. Bir kez kayıtlı `AddHttpMessageHandler` bulunan tür işleyicisi geçirme çağrılabilir.

Birden çok işleyici yürütme sırayla kaydedilebilir. Her işleyici sonraki işleyici kadar en son sarmalar `HttpClientHandler` isteği yürütür:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Polly tabanlı işleyicilerini kullanın

`IHttpClientFactory` adlı popüler bir üçüncü taraf kitaplığı ile tümleşir [Polly](https://github.com/App-vNext/Polly). Polly kapsamlı esnekliği ve .NET için geçici hata işleme Kitaplığı ' dir. Geliştiricilerin fluent ve iş parçacığı açısından güvenli bir şekilde yeniden deneme devre kesici, zaman aşımı, Bulkhead yalıtım ve geri dönüş gibi ilkeleri express olanak tanır.

Genişletme yöntemleri Polly ilkeleriyle kullanımını etkinleştirmek için yapılandırılmış sağlanan `HttpClient` örnekleri. Polly Uzantıları 'Microsoft.Extensions.Http.Polly' adlı bir NuGet paketi kullanılabilir. Bu paketi 'Microsoft.AspNetCore.App' metapackage tarafından varsayılan olarak dahil edilmez. Uzantıları kullanmak için bir PackageReference açıkça projeye eklenmelidir.

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

Bu paket geri yükledikten sonra Polly tabanlı istemcilere işleyicileri desteklemek genişletme yöntemleri kullanılabilir.

### <a name="handle-transient-faults"></a>Geçici hataları işleme

Dış HTTP çağrıları yapma meydana gelmesini beklediğiniz en yaygın hataları geçici olacaktır. Uygun genişletme yöntemi olarak adlandırılan `AddTransientHttpErrorPolicy` olan geçici hataları işlemek için tanımlanmış bir ilke sağlayan dahil. Bu uzantı yöntemi tanıtıcıyla yapılandırılmış ilkeler `HttpRequestException`, HTTP 5xx yanıtlar ve HTTP 408 yanıtlar.

`AddTransientHttpErrorPolicy` Uzantısı içinde kullanılabilir `ConfigureServices`. Uzantı erişim sağlayan bir `PolicyBuilder` olası geçici bir hata temsil eden hataları işlemek için yapılandırılmış nesnesi:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

Önceki kod içinde bir `WaitAndRetryAsync` İlkesi tanımlanır. Başarısız istekler girişimleri arasındaki 600 MS gecikmeyle üç kez kadar denenir.

### <a name="dynamically-select-policies"></a>Dinamik olarak İlkeleri'ni seçin

Ek genişletme yöntemleri Polly tabanlı işleyicileri eklemek için kullanılabilecek mevcut. Bu tür bir uzantısıdır `AddPolicyHandler`, birden çok aşırı sahiptir. Aşırı yüklemesiyle uygulamak için hangi ilkeyi tanımlarken denetlenecek istek sağlar:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

Giden istek bir GET ise önceki kodda 10 saniyelik zaman aşımı uygulanır. Diğer HTTP yöntemi için 30 saniyelik zaman aşımı kullanılır.

### <a name="add-multiple-polly-handlers"></a>Birden çok Polly işleyicileri ekleme

Gelişmiş işlevselliği sağlamak için Polly ilkeleri yerleştirmek için ortaktır:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

Önceki örnekte, iki işleyicileri eklenir. İlk kullandığı `AddTransientHttpErrorPolicy` bir yeniden deneme ilkesi eklemek için uzantı. Başarısız istekler üç kereye kadar yeniden denenir. İkinci çağrı `AddTransientHttpErrorPolicy` devre kesici ilkesi ekler. Daha fazla beş başarısız girişim sırayla oluşursa dış istekleri 30 saniye engellenir. Devre kesici ilkeleri durum bilgisi yok. Tüm çağrılar bu istemci aynı devre durumunu paylaşır.

### <a name="add-policies-from-the-polly-registry"></a>İlkeleri Polly kayıt defterinden ekleme

Düzenli olarak kullanılan ilkelerini yönetmek için bir kez tanımlayın ve bunları kaydetmek için yaklaşımdır bir `PolicyRegistry`. Bir genişletme yöntemi, kayıt defterinden bir ilkeyi kullanarak eklemek bir işleyici sağlayan sağlanır:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

Önceki kodda bir PolicyRegistry eklenen `ServiceCollection` ve iki ilke kayıtlı ile. Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır, uygulanacak ilke adını geçirme.

Daha fazla bilgi hakkında `IHttpClientFactory` ve Polly tümleştirmeler bulunabilir [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient ve ömür boyu Yönetimi

Her zaman `CreateClient` üzerinde adlı `IHttpClientFactory`, yeni bir örneğini bir `HttpClient` döndürülür. Olacaktır bir `HttpMessageHandler` adlandırılmış istemci başına. `IHttpClientFactory` havuzunun `HttpMessageHandler` kaynak kullanımını azaltma fabrikası tarafından oluşturulan örnekleri. A `HttpMessageHandler` örneği yeniden kullanılabilir havuzundan yeni bir oluştururken `HttpClient` Yaşam Süresi dolmamışsa örneği. 

Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinde işleyicileri havuzu tercih edilir; gerekenden daha fazla işleyicileri oluşturma bağlantı gecikmelere yol açabilir. Bazı işleyicileri de bağlantıları açık süresiz olarak, DNS değişiklikleri tepki hangi işleyici engelleyebilirsiniz tutun.

Varsayılan işleyici yaşam iki dakikadır. Varsayılan değer üzerinde geçersiz kılınabilir bir adlandırılmış istemci temelinde. Geçersiz kılmak için arama `SetHandlerLifetime` üzerinde `IHttpClientBuilder` istemci oluştururken döndürdü:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a>Günlüğe Kaydetme

İstemciler ile oluşturulan `IHttpClientFactory` tüm istekler için günlük iletilerini kaydedin. Varsayılan günlük iletilerini görmek için günlüğü yapılandırmanıza uygun bilgileri düzeyi etkinleştirmek için gerekir. İstek üstbilgilerini günlüğe gibi ek günlük adresindeki dahil yalnızca izleme düzeyi.

Her istemci için kullanılan günlük kategorisi istemcinin adını içerir. Örneğin, "MyNamedClient" adlı bir istemci bir kategori iletilerini kaydeder `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. "LogicalHandler" soneki iletileri istek işleyicisi ardışık dışarıda oluşur. Herhangi bir işleyicileri ardışık düzeninde, işlenmiş önce istekte, iletileri günlüğe kaydedilir. Başka bir işlem hattı işleyicileri yanıt aldıktan sonra yanıtta iletileri günlüğe kaydedilir.

Günlük kaydı ayrıca istek işleyicisi ardışık iç oluşur. "MyNamedClient" örnek söz konusu olduğunda, bu iletileri karşı günlük kategori günlüğe kaydedilen `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. İstek için tüm işleyiciler çalıştırdıktan sonra ve istek ağda hemen gönderilmeden önce gerçekleşir. Geri işleyici ardışık düzen üzerinden geçirmeden önce yanıtta bu günlüğü yanıt durumunu içerir.

Dış ve ardışık düzen içinde günlüğünü etkinleştirme bir ardışık düzen işleyiciler tarafından yapılan değişiklikleri incelemesini etkinleştirir. Bu örneğin veya yanıt durum kodu için istek üst bilgileri, değişiklik içerebilir.

İstemci günlük kategorisinde da dahil olmak üzere, özel adlandırılmış istemciler için gerektiğinde filtreleme günlük sağlar.

## <a name="configure-the-httpmessagehandler"></a>HttpMessageHandler yapılandırın

İç yapılandırmasını denetlemek gerekli olabilir `HttpMessageHandler` bir istemci tarafından kullanılan.

Bir `IHttpClientBuilder` eklerken veya adlı yazılan istemcileri döndürülür. `ConfigurePrimaryHttpMessageHandler` Genişletme yöntemi, bir temsilci tanımlamak için kullanılabilir. Temsilciyi oluşturmak ve birincil yapılandırmak için kullanılan `HttpMessageHandler` , istemci tarafından kullanılan:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
