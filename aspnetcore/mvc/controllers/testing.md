---
title: ASP.NET Core 'de test denetleyicisi mantığı
author: ardalis
description: Moq ve xUnit ile ASP.NET Core denetleyici mantığını test etme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: mvc/controllers/testing
ms.openlocfilehash: 449d8791962e4233d599f364b2e8c922f0975d2f
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681103"
---
# <a name="unit-test-controller-logic-in-aspnet-core"></a>ASP.NET Core 'de birim test denetleyicisi mantığı

[Steve Smith](https://ardalis.com/) tarafından

::: moniker range=">= aspnetcore-3.0"

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) , bir uygulamanın bir bölümünü altyapısından ve bağımlılıklarından yalıtımıyla test etmeyi içerir. Birim testi denetleyici mantığı olduğunda, yalnızca tek bir eylemin içerikleri test edilir, onun bağımlılıkları veya çerçevenin kendisi değildir.

## <a name="unit-testing-controllers"></a>Birim test denetleyicileri

Denetleyicinin davranışına odaklanmak için denetleyici eylemlerinin birim testlerini ayarlayın. Denetleyici birim testi [Filtreler](xref:mvc/controllers/filters), [yönlendirme](xref:fundamentals/routing)ve [model bağlama](xref:mvc/models/model-binding)gibi senaryoları önler. Bir isteğe topluca yanıt veren bileşenler arasındaki etkileşimleri kapsayan testler, *tümleştirme testleri*tarafından işlenir. Tümleştirme testleri hakkında daha fazla bilgi için bkz. <xref:test/integration-tests>.

Özel filtreler ve rotalar yazıyorsanız, birim, belirli bir denetleyici eyleminde testlerin bir parçası olarak değil, yalıtımına göre test eder.

Denetleyici birim testlerini göstermek için örnek uygulamada aşağıdaki denetleyiciyi gözden geçirin. 

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/testing/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Ana denetleyici bir beyin fırtınası oturumlarının listesini görüntüler ve yeni beyin fırtınası oturumlarının bir POST isteğiyle oluşturulmasına izin verir:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Önceki denetleyici:

* [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)izler.
* `IBrainstormSessionRepository`örneğini sağlamak için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) bekliyor.
* , [Moq](https://www.nuget.org/packages/Moq/)gibi bir sahte nesne çerçevesi kullanılarak bir moclenmiş `IBrainstormSessionRepository` hizmeti ile test edilebilir. Bir ilişkili *nesne* , test için kullanılan önceden tanımlanmış bir özellik ve Yöntem davranışları kümesine sahip bir fabricobject nesnesidir. Daha fazla bilgi için bkz. [tümleştirme testlerine giriş](xref:test/integration-tests#introduction-to-integration-tests).

`HTTP GET Index` yöntemi döngü veya dallandırma içermez ve yalnızca bir yöntem çağırır. Bu eylemin birim testi:

* `GetTestSessions` yöntemini kullanarak `IBrainstormSessionRepository` hizmetini gizler. `GetTestSessions`, tarihler ve oturum adlarıyla iki adet sahte beyin fırtınası oturumu oluşturur.
* `Index` yöntemini yürütür.
* Yöntemi tarafından döndürülen sonuç üzerinde onaylama işlemleri yapar:
  * Bir <xref:Microsoft.AspNetCore.Mvc.ViewResult> döndürülür.
  * [ViewDataDictionary. model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) bir `StormSessionViewModel`.
  * `ViewDataDictionary.Model`depolanan iki beyin fırtınası oturumu vardır.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Ana denetleyicinin `HTTP POST Index` yöntemi testleri şunları doğrular:

* [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) `false`olduğunda, eylem yöntemi uygun verilerle bir *400 hatalı istek* <xref:Microsoft.AspNetCore.Mvc.ViewResult> döndürür.
* `ModelState.IsValid` `true`:
  * Depodaki `Add` yöntemi çağrılır.
  * Doğru bağımsız değişkenlerle bir <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> döndürülür.

Geçersiz bir model durumu, aşağıdaki ilk testte gösterildiği gibi <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> kullanılarak hatalar eklenerek test edilir:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) geçerli olmadığında, bir get isteği için aynı `ViewResult` döndürülür. Test geçersiz bir modeli geçirmeye çalışmıyor. Model bağlama çalışmadığından (bir [tümleştirme testi](xref:test/integration-tests) model bağlamayı kullansa da), geçersiz bir modelin geçirilmesi geçerli bir yaklaşım değildir. Bu durumda model bağlama sınanmamıştır. Bu birim testleri yalnızca eylem yöntemindeki kodu test eder.

İkinci test `ModelState` geçerli olduğunda doğrular:

* Yeni bir `BrainstormSession` eklenir (depo aracılığıyla).
* Yöntemi, beklenen özelliklerle bir `RedirectToActionResult` döndürür.

Çağrılmayan hiçbir çağrı normalde yok sayılır, ancak kurulum çağrısının sonunda `Verifiable` çağrısı testte sahte doğrulamaya izin verir. Bu, beklenen yöntemin çağrılmaması durumunda test başarısız olan `mockRepo.Verify`çağrısıyla gerçekleştirilir.

> [!NOTE]
> Bu örnekte kullanılan moq kitaplığı, doğrulanabilir olmayan bir şekilde ("gevşek" bir veya saplamalar olarak da adlandırılır) doğrulanabilir veya "katı" olarak karışık bir şekilde karışık bir şekilde karıştırılamaz. [Moq Ile sahte davranışı özelleştirme](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)hakkında daha fazla bilgi edinin.

Örnek uygulamadaki [Sessioncontroller](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) , belirli bir beyin fırtınası oturumuyla ilgili bilgileri görüntüler. Denetleyici geçersiz `id` değerlerle ilgilenme mantığını içerir (bu senaryoları kapsayan aşağıdaki örnekte iki `return` senaryo vardır). Son `return` ifade görünüme yeni bir `StormSessionViewModel` döndürür (*Controllers/SessionController. cs*):

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Birim testleri, oturum denetleyicisi `Index` eyleminde her bir `return` senaryosu için bir test içerir:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Uygulama, fikirler denetleyicisine geçiş yaparken `api/ideas` rotasında bir Web API 'SI olarak işlevselliği kullanıma sunar:

* Bir beyin fırtınası oturumuyla ilişkili fikirler (`IdeaDTO`) listesi, `ForSession` yöntemi tarafından döndürülür.
* `Create` yöntemi bir oturuma yeni fikirler ekler.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

İş etki alanı varlıklarını doğrudan API çağrıları aracılığıyla döndürmekten kaçının. Etki alanı varlıkları:

* Genellikle istemcinin gerektirdiğinden daha fazla veri içerir.
* Genel olarak sunulan API ile uygulamanın iç etki alanı modelini gereksiz bir şekilde yapın.

Etki alanı varlıkları ve istemciye döndürülen türler arasında eşleme gerçekleştirilebilir:

* Örnek uygulamanın kullandığı şekilde bir LINQ `Select`el ile. Daha fazla bilgi için bkz. [LINQ (dil Ile tümleşik sorgu)](/dotnet/standard/using-linq).
* Otomatik olarak bir kitaplıkla (örneğin, [Automaber](https://github.com/AutoMapper/AutoMapper)).

Ardından, örnek uygulama, fikirler denetleyicisinin `Create` ve `ForSession` API yöntemlerine yönelik birim testlerini gösterir.

Örnek uygulama iki `ForSession` test içerir. İlk test, `ForSession` geçersiz bir oturum için <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (HTTP bulunamadı) döndürüp döndürmeyeceğini belirler:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

İkinci `ForSession` testi, `ForSession` geçerli bir oturum için oturum fikirleri (`<List<IdeaDTO>>`) listesini döndürüp döndürmeyeceğini belirler. Denetimler Ayrıca, `Name` özelliğinin doğru olduğunu onaylamak için ilk fikri de inceler:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

`ModelState` geçersiz olduğunda `Create` yönteminin davranışını test etmek için, örnek uygulama, testin bir parçası olarak denetleyiciye bir model hatası ekler. Birim testlerinde model doğrulama veya model bağlamayı sınamayı denemeyin&mdash;geçersiz bir `ModelState`sahip olduğunda eylem yönteminin davranışını test edin:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

`Create` ikinci testi, `null`döndüren depoya bağlıdır, bu nedenle, sahte depo `null`döndürecek şekilde yapılandırılır. Bir test veritabanı (bellekte veya başka türlü) oluşturmanız gerekmez ve bu sonucu döndüren bir sorgu oluşturun. Örnek kodun gösterildiği gibi, test tek bir bildirimde gerçekleştirilebilir:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

`Create_ReturnsNewlyCreatedIdeaForSession`üçüncü `Create` testi, deponun `UpdateAsync` yönteminin çağrıldığını doğrular. Sahte, `Verifiable`ile çağrılır ve doğrulanabilen yöntemin yürütüldüğünü onaylamak için, moclenmiş deponun `Verify` yöntemi çağırılır. `UpdateAsync` yönteminin bir tümleştirme testiyle gerçekleştirilebilecek verileri&mdash;kaydettiğinizden emin olmak için birim testinin sorumluluğu yoktur.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

## <a name="test-actionresultt"></a>Test ActionResult\<T >

ASP.NET Core 2,1 veya sonraki bir sürümde, [actionresult\<t >](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult%601>) `ActionResult` türetilen bir tür döndürmenizi veya belirli bir tür döndürmenizi sağlar.

Örnek uygulama, belirli bir oturum `id`için `List<IdeaDTO>` döndüren bir yöntemi içerir. Oturum `id` yoksa, denetleyici <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>döndürür:

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

`ForSessionActionResult` denetleyicisinin iki testi `ApiIdeasControllerTests`dahil edilir.

İlk test, denetleyicinin varolmayan bir oturum `id`için varolmayan bir fikir listesi `ActionResult` döndürdüğünü onaylar:

* `ActionResult` türü `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> bir <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

Geçerli bir oturum `id`için ikinci test, yöntemin döndürdüğünü doğrular:

* `List<IdeaDTO>` türüne sahip bir `ActionResult`.
* [ActionResult\<t >. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) bir `List<IdeaDTO>` türüdür.
* Listedeki ilk öğe, sahte oturumda saklanan fikrle eşleşen geçerli bir fikrdir (`GetTestSession`çağırarak elde edilir).

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Örnek uygulama, belirli bir oturum için yeni `Idea` oluşturmak üzere bir yöntemi de içerir. Denetleyici şunu döndürür:

* geçersiz bir model için <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>.
* oturum yoksa <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>.
* oturum yeni fikrle güncelleştirildiği zaman <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

`CreateActionResult` üç test `ApiIdeasControllerTests`dahil edilir.

İlk metin, geçersiz bir model için <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> döndürüldüğünü onaylar.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

İkinci test, oturum yoksa <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> döndürülüp döndürülmediğini denetler.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

Geçerli bir oturum `id`için son test şunları onaylar:

* Yöntemi, bir `BrainstormSession` türü ile `ActionResult` döndürür.
* [ActionResult\<t >. Sonuç](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Result*) bir <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult`, bir `Location` üst bilgisiyle *201 tarafından oluşturulan* bir yanıta benzerdir.
* [ActionResult\<t >. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) bir `BrainstormSession` türüdür.
* Oturumu güncelleştirmek için kullanılan sahte çağrı, `UpdateAsync(testSession)`çağrıldı. `Verifiable` yöntemi çağrısı onaylamalarda `mockRepo.Verify()` yürütülerek denetlenir.
* Oturum için iki `Idea` nesnesi döndürülür.
* Son öğe (sahte çağrının `UpdateAsync`tarafından eklenen `Idea`) testteki oturuma eklenen `newIdea` eşleşir.

[!code-csharp[](testing/samples/3.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Denetleyiciler](xref:mvc/controllers/actions) herhangi BIR ASP.NET Core MVC uygulamasında merkezi bir rol oynar. Bu nedenle, denetleyicilerin amaçlanan gibi davrandığına güvenmelisiniz. Otomatik testler, uygulama bir üretim ortamına dağıtılmadan önce hataları tespit edebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/testing/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Denetleyici mantığının birim testleri

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) , bir uygulamanın bir bölümünü altyapısından ve bağımlılıklarından yalıtımıyla test etmeyi içerir. Birim testi denetleyici mantığı olduğunda, yalnızca tek bir eylemin içerikleri test edilir, onun bağımlılıkları veya çerçevenin kendisi değildir.

Denetleyicinin davranışına odaklanmak için denetleyici eylemlerinin birim testlerini ayarlayın. Denetleyici birim testi [Filtreler](xref:mvc/controllers/filters), [yönlendirme](xref:fundamentals/routing)ve [model bağlama](xref:mvc/models/model-binding)gibi senaryoları önler. Bir isteğe topluca yanıt veren bileşenler arasındaki etkileşimleri kapsayan testler, *tümleştirme testleri*tarafından işlenir. Tümleştirme testleri hakkında daha fazla bilgi için bkz. <xref:test/integration-tests>.

Özel filtreler ve rotalar yazıyorsanız, birim, belirli bir denetleyici eyleminde testlerin bir parçası olarak değil, yalıtımına göre test eder.

Denetleyici birim testlerini göstermek için örnek uygulamada aşağıdaki denetleyiciyi gözden geçirin. Ana denetleyici bir beyin fırtınası oturumlarının listesini görüntüler ve yeni beyin fırtınası oturumlarının bir POST isteğiyle oluşturulmasına izin verir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Önceki denetleyici:

* [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)izler.
* `IBrainstormSessionRepository`örneğini sağlamak için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) bekliyor.
* , [Moq](https://www.nuget.org/packages/Moq/)gibi bir sahte nesne çerçevesi kullanılarak bir moclenmiş `IBrainstormSessionRepository` hizmeti ile test edilebilir. Bir ilişkili *nesne* , test için kullanılan önceden tanımlanmış bir özellik ve Yöntem davranışları kümesine sahip bir fabricobject nesnesidir. Daha fazla bilgi için bkz. [tümleştirme testlerine giriş](xref:test/integration-tests#introduction-to-integration-tests).

`HTTP GET Index` yöntemi döngü veya dallandırma içermez ve yalnızca bir yöntem çağırır. Bu eylemin birim testi:

* `GetTestSessions` yöntemini kullanarak `IBrainstormSessionRepository` hizmetini gizler. `GetTestSessions`, tarihler ve oturum adlarıyla iki adet sahte beyin fırtınası oturumu oluşturur.
* `Index` yöntemini yürütür.
* Yöntemi tarafından döndürülen sonuç üzerinde onaylama işlemleri yapar:
  * Bir <xref:Microsoft.AspNetCore.Mvc.ViewResult> döndürülür.
  * [ViewDataDictionary. model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) bir `StormSessionViewModel`.
  * `ViewDataDictionary.Model`depolanan iki beyin fırtınası oturumu vardır.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Ana denetleyicinin `HTTP POST Index` yöntemi testleri şunları doğrular:

* [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) `false`olduğunda, eylem yöntemi uygun verilerle bir *400 hatalı istek* <xref:Microsoft.AspNetCore.Mvc.ViewResult> döndürür.
* `ModelState.IsValid` `true`:
  * Depodaki `Add` yöntemi çağrılır.
  * Doğru bağımsız değişkenlerle bir <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> döndürülür.

Geçersiz bir model durumu, aşağıdaki ilk testte gösterildiği gibi <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> kullanılarak hatalar eklenerek test edilir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) geçerli olmadığında, bir get isteği için aynı `ViewResult` döndürülür. Test geçersiz bir modeli geçirmeye çalışmıyor. Model bağlama çalışmadığından (bir [tümleştirme testi](xref:test/integration-tests) model bağlamayı kullansa da), geçersiz bir modelin geçirilmesi geçerli bir yaklaşım değildir. Bu durumda model bağlama sınanmamıştır. Bu birim testleri yalnızca eylem yöntemindeki kodu test eder.

İkinci test `ModelState` geçerli olduğunda doğrular:

* Yeni bir `BrainstormSession` eklenir (depo aracılığıyla).
* Yöntemi, beklenen özelliklerle bir `RedirectToActionResult` döndürür.

Çağrılmayan hiçbir çağrı normalde yok sayılır, ancak kurulum çağrısının sonunda `Verifiable` çağrısı testte sahte doğrulamaya izin verir. Bu, beklenen yöntemin çağrılmaması durumunda test başarısız olan `mockRepo.Verify`çağrısıyla gerçekleştirilir.

> [!NOTE]
> Bu örnekte kullanılan moq kitaplığı, doğrulanabilir olmayan bir şekilde ("gevşek" bir veya saplamalar olarak da adlandırılır) doğrulanabilir veya "katı" olarak karışık bir şekilde karışık bir şekilde karıştırılamaz. [Moq Ile sahte davranışı özelleştirme](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)hakkında daha fazla bilgi edinin.

Örnek uygulamadaki [Sessioncontroller](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) , belirli bir beyin fırtınası oturumuyla ilgili bilgileri görüntüler. Denetleyici geçersiz `id` değerlerle ilgilenme mantığını içerir (bu senaryoları kapsayan aşağıdaki örnekte iki `return` senaryo vardır). Son `return` ifade görünüme yeni bir `StormSessionViewModel` döndürür (*Controllers/SessionController. cs*):

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Birim testleri, oturum denetleyicisi `Index` eyleminde her bir `return` senaryosu için bir test içerir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Uygulama, fikirler denetleyicisine geçiş yaparken `api/ideas` rotasında bir Web API 'SI olarak işlevselliği kullanıma sunar:

* Bir beyin fırtınası oturumuyla ilişkili fikirler (`IdeaDTO`) listesi, `ForSession` yöntemi tarafından döndürülür.
* `Create` yöntemi bir oturuma yeni fikirler ekler.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

İş etki alanı varlıklarını doğrudan API çağrıları aracılığıyla döndürmekten kaçının. Etki alanı varlıkları:

* Genellikle istemcinin gerektirdiğinden daha fazla veri içerir.
* Genel olarak sunulan API ile uygulamanın iç etki alanı modelini gereksiz bir şekilde yapın.

Etki alanı varlıkları ve istemciye döndürülen türler arasında eşleme gerçekleştirilebilir:

* Örnek uygulamanın kullandığı şekilde bir LINQ `Select`el ile. Daha fazla bilgi için bkz. [LINQ (dil Ile tümleşik sorgu)](/dotnet/standard/using-linq).
* Otomatik olarak bir kitaplıkla (örneğin, [Automaber](https://github.com/AutoMapper/AutoMapper)).

Ardından, örnek uygulama, fikirler denetleyicisinin `Create` ve `ForSession` API yöntemlerine yönelik birim testlerini gösterir.

Örnek uygulama iki `ForSession` test içerir. İlk test, `ForSession` geçersiz bir oturum için <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (HTTP bulunamadı) döndürüp döndürmeyeceğini belirler:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

İkinci `ForSession` testi, `ForSession` geçerli bir oturum için oturum fikirleri (`<List<IdeaDTO>>`) listesini döndürüp döndürmeyeceğini belirler. Denetimler Ayrıca, `Name` özelliğinin doğru olduğunu onaylamak için ilk fikri de inceler:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

`ModelState` geçersiz olduğunda `Create` yönteminin davranışını test etmek için, örnek uygulama, testin bir parçası olarak denetleyiciye bir model hatası ekler. Birim testlerinde model doğrulama veya model bağlamayı sınamayı denemeyin&mdash;geçersiz bir `ModelState`sahip olduğunda eylem yönteminin davranışını test edin:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

`Create` ikinci testi, `null`döndüren depoya bağlıdır, bu nedenle, sahte depo `null`döndürecek şekilde yapılandırılır. Bir test veritabanı (bellekte veya başka türlü) oluşturmanız gerekmez ve bu sonucu döndüren bir sorgu oluşturun. Örnek kodun gösterildiği gibi, test tek bir bildirimde gerçekleştirilebilir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

`Create_ReturnsNewlyCreatedIdeaForSession`üçüncü `Create` testi, deponun `UpdateAsync` yönteminin çağrıldığını doğrular. Sahte, `Verifiable`ile çağrılır ve doğrulanabilen yöntemin yürütüldüğünü onaylamak için, moclenmiş deponun `Verify` yöntemi çağırılır. `UpdateAsync` yönteminin bir tümleştirme testiyle gerçekleştirilebilecek verileri&mdash;kaydettiğinizden emin olmak için birim testinin sorumluluğu yoktur.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

## <a name="test-actionresultt"></a>Test ActionResult\<T >

ASP.NET Core 2,1 veya sonraki bir sürümde, [actionresult\<t >](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult%601>) `ActionResult` türetilen bir tür döndürmenizi veya belirli bir tür döndürmenizi sağlar.

Örnek uygulama, belirli bir oturum `id`için `List<IdeaDTO>` döndüren bir yöntemi içerir. Oturum `id` yoksa, denetleyici <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>döndürür:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

`ForSessionActionResult` denetleyicisinin iki testi `ApiIdeasControllerTests`dahil edilir.

İlk test, denetleyicinin varolmayan bir oturum `id`için varolmayan bir fikir listesi `ActionResult` döndürdüğünü onaylar:

* `ActionResult` türü `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> bir <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

Geçerli bir oturum `id`için ikinci test, yöntemin döndürdüğünü doğrular:

* `List<IdeaDTO>` türüne sahip bir `ActionResult`.
* [ActionResult\<t >. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) bir `List<IdeaDTO>` türüdür.
* Listedeki ilk öğe, sahte oturumda saklanan fikrle eşleşen geçerli bir fikrdir (`GetTestSession`çağırarak elde edilir).

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Örnek uygulama, belirli bir oturum için yeni `Idea` oluşturmak üzere bir yöntemi de içerir. Denetleyici şunu döndürür:

* geçersiz bir model için <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>.
* oturum yoksa <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>.
* oturum yeni fikrle güncelleştirildiği zaman <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

`CreateActionResult` üç test `ApiIdeasControllerTests`dahil edilir.

İlk metin, geçersiz bir model için <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> döndürüldüğünü onaylar.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

İkinci test, oturum yoksa <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> döndürülüp döndürülmediğini denetler.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

Geçerli bir oturum `id`için son test şunları onaylar:

* Yöntemi, bir `BrainstormSession` türü ile `ActionResult` döndürür.
* [ActionResult\<t >. Sonuç](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Result*) bir <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult`, bir `Location` üst bilgisiyle *201 tarafından oluşturulan* bir yanıta benzerdir.
* [ActionResult\<t >. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) bir `BrainstormSession` türüdür.
* Oturumu güncelleştirmek için kullanılan sahte çağrı, `UpdateAsync(testSession)`çağrıldı. `Verifiable` yöntemi çağrısı onaylamalarda `mockRepo.Verify()` yürütülerek denetlenir.
* Oturum için iki `Idea` nesnesi döndürülür.
* Son öğe (sahte çağrının `UpdateAsync`tarafından eklenen `Idea`) testteki oturuma eklenen `newIdea` eşleşir.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/integration-tests>
* [Visual Studio ile birim testleri oluşturma ve çalıştırma](/visualstudio/test/unit-test-your-code)
* [ASP.NET Core &ndash; MVC Için Mysınanan. AspNetCore. Mvc-Floent test Kitaplığı](https://github.com/ivaylokenov/MyTested.AspNetCore.Mvc) , MVC ve Web API uygulamalarını test etmek için akıcı bir arabirim sağlar. (*Microsoft tarafından korunmaz veya desteklenmez.* )

