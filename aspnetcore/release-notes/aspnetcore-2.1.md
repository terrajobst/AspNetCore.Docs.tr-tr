---
title: ASP.NET Core 2.1 yenilikler nelerdir?
author: isaac2004
description: ASP.NET Core 2.1 yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: e16bb874f317b922f3900b540596f6ff38debb2f
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206841"
---
# <a name="whats-new-in-aspnet-core-21"></a>ASP.NET Core 2.1 yenilikler nelerdir?

Bu makalede, ASP.NET Core 2.1 ile ilgili belgelere bağlantılar en önemli değişiklikleri vurgular.

## <a name="signalr"></a>SignalR

SignalR, ASP.NET Core 2.1 için yazılmıştır. ASP.NET Core SignalR geliştirmeleri içerir:

* Basitleştirilmiş bir ölçek genişletme modeli.
* JQuery bağımlılığı olmayan ile yeni bir JavaScript istemci.
* MessagePack üzerinde bağlı olan yeni sıkıştırılmış ikili protokolü.
* Özel protokoller için destek.
* Yeni akış yanıt modeli.
* Üzerinde tam WebSockets tabanlı istemciler için destek.

Daha fazla bilgi için [ASP.NET Core SignalR](xref:signalr/index).

## <a name="razor-class-libraries"></a>Razor sınıf kitaplıkları

ASP.NET Core 2.1 oluşturmak ve Razor tabanlı UI kitaplığa ve birden çok projede paylaşmak kolaylaştırır. Sınıf kitaplığı projesine bir NuGet paketi paketlenen Razor dosyalarıyla yeni Razor SDK'sı sağlar. Görünümlere ve sayfalara kitaplıklarında otomatik olarak bulunur ve uygulama tarafından geçersiz kılınabilir. Razor derleme yapıda tümleştirerek:

* Uygulama başlatma süresini önemli ölçüde daha hızlıdır.
* Razor görünümleri ve çalışma zamanında sayfalarına hızlı güncelleştirmeler hala kullanılabilir bir yinelemeli geliştirme iş akışının parçası olarak.

Daha fazla bilgi için [Razor sınıf kitaplığı projesi kullanarak yeniden kullanılabilir kullanıcı Arabirimi oluşturma](xref:razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Kimlik kullanıcı Arabirimi kitaplığı ve yapı iskelesi

ASP.NET Core 2.1 sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class). Kimlik içeren uygulamaları seçmeli olarak kimlik Razor sınıf kitaplığı (RCL) yer alan kaynak kodu eklemek için yeni kimlik destek uygulayabilirsiniz. Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz. Örneğin, kayıt için kullanılan kod üretmek için iskele kurucu toplamasını. Oluşturulan kod aynı kimlik RCL kodda daha önceliklidir.

Yapan uygulamalar **değil** dahil kimlik doğrulaması kimlik iskele kurucu RCL kimlik paketini eklemek için geçerli olabilir. Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.

Daha fazla bilgi için [İskele kimlik ASP.NET Core projelerinde](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

Güvenlik ve gizlilik hakkında daha fazla odaklanma ile web apps için HTTPS'yi etkinleştirme önemlidir. HTTPS zorlama Web'de giderek katı gelmektedir. HTTPS kullanmayın siteleri güvenli olarak kabul edilir. Web özellikleri güvenli bağlamından kullanılmalıdır zorlamak tarayıcı (Chromium, Mozilla) başlıyor. [GDPR](xref:security/gdpr) kullanıcı gizliliğini korumak için HTTPS kullanılmasını gerektirir. HTTPS kullanarak geliştirme, üretimde HTTPS kullanarak kritik olsa da, (örneğin, güvenli bağlantılar) dağıtım sorunlarını engellemeye yardımcı olabilir. ASP.NET Core 2.1 geliştirme HTTPS kullanmak için ve HTTPS üretimde yapılandırmak için daha kolay geliştirmeleri içerir. Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Üzerinde varsayılan olarak

Güvenli Web sitesi geliştirme kolaylaştırmak için HTTPS artık varsayılan olarak etkindir. Dinlediği Kestrel 2.1 itibaren `https://localhost:5001` yerel geliştirme sertifikası olduğunda mevcut. Bir geliştirme sertifikası oluşturulur:

* SDK'yı ilk kez kullandığınızda, .NET Core SDK'yı ilk kez çalıştırma deneyimi bir parçası olarak.
* El ile kullanarak yeni `dev-certs` aracı.

Çalıştırma `dotnet dev-certs https --trust` sertifikasına güvenmek için.

### <a name="https-redirection-and-enforcement"></a>HTTPS yeniden yönlendirmesi ve zorlama

Web uygulamaları, genellikle hem HTTP hem de HTTPS üzerinde dinleme yapar ancak ardından tüm HTTP trafiğini HTTPS'ye yönlendirmek gerekir. 2.1, akıllı bir şekilde yönlendiren özel HTTPS yeniden yönlendirmesi ara yazılım yapılandırma varlığını temel alarak veya ilişkili sunucusu bağlantı noktaları sunulmuş.

HTTPS kullanımı daha fazla zorunlu kullanarak [HTTP katı Aktarım güvenlik protokolü (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). Her zaman HTTPS üzerinden erişmesini tarayıcılar HSTS bildirir. ASP.NET Core 2.1 en yüksek yaş, alt etki alanlarını, seçeneklerini destekler HSTS ara yazılım ekler ve liste HSTS dağıtılacak.

### <a name="configuration-for-production"></a>Üretim için yapılandırma

Üretim ortamında, HTTPS açıkça yapılandırılmış olması gerekir. 2.1 içinde Kestrel HTTPS yapılandırmak için varsayılan yapılandırma şeması eklendi. Uygulamaları kullanmak için yapılandırılabilir:

* URL'leri dahil olmak üzere birden fazla uç nokta. Daha fazla bilgi için [Kestrel web sunucusu uygulaması: uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).
* HTTPS disk üzerindeki bir dosyadan veya bir sertifika deposu için kullanılacak sertifika.

## <a name="gdpr"></a>GDPR

ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri. Daha fazla bilgi için [GDPR desteği de ASP.NET Core](xref:security/gdpr). A [örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) nasıl kullanılacağını gösterir ve GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonlarına eklenen API'leri çoğunu sınamanızı sağlar.

## <a name="integration-tests"></a>Tümleştirme testleri

Yeni bir paket ölçeklendirerek oluşturma ve yürütme test sunulmuştur. [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) paket, aşağıdaki görevleri gerçekleştirir:

* Bağımlılık dosya kopyalar (*\*.deps*) test projesinin içinde test edilmiş uygulamasından *bin* klasör.
* İçerik kök test edilen uygulamanın proje kök dizinine ayarlar, böylece zaman testler yürütülmeden statik dosyalar ve sayfalar/görünümler bulunur.
* Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) önyükleme ile test edilen uygulamayı kolaylaştırmak için sınıf [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Aşağıdaki test kullandığı [xUnit](https://xunit.github.io/) dizin sayfası doğru Content-Type üst bilgisiyle bir başarı durum kodu ile yüklendiğini kontrol etmek için:

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

Daha fazla bilgi için [tümleştirme testleri](xref:test/integration-tests) konu.

## <a name="apicontroller-actionresultt"></a>[ApiController] ActionResult\<T >

ASP.NET Core 2.1 temiz ve açıklayıcı web API'lerini de oluşturabilirler daha kolay hale getirmek ve programlama yeni kurallar ekler. `ActionResult<T>` Yeni bir tür, hala yanıt türü belirten sırasında yanıt türü veya (IActionResult için benzer şekilde), diğer herhangi bir eylem sonucu döndürmek uygulama izin verecek şekilde eklenir. `[ApiController]` Özniteliği de olarak eklendiğinden, Web API özel kuralları ve davranışları için iyileştirilmiş şekilde.

Daha fazla bilgi için [ASP.NET Core Web API yapı](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

ASP.NET Core 2.1 içeren yeni bir `IHttpClientFactory` yapılandırmak ve örneklerini kullanma kolaylaştırması hizmet `HttpClient` uygulamalarında. `HttpClient` Giden HTTP istekleri için birbirine işleyicileri temsilci olarak görevlendirme kavramı zaten sahip. Fabrika:

* ' In örneğini kaydetme yapar `HttpClient` daha sezgisel adlandırılmış istemci başına.
* Yeniden deneme CircuitBreakers, vb. için kullanılacak ilkeleri Polly izin veren bir Polly işleyici uygular.

Daha fazla bilgi için [HTTP isteklerini başlatma](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Kestrel'i aktarım yapılandırma

ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre. Daha fazla bilgi için [Kestrel web sunucusu uygulaması: aktarım yapılandırma](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Genel konak Oluşturucusu

Genel konak Oluşturucusu (`HostBuilder`) eklendi. Bu oluşturucu, HTTP isteklerini (ileti, arka plan görevleri, vs.) mıdl'ye işleme uygulamaları için kullanılabilir.

Daha fazla bilgi için [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Güncelleştirilmiş SPA şablonları

Tek sayfalı uygulama şablonları için Angular, React ve Redux ile React standart proje yapılarını kullanın ve her bir çerçeve için sistemleri oluşturmak için güncelleştirilir.

Angular CLI Angular şablonu temel alır ve React şablonlar oluşturma react-uygulama temel alır.
Daha fazla bilgi için [ASP.NET Core ile tek sayfalı uygulama şablonlarını kullanma](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Razor sayfaları için Razor varlıkları arama

2.1 içinde listelenen sırayla aşağıdaki dizinlerde Razor varlıklar (örneğin, düzenler ve kısmi bölümleri) Razor sayfaları arayın:

1. Geçerli sayfa klasör.
1. */Pages/paylaşılan /*
1. */Views/paylaşılan /*

## <a name="razor-pages-in-an-area"></a>Bir alanı, Razor sayfaları

Razor sayfaları artık destek [alanları](xref:mvc/controllers/areas). Alanların bir örnek için bireysel kullanıcı hesapları ile yeni bir Razor sayfaları web uygulaması oluşturun. Bireysel kullanıcı hesapları ile Razor sayfaları web uygulaması içeren */Areas/Identity/Pages*.

## <a name="mvc-compatibility-version"></a>MVC uyumluluk sürümü

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar.

Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.

## <a name="migrate-from-20-to-21"></a>2.1 için 2. 0'ı geçirme

Bkz: [ASP.NET Core 2.0 için 2.1 geçiş](xref:migration/20_21).

## <a name="additional-information"></a>Ek bilgiler

Değişikliklerin tam listesi için bkz. [ASP.NET Core 2.1 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.1.0).
