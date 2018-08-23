---
title: ASP.NET Core denetleyicisi mantıksal test
author: ardalis
description: ASP.NET Core Moq ve xUnit ile test denetleyicisi mantıksal öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41755678"
---
# <a name="test-controller-logic-in-aspnet-core"></a>ASP.NET Core denetleyicisi mantıksal test

Tarafından [Steve Smith](https://ardalis.com/)

Denetleyicileri, herhangi bir ASP.NET Core MVC uygulaması, merkezi bir parçasıdır. Bu nedenle, uygulamanız için tasarlandığı gibi davranırlar güven olması gerekir. Otomatikleştirilmiş testleri Bu güvenle sağlayabilir ve üretime ulaşmadan önce hatalar da algılanabilir. Gereksiz sorumlulukları denetleyicilerinizi içinde yerleştirmekten kaçının ve denetleyici sorumlulukları yalnızca, testleri odaklanan sağlamak önemlidir.

Denetleyici mantığı en az olmalıdır ve iş mantığı ya da altyapı olarak ilk (örneğin, veri erişimi) odaklı olmayan. Denetleyici mantığı, framework test edin. Test nasıl denetleyici *davranışını* geçerli ya da geçersiz girişler temelinde. Denetleyici yanıtları gerçekleştirdiği iş işleminin sonucuna dayalı test edin.

Tipik denetleyicisi sorumlulukları:

* Doğrulama `ModelState.IsValid`.
* Bir hata yanıtı döndürür `ModelState` geçersiz.
* Bir iş varlığı Kalıcılık alın.
* İş varlıkta bir eylem gerçekleştirin.
* İş varlığı için Kalıcılık kaydedin.
* Uygun bir dönüş `IActionResult`.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Denetleyici mantıksal birim testleri

[Birim testleri](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) bir uygulamanın bir parçası, altyapısını ve bağımlılıklarını yalıtımdan test içerir. Birim testi denetleyicisi mantığı, yalnızca tek bir eylem içeriği ne zaman sınanırken, olmayan davranış framework'ün ya da bağımlılıklarından biri. Test denetleyicisi eylemlerinizi, birimi, yalnızca davranışını üzerinde odaklanın emin olun. Bir denetleyici birim testi gibi şeyleri önler [filtreleri](xref:mvc/controllers/filters), [yönlendirme](xref:fundamentals/routing), veya [model bağlama](xref:mvc/models/model-binding). Tek şey sınamalara odaklanarak, birim testleri genellikle basit yazmak ve çalıştırmak hızlı. Birim testleri iyi-yazılan bir dizi sık kadar ek yük çalıştırabilirsiniz. Ancak, birim testleri sorunları algılama bileşenleri arasındaki etkileşim içinde olan amacı [tümleştirme testleri](xref:test/integration-tests).

Özel Filtreler ve yollar yazıyorsanız, birim gereken testleri belirli bir denetleyici eylemi bir parçası olarak değil, yalıtım, bunları test edin.

> [!TIP]
> [Oluşturma ve birim testlerini Visual Studio ile çalıştırma](/visualstudio/test/unit-test-your-code).

Birim testi göstermek için aşağıdaki denetleyicisi gözden geçirin. Oturumlarının fırtınası listesini görüntüler ve yeni bir GÖNDERİ ile oluşturulacak oturumları fırtınası sağlar:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Denetleyici aşağıdaki [özel bağımlılıklar İlkesi](http://deviq.com/explicit-dependencies-principle/), bağımlılık ekleme örneği ile sağlamak için bekleniyor `IBrainstormSessionRepository`. Bu oldukça, sahte nesne çerçeve gibi kullanarak test etmek kolaylaştırır [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` Yöntemi hiçbir döngü ya da dallanma ve yalnızca çağrıları bir yöntemi vardır. Bunu test etmek için `Index` yöntemi ihtiyacımız doğrulamak bir `ViewResult` döndürülecek ile bir `ViewModel` deponun gelen `List` yöntemi.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` Yöntemini (yukarıda gösterilmiştir) doğrulayın:

* Hatalı istek eylem yöntemine döndürür `ViewResult` uygun verileri ile zaman `ModelState.IsValid` olduğu `false`.

* `Add` Deposunda yöntemi çağrılır ve bir `RedirectToActionResult` doğru bağımsız değişkenleri ile döndürülen zaman `ModelState.IsValid` geçerlidir.

Geçersiz model durumu hataları kullanarak ekleyerek edilebilirler `AddModelError` ilk test aşağıda gösterildiği gibi.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

İlk testin ne zaman onaylar `ModelState` geçersiz, aynı `ViewResult` olarak için döndürülen bir `GET` istek. Test geçersiz bir modelde geçirilecek çalışmaz unutmayın. Bu mıydı işe yine de model bağlama çalışmadığından bu yana (ancak bir [tümleştirme test](xref:test/integration-tests) alıştırma model bağlama kullanırsınız). Bu durumda, model bağlama test edilen değil. Bu birim testlerini, yalnızca ne yaptığını kodda bir eylem yöntemi test edersiniz.

O zaman ikinci test doğrular `ModelState` geçerli olduğu yeni bir `BrainstormSession` (depo) eklenir ve yöntemi bir `RedirectToActionResult` beklenen özelliklere sahip. Adı olmayan sahte çağrıları normalde yoksayıldı, ancak arama `Verifiable` Kurulum sonunda çağrı testinde doğrulanması izin verir. Bu çağrı yapılır `mockRepo.Verify`, hangi başarısız olur test beklenen yöntemi çağrılırsa değildi.

> [!NOTE]
> Bu örnekte kullanılan Moq kitaplığı doğrulanabilir ya da "strict" mocks doğrulanamaz mocks ("belirsiz" mocks veya yer tutucular olarak da bilinir) ile karıştırmak kolaylaştırır. Daha fazla bilgi edinin [Moq ile sahte davranışını özelleştirme](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Başka bir denetleyici uygulamasında belirli fırtınası oturumuyla ilgili bilgileri görüntüler. Geçersiz kod değerleri ile dağıtılacak mantığa aşağıdakileri içerir:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Denetleyici eylemini test etmek için her biri üç durumda olan `return` deyimi:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Uygulama bir web API (fırtınası oturumu ve bir oturumu yeni fikirler eklemek için bir yöntem ile ilgili fikirlerinizi listesi) işlevselliği kullanıma sunar:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Yöntemi listesini döndürür `IdeaDTO` türleri. İş etki alanı varlıklarınızı doğrudan API çağrıları geri dönmekten kaçının, API istemcisi gerektirir ve bunlar gereksiz yere uygulamanızın iç etki alanı modeli, harici olarak kullanıma API ile eşleştiği daha fazla veri beri sık içerirler. Etki alanı varlıklarının kablo üzerinden döndürecektir türleri arasında eşleme el ile yapılabilir (bir LINQ kullanarak `Select` burada gösterildiği gibi) veya benzer bir kitaplık kullanılarak [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Birim testleri için `Create` ve `ForSession` API yöntemleri:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Yöntemin davranışını test etmek için daha önce belirtildiği gibi `ModelState` geçersiz model hatası denetleyiciye testin bir parçası ekleyin. Model doğrulama veya model bağlama birim testlerinizde test-yalnızca belirli bir ile karşılaşıldığında, eylem yöntemin davranışını test çalışmayın `ModelState` değeri.

İkinci test sahte deposunun null döndürmek üzere yapılandırıldığı şekilde null döndüren havuzda bağlıdır. Bir test veritabanı oluşturmaya gerek yoktur (bellekte veya başka türlü) ve bu sonuç döndüren bir sorgu oluşturun - bu tek bir deyimde gösterildiği yapılabilir.

Son test doğrular deponun `Update` yöntemi çağrılır. Daha önce yaptığımız gibi sahte çağrılır `Verifiable` ve ardından sahte deponun `Verify` yöntemi doğrulanabilir yöntemi yürütüldü doğrulamak için çağrılır. Emin olmak için bir birim test sorumluluğu değil `Update` yöntemi kaydedilen verileri; ile bir tümleştirme testi yapılabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/integration-tests>
