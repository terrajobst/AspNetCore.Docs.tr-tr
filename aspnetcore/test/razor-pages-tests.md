---
title: ASP.NET Core Razor sayfalar birim testleri
author: guardrex
description: Razor sayfaları uygulamaları için birim testleri oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 924908a92eea23fd2dc81a3809e74760d9295e4f
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477416"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="32199-103">ASP.NET Core Razor sayfalar birim testleri</span><span class="sxs-lookup"><span data-stu-id="32199-103">Razor Pages unit tests in ASP.NET Core</span></span>

<span data-ttu-id="32199-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="32199-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="32199-105">ASP.NET Core Razor sayfaları uygulamalarının birim testlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="32199-105">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="32199-106">Testleri veri erişim katmanı (DAL) ve sayfa modelleri emin olunmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="32199-106">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="32199-107">Razor sayfaları uygulama bölümlerini uygulama oluşturma sırasında bağımsız olarak ve bir birim olarak birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="32199-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="32199-108">Sınıflar ve yöntemler sorumluluk kapsamları sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="32199-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="32199-109">Uygulamayı nasıl davranacağı ek belgeler bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="32199-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="32199-110">Kod güncelleştirmeleri tarafından hazırlanmıştır hataları olan gerilemeleri otomatik yapı ve dağıtım sırasında bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="32199-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="32199-111">Bu konu, Razor sayfaları uygulamaları ve birim testleri temel bir anlayışa sahip olduğunuzu varsayar.</span><span class="sxs-lookup"><span data-stu-id="32199-111">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="32199-112">Razor sayfaları uygulamaları veya test kavramlar ile bilmiyorsanız, aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="32199-112">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* [<span data-ttu-id="32199-113">Razor sayfaları giriş</span><span class="sxs-lookup"><span data-stu-id="32199-113">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="32199-114">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="32199-114">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="32199-115">Birim testi C# .NET Core dotnet testi ve xUnit kullanma</span><span class="sxs-lookup"><span data-stu-id="32199-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="32199-116">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32199-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="32199-117">Örnek Proje iki uygulama oluşur:</span><span class="sxs-lookup"><span data-stu-id="32199-117">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="32199-118">Uygulama</span><span class="sxs-lookup"><span data-stu-id="32199-118">App</span></span>         | <span data-ttu-id="32199-119">Proje klasörü</span><span class="sxs-lookup"><span data-stu-id="32199-119">Project folder</span></span>                        | <span data-ttu-id="32199-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="32199-120">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="32199-121">İleti uygulaması</span><span class="sxs-lookup"><span data-stu-id="32199-121">Message app</span></span> | <span data-ttu-id="32199-122">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="32199-122">*src/RazorPagesTestSample*</span></span>            | <span data-ttu-id="32199-123">Eklemek, silmek, tüm silmek ve iletileri çözümleme açmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="32199-123">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="32199-124">Test uygulaması</span><span class="sxs-lookup"><span data-stu-id="32199-124">Test app</span></span>    | <span data-ttu-id="32199-125">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="32199-125">*tests/RazorPagesTestSample.Tests*</span></span>    | <span data-ttu-id="32199-126">Birim testi ileti uygulaması için kullanılan: veri erişim katmanı (DAL) ve dizin sayfa modeli.</span><span class="sxs-lookup"><span data-stu-id="32199-126">Used to unit test the message app: Data access layer (DAL) and Index page model.</span></span> |

<span data-ttu-id="32199-127">Bir IDE özelliklerini yerleşik test gibi kullanarak testler çalıştırılabilir [Visual Studio](https://www.visualstudio.com/vs/).</span><span class="sxs-lookup"><span data-stu-id="32199-127">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="32199-128">Kullanıyorsanız [Visual Studio Code](https://code.visualstudio.com/) veya bir komut isteminde aşağıdaki komutu yürütün komut satırının *tests/RazorPagesTestSample.Tests* klasörü:</span><span class="sxs-lookup"><span data-stu-id="32199-128">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="32199-129">İleti uygulaması kuruluş</span><span class="sxs-lookup"><span data-stu-id="32199-129">Message app organization</span></span>

<span data-ttu-id="32199-130">İleti, aşağıdaki özelliklere sahip basit bir Razor sayfaları ileti sistemi uygulamadır:</span><span class="sxs-lookup"><span data-stu-id="32199-130">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="32199-131">Uygulama dizin sayfasına (*Pages/Index.cshtml* ve *Pages/Index.cshtml.cs*) UI ve sayfa ekleme, silme ve analiz iletilerinin (ileti başına ortalama kelimeler) denetlemek için model yöntemleri sağlar. .</span><span class="sxs-lookup"><span data-stu-id="32199-131">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="32199-132">İleti tarafından açıklanan `Message` sınıfı (*Data/Message.cs*) iki özelliğe sahip: `Id` (anahtar) ve `Text` (mesaj).</span><span class="sxs-lookup"><span data-stu-id="32199-132">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="32199-133">`Text` Özelliği gereklidir ve 200 karakterle sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="32199-133">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="32199-134">İletileri kullanarak depolanan [Entity Framework'ün bellek içi veritabanına](/ef/core/providers/in-memory/)&#8224;.</span><span class="sxs-lookup"><span data-stu-id="32199-134">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="32199-135">Uygulama, veritabanı bağlamı sınıfının bir veri erişim katmanı (DAL) içeren `AppDbContext` (*Data/AppDbContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="32199-135">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="32199-136">DAL yöntemleri işaretlenmiş `virtual`, yöntemleri testleri kullanmak için sahte işlem izin verir.</span><span class="sxs-lookup"><span data-stu-id="32199-136">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="32199-137">Uygulama başlangıcında veritabanı boşsa, ileti deposu üç iletileri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="32199-137">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="32199-138">Bunlar *sağlanmış iletiler* testleri de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="32199-138">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="32199-139">&#8224;EF konu [Inmemory ile Test](/ef/core/miscellaneous/testing/in-memory), MSTest ile testleri için bellek içi veritabanına nasıl kullanıldığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="32199-139">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="32199-140">Bu konuda kullanan [xUnit](https://xunit.github.io/) test çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="32199-140">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="32199-141">Test kavramları ve test uygulamaları arasında farklı test çerçeveleri benzer, ancak aynı değildir.</span><span class="sxs-lookup"><span data-stu-id="32199-141">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="32199-142">Uygulama deposu düzeni kullanmaz ve etkili bir örneği değil ancak [iş birimi (UoW) deseni](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor sayfaları geliştirme bu desenleri destekler.</span><span class="sxs-lookup"><span data-stu-id="32199-142">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="32199-143">Daha fazla bilgi için [altyapı Kalıcılık katmanını tasarlama](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) ve [Test denetleyicisi mantığı](/aspnet/core/mvc/controllers/testing) (örnek depo Yapılacaklar listesi).</span><span class="sxs-lookup"><span data-stu-id="32199-143">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="32199-144">Test uygulama kuruluş</span><span class="sxs-lookup"><span data-stu-id="32199-144">Test app organization</span></span>

<span data-ttu-id="32199-145">Bir konsol uygulaması içinde test uygulamasıdır *tests/RazorPagesTestSample.Tests* klasör.</span><span class="sxs-lookup"><span data-stu-id="32199-145">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="32199-146">Test uygulama klasörü</span><span class="sxs-lookup"><span data-stu-id="32199-146">Test app folder</span></span> | <span data-ttu-id="32199-147">Açıklama</span><span class="sxs-lookup"><span data-stu-id="32199-147">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="32199-148">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="32199-148">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="32199-149">*DataAccessLayerTest.cs* DAL için birim testlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="32199-149">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="32199-150">*IndexPageTests.cs* dizin sayfa modeli için birim testlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="32199-150">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="32199-151">*Yardımcı programları*</span><span class="sxs-lookup"><span data-stu-id="32199-151">*Utilities*</span></span>     | <span data-ttu-id="32199-152">İçeren `TestingDbContextOptions` veritabanı her test için kendi temel koşul sıfırlanır her DAL birim testi için içerik seçeneklerini yeni veritabanı oluşturmak için kullanılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="32199-152">Contains the `TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="32199-153">Test çerçevesi [xUnit](https://xunit.github.io/).</span><span class="sxs-lookup"><span data-stu-id="32199-153">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="32199-154">Framework sahte işlem nesnesi [Moq](https://github.com/moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="32199-154">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="32199-155">Birim testleri veri erişim katmanı (DAL)</span><span class="sxs-lookup"><span data-stu-id="32199-155">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="32199-156">İleti uygulaması içinde bulunan dört yöntemleri bir DAL sahip `AppDbContext` sınıfı (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="32199-156">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="32199-157">Her yöntemin bir veya iki birim testleri test uygulamada vardır.</span><span class="sxs-lookup"><span data-stu-id="32199-157">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="32199-158">DAL yöntemi</span><span class="sxs-lookup"><span data-stu-id="32199-158">DAL method</span></span>               | <span data-ttu-id="32199-159">İşlev</span><span class="sxs-lookup"><span data-stu-id="32199-159">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="32199-160">Alır bir `List<Message>` ölçütü veritabanından `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="32199-160">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="32199-161">Ekler bir `Message` veritabanı.</span><span class="sxs-lookup"><span data-stu-id="32199-161">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="32199-162">Tüm siler `Message` girişler veritabanından.</span><span class="sxs-lookup"><span data-stu-id="32199-162">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="32199-163">Tek bir siler `Message` tarafından veritabanından `Id`.</span><span class="sxs-lookup"><span data-stu-id="32199-163">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="32199-164">DAL, birim testleri gerektiren [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) yeni oluştururken `AppDbContext` her test için.</span><span class="sxs-lookup"><span data-stu-id="32199-164">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="32199-165">Oluşturma bir yaklaşım `DbContextOptions` kullanmak için her test için bir [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="32199-165">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="32199-166">Bu yaklaşım sorun, her test ne olursa olsun durumu önceki testte, sol veritabanı almaya başlar.</span><span class="sxs-lookup"><span data-stu-id="32199-166">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="32199-167">Birbiriyle uğratmaz atomik birim testleri yazma çalışırken bu sorunlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="32199-167">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="32199-168">Zorlamak için `AppDbContext` her test için yeni bir veritabanı bağlamı kullanmak için tedarik bir `DbContextOptions` yeni bir hizmet sağlayıcısını temel örneği.</span><span class="sxs-lookup"><span data-stu-id="32199-168">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="32199-169">Test uygulaması kullanarak bunun nasıl yapılacağını gösterir, `Utilities` sınıfı yöntemi `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="32199-169">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="32199-170">Kullanarak `DbContextOptions` DAL birimi testleri atomik olarak ile yeni veritabanı örneği çalıştırmak her bir testi sağlar:</span><span class="sxs-lookup"><span data-stu-id="32199-170">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="32199-171">Her test yönteminde `DataAccessLayerTest` sınıfı (*UnitTests/DataAccessLayerTest.cs*) benzer bir Düzenle-Yasası onaylama deseni izler:</span><span class="sxs-lookup"><span data-stu-id="32199-171">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="32199-172">Düzenleyin: Veritabanı test için yapılandırılır ve/veya beklenen sonucu tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="32199-172">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="32199-173">ACT: Test yürütülür.</span><span class="sxs-lookup"><span data-stu-id="32199-173">Act: The test is executed.</span></span>
1. <span data-ttu-id="32199-174">Assert: Onaylar, test sonucu başarılı olup olmadığını belirlemek için yapılır.</span><span class="sxs-lookup"><span data-stu-id="32199-174">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="32199-175">Örneğin, `DeleteMessageAsync` yöntemi tarafından tanımlanmış tek bir ileti kaldırmak için sorumlu kendi `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="32199-175">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="32199-176">Bu yöntem için iki test vardır.</span><span class="sxs-lookup"><span data-stu-id="32199-176">There are two tests for this method.</span></span> <span data-ttu-id="32199-177">Bir test yöntemi iletiyi veritabanında mevcut olduğunda bir ileti siler denetler.</span><span class="sxs-lookup"><span data-stu-id="32199-177">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="32199-178">Veritabanı, değişmez bir yöntem testleri ileti `Id` için silme işlemi mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="32199-178">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="32199-179">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` Yöntemi aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="32199-179">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="32199-180">İlk olarak, yöntem Yasası adım hazırlığı gerçekleştiği düzenleme adımı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="32199-180">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="32199-181">Dengeli dağıtım iletiler alınan ve tutulan `seedMessages`.</span><span class="sxs-lookup"><span data-stu-id="32199-181">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="32199-182">Dengeli dağıtım iletiler veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="32199-182">The seeding messages are saved into the database.</span></span> <span data-ttu-id="32199-183">İletinin bir `Id` , `1` silinmek üzere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="32199-183">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="32199-184">Zaman `DeleteMessageAsync` yöntemi yürütüldüğünde, beklenen iletilerle ile dışındaki tüm iletileri sahip bir `Id` , `1`.</span><span class="sxs-lookup"><span data-stu-id="32199-184">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="32199-185">`expectedMessages` Değişkeni bu beklenen sonucu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="32199-185">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="32199-186">Yöntem davranır: `DeleteMessageAsync` tümleştirilmesidir yöntemi yürütüldüğünde `recId` , `1`:</span><span class="sxs-lookup"><span data-stu-id="32199-186">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="32199-187">Son olarak, yöntemi alır `Messages` bağlamından ve kendisine karşılaştırır `expectedMessages` iki eşit olduğunu sunduğundan:</span><span class="sxs-lookup"><span data-stu-id="32199-187">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="32199-188">Bu karşılaştırma için iki `List<Message>` aynıdır:</span><span class="sxs-lookup"><span data-stu-id="32199-188">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="32199-189">İletileri göre sıralanmış `Id`.</span><span class="sxs-lookup"><span data-stu-id="32199-189">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="32199-190">İleti çiftleri karşılaştırılır `Text` özelliği.</span><span class="sxs-lookup"><span data-stu-id="32199-190">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="32199-191">Benzer bir test yöntemi `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` mevcut olmayan bir ileti silinmeye çalışılıyor sonucunu denetler.</span><span class="sxs-lookup"><span data-stu-id="32199-191">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="32199-192">Bu durumda, beklenen iletilerle veritabanındaki gerçek iletilere sonra eşit olmalıdır `DeleteMessageAsync` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="32199-192">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="32199-193">Veritabanının içeriği için herhangi bir değişiklik olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="32199-193">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="32199-194">Sayfa modeli yöntemlerin birim testleri</span><span class="sxs-lookup"><span data-stu-id="32199-194">Unit tests of the page model methods</span></span>

<span data-ttu-id="32199-195">Başka bir dizi birim testi için sayfa modeli yöntemlerin testleri sorumludur.</span><span class="sxs-lookup"><span data-stu-id="32199-195">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="32199-196">Dizin Sayfası modelleri bulunan ileti uygulamada `IndexModel` sınıfını *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="32199-196">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="32199-197">Sayfa modeli yöntemi</span><span class="sxs-lookup"><span data-stu-id="32199-197">Page model method</span></span> | <span data-ttu-id="32199-198">İşlev</span><span class="sxs-lookup"><span data-stu-id="32199-198">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="32199-199">Kullanıcı arabirimini kullanarak için DAL iletileri elde `GetMessagesAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="32199-199">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="32199-200">Varsa `ModelState` geçerlidir, çağıran `AddMessageAsync` iletiye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="32199-200">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="32199-201">Çağrıları `DeleteAllMessagesAsync` veritabanındaki tüm iletileri silmek için.</span><span class="sxs-lookup"><span data-stu-id="32199-201">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="32199-202">Yürütür `DeleteMessageAsync` içeren bir ileti silinecek `Id` belirtilen.</span><span class="sxs-lookup"><span data-stu-id="32199-202">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="32199-203">Bir veya daha fazla ileti veritabanında bulunan, sözcük ileti başına ortalama sayısını hesaplar.</span><span class="sxs-lookup"><span data-stu-id="32199-203">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="32199-204">Sayfa modeli yöntemleri yedi testler kullanarak test `IndexPageTests` sınıfı (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span><span class="sxs-lookup"><span data-stu-id="32199-204">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="32199-205">Testleri tanıdık Yerleştir Assert Yasası deseni kullanır.</span><span class="sxs-lookup"><span data-stu-id="32199-205">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="32199-206">Bu testleri odaklanır:</span><span class="sxs-lookup"><span data-stu-id="32199-206">These tests focus on:</span></span>

* <span data-ttu-id="32199-207">Yöntemleri doğru davranışı izlerseniz belirleyen zaman `ModelState` geçersiz.</span><span class="sxs-lookup"><span data-stu-id="32199-207">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="32199-208">Yöntemleri onaylayan üretmek doğru `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="32199-208">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="32199-209">Özellik değeri atamaları doğru şekilde yapıldığını denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="32199-209">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="32199-210">Bu grup test genellikle sahte sayfa modeli yöntemi yürütüldüğü Yasası adımı için beklenen verileri üretmek üzere DAL yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="32199-210">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="32199-211">Örneğin, `GetMessagesAsync` yöntemi `AppDbContext` çıktı oluşturmak için örnek.</span><span class="sxs-lookup"><span data-stu-id="32199-211">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="32199-212">Sayfa modeli yöntemi bu yöntemi yürütüldüğünde, sahte sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="32199-212">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="32199-213">Verileri veritabanından gelmez.</span><span class="sxs-lookup"><span data-stu-id="32199-213">The data doesn't come from the database.</span></span> <span data-ttu-id="32199-214">Bu DAL sayfa modeli testler kullanarak öngörülebilir, güvenilir test koşulları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="32199-214">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="32199-215">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` Test gösterir nasıl `GetMessagesAsync` yöntemi sayfa modeli için örnek:</span><span class="sxs-lookup"><span data-stu-id="32199-215">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="32199-216">Zaman `OnGetAsync` Yasası adımda yöntemi yürütüldüğünde, sayfa modelin çağırır `GetMessagesAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="32199-216">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="32199-217">Birim testi Yasası adım (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span><span class="sxs-lookup"><span data-stu-id="32199-217">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="32199-218">`IndexPage` Sayfa modeli `OnGetAsync` yöntemi (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="32199-218">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="32199-219">`GetMessagesAsync` DAL yöntemi, bu yöntem çağrısı için sonuç döndürmez.</span><span class="sxs-lookup"><span data-stu-id="32199-219">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="32199-220">Sahte sürüm yöntemin sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="32199-220">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="32199-221">İçinde `Assert` adım, gerçek iletileri (`actualMessages`) atanır `Messages` sayfa modeli özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="32199-221">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="32199-222">İletileri atandığında bir tür denetimi de gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="32199-222">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="32199-223">Beklenen ve gerçek iletileri ile karşılaştırıldığında bunların `Text` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="32199-223">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="32199-224">Test, onaylar iki `List<Message>` örnekleri aynı iletileri içerir.</span><span class="sxs-lookup"><span data-stu-id="32199-224">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="32199-225">Bu gruptaki diğer testleri sayfası içeren model nesneleri oluşturma `DefaultHttpContext`, `ModelStateDictionary`e `ActionContext` kurmaya `PageContext`, `ViewDataDictionary`ve `PageContext`.</span><span class="sxs-lookup"><span data-stu-id="32199-225">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="32199-226">Bu testler yürütmek de kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="32199-226">These are useful in conducting tests.</span></span> <span data-ttu-id="32199-227">Örneğin, ileti uygulaması oluşturur bir `ModelState` hatasıyla `AddModelError` denetlemek için geçerli bir `PageResult` olduğunda döndürülen `OnPostAddMessageAsync` çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="32199-227">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="32199-228">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="32199-228">Additional resources</span></span>

* [<span data-ttu-id="32199-229">Birim testi C# .NET Core dotnet testi ve xUnit kullanma</span><span class="sxs-lookup"><span data-stu-id="32199-229">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="32199-230">Test denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="32199-230">Test controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="32199-231">[Birim testi kod](/visualstudio/test/unit-test-your-code) (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="32199-231">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="32199-232">Tümleştirme testleri</span><span class="sxs-lookup"><span data-stu-id="32199-232">Integration tests</span></span>](xref:test/integration-tests)
* [<span data-ttu-id="32199-233">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="32199-233">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="32199-234">XUnit.net (.NET Core Core/ASP.NET) ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="32199-234">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="32199-235">Moq</span><span class="sxs-lookup"><span data-stu-id="32199-235">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="32199-236">Moq hızlı başlangıç</span><span class="sxs-lookup"><span data-stu-id="32199-236">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
