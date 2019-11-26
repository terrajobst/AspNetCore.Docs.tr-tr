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
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>ASP.NET Core 'de ıhttpclientfactory kullanarak HTTP istekleri yapın

::: moniker range=">= aspnetcore-3.0"

[Glenn CONDRON](https://github.com/glennc), [Ryan şimdi ak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Kirk larkabağı](https://github.com/serpent5)

Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir. `IHttpClientFactory` aşağıdaki avantajları sunar:

* , Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar. Örneğin, *GitHub* adlı bir Istemci, [GitHub](https://github.com/)'a erişmek için kaydedilebilir ve yapılandırılabilir. Varsayılan istemci, genel erişim için kaydedilebilir.
* `HttpClient`, işleyiciler için temsilci atama yoluyla giden ara yazılım kavramını daha da artırır. `HttpClient`' de işleyiciler temsilci seçme avantajlarından faydalanmak için, Polya tabanlı bir ara yazılım için uzantılar sağlar.
* Temel alınan `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir. Otomatik yönetim, `HttpClient` yaşam sürelerini el ile yönetirken oluşan ortak DNS (etki alanı adı sistemi) sorunlarını önler.
* Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).

Bu konu sürümündeki örnek kod, HTTP yanıtlarında döndürülen JSON içeriğinin serisini kaldırmak için <xref:System.Text.Json> kullanır. `Json.NET` ve `ReadAsAsync<T>`kullanan örnekler için, bu konunun 2. x sürümünü seçmek üzere sürüm seçiciyi kullanın.

## <a name="consumption-patterns"></a>Tüketim desenleri

Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:

* [Temel kullanım](#basic-usage)
* [Adlandırılmış istemciler](#named-clients)
* [Yazılan istemciler](#typed-clients)
* [Oluşturulan istemciler](#generated-clients)

En iyi yaklaşım, uygulamanın gereksinimlerine bağlı olarak değişir.

### <a name="basic-usage"></a>Temel kullanım

`IHttpClientFactory`, `AddHttpClient`çağırarak kaydedilebilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

`IHttpClientFactory`, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanılarak istenebilir. Aşağıdaki kod, bir `HttpClient` örneği oluşturmak için `IHttpClientFactory` kullanır:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Yukarıdaki örnekte olduğu gibi `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur. `HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur. Mevcut bir uygulamada `HttpClient` örneklerinin oluşturulduğu yerlerde, bu oluşumları <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrılarıyla değiştirin.

### <a name="named-clients"></a>Adlandırılmış istemciler

Adlandırılmış istemciler şu durumlarda iyi bir seçimdir:

* Uygulama birçok farklı `HttpClient`kullanımı gerektirir.
* Birçok `HttpClient`farklı yapılandırmaya sahiptir.

Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

İstemcinin yapılandırıldığı önceki kodda:

* Temel adres `https://api.github.com/`.
* GitHub API 'SI ile çalışmak için iki üst bilgi gereklidir.

#### <a name="createclient"></a>CreateClient

<xref:System.Net.Http.IHttpClientFactory.CreateClient*> her çağrıldığında:

* Yeni bir `HttpClient` örneği oluşturulur.
* Yapılandırma eylemi çağrılır.

Adlandırılmış bir istemci oluşturmak için adını `CreateClient`geçirin:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez. İstemci için yapılandırılan taban adresi kullanıldığından, kod yalnızca yolu geçirebilir.

### <a name="typed-clients"></a>Yazılan istemciler

Yazılan istemciler:

* Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.
* İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.
* Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın. Örneğin, tek bir türü belirtilmiş istemci kullanılabilir:
  * Tek bir arka uç uç noktası için.
  * Uç nokta ile ilgili tüm mantığı kapsüllemek için.
* DI ile birlikte çalışın ve uygulamada gerektiğinde eklenebilir.

Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Önceki kodda:

* Yapılandırma, yazılan istemciye taşınır.
* `HttpClient` nesnesi ortak bir özellik olarak sunulur.

`HttpClient` işlevselliği ortaya çıkaran API 'ye özgü Yöntemler oluşturulabilir. Örneğin, `GetAspNetDocsIssues` yöntemi açık sorunları almak için kodu kapsüller.

Aşağıdaki kod, bir tür istemci sınıfını kaydetmek için `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> çağırır:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Yazılan istemci, DI ile geçici olarak kaydedilir. Yazılan istemci doğrudan eklenebilir ve tüketilebilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Türü belirlenmiş bir istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

`HttpClient`, türü belirlenmiş bir istemci içinde kapsüllenebilir. Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran bir yöntem tanımlayın:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Yukarıdaki kodda `HttpClient` bir özel alanda depolanır. `HttpClient` erişim, genel `GetRepos` yöntemine göre yapılır.

### <a name="generated-clients"></a>Oluşturulan istemciler

`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi üçüncü taraf kitaplıklarla birlikte kullanılabilir. Yeniden sığdırma, .NET için bir REST kitaplığıdır. REST API 'Leri canlı arabirimlere dönüştürür. Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.

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

    services.AddControllers();
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

`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır. `IHttpClientFactory`:

* Her bir adlandırılmış istemci için uygulanacak işleyiciler tanımlamayı basitleştirir.
* Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydedilmesini ve zincirleme kullanımını destekler. Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir. Bu model:

  * ASP.NET Core gelen ara yazılım ardışık düzenine benzerdir.
  * , HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar, örneğin:

    * önbelleğe alma
    * hata işleme
    * serileştirme
    * günlük kaydı

Temsilci seçme işleyicisi oluşturmak için:

* <xref:System.Net.Http.DelegatingHandler>türet.
* <xref:System.Net.Http.DelegatingHandler.SendAsync*>geçersiz kıl. İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütün:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Yukarıdaki kod, `X-API-KEY` üst bilgisinin istekte olup olmadığını denetler. `X-API-KEY` eksikse, <xref:System.Net.HttpStatusCode.BadRequest> döndürülür.

<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>bir `HttpClient` yapılandırmasına birden fazla işleyici eklenebilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir. `IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur. İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olabilir. İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.

Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.

Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir. Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:

* [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties)kullanarak işleyicide veri geçirin.
* Geçerli isteğe erişmek için <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> kullanın.
* Verileri geçirmek için özel bir <xref:System.Threading.AsyncLocal`1> depolama nesnesi oluşturun.

## <a name="use-polly-based-handlers"></a>Polly tabanlı işleyiciler kullanın

`IHttpClientFactory`, üçüncü taraf kitaplığı [Polly](https://github.com/App-vNext/Polly)ile tümleşir. Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır. Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.

Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır. Polly uzantıları, istemcilere Polly tabanlı işleyiciler eklemeyi destekler. Polly, [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini gerektirir.

### <a name="handle-transient-faults"></a>Geçici hataları işle

Hatalar genellikle dış HTTP çağrıları geçici olduğunda oluşur. <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, geçici hataları işlemek için bir ilkenin tanımlanmasını sağlar. `AddTransientHttpErrorPolicy` ile yapılandırılan ilkeler aşağıdaki yanıtları işleyecek şekilde yapılandırılır:

* <xref:System.Net.Http.HttpRequestException>
* HTTP 5xx
* HTTP 408

`AddTransientHttpErrorPolicy`, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır. Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.

### <a name="dynamically-select-policies"></a>Dinamik olarak ilke seçme

Uzantı yöntemleri, örneğin <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>, Polly tabanlı işleyiciler eklemek için sağlanır. Aşağıdaki `AddPolicyHandler` aşırı yüklemesi hangi ilkenin uygulanacağını belirlemek için isteği inceler:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır. Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.

### <a name="add-multiple-polly-handlers"></a>Birden çok Polly işleyici ekleme

Polly ilkeleri iç içe almak yaygın bir şekilde yapılır:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Yukarıdaki örnekte:

* İki işleyici eklenir.
* İlk işleyici, yeniden deneme ilkesi eklemek için <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> kullanır. Başarısız istekler en fazla üç kez yeniden denenir.
* İkinci `AddTransientHttpErrorPolicy` çağrısı bir devre kesici ilkesi ekler. 5 başarısız girişim sıralı olarak gerçekleşirse, daha fazla dış istek 30 saniye boyunca engellenir. Devre kesici ilkeleri durum bilgisi vardır. Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.

### <a name="add-policies-from-the-polly-registry"></a>Polly kayıt defterinden ilke ekleme

Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir.

Aşağıdaki kodda:

* "Normal" ve "uzun" ilkeler eklenmiştir.
* <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>, kayıt defterinden "normal" ve "uzun" ilkeleri ekler.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi için bkz. [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient ve ömür yönetimi

`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür. Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> oluşturulur. Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.

kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar. `HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.

Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir. Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir. Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS (etki alanı adı sistemi) değişikliklerine yeniden davranmasını engelleyebilir.

Varsayılan işleyici ömrü iki dakikadır. Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

`HttpClient` örnekleri genellikle aktiften **çıkarma gerektirmeyen .NET nesneleri olarak** kabul edilebilir. Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar. `IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır.

Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir. Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.

### <a name="alternatives-to-ihttpclientfactory"></a>Ihttpclientfactory alternatifleri

Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:

* `HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.
* Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.

Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.

- Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.
- <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.
- Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.

Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.

- `SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır. Bu paylaşım, yuva azalmasına engel olur.
- `SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.

### <a name="cookies"></a>Özgü

Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur. Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır. Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:

 - Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma
 - `IHttpClientFactory` önleme

Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Günlüğe kaydetme

Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler. Varsayılan günlük iletilerini görmek için günlük yapılandırmasında uygun bilgi düzeyini etkinleştirin. İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.

Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir. Örneğin, *Mynamedclient*adlı bir istemci, "System .net. http. HttpClient" kategorisine sahip iletileri günlüğe kaydeder. **Mynamedclient**. LogicalHandler ". *Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur. İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir. Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.

Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir. *Mynamedclient* örneğinde, bu Iletiler "System .net. http. HttpClient" günlük kategorisiyle günlüğe kaydedilir. **Mynamedclient**. ClientHandler ". İstek için bu, tüm diğer işleyiciler çalıştırıldıktan sonra ve istek gönderilmeden hemen önce gerçekleşir. Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.

İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir. Bu, istek üst bilgilerinde veya yanıt durum kodunda yapılan değişiklikleri içerebilir.

İstemcinin adını log kategorisinde da içermek, belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.

## <a name="configure-the-httpmessagehandler"></a>HttpMessageHandler 'ı yapılandırma

İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.

Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür. <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir. Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Konsol uygulamasında ıhttpclientfactory kullanma

Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:

* [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Aşağıdaki örnekte:

* <xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.
* `MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur. `HttpClient`, bir Web sayfasını almak için kullanılır.
* `Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Devre Kesici desenini uygulama](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)

Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir. Aşağıdaki avantajları sunar:

* , Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar. Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir. Varsayılan istemci, diğer amaçlar için kaydedilebilir.
* `HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.
* `HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.
* Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="consumption-patterns"></a>Tüketim desenleri

Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:

* [Temel kullanım](#basic-usage)
* [Adlandırılmış istemciler](#named-clients)
* [Yazılan istemciler](#typed-clients)
* [Oluşturulan istemciler](#generated-clients)

Hiçbiri diğerinden tamamen üst değildir. En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.

### <a name="basic-usage"></a>Temel kullanım

`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir. `IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur. `HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur. `HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.

### <a name="named-clients"></a>Adlandırılmış istemciler

Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır. Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar. Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.

`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.

Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir. Oluşturulacak istemcinin adını belirtin:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez. İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.

### <a name="typed-clients"></a>Yazılan istemciler

Yazılan istemciler:

* Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.
* İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.
* Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın. Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.
* DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.

Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Önceki kodda, yapılandırma yazılan istemciye taşınır. `HttpClient` nesnesi ortak bir özellik olarak sunulur. `HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür. `GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.

Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Yazılan istemci, DI ile geçici olarak kaydedilir. Yazılan istemci doğrudan eklenebilir ve tüketilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür. Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır. Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.

### <a name="generated-clients"></a>Oluşturulan istemciler

`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir. Yeniden sığdırma, .NET için bir REST kitaplığıdır. REST API 'Leri canlı arabirimlere dönüştürür. Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.

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
        c.BaseAddress = new Uri("https://localhost:5001");
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

`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır. `IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır. Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler. Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir. Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer. Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.

Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın. İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Yukarıdaki kod, temel bir işleyiciyi tanımlar. İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler. Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.

Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir. Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir. `IHttpClientFactory` her işleyici için ayrı bir dı kapsamı oluşturur. İşleyiciler herhangi bir kapsamın hizmetlerine bağlı olarak ücretsizdir. İşleyicilerin bağımlı olduğu hizmetler, işleyicinin elden çıkarılmasıyla kaldırılır.

Kaydedildikten sonra, işleyicinin türü olarak <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> çağrılabilir.

Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir. Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:

* `HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.
* Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.
* Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.

## <a name="use-polly-based-handlers"></a>Polly tabanlı işleyiciler kullanın

`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir. Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır. Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.

Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır. Polly uzantıları:

* İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.
* , [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir. Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.

### <a name="handle-transient-faults"></a>Geçici hataları işle

Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur. `AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır. Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.

`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir. Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır. Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.

### <a name="dynamically-select-policies"></a>Dinamik olarak ilke seçme

Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır. Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`. Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır. Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.

### <a name="add-multiple-polly-handlers"></a>Birden çok Polly işleyici ekleme

Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Yukarıdaki örnekte, iki işleyici eklenmiştir. İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır. Başarısız istekler en fazla üç kez yeniden denenir. `AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler. Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir. Devre kesici ilkeleri durum bilgisi vardır. Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.

### <a name="add-policies-from-the-polly-registry"></a>Polly kayıt defterinden ilke ekleme

Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir. Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir. Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.

`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.

## <a name="httpclient-and-lifetime-management"></a>HttpClient ve ömür yönetimi

`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür. Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır. Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.

kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar. `HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.

Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir. Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir. Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.

Varsayılan işleyici ömrü iki dakikadır. Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir. Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

İstemcinin çıkarılması gerekli değildir. Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar. `IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır. `HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.

Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir. Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.

### <a name="alternatives-to-ihttpclientfactory"></a>Ihttpclientfactory alternatifleri

Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:

* `HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.
* Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.

Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.

- Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.
- <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.
- Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.

Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.

- `SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır. Bu paylaşım, yuva azalmasına engel olur.
- `SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.

### <a name="cookies"></a>Özgü

Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur. Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır. Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:

 - Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma
 - `IHttpClientFactory` önleme

Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Günlüğe kaydetme

Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler. Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin. İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.

Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir. Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder. *Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur. İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir. Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.

Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir. *Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir. İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir. Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.

İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir. Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.

İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.

## <a name="configure-the-httpmessagehandler"></a>HttpMessageHandler 'ı yapılandırma

İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.

Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür. <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir. Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Konsol uygulamasında ıhttpclientfactory kullanma

Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:

* [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Aşağıdaki örnekte:

* <xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.
* `MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur. `HttpClient`, bir Web sayfasını almak için kullanılır.
* `Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Devre Kesici desenini uygulama](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

, [Glenn CONDRON](https://github.com/glennc), [Ryan şimdi e](https://github.com/rynowak)ve [Steve Gordon](https://github.com/stevejgordon)

Bir <xref:System.Net.Http.IHttpClientFactory>, bir uygulamadaki <xref:System.Net.Http.HttpClient> örnekleri yapılandırmak ve oluşturmak için kaydedilebilir ve kullanılabilir. Aşağıdaki avantajları sunar:

* , Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar. Örneğin, *GitHub istemcisi kayıtlı* ve [GitHub](https://github.com/)'a erişebilecek şekilde yapılandırılabilir. Varsayılan istemci, diğer amaçlar için kaydedilebilir.
* `HttpClient` ' de işleyiciler temsilci seçme yoluyla giden ara yazılım kavramını daha da artırır ve bundan faydalanmak için, Polya tabanlı ara yazılım için uzantılar sağlar.
* `HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.
* Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

.NET Framework hedefleyen projeler [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet paketinin yüklenmesini gerektirir. .NET Core ile hedeflenen ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvuran projeler zaten `Microsoft.Extensions.Http` paketini içerir.

## <a name="consumption-patterns"></a>Tüketim desenleri

Bir uygulamada `IHttpClientFactory` çeşitli yollar vardır:

* [Temel kullanım](#basic-usage)
* [Adlandırılmış istemciler](#named-clients)
* [Yazılan istemciler](#typed-clients)
* [Oluşturulan istemciler](#generated-clients)

Hiçbiri diğerinden tamamen üst değildir. En iyi yaklaşım, uygulamanın kısıtlamalarına bağlıdır.

### <a name="basic-usage"></a>Temel kullanım

`IHttpClientFactory`, `Startup.ConfigureServices` yönteminin içindeki `IServiceCollection``AddHttpClient` genişletme yöntemi çağırarak kaydedilebilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Kaydedildikten sonra kod, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile her yerden `IHttpClientFactory` kabul edebilir. `IHttpClientFactory`, bir `HttpClient` örneği oluşturmak için kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Bu biçimde `IHttpClientFactory` kullanmak, mevcut bir uygulamayı yeniden düzenleme için iyi bir yoldur. `HttpClient` kullanılma şekli üzerinde hiçbir etkisi yoktur. `HttpClient` örneklerinin Şu anda oluşturulduğu yerlerde, bu oluşumların <xref:System.Net.Http.IHttpClientFactory.CreateClient*>çağrısı ile değiştirin.

### <a name="named-clients"></a>Adlandırılmış istemciler

Bir uygulama, her biri farklı bir yapılandırmaya sahip `HttpClient`birçok farklı kullanım gerektiriyorsa, **adlandırılmış istemciler**kullanılır. Adlandırılmış bir `HttpClient` yapılandırması, `Startup.ConfigureServices`kayıt sırasında belirtilebilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Yukarıdaki kodda `AddHttpClient`, *GitHub*adlı adı sağlar. Bu istemci, GitHub API 'SI ile çalışmak için gerekli olan temel adres ve iki üst bilgiyle&mdash;uygulanmış olan bazı varsayılan yapılandırmaları içerir.

`CreateClient` her çağrıldığında, yeni bir `HttpClient` örneği oluşturulur ve yapılandırma eylemi çağrılır.

Adlandırılmış bir istemciyi kullanmak için, `CreateClient`bir dize parametresi geçirilebilir. Oluşturulacak istemcinin adını belirtin:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Yukarıdaki kodda, isteğin bir ana bilgisayar adı belirtmesi gerekmez. İstemci için yapılandırılan taban adresi kullanıldığından, bu yalnızca yolu geçirebilir.

### <a name="typed-clients"></a>Yazılan istemciler

Yazılan istemciler:

* Dizeleri anahtar olarak kullanma gereksinimi olmadan, adlandırılmış istemcilerle aynı özellikleri sağlayın.
* İstemcileri tükettiren IntelliSense ve derleyici yardımı sağlar.
* Yapılandırmak ve belirli bir `HttpClient`etkileşimde bulunmak için tek bir konum sağlayın. Örneğin, tek bir arka uç uç noktası için tek bir adet yazılmış istemci kullanılabilir ve bu uç nokta ile ilgili tüm mantığı kapsüllenebilir.
* DI ile birlikte çalışın ve uygulamanızda gerektiğinde eklenebilir.

Türü belirtilmiş istemci, oluşturucusunda bir `HttpClient` parametresi kabul eder:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Önceki kodda, yapılandırma yazılan istemciye taşınır. `HttpClient` nesnesi ortak bir özellik olarak sunulur. `HttpClient` işlevselliği ortaya çıkaran API 'ye özel yöntemler tanımlamak mümkündür. `GetAspNetDocsIssues` yöntemi, GitHub deposundan en son açık sorunları sorgulamak ve ayrıştırmak için gereken kodu kapsüller.

Türü belirtilmiş bir istemciyi kaydetmek için genel <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> uzantısı yöntemi, türü belirlenmiş istemci sınıfını belirterek `Startup.ConfigureServices`içinde kullanılabilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Yazılan istemci, DI ile geçici olarak kaydedilir. Yazılan istemci doğrudan eklenebilir ve tüketilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Tercih edilirse, yazılan istemcinin yapılandırması, türü belirlenmiş istemcinin Oluşturucusu yerine `Startup.ConfigureServices`kayıt sırasında belirtilebilir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Türü belirlenmiş bir istemci içinde tamamen kapsül`HttpClient` lemek mümkündür. Bunu bir özellik olarak göstermek yerine, `HttpClient` örneğini dahili olarak çağıran ortak Yöntemler sunulabilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Yukarıdaki kodda `HttpClient` bir özel alan olarak depolanır. Dış çağrıları yapmak için tüm erişim `GetRepos` yönteminden geçer.

### <a name="generated-clients"></a>Oluşturulan istemciler

`IHttpClientFactory`, [yeniden sığdırma](https://github.com/paulcbetts/refit)gibi diğer üçüncü taraf kitaplıklarla birlikte kullanılabilir. Yeniden sığdırma, .NET için bir REST kitaplığıdır. REST API 'Leri canlı arabirimlere dönüştürür. Bir arabirimin uygulanması, dış HTTP çağrılarını yapmak için `HttpClient` kullanılarak `RestService`tarafından dinamik olarak oluşturulur.

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

`HttpClient`, giden HTTP istekleri için bir araya bağlanabilen işleyicileri temsilci seçme kavramıdır. `IHttpClientFactory`, her bir adlandırılmış istemci için uygulanacak işleyicileri tanımlamanızı kolaylaştırır. Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok işleyicinin kaydını ve zincirlemeyi destekler. Bu işleyicilerin her biri, giden istekten önce ve sonra iş gerçekleştirebilir. Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer. Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.

Bir işleyici oluşturmak için <xref:System.Net.Http.DelegatingHandler>türetilen bir sınıf tanımlayın. İsteği ardışık düzen içindeki bir sonraki işleyiciye geçirmeden önce kodu yürütmek için `SendAsync` yöntemini geçersiz kılın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Yukarıdaki kod, temel bir işleyiciyi tanımlar. İsteğe bağlı bir `X-API-KEY` üst bilgisi olup olmadığını denetler. Üst bilgi eksikse, HTTP çağrısından kaçınabilir ve uygun bir yanıt döndürebilir.

Kayıt sırasında, bir `HttpClient`yapılandırmasına bir veya daha fazla işleyici eklenebilir. Bu görev <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>uzantı yöntemleri aracılığıyla gerçekleştirilir.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

Yukarıdaki kodda `ValidateHeaderHandler` DI ile kaydedilir. İşleyicinin, bir geçici hizmet olarak dı 'ye kayıtlı olması **gerekir** , hiçbir koşulda kapsamı yoktur. İşleyici kapsamlı bir hizmet olarak kayıtlıysa ve işleyicinin bağımlı olduğu tüm hizmetler atılabilir olur:

* İşleyici kapsam dışına geçmeden önce işleyicinin Hizmetleri atılamaz.
* Atılmış işleyici Hizmetleri işleyicinin başarısız olmasına neden olur.

Kaydedildikten sonra, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, işleyici türünü geçirerek çağrılabilir.

Birden çok işleyici, yürütülmesi gereken sırayla kaydedilebilir. Her işleyici, son `HttpClientHandler` isteği çalıştırana kadar sonraki işleyiciyi sarmalar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

İleti işleyicileriyle istek başına durumu paylaşmak için aşağıdaki yaklaşımlardan birini kullanın:

* `HttpRequestMessage.Properties`kullanarak işleyicide veri geçirin.
* Geçerli isteğe erişmek için `IHttpContextAccessor` kullanın.
* Verileri geçirmek için özel bir `AsyncLocal` depolama nesnesi oluşturun.

## <a name="use-polly-based-handlers"></a>Polly tabanlı işleyiciler kullanın

`IHttpClientFactory`, [Polly](https://github.com/App-vNext/Polly)adlı popüler bir üçüncü taraf kitaplıkla tümleştirilir. Polly, .NET için kapsamlı bir esnekliği ve geçici hata işleme kitaplığıdır. Geliştiricilerin yeniden deneme, devre kesici, zaman aşımı, Bulkbaş yalıtımı, akıcı ve iş parçacığı açısından güvenli bir şekilde geri dönüş gibi ilkeler almasına olanak tanır.

Uzantı yöntemleri, yapılandırılmış `HttpClient` örnekleri ile Polly ilkelerin kullanımını etkinleştirmek için sağlanır. Polly uzantıları:

* İstemcilere Polly tabanlı işleyiciler eklemeyi destekler.
* , [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet paketini yükledikten sonra kullanılabilir. Paket, ASP.NET Core paylaşılan çerçevesine dahil değildir.

### <a name="handle-transient-faults"></a>Geçici hataları işle

Yaygın hatalar, dış HTTP çağrıları geçici olduğunda oluşur. `AddTransientHttpErrorPolicy` adlı, bir ilkenin geçici hataları işleyecek şekilde tanımlanmasını sağlayan uygun bir genişletme yöntemi vardır. Bu uzantı yöntemiyle yapılandırılan ilkeler `HttpRequestException`, HTTP 5xx yanıtları ve HTTP 408 yanıtları.

`AddTransientHttpErrorPolicy` uzantısı `Startup.ConfigureServices`içinde kullanılabilir. Uzantı, olası bir geçici hatayı temsil eden hataları işlemek için yapılandırılmış bir `PolicyBuilder` nesnesine erişim sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Yukarıdaki kodda `WaitAndRetryAsync` bir ilke tanımlanmıştır. Başarısız istekler, denemeler arasındaki 600 MS gecikmeyle en fazla üç kez yeniden denenir.

### <a name="dynamically-select-policies"></a>Dinamik olarak ilke seçme

Polly tabanlı işleyiciler eklemek için kullanılabilecek ek uzantı yöntemleri vardır. Bu tür bir uzantı birden çok aşırı yüklemesi olan `AddPolicyHandler`. Bir aşırı yükleme, hangi ilkenin uygulanacağını tanımlarken isteğin incelenebilirliğini sağlar:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Yukarıdaki kodda, giden istek bir HTTP GET ise, 10 saniyelik bir zaman aşımı uygulanır. Diğer HTTP yöntemleri için, 30 saniyelik bir zaman aşımı kullanılır.

### <a name="add-multiple-polly-handlers"></a>Birden çok Polly işleyici ekleme

Gelişmiş işlevsellik sağlamak için çok fazla ilke iç içe geçmiş bir yaygın hale gelir:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Yukarıdaki örnekte, iki işleyici eklenmiştir. İlki, yeniden deneme ilkesi eklemek için `AddTransientHttpErrorPolicy` uzantısını kullanır. Başarısız istekler en fazla üç kez yeniden denenir. `AddTransientHttpErrorPolicy` ikinci çağrısı, devre kesici ilkesi ekler. Beş başarısız girişim sırayla gerçekleşiyorsa, daha fazla dış istek 30 saniye için engellenir. Devre kesici ilkeleri durum bilgisi vardır. Bu istemci aracılığıyla yapılan tüm çağrılar aynı devre durumunu paylaşır.

### <a name="add-policies-from-the-polly-registry"></a>Polly kayıt defterinden ilke ekleme

Düzenli olarak kullanılan ilkeleri yönetmeye yönelik bir yaklaşım, bunları bir kez tanımlamak ve bir `PolicyRegistry`kaydetmektir. Kayıt defterinden bir ilke kullanılarak bir işleyicinin eklenmesine izin veren bir genişletme yöntemi sağlanır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Yukarıdaki kodda, `PolicyRegistry` `ServiceCollection`eklendiğinde iki ilke kaydedilir. Kayıt defterinden bir ilke kullanmak için `AddPolicyHandlerFromRegistry` yöntemi kullanılır ve uygulanacak ilke adı geçer.

`IHttpClientFactory` ve Polly tümleştirmeler hakkında daha fazla bilgi, [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)üzerinde bulunabilir.

## <a name="httpclient-and-lifetime-management"></a>HttpClient ve ömür yönetimi

`IHttpClientFactory``CreateClient` her çağrıldığında yeni bir `HttpClient` örneği döndürülür. Adlandırılmış istemci başına bir <xref:System.Net.Http.HttpMessageHandler> vardır. Fabrika `HttpMessageHandler` örneklerinin yaşam sürelerini yönetir.

kaynak tüketimini azaltmak için fabrika tarafından oluşturulan `HttpMessageHandler` örnekleri `IHttpClientFactory` havuzlar. `HttpMessageHandler` bir örnek, süresi dolmamışsa yeni bir `HttpClient` örneği oluşturulurken havuzdan yeniden kullanılabilir.

Her işleyici genellikle kendi temel HTTP bağlantılarını yönettiğinden, işleyicilerin havuzlaması tercih edilir. Gerekenden daha fazla işleyici oluşturulması bağlantı gecikmeleri oluşmasına neden olabilir. Ayrıca, bazı işleyiciler bağlantıları süresiz olarak açık tutar, bu da işleyicinin DNS değişikliklerine yeniden davranmasını engelleyebilir.

Varsayılan işleyici ömrü iki dakikadır. Varsayılan değer, adlandırılmış istemci temelinde geçersiz kılınabilir. Bunu geçersiz kılmak için, istemci oluştururken döndürülen `IHttpClientBuilder` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

İstemcinin çıkarılması gerekli değildir. Aktiften çıkarma giden istekleri iptal eder ve <xref:System.IDisposable.Dispose*>çağrıldıktan sonra verilen `HttpClient` örneğinin kullanılamaz olmasını sağlar. `IHttpClientFactory`, `HttpClient` örnekleri tarafından kullanılan kaynakları izler ve ortadan kaldırır. `HttpClient` örnekleri genellikle aktiften çıkarma gerektirmeyen .NET nesneleri olarak kabul edilebilir.

Tek bir `HttpClient` örneğinin uzun süre canlı tutulması, `IHttpClientFactory`önünde kullanılmadan önce kullanılan ortak bir modeldir. Bu kalıp `IHttpClientFactory`geçtikten sonra gereksiz hale gelir.

### <a name="alternatives-to-ihttpclientfactory"></a>Ihttpclientfactory alternatifleri

Dı özellikli bir uygulamada `IHttpClientFactory` kullanmak şunları önler:

* `HttpMessageHandler` örnekleri havuza alarak kaynak tükenmesi sorunları.
* Normal örneklerde `HttpMessageHandler` örnekleri geçirerek eski DNS sorunları.

Uzun süreli <xref:System.Net.Http.SocketsHttpHandler> örneği kullanarak önceki sorunları çözmenin alternatif yolları vardır.

- Uygulama başlatıldığında `SocketsHttpHandler` örneğini oluşturun ve uygulamanın ömrü boyunca kullanın.
- <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS yenileme zamanına göre uygun bir değere yapılandırın.
- Gerektiğinde `new HttpClient(handler, dispostHandler: false)` kullanarak `HttpClient` örnekleri oluşturun.

Yukarıdaki yaklaşımlar `IHttpClientFactory` benzer bir şekilde çözdüğü kaynak yönetimi sorunlarını çözer.

- `SocketsHttpHandler`, `HttpClient` örnekleri arasında bağlantıları paylaşır. Bu paylaşım, yuva azalmasına engel olur.
- `SocketsHttpHandler `, durum DNS sorunlarından kaçınmak için bağlantıları `PooledConnectionLifetime` göre döngüler.

### <a name="cookies"></a>Özgü

Havuza alınmış `HttpMessageHandler` örnekleri, `CookieContainer` nesneleri paylaşılmasına neden olur. Beklenmeyen `CookieContainer` nesne paylaşımı genellikle hatalı kodla sonuçlanır. Tanımlama bilgileri gerektiren uygulamalar için şunlardan birini göz önünde bulundurun:

 - Otomatik tanımlama bilgisi işlemeyi devre dışı bırakma
 - `IHttpClientFactory` önleme

Otomatik tanımlama bilgisi işlemeyi devre dışı bırakmak için <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> çağırın:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Günlüğe kaydetme

Tüm istekler için `IHttpClientFactory` kayıt günlüğü iletileri aracılığıyla oluşturulan istemciler. Varsayılan günlük iletilerini görmek için günlük yapılandırmanızda uygun bilgi düzeyini etkinleştirin. İstek üst bilgilerinin günlüğe kaydedilmesi gibi ek Günlükler yalnızca izleme düzeyinde yer alır.

Her istemci için kullanılan günlük kategorisi, istemcinin adını içerir. Örneğin, *Mynamedclient*adlı bir istemci, iletileri bir `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`kategorisi ile günlüğe kaydeder. *Logicalhandler* ile düzeltilen iletiler istek işleyicisi ardışık düzeni dışında oluşur. İstekte, işlem hattındaki diğer işleyiciler işlenmeden önce iletiler günlüğe kaydedilir. Yanıtta, tüm diğer işlem hattı işleyicileri yanıtı aldıktan sonra iletiler günlüğe kaydedilir.

Günlüğe kaydetme, istek işleyicisi ardışık düzeni içinde de gerçekleşir. *Mynamedclient* örneğinde, bu iletiler `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`günlük kategorisine göre günlüğe kaydedilir. İstek için bu, tüm diğer işleyiciler çalıştıktan sonra ve istek ağda gönderilmeden hemen önce gerçekleşir. Yanıtta, bu günlüğe kaydetme, işleyicinin işleyici işlem hattı üzerinden geri geçirmeden önce yanıtın durumunu içerir.

İşlem hattının dışında ve içinde günlüğe kaydetmenin etkinleştirilmesi, diğer işlem hattı işleyicileri tarafından yapılan değişikliklerin incelemesini etkinleştirir. Bu, örneğin veya yanıt durum kodunda istek başlıklarındaki değişiklikleri içerebilir.

İstemci adı ' nı log kategorisinde da içermek, gerektiğinde belirli adlandırılmış istemciler için günlük filtrelemeyi sunar.

## <a name="configure-the-httpmessagehandler"></a>HttpMessageHandler 'ı yapılandırma

İstemci tarafından kullanılan iç `HttpMessageHandler` yapılandırmasını denetlemek gerekli olabilir.

Adlandırılmış veya yazılan istemciler eklenirken bir `IHttpClientBuilder` döndürülür. <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> uzantısı yöntemi bir temsilciyi tanımlamak için kullanılabilir. Temsilci, bu istemci tarafından kullanılan birincil `HttpMessageHandler` oluşturmak ve yapılandırmak için kullanılır:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Konsol uygulamasında ıhttpclientfactory kullanma

Konsol uygulamasında, aşağıdaki paket başvurularını projeye ekleyin:

* [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Aşağıdaki örnekte:

* <xref:System.Net.Http.IHttpClientFactory>, [genel konağın](xref:fundamentals/host/generic-host) hizmet kapsayıcısına kaydedilir.
* `MyService`, hizmetten bir `HttpClient`oluşturmak için kullanılan bir istemci fabrikası örneği oluşturur. `HttpClient`, bir Web sayfasını almak için kullanılır.
* `Main`, hizmetin `GetPage` yöntemini yürütmek için bir kapsam oluşturur ve Web sayfası içeriğinin ilk 500 karakterini konsola yazar.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Dayanıklı HTTP isteklerini uygulamak için HttpClientFactory kullanma](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [HttpClientFactory ve Polly ilkeleriyle üstel geri alma ile HTTP çağrı yeniden denemeleri uygulayın](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Devre Kesici desenini uygulama](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
