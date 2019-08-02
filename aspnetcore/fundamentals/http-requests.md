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
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın

, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)

Bir <xref:System.Net.Http.IHttpClientFactory> uygulamadaki örnekleri yapılandırmak ve oluşturmak <xref:System.Net.Http.HttpClient> için kayıt yapılabilir ve kullanılabilir. Aşağıdaki avantajları sunar:

* , Mantıksal `HttpClient` örnekleri adlandırmak ve yapılandırmak için merkezi bir konum sağlar. Örneğin, GitHub istemcisi kayıtlı ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir. Varsayılan istemci, diğer amaçlar için kaydedilebilir.
* ' De `HttpClient` işleyiciler için temsilci atama ile giden ara yazılım kavramı ve bundan faydalanmak için, Polly tabanlı ara yazılım için uzantılar sağlar.
* Yaşam sürelerini el ile yönetirken `HttpClientMessageHandler` `HttpClient` gerçekleşen yaygın DNS sorunlarından kaçınmak için temeldeki örneklerin biriktirmesini ve ömrünü yönetir.
* Fabrika tarafından oluşturulan istemciler aracılığıyla gönderilen tüm `ILogger`istekler için yapılandırılabilir bir günlüğe kaydetme deneyimi ekler (aracılığıyla).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a>Önkoşullar

.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir. .NET Core ile hedeflenen ve [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketi içeriyor.

::: moniker-end

## <a name="consumption-patterns"></a>Tüketim desenleri

Bir uygulamada çeşitli yollar `IHttpClientFactory` kullanılabilir:

* [Temel kullanım](#basic-usage)
* [Adlandırılmış istemciler](#named-clients)
* [Yazılan istemciler](#typed-clients)
* [Oluşturulan istemciler](#generated-clients)

Hiçbiri diğerinden tamamen üst değildir. En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.

### <a name="basic-usage"></a>Temel kullanım

, Yöntemi içindeki `AddHttpClient` içindeki genişletme yöntemi `IServiceCollection`çağırarak kaydedilebilir. `Startup.ConfigureServices` `IHttpClientFactory`

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Kaydedildikten sonra kod, `IHttpClientFactory` [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden bir hizmeti kabul edebilir. `IHttpClientFactory` Bir`HttpClient` örnek oluşturmak için kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Bu `IHttpClientFactory` biçimde kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur. Kullanım şekli `HttpClient` üzerinde hiçbir etkisi yoktur. `HttpClient` Örneklerin Şu anda oluşturulduğu yerlerde, bu tekrarlamaları ' a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrı ile değiştirin.

### <a name="named-clients"></a>Adlandırılmış istemciler

Bir uygulama `HttpClient`, her biri farklı bir yapılandırmaya sahip birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır. Adlandırılmış `HttpClient` için yapılandırma, içinde `Startup.ConfigureServices`kayıt sırasında belirtilebilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Yukarıdaki kodda, `AddHttpClient` *GitHub*adının sağlanması denir. Bu istemci, GitHub API 'siyle birlikte&mdash;çalışmak için gerekli olan temel adres ve iki üst bilgi olan bazı varsayılan yapılandırma uygulanmış.

Her seferinde `CreateClient` her çağrıldığında yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.

Adlandırılmış bir istemciyi kullanmak için, bir dize parametresi iletilebilir `CreateClient`. Oluşturulacak istemcinin adını belirtin:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez. İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.

### <a name="typed-clients"></a>Yazılan istemciler

Yazılan istemciler:

* Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.
* İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.
* Yapılandırmak ve belirli `HttpClient`bir ile etkileşimde bulunmak için tek bir konum belirtin. Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.
* DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.

Türü belirtilmiş istemci, oluşturucusunda `HttpClient` bir parametreyi kabul eder:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Önceki kodda, yapılandırma yazılan istemciye taşınır. `HttpClient` Nesne bir ortak özellik olarak sunulur. İşlevselliği kullanıma `HttpClient` sunan API 'ye özel yöntemler tanımlamak mümkündür. Yöntemi `GetAspNetDocsIssues` , GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu saklar.

Türü belirtilmiş bir istemciyi kaydettirmek için, genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> genişletme yöntemi içinde `Startup.ConfigureServices`kullanılabilir istemci sınıfını belirterek kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Yazılan istemci, DI ile geçici olarak kaydedilir. Yazılan istemci doğrudan eklenebilir ve tüketilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine kayıt `Startup.ConfigureServices`sırasında belirtilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Türü belirtilmiş bir istemci `HttpClient` içinde tamamen kapsüllenebilir. Bunu bir özellik olarak göstermek yerine, `HttpClient` örneği dahili olarak çağıran ortak Yöntemler sunulabilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Önceki kodda `HttpClient` , bir özel alan olarak depolanır. Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.

### <a name="generated-clients"></a>Oluşturulan istemciler

`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarıyla birlikte kullanılabilir. Yeniden sığdırma, .NET için bir REST kitaplığıdır. REST API 'Leri canlı arabirimlere dönüştürür. Arabirim bir uygulama, dış http çağrıları yapmak için kullanılarak `RestService` `HttpClient` tarafından dinamik olarak oluşturulur.

Bir arabirim ve yanıt, dış API 'yi ve yanıtını temsil edecek şekilde tanımlanır:

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

Türü belirlenmiş bir istemci eklenebilir, uygulamayı oluşturmak için yeniden sığdırma kullanımı kullanılabilir:

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

Tanımlı arabirim, gereken yerde, mak ve Refit tarafından sağlanmış uygulama ile kullanılabilir.

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

## <a name="outgoing-request-middleware"></a>Giden istek ara yazılımı

`HttpClient`, giden HTTP istekleri için birlikte bağlanabilen işleyicileri temsilci seçme kavramı zaten var. , `IHttpClientFactory` Her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır. Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler. Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir. Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer. Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.

Bir işleyici oluşturmak için, öğesinden türeten <xref:System.Net.Http.DelegatingHandler>bir sınıf tanımlayın. İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için yöntemigeçersizkılın:`SendAsync`

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Yukarıdaki kod, temel bir işleyiciyi tanımlar. İsteğe bağlı bir `X-API-KEY` başlık olup olmadığını denetler. Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.

Kayıt sırasında bir veya daha fazla işleyici bir `HttpClient`için yapılandırmasına eklenebilir. Bu görev, <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>üzerindeki genişletme yöntemleri aracılığıyla gerçekleştirilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

Yukarıdaki kodda `ValidateHeaderHandler` ,, dı ile kaydedilir. Her işleyici için ayrı bir dı kapsamı oluşturur.`IHttpClientFactory` İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir. İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.

Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> işleyicinin türü geçirerek, çağrılabilir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yukarıdaki kodda `ValidateHeaderHandler` ,, dı ile kaydedilir. İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur. İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:

* İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.
* Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.

Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> işleyici türünü geçirerek çağrılabilir.

::: moniker-end

Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir. Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:

* Kullanarak `HttpRequestMessage.Properties`işleyicide veri geçirin.
* Geçerli `IHttpContextAccessor` isteğe erişmek için kullanın.
* Verileri geçirmek için `AsyncLocal` özel bir depolama nesnesi oluşturun.

## <a name="use-polly-based-handlers"></a>Polly tabanlı işleyiciler kullanın

`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplığı ile tümleşir. Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır. Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.

Uzantı yöntemleri, yapılandırılmış `HttpClient` örneklerle Polly ilkelerin kullanımını etkinleştirmek için sağlanır. Polly uzantıları:

* İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.
* , [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir. Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.

### <a name="handle-transient-faults"></a>Geçici hataları işle

Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur. Geçici hataları işlemek üzere bir `AddTransientHttpErrorPolicy` ilkenin tanımlanmasını sağlayan, çağrılan bir uygun genişletme yöntemi eklenmiştir. Bu uzantı yöntemi tanıtıcısıyla `HttpRequestException`yapılandırılmış ilkeler, http 5xx yanıtları ve http 408 yanıtları.

`AddTransientHttpErrorPolicy` Uzantı içinde`Startup.ConfigureServices`kullanılabilir. Uzantı, olası bir geçici hatayı `PolicyBuilder` temsil eden hataları işlemek için yapılandırılmış bir nesneye erişim sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Yukarıdaki kodda bir `WaitAndRetryAsync` ilke tanımlanmıştır. Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.

### <a name="dynamically-select-policies"></a>Dinamik olarak ilke seçme

Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır. Bu tür bir uzantının `AddPolicyHandler`birden çok aşırı yüklemesi vardır. Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır. Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.

### <a name="add-multiple-polly-handlers"></a>Birden çok Polly işleyici ekleme

Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Yukarıdaki örnekte, iki işleyici eklenmiştir. İlki, yeniden deneme `AddTransientHttpErrorPolicy` ilkesi eklemek için uzantıyı kullanır. Başarısız istekler en fazla üç kez yeniden denenir. İçin `AddTransientHttpErrorPolicy` ikinci çağrı, bir devre kesici ilkesi ekler. Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir. Devre kesici ilkeleri durum bilgisi vardır. Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.

### <a name="add-policies-from-the-polly-registry"></a>Polly kayıt defterinden ilke ekleme

Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlayıp bir `PolicyRegistry`ile kaydetmektir. Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`öğesine eklendiğinde iki ilke kaydedilir. Kayıt defterinden `AddPolicyHandlerFromRegistry` bir ilke kullanmak için yöntemi kullanılır ve uygulanacak ilke adı geçer.

Ve daha fazla `IHttpClientFactory` tümleştirme hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)' de bulunabilir.

## <a name="httpclient-and-lifetime-management"></a>HttpClient ve ömür yönetimi

Her çağrıldığında `HttpClient`Yenibir örnek döndürülür. `IHttpClientFactory` `CreateClient` Adlandırılmış istemci başına <xref:System.Net.Http.HttpMessageHandler> . Fabrika, `HttpMessageHandler` örneklerin yaşam sürelerini yönetir.

`IHttpClientFactory`kaynak tüketimini azaltmak için fabrika tarafından oluşturulan örneklerihavuzlar.`HttpMessageHandler` Bir `HttpMessageHandler` örnek, süresi dolmamışsa yeni `HttpClient` bir örnek oluştururken havuzdan yeniden kullanılabilir.

Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir. Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir. Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.

Varsayılan işleyici ömrü iki dakikadır. Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir. Bunu geçersiz kılmak için, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> istemcisini oluştururken `IHttpClientBuilder` döndürülen öğesini çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

İstemcinin çıkarılması gerekli değildir. Çıkarma giden istekleri iptal eder ve çağırma `HttpClient` <xref:System.IDisposable.Dispose*>sonrasında verilen örneğin kullanılamaz olmasını sağlar. `IHttpClientFactory`örnekler tarafından `HttpClient` kullanılan kaynakları izler ve ortadan kaldırdık. `HttpClient` Örnekler genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.

Tek `HttpClient` bir örneğinin uzun süre canlı tutulması, önünde `IHttpClientFactory`kullanılmadan önce kullanılan ortak bir modeldir. Bu model, ' a geçtikten sonra `IHttpClientFactory`gereksiz hale gelir.

## <a name="logging"></a>Günlüğe Kaydetme

Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler. Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin. İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.

Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir. Örneğin, *Mynamedclient*adlı bir istemci, bir kategorisine `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`sahip iletileri günlüğe kaydeder. *Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur. İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir. Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.

Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir. *Mynamedclient* örneğinde, bu iletiler günlük kategorisine `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`göre günlüğe kaydedilir. İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir. Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.

İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir. Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.

İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.

## <a name="configure-the-httpmessagehandler"></a>HttpMessageHandler 'ı yapılandırma

İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmayı denetlemek gerekli olabilir.

Adlandırılmış `IHttpClientBuilder` veya yazılan istemciler eklenirken bir döndürülür. <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> Genişletme yöntemi bir temsilciyi tanımlamak için kullanılabilir. Temsilci, bu istemci tarafından kullanılan birincili `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Konsol uygulamasında ıhttpclientfactory kullanma

Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:

* [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Aşağıdaki örnekte:

* <xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.
* `MyService`hizmetinden bir `HttpClient`istemci fabrikası örneği oluşturur. `HttpClient`, bir Web sayfasını almak için kullanılır.
* `Main`Hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Devre Kesici desenini uygulama](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
