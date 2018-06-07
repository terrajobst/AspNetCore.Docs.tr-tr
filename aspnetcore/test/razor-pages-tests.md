---
title: ASP.NET Core Razor sayfalarının birim testleri
author: guardrex
description: Razor sayfalarının uygulamaları için birim testleri oluşturmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/razor-pages-tests
ms.openlocfilehash: 460c35754750691d3d940dac04d06823083133c2
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734723"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>ASP.NET Core Razor sayfalarının birim testleri

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core birim testleri Razor sayfalarının uygulamaları destekler. Verilerin testleri düzeyine (DAL) erişmek ve sayfa modelleri sağlamaya yardımcı olur:

* Razor sayfalarının uygulama bölümlerini uygulama oluşturma sırasında bağımsız olarak ve bir birim olarak birlikte çalışır.
* Sınıflar ve yöntemler sorumluluk kapsamları sınırlı.
* Ek belgeleri nasıl uygulama hareket etmesi gerektiğini üzerinde bulunmaktadır.
* Güncelleştirmeleri koduna göre duruma hatalardır, gerileme otomatik yapı ve dağıtım sırasında bulunamadı.

Bu konu Razor sayfalarının uygulamaları ve birim testleri temel bilgilere sahip olduğunuzu varsayar. Razor sayfalarının uygulamaları veya test kavramları emin değilseniz aşağıdaki konulara bakın:

* [Razor sayfalarının giriş](xref:mvc/razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Birim testi C# .NET çekirdek dotnet test ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tests/razor-pages-tests/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek Proje iki uygulamaların oluşur:

| Uygulama         | Proje klasörü                        | Açıklama |
| ----------- | ------------------------------------- | ----------- |
| İleti Uygulama | *src/RazorPagesTestSample*            | Bir kullanıcı eklemek, silmek, tüm silin ve iletileri çözümlemek sağlar. |
| Test uygulama    | *tests/RazorPagesTestSample.Tests*    | Birim testi ileti uygulama için kullanılan: veri erişim katmanı (DAL) ve dizin sayfası modeli. |

Testleri bir IDE yerleşik test özelliklerini kullanarak çalıştırabilirsiniz [Visual Studio](https://www.visualstudio.com/vs/). Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırı *tests/RazorPagesTestSample.Tests* klasörü:

```console
dotnet test
```

## <a name="message-app-organization"></a>İleti Uygulama kuruluş

İleti uygulama basit bir Razor sayfalarının ileti sistemi aşağıdaki özelliklere sahip olur:

* Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) kullanıcı Arabirimi ve sayfa ekleme, silme ve iletileri (ileti başına ortalama sözcük) analizini denetlemek için modeli yöntemleri sağlar .
* Bir ileti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliklere sahip: `Id` (anahtar) ve `Text` (ileti). `Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.
* İletileri kullanarak depolanır [Entity Framework'ün bellek içi veritabanı](/ef/core/providers/in-memory/)&#8224;.
* Uygulama bir veri erişim katmanı (DAL), veritabanı bağlamı sınıfının içeren `AppDbContext` (*Data/AppDbContext.cs*). DAL yöntemleri işaretlenmiş `virtual`, testleri kullanımı için yöntemleri mocking izin verir.
* Veritabanı uygulama başlangıçta boş ise, ileti deposunu üç iletileri ile başlatıldı. Bunlar *sağlanmış iletileri* testlerinde de kullanılır.

&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), mstest'i testler için bir bellek içi veritabanı kullanımı açıklanmaktadır. Bu konuda kullanan [xUnit](https://xunit.github.io/) test çerçevesi. Test kavramları ve test uygulamaları farklı bir test çerçevelerini arasında benzer, ancak aynı değildir.

Uygulama kullanmayan ancak [havuz deseni](http://martinfowler.com/eaaCatalog/repository.html) ve etkili bir örneği değil. [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfalarının geliştirme bu desenleri destekler. Daha fazla bilgi için bkz: [altyapı saklama katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [depo ve iş desenleri bir ASP.NET MVC uygulamasındaki uygulama](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek havuz deseni uygular).

## <a name="test-app-organization"></a>Test uygulama kuruluş

İçinde bir konsol uygulaması test uygulamadır *tests/RazorPagesTestSample.Tests* klasör.

| Test uygulama klasörü | Açıklama |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* DAL için birim testleri içerir.</li><li>*IndexPageTests.cs* dizin sayfası modeli için birim testleri içerir.</li></ul> |
| *Yardımcı programlar*     | İçeren `TestingDbContextOptions` veritabanı her test için kendi temel koşul sıfırlanır her DAL birim testi için içerik seçeneklerini yeni bir veritabanı oluşturmak için kullanılan yöntem. |

Test çerçevesi [xUnit](https://xunit.github.io/). Framework mocking nesne [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Birim testleri verilerin düzeyine (DAL) erişim

İleti uygulama içinde yer alan dört yöntem bir DAL sahip `AppDbContext` sınıfı (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Her yöntemin bir veya iki birim testleri test uygulamada vardır.

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

Bu yaklaşım sorun ne olursa olsun durumu önceki test, sol içindeki her bir test veritabanı aldığını olmasıdır. Birbiriyle uğratmaz atomik birim testleri yazma girişimi sırasında bu bağlantı sorunlu olabilir. Zorlamak için `AppDbContext` her test için yeni bir veritabanı içeriği kullanmak için girmeniz bir `DbContextOptions` yeni bir hizmet sağlayıcısını temel örneği. Test uygulaması kullanarak bunu gösterilmektedir, `Utilities` sınıf yöntemi `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

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

Örneğin, `DeleteMessageAsync` yöntemi tarafından tanımlanan tek bir ileti kaldırmak için sorumlu kendi `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Bu yöntem için iki testleri vardır. Bir sınama iletisi veritabanında mevcut olduğunda yöntemi bir iletiyi siler denetler. Veritabanı varsa değişmez bir yöntem testleri ileti `Id` için silme işlemi yok. `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Yöntemi aşağıda gösterilmektedir:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

İlk olarak, yöntem, Act adım hazırlığı gerçekleştiği Yerleştir adım gerçekleştirir. Dengeli dağıtım iletiler elde ve içinde tutulan `seedMessages`. Dengeli dağıtım iletiler veritabanına kaydedilir. İletinin bir `Id` , `1` silme işlemi için ayarlanır. Zaman `DeleteMessageAsync` yöntemi yürütüldüğünde, beklenen iletiler biriyle hariç tüm iletileri olmalıdır bir `Id` , `1`. `expectedMessages` Değişkeni bu beklenen sonucu temsil eder.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Yöntem davranır: `DeleteMessageAsync` tümleştirilmesidir yöntemi yürütüldüğünde `recId` , `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Son olarak, yöntemi alır `Messages` bağlamından ve kendisine karşılaştırır `expectedMessages` iki eşit sunduğundan:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Karşılaştırmak için iki `List<Message>` aynıdır:

* İletileri göre sıralanmış `Id`.
* İleti çiftleri karşılaştırılır `Text` özelliği.

Benzer bir test yöntem `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` mevcut olmayan bir ileti silinmeye çalışılıyor sonucunu denetler. Bu durumda, beklenen veritabanındaki sonra gerçek iletileri eşit mesajlarının `DeleteMessageAsync` yöntemi yürütüldüğünde. Veritabanının içeriği için herhangi bir değişiklik olması gerekir:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Birim testleri sayfası modeli yöntemi

Başka bir birim testleri sayfası modeli yöntemleri testler için sorumlu kümesidir. Dizin Sayfası modelleri bulunan ileti uygulamada `IndexModel` sınıfını *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Sayfa modeli yöntemi | İşlev |
| ----------------- | -------- |
| `OnGetAsync` | Kullanıcı arabirimini kullanarak için DAL iletileri elde `GetMessagesAsync` yöntemi. |
| `OnPostAddMessageAsync` | Varsa `ModelState` geçerlidir, çağıran `AddMessageAsync` veritabanına iletiye ekleyin. |
| `OnPostDeleteAllMessagesAsync` | Çağrıları `DeleteAllMessagesAsync` veritabanındaki tüm iletileri silmek için. |
| `OnPostDeleteMessageAsync` | Yürütür `DeleteMessageAsync` içeren bir ileti silmek için `Id` belirtilen. |
| `OnPostAnalyzeMessagesAsync` | Bir veya daha fazla ileti veritabanında varsa, sözcük ileti başına ortalama sayısını hesaplar. |

Sayfa modeli yöntemleri yedi testler kullanarak test edildiğini `IndexPageTests` sınıfı (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Testleri tanıdık Yerleştir Assert Act desen kullanır. Bu testler odaklanır:

* Yöntemleri doğru davranış izlerseniz belirleme zaman `ModelState` geçersiz.
* Yöntemleri onaylayan üretmek doğru `IActionResult`.
* Özellik değeri atamaları doğru şekilde yapıldığını denetleniyor.

Bu grup test genellikle bir sayfa modeli yöntemi yürütüldüğü Act adımı için beklenen veri üretmek için DAL yöntemlerinin mock. Örneğin, `GetMessagesAsync` yöntemi `AppDbContext` çıktı oluşturmak için mocked. Bir sayfa modeli yöntemi bu yöntemi yürütüldüğünde, mock sonucunu döndürür. Verileri veritabanından gelmez. Bu sayfanın modeli testlerinde DAL kullanan için tahmin edilebilir, güvenilir test koşulları oluşturur.

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Test gösterir nasıl `GetMessagesAsync` yöntemi için sayfa modeli mocked:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Zaman `OnGetAsync` yöntemi Act adımda yürütülürse, sayfa modelinin çağırır `GetMessagesAsync` yöntemi.

Birim testi Act adım (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` Sayfa modelinin `OnGetAsync` yöntemi (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` DAL yönteminde değil Bu yöntem çağrısının sonucunu döndürür. Yöntemin mocked sürümü sonucunu döndürür.

İçinde `Assert` adım, gerçek iletileri (`actualMessages`) atanır `Messages` sayfa modelin özelliği. İletileri atandığında bir tür denetimi da gerçekleştirilir. Beklenen ve mevcut iletileri tarafından karşılaştırılır kendi `Text` özellikleri. Test onaylar iki `List<Message>` örnekleri aynı iletiler içerir.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Bu gruptaki diğer testleri oluşturma sayfası dahil model nesneleri `DefaultHttpContext`, `ModelStateDictionary`, bir `ActionContext` kurmak için `PageContext`, `ViewDataDictionary`ve bir `PageContext`. Bunlar, testleri gerçekleştirme faydalıdır. Örneğin, ileti uygulama kurar bir `ModelState` hatasıyla `AddModelError` denetlemek için geçerli bir `PageResult` olduğunda döndürülen `OnPostAddMessageAsync` yürütülür:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Birim testi C# .NET çekirdek dotnet test ve xUnit kullanma](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Test denetleyicileri](xref:mvc/controllers/testing)
* [Birim testi kodunuzu](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Tümleştirme testleri](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [XUnit.net (.NET Core/ASP.NET çekirdek) ile çalışmaya başlama](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq hızlı başlangıç](https://github.com/Moq/moq4/wiki/Quickstart)
