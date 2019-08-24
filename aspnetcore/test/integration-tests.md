---
title: ASP.NET Core tümleştirme testleri
author: guardrex
description: Tümleştirme testlerinin, bir uygulamanın bileşenlerinin, veritabanı, dosya sistemi ve ağ dahil olmak üzere altyapı düzeyinde doğru şekilde çalışmasını nasıl sağladığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: test/integration-tests
ms.openlocfilehash: 195acd3e03f3de63ebd61767f2c86d1c0f38fca5
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017441"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core tümleştirme testleri

[Luke Latham](https://github.com/guardrex) ve [Steve Smith](https://ardalis.com/) tarafından

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

* Veritabanı
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

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core tümleştirme testleri

ASP.NET Core tümleştirme testleri şunları gerektirir:

* Testleri içermesi ve yürütmek için bir test projesi kullanılır. Test projesi *test edilen sistem* (SUT) adlı test ASP.NET Core projesine bir başvuru içerir. _Bu konu başlığı altında, sınanan uygulamaya başvurmak için "SUT" kullanılır._
* Test projesi SUT için bir test Web ana bilgisayarı oluşturur ve SUT isteklerini ve yanıtlarını işlemek için bir test sunucusu istemcisi kullanır.
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

`Microsoft.AspNetCore.Mvc.Testing` Paket aşağıdaki görevleri gerçekleştirir:

* Bağımlılıklar dosyasını ( *\*. Deps*) sut 'den test projesinin *bin* dizinine kopyalar.
* , Testler yürütüldüğünde statik dosya ve sayfa/görünümlerin bulunması için içerik kökünü SUT 'nin proje köküne ayarlar.
* , İle `TestServer`sut önyükleyiciyi kolaylaştırmak için [webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) sınıfını sağlar.

[Birim](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) testi belgeleri bir test projesi ve Test Çalıştırıcısı ayarlamayı ve testlerin ve test sınıflarının nasıl belirleneceğini gösteren testlerin ve önerilerin nasıl çalıştırılacağını gösteren ayrıntılı yönergelerle birlikte açıklanır.

> [!NOTE]
> Bir uygulama için test projesi oluştururken, birim testlerini tümleştirme testlerinden farklı projelere ayırın. Bu, altyapı testi bileşenlerinin birim testlerine yanlışlıkla dahil olmamasını sağlamaya yardımcı olur. Birim ve tümleştirme testlerinin ayrımı, hangi test kümesinin çalıştırılmasına da izin verir.

Razor Pages Apps ve MVC uygulamalarının testleri için yapılandırma arasında neredeyse fark yoktur. Tek fark testlerin adlandırılmasınlardır. Razor Pages uygulamasında, sayfa uç noktalarının testleri genellikle sayfa modeli sınıfından sonra (örneğin, `IndexPageTests` dizin sayfasına yönelik bileşen tümleştirmesini test etmek için) adlandırılır. MVC uygulamasında, testler genellikle denetleyici sınıfları tarafından düzenlenir ve test ettikleri denetleyicilerde (örneğin, `HomeControllerTests` ana denetleyiciye yönelik bileşen tümleştirmesini test etmek için) adlandırılır.

## <a name="test-app-prerequisites"></a>Test uygulaması önkoşulları

Test projesi şunları vermelidir:

* Aşağıdaki paketlere başvurun:
  * [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft. AspNetCore. Mvc. test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* (`<Project Sdk="Microsoft.NET.Sdk.Web">`) Proje dosyasında Web SDK 'sını belirtin. [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e başvururken Web SDK 'sı gereklidir.

Bu Önkoşullar [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)görülebilir. *Testler/RazorPagesProject. Tests/RazorPagesProject. Tests. csproj* dosyasını inceleyin. Örnek uygulama, [xUnit](https://xunit.github.io/) test çerçevesini ve [anglesharp](https://anglesharp.github.io/) Parser kitaplığını kullanır, bu nedenle örnek uygulama de şu şekilde başvurur:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit. Çalıştırıcısı. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>SUT ortamı

SUT [ortamı](xref:fundamentals/environments) ayarlanmamışsa, ortam varsayılan olarak geliştirme olur.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Varsayılan WebApplicationFactory ile temel testler

[Webapplicationfactory&lt;tentrypoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) , tümleştirme testleri için bir [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) oluşturmak üzere kullanılır. `TEntryPoint`, genellikle `Startup` sınıfının, sut 'ın giriş noktası sınıfıdır.

Test sınıfları sınıfı testlerin içerdiğini göstermek ve sınıftaki testler arasında paylaşılan nesne örnekleri sağlamak için bir *sınıf armatürü* arabirimi ([ıssfixture](https://xunit.github.io/docs/shared-context#class-fixture)) uygular.

### <a name="basic-test-of-app-endpoints"></a>Uygulama uç noktalarının temel testi

`BasicTests`Aşağıdaki test sınıfı, sut 'yi önyüklemek `WebApplicationFactory` ve bir test yöntemine `Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient) sağlamak için öğesini kullanır. Yöntemi, yanıt durum kodunun başarılı olup olmadığını denetler (200-299 aralığındaki durum kodları) ve `Content-Type` `text/html; charset=utf-8` üst bilgi birçok uygulama sayfasına yöneliktir.

[Createclient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) , `HttpClient` yeniden yönlendirmeleri otomatik olarak izleyen ve tanımlama bilgilerini işleyen bir örneği oluşturur.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Varsayılan olarak, [GDPR onay ilkesi](xref:security/gdpr) etkinleştirildiğinde, önemli olmayan tanımlama bilgileri istekler arasında korunmaz. TempData sağlayıcısı tarafından kullanılanlar gibi, önemli olmayan tanımlama bilgilerini korumak için, bunları testlerinizde gerekli olarak işaretleyin. Tanımlama bilgisini önemli olarak işaretleme hakkında yönergeler için bkz. [temel tanımlama bilgileri](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Güvenli uç noktayı test etme

`BasicTests` Sınıftaki başka bir test, güvenli bir uç noktanın kimliği doğrulanmamış bir kullanıcıyı uygulamanın oturum açma sayfasına yönlendirdiğini denetler.

Sut `/SecurePage` sayfasında, sayfada bir [authorizefilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) uygulamak için Bu sayfa bir [authorizepage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı kullanır. Daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Testte, bir [webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) , [allowclıredirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) ' i ayarlayarak yeniden `false`yönlendirmeye izin vermez olarak ayarlanır: `Get_SecurePageRequiresAnAuthenticatedUser`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

İstemcinin yeniden yönlendirmeyi izlemeye izin vererek aşağıdaki denetimler yapılabilir:

* SUT tarafından döndürülen durum kodu, oturum açma sayfasına yeniden yönlendirmeden sonra, [HttpStatusCode. Tamam](/dotnet/api/system.net.httpstatuscode)olacak şekilde, son durum koduna değil beklenen [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) sonucuyla denetlenebilir.
* Yanıt üst bilgilerindeki `Location` `http://localhost/Identity/Account/Login` `Location` üst bilgi değeri, üst bilgi bulunmayan son oturum açma sayfası yanıtı değil, ile başladığı onaylanacak şekilde denetlenir.

Hakkında `WebApplicationFactoryClientOptions`daha fazla bilgi için [istemci seçenekleri](#client-options) bölümüne bakın.

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory 'yi özelleştirme

Web ana bilgisayar yapılandırması, öğesinden `WebApplicationFactory` devralarak bir veya daha fazla özel fabrika oluşturmak için test sınıflarından bağımsız olarak oluşturulabilir:

1. `WebApplicationFactory` [Configurewebhost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)'ten devralma ve geçersiz kılma. [Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) , hizmet koleksiyonunun [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)ile yapılandırılmasına izin verir:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   [Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) veritabanı dengeli dağıtımı, `InitializeDbForTests` yöntemi tarafından gerçekleştirilir. Yöntemi, [tümleştirme testleri örneğinde açıklanmıştır: Uygulama organizasyonunu](#test-app-organization) test etme bölümü.

2. Özel `CustomWebApplicationFactory` test sınıflarında kullanın. Aşağıdaki örnek, `IndexPageTests` sınıfında fabrikası kullanır:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Örnek uygulamanın istemcisi, `HttpClient` aşağıdaki yeniden yönlendirmeleri engelleyecek şekilde yapılandırılmıştır. [Güvenli uç nokta testi](#test-a-secure-endpoint) bölümünde açıklandığı gibi, bu, testlerin uygulamanın ilk yanıtının sonucunu denetlemesini sağlar. İlk yanıt, bu testlerin çoğunda `Location` üst bilgiyle bir yeniden yönlendirmelidir.

3. Tipik bir test, isteği `HttpClient` ve yanıtı işlemek için ve yardımcı yöntemlerini kullanır:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

SUT 'a yönelik herhangi bir POST isteği, uygulamanın [veri koruma antiforgery sistemi](xref:security/data-protection/introduction)tarafından otomatik olarak oluşturulan antiforgery denetimini karşılamalıdır. Bir testin POST isteğini düzenlemek için test uygulaması şunları kullanmalıdır:

1. Sayfa için bir istek oluşturun.
1. Antiforgery tanımlama bilgisini ayrıştırın ve yanıt doğrulama belirtecini istekten isteyin.
1. POST isteğini, antiforgery tanımlama bilgisiyle ve istek doğrulama belirteciyle birlikte yapın.

[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) `GetDocumentAsync` yardımcı uzantı yöntemleri (*yardımcılar/httpclienconversionsions. cs*) ve yardımcı yöntemi (*yardımcılar/htmlyardımcıları. cs*), antipergery 'yi işlemek için [anglesharp](https://anglesharp.github.io/) ayrıştırıcılarını kullanır `SendAsync` Aşağıdaki yöntemlerle denetleyin:

* `GetDocumentAsync`HttpResponseMessage [](/dotnet/api/system.net.http.httpresponsemessage) 'ı alır ve döndürür `IHtmlDocument`. &ndash; `GetDocumentAsync`, orijinalin `HttpResponseMessage`temelinde bir *sanal yanıt* hazırlayan bir fabrika kullanır. Daha fazla bilgi için bkz. [Anglesharp belgeleri](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync`bir HttpRequestMessage [](/dotnet/api/system.net.http.httprequestmessage) [](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) oluşturun ve istekleri sut 'a göndermek için sendadsync (HttpRequestMessage) çağrısı yapın. `HttpClient` HTML biçimini `SendAsync` kabul etmek için aşırı yüklemeler`IHtmlFormElement`() ve şunları yapın:
  * Formun (`IHtmlElement`) Gönder düğmesi
  * Form değerleri koleksiyonu (`IEnumerable<KeyValuePair<string, string>>`)
  * Düğme (`IHtmlElement`) ve form değerlerini (`IEnumerable<KeyValuePair<string, string>>`) gönder

> [!NOTE]
> [Anglesharp](https://anglesharp.github.io/) , bu konuda ve örnek uygulamada Gösterim amacıyla kullanılan bir üçüncü taraf ayrıştırma kitaplığıdır. ASP.NET Core uygulamalarının tümleştirme testi için AngleSharp desteklenmez veya gerekli değildir. Diğer çözümleyiciler, [HTML çevikliği paketi (HAP)](https://html-agility-pack.net/)gibi kullanılabilir. Diğer bir yaklaşım ise, antiforgery sisteminin istek doğrulama belirtecini ve antiforgery tanımlama bilgisini doğrudan işlemek için kod yazmaktır.

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder ile istemciyi özelleştirme

Bir test yönteminde ek yapılandırma gerektiğinde, [withwebhostbuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) , yapılandırmaya göre daha fazla özelleştirilmiş `WebApplicationFactory` bir [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ile yeni bir oluşturur.

Örnek uygulamanın `WithWebHostBuilder`test yöntemi öğesinin kullanımını gösterir. [](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) `Post_DeleteMessageHandler_ReturnsRedirectToRoot` Bu test, SUT 'da form gönderimini tetikleyerek veritabanında silme işlemini gerçekleştirir.

`IndexPageTests` Sınıftaki başka bir test veritabanındaki tüm kayıtları silen ve `Post_DeleteMessageHandler_ReturnsRedirectToRoot` yönteminden önce çalışabilen bir işlem gerçekleştirdiğinden, sut 'in silinmesine yönelik bir kaydın mevcut olduğundan emin olmak için veritabanı bu test yönteminde gerçekleştirilir. Sut içindeki `messages` formun düğmeseçilmesi,sutisteğinegörebenzetilir:`deleteBtn1`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>İstemci seçenekleri

Aşağıdaki tabloda, örnek oluştururken `HttpClient` kullanılabilen varsayılan [webapplicationfactoryclientoptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) gösterilmektedir.

| Seçenek | Açıklama | Varsayılan |
| ------ | ----------- | ------- |
| [Allowoto yeniden yönlendirme](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | `HttpClient` Örneklerin otomatik olarak yeniden yönlendirme yanıtlarını izleyip izmeyeceğini alır veya ayarlar. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | `HttpClient` Örneklerin temel adresini alır veya ayarlar. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Örneklerin tanımlama bilgilerini işlemesinin gerekip gerekmediğini `HttpClient` alır veya ayarlar. | `true` |
| [Maxautomaticyönlendirmeler](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | `HttpClient` Örneklerin izlemesi gereken en fazla yeniden yönlendirme yanıtı sayısını alır veya ayarlar. | 7 |

Sınıfını oluşturun ve createclient yöntemine geçirin (varsayılan değerler kod örneğinde gösterilir): [](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `WebApplicationFactoryClientOptions`

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

Hizmetler, ana bilgisayar tasarımcısında [Configuretestservices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) çağrısıyla bir testte geçersiz kılınabilir. **Sahte hizmetleri eklemek için sut, `Startup` `Startup.ConfigureServices` yöntemi olan bir sınıfa sahip olmalıdır.**

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

Bir tümleştirme testinde hizmet ve teklif ekleme işlemini test etmek için, test tarafından SUT 'ye bir sahte hizmet eklenir. Sahte hizmet, uygulamanın `QuoteService` adı verilen `TestQuoteService`test uygulaması tarafından verilen bir hizmetle değiştirilir:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`çağrılır ve kapsamlı hizmet kaydedilir:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Testin yürütülmesi sırasında üretilen biçimlendirme tarafından `TestQuoteService`sağlanan teklif metnini yansıtır, bu nedenle onaylama işlemi geçer:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Test altyapısının uygulama içeriği kök yolunu nasıl öğrendiğini

Oluşturucu, `TEntryPoint` [](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) Derleme`System.Reflection.Assembly.FullName`üzerinde bir anahtarla eşit anahtar olan tümleştirme testlerini içeren bir webapplicationfactorycontentrootattribute arayarak uygulama içeriği kök yolunu alır. `WebApplicationFactory` Doğru anahtara sahip bir özniteliğin bulunamaması durumunda, `WebApplicationFactory` bir çözüm dosyası ( *\** `TEntryPoint` . sln) aramaya geri döner ve derleme adını çözüm dizinine ekler. Uygulama kök dizini (içerik kök yolu) görünümleri ve içerik dosyalarını saptamak için kullanılır.

## <a name="disable-shadow-copying"></a>Gölge kopyalamayı devre dışı bırak

Gölge kopyalama, testlerin çıkış dizininden farklı bir dizinde yürütülmesine neden olur. Testlerin düzgün çalışması için gölge kopyalama devre dışı bırakılmalıdır. [Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) xUnit kullanır ve doğru yapılandırma ayarıyla bir *xUnit. Runner. JSON* dosyası ekleyerek xUnit için gölge kopyalamayı devre dışı bırakır. Daha fazla bilgi için bkz. [JSON Ile xUnit yapılandırma](https://xunit.github.io/docs/configuring-with-json.html).

Aşağıdaki içeriğe sahip test projesinin köküne *xUnit. Runner. JSON* dosyasını ekleyin:

```json
{
  "shadowCopy": false
}
```

Visual Studio kullanıyorsanız, dosyanın **Çıkış Dizinine Kopyala** özelliğini **her zaman Kopyala**olarak ayarlayın. Visual Studio kullanmıyorsanız, test uygulamasının proje dosyasına `Content` bir hedef ekleyin:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Nesnelerin elden çıkarılması

`IClassFixture` Uygulamanın testleri yürütüldükten sonra, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) ve [HttpClient](/dotnet/api/system.net.http.httpclient) , [webapplicationfactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)'nin xUnit 'i çıkardığı zaman yürütülür. Geliştirici tarafından oluşturulan nesneler elden çıkarma gerektiriyorsa, bunları `IClassFixture` uygulamada atın. Daha fazla bilgi için bkz. [Dispose yöntemi uygulama](/dotnet/standard/garbage-collection/implementing-dispose).

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
* Bir ileti, `Message` sınıfı (*Data/Message. cs*) ile iki özelliği `Id` olan (anahtar) ve `Text` (ileti) açıklanmaktadır. `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletiler, [Entity Framework bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;kullanılarak depolanır.
* Uygulama, `AppDbContext` veritabanı bağlamı sınıfında (*Data/appdbcontext. cs*) bir veri erişim katmanı (dal) içerir.
* Veritabanı uygulama başlangıcında boşsa, ileti deposu üç iletiyle başlatılır.
* Uygulama yalnızca kimliği doğrulanmış `/SecurePage` bir kullanıcı tarafından erişilebilen bir içerir.

&#8224;[InMemory Ile test](/ef/core/miscellaneous/testing/in-memory)olan EF konusu, MSTest ile testler için bellek içi bir veritabanının nasıl kullanılacağını açıklar. Bu konu [xUnit](https://xunit.github.io/) test çerçevesini kullanır. Farklı test çerçeveleri genelinde test kavramları ve test uygulamaları benzerdir ancak aynı değildir.

Uygulama, depo desenini kullanmıyor ve [Iş birimi (UoW) düzeninin](https://martinfowler.com/eaaCatalog/unitOfWork.html)etkin bir örneği olmamasına karşın, Razor Pages bu geliştirme düzenlerini destekler. Daha fazla bilgi için bkz. [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek, depo modelini uygular).

### <a name="test-app-organization"></a>Test uygulaması kuruluşu

Test uygulaması, *testler/RazorPagesProject. Tests* dizini içindeki bir konsol uygulamasıdır.

| Uygulama dizinini test et | Açıklama |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* , yönlendirme için test yöntemleri, kimliği doğrulanmamış bir kullanıcı tarafından güvenli bir sayfaya erişmek ve bir GitHub Kullanıcı profili elde etmek ve profilin Kullanıcı oturum açma bilgilerini denetlemek içerir. |
| *Tümleştirme Testleri* | *IndexPageTests.cs* , özel `WebApplicationFactory` sınıf kullanarak Dizin sayfası için tümleştirme testlerini içerir. |
| *Yardımcılar/yardımcı programlar* | <ul><li>*Utilities.cs* , `InitializeDbForTests` veritabanını test verileriyle tohum için kullanılan yöntemi içerir.</li><li>*HtmlHelpers.cs* , test yöntemleri tarafından kullanılmak üzere anglesharp `IHtmlDocument` döndüren bir yöntem sağlar.</li><li>*HttpClientExtensions.cs* , istekleri sut 'a göndermek için için `SendAsync` aşırı yüklemeler sağlar.</li></ul> |

Test çerçevesi [xUnit](https://xunit.github.io/)' dir. Tümleştirme testleri, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)içeren [Microsoft. Aspnetcore. testhost](/dotnet/api/microsoft.aspnetcore.testhost)kullanılarak yürütülür. Test konağını ve test sunucusunu yapılandırmak için [Microsoft. aspnetcore. Mvc. Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) paketi kullanıldığından, `TestHost` ve `TestServer` paketleri test uygulamasının proje dosyasında veya geliştiricisinden doğrudan paket başvuruları gerektirmez test uygulamasında yapılandırma.

**Test için veritabanının temelini sağlama**

Tümleştirme testleri genellikle veritabanında test yürütmeden önce küçük bir veri kümesi gerektirir. Örneğin, veritabanı kaydı silme için bir silme testi çağrılarında, silme isteğinin başarılı olması için veritabanının en az bir kaydı olmalıdır.

Örnek uygulama, *Utilities.cs* içinde testlerin, yürütme sırasında kullanabileceği üç iletiyle birlikte veritabanını kullanır:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Test denetleyicileri](xref:mvc/controllers/testing)
