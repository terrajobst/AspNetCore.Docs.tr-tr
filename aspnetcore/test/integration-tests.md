---
title: ASP.NET Core tümleştirme testleri
author: guardrex
description: Tümleştirme testlerinin, bir uygulamanın bileşenlerinin, veritabanı, dosya sistemi ve ağ dahil olmak üzere altyapı düzeyinde doğru şekilde çalışmasını nasıl sağladığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: test/integration-tests
ms.openlocfilehash: 2825073962d135608c52e7bde42106e7786de521
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007461"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core tümleştirme testleri

[Luke Latham](https://github.com/guardrex) ve [Steve Smith](https://ardalis.com/) tarafından

::: moniker range=">= aspnetcore-3.0"

Tümleştirme sınamaları, uygulamanın bileşenlerinin veritabanı, dosya sistemi ve ağ gibi destekleyici altyapısını içeren bir düzeyde doğru şekilde çalışmasını güvence altına alır. ASP.NET Core, test Web ana bilgisayarı ve bellek içi test sunucusu olan bir birim testi çerçevesini kullanarak tümleştirme testlerini destekler.

Bu konuda, birim testlerinin temel bir şekilde anlaşıldığı varsayılır. Test kavramları hakkında bilgi sahibi değilseniz, [.NET Core 'Da birim testine ve .NET Standard](/dotnet/core/testing/) konusuna ve bağlı içeriğine bakın.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama bir Razor Pages uygulamasıdır ve Razor Pages temel bir anlama sahip olduğunu varsayar. Razor Pages hakkında bilginiz yoksa, aşağıdaki konulara bakın:

* [Razor Pages’e giriş](xref:razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)

> [!NOTE]
> Maça 'Ları test etmek için, bir tarayıcıyı otomatikleştirebilen [Selenium](https://www.seleniumhq.org/)gibi bir araç öneririz.

## <a name="introduction-to-integration-tests"></a>Tümleştirme testlerine giriş

Tümleştirme testleri, bir uygulamanın bileşenlerini [birim testlerinden](/dotnet/core/testing/)daha geniş bir düzeyde değerlendirir. Birim testleri, ayrı sınıf yöntemleri gibi yalıtılmış yazılım bileşenlerini test etmek için kullanılır. Tümleştirme testleri iki veya daha fazla uygulama bileşeninin beklenen bir sonuç üretmek için birlikte çalıştığını ve muhtemelen bir isteği tam olarak işlemek için gereken her bileşeni de dahil olduğunu onaylar.

Bu geniş testler, uygulamanın altyapısını ve tüm çatısını test etmek için kullanılır, genellikle aşağıdaki bileşenler dahil:

* Database
* Dosya sistemi
* Ağ gereçleri
* İstek-yanıt işlem hattı

Birim testleri, altyapı bileşenlerinin yerine, *Fakes* veya *sahte nesneler*olarak bilinen fabriccomponents bileşenlerini kullanır.

Birim testlerinin aksine, tümleştirme testleri:

* Uygulamanın üretimde kullandığı gerçek bileşenleri kullanın.
* Daha fazla kod ve veri işleme gerektir.
* Çalıştırmak daha uzun sürer.

Bu nedenle, tümleştirme testlerinin kullanımını en önemli altyapı senaryolarıyla sınırlayın. Bir davranış, birim testi veya tümleştirme testi kullanılarak test edilebilir ise, birim testini seçin.

> [!TIP]
> Veritabanları ve dosya sistemleriyle ilgili her olası veri ve dosya erişimi için tümleştirme testleri yazma. Bir uygulamadaki kaç yerde veritabanlarıyla ve dosya sistemleriyle etkileşime girdiğinize bakılmaksızın, bir dizi Read, Write, Update ve DELETE tümleştirme testi, genellikle veritabanı ve dosya sistemi bileşenlerinin yeterli şekilde test edilmesine sahip olur. Bu bileşenlerle etkileşime geçen Yöntem mantığının rutin testleri için birim testleri kullanın. Birim testlerinde, altyapı kullanımı ve moizler 'in kullanılması daha hızlı test yürütmesiyle sonuçlanır.

> [!NOTE]
> Tümleştirme testlerinin tartışmalarında, test edilen proje genellikle *test altındaki sistem*veya kısa IÇIN "sut" olarak adlandırılır.
>
> *Bu konu başlığı altında, sınanan ASP.NET Core uygulamasına başvurmak için "SUT" kullanılır.*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core tümleştirme testleri

ASP.NET Core tümleştirme testleri şunları gerektirir:

* Testleri içermesi ve yürütmek için bir test projesi kullanılır. Test projesinin SUT 'a bir başvurusu vardır.
* Test projesi SUT için bir test Web Konağı oluşturur ve SUT ile istekleri ve yanıtları işlemek için bir test sunucusu istemcisi kullanır.
* Test Çalıştırıcısı, testleri yürütmek ve test sonuçlarını raporlamak için kullanılır.

Tümleştirme testleri, olağan *düzenleme*, *hareket*ve *onaylama* testi adımlarını içeren bir olay dizisini izler:

1. SUT 'un web ana bilgisayarı yapılandırıldı.
1. İstekleri uygulamaya göndermek için bir test sunucusu istemcisi oluşturulur.
1. *Düzenleme* testi adımı yürütülür: Test uygulaması bir istek hazırlar.
1. *Davran* test adımı yürütülür: İstemci isteği gönderir ve yanıtını alır.
1. *Onaylama* testi adımı yürütülür: *Gerçek* yanıt, *beklenen* bir yanıta bağlı olarak *başarılı* veya *başarısız* olarak onaylanır.
1. İşlem, tüm testler yürütülene kadar devam eder.
1. Test sonuçları raporlanır.

Genellikle, test Web ana bilgisayarı, test çalıştırmaları için uygulamanın normal web ana bilgisayarında farklı şekilde yapılandırılır. Örneğin, testler için farklı bir veritabanı veya farklı uygulama ayarları kullanılıyor olabilir.

Test Web Konağı ve bellek içi test sunucusu ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)) gibi altyapı bileşenleri, [Microsoft. Aspnetcore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketi tarafından sağlanır veya yönetilir. Bu paketin kullanımı, test oluşturma ve yürütmeyi kolaylaştırır.

@No__t-0 paketi aşağıdaki görevleri gerçekleştirir:

* Bağımlılıklar dosyasını ( *. Deps*) sut 'den test projesinin *bin* dizinine kopyalar.
* , Testler yürütüldüğünde statik dosya ve sayfa/görünümlerin bulunması için [içerik kökünü](xref:fundamentals/index#content-root) sut 'nin proje köküne ayarlar.
* @No__t-1 ile SUT önyükleyiciyi kolaylaştırmak için [Webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) sınıfını sağlar.

[Birim](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) testi belgeleri bir test projesi ve Test Çalıştırıcısı ayarlamayı ve testlerin ve test sınıflarının nasıl belirleneceğini gösteren testlerin ve önerilerin nasıl çalıştırılacağını gösteren ayrıntılı yönergelerle birlikte açıklanır.

> [!NOTE]
> Bir uygulama için test projesi oluştururken, birim testlerini tümleştirme testlerinden farklı projelere ayırın. Bu, altyapı testi bileşenlerinin birim testlerine yanlışlıkla dahil olmamasını sağlamaya yardımcı olur. Birim ve tümleştirme testlerinin ayrımı, hangi test kümesinin çalıştırılmasına da izin verir.

Razor Pages Apps ve MVC uygulamalarının testleri için yapılandırma arasında neredeyse fark yoktur. Tek fark testlerin adlandırılmasınlardır. Razor Pages uygulamasında, sayfa uç noktalarının testleri genellikle sayfa modeli sınıfından sonra adlandırılır (örneğin, dizin sayfasına yönelik bileşen tümleştirmesini test etmek için `IndexPageTests`). MVC uygulamasında testler genellikle denetleyici sınıfları tarafından düzenlenir ve test ettikleri denetleyiciler (örneğin, ana denetleyicinin bileşen tümleştirmesini test etmek için `HomeControllerTests`).

## <a name="test-app-prerequisites"></a>Test uygulaması önkoşulları

Test projesi şunları vermelidir:

* [Microsoft. AspNetCore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketine başvurun.
* Proje dosyasında Web SDK 'sını belirtin (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Bu Önkoşullar [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)görülebilir. *Testler/RazorPagesProject. Tests/RazorPagesProject. Tests. csproj* dosyasını inceleyin. Örnek uygulama, [xUnit](https://xunit.github.io/) test çerçevesini ve [anglesharp](https://anglesharp.github.io/) Parser kitaplığını kullanır, bu nedenle örnek uygulama de şu şekilde başvurur:

* [xUnit](https://www.nuget.org/packages/xunit)
* [xUnit. Çalıştırıcısı. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp)

Entity Framework Core, testlerde de kullanılır. Uygulama başvuru:

* [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [Microsoft. AspNetCore. Identity. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a>SUT ortamı

SUT [ortamı](xref:fundamentals/environments) ayarlanmamışsa, ortam varsayılan olarak geliştirme olur.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Varsayılan WebApplicationFactory ile temel testler

[Webapplicationfactory @ no__t-1TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) , tümleştirme testleri Için bir [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) oluşturmak üzere kullanılır. `TEntryPoint`, genellikle `Startup` sınıfının SUT giriş noktası sınıfıdır.

Test sınıfları sınıfı testlerin içerdiğini göstermek ve sınıftaki testler arasında paylaşılan nesne örnekleri sağlamak için bir *sınıf armatürü* arabirimi ([ıssfixture](https://xunit.github.io/docs/shared-context#class-fixture)) uygular.

### <a name="basic-test-of-app-endpoints"></a>Uygulama uç noktalarının temel testi

Aşağıdaki `BasicTests` test sınıfı, SUT 'yi önyüklemek ve bir test yöntemine [HttpClient](/dotnet/api/system.net.http.httpclient) sağlamak için `WebApplicationFactory` kullanır `Get_EndpointsReturnSuccessAndCorrectContentType`. Yöntemi, yanıt durum kodunun başarılı olup olmadığını denetler (200-299 aralığındaki durum kodları) ve `Content-Type` üstbilgisi çeşitli uygulama sayfaları için `text/html; charset=utf-8` ' dir.

[Createclient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) , yeniden yönlendirmeleri otomatik olarak izleyen ve tanımlama bilgilerini işleyen bir `HttpClient` örneği oluşturur.

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Varsayılan olarak, [GDPR onay ilkesi](xref:security/gdpr) etkinleştirildiğinde, önemli olmayan tanımlama bilgileri istekler arasında korunmaz. TempData sağlayıcısı tarafından kullanılanlar gibi, önemli olmayan tanımlama bilgilerini korumak için, bunları testlerinizde gerekli olarak işaretleyin. Tanımlama bilgisini önemli olarak işaretleme hakkında yönergeler için bkz. [temel tanımlama bilgileri](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Güvenli uç noktayı test etme

@No__t-0 sınıfındaki başka bir test, güvenli bir uç noktanın, kimliği doğrulanmamış bir kullanıcıyı uygulamanın oturum açma sayfasına yönlendirdiğini denetler.

SUT 'de `/SecurePage` sayfası, sayfaya bir [Authorizefilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) uygulamak Için bir [authorizepage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı kullanır. Daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

@No__t-0 testinde, bir [Webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) , [allowoto redirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) ' ı `false` ' e ayarlayarak yeniden yönlendirmeye izin vermez olarak ayarlanır:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

İstemcinin yeniden yönlendirmeyi izlemeye izin vererek aşağıdaki denetimler yapılabilir:

* SUT tarafından döndürülen durum kodu, oturum açma sayfasına yeniden yönlendirmeden sonra, [HttpStatusCode. Tamam](/dotnet/api/system.net.httpstatuscode)olacak şekilde, son durum koduna değil beklenen [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) sonucuyla denetlenebilir.
* Yanıt üst bilgilerindeki `Location` üst bilgi değeri, `Location` üstbilgisinin mevcut olmadığı son oturum açma sayfası yanıtıyla değil `http://localhost/Identity/Account/Login` ile başlayacağını doğrulamak üzere denetlenir.

@No__t-0 hakkında daha fazla bilgi için bkz. [istemci seçenekleri](#client-options) bölümü.

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory 'yi özelleştirme

Web ana bilgisayar yapılandırması, bir veya daha fazla özel fabrika oluşturmak için `WebApplicationFactory` ' dan devralarak test sınıflarından bağımsız olarak oluşturulabilir:

1. @No__t-0 ' dan devralma ve [Configurewebhost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)'i geçersiz kılma. [Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , hizmet koleksiyonunun [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)ile yapılandırılmasına izin verir:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) veritabanı dengeli dağıtımı `InitializeDbForTests` yöntemi tarafından gerçekleştirilir. Yöntemi [ıntegration Tests örneğinde açıklanmıştır: Test uygulaması kuruluş @ no__t-0 bölümü.

2. Test sınıflarında özel `CustomWebApplicationFactory` kullanın. Aşağıdaki örnek `IndexPageTests` sınıfında fabrikası kullanır:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Örnek uygulamanın istemcisi, aşağıdaki yeniden yönlendirmelere `HttpClient` ' i engelleyecek şekilde yapılandırılmıştır. [Güvenli uç nokta testi](#test-a-secure-endpoint) bölümünde açıklandığı gibi, bu, testlerin uygulamanın ilk yanıtının sonucunu denetlemesini sağlar. İlk yanıt, `Location` üst bilgisiyle bu testlerin çoğunda bir yeniden yönlendirmelidir.

3. Tipik bir test, isteği ve yanıtı işlemek için `HttpClient` ve yardımcı yöntemlerini kullanır:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

SUT 'a yönelik herhangi bir POST isteği, uygulamanın [veri koruma antiforgery sistemi](xref:security/data-protection/introduction)tarafından otomatik olarak oluşturulan antiforgery denetimini karşılamalıdır. Bir testin POST isteğini düzenlemek için test uygulaması şunları kullanmalıdır:

1. Sayfa için bir istek oluşturun.
1. Antiforgery tanımlama bilgisini ayrıştırın ve yanıt doğrulama belirtecini istekten isteyin.
1. POST isteğini, antiforgery tanımlama bilgisiyle ve istek doğrulama belirteciyle birlikte yapın.

[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) `SendAsync` yardımcı uzantı yöntemleri (*yardımcılar/Httpclienconversionsions. cs*) ve `GetDocumentAsync` yardımcı yöntemi (*yardımcılar/htmlyardımcıları. cs* [),](https://anglesharp.github.io/) Aşağıdaki Yöntemler:

* `GetDocumentAsync` &ndash; [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) 'yi alır ve bir `IHtmlDocument` döndürür. `GetDocumentAsync`, özgün @no__t 2 ' ye dayalı olarak bir *sanal yanıt* hazırlayan bir fabrika kullanır. Daha fazla bilgi için bkz. [Anglesharp belgeleri](https://github.com/AngleSharp/AngleSharp#documentation).
* `HttpClient` için `SendAsync` genişletme yöntemleri bir [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) oluşturun ve istekleri sut 'e göndermek Için [sendadsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) çağrısı yapın. @No__t için aşırı yüklemeler-0 HTML formunu kabul eder (`IHtmlFormElement`) ve şunları yapın:
  * Formun Gönder düğmesi (`IHtmlElement`)
  * Form değerleri koleksiyonu (`IEnumerable<KeyValuePair<string, string>>`)
  * Gönder düğmesi (`IHtmlElement`) ve form değerleri (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [Anglesharp](https://anglesharp.github.io/) , bu konuda ve örnek uygulamada Gösterim amacıyla kullanılan bir üçüncü taraf ayrıştırma kitaplığıdır. ASP.NET Core uygulamalarının tümleştirme testi için AngleSharp desteklenmez veya gerekli değildir. Diğer çözümleyiciler, [HTML çevikliği paketi (HAP)](https://html-agility-pack.net/)gibi kullanılabilir. Diğer bir yaklaşım ise, antiforgery sisteminin istek doğrulama belirtecini ve antiforgery tanımlama bilgisini doğrudan işlemek için kod yazmaktır.

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder ile istemciyi özelleştirme

Bir test yönteminde ek yapılandırma gerektiğinde, [Withwebhostbuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) , yapılandırmaya göre daha fazla özelleştirilmiş bir [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ile yeni bir `WebApplicationFactory` oluşturur.

[Örnek uygulamanın](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test yöntemi, `WithWebHostBuilder` kullanımını gösterir. Bu test, SUT 'da form gönderimini tetikleyerek veritabanında silme işlemini gerçekleştirir.

@No__t-0 sınıfındaki başka bir test veritabanındaki tüm kayıtları silen ve `Post_DeleteMessageHandler_ReturnsRedirectToRoot` yönteminden önce çalışabilen bir işlem gerçekleştirdiğinden, SUT 'in silinmesine yönelik bir kaydın mevcut olduğundan emin olmak için veritabanı bu test yönteminde yeniden oluşturulur. SUT içindeki `messages` formunun ilk Sil düğmesini seçmek, SUT isteğine göre benzetilir:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>İstemci seçenekleri

Aşağıdaki tabloda `HttpClient` örnekleri oluşturulurken kullanılabilen varsayılan [Webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) gösterilmektedir.

| Seçenek | Açıklama | Varsayılan |
| ------ | ----------- | ------- |
| [Allowoto yeniden yönlendirme](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | @No__t-0 örneklerinin yeniden yönlendirme yanıtlarını otomatik olarak izlemesi gerekip gerekmediğini alır veya ayarlar. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | @No__t-0 örneklerinin temel adresini alır veya ayarlar. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | @No__t-0 örneklerinin tanımlama bilgilerini işlemesinin gerekip gerekmediğini alır veya ayarlar. | `true` |
| [Maxautomaticyönlendirmeler](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | @No__t-0 örneklerinin izlemesi gereken en fazla yeniden yönlendirme yanıtı sayısını alır veya ayarlar. | 7 |

@No__t-0 sınıfını oluşturun ve [Createclient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) yöntemine geçirin (varsayılan değerler kod örneğinde gösterilir):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Sahte hizmetler ekleme

Hizmetler, ana bilgisayar tasarımcısında [Configuretestservices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) çağrısıyla bir testte geçersiz kılınabilir. **Sahte hizmetleri eklemek için SUT, `Startup.ConfigureServices` yöntemine sahip `Startup` sınıfına sahip olmalıdır.**

Örnek SUT, bir teklif döndüren kapsamlı bir hizmet içerir. Dizin sayfası istendiğinde, teklif Dizin sayfasındaki gizli bir alana katıştırılır.

*Hizmetler/ıquoteservice. cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Hizmetler/QuoteService. cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index. cshtml. cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Sayfa/dizin. cs*:

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Aşağıdaki biçimlendirme, SUT uygulaması çalıştırıldığında oluşturulur:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Bir tümleştirme testinde hizmet ve teklif ekleme işlemini test etmek için, test tarafından SUT 'ye bir sahte hizmet eklenir. Sahte hizmet, uygulamanın `QuoteService` ' ı, `TestQuoteService` adlı test uygulaması tarafından verilen bir hizmetle değiştirir:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` çağrılır ve kapsamlı hizmet kaydedilir:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Testin yürütülmesi sırasında üretilen biçimlendirme `TestQuoteService` tarafından sağlanan tırnak metnini yansıtır, bu nedenle onaylama işlemi geçirilir:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Test altyapısının uygulama içeriği kök yolunu nasıl öğrendiğini

@No__t-0 Oluşturucusu, `TEntryPoint` derlemesine `System.Reflection.Assembly.FullName` ' e eşit bir anahtarla tümleştirme testlerini içeren derlemede bir [Webapplicationfactorycontentrootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) öğesini arayarak uygulama [içeriği kök](xref:fundamentals/index#content-root) yolunu alır. Doğru anahtara sahip bir özniteliğin bulunamaması durumunda, `WebApplicationFactory` bir çözüm dosyasını ( *. sln*) aramaya geri döner ve `TEntryPoint` derleme adını çözüm dizinine ekler. Uygulama kök dizini (içerik kök yolu) görünümleri ve içerik dosyalarını saptamak için kullanılır.

## <a name="disable-shadow-copying"></a>Gölge kopyalamayı devre dışı bırak

Gölge kopyalama, testlerin çıkış dizininden farklı bir dizinde yürütülmesine neden olur. Testlerin düzgün çalışması için gölge kopyalama devre dışı bırakılmalıdır. [Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit kullanır ve doğru yapılandırma ayarıyla bir *xUnit. Runner. JSON* dosyası ekleyerek xUnit için gölge kopyalamayı devre dışı bırakır. Daha fazla bilgi için bkz. [JSON Ile xUnit yapılandırma](https://xunit.github.io/docs/configuring-with-json.html).

Aşağıdaki içeriğe sahip test projesinin köküne *xUnit. Runner. JSON* dosyasını ekleyin:

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>Nesnelerin elden çıkarılması

@No__t-0 uygulamasının testleri yürütüldükten sonra, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) ve [HttpClient](/dotnet/api/system.net.http.httpclient) , [webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)'nin xUnit 'i çıkardığı zaman yürütülür. Geliştirici tarafından oluşturulan nesneler için aktiften çıkarma gerekiyorsa, bunları `IClassFixture` uygulamasında atın. Daha fazla bilgi için bkz. [Dispose yöntemi uygulama](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Tümleştirme Testleri örneği

[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) iki uygulamalardan oluşur:

| Uygulama | Proje dizini | Açıklama |
| --- | ----------------- | ----------- |
| İleti uygulaması (SUT) | *src/RazorPagesProject* | Bir kullanıcının, iletileri eklemesini, silmesini, silmesini ve analiz etmesini sağlar. |
| Test uygulaması | *testler/RazorPagesProject. testler* | SUT test tümleştirmesi için kullanılır. |

Testler, [Visual Studio](https://visualstudio.microsoft.com)gıbı bir IDE 'nin yerleşik test özellikleri kullanılarak çalıştırılabilir. [Visual Studio Code](https://code.visualstudio.com/) veya komut satırı kullanıyorsanız, *testler/RazorPagesProject. Tests* dizinindeki bir komut isteminde aşağıdaki komutu yürütün:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>İleti uygulaması (SUT) kuruluşu

SUT, aşağıdaki özelliklere sahip bir Razor Pages ileti sistemidir:

* Uygulamanın (*Pages/Index. cshtml* ve *Pages/index. cshtml. cs*) dizin sayfası, iletilerin eklenmesi, silinmesini ve ANALIZINI denetlemek için bir UI ve sayfa modeli yöntemleri sağlar (ileti başına ortalama sözcük).
* Bir ileti, iki özellik içeren `Message` sınıfı (*Data/Message. cs*) tarafından tanımlanır: `Id` (anahtar) ve `Text` (ileti). @No__t-0 özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletiler, [Entity Framework bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;kullanılarak depolanır.
* Uygulama, `AppDbContext` (*Data/AppDbContext. cs*) veritabanı bağlamı sınıfında bir veri erişim KATMANı (dal) içerir.
* Veritabanı uygulama başlangıcında boşsa, ileti deposu üç iletiyle başlatılır.
* Uygulama yalnızca kimliği doğrulanmış bir kullanıcı tarafından erişilebilen bir `/SecurePage` içerir.

&#8224;[InMemory Ile test](/ef/core/miscellaneous/testing/in-memory)olan EF konusu, MSTest ile testler için bellek içi bir veritabanının nasıl kullanılacağını açıklar. Bu konu [xUnit](https://xunit.github.io/) test çerçevesini kullanır. Farklı test çerçeveleri genelinde test kavramları ve test uygulamaları benzerdir ancak aynı değildir.

Uygulama, depo desenini kullanmıyor ve [Iş birimi (UoW) düzeninin](https://martinfowler.com/eaaCatalog/unitOfWork.html)etkin bir örneği olmamasına karşın, Razor Pages bu geliştirme düzenlerini destekler. Daha fazla bilgi için bkz. [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek, depo modelini uygular).

### <a name="test-app-organization"></a>Test uygulaması kuruluşu

Test uygulaması, *testler/RazorPagesProject. Tests* dizini içindeki bir konsol uygulamasıdır.

| Uygulama dizinini test et | Açıklama |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* , yönlendirme için test yöntemleri, kimliği doğrulanmamış bir kullanıcı tarafından güvenli bir sayfaya erişmek ve bir GitHub Kullanıcı profili elde etmek ve profilin Kullanıcı oturum açma bilgilerini denetlemek içerir. |
| *Tümleştirme Testleri* | *IndexPageTests.cs* , özel `WebApplicationFactory` sınıfı kullanarak Dizin sayfası için tümleştirme testlerini içerir. |
| *Yardımcılar/yardımcı programlar* | <ul><li>*Utilities.cs* , veritabanını test verileriyle tohum için kullanılan `InitializeDbForTests` metodunu içerir.</li><li>*HtmlHelpers.cs* , test yöntemleri tarafından kullanılmak üzere AngleSharp `IHtmlDocument` döndüren bir yöntem sağlar.</li><li>*HttpClientExtensions.cs* , istekleri sut 'a göndermek için `SendAsync` için aşırı yüklemeler sağlar.</li></ul> |

Test çerçevesi [xUnit](https://xunit.github.io/)' dir. Tümleştirme testleri, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)içeren [Microsoft. Aspnetcore. testhost](/dotnet/api/microsoft.aspnetcore.testhost)kullanılarak yürütülür. Test ana bilgisayarı ve test sunucusunu yapılandırmak için [Microsoft. AspNetCore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketi kullanıldığından, `TestHost` ve `TestServer` paketleri test uygulamasının proje dosyasında ya da testteki geliştirici yapılandırmasında doğrudan paket başvuruları gerektirmez uygulamanızda.

**Test için veritabanının temelini sağlama**

Tümleştirme testleri genellikle veritabanında test yürütmeden önce küçük bir veri kümesi gerektirir. Örneğin, veritabanı kaydı silme için bir silme testi çağrılarında, silme isteğinin başarılı olması için veritabanının en az bir kaydı olmalıdır.

Örnek uygulama, *Utilities.cs* içinde testlerin, yürütme sırasında kullanabileceği üç iletiyle birlikte veritabanını kullanır:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Tümleştirme sınamaları, uygulamanın bileşenlerinin veritabanı, dosya sistemi ve ağ gibi destekleyici altyapısını içeren bir düzeyde doğru şekilde çalışmasını güvence altına alır. ASP.NET Core, test Web ana bilgisayarı ve bellek içi test sunucusu olan bir birim testi çerçevesini kullanarak tümleştirme testlerini destekler.

Bu konuda, birim testlerinin temel bir şekilde anlaşıldığı varsayılır. Test kavramları hakkında bilgi sahibi değilseniz, [.NET Core 'Da birim testine ve .NET Standard](/dotnet/core/testing/) konusuna ve bağlı içeriğine bakın.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama bir Razor Pages uygulamasıdır ve Razor Pages temel bir anlama sahip olduğunu varsayar. Razor Pages hakkında bilginiz yoksa, aşağıdaki konulara bakın:

* [Razor Pages’e giriş](xref:razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)

> [!NOTE]
> Maça 'Ları test etmek için, bir tarayıcıyı otomatikleştirebilen [Selenium](https://www.seleniumhq.org/)gibi bir araç öneririz.

## <a name="introduction-to-integration-tests"></a>Tümleştirme testlerine giriş

Tümleştirme testleri, bir uygulamanın bileşenlerini [birim testlerinden](/dotnet/core/testing/)daha geniş bir düzeyde değerlendirir. Birim testleri, ayrı sınıf yöntemleri gibi yalıtılmış yazılım bileşenlerini test etmek için kullanılır. Tümleştirme testleri iki veya daha fazla uygulama bileşeninin beklenen bir sonuç üretmek için birlikte çalıştığını ve muhtemelen bir isteği tam olarak işlemek için gereken her bileşeni de dahil olduğunu onaylar.

Bu geniş testler, uygulamanın altyapısını ve tüm çatısını test etmek için kullanılır, genellikle aşağıdaki bileşenler dahil:

* Database
* Dosya sistemi
* Ağ gereçleri
* İstek-yanıt işlem hattı

Birim testleri, altyapı bileşenlerinin yerine, *Fakes* veya *sahte nesneler*olarak bilinen fabriccomponents bileşenlerini kullanır.

Birim testlerinin aksine, tümleştirme testleri:

* Uygulamanın üretimde kullandığı gerçek bileşenleri kullanın.
* Daha fazla kod ve veri işleme gerektir.
* Çalıştırmak daha uzun sürer.

Bu nedenle, tümleştirme testlerinin kullanımını en önemli altyapı senaryolarıyla sınırlayın. Bir davranış, birim testi veya tümleştirme testi kullanılarak test edilebilir ise, birim testini seçin.

> [!TIP]
> Veritabanları ve dosya sistemleriyle ilgili her olası veri ve dosya erişimi için tümleştirme testleri yazma. Bir uygulamadaki kaç yerde veritabanlarıyla ve dosya sistemleriyle etkileşime girdiğinize bakılmaksızın, bir dizi Read, Write, Update ve DELETE tümleştirme testi, genellikle veritabanı ve dosya sistemi bileşenlerinin yeterli şekilde test edilmesine sahip olur. Bu bileşenlerle etkileşime geçen Yöntem mantığının rutin testleri için birim testleri kullanın. Birim testlerinde, altyapı kullanımı ve moizler 'in kullanılması daha hızlı test yürütmesiyle sonuçlanır.

> [!NOTE]
> Tümleştirme testlerinin tartışmalarında, test edilen proje genellikle *test altındaki sistem*veya kısa IÇIN "sut" olarak adlandırılır.
>
> *Bu konu başlığı altında, sınanan ASP.NET Core uygulamasına başvurmak için "SUT" kullanılır.*

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core tümleştirme testleri

ASP.NET Core tümleştirme testleri şunları gerektirir:

* Testleri içermesi ve yürütmek için bir test projesi kullanılır. Test projesinin SUT 'a bir başvurusu vardır.
* Test projesi SUT için bir test Web Konağı oluşturur ve SUT ile istekleri ve yanıtları işlemek için bir test sunucusu istemcisi kullanır.
* Test Çalıştırıcısı, testleri yürütmek ve test sonuçlarını raporlamak için kullanılır.

Tümleştirme testleri, olağan *düzenleme*, *hareket*ve *onaylama* testi adımlarını içeren bir olay dizisini izler:

1. SUT 'un web ana bilgisayarı yapılandırıldı.
1. İstekleri uygulamaya göndermek için bir test sunucusu istemcisi oluşturulur.
1. *Düzenleme* testi adımı yürütülür: Test uygulaması bir istek hazırlar.
1. *Davran* test adımı yürütülür: İstemci isteği gönderir ve yanıtını alır.
1. *Onaylama* testi adımı yürütülür: *Gerçek* yanıt, *beklenen* bir yanıta bağlı olarak *başarılı* veya *başarısız* olarak onaylanır.
1. İşlem, tüm testler yürütülene kadar devam eder.
1. Test sonuçları raporlanır.

Genellikle, test Web ana bilgisayarı, test çalıştırmaları için uygulamanın normal web ana bilgisayarında farklı şekilde yapılandırılır. Örneğin, testler için farklı bir veritabanı veya farklı uygulama ayarları kullanılıyor olabilir.

Test Web Konağı ve bellek içi test sunucusu ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)) gibi altyapı bileşenleri, [Microsoft. Aspnetcore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketi tarafından sağlanır veya yönetilir. Bu paketin kullanımı, test oluşturma ve yürütmeyi kolaylaştırır.

@No__t-0 paketi aşağıdaki görevleri gerçekleştirir:

* Bağımlılıklar dosyasını ( *. Deps*) sut 'den test projesinin *bin* dizinine kopyalar.
* , Testler yürütüldüğünde statik dosya ve sayfa/görünümlerin bulunması için [içerik kökünü](xref:fundamentals/index#content-root) sut 'nin proje köküne ayarlar.
* @No__t-1 ile SUT önyükleyiciyi kolaylaştırmak için [Webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) sınıfını sağlar.

[Birim](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) testi belgeleri bir test projesi ve Test Çalıştırıcısı ayarlamayı ve testlerin ve test sınıflarının nasıl belirleneceğini gösteren testlerin ve önerilerin nasıl çalıştırılacağını gösteren ayrıntılı yönergelerle birlikte açıklanır.

> [!NOTE]
> Bir uygulama için test projesi oluştururken, birim testlerini tümleştirme testlerinden farklı projelere ayırın. Bu, altyapı testi bileşenlerinin birim testlerine yanlışlıkla dahil olmamasını sağlamaya yardımcı olur. Birim ve tümleştirme testlerinin ayrımı, hangi test kümesinin çalıştırılmasına da izin verir.

Razor Pages Apps ve MVC uygulamalarının testleri için yapılandırma arasında neredeyse fark yoktur. Tek fark testlerin adlandırılmasınlardır. Razor Pages uygulamasında, sayfa uç noktalarının testleri genellikle sayfa modeli sınıfından sonra adlandırılır (örneğin, dizin sayfasına yönelik bileşen tümleştirmesini test etmek için `IndexPageTests`). MVC uygulamasında testler genellikle denetleyici sınıfları tarafından düzenlenir ve test ettikleri denetleyiciler (örneğin, ana denetleyicinin bileşen tümleştirmesini test etmek için `HomeControllerTests`).

## <a name="test-app-prerequisites"></a>Test uygulaması önkoşulları

Test projesi şunları vermelidir:

* Aşağıdaki paketlere başvurun:
  * [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft. AspNetCore. Mvc. test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Proje dosyasında Web SDK 'sını belirtin (`<Project Sdk="Microsoft.NET.Sdk.Web">`). [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e başvururken Web SDK 'sı gereklidir.

Bu Önkoşullar [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)görülebilir. *Testler/RazorPagesProject. Tests/RazorPagesProject. Tests. csproj* dosyasını inceleyin. Örnek uygulama, [xUnit](https://xunit.github.io/) test çerçevesini ve [anglesharp](https://anglesharp.github.io/) Parser kitaplığını kullanır, bu nedenle örnek uygulama de şu şekilde başvurur:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit. Çalıştırıcısı. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>SUT ortamı

SUT [ortamı](xref:fundamentals/environments) ayarlanmamışsa, ortam varsayılan olarak geliştirme olur.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Varsayılan WebApplicationFactory ile temel testler

[Webapplicationfactory @ no__t-1TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) , tümleştirme testleri Için bir [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) oluşturmak üzere kullanılır. `TEntryPoint`, genellikle `Startup` sınıfının SUT giriş noktası sınıfıdır.

Test sınıfları sınıfı testlerin içerdiğini göstermek ve sınıftaki testler arasında paylaşılan nesne örnekleri sağlamak için bir *sınıf armatürü* arabirimi ([ıssfixture](https://xunit.github.io/docs/shared-context#class-fixture)) uygular.

### <a name="basic-test-of-app-endpoints"></a>Uygulama uç noktalarının temel testi

Aşağıdaki `BasicTests` test sınıfı, SUT 'yi önyüklemek ve bir test yöntemine [HttpClient](/dotnet/api/system.net.http.httpclient) sağlamak için `WebApplicationFactory` kullanır `Get_EndpointsReturnSuccessAndCorrectContentType`. Yöntemi, yanıt durum kodunun başarılı olup olmadığını denetler (200-299 aralığındaki durum kodları) ve `Content-Type` üstbilgisi çeşitli uygulama sayfaları için `text/html; charset=utf-8` ' dir.

[Createclient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) , yeniden yönlendirmeleri otomatik olarak izleyen ve tanımlama bilgilerini işleyen bir `HttpClient` örneği oluşturur.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Varsayılan olarak, [GDPR onay ilkesi](xref:security/gdpr) etkinleştirildiğinde, önemli olmayan tanımlama bilgileri istekler arasında korunmaz. TempData sağlayıcısı tarafından kullanılanlar gibi, önemli olmayan tanımlama bilgilerini korumak için, bunları testlerinizde gerekli olarak işaretleyin. Tanımlama bilgisini önemli olarak işaretleme hakkında yönergeler için bkz. [temel tanımlama bilgileri](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Güvenli uç noktayı test etme

@No__t-0 sınıfındaki başka bir test, güvenli bir uç noktanın, kimliği doğrulanmamış bir kullanıcıyı uygulamanın oturum açma sayfasına yönlendirdiğini denetler.

SUT 'de `/SecurePage` sayfası, sayfaya bir [Authorizefilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) uygulamak Için bir [authorizepage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı kullanır. Daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

@No__t-0 testinde, bir [Webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) , [allowoto redirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) ' ı `false` ' e ayarlayarak yeniden yönlendirmeye izin vermez olarak ayarlanır:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

İstemcinin yeniden yönlendirmeyi izlemeye izin vererek aşağıdaki denetimler yapılabilir:

* SUT tarafından döndürülen durum kodu, oturum açma sayfasına yeniden yönlendirmeden sonra, [HttpStatusCode. Tamam](/dotnet/api/system.net.httpstatuscode)olacak şekilde, son durum koduna değil beklenen [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) sonucuyla denetlenebilir.
* Yanıt üst bilgilerindeki `Location` üst bilgi değeri, `Location` üstbilgisinin mevcut olmadığı son oturum açma sayfası yanıtıyla değil `http://localhost/Identity/Account/Login` ile başlayacağını doğrulamak üzere denetlenir.

@No__t-0 hakkında daha fazla bilgi için bkz. [istemci seçenekleri](#client-options) bölümü.

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory 'yi özelleştirme

Web ana bilgisayar yapılandırması, bir veya daha fazla özel fabrika oluşturmak için `WebApplicationFactory` ' dan devralarak test sınıflarından bağımsız olarak oluşturulabilir:

1. @No__t-0 ' dan devralma ve [Configurewebhost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)'i geçersiz kılma. [Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , hizmet koleksiyonunun [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)ile yapılandırılmasına izin verir:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) veritabanı dengeli dağıtımı `InitializeDbForTests` yöntemi tarafından gerçekleştirilir. Yöntemi [ıntegration Tests örneğinde açıklanmıştır: Test uygulaması kuruluş @ no__t-0 bölümü.

2. Test sınıflarında özel `CustomWebApplicationFactory` kullanın. Aşağıdaki örnek `IndexPageTests` sınıfında fabrikası kullanır:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Örnek uygulamanın istemcisi, aşağıdaki yeniden yönlendirmelere `HttpClient` ' i engelleyecek şekilde yapılandırılmıştır. [Güvenli uç nokta testi](#test-a-secure-endpoint) bölümünde açıklandığı gibi, bu, testlerin uygulamanın ilk yanıtının sonucunu denetlemesini sağlar. İlk yanıt, `Location` üst bilgisiyle bu testlerin çoğunda bir yeniden yönlendirmelidir.

3. Tipik bir test, isteği ve yanıtı işlemek için `HttpClient` ve yardımcı yöntemlerini kullanır:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

SUT 'a yönelik herhangi bir POST isteği, uygulamanın [veri koruma antiforgery sistemi](xref:security/data-protection/introduction)tarafından otomatik olarak oluşturulan antiforgery denetimini karşılamalıdır. Bir testin POST isteğini düzenlemek için test uygulaması şunları kullanmalıdır:

1. Sayfa için bir istek oluşturun.
1. Antiforgery tanımlama bilgisini ayrıştırın ve yanıt doğrulama belirtecini istekten isteyin.
1. POST isteğini, antiforgery tanımlama bilgisiyle ve istek doğrulama belirteciyle birlikte yapın.

[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) `SendAsync` yardımcı uzantı yöntemleri (*yardımcılar/Httpclienconversionsions. cs*) ve `GetDocumentAsync` yardımcı yöntemi (*yardımcılar/htmlyardımcıları. cs* [),](https://anglesharp.github.io/) Aşağıdaki Yöntemler:

* `GetDocumentAsync` &ndash; [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) 'yi alır ve bir `IHtmlDocument` döndürür. `GetDocumentAsync`, özgün @no__t 2 ' ye dayalı olarak bir *sanal yanıt* hazırlayan bir fabrika kullanır. Daha fazla bilgi için bkz. [Anglesharp belgeleri](https://github.com/AngleSharp/AngleSharp#documentation).
* `HttpClient` için `SendAsync` genişletme yöntemleri bir [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) oluşturun ve istekleri sut 'e göndermek Için [sendadsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) çağrısı yapın. @No__t için aşırı yüklemeler-0 HTML formunu kabul eder (`IHtmlFormElement`) ve şunları yapın:
  * Formun Gönder düğmesi (`IHtmlElement`)
  * Form değerleri koleksiyonu (`IEnumerable<KeyValuePair<string, string>>`)
  * Gönder düğmesi (`IHtmlElement`) ve form değerleri (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [Anglesharp](https://anglesharp.github.io/) , bu konuda ve örnek uygulamada Gösterim amacıyla kullanılan bir üçüncü taraf ayrıştırma kitaplığıdır. ASP.NET Core uygulamalarının tümleştirme testi için AngleSharp desteklenmez veya gerekli değildir. Diğer çözümleyiciler, [HTML çevikliği paketi (HAP)](https://html-agility-pack.net/)gibi kullanılabilir. Diğer bir yaklaşım ise, antiforgery sisteminin istek doğrulama belirtecini ve antiforgery tanımlama bilgisini doğrudan işlemek için kod yazmaktır.

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder ile istemciyi özelleştirme

Bir test yönteminde ek yapılandırma gerektiğinde, [Withwebhostbuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) , yapılandırmaya göre daha fazla özelleştirilmiş bir [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ile yeni bir `WebApplicationFactory` oluşturur.

[Örnek uygulamanın](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test yöntemi, `WithWebHostBuilder` kullanımını gösterir. Bu test, SUT 'da form gönderimini tetikleyerek veritabanında silme işlemini gerçekleştirir.

@No__t-0 sınıfındaki başka bir test veritabanındaki tüm kayıtları silen ve `Post_DeleteMessageHandler_ReturnsRedirectToRoot` yönteminden önce çalışabilen bir işlem gerçekleştirdiğinden, SUT 'in silinmesine yönelik bir kaydın mevcut olduğundan emin olmak için veritabanı bu test yönteminde yeniden oluşturulur. SUT içindeki `messages` formunun ilk Sil düğmesini seçmek, SUT isteğine göre benzetilir:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>İstemci seçenekleri

Aşağıdaki tabloda `HttpClient` örnekleri oluşturulurken kullanılabilen varsayılan [Webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) gösterilmektedir.

| Seçenek | Açıklama | Varsayılan |
| ------ | ----------- | ------- |
| [Allowoto yeniden yönlendirme](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | @No__t-0 örneklerinin yeniden yönlendirme yanıtlarını otomatik olarak izlemesi gerekip gerekmediğini alır veya ayarlar. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | @No__t-0 örneklerinin temel adresini alır veya ayarlar. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | @No__t-0 örneklerinin tanımlama bilgilerini işlemesinin gerekip gerekmediğini alır veya ayarlar. | `true` |
| [Maxautomaticyönlendirmeler](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | @No__t-0 örneklerinin izlemesi gereken en fazla yeniden yönlendirme yanıtı sayısını alır veya ayarlar. | 7 |

@No__t-0 sınıfını oluşturun ve [Createclient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) yöntemine geçirin (varsayılan değerler kod örneğinde gösterilir):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Sahte hizmetler ekleme

Hizmetler, ana bilgisayar tasarımcısında [Configuretestservices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) çağrısıyla bir testte geçersiz kılınabilir. **Sahte hizmetleri eklemek için SUT, `Startup.ConfigureServices` yöntemine sahip `Startup` sınıfına sahip olmalıdır.**

Örnek SUT, bir teklif döndüren kapsamlı bir hizmet içerir. Dizin sayfası istendiğinde, teklif Dizin sayfasındaki gizli bir alana katıştırılır.

*Hizmetler/ıquoteservice. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Hizmetler/QuoteService. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index. cshtml. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Sayfa/dizin. cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Aşağıdaki biçimlendirme, SUT uygulaması çalıştırıldığında oluşturulur:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Bir tümleştirme testinde hizmet ve teklif ekleme işlemini test etmek için, test tarafından SUT 'ye bir sahte hizmet eklenir. Sahte hizmet, uygulamanın `QuoteService` ' ı, `TestQuoteService` adlı test uygulaması tarafından verilen bir hizmetle değiştirir:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` çağrılır ve kapsamlı hizmet kaydedilir:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Testin yürütülmesi sırasında üretilen biçimlendirme `TestQuoteService` tarafından sağlanan tırnak metnini yansıtır, bu nedenle onaylama işlemi geçirilir:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Test altyapısının uygulama içeriği kök yolunu nasıl öğrendiğini

@No__t-0 Oluşturucusu, `TEntryPoint` derlemesine `System.Reflection.Assembly.FullName` ' e eşit bir anahtarla tümleştirme testlerini içeren derlemede bir [Webapplicationfactorycontentrootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) öğesini arayarak uygulama [içeriği kök](xref:fundamentals/index#content-root) yolunu alır. Doğru anahtara sahip bir özniteliğin bulunamaması durumunda, `WebApplicationFactory` bir çözüm dosyasını ( *. sln*) aramaya geri döner ve `TEntryPoint` derleme adını çözüm dizinine ekler. Uygulama kök dizini (içerik kök yolu) görünümleri ve içerik dosyalarını saptamak için kullanılır.

## <a name="disable-shadow-copying"></a>Gölge kopyalamayı devre dışı bırak

Gölge kopyalama, testlerin çıkış dizininden farklı bir dizinde yürütülmesine neden olur. Testlerin düzgün çalışması için gölge kopyalama devre dışı bırakılmalıdır. [Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit kullanır ve doğru yapılandırma ayarıyla bir *xUnit. Runner. JSON* dosyası ekleyerek xUnit için gölge kopyalamayı devre dışı bırakır. Daha fazla bilgi için bkz. [JSON Ile xUnit yapılandırma](https://xunit.github.io/docs/configuring-with-json.html).

Aşağıdaki içeriğe sahip test projesinin köküne *xUnit. Runner. JSON* dosyasını ekleyin:

```json
{
  "shadowCopy": false
}
```

Visual Studio kullanıyorsanız, dosyanın **Çıkış Dizinine Kopyala** özelliğini **her zaman Kopyala**olarak ayarlayın. Visual Studio kullanmıyorsanız, test uygulamasının proje dosyasına bir `Content` hedefi ekleyin:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Nesnelerin elden çıkarılması

@No__t-0 uygulamasının testleri yürütüldükten sonra, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) ve [HttpClient](/dotnet/api/system.net.http.httpclient) , [webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)'nin xUnit 'i çıkardığı zaman yürütülür. Geliştirici tarafından oluşturulan nesneler için aktiften çıkarma gerekiyorsa, bunları `IClassFixture` uygulamasında atın. Daha fazla bilgi için bkz. [Dispose yöntemi uygulama](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Tümleştirme Testleri örneği

[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) iki uygulamalardan oluşur:

| Uygulama | Proje dizini | Açıklama |
| --- | ----------------- | ----------- |
| İleti uygulaması (SUT) | *src/RazorPagesProject* | Bir kullanıcının, iletileri eklemesini, silmesini, silmesini ve analiz etmesini sağlar. |
| Test uygulaması | *testler/RazorPagesProject. testler* | SUT test tümleştirmesi için kullanılır. |

Testler, [Visual Studio](https://visualstudio.microsoft.com)gıbı bir IDE 'nin yerleşik test özellikleri kullanılarak çalıştırılabilir. [Visual Studio Code](https://code.visualstudio.com/) veya komut satırı kullanıyorsanız, *testler/RazorPagesProject. Tests* dizinindeki bir komut isteminde aşağıdaki komutu yürütün:

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>İleti uygulaması (SUT) kuruluşu

SUT, aşağıdaki özelliklere sahip bir Razor Pages ileti sistemidir:

* Uygulamanın (*Pages/Index. cshtml* ve *Pages/index. cshtml. cs*) dizin sayfası, iletilerin eklenmesi, silinmesini ve ANALIZINI denetlemek için bir UI ve sayfa modeli yöntemleri sağlar (ileti başına ortalama sözcük).
* Bir ileti, iki özellik içeren `Message` sınıfı (*Data/Message. cs*) tarafından tanımlanır: `Id` (anahtar) ve `Text` (ileti). @No__t-0 özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletiler, [Entity Framework bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;kullanılarak depolanır.
* Uygulama, `AppDbContext` (*Data/AppDbContext. cs*) veritabanı bağlamı sınıfında bir veri erişim KATMANı (dal) içerir.
* Veritabanı uygulama başlangıcında boşsa, ileti deposu üç iletiyle başlatılır.
* Uygulama yalnızca kimliği doğrulanmış bir kullanıcı tarafından erişilebilen bir `/SecurePage` içerir.

&#8224;[InMemory Ile test](/ef/core/miscellaneous/testing/in-memory)olan EF konusu, MSTest ile testler için bellek içi bir veritabanının nasıl kullanılacağını açıklar. Bu konu [xUnit](https://xunit.github.io/) test çerçevesini kullanır. Farklı test çerçeveleri genelinde test kavramları ve test uygulamaları benzerdir ancak aynı değildir.

Uygulama, depo desenini kullanmıyor ve [Iş birimi (UoW) düzeninin](https://martinfowler.com/eaaCatalog/unitOfWork.html)etkin bir örneği olmamasına karşın, Razor Pages bu geliştirme düzenlerini destekler. Daha fazla bilgi için bkz. [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek, depo modelini uygular).

### <a name="test-app-organization"></a>Test uygulaması kuruluşu

Test uygulaması, *testler/RazorPagesProject. Tests* dizini içindeki bir konsol uygulamasıdır.

| Uygulama dizinini test et | Açıklama |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* , yönlendirme için test yöntemleri, kimliği doğrulanmamış bir kullanıcı tarafından güvenli bir sayfaya erişmek ve bir GitHub Kullanıcı profili elde etmek ve profilin Kullanıcı oturum açma bilgilerini denetlemek içerir. |
| *Tümleştirme Testleri* | *IndexPageTests.cs* , özel `WebApplicationFactory` sınıfı kullanarak Dizin sayfası için tümleştirme testlerini içerir. |
| *Yardımcılar/yardımcı programlar* | <ul><li>*Utilities.cs* , veritabanını test verileriyle tohum için kullanılan `InitializeDbForTests` metodunu içerir.</li><li>*HtmlHelpers.cs* , test yöntemleri tarafından kullanılmak üzere AngleSharp `IHtmlDocument` döndüren bir yöntem sağlar.</li><li>*HttpClientExtensions.cs* , istekleri sut 'a göndermek için `SendAsync` için aşırı yüklemeler sağlar.</li></ul> |

Test çerçevesi [xUnit](https://xunit.github.io/)' dir. Tümleştirme testleri, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)içeren [Microsoft. Aspnetcore. testhost](/dotnet/api/microsoft.aspnetcore.testhost)kullanılarak yürütülür. Test ana bilgisayarı ve test sunucusunu yapılandırmak için [Microsoft. AspNetCore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketi kullanıldığından, `TestHost` ve `TestServer` paketleri test uygulamasının proje dosyasında ya da testteki geliştirici yapılandırmasında doğrudan paket başvuruları gerektirmez uygulamanızda.

**Test için veritabanının temelini sağlama**

Tümleştirme testleri genellikle veritabanında test yürütmeden önce küçük bir veri kümesi gerektirir. Örneğin, veritabanı kaydı silme için bir silme testi çağrılarında, silme isteğinin başarılı olması için veritabanının en az bir kaydı olmalıdır.

Örnek uygulama, *Utilities.cs* içinde testlerin, yürütme sırasında kullanabileceği üç iletiyle birlikte veritabanını kullanır:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Test denetleyicileri](xref:mvc/controllers/testing)
