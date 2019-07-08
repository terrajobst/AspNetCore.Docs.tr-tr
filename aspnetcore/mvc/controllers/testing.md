---
title: ASP.NET Core denetleyicisi mantıksal test
author: ardalis
description: ASP.NET Core Moq ve xUnit ile test denetleyicisi mantıksal öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2019
uid: mvc/controllers/testing
ms.openlocfilehash: 8dd2fc5d581dbcb11afbcdc0c154c0e2640f9259
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622736"
---
# <a name="test-controller-logic-in-aspnet-core"></a>ASP.NET Core denetleyicisi mantıksal test

Tarafından [Steve Smith](https://ardalis.com/)

[Denetleyicileri](xref:mvc/controllers/actions) merkezi bir rol herhangi bir ASP.NET Core MVC uygulamasında Yürüt. Bu nedenle, denetleyicileri beklendiği gibi davranan bir güven olması gerekir. Uygulamayı bir üretim ortamına dağıtılmadan önce otomatikleştirilmiş testleri hatalar da algılanabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Denetleyici mantıksal birim testleri

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) bir uygulamanın bir parçası, altyapısını ve bağımlılıklarını yalıtımdan test içerir. Test denetleyicisi mantıksal birimi, yalnızca tek bir eylem içeriği olduğunda değil davranışı bağımlılıklarından biri veya framework'ün test.

Denetleyicinin davranışına odaklanmak için denetleyici eylemleri, birim testleri ayarlayın. Bir denetleyici birim testi senaryoları gibi önler [filtreleri](xref:mvc/controllers/filters), [yönlendirme](xref:fundamentals/routing), ve [model bağlama](xref:mvc/models/model-binding). Tarafından işlenmesini kapsayan topluca bir isteğe yanıt bileşenleri arasındaki etkileşimdir testleri *tümleştirme testleri*. Tümleştirme testleri hakkında daha fazla bilgi için bkz. <xref:test/integration-tests>.

Özel Filtreler ve yollar yazıyorsanız, birim testi bunları ayrı, testleri belirli bir denetleyici eylemi bir parçası olarak değil.

Denetleyici birim testleri göstermek için örnek uygulama aşağıdaki denetleyicide gözden geçirin. Giriş denetleyicisine oturumları fırtınası listesini görüntüler ve bir POST isteği ile yeni fırtınası oturumlarının oluşturulmasına izin verir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

Önceki denetleyici:

* Aşağıdaki [özel bağımlılıklar ilkesine](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).
* Bekliyor [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) bir örneğini sağlamak için `IBrainstormSessionRepository`.
* Test edilebilir bir sahte ile `IBrainstormSessionRepository` gibi bir sahte nesne çerçevesi kullanılarak hizmet [Moq](https://www.nuget.org/packages/Moq/). A *sahte nesne* test etmek için kullanılan özellik ve yöntem davranışlarını önceden belirlenmiş bir dizi üretilmiş bir nesnedir. Daha fazla bilgi için [tümleştirme testleri giriş](xref:test/integration-tests#introduction-to-integration-tests).

`HTTP GET Index` Yöntemi hiçbir döngü ya da dallanma ve yalnızca çağrıları bir yöntemi vardır. Bu eylem için birim testi:

* Mocks `IBrainstormSessionRepository` kullanarak hizmet `GetTestSessions` yöntemi. `GetTestSessions` tarihler ve oturum adları ile iki sahte beyin fırtınasını oturumu oluşturur.
* Yürütür `Index` yöntemi.
* Onaylamalar yöntemi tarafından döndürülen sonuç sağlar:
  * A <xref:Microsoft.AspNetCore.Mvc.ViewResult> döndürülür.
  * [ViewDataDictionary.Model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) olduğu bir `StormSessionViewModel`.
  * İki fırtınası oturumları burada depolanır `ViewDataDictionary.Model`.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

Giriş denetleyicinin `HTTP POST Index` yöntemi testleri doğrular:

* Zaman [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) olduğu `false`, eylem yöntemine döndürür bir *400 Hatalı istek* <xref:Microsoft.AspNetCore.Mvc.ViewResult> uygun verileri.
* Zaman `ModelState.IsValid` olduğu `true`:
  * `Add` Deposunda yöntemi çağrılır.
  * A <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult> doğru bağımsız değişkenleri ile döndürülür.

Geçersiz model durumu hataları kullanarak ekleyerek test <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> ilk test aşağıda gösterildiği gibi:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

Zaman [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) geçersiz, aynı `ViewResult` bir GET isteği olduğu gibi döndürülür. Test, geçersiz bir modelde geçirmek çalışmaz. Geçersiz bir model geçirme olmadığından geçerli bir yaklaşım, model bağlama çalışmadığından bu yana (rağmen bir [tümleştirme test](xref:test/integration-tests) model bağlama kullanın). Bu durumda, model bağlama test değil. Bu birim testlerini kod içinde eylem yönteminin yalnızca test edersiniz.

O zaman ikinci test doğrular `ModelState` geçerlidir:

* Yeni bir `BrainstormSession` (depo) eklenir.
* Yöntem döndürür bir `RedirectToActionResult` beklenen özelliklere sahip.

Adı olmayan sahte çağrıları normalde yoksayıldı, ancak arama `Verifiable` Kurulum sonunda çağrı sahte doğrulama testinde izin verir. Bu çağrı ile gerçekleştirilir `mockRepo.Verify`, beklenen yöntemi çağrılırsa değildi testi başarısız.

> [!NOTE]
> Bu örnekte kullanılan Moq kitaplığı doğrulanabilir ya da "strict" mocks doğrulanamaz mocks ("belirsiz" mocks veya yer tutucular olarak da bilinir) ile birlikte mümkün kılar. Daha fazla bilgi edinin [Moq ile sahte davranışını özelleştirme](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

[SessionController](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/controllers/testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs) örnek uygulama belirli fırtınası oturumuyla ilgili bilgileri görüntüler. Denetleyici geçersiz dağıtılacak mantığı içerir `id` değerleri (iki `return` bu senaryolarınızı kapsaması amacıyla aşağıdaki örnek senaryolarda). En son `return` yeni bir ifade döndürür `StormSessionViewModel` görünümüne (*Controllers/SessionController.cs*):

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

Birim testleri içeren her bir test `return` oturumu denetleyicisi senaryoda `Index` eylem:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

Fikirlerinizi denetleyiciye taşıma, uygulamayı bir web API işlevleri üzerinde kullanıma sunar. `api/ideas` yolu:

* Fikirlerinizi listesini (`IdeaDTO`) ilişkili bir fırtınası ile oturum tarafından döndürülen `ForSession` yöntemi.
* `Create` Yöntemi yeni fikirleri oturuma ekler.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

API çağrıları doğrudan iş etki alanı varlığı geri dönmekten kaçının. Etki alanı varlıklarının:

* Genellikle istemcinin ihtiyaç duyduğu daha fazla veri içerir.
* Gereksiz yere uygulamanın iç etki alanı modeli herkese açık bir API ile birleştirin.

Etki alanı varlıklarının istemciye döndürülen türleri arasında eşleme gerçekleştirilebilir:

* El ile bir LINQ `Select`, örnek uygulamayı kullanır. Daha fazla bilgi için [LINQ (dil ile tümleşik sorgu)](/dotnet/standard/using-linq).
* Bir kitaplığı ile otomatik olarak gibi [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Ardından, birim testleri için örnek uygulamayı gösterir `Create` ve `ForSession` fikirleri denetleyicinin API yöntemleri.

Örnek uygulamayı iki tane `ForSession` testleri. İlk test belirler `ForSession` döndürür bir <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (HTTP bulunamadı) geçersiz bir oturum için:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

İkinci `ForSession` test belirler, `ForSession` oturumu fikirleri listesini döndürür (`<List<IdeaDTO>>`) geçerli bir oturum için. Denetimleri de onaylamak için ilk fikir inceleyin, `Name` özelliği doğru:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

Davranışını test etmek için `Create` yöntemi zaman `ModelState` olan geçersiz, örnek uygulamayı model hatası denetleyiciye testin bir parçası ekler. Model doğrulama testi veya birim testleri bağlamasında model çalışmayın&mdash;yalnızca eylem yöntemin davranışını geçersiz ile karşılaşıldığında test `ModelState`:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

İkinci testi `Create` döndüren havuzda bağlıdır `null`, sahte depo döndürmek için yapılandırılmış `null`. Bir test veritabanı oluşturmaya gerek yoktur (bellekte veya başka türlü) ve bu sonuç döndüren bir sorgu oluşturun. Örnek kodu gösterildiği gibi tek bir deyimde test gerçekleştirilebilir:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

Üçüncü `Create` testi `Create_ReturnsNewlyCreatedIdeaForSession`, doğrular deponun `UpdateAsync` yöntemi çağrılır. Sahte çağrılır `Verifiable`ve sahte deponun `Verify` yöntemi doğrulanabilir yöntemi yürütüldüğünde doğrulamak için çağrılır. Birim testin sorumluluk emin olmak için değil `UpdateAsync` yöntemi kaydedilmiş veri&mdash;ile bir tümleştirme testini gerçekleştirilebilir.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

## <a name="test-actionresultlttgt"></a>Test ActionResult&lt;T&gt;

ASP.NET Core 2.1 veya daha sonra [actionresult öğesini&lt;T&gt; ](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult%601>) dönüş türetme bir türü sayesinde `ActionResult` veya belirli bir tür döndürür.

Örnek uygulamayı döndüren bir yöntem içerir. bir `List<IdeaDTO>` belirli bir oturum için `id`. Varsa oturum `id` , denetleyici döndürür yok <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>:

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

İki test `ForSessionActionResult` denetleyicisi dahil edilecek `ApiIdeasControllerTests`.

İlk test denetleyicisi döndürdüğünü onaylar bir `ActionResult` ancak varolmayan bir oturum için fikir değil varolmayan bir listesi `id`:

* `ActionResult` Türü `ActionResult<List<IdeaDTO>>`.
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> Olduğu bir <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

Geçerli bir oturum için `id`, ikinci test yöntemi döndürdüğünü onaylar:

* Bir `ActionResult` ile bir `List<IdeaDTO>` türü.
* [Actionresult öğesini&lt;T&gt;. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) olduğu bir `List<IdeaDTO>` türü.
* Listedeki ilk öğeye sahte oturumda depolanan fikir eşleşen geçerli bir fikir olduğunu (çağırılarak `GetTestSession`).

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

Örnek uygulamayı yeni bir oluşturmak için bir yöntem de içerir. `Idea` belirli bir oturum için. Denetleyici döndürür:

* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> için geçersiz bir model.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> oturum yoksa.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> oturum ile yeni fikir güncelleştirildiğinde.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

Üç testleri `CreateActionResult` dahil `ApiIdeasControllerTests`.

İlk metin teyit bir <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> için geçersiz bir model döndürülür.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

İkinci test denetleyen bir <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> oturum yoksa döndürülür.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

Geçerli bir oturum için `id`, son testte, onaylar:

* Yöntem döndürür bir `ActionResult` ile bir `BrainstormSession` türü.
* [Actionresult öğesini&lt;T&gt;. Sonuç](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Result*) olduğu bir <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult` için benzer bir *201 oluşturuldu* yanıtıyla bir `Location` başlığı.
* [Actionresult öğesini&lt;T&gt;. Değer](xref:Microsoft.AspNetCore.Mvc.ActionResult%601.Value*) olduğu bir `BrainstormSession` türü.
* Oturum güncelleştirmek için sahte arama `UpdateAsync(testSession)`, çağrıldı. `Verifiable` Yöntem çağrısının yürüterek işaretli `mockRepo.Verify()` içinde onaylar.
* İki `Idea` nesneleri, oturum için döndürülür.
* Son öğe ( `Idea` sahte çağrısı tarafından eklenen `UpdateAsync`) eşleşen `newIdea` test oturumu eklenir.

[!code-csharp[](testing/samples/2.x/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/integration-tests>
* [Oluşturma ve birim testlerini Visual Studio ile çalıştırma](/visualstudio/test/unit-test-your-code).
