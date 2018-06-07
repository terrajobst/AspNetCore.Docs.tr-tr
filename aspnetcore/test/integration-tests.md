---
title: ASP.NET Core tümleştirme testlerinde
author: guardrex
description: Bir uygulamanın bileşenleri doğru veritabanı, dosya sistemi ve ağ dahil olmak üzere altyapı düzeyinde işlev tümleştirme testleri nasıl emin öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734720"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core tümleştirme testlerinde

Tarafından [Luke Latham](https://github.com/guardrex) ve [Steve Smith](https://ardalis.com/)

Tümleştirme testleri bir uygulamanın bileşenleri veritabanı, dosya sistemi ve ağ gibi uygulamanın destekleyici altyapının içeren düzeyde düzgün emin olun. ASP.NET Core test web ana bilgisayar ve bir bellek içi test sunucusu ile bir birim testi çerçevesi kullanarak tümleştirme testleri destekler.

Bu konu, birim testleri temel bir anlayış varsayar. Tanınmayan test kavramları bkz [birim testi .NET Core ve .NET standart](/dotnet/core/testing/) konu ve bağlantılı içeriği.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama Razor sayfalarının uygulama ve Razor sayfalarının temel bir anlayış varsayar. Tanınmayan Razor sayfaları ile varsa, aşağıdaki konulara bakın:

* [Razor sayfalarının giriş](xref:mvc/razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Razor sayfalarının birim testleri](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Tümleştirme testleri giriş

Bir uygulamanın bileşenleri daha geniş bir düzeyde tümleştirme testleri değerlendirmek [birim testleri](/dotnet/core/testing/). Birim testleri, tek tek sınıf yöntemlerini gibi yalıtılmış yazılım bileşenleri test etmek için kullanılır. Tümleştirme testleri, iki veya daha fazla uygulama bileşenleri büyük olasılıkla tam olarak bir isteği işlemek için gereken her bir bileşen dahil olmak üzere bir beklenen sonucu oluşturmak için birlikte çalışan olduğunu onaylayın.

Daha geniş bu testler, uygulamanın altyapı ve genellikle aşağıdaki bileşenler de dahil olmak üzere tüm framework test etmek için kullanılır:

* Veritabanı
* Dosya sistemi
* Ağ uygulamaları
* İstek-yanıt ardışık düzen

Birim testleri olarak bilinen üretilmiş kullanım bileşenleri *fakes* veya *mock nesneleri*, altyapı bileşenleri yerine.

Birim testleri aksine tümleştirme testleri:

* Üretimde uygulamanın kullandığı gerçek bileşenlerini kullanın.
* Daha fazla kod ve veri işleme gerektirir.
* Çalıştırmak için daha uzun sürer.

Bu nedenle, tümleştirme testleri için en önemli altyapı senaryolarıyla kullanımını sınırlayın. Bir davranış birim testi veya bir tümleştirme testi kullanılarak test edilebilir birim testi belirleyin.

> [!TIP]
> Veri ve dosya erişim veritabanları ve dosya sistemleri ile olası her nesne için tümleştirme testleri yazmayın. Bir uygulama kaç yerleştirir bağımsız olarak veritabanları ve dosya sistemleri, okuma, yazma, güncelleştirme ve silme tümleştirme testleri yeterli test veritabanı genellikle özelliğine sahiptir ve sistem bileşenleri dosya odaklanmış bir dizi ile etkileşim. Bu bileşenleri ile etkileşim rutin testleri yöntemi mantığı için kullanım birim testleri. Birim testleri altyapı kullanımını fakes/daha hızlı bir testi çalıştırma sonucu mocks.

> [!NOTE]
> Sınanan proje sık olarak adlandırılan tümleştirme testleri tartışmalara *test sisteminde*, veya kısaca "SUT".

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core tümleştirme testleri

ASP.NET Core tümleştirme testlerinde aşağıdakiler gerekir:

* Bir test projesi içeren ve testler yürütmek için kullanılır. Oluşturduğunuz test projesinin adlı test edilmiş ASP.NET Core projesi başvuruyor *test sisteminde* (SUT). _"SUT", test edilmiş uygulamaya başvurmak için bu konu boyunca kullanılır._
* Test projesinin test web ana bilgisayarı SUT için oluşturur ve bir test sunucusu istemci isteklerinin ve yanıtlarının SUT işlemek için kullanır.
* Test Çalıştırıcısı, sınamaları ve rapor test sonuçlarını yürütmek için kullanılır.

Tümleştirme testleri izleyin normal dahil olaylar dizisi *Yerleştir*, *Act*, ve *Assert* test adımları:

1. SUT'ın web barındırma yapılandırılır.
1. Bir test sunucusu istemci uygulamanın istekleri göndermek için oluşturulur.
1. *Yerleştir* test adımı yürütüldüğünde: test uygulama isteği hazırlar.
1. *Act* test adımı yürütüldüğünde: istemci isteği gönderir ve yanıtını alır.
1. *Assert* test adımı yürütüldüğünde: *gerçek* yanıt doğrulanmış olarak bir *geçirmek* veya *başarısız* dayalı bir *bekleniyor*  yanıt.
1. İşlem, tüm testleri yürütülen kadar devam eder.
1. Test sonuçları raporlanır.

Genellikle, test için uygulamanın normal web konak çalıştırır daha test web ana farklı şekilde yapılandırılır. Örneğin, farklı bir veritabanı veya farklı uygulama ayarları testler için kullanılabilir.

Bellek içi test sunucusuna ve test web ana gibi altyapı bileşenlerini ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), sağlanan veya tarafından yönetilen [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paket. Bu paket kullanımını test oluşturma ve yürütme kolaylaştırır.

`Microsoft.AspNetCore.Mvc.Testing` Paketi aşağıdaki görevleri gerçekleştirir:

* Bağımlılıklar dosya kopyalar (*\*.deps*) test projesinin içine SUT gelen *bin* klasör.
* Statik dosyalar ve sayfaları/görünümleri testleri ne zaman çalıştırılır böylece bulunur içerik kök SUT'ın Proje kök dizinine ayarlar.
* Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) ile SUT önyükleme kolaylaştırmak için sınıf `TestServer`.

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) belgelerine nasıl bir test projesine ve test Çalıştırıcısı, sınamaları ve öneriler nasıl adı için testler ve sınıfları sınamak ayrıntılı yönergeler birlikte ayarlanacağı açıklanmaktadır.

> [!NOTE]
> Bir uygulama için bir test projesi oluştururken, birim testleri farklı projelere ayırma tümleştirme testlerden ayırın. Bu, birim testleri dahil altyapı test bileşenleri yanlışlıkla olmadığından emin olun yardımcı olur. Ayırma birimi ve tümleştirme testleri de sağlar hangi testlerin küme üzerinde çalıştığını denetleyen.

Neredeyse Razor sayfalarının uygulamaların testleri için yapılandırma ve MVC uygulamalar arasında fark yoktur. Testleri nasıl adlandırıldığı yalnızca farktır. Razor sayfalarının uygulamada, testleri sayfası uç noktaları genellikle sonra sayfa model sınıfı adlandırılır (örneğin, `IndexPageTests` bileşen tümleştirme dizin sayfası için test etmek için). Bir MVC uygulamasında testleri genellikle denetleyicisi sınıfları tarafından düzenlenen ve bunlar test denetleyicilerini sonra adlı (örneğin, `HomeControllerTests` bileşen tümleştirme giriş denetleyicisi için test etmek için).

## <a name="test-app-prerequisites"></a>Test uygulama önkoşulları

Oluşturduğunuz test projesinin gerekir:

* Bir paket başvurusunu sahip [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Proje dosyasında Web SDK'sını kullanın (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Bu prerequesities görülebilir [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). İnceleme *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* dosya.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Varsayılan WebApplicationFactory ile temel testleri

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) oluşturmak için kullanılan bir [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) tümleştirme testleri için. `TEntryPoint` Giriş noktası SUT genellikle sınıfıdır `Startup` sınıfı.

Test sınıfları uygulayan bir *sınıfı donanımı* arabirimi (`IClassFixture`) sınıf testleri içerir ve sınıf testlerinde arasında paylaşılan nesne örnekleri sağlayın belirtmek için.

### <a name="basic-test-of-app-endpoints"></a>Uygulama uç noktaları temel testi

Aşağıdaki sınıf, test `BasicTests`, kullanan `WebApplicationFactory` SUT bootstrap ve sağlamak için bir [HttpClient](/dotnet/api/system.net.http.httpclient) test yöntemine `Get_EndpointsReturnSuccessAndCorrectContentType`. Yöntem yanıt durum kodu başarılı olup olmadığını denetler (200 299 aralıktaki durum kodları) ve `Content-Type` üstbilgisi `text/html; charset=utf-8` birkaç uygulama sayfaları için.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) bir örneğini oluşturur `HttpClient` otomatik olarak yeniden yönlendirmeleri izler ve tanımlama bilgilerini işler.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Güvenli bir uç nokta test

Başka bir testte `BasicTests` sınıfı denetler güvenli bir uç noktası kimliği doğrulanmamış bir kullanıcı uygulamanın oturum açma sayfasına yönlendirir.

SUT içinde `/SecurePage` sayfasında kullanan bir [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) uygulamak için kuralı bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) sayfasına. Daha fazla bilgi için bkz: [Razor sayfalarının yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

İçinde `Get_SecurePageRequiresAnAuthenticatedUser` sınamak, bir [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) ayarlayarak yeniden yönlendirmeleri izin vermeyecek şekilde ayarlanmış [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) için `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Yeniden yönlendirme izlemek için istemci engelleyerek aşağıdaki denetimleri yapılabilir:

* SUT tarafından döndürülen durum kodu denetlenebilir beklenen karşı [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) sonucu, değil olacaktır oturum açma sayfasına yeniden yönlendir sonra son durum kodu [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location` Üstbilgi değeri yanıt üst bilgileri ile başlayan onaylamak için işaretli `http://localhost/Identity/Account/Login`, değil son oturum açma sayfası yanıt, burada `Location` üstbilgi olmayacaktır mevcut olmalı.

Daha fazla bilgi için `WebApplicationFactoryClientOptions`, bkz: [istemci seçenekleri](#client-options) bölümü.

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory özelleştirme

Web ana bilgisayar yapılandırması oluşturulabilir bağımsız olarak test sınıfları devralarak `WebApplicationFactory` bir veya daha fazla özel oluşturucuları oluşturmak için:

1. Devralınan `WebApplicationFactory` ve geçersiz kılma [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) hizmet koleksiyonuyla yapılandırılmasını sağlar [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Veritabanı içinde dengeli [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) tarafından gerçekleştirilen `InitializeDbForTests` yöntemi. Yöntem açıklanan [tümleştirme testleri örnek: Test uygulama kuruluş](#test-app-organization) bölümü.

2. Özel kullanmanın `CustomWebApplicationFactory` test sınıflardaki. Aşağıdaki örnek, fabrikada kullanır `IndexPageTests` sınıfı:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Örnek uygulamanın istemci engellemek için yapılandırılmış `HttpClient` gelen aşağıdaki yeniden yönlendirir. İçinde anlatıldığı gibi [güvenli bir uç nokta Test](#test-a-secure-endpoint) bölümü, bu izin verir uygulamanın ilk yanıt sonucu denetlemek için testleri. İlk yanıt birçok bu testleri ile bir yeniden yönlendirme olan bir `Location` üstbilgi.

3. Tipik bir test kullanır `HttpClient` ve istek ve yanıt işlemek için yardımcı yöntemler:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Herhangi bir POST isteği SUT için otomatik olarak uygulamanın tarafından yapılan antiforgery onay getirmelidir [veri koruması antiforgery sistemi](xref:security/data-protection/introduction). Bir testin POST isteği için düzenlemek için uygulamayı sınayın gerekir:

1. Sayfa için bir istek olun.
1. Antiforgery tanımlama bilgisi ve istek doğrulama belirtecinden yanıtı ayrıştıramadı.
1. Antiforgery tanımlama bilgisi ve istek doğrulama POST isteğini yerinde belirteci olun.

`SendAsync` Yardımcı genişletme yöntemleri (*Helpers/HttpClientExtensions.cs*) ve `GetDocumentAsync` yardımcı yöntemi (*Helpers/HtmlHelpers.cs*) içinde [örnekuygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) kullanmak [AngleSharp](https://anglesharp.github.io/) antiforgery onay aşağıdaki yöntemlerle işlemek için ayrıştırıcı:

* `GetDocumentAsync` &ndash; Alan [httpresponsemessage öğesini](/dotnet/api/system.net.http.httpresponsemessage) ve döndüren bir `IHtmlDocument`. `GetDocumentAsync` hazırlayan bir Fabrika kullanan bir *sanal yanıt* özgün göre `HttpResponseMessage`. Daha fazla bilgi için bkz: [AngleSharp belgelerine](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` için genişletme yöntemleri `HttpClient` oluşturan bir [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) ve arama [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) SUT istekleri göndermek için. İçin overloads `SendAsync` HTML formu kabul (`IHtmlFormElement`) ve aşağıdaki:
  - Gönder düğmesi biçiminde (`IHtmlElement`)
  - Form değerleri koleksiyonunu (`IEnumerable<KeyValuePair<string, string>>`)
  - Gönder düğmesi (`IHtmlElement`) ve form değerleri (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) olan üçüncü taraf ayrıştırma Bu konu ve örnek uygulaması tanıtım amacıyla kullanılan kitaplığı. AngleSharp desteklenen veya tümleştirme ASP.NET Core uygulamaları test etmek için gerekli değildir. Diğer ayrıştırıcıları, gibi kullanılabilir [Html çeviklik paketi (HAP)](http://html-agility-pack.net/). Başka bir yaklaşım antiforgery sistemin istek Doğrulama belirtecini ve antiforgery tanımlama bilgisi doğrudan işlemek için kod yazmaktır.

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder istemcisiyle özelleştirme

Test yöntemi içinde ek yapılandırma gerektiğinde [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) yeni bir `WebApplicationFactory` ile bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , daha fazla özelleştirilmiş tarafından yapılandırma.

`Post_DeleteMessageHandler_ReturnsRedirectToRoot` Test yöntemi [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) kullanımını gösteren `WithWebHostBuilder`. Bu test, SUT form gönderisine tetikleme tarafından veritabanında bir kayıt silme gerçekleştirir.

Çünkü başka bir test `IndexPageTests` sınıfı veritabanındaki kayıtların tümünü siler ve önce çalışabilir bir işlem gerçekleştirir `Post_DeleteMessageHandler_ReturnsRedirectToRoot` yöntemi, veritabanı sağlanmış bu test yönteminde bir kayıt SUT silmek mevcut olduğundan emin olun. Seçme `deleteBtn1` düğmesine `messages` SUT isteğinde SUT formunda benzetimli:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>İstemci seçenekleri

Aşağıdaki tabloda, varsayılan gösterilmektedir [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) oluştururken kullanılabilir `HttpClient` örnekleri.

| Seçenek | Açıklama | Varsayılan |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Alır veya ayarlar desteklemediğini `HttpClient` örnekleri otomatik olarak yeniden yönlendirme yanıtlarını izlemelidir. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Taban adresi alır veya ayarlar `HttpClient` örnekleri. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Alır veya ayarlar olup olmadığını `HttpClient` örnekleri tanımlama bilgilerini işleme. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Alır veya yeniden yönlendirme yanıtlarını üst sınırını ayarlar `HttpClient` örnekleri izlemelidir. | 7 |

Oluşturma `WebApplicationFactoryClientOptions` sınıfı ve ona geçirin [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) yöntemi (varsayılan değerler, aşağıdaki kod örneğinde gösterilir):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Uygulama içerik kök yolu test altyapısını nasıl oluşturur

`WebApplicationFactory` Oluşturucusu oluşturur uygulama içerik kök yolu için arama yaparak bir [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) eşit bir anahtarla tümleştirme testleri içeren derlemenin üzerinde `TEntryPoint` derleme `System.Reflection.Assembly.FullName`. Bir öznitelik doğru anahtarla bulunamadığında durumda `WebApplicationFactory` bir çözüm dosyası için arama yapmayı geri döner (*\*.sln*) ve ekler `TEntryPoint` çözüm dizini için derleme adı. Uygulama kök dizini (içerik kök yolu), görünümler ve içerik dosyalarını bulmak için kullanılır.

Çoğu durumda, daha arama mantığı çalışma zamanında doğru içerik kök genellikle buldukça bile uygulama içerik kök açıkça ayarlamak gerekli değildir. İçerik kök bulunduğu değil özel senaryolarda yerleşik arama algoritması, kök açıkça veya Özel mantık kullanarak belirtilen içerik uygulama kullanıyor. Bu senaryolarda uygulama içerik kök ayarlamak için arama `UseSolutionRelativeContentRoot` uzantısı yönteminden [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) paket. Çözümün göreli yol ve isteğe bağlı çözüm dosya adı veya glob deseni sağlayın (varsayılan = `*.sln`).

Çağrı [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) uzantısı yöntemiyle *bir* aşağıdaki yaklaşımlardan:

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

* Özel bir test sınıflarını yapılandırırken `WebApplicationFactory`, devralınan `WebApplicationFactory` ve geçersiz kılma [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

Gölge kopyalama çıkış klasörü değerinden farklı bir klasörde yürütmek için testleri neden olur. Gölge kopyalama düzgün çalışması testleri için devre dışı bırakılmalıdır. [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit kullanır ve ekleyerek xUnit için gölge kopyalama devre dışı bırakan bir *xunit.runner.json* doğru yapılandırma ayarı dosyası. Daha fazla bilgi için bkz: [xUnit.net JSON ile yapılandırma](https://xunit.github.io/docs/configuring-with-json.html).

Ekleme *xunit.runner.json* aşağıdaki içeriğe sahip test projesinin kök dosyaya:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Tümleştirme testleri örneği

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) iki uygulamaların oluşur:

| Uygulama | Proje klasörü | Açıklama |
| --- | -------------- | ----------- |
| İleti Uygulama (SUT) | *src/RazorPagesProject* | Bir kullanıcı eklemek, silmek, tüm silin ve iletileri çözümlemek sağlar. |
| Test uygulama | *tests/RazorPagesProject.Tests* | Tümleştirme testine SUT kullanılan. |

Testleri bir IDE yerleşik test özelliklerini kullanarak çalıştırabilirsiniz [Visual Studio](https://www.visualstudio.com/vs/). Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırı *tests/RazorPagesProject.Tests* klasörü:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>İleti Uygulama (SUT) kuruluş

SUT, aşağıdaki özelliklere sahip bir Razor sayfalarının ileti sistemidir:

* Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) kullanıcı Arabirimi ve sayfa ekleme, silme ve iletileri (ileti başına ortalama sözcük) analizini denetlemek için modeli yöntemleri sağlar .
* Bir ileti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliklere sahip: `Id` (anahtar) ve `Text` (ileti). `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletileri kullanarak depolanır [Entity Framework'ün bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;.
* Uygulama bir veri erişim katmanı (DAL), veritabanı bağlamı sınıfının içeren `AppDbContext` (*Data/AppDbContext.cs*).
* Veritabanı uygulama başlangıçta boş ise, ileti deposunu üç iletileri ile başlatıldı.
* Uygulamasını içeren bir `/SecurePage` , yalnızca erişilebilir tarafından kimliği doğrulanmış bir kullanıcı.

&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), mstest'i testler için bir bellek içi veritabanı kullanımı açıklanmaktadır. Bu konuda kullanan [xUnit](https://xunit.github.io/) test çerçevesi. Test kavramları ve test uygulamaları farklı bir test çerçevelerini arasında benzer, ancak aynı değildir.

Uygulama kullanmayan ancak [havuz deseni](http://martinfowler.com/eaaCatalog/repository.html) ve etkili bir örneği değil. [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfalarının geliştirme bu desenleri destekler. Daha fazla bilgi için bkz: [altyapı saklama katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [depo ve iş desenleri bir ASP.NET MVC uygulamasındaki uygulama](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek havuz deseni uygular).

### <a name="test-app-organization"></a>Test uygulama kuruluş

İçinde bir konsol uygulaması test uygulamadır *tests/RazorPagesProject.Tests* klasör.

| Test uygulama klasörü | Açıklama |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* yönlendirme, güvenli bir sayfa kimliği doğrulanmamış bir kullanıcı tarafından erişmek ve GitHub kullanıcı profili alma ve profilin kullanıcı oturum açma denetimi test yöntemleri içerir. |
| *IntegrationTests* | *IndexPageTests.cs* özel kullanarak dizin sayfası tümleştirme testleri içeren `WebApplicationFactory` sınıfı. |
| *Yardımcıları/yardımcı programları* | <ul><li>*Utilities.cs* içeren `InitializeDbForTests` test verileri ile veritabanı oluşturmak için kullanılan yöntem.</li><li>*HtmlHelpers.cs* bir AngleSharp döndürmek için bir yöntem sağlar `IHtmlDocument` test yöntemleri tarafından kullanım için.</li><li>*HttpClientExtensions.cs* sağlamak için aşırı `SendAsync` SUT istekleri göndermek için.</li></ul> |

Test çerçevesi [xUnit](https://xunit.github.io/). Tümleştirme testleri gerçekleştirilen kullanarak [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), içeren [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Çünkü [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paket test konak ve test sunucusunu yapılandırmak için kullanılan `TestHost` ve `TestServer` paketi doğrudan paket referanslarını test uygulamanın proje dosyasında gerektirir yok veya test uygulamasında Geliştirici yapılandırması.

**Test veritabanı üretme**

Tümleştirme testleri test yürütülmeden önce veritabanında küçük bir veri kümesini genellikle gerektirir. Örneğin, bir silme test çağrıları veritabanı kayıt silme için veritabanı silme isteği başarılı olması için en az bir kaydı olabilir.

Örnek uygulamayı üç iletilerinde veritabanıyla çekirdeğini oluşturur *Utilities.cs* bunlar yürüttüğünüzde testleri kullanabilirsiniz:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor sayfalarının birim testleri](xref:test/razor-pages-tests)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Test denetleyicileri](xref:mvc/controllers/testing)
