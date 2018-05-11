---
title: ASP.NET Core tümleştirme testlerinde
author: ardalis
description: Bir uygulamanın bileşenleri düzgün emin olmak için test ASP.NET Core tümleştirme kullanmayı.
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: ac3a9e00edfd4c736ee1e7d5c0c724c3e52d0b6b
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core tümleştirme testlerinde

Tarafından [Steve Smith](https://ardalis.com/)

Tümleştirme sınaması uygulamanın bileşenleri birlikte birleştirilen zaman düzgün çalışması sağlar. ASP.NET Core destekler tümleştirme birim test çerçevelerini ve ağ yükü olmadan isteklerini işlemek için kullanılan bir yerleşik test web ana bilgisayarı kullanarak test etme.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Tümleştirme sınaması giriş

Tümleştirme testleri, bir uygulamanın farklı bölümlerini doğru bir şekilde birlikte çalıştığını doğrulayın. Farklı [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), tümleştirme testleri sık bir veritabanı, dosya sistemi, ağ kaynaklarına veya web isteklerini ve yanıtlarını gibi uygulama altyapısı konuları içerir. Birim testleri fakes veya bu sorunları yerine sahte nesneler kullanır, ancak sistem bu sistemleriyle beklendiği gibi çalıştığını doğrulamak için tümleştirme testleri amacı budur.

Tümleştirme testleri büyük kod parçalarını çalışma olduğundan ve altyapı öğelerde dayandığından büyüklük birim testleri yavaş olma eğilimindedir. Birim testi ile aynı davranışı özellikle test edebilirsiniz, bu nedenle, onu yazarsanız, kaç tane tümleştirme testleri sınırlamak için bir fikirdir.

> [!NOTE]
> Bazı davranışı birim testi veya bir tümleştirme testi kullanılarak test edilebilir, neredeyse her zaman daha hızlı olması olacağından birim testi tercih eder. Düzinelerce veya birçok farklı girişleri ile birim testleri yüzlerce ancak yalnızca en önemli senaryolar kapsayan tümleştirme testleri sayıda olabilir.

Kendi yöntemleri mantık sınama, genellikle birim testleri etki alanıdır. Örneğin ASP.NET Core veya bir veritabanı ile kendi framework içinde uygulamanızı nasıl çalıştığını test burada tümleştirme testleri oyuna gelen. Bir satır veritabanına yazma ve geri okuma onaylamak için çok fazla tümleştirme testleri almaz. Veri erişim kodunuzun her olası sıralamaya test gerekmez - uygulamanızın düzgün çalıştığını güvenirlik vermek için yeterli test etmek yeterlidir.

## <a name="integration-testing-aspnet-core"></a>Tümleştirme sınama ASP.NET Çekirdeği

En fazla çalışma tümleştirme testleri ayarlanmış almak için bir test projesi oluşturmak, ASP.NET Core web projeniz için bir başvuru ekleyin ve bir test Çalıştırıcısı yüklemek gerekir. Bu işlem açıklanan [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) belgeleri testleri ve testleri ve test sınıfları adlandırma öneriler çalıştırma hakkında daha ayrıntılı yönergeler.

> [!NOTE]
> Birim testleri ve tümleştirme Testlerinizi farklı projelere kullanarak ayırın. Bu altyapı sorunları birim testlerinizi yanlışlıkla tanıtmak yok sağlamaya yardımcı olur ve kolayca hangi çalıştırmak için test kümesini seçmenize olanak sağlar.

### <a name="the-test-host"></a>Test ana

ASP.NET Core tümleştirme test projeleri için eklenebilir ve gerçek web ana gerek kalmadan sunma test istekleri uygulamaları, ASP.NET Core konağa kullanılacak bir test ana içerir. Sağlanan örnek kullanacak şekilde yapılandırılmış bir tümleştirme test projesi içeren [xUnit](https://xunit.github.io) ve Test ana bilgisayar. Kullandığı [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost/) NuGet paketi.

Bir kez `Microsoft.AspNetCore.TestHost` paketini projeye dahil, oluşturma ve yapılandırma kullanabileceksiniz bir `TestServer` testlerinizde. Aşağıdaki sınama bir sitenin kök yapılan bir istek "Hello World!" döndürür doğrulamak nasıl gösterir ve ASP.NET Core boş Web şablonu Visual Studio tarafından oluşturulan varsayılan karşı başarıyla çalıştırmalısınız.

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Bu test Yerleştir Act Assert düzeni kullanıyor. Yerleştir adım örneği oluşturan oluşturucusunda yapılır `TestServer`. Yapılandırılan bir `WebHostBuilder` oluşturmak için kullanılan bir `TestHost`; Bu örnekte, `Configure` testin (SUT) altında sistem yönteminden `Startup` sınıfı iletilir `WebHostBuilder`. Bu yöntem, istek ardışık düzenini yapılandırmak için kullanılan `TestServer` nasıl SUT sunucusunun yapılandırılması için aynı.

Test Act kısmı için bir istek yapıldığında `TestServer` örneği "/" yolu ve yanıt için bir dize olarak okunur. Bu dize, "Hello World!" beklenen dizeyi ile karşılaştırılır. Eşleşirlerse, test geçirir; Aksi takdirde başarısız olur.

Artık işlevselliği denetimi asal web uygulaması aracılığıyla çalıştığını doğrulamak için birkaç ek tümleştirme testleri ekleyebilirsiniz:

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Değil gerçekten asal sayı denetleyicisi doğruluğunu ile bu testleri test etmeye çalıştığınız ancak bunun yerine, web uygulaması beklediğiniz yaptığını unutmayın. Güven verir birim testi kapsamı zaten `PrimeService`, burada görüldüğü gibi:

![Test Gezgini](integration-testing/_static/test-explorer.png)

Birim testleri hakkında daha fazla bilgiyi [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) makalesi.


### <a name="integration-testing-mvcrazor"></a>MVC/Razor test tümleştirme

Razor görünümleri içeren test projeleri gerektiren `<PreserveCompilationContext>` ayarlanabilir true olarak *.csproj* dosyası:


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

Bu öğe eksik projeleri aşağıdakine benzer bir hata oluşturur:
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Ara yazılım kullanacak şekilde yeniden düzenleme

Yeniden düzenleme davranışını değiştirmeden tasarımını geliştirmek için uygulamanın kodunu değiştirme işlemidir. Bu Yardım değişikliklerden önceki ve sonraki sistemin davranışı aynı kalmasını sağlamak bu yana testleri, geçirme, bir paketi olduğunda ideal olarak yapılmalıdır. Web uygulamasının denetleme mantığı asal uygulanır şekilde bakarak `Configure` yöntemi, bkz:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

Bu kod çalışır, ancak nasıl işlevselliği bu tür bir ASP.NET Core uygulamada, bu bile kadar basit uygulamak istediğiniz gölgeden uzak olan. Ne düşünün `Configure` yöntemi uygulamamız görünecektir gibi başka bir URL uç eklediğiniz her zaman bu kadar kodu ekleyin gerekirse!

Dikkate alınması gereken bir seçenek ekleme [MVC](xref:mvc/overview) prime denetimi işlemek için uygulama ve bir denetleyici oluşturma. Ancak, şu anda yok varsayarak bir bit herhangi diğer MVC işlevselliği, overkill.

Ancak, ASP.NET Core avantajlarından yararlanmak [ara yazılımı](xref:fundamentals/middleware/index), hangi yardımcı olacak bize denetleme mantığı kendi sınıfında asal kapsüllemek ve daha iyi elde [sorunları ayrılması](http://deviq.com/separation-of-concerns/) içinde `Configure` yöntem.

Ara yazılım kullanan ara yazılım sınıfı bekliyor biçimde bir parametre olarak belirtilmesi için bir yola izin vermek istediğiniz bir `RequestDelegate` ve `PrimeCheckerOptions` kurucusu örneği. İstek yolu bu ara yazılımın nedir eşleşmiyorsa beklediğiniz şekilde yapılandırılmış, sadece zincirde bir sonraki ara yazılım çağırın ve başka hiçbir şey yapma. İçinde uygulama kodu kalan `Configure` olduğunu `Invoke` yöntemi.

> [!NOTE]
> Ara yazılım bağımlı olduğundan `PrimeService` service, ayrıca isteyen Oluşturucusu ile bu hizmet örneği. Bu hizmeti aracılığıyla framework sağlayacak [bağımlılık ekleme](xref:fundamentals/dependency-injection), yapılandırılmış, örneğin varsayılarak `ConfigureServices`.

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Yolunu eşleştiğinde bu ara yazılım istek temsilci zincirde bir uç nokta gibi davranır, yoktur hiçbir çağrısına `_next.Invoke` ne zaman bu ara yazılım isteği işler.

Bu ara yazılım ve yapılandırma, kolaylaştırmak için oluşturulan bazı yararlı genişletme yöntemleri işlenmiş olan `Configure` yöntemi şöyle görünür:

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Bu yeniden düzenleme aşağıdaki, tüm tümleştirme testlerinizi geçtiğiniz beri web uygulaması halen önceki gibi çalışır durumda olduğundan emin olursunuz.

> [!NOTE]
> Bir yeniden düzenleme tamamladıktan sonra testlerinizi geçirmek için kaynak denetimi değişikliklerinizi uygulamak için iyi bir fikirdir. Test güdümlü geliştirme, kapattığınızdan varsa [kırmızı yeşil düzenleme döngüyü yürütme eklemeyi düşünün](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Kaynaklar

* [Birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Test denetleyicileri](xref:mvc/controllers/testing)
