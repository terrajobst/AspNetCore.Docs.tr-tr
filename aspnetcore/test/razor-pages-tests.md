---
title: ASP.NET Core Razor sayfalar birim testleri
author: guardrex
description: Razor sayfaları uygulamaları için birim testleri oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 364c4c0fd75954f1c7e0bfbc221938afe5332bde
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756361"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>ASP.NET Core Razor sayfalar birim testleri

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core Razor sayfaları uygulamalarının birim testlerini destekler. Testleri veri erişim katmanı (DAL) ve sayfa modelleri emin olunmasını sağlar:

* Razor sayfaları uygulama bölümlerini uygulama oluşturma sırasında bağımsız olarak ve bir birim olarak birlikte çalışır.
* Sınıflar ve yöntemler sorumluluk kapsamları sınırlıdır.
* Uygulamayı nasıl davranacağı ek belgeler bulunmaktadır.
* Kod güncelleştirmeleri tarafından hazırlanmıştır hataları olan gerilemeleri otomatik yapı ve dağıtım sırasında bulunamadı.

Bu konu, Razor sayfaları uygulamaları ve birim testleri temel bir anlayışa sahip olduğunuzu varsayar. Razor sayfaları uygulamaları veya test kavramlar ile bilmiyorsanız, aşağıdaki konulara bakın:

* [Razor sayfaları giriş](xref:razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Birim testi C# .NET Core dotnet testi ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek Proje iki uygulama oluşur:

| Uygulama         | Proje klasörü                        | Açıklama |
| ----------- | ------------------------------------- | ----------- |
| İleti uygulaması | *src/RazorPagesTestSample*            | Eklemek, silmek, tüm silmek ve iletileri çözümleme açmasına olanak sağlar. |
| Test uygulaması    | *tests/RazorPagesTestSample.Tests*    | Birim testi ileti uygulaması için kullanılan: veri erişim katmanı (DAL) ve dizin sayfa modeli. |

Bir IDE özelliklerini yerleşik test gibi kullanarak testler çalıştırılabilir [Visual Studio](https://www.visualstudio.com/vs/). Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırının *tests/RazorPagesTestSample.Tests* klasörü:

```console
dotnet test
```

## <a name="message-app-organization"></a>İleti uygulaması kuruluş

İleti, aşağıdaki özelliklere sahip basit bir Razor sayfaları ileti sistemi uygulamadır:

* Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) UI ve sayfa ekleme, silme ve analiz iletilerinin (ileti başına ortalama kelimeler) denetlemek için model yöntemleri sağlar. .
* İleti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliğe sahip: `Id` (anahtar) ve `Text` (mesaj). `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletileri kullanarak depolanan [Entity Framework'ün bellek içi veritabanına](/ef/core/providers/in-memory/)&#8224;.
* Uygulama, veritabanı bağlamı sınıfının bir veri erişim katmanı (DAL) içeren `AppDbContext` (*Data/AppDbContext.cs*). DAL yöntemleri işaretlenmiş `virtual`, yöntemleri testleri kullanmak için sahte işlem izin verir.
* Uygulama başlangıcında veritabanı boşsa, ileti deposu üç iletileri ile başlatılır. Bunlar *sağlanmış iletiler* testleri de kullanılır.

&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), MSTest ile testleri için bellek içi veritabanına nasıl kullanıldığını açıklar. Bu konuda kullanan [xUnit](https://xunit.github.io/) test çerçevesi. Test kavramları ve test uygulamaları arasında farklı test çerçeveleri benzer, ancak aynı değildir.

Uygulama kullanmaz ancak [depo deseni](xref:fundamentals/repository-pattern) ve etkili bir örneği değil [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfaları geliştirme bu desenleri destekler. Daha fazla bilgi için [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), <xref:fundamentals/repository-pattern>, ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek depo Yapılacaklar listesi).

## <a name="test-app-organization"></a>Test uygulama kuruluş

Bir konsol uygulaması içinde test uygulamasıdır *tests/RazorPagesTestSample.Tests* klasör.

| Test uygulama klasörü | Açıklama |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* DAL için birim testlerini içerir.</li><li>*IndexPageTests.cs* dizin sayfa modeli için birim testlerini içerir.</li></ul> |
| *Yardımcı programları*     | İçeren `TestingDbContextOptions` veritabanı her test için kendi temel koşul sıfırlanır her DAL birim testi için içerik seçeneklerini yeni veritabanı oluşturmak için kullanılan yöntem. |

Test çerçevesi [xUnit](https://xunit.github.io/). Framework sahte işlem nesnesi [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Birim testleri veri erişim katmanı (DAL)

İleti uygulaması içinde bulunan dört yöntemleri bir DAL sahip `AppDbContext` sınıfı (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Her yöntemin bir veya iki birim testleri test uygulamada vardır.

| DAL yöntemi               | İşlev                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Alır bir `List<Message>` ölçütü veritabanından `Text` özelliği. |
| `AddMessageAsync`        | Ekler bir `Message` veritabanı.                                          |
| `DeleteAllMessagesAsync` | Tüm siler `Message` girişler veritabanından.                           |
| `DeleteMessageAsync`     | Tek bir siler `Message` tarafından veritabanından `Id`.                      |

DAL, birim testleri gerektiren [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) yeni oluştururken `AppDbContext` her test için. Oluşturma bir yaklaşım `DbContextOptions` kullanmak için her test için bir [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Bu yaklaşım sorun, her test ne olursa olsun durumu önceki testte, sol veritabanı almaya başlar. Birbiriyle uğratmaz atomik birim testleri yazma çalışırken bu sorunlara neden olabilir. Zorlamak için `AppDbContext` her test için yeni bir veritabanı bağlamı kullanmak için tedarik bir `DbContextOptions` yeni bir hizmet sağlayıcısını temel örneği. Test uygulaması kullanarak bunun nasıl yapılacağını gösterir, `Utilities` sınıfı yöntemi `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Kullanarak `DbContextOptions` DAL birimi testleri atomik olarak ile yeni veritabanı örneği çalıştırmak her bir testi sağlar:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Her test yönteminde `DataAccessLayerTest` sınıfı (*UnitTests/DataAccessLayerTest.cs*) benzer bir Düzenle-Yasası onaylama deseni izler:

1. Düzenleyin: Veritabanı test için yapılandırılır ve/veya beklenen sonucu tanımlanır.
1. ACT: Test yürütülür.
1. Assert: Onaylar, test sonucu başarılı olup olmadığını belirlemek için yapılır.

Örneğin, `DeleteMessageAsync` yöntemi tarafından tanımlanmış tek bir ileti kaldırmak için sorumlu kendi `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Bu yöntem için iki test vardır. Bir test yöntemi iletiyi veritabanında mevcut olduğunda bir ileti siler denetler. Veritabanı, değişmez bir yöntem testleri ileti `Id` için silme işlemi mevcut değil. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Yöntemi aşağıda gösterilmektedir:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

İlk olarak, yöntem Yasası adım hazırlığı gerçekleştiği düzenleme adımı gerçekleştirir. Dengeli dağıtım iletiler alınan ve tutulan `seedMessages`. Dengeli dağıtım iletiler veritabanına kaydedilir. İletinin bir `Id` , `1` silinmek üzere ayarlayın. Zaman `DeleteMessageAsync` yöntemi yürütüldüğünde, beklenen iletilerle ile dışındaki tüm iletileri sahip bir `Id` , `1`. `expectedMessages` Değişkeni bu beklenen sonucu temsil eder.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Yöntem davranır: `DeleteMessageAsync` tümleştirilmesidir yöntemi yürütüldüğünde `recId` , `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Son olarak, yöntemi alır `Messages` bağlamından ve kendisine karşılaştırır `expectedMessages` iki eşit olduğunu sunduğundan:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Bu karşılaştırma için iki `List<Message>` aynıdır:

* İletileri göre sıralanmış `Id`.
* İleti çiftleri karşılaştırılır `Text` özelliği.

Benzer bir test yöntemi `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` mevcut olmayan bir ileti silinmeye çalışılıyor sonucunu denetler. Bu durumda, beklenen iletilerle veritabanındaki gerçek iletilere sonra eşit olmalıdır `DeleteMessageAsync` yöntemi yürütülür. Veritabanının içeriği için herhangi bir değişiklik olması gerekir:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Sayfa modeli yöntemlerin birim testleri

Başka bir dizi birim testi için sayfa modeli yöntemlerin testleri sorumludur. Dizin Sayfası modelleri bulunan ileti uygulamada `IndexModel` sınıfını *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Sayfa modeli yöntemi | İşlev |
| ----------------- | -------- |
| `OnGetAsync` | Kullanıcı arabirimini kullanarak için DAL iletileri elde `GetMessagesAsync` yöntemi. |
| `OnPostAddMessageAsync` | Varsa `ModelState` geçerlidir, çağıran `AddMessageAsync` iletiye eklemek için. |
| `OnPostDeleteAllMessagesAsync` | Çağrıları `DeleteAllMessagesAsync` veritabanındaki tüm iletileri silmek için. |
| `OnPostDeleteMessageAsync` | Yürütür `DeleteMessageAsync` içeren bir ileti silinecek `Id` belirtilen. |
| `OnPostAnalyzeMessagesAsync` | Bir veya daha fazla ileti veritabanında bulunan, sözcük ileti başına ortalama sayısını hesaplar. |

Sayfa modeli yöntemleri yedi testler kullanarak test `IndexPageTests` sınıfı (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Testleri tanıdık Yerleştir Assert Yasası deseni kullanır. Bu testleri odaklanır:

* Yöntemleri doğru davranışı izlerseniz belirleyen zaman `ModelState` geçersiz.
* Yöntemleri onaylayan üretmek doğru `IActionResult`.
* Özellik değeri atamaları doğru şekilde yapıldığını denetleniyor.

Bu grup test genellikle sahte sayfa modeli yöntemi yürütüldüğü Yasası adımı için beklenen verileri üretmek üzere DAL yöntemleri. Örneğin, `GetMessagesAsync` yöntemi `AppDbContext` çıktı oluşturmak için örnek. Sayfa modeli yöntemi bu yöntemi yürütüldüğünde, sahte sonucunu döndürür. Verileri veritabanından gelmez. Bu DAL sayfa modeli testler kullanarak öngörülebilir, güvenilir test koşulları oluşturur.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Test gösterir nasıl `GetMessagesAsync` yöntemi sayfa modeli için örnek:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Zaman `OnGetAsync` Yasası adımda yöntemi yürütüldüğünde, sayfa modelin çağırır `GetMessagesAsync` yöntemi.

Birim testi Yasası adım (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` Sayfa modeli `OnGetAsync` yöntemi (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` DAL yöntemi, bu yöntem çağrısı için sonuç döndürmez. Sahte sürüm yöntemin sonucunu döndürür.

İçinde `Assert` adım, gerçek iletileri (`actualMessages`) atanır `Messages` sayfa modeli özelliğidir. İletileri atandığında bir tür denetimi de gerçekleştirilir. Beklenen ve gerçek iletileri ile karşılaştırıldığında bunların `Text` özellikleri. Test, onaylar iki `List<Message>` örnekleri aynı iletileri içerir.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Bu gruptaki diğer testleri sayfası içeren model nesneleri oluşturma `DefaultHttpContext`, `ModelStateDictionary`e `ActionContext` kurmaya `PageContext`, `ViewDataDictionary`ve `PageContext`. Bu testler yürütmek de kullanışlıdır. Örneğin, ileti uygulaması oluşturur bir `ModelState` hatasıyla `AddModelError` denetlemek için geçerli bir `PageResult` olduğunda döndürülen `OnPostAddMessageAsync` çalıştırılır:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testi C# .NET Core dotnet testi ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Test denetleyicileri](xref:mvc/controllers/testing)
* [Birim testi kod](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Tümleştirme testleri](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [XUnit.net (.NET Core Core/ASP.NET) ile çalışmaya başlama](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq hızlı başlangıç](https://github.com/Moq/moq4/wiki/Quickstart)
