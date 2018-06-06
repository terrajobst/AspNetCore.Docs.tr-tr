---
title: ASP.NET Core 2.1 yenilikler nelerdir?
author: isaac2004
description: ASP.NET Core 2.1 yeni özellikler hakkında bilgi edinin.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.1
ms.openlocfilehash: 97c9f3d82264715358591a656dc4c1c5975b6113
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728953"
---
# <a name="whats-new-in-aspnet-core-21"></a>ASP.NET Core 2.1 yenilikler nelerdir?

Bu makalede, ASP.NET Core 2.1 en önemli değişiklikleri ile ilgili belgelere bağlantılar vurgular.

## <a name="signalr"></a>SignalR

SignalR ASP.NET Core 2.1 için yazılmıştır. ASP.NET Core SignalR bazı geliştirmeler içerir:

* Basitleştirilmiş bir genişleme modeli.
* Yeni bir JavaScript istemci hiçbir jQuery bağımlılığı.
* Üzerinde MessagePack bağlı olan yeni sıkıştırılmış ikili protokolü.
* Özel protokoller için destek.
* Yeni bir akış yanıt modeli.
* Üzerinde tam WebSockets tabanlı istemciler için destek.

Daha fazla bilgi için bkz: [ASP.NET Core SignalR](xref:signalr/index).

## <a name="razor-class-libraries"></a>Razor sınıf kitaplıkları

ASP.NET Core 2.1 yapı ve Razor tabanlı UI kitaplığa ve birden çok projeler arasında paylaşmak kolaylaştırır. Bir NuGet paketi paketlenen sınıf kitaplığı projesine Razor dosyalarıyla yeni Razor SDK'sı sağlar. Görünümleri ve kitaplıkları sayfalarında otomatik olarak bulunur ve uygulama tarafından geçersiz kılınabilir. Yapıda Razor derleme tümleştirerek:

* Uygulama başlangıç zamanı önemli ölçüde daha hızlıdır.
* Hızlı Razor görünümleri ve çalışma zamanında sayfalarına güncelleştirmelerin hala kullanılabilir bir yinelemeli geliştirme iş akışının parçası olarak.

Daha fazla bilgi için bkz: [Razor sınıf kitaplığı proje kullanarak yeniden kullanılabilir kullanıcı Arabirimi oluşturma](xref:mvc/razor-pages/ui-class).

## <a name="identity-ui-library--scaffolding"></a>Kimlik kullanıcı Arabirimi kitaplığı ve yapı iskelesi

ASP.NET Core 2.1 sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:mvc/razor-pages/ui-class). Kimliği içeren uygulamaları kimlik Razor sınıf kitaplığı (RCL) bulunan kaynak koduna seçerek eklemek için yeni kimlik iskele kurucu uygulayabilirsiniz. Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz. Örneğin, kayıt için kullanılan kodu oluşturmak için iskele kurucu istemeniz. Oluşturulan kod kimlik RCL aynı kodu daha önceliklidir.

Yapmak uygulamaları **değil** dahil kimlik doğrulama kimlik iskele kurucu RCL kimlik paketini eklemek için geçerli olabilir. Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.

Daha fazla bilgi için bkz: [İskele kimlik ASP.NET Core projelerinde](xref:security/authentication/scaffold-identity).

## <a name="https"></a>HTTPS

Güvenlik ve gizlilik hakkında daha fazla odak ile web uygulamaları için HTTPS'yi etkinleştirme önemlidir. HTTPS zorlama Web'de katı giderek büyüyor. HTTPS kullanmayan siteleri güvenli olarak kabul edilir. Tarayıcılar (Chromium, Mozilla), web özellikleri güvenli bağlamından kullanılmalıdır zorlamak başladılar. [GDPR](xref:security/gdpr) kullanıcı gizliliğini korumak için HTTPS kullanılmasını gerektirir. Üretimde HTTPS kullanarak kritik olsa da, geliştirme HTTPS kullanarak sorunları (örneğin, güvenli olmayan bağlantıları) dağıtımda önlenmesine yardımcı olabilir. ASP.NET Core 2.1 birkaç geliştirme HTTPS kullanmak üzere ve üretimde HTTPS yapılandırmak için daha kolay geliştirme içerir. Daha fazla bilgi için bkz: [zorunlu HTTPS](xref:security/enforcing-ssl).

### <a name="on-by-default"></a>Üzerinde varsayılan olarak

Güvenli Web sitesi geliştirme kolaylaştırmak için HTTPS artık varsayılan olarak etkindir. Dinlediği Kestrel 2.1 başlayarak, `https://localhost:5001` yerel geliştirme sertifikası olduğunda mevcut. Geliştirme sertifikası oluşturulur:

* SDK'yı ilk kez kullandığınızda, .NET Core SDK ilk çalıştırma deneyimi bir parçası olarak.
* Yeni kullanarak el ile `dev-certs` aracı.

Çalıştırma `dotnet dev-certs https --trust` sertifikaya güvenmelerini.

### <a name="https-redirection-and-enforcement"></a>HTTPS yeniden yönlendirmesi ve zorlama

Web uygulamaları genellikle hem HTTP hem de HTTPS dinleme ancak HTTPS için tüm HTTP trafiği yeniden yönlendirmeniz gerekir. 2.1 akıllıca yönlendiren özel HTTPS yeniden yönlendirmesi ara yazılım yapılandırma varlığına göre veya ilişkili sunucu bağlantı noktaları sunulan.

HTTPS kullanımı daha fazla zorunlu kullanarak [HTTP katı Aktarım güvenlik protokolü (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts). HSTS daima HTTPS üzerinden siteye erişmek için tarayıcılar bildirir. ASP.NET Core 2.1 max yaşı, alt etki alanları, seçeneklerini destekler HSTS ara yazılımı ekler ve listeyi HSTS dağıtılacak.

### <a name="configuration-for-production"></a>Üretim için yapılandırma

Üretimde HTTPS açıkça yapılandırılmalıdır. 2.1 içinde HTTPS Kestrel için yapılandırmak için varsayılan yapılandırma şeması eklendi. Uygulamalar kullanacak şekilde yapılandırılabilir:

* URL'leri dahil olmak üzere birden çok uç nokta. Daha fazla bilgi için bkz: [Kestrel web sunucusu uygulaması: uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).
* Diskteki bir dosyanın veya bir sertifika deposundan HTTPS kullanmak üzere sertifika.

## <a name="gdpr"></a>GDPR

ASP.NET Core API ve şablonları bazı karşılamak amacıyla sağlar [AB genel veri koruma düzenleme (GDPR)](https://www.eugdpr.org/) gereksinimleri. Daha fazla bilgi için bkz: [GDPR destek ASP.NET Core](xref:security/gdpr). A [örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) nasıl kullanılacağını gösterir ve GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonları eklenen API'leri çoğunu test olanak tanır.

## <a name="integration-tests"></a>Tümleştirme testleri

Yeni bir paket ölçeklendirerek oluşturma ve yürütme test sunulmuştur. [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) paketi aşağıdaki görevleri gerçekleştirir:

* Bağımlılık dosyası kopyalar (*\*.deps*) test projesinin içine sınanan uygulamadan *bin* klasör.
* Statik dosyalar ve sayfaları/görünümleri testleri ne zaman çalıştırılır böylece bulunur içerik kök sınanan uygulamanın proje kök dizinine ayarlar.
* Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) sınanan uygulamayla önyükleme kolaylaştırmak için sınıf [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).

Aşağıdaki sınama kullanır [xUnit](https://xunit.github.io/) dizin sayfası bir başarı durum kodu ile doğru Content-Type üstbilgisi yükler denetlemek için:

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

Daha fazla bilgi için bkz: [tümleştirme testleri](xref:test/integration-tests) konu.

## <a name="apicontroller-actionresult"></a>[ApiController] ActionResult

ASP.NET Core 2.1 temiz ve açıklayıcı web API oluşturma kolaylaştıran yeni programlama kuralları ekler. `ActionResult<T>` Yeni bir tür bir uygulamayı hala yanıt türünü belirten sırasında bir yanıt türü veya (IActionResult için benzer şekilde), diğer herhangi bir eylem sonucu dönmek için izin vermek için eklenir. `[ApiController]` Özniteliği de olarak eklendiğinden Web API özel kuralları ve davranışları kabul yolu.

Daha fazla bilgi için bkz: [yapı Web API ile ASP.NET çekirdek](xref:web-api/index).

## <a name="ihttpclientfactory"></a>IHttpClientFactory

ASP.NET Core 2.1 içeren yeni bir `IHttpClientFactory` yapılandırmak ve örnekleri kullanmak kolaylaştırır hizmet `HttpClient` uygulamalarda. `HttpClient` zaten giden HTTP istekleri için birbirine işleyicileri için temsilci seçme kavramı vardır. Fabrika:

* Örnekleri kaydetme yapar `HttpClient` daha sezgisel adlandırılmış istemci başına.
* Yeniden deneme, CircuitBreakers vb. için kullanılacak ilke Polly veren Polly işleyici uygular.

Daha fazla bilgi için bkz: [başlatmak HTTP isteklerini](xref:fundamentals/http-requests).

## <a name="kestrel-transport-configuration"></a>Kestrel aktarım yapılandırma

ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda dayalı. Daha fazla bilgi için bkz: [Kestrel web sunucusu uygulaması: aktarım yapılandırma](xref:fundamentals/servers/kestrel#transport-configuration).

## <a name="generic-host-builder"></a>Genel ana bilgisayar Oluşturucusu

Genel ana bilgisayar Oluşturucusu (`HostBuilder`) sunulmuştur. Bu oluşturucu, HTTP isteklerini (ileti, arka plan görevleri, vs.) işleyen olmayan uygulamalar için kullanılabilir.

Daha fazla bilgi için bkz: [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).

## <a name="updated-spa-templates"></a>Güncelleştirilmiş SPA şablonları

Angular, tepki, tek sayfa uygulaması şablonları ve tepki Redux ile standart proje yapıları kullanın ve her çerçevesi için sistemler oluşturmak için güncelleştirilir.

Açısal şablonu Açısal CLI üzerinde temel alır ve tepki şablonları oluşturma tepki-uygulama üzerinde temel alır.
Daha fazla bilgi için bkz: [ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma](xref:spa/index).

## <a name="razor-pages-search-for-razor-assets"></a>Razor sayfalarının Razor varlıkları için arama

2.1 içinde listelenen sırayla aşağıdaki dizinlerde Razor varlıklar (örneğin, düzenleri ve kısmi) Razor sayfalarının arayın:

1. Geçerli sayfa klasör.
1. */Pages/paylaşılan /*
1. */Views/paylaşılan /*

## <a name="razor-pages-in-an-area"></a>Bir alanı Razor sayfalarında

Razor sayfalarının şimdi destek [alanları](xref:mvc/controllers/areas). Örneği alanları görmek için bireysel kullanıcı hesapları ile yeni bir Razor sayfalarının web uygulaması oluşturun. Bireysel kullanıcı hesapları ile Razor sayfalarının web uygulamasını içeren */Areas/Identity/Pages*.

## <a name="migrate-from-20-to-21"></a>2. 0 2.1 için geçirme

Bkz: [ASP.NET çekirdek 2.0 için 2.1 geçirmek](xref:migration/20_21).

## <a name="additional-information"></a>Ek bilgiler

Değişiklikleri tam listesi için bkz: [ASP.NET Core 2.1 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.1.0).
