---
title: ASP.NET core'da tümleştirme testleri
author: guardrex
description: Bir uygulamanın bileşenleri doğru veritabanı, dosya sistemi ve ağ gibi altyapı düzeyinde çalışması tümleştirme testleri nasıl emin öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 15186383497f8dd9a9766f8fe7cf32f70a121c8e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477598"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET core'da tümleştirme testleri

Tarafından [Luke Latham](https://github.com/guardrex) ve [Steve Smith](https://ardalis.com/)

Tümleştirme testleri, bir uygulamanın bileşenleri içeren bir veritabanı, dosya sistemi ve ağ gibi uygulamanın destekleyen altyapı düzeyinde düzgün emin olun. ASP.NET Core, tümleştirme testlerini bir test web ana bilgisayarı ve bir bellek içi test sunucusu ile birim testi çerçevesini kullanarak destekler.

Bu konuda, birim testleri temel bir anlayış varsayılır. Bilinmeyen test kavramlarına aşina değilse [birim testi .NET Core ve .NET Standard](/dotnet/core/testing/) konu ve bağlantılı içeriği.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama bir Razor sayfaları uygulamasıdır ve Razor sayfaları temel bir anlayış varsayar. Tanınmayan Razor sayfalarıyla istiyorsanız, aşağıdaki konulara bakın:

* [Razor sayfaları giriş](xref:razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)

> [!NOTE]
> Spa'lar test etmek için bir aracı gibi önerilir [Selenium](https://www.seleniumhq.org/), bir tarayıcı getirmenizi.

## <a name="introduction-to-integration-tests"></a>Tümleştirme testleri giriş

Bir uygulamanın bileşenleri daha geniş bir düzeyde tümleştirme testleri değerlendirmek [birim testleri](/dotnet/core/testing/). Birim testleri gibi tek tek sınıf yöntemlerini yalıtılmış yazılım bileşenlerini test etmek için kullanılır. Tümleştirme testleri, iki veya daha fazla uygulama bileşenleri birlikte büyük olasılıkla tam olarak bir isteği işlemek için gerekli her bileşen dahil olmak üzere bir beklenen sonucu verecek çalıştığını doğrulayın.

Daha geniş olan bu testler, uygulamanın altyapı ve genellikle aşağıdaki bileşenler dahil olmak üzere tüm framework test etmek için kullanılır:

* Veritabanı
* Dosya sistemi
* Ağ Gereçleri
* İstek-yanıt işlem hattı

Birim testleri üretilmiş kullanım bileşenleri olarak bilinen *fakes* veya *sahte nesneler*, altyapı bileşenlerini yerine.

Birim testleri aksine, tümleştirme testleri:

* Üretim ortamında uygulamanın kullandığı gerçek bileşenlerini kullanın.
* Daha fazla kod ve veri işleme gerektirir.
* Çalıştırmak için daha uzun sürer.

Bu nedenle, tümleştirme testlerini en önemli altyapı senaryolarından kullanımını sınırlayın. Bir davranış birim testi veya bir tümleştirme testi kullanılarak test edilebilir, birim testi seçin.

> [!TIP]
> Her olası permütasyon ile veritabanları ve dosya sistemleri veri ve dosya erişimi için tümleştirme testleri yazmayın. Bir uygulama arasında kaç yerleştirir bağımsız olarak veritabanları ve dosya sistemleri, testleri yeterince test veritabanı genellikle uyumlu ve dosya sistemi okuma, yazma, güncelleştirme ve silme tümleştirme odaklanmış bir dizi ile etkileşim. Bu bileşenlerle etkileşime düzenli testler yöntemi mantığı için kullanım birim testleri. Birim testlerinde altyapı kullanımını fakes/daha hızlı bir testi çalıştırma sonucu mocks.

> [!NOTE]
> Test edilen projenin sık olarak adlandırılan tümleştirme testleri tartışmalara *test altındaki sistem*, veya kısaca "SUT".

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core tümleştirme testleri

ASP.NET core'da tümleştirme testleri aşağıdakileri gerektirir:

* Bir test projesi içeren ve testleri yürütmek için kullanılır. Test projesi adlı test edilmiş ASP.NET Core projesi bir başvuru içeriyor *test altındaki sistem* (SUT). _"SUT" Bu konu başlığı altında yaptığımız, test edilen uygulamaya başvurmak için kullanılır._
* Test projesi SUT için bir test web ana bilgisayar oluşturur ve bir test sunucusu istemci isteklerinin ve yanıtlarının SUT işlemek için kullanır.
* Test Çalıştırıcısı, test sonuçlarını rapor ve testleri yürütmek için kullanılır.

Tümleştirme testleri, normal içeren olayların sırasını izleyin *Yerleştir*, *Yasası*, ve *Assert* test adımları:

1. SUT'ın web ana bilgisayarı yapılandırılır.
1. Bir test sunucusu istemci uygulamanın istekleri göndermek için oluşturulur.
1. *Yerleştir* test adımı yürütülür: istek test uygulaması hazırlar.
1. *Yasası* test adımı yürütülür: istemci isteği gönderir ve yanıtı alır.
1. *Assert* test adımı yürütülür: *gerçek* yanıt olarak doğrulanmış bir *geçirmek* veya *başarısız* dayalı bir *bekleniyor*  yanıt.
1. İşlem, tüm testleri yürütülür kadar devam eder.
1. Test sonuçları raporlanır.

Genellikle, uygulamanın normal web ana bilgisayarı için test çalıştıran çok test web ana bilgisayarı farklı şekilde yapılandırılır. Örneğin, farklı bir veritabanı veya farklı uygulama ayarları testler için kullanılabilir.

Test web ana bilgisayarı ve bellek içi test sunucusuna gibi altyapı bileşenlerini ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), sağlanan veya yönetilen [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paket. Bu paket kullanımını test oluşturma ve yürütme kolaylaştırır.

`Microsoft.AspNetCore.Mvc.Testing` Paket, aşağıdaki görevleri gerçekleştirir:

* Bağımlılıkları dosyayı kopyalar (*\*.deps*) test projesinin içine SUT gelen *bin* klasör.
* Böylece, testler yürütülmeden statik dosyalar ve sayfalar/görünümler bulunur içerik kök SUT'ın Proje kök dizinine ayarlar.
* Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) ile SUT önyükleme kolaylaştırmak için sınıf `TestServer`.

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) belgeleri nasıl bir test projesi ve test Çalıştırıcısı, testleri ve öneriler için nasıl adı testleri çalıştırmak ve test sınıflarına konusunda ayrıntılı yönergeler birlikte ayarlanacağı açıklanmaktadır.

> [!NOTE]
> Bir uygulama için bir test projesi oluştururken, birim testleri tümleştirme testleri farklı projelere ayırma ayırın. Bu, birim testlerine dahil altyapı test bileşenlerini yanlışlıkla olmadığından emin olun yardımcı olur. Ayrıca birim ve tümleştirme testleri ayrımı sağlar hangi testler küme üzerinde çalıştırılır denetleyen.

Razor sayfaları uygulamaların testler için yapılandırma ve MVC uygulamaları arasındaki neredeyse hiçbir fark yoktur. Tek fark, testleri nasıl adlandırıldığı ' dir. Bir Razor sayfaları uygulamasında sayfasında uç noktası testleri genellikle sonra sayfa modeli sınıfı adlandırılır (örneğin, `IndexPageTests` bileşen tümleştirme dizin sayfası için test etmek için). Bir MVC uygulamasında testleri genellikle denetleyicisi sınıfları tarafından düzenlenen ve bunlar test denetleyicileri sonra adlı (örneğin, `HomeControllerTests` bileşen tümleştirme giriş denetleyicisine için test etmek için).

## <a name="test-app-prerequisites"></a>Test uygulama önkoşulları

Test projesi gerekir:

* Aşağıdaki paketler başvuru:
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Proje dosyasında Web SDK'sı belirtin (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Web SDK'sı başvururken gereklidir [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

Bu Önkoşullar şurada görülebilir [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). İnceleme *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* dosya. Örnek uygulama kullandığı [xUnit](https://xunit.github.io/) test çerçevesi ve [AngleSharp](https://anglesharp.github.io/) örnek uygulamasını da başvurduğu için ayrıştırıcı kitaplığı:

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.Runner.VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>' % S'varsayılan WebApplicationFactory temel testleri

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) oluşturmak için kullanılan bir [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) tümleştirme testleri için. `TEntryPoint` Giriş noktası SUT genellikle sınıfıdır `Startup` sınıfı.

Test sınıfları uygulama bir *sınıfı düzeni* arabirimi (`IClassFixture`) sınıfı testleri içeren ve paylaşılan nesne örneklerini sınıfındaki sağlamak belirtmek için.

### <a name="basic-test-of-app-endpoints"></a>Uygulama uç noktalar için temel test

Aşağıdaki test sınıfı `BasicTests`, kullandığı `WebApplicationFactory` SUT bootstrap ve sağlamak için bir [HttpClient](/dotnet/api/system.net.http.httpclient) bir test yöntemine `Get_EndpointsReturnSuccessAndCorrectContentType`. Yöntemi, yanıt durumu kodu başarılı olup olmadığını denetler (200 299 aralıktaki durum kodları) ve `Content-Type` üstbilgisi `text/html; charset=utf-8` birkaç uygulama sayfaları için.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) örneği oluşturur `HttpClient` otomatik olarak yeniden yönlendirmeleri takip eden ve tanımlama bilgilerini işler.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Güvenli bir uç nokta test

Başka bir testte `BasicTests` sınıfı denetler güvenli bir uç noktası kimliği doğrulanmamış bir kullanıcı uygulamanın oturum açma sayfasına yönlendirir.

SUT içinde `/SecurePage` sayfasında kullanan bir [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı uygulamak için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) sayfası. Daha fazla bilgi için [Razor sayfaları yetkilendirmesi kuralları](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

İçinde `Get_SecurePageRequiresAnAuthenticatedUser` test, bir [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) ayarlayarak yeniden yönlendirmeleri izin vermeyecek şekilde [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) için `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Yeniden yönlendirme izlemek için istemci engelleyerek, aşağıdaki denetimleri hale getirilebilir:

* SUT tarafından döndürülen durum kodu denetlenebilir beklenen karşı [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) sonuç, son durum kodu değil olabilecek oturum açma sayfasına yeniden yönlendirme sonra [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Yanıt üstbilgilerini üst bilgi değeri ile başlayan onaylamak için işaretli `http://localhost/Identity/Account/Login`, olmayan son oturum açma sayfası yanıt, burada `Location` üstbilgi mıydı mevcut olabilir.

Daha fazla bilgi için `WebApplicationFactoryClientOptions`, bkz: [istemci seçenekleri](#client-options) bölümü.

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory özelleştirme

Web ana bilgisayar yapılandırması oluşturulabilir bağımsız olarak test sınıflarında devralarak `WebApplicationFactory` bir veya daha fazla özel fabrikaları oluşturmak için:

1. Devralınan `WebApplicationFactory` ve geçersiz kılma [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) hizmet koleksiyonuyla yapılandırılmasını sağlayan [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Veritabanı içinde dengeli dağıtım [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) tarafından gerçekleştirilen `InitializeDbForTests` yöntemi. Yöntem açıklanan [tümleştirme testleri örnek: Test uygulama kuruluş](#test-app-organization) bölümü.

2. Özel kullanmanın `CustomWebApplicationFactory` test sınıflarında. Aşağıdaki örnek, fabrikada kullanır `IndexPageTests` sınıfı:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Örnek uygulamanın istemci engelleyecek şekilde yapılandırılmış `HttpClient` gelen aşağıdaki yeniden yönlendirir. İçinde anlatıldığı gibi [güvenli bir uç nokta Test](#test-a-secure-endpoint) bölümünde bu verir testleri uygulamanın ilk yanıtın sonucunu denetle. İlk yanıt, bir yeniden yönlendirme ile bu testleri birçok bir `Location` başlığı.

3. Tipik bir test kullanan `HttpClient` ve istek ve yanıt işlemek için yardımcı yöntemler:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

SUT yapılan tüm POST istekleri otomatik olarak uygulama tarafından yapılan antiforgery onay karşılamalıdır [veri koruma sisteminde antiforgery](xref:security/data-protection/introduction). Bir testin POST isteği için düzenlemek için test uygulaması gerekir:

1. Sayfa için bir istek olun.
1. İstek Doğrulama belirtecini yanıtından ve antiforgery tanımlama bilgisi ayrıştırılamıyor.
1. POST isteğini antiforgery tanımlama bilgisi ve istek doğrulama ile yerinde belirteci olun.

`SendAsync` Yardımcı uzantı yöntemleri (*Helpers/HttpClientExtensions.cs*) ve `GetDocumentAsync` yardımcı yöntemini (*Helpers/HtmlHelpers.cs*) içinde [örnekuygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) kullanın [AngleSharp](https://anglesharp.github.io/) aşağıdaki yöntemlerle antiforgery onay işlemek için ayrıştırıcı:

* `GetDocumentAsync` &ndash; Alan [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ve döndüren bir `IHtmlDocument`. `GetDocumentAsync` hazırlayan bir Fabrika kullanan bir *sanal yanıt* özgün tabanlı `HttpResponseMessage`. Daha fazla bilgi için [AngleSharp belgeleri](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` için genişletme yöntemleri `HttpClient` oluşturan bir [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) ve çağrı [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) SUT istekleri göndermek için. İçin aşırı yüklediği `SendAsync` HTML formu kabul (`IHtmlFormElement`) ve aşağıdaki:
  - Düğmeyi form gönderme (`IHtmlElement`)
  - Form değerleri koleksiyonunu (`IEnumerable<KeyValuePair<string, string>>`)
  - Gönder düğmesine (`IHtmlElement`) ve form değerleri (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) üçüncü taraf ayrıştırma olan bu konu ve örnek uygulamayı tanıtım amacıyla kullanılan kitaplığı. AngleSharp desteklenen veya tümleştirme ASP.NET Core uygulamalarını test etmek için gerekli değildir. Diğer Çözümleyicileri, aşağıdaki gibi kullanılabilir [Html çevikliği paketi (HAP)](http://html-agility-pack.net/). Başka bir yaklaşım antiforgery sistemin istek Doğrulama belirtecini ve antiforgery tanımlama bilgisi doğrudan işlemek için kod yazmaktır.

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder istemcisiyle özelleştirme

Bir test yöntemi içinde ek yapılandırma gerektiğinde [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) yeni bir oluşturur `WebApplicationFactory` ile bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , daha fazla özelleştirilmiş yapılandırma.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Test yöntemi [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) kullanımını gösteren `WithWebHostBuilder`. Bu test, bir form gönderimi SUT tetikleyerek, veritabanında bir kayıt silme gerçekleştirir.

Çünkü başka test `IndexPageTests` sınıfı tüm veritabanındaki kayıtları siler ve önce çalıştırılabilir bir işlem gerçekleştiren `Post_DeleteMessageHandler_ReturnsRedirectToRoot` yöntem, veritabanı çekirdek değeri oluşturulmuş bir kaydı silmek için SUT mevcut olduğundan emin olmak için bu test yönteminde. Seçme `deleteBtn1` düğmesini `messages` SUT biçiminde benzetimli SUT isteğinde:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>İstemci seçenekleri

Aşağıdaki tabloda, varsayılan gösterilmektedir [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) oluşturulurken kullanılabilir `HttpClient` örnekleri.

| Seçenek | Açıklama | Varsayılan |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Alır veya ayarlar olsun veya olmasın `HttpClient` örnekleri otomatik olarak yeniden yönlendirme yanıtlarını izlemelidir. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Alır veya ayarlar temel adresini `HttpClient` örnekleri. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Alır veya ayarlar olmadığını `HttpClient` örnekleri tanımlama bilgilerini işleme. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Alır veya ayarlar yeniden yönlendirme yanıtlarını sayısı `HttpClient` örnekleri izlemelidir. | 7 |

Oluşturma `WebApplicationFactoryClientOptions` sınıfı ve geçirin [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) yöntemi (varsayılan değerler, aşağıdaki kod örneğinde gösterilmiştir):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Sahte Hizmetleri ekleme

Hizmetleri, bir test çağrısı ile kılınabilir [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) üzerinde konak Oluşturucusu. **Sahte Hizmetleri eklemesine SUT olmalıdır bir `Startup` sınıfıyla birlikte bir `Startup.ConfigureServices` yöntemi.**

Örnek SUT teklif döndüren bir kapsamlı hizmet içerir. Dizin Sayfası istendiğinde teklif gizli alanda dizin sayfasına katıştırılır.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Aşağıdaki biçimlendirmede SUT uygulama çalıştırıldığında oluşturulur:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Bir tümleştirme testi hizmet ve teklif ekleme için sahte bir hizmet tarafından test SUT eklenmiş olur. Sahte hizmet uygulamanın yerini alan `QuoteService` test uygulama tarafından sağlanan bir hizmetle adlı `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` olarak adlandırılan ve kapsamlı hizmet kayıtlı:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Test yürütme sırasında üretilen biçimlendirme tarafından sağlanan teklif metin yansıtır `TestQuoteService`, bu nedenle onaylama geçişleri:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Sınama altyapının uygulama içerik kök yolu nasıl algılar

`WebApplicationFactory` Oluşturucusu çıkarsar uygulama içerik kök yolu arayarak bir [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) eşit bir anahtarla tümleştirme testleri içeren derleme üzerinde `TEntryPoint` derleme`System.Reflection.Assembly.FullName`. Özniteliğin doğru anahtarla bulunamadığında durumda `WebApplicationFactory` bir çözüm dosyası için aramaya geri döner (*\*.sln*) ve ekler `TEntryPoint` çözüm dizinine derleme adı. Uygulama kök dizinindeki (içerik kök yol), görünümleri ve içerik dosyalarını bulmak için kullanılır.

Arama mantığı çalışma zamanında doğru içerik kök genellikle bulur. çoğu durumda, bu uygulama içerik kök açıkça ayarlamak gerekli değildir. İçerik kök bulunduğu değil özel senaryolarda yerleşik arama algoritması, uygulama kök açıkça veya özel mantığı kullanarak belirtilen içerik kullanma. Bu senaryolarda uygulama içerik kök ayarlamak için çağrı `UseSolutionRelativeContentRoot` şuradan genişleme metodu [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) paket. Çözümün göreli yol ve dosya adı veya glob deseni isteğe bağlı bir çözüm sağlamak (varsayılan = `*.sln`).

Çağrı [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) uzantısı yöntemiyle *bir* aşağıdaki yaklaşımlardan biri:

* Test sınıflarıyla yapılandırırken `WebApplicationFactory`, özel bir yapılandırma sağlamak [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Özel bir test sınıflarında yapılandırırken `WebApplicationFactory`, devralınan `WebApplicationFactory` ve geçersiz kılma [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Gölge kopyalama devre dışı bırak

Gölge kopyalama çıktı klasörünü farklı bir klasörde yürütülecek testleri neden olur. Gölge kopyalama düzgün çalışması testleri için devre dışı bırakılmalıdır. [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit kullanır ve dahil ederek xUnit için gölge kopyalama devre dışı bırakan bir *xunit.runner.json* doğru yapılandırma ayarı dosyası. Daha fazla bilgi için [xUnit.net ile JSON yapılandırma](https://xunit.github.io/docs/configuring-with-json.html).

Ekleme *xunit.runner.json* dosya aşağıdaki içeriğe sahip test projesinin kök:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Tümleştirme testleri örneği

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) iki uygulama oluşur:

| Uygulama | Proje klasörü | Açıklama |
| --- | -------------- | ----------- |
| İleti uygulaması (SUT) | *src/RazorPagesProject* | Eklemek, silmek, tüm silmek ve iletileri çözümleme açmasına olanak sağlar. |
| Test uygulaması | *tests/RazorPagesProject.Tests* | Tümleştirme testi SUT için kullanılır. |

Bir IDE özelliklerini yerleşik test gibi kullanarak testler çalıştırılabilir [Visual Studio](https://www.visualstudio.com/vs/). Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırının *tests/RazorPagesProject.Tests* klasörü:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>İleti uygulaması (SUT) kuruluş

Aşağıdaki özelliklere sahip bir Razor sayfaları ileti sistemi SUT şöyledir:

* Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) UI ve sayfa ekleme, silme ve analiz iletilerinin (ileti başına ortalama kelimeler) denetlemek için model yöntemleri sağlar. .
* İleti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliğe sahip: `Id` (anahtar) ve `Text` (mesaj). `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletileri kullanarak depolanan [Entity Framework'ün bellek içi veritabanına](/ef/core/providers/in-memory/)&#8224;.
* Uygulama, veritabanı bağlamı sınıfının bir veri erişim katmanı (DAL) içeren `AppDbContext` (*Data/AppDbContext.cs*).
* Uygulama başlangıcında veritabanı boşsa, ileti deposu üç iletileri ile başlatılır.
* Uygulamayı içeren bir `/SecurePage` , yalnızca erişilebilir bir kimliği doğrulanmış kullanıcı tarafından.

&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), MSTest ile testleri için bellek içi veritabanına nasıl kullanıldığını açıklar. Bu konuda kullanan [xUnit](https://xunit.github.io/) test çerçevesi. Test kavramları ve test uygulamaları arasında farklı test çerçeveleri benzer, ancak aynı değildir.

Uygulama deposu düzeni kullanmaz ve etkili bir örneği değil ancak [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfaları geliştirme bu desenleri destekler. Daha fazla bilgi için [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek depo Yapılacaklar listesi).

### <a name="test-app-organization"></a>Test uygulama kuruluş

Bir konsol uygulaması içinde test uygulamasıdır *tests/RazorPagesProject.Tests* klasör.

| Test uygulama klasörü | Açıklama |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* yönlendirme, güvenli bir sayfa kimliği doğrulanmamış bir kullanıcı tarafından erişme ve GitHub kullanıcı profili edinme ve profilinin kullanıcı oturum açma denetimi için test yöntemleri içerir. |
| *IntegrationTests* | *IndexPageTests.cs* özel kullanarak dizin sayfasına için tümleştirme testleri içeren `WebApplicationFactory` sınıfı. |
| *Yardımcıları/yardımcı programları* | <ul><li>*Utilities.cs* içeren `InitializeDbForTests` test verileri ile veritabanının çekirdeğini oluşturma için kullanılan yöntem.</li><li>*HtmlHelpers.cs* bir AngleSharp döndürmek için bir yöntem sağlar `IHtmlDocument` test yöntemleri tarafından kullanılacak.</li><li>*HttpClientExtensions.cs* için aşırı yüklemeler sağlar `SendAsync` SUT istekleri göndermek için.</li></ul> |

Test çerçevesi [xUnit](https://xunit.github.io/). Tümleştirme testleri kullanarak gerçekleştirilen [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), içeren [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Çünkü [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paket test ana bilgisayarı ve test sunucuyu yapılandırmak için kullanılan `TestHost` ve `TestServer` paketi doğrudan paket başvuruları test uygulamanın proje dosyasında gerektirir yok veya test uygulamadaki Geliştirici yapılandırması.

**Test etmek için veritabanı dengeli dağıtım**

Tümleştirme testleri genellikle test yürütülmeden önce veritabanında küçük bir veri kümesi gerektirir. Örneğin, bir silme test çağrıları için veritabanı kayıt silme işlemi, bu nedenle veritabanı silme isteği başarılı olması için en az bir kayıt olması gerekir.

Örnek uygulamayı üç iletilerinde veritabanıyla çekirdeğini *Utilities.cs* bunlar yürüttüğünüzde testlerini kullanabilirsiniz:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Test denetleyicileri](xref:mvc/controllers/testing)
