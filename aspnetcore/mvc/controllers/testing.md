---
title: "ASP.NET Core test denetleyicisi mantığı"
author: ardalis
description: "Denetleyici mantığında ASP.NET Core Moq ve xUnit test öğrenin."
keywords: "ASP.NET Core, denetleyicisi, test, test, birim testi, birim testi, tümleştirme test, tümleştirme sınaması, xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: aa60912e06946bd0df4936d33c88d3bf7b69984c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>ASP.NET Core test denetleyicisi mantığı

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET MVC uygulamalarında denetleyicileri, küçük ve kullanıcı arabirimi sorunları odaklanmıştır olması gerekir. Kullanıcı Arabirimi sorunları ile ilgili büyük denetleyicileri test ve sürdürmek daha zordur.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Test denetleyicileri

Denetleyicileri herhangi bir ASP.NET Core MVC uygulama merkezi bir parçasıdır. Bu nedenle, uygulamanız için tasarlandığı gibi davranırlar güven olmalıdır. Otomatikleştirilmiş test üretim düşmeden önce hatalarını algılayabilir ve bu güvenle sağlayabilirsiniz. Denetleyicilerinizi içinde gereksiz sorumlulukları yerleştirmez ve testleri odağınız yalnızca denetleyici sorumlulukları üzerinde sağlamak önemlidir.

Denetleyici mantığında iş mantığı ya da altyapı sorunları üzerinde (örneğin, veri erişimi) odaklı olmayan ve en az olması gerekir. Denetleyici mantığında, framework sınayın. Test nasıl denetleyicisi *davranır* geçerli veya geçersiz girişler için temel. Denetleyici yanıtları sonucuna göre gerçekleştirdiği iş işlemi sınayın.

Tipik denetleyicisi sorumlulukları:

* Doğrulama `ModelState.IsValid`.
* Bir hata yanıtı döndürür `ModelState` geçersiz.
* Bir iş varlığı Kalıcılık alın.
* Bir eylem iş varlık üzerinde gerçekleştirin.
* İş varlığı için Kalıcılık kaydedin.
* Uygun bir dönüş `IActionResult`.

## <a name="unit-testing"></a>Birim testi

[Birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) haklarında altyapı ve bağımlılıklar ayrı bir parçası olan bir uygulamayı test içerir. Birim testi denetleyicisi mantığı, yalnızca tek bir eylem'in içeriği ne zaman test, değil davranışı bağımlılıklarından biri veya framework'ün. Test denetleyicisi eylemleriniz, birimi, yalnızca davranışını üzerinde odaklanmak emin olun. Denetleyici birim testi gibi şeyleri önler [filtreleri](filters.md), [yönlendirme](../../fundamentals/routing.md), veya [model bağlama](../models/model-binding.md). Tek şey test Odaklanıldığında, birim testleri genellikle yazmak basit ve hızlı çalıştırmak. Birim testleri iyi yazılmış bir dizi sık kadar ek yükü çalıştırabilirsiniz. Ancak, birim testleri sorunları algılamaz bileşenleri arasındaki etkileşim içinde olduğu amacı [tümleştirme sınaması](xref:mvc/controllers/testing#integration-testing).

Özel Filtreler, yollar, vb., yazıyorsanız, birim testi gereken bunları, ancak belirli denetleyici eylemi sınamalarınızı parçası olarak değil. Bunlar test yalıtım modunda.

> [!TIP]
> [Oluşturma ve Visual Studio ile birim testleri çalıştırma](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).

Birim testi göstermek için aşağıdaki denetleyicisiyle gözden geçirin. Oturumları fırtınası listesini görüntüler ve yeni bir posta ile oluşturulacak oturumları fırtınası sağlar:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Denetleyici aşağıdaki [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/), bağımlılık ekleme örneği ile sağlamak için bekleniyor `IBrainstormSessionRepository`. Bu, oldukça sahte nesne framework gibi kullanarak test kolaylaştırır [Moq](https://www.nuget.org/packages/Moq/). `HTTP GET Index` Yöntemi hiçbir döngü veya dallanma ve yalnızca çağrıları bir yöntem vardır. Bunu test etmek için `Index` yöntemi, ihtiyacımız doğrulamak bir `ViewResult` döndürülür, ile bir `ViewModel` deponun gelen `List` yöntemi.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` (Yukarıda gösterilen) yöntemini doğrulayın:

* Hatalı istek eylem yöntemine döndürür `ViewResult` uygun verilerle zaman `ModelState.IsValid` olduğu`false`

* `Add` Deposunda yöntemi çağrılır ve bir `RedirectToActionResult` doğru bağımsız değişkenlerle döndürülen zaman `ModelState.IsValid` doğrudur.

Geçersiz model durumu kullanarak hataları ekleyerek test edilmiş `AddModelError` ilk testinde aşağıda gösterildiği gibi.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

İlk test ne zaman onaylar `ModelState` geçerli değil aynı `ViewResult` olarak için döndürülen bir `GET` isteği. Test geçersiz bir modelde geçirmek girişimi değil unutmayın. Bu olmayacaktır işe yine de model bağlama çalışmadığından bu yana (ancak bir [tümleştirme test](xref:mvc/controllers/testing#integration-testing) alıştırma model bağlama kullanırsınız). Bu durumda, model bağlama test edilmekte olan değil. Bu birim testleri eylem yöntemindeki kod yaptığı yalnızca test.

İkinci test olduğunda doğrular `ModelState` geçerli olduğu yeni bir `BrainstormSession` (depo), eklenen ve yöntemi bir `RedirectToActionResult` beklenen özelliklere sahip. Adlı olmayan mocked çağrıları normalde yoksayılan ancak çağırma `Verifiable` Kurulum sonunda çağrısı testinde doğrulanması izin verir. Bu çağrısıyla yapılır `mockRepo.Verify`, hangi başarısız olur test beklenen yöntemi olmayan çağrıldıklarında.

> [!NOTE]
> Bu örnekte kullanılan Moq kitaplığı doğrulanabilen olmayan mocks ("gevşek" mocks veya yer tutucular olarak da bilinir) ile doğrulanabilir ya da "katı" mocks karışımı kolay hale getirir. Daha fazla bilgi edinmek [Moq Mock davranışını özelleştirme](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Uygulama başka bir denetleyici belirli fırtınası oturumuyla ilgili bilgiler görüntüler. Geçersiz kimlik değerleri ile mücadele etmek için bazı mantığı içerir:

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Denetleyici eylemini sınamak için her üç durumda olan `return` deyimi:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

Uygulama bir web API (fırtınası oturumu ve yeni fikirleri bir oturuma eklemek için bir yöntem ile ilişkili fikirler listesi) işlevselliği sunar:

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` Yöntemi listesini döndürür `IdeaDTO` türleri. Doğrudan API çağrıları, iş etki alanı varlık döndüren kaçınmak için API İstemci gerektirir ve bunlar gereksiz yere, uygulamanızın iç etki alanı modeli dışarıdan kullanıma API'si ile eşleştiği daha fazla veri itibaren sık içerirler. Etki alanı varlıkları ve kablo üzerinden döndürecektir türleri arasında eşleme el ile yapılabilir (LINQ kullanarak `Select` aşağıda gösterildiği gibi) veya benzer bir kitaplık kullanılarak [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Birim testleri için `Create` ve `ForSession` API yöntemlerini:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Yöntem davranışını test etmek için daha önce belirtildiği gibi `ModelState` geçersiz model hatası denetleyiciye testin bir parçası ekleyin. Model doğrulama veya model bağlama, birim testleri test-yalnızca belirli bir ile karşılaşıldığında, eylem yönteminin davranışı test denemeyin `ModelState` değeri.

İkinci test sahte depo null döndürmek için yapılandırılmış şekilde null döndüren depo bağlıdır. Bir test veritabanı oluşturmak için gerek yoktur (bellekte veya aksi halde) ve bu sonuç döndüren bir sorgu oluşturun -, tek bir deyimde gösterildiği gibi yapılabilir.

Son test doğrular deponun `Update` yöntemi çağrılır. Daha önce yaptığımız gibi mock ile çağrılır `Verifiable` ve ardından mocked deponun `Verify` yöntemi doğrulanabilen yöntemi yürütüldü onaylamak için çağrılır. Emin olmak için bir birim testi sorumluluğu değil `Update` yöntemi kaydedilen veri; tümleştirme test ile yapılabilir.

## <a name="integration-testing"></a>Tümleştirme sınaması

[Tümleştirme sınaması](../../testing/integration-testing.md) uygulama iş içindeki ayrı modülleri doğru birlikte emin olmak için gerçekleştirilir. Genellikle, herhangi bir şey birim testi ile test ile tümleştirme test ayrıca test edebilirsiniz, ancak tersi doğru değil. Ancak, tümleştirme testleri birim testleri çok daha yavaş olma eğilimindedir. Bu nedenle, ne olursa olsun da ile birim testleri ve birden çok ortak çalışanlar gerektiren senaryolar için tümleştirme testleri kullanın test etmek en iyisidir.

Bunlar hala yararlı olabilir ancak sahte nesneler tümleştirme testlerinde nadiren kullanılır. Birim testi sınanan birim dışında ortak test amaçları doğrultusunda nasıl hareket etmesi gerektiğini denetlemek için etkili bir yol sahte nesneleridir. Bir tümleştirme testinde gerçek ortak çalışanlar, tüm alt sistemi birlikte düzgün çalıştığını doğrulamak için kullanılır.

### <a name="application-state"></a>Uygulama durumu

Tümleştirme sınaması gerçekleştirirken bir önemli uygulamanızın durumunun nasıl ayarlanacağı konudur. Testleri birbirlerinden bağımsız çalıştırmanız gerekir ve böylece her test bilinen bir duruma uygulamada ile başlamanız gerekir. Uygulamanızı değil veya bir veritabanını kullanmak herhangi Kalıcılık varsa, bu bir sorun olabilir. Ancak, veri deposunu sıfırlama sürece bir test tarafından yapılan değişiklikler başka bir test etkileyebilir şekilde durumlarına bazı tür bir veri deposu, çoğu gerçek uygulamalar kalıcı olmasını sağlar. Yerleşik kullanarak `TestServer`bizim tümleştirme testleri içindeki konak ASP.NET Core uygulamaları çok basit ancak, değil mutlaka erişimi izni verin, kullanacağınız veri. Gerçek bir veritabanı kullanıyorsanız, bir test veritabanına bağlanan uygulamanız için bir yaklaşım ise her test yürütülmeden önce testlerinizi erişmek ve sağlamak için bilinen bir duruma sıfırlanır.

Yalnızca kendisine my test projesinden bağlanamıyorum şekilde bu örnek uygulamasında Entity Framework Çekirdek'ın InMemoryDatabase destek kullanıyorum. Bunun yerine, ı kullanıma bir `InitializeDatabase` uygulamanın yönteminden `Startup` bu ise, uygulama başlatıldığında ı çağrı sınıfını `Development` ortamı. Bunlar ortam kümesine sürece my tümleştirme testleri otomatik olarak bu yararlı `Development`. Veritabanı, uygulama her başladığında InMemoryDatabase sıfırlama beri sıfırlama hakkında endişelenmeniz gerekmez.

`Startup` Sınıfı:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Göreceğiniz `GetTestSession` tümleştirme testlerinde sık kullanılan yöntem.

### <a name="accessing-views"></a>Görünümleri erişme

Her tümleştirme test sınıfı yapılandırır `TestServer` ASP.NET Core uygulama çalışır. Varsayılan olarak, `TestServer` onu çalıştırdığı - bu durumda, test proje klasöründen klasör web uygulamasında barındırır. Bu nedenle, çalıştığınızda dönüş denetleyici eylemleri test etmek `ViewResult`, bu hatayı görebilirsiniz:

```
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

Bu sorunu gidermek için test projesi için görünümleri gidebilecektir sunucunun içerik kök yapılandırmanız gerekir. Bu çağrısıyla yapılır `UseContentRoot` içinde `TestFixture` sınıfı, aşağıda gösterilmektedir:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture` Sınıfı, yapılandırma ve oluşturma sorumlu `TestServer`, ayarlanırken bir `HttpClient` ile iletişim kurmak için `TestServer`. Her tümleştirme kullanır testleri `Client` özelliğini test sunucusuna bağlanmak ve bir istekte bulunun.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

Yukarıdaki ilk testinde `responseString` gerçek işlenen HTML beklenen sonuçları içerdiği onaylamak için Denetlenmekte görünümünden tutar.

İkinci test benzersiz oturum adı ile bir form POST oluşturur ve uygulamaya gönderir, ardından beklenen yeniden yönlendirme döndürülür doğrular.

### <a name="api-methods"></a>API yöntemlerini

Uygulamanızı web API'leri, buna ait otomatikleştirilmiş testleri için iyi bir fikir onaylayın bunlar yürütme beklendiği gibi göstermiyorsa. Yerleşik `TestServer` web API'leri test kolay hale getirir. Model bağlama, API yöntemlerini kullanıyorsanız, her zaman denetlemelisiniz `ModelState.IsValid`, ve tümleştirme testleri, model doğrulama düzgün çalıştığını doğrulamak için doğru yerde.

Testleri hedef aşağıdaki kümesini `Create` yönteminde [IdeasController](xref:mvc/controllers/testing#ideas-controller) yukarıda gösterilen sınıfı:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

HTML görünümleri döndüren eylem tümleştirme testlerin aksine, son test yukarıda gösterildiği gibi sonuçları döndüren web API yöntemleri genellikle güçlü şekilde yazılan nesnelerin seri durumdan çıkarılmış olabilir. Bu durumda, test sonucu çıkarır bir `BrainstormSession` örneği ve fikir fikirleri kendi koleksiyonuna doğru eklendiğini doğrular.

Bu makalede 's tümleştirme testleri ek örnekler bulacaksınız [örnek proje](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
