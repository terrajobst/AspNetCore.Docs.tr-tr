---
title: Razor sayfalarının birim ve tümleştirme testlerinde ASP.NET Çekirdeği
author: guardrex
description: Razor sayfalarının uygulamaları için birim ve tümleştirme testleri oluşturmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: dc5e8651f873b8e86aaa8fdf2527e461bb065424
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="razor-pages-unit-and-integration-tests-in-aspnet-core"></a>Razor sayfalarının birim ve tümleştirme testlerinde ASP.NET Çekirdeği

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core, birim ve tümleştirme Razor sayfalarının uygulamalarını test etme destekler. Veri erişim katmanı (DAL), sayfa modelleri ve tümleşik sayfa bileşenleri test sağlamaya yardımcı olur:

* Razor sayfalarının uygulama bölümlerini uygulama oluşturma sırasında bağımsız olarak ve bir birim olarak birlikte çalışır.
* Sınıflar ve yöntemler sorumluluk kapsamları sınırlı.
* Ek belgeleri nasıl uygulama hareket etmesi gerektiğini üzerinde bulunmaktadır.
* Güncelleştirmeleri koduna göre duruma hatalardır, gerileme otomatik yapı ve dağıtım sırasında bulunamadı.

Bu konu, Razor sayfalarının uygulamaların, birim testi ve tümleştirme ilgili temel bilgilere sahip olduğunuzu varsayar test etme. Razor sayfalarının uygulamaları veya test kavramları emin değilseniz aşağıdaki konulara bakın:

* [Razor sayfalarının giriş](xref:mvc/razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Birim testi C# .NET çekirdek dotnet test ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Tümleştirme testleri](xref:testing/integration-testing)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek Proje iki uygulamaların oluşur:

| Uygulama         | Proje klasörü                        | Açıklama |
| ----------- | ------------------------------------- | ----------- |
| İleti Uygulama | *src/RazorPagesTestingSample*         | Bir kullanıcı eklemek, silmek, tüm silin ve iletileri çözümlemek sağlar. |
| Test uygulama    | *tests/RazorPagesTestingSample.Tests* | İleti Uygulama test etmek için kullanılır.<ul><li>Birim testleri: veri erişim katmanı (DAL) dizin sayfası modeli</li><li>Tümleştirme testleri: dizin sayfası</li></ul> |

Testleri bir IDE yerleşik test özelliklerini kullanarak çalıştırabilirsiniz [Visual Studio](https://www.visualstudio.com/vs/). Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırı *tests/RazorPagesTestingSample.Tests* klasörü:

```console
dotnet test
```

## <a name="message-app-organization"></a>İleti Uygulama kuruluş

İleti uygulama basit bir Razor sayfalarının ileti sistemi aşağıdaki özelliklere sahip olur:

* Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) kullanıcı Arabirimi ve sayfa ekleme, silme ve iletileri (ileti başına ortalama sözcük) analizini denetlemek için modeli yöntemleri sağlar .
* Bir ileti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliklere sahip: `Id` (anahtar) ve `Text` (ileti). `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletileri kullanarak depolanır [Entity Framework'ün bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;.
* Uygulama bir veri erişim katmanı (DAL), veritabanı bağlamı sınıfının içeren `AppDbContext` (*Data/AppDbContext.cs*). DAL yöntemleri işaretlenmiş `virtual`, testleri kullanımı için yöntemleri mocking izin verir.
* Veritabanı uygulama başlangıçta boş ise, ileti deposunu üç iletileri ile başlatıldı. Bunlar *sağlanmış iletileri* testinde de kullanılır.

&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), mstest'i ile test etmek için bir bellek içi veritabanı kullanımı açıklanmaktadır. Bu konuda kullanan [xUnit](https://xunit.github.io/) framework test etme. Sınama kavramları ve test uygulamaları farklı test çerçevelerini arasında benzer, ancak aynı değildir.

Uygulama kullanmayan ancak [havuz deseni](http://martinfowler.com/eaaCatalog/repository.html) ve etkili bir örneği değil. [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfalarının geliştirme bu desenleri destekler. Daha fazla bilgi için bkz: [altyapı saklama katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [depo ve iş desenleri bir ASP.NET MVC uygulamasındaki uygulama](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek havuz deseni uygular).

## <a name="test-app-organization"></a>Test uygulama kuruluş

İçinde bir konsol uygulaması test uygulamadır *tests/RazorPagesTestingSample.Tests* klasörü:

| Test uygulama klasörü    | Açıklama |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* dizin sayfası için tümleştirme testleri içerir.</li><li>*TestFixture.cs* ileti uygulama test etmek için test ana bilgisayar oluşturur.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* DAL için birim testleri içerir.</li><li>*IndexPageTest.cs* dizin sayfası modeli için birim testleri içerir.</li></ul> |
| *Yardımcı programlar*        | *Utilities.cs* içerir:<ul><li>`TestingDbContextOptions` Veritabanı her test için kendi temel koşul sıfırlanır her DAL birim testi için içerik seçeneklerini yeni bir veritabanı oluşturmak için kullanılan yöntem.</li><li>`GetRequestContentAsync` hazırlamak için kullanılan yöntem `HttpClient` ve tümleştirme sınaması sırasında ileti uygulamaya gönderilen istekleri için içerik.</li></ul>

Test çerçevesi [xUnit](https://xunit.github.io/). Framework mocking nesne [Moq](https://github.com/moq/moq4). Tümleştirme testleri gerçekleştirilen kullanarak [ASP.NET Core Test ana](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>Birim testi veri erişim katmanı'nı (DAL)

İleti uygulama içinde yer alan dört yöntem bir DAL sahip `AppDbContext` sınıfı (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). Her yöntemin bir veya iki birim testleri test uygulamada vardır.

| DAL yöntemi               | İşlev                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Alacağı bir `List<Message>` göre sıralanmış veritabanından `Text` özelliği. |
| `AddMessageAsync`        | Ekler bir `Message` veritabanı.                                          |
| `DeleteAllMessagesAsync` | Tüm siler `Message` girişler veritabanından.                           |
| `DeleteMessageAsync`     | Tek bir siler `Message` tarafından veritabanından `Id`.                      |

DAL, birim testleri gerektiren [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) yeni oluştururken `AppDbContext` her test için. Oluşturmanın bir yaklaşım `DbContextOptions` her test kullanmak için bir [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Bu yaklaşım sorun ne olursa olsun durumu önceki test, sol içindeki her bir test veritabanı aldığını olmasıdır. Birbiriyle uğratmaz atomik birim testleri yazma girişimi sırasında bu bağlantı sorunlu olabilir. Zorlamak için `AppDbContext` her test için yeni bir veritabanı içeriği kullanmak için girmeniz bir `DbContextOptions` yeni bir hizmet sağlayıcısını temel örneği. Test uygulaması kullanarak bunu gösterilmektedir, `Utilities` sınıf yöntemi `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Kullanarak `DbContextOptions` DAL biriminde testleri otomatik olarak yeni veritabanı örneği ile çalıştırmak her bir test sağlar:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Her bir test yöntemin `DataAccessLayerTest` sınıfı (*UnitTests/DataAccessLayerTest.cs*) Yerleştir Act Assert benzer bir desen izler:

1. Düzenleyin: Veritabanı test için yapılandırılır ve/veya beklenen sonucu tanımlanır.
1. ACT: Test yürütülür.
1. Assert: Onaylar test sonucu başarılı olup olmadığını belirlemek için yapılır.

Örneğin, `DeleteMessageAsync` yöntemi tarafından tanımlanan tek bir ileti kaldırmak için sorumlu kendi `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Bu yöntem için iki testleri vardır. Bir sınama iletisi veritabanında mevcut olduğunda yöntemi bir iletiyi siler denetler. Veritabanı varsa değişmez bir yöntem testleri ileti `Id` için silme işlemi yok. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Yöntemi aşağıda gösterilmektedir:

[!code-csharp[](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

İlk olarak, yöntem, Act adım hazırlığı gerçekleştiği Yerleştir adım gerçekleştirir. Dengeli dağıtım iletiler elde ve içinde tutulan `seedMessages`. Dengeli dağıtım iletiler veritabanına kaydedilir. İletinin bir `Id` , `1` silme işlemi için ayarlanır. Zaman `DeleteMessageAsync` yöntemi yürütüldüğünde, beklenen iletiler biriyle hariç tüm iletileri olmalıdır bir `Id` , `1`. `expectedMessages` Değişkeni bu beklenen sonucu temsil eder.

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Yöntem davranır: `DeleteMessageAsync` tümleştirilmesidir yöntemi yürütüldüğünde `recId` , `1`:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Son olarak, yöntemi alır `Messages` bağlamından ve kendisine karşılaştırır `expectedMessages` iki eşit sunduğundan:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Karşılaştırmak için iki `List<Message>` aynıdır:

* İletileri göre sıralanmış `Id`.
* İleti çiftleri karşılaştırılır `Text` özelliği.

Benzer bir test yöntem `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` mevcut olmayan bir ileti silinmeye çalışılıyor sonucunu denetler. Bu durumda, beklenen veritabanındaki sonra gerçek iletileri eşit mesajlarının `DeleteMessageAsync` yöntemi yürütüldüğünde. Veritabanının içeriği için herhangi bir değişiklik olması gerekir:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Birim testi sayfa modeli yöntemleri

Başka bir birim testleri sayfası modeli yöntemleri test etmek için sorumlu kümesidir. Dizin Sayfası modelleri bulunan ileti uygulamada `IndexModel` sınıfını *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Sayfa modeli yöntemi | İşlev |
| ----------------- | -------- | 
| `OnGetAsync` | Kullanıcı arabirimini kullanarak için DAL iletileri elde `GetMessagesAsync` yöntemi. |
| `OnPostAddMessageAsync` | Varsa `ModelState` geçerlidir, çağıran `AddMessageAsync` veritabanına iletiye ekleyin. | 
| `OnPostDeleteAllMessagesAsync` | Çağrıları `DeleteAllMessagesAsync` veritabanındaki tüm iletileri silmek için. |
| `OnPostDeleteMessageAsync` | Yürütür `DeleteMessageAsync` içeren bir ileti silmek için `Id` belirtilen. |
| `OnPostAnalyzeMessagesAsync` | Bir veya daha fazla ileti veritabanında varsa, sözcük ileti başına ortalama sayısını hesaplar. |

Sayfa modeli yöntemleri yedi testler kullanarak test edildiğini `IndexPageTest` sınıfı (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). Testleri tanıdık Yerleştir Assert Act desen kullanır. Bu testler odaklanır:

* Yöntemleri doğru davranış izlerseniz belirleme zaman `ModelState` geçersiz.
* Yöntemleri onaylayan üretmek doğru `IActionResult`.
* Özellik değeri atamaları doğru şekilde yapıldığını denetleniyor.

Bu grup test genellikle bir sayfa modeli yöntemi yürütüldüğü Act adımı için beklenen veri üretmek için DAL yöntemlerinin mock. Örneğin, `GetMessagesAsync` yöntemi `AppDbContext` çıktı oluşturmak için mocked. Bir sayfa modeli yöntemi bu yöntemi yürütüldüğünde, mock sonucunu döndürür. Verileri veritabanından gelmez. Bu sayfanın modeli testlerinde DAL kullanan için tahmin edilebilir, güvenilir test koşulları oluşturur.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Test gösterir nasıl `GetMessagesAsync` yöntemi için sayfa modeli mocked:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Zaman `OnGetAsync` yöntemi Act adımda yürütülürse, sayfa modelinin çağırır `GetMessagesAsync` yöntemi.

Birim testi Act adım (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage` Sayfa modelinin `OnGetAsync` yöntemi (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` DAL yönteminde değil Bu yöntem çağrısının sonucunu döndürür. Yöntemin mocked sürümü sonucunu döndürür.

İçinde `Assert` adım, gerçek iletileri (`actualMessages`) atanır `Messages` sayfa modelin özelliği. İletileri atandığında bir tür denetimi da gerçekleştirilir. Beklenen ve mevcut iletileri tarafından karşılaştırılır kendi `Text` özellikleri. Test onaylar iki `List<Message>` örnekleri aynı iletiler içerir.

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Bu gruptaki diğer testleri oluşturma sayfası dahil model nesneleri `DefaultHttpContext`, `ModelStateDictionary`, bir `ActionContext` kurmak için `PageContext`, `ViewDataDictionary`ve bir `PageContext`. Bunlar, testleri gerçekleştirme faydalıdır. Örneğin, ileti uygulama kurar bir `ModelState` hatasıyla `AddModelError` denetlemek için geçerli bir `PageResult` olduğunda döndürülen `OnPostAddMessageAsync` yürütülür:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>Uygulamayı test etme tümleştirme

Tümleştirme uygulamanın bileşenleri birlikte çalışan sınama odaklanmasına sınar. Tümleştirme testleri gerçekleştirilen kullanarak [ASP.NET Core Test ana](xref:testing/integration-testing#the-test-host). Tam istek-yanıt yaşam döngüsü işleme test edilir. Bu testleri sayfası doğru durum kodunu üretir assert ve `Location` başlık varsa ayarlayın.

İleti Uygulama dizin sayfasına isteyen sonucunu bir tümleştirme örnek örnekten sınaması denetler (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

Hiçbir Yerleştir adım yoktur. `GetAsync` Yöntemi çağrıldığında `HttpClient` uç noktaya bir GET isteği göndermek için. Test sonucu 200 Tamam durum kodu olduğunu onaylar.

Herhangi bir POST isteği iletisi App uygulamanın tarafından otomatik olarak yapılan antiforgery onay getirmelidir [veri koruması antiforgery sistemi](xref:security/data-protection/introduction). Bir testin POST isteği için düzenlemek için uygulamayı sınayın gerekir:

1. Sayfa için bir istek olun.
1. Antiforgery tanımlama bilgisi ve istek doğrulama belirtecinden yanıtı ayrıştıramadı.
1. Antiforgery tanımlama bilgisi ve istek doğrulama POST isteğini yerinde belirteci olun.

`Post_AddMessageHandler_ReturnsRedirectToRoot` Test yöntemi:

* Bir ileti hazırlar ve `HttpClient`.
* Uygulamaya bir POST isteği yapar.
* Yanıt yeniden yönlendirme geri dizin sayfası denetler.

`Post_AddMessageHandler_ReturnsRedirectToRoot ` yöntem (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

`GetRequestContentAsync` Yardımcı program yöntemi yönetir antiforgery tanımlama bilgisi ve istek doğrulama belirteci istemcisiyle hazırlanıyor. Yöntemine nasıl aldığı Not bir `IDictionary` isteğin istek doğrulama belirteci ile birlikte kodlamak veri geçirmek için arama test yöntemini verir (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Tümleştirme testleri, ayrıca uygulamanın yanıt davranışını test etmek için uygulamaya hatalı veri geçirebilirsiniz. İleti Uygulama sınırlar 200 karakter ileti uzunluğu (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` Test `Message` 201 "X" karakterleri içeren metin açıkça geçirir. Bunun bir `ModelState` hata. POST dizin sayfasına yeniden yönlendir değil. 200 Tamam ile döndüren bir `null` `Location` üstbilgi (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>Ayrıca bkz.

* [Birim testi C# .NET çekirdek dotnet test ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Tümleştirme testleri](xref:testing/integration-testing)
* [Test denetleyicileri](xref:mvc/controllers/testing)
* [Birim testi kodunuzu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [XUnit.net (.NET Core/ASP.NET çekirdek) ile çalışmaya başlama](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq hızlı başlangıç](https://github.com/Moq/moq4/wiki/Quickstart)
