---
title: "ASP.NET Core test tümleştirme"
author: ardalis
description: "Bir uygulamanın bileşenleri düzgün emin olmak için test ASP.NET Core tümleştirme kullanmayı."
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 8b0d741c05a723ad80fe812254c9a500a9fd9204
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="fdf17-103">ASP.NET Core test tümleştirme</span><span class="sxs-lookup"><span data-stu-id="fdf17-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="fdf17-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fdf17-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fdf17-105">Tümleştirme sınaması uygulamanın bileşenleri birlikte birleştirilen zaman düzgün çalışması sağlar.</span><span class="sxs-lookup"><span data-stu-id="fdf17-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="fdf17-106">ASP.NET Core destekler tümleştirme birim test çerçevelerini ve ağ yükü olmadan isteklerini işlemek için kullanılan bir yerleşik test web ana bilgisayarı kullanarak test etme.</span><span class="sxs-lookup"><span data-stu-id="fdf17-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="fdf17-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fdf17-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="fdf17-108">Tümleştirme sınaması giriş</span><span class="sxs-lookup"><span data-stu-id="fdf17-108">Introduction to integration testing</span></span>

<span data-ttu-id="fdf17-109">Tümleştirme testleri, bir uygulamanın farklı bölümlerini doğru bir şekilde birlikte çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fdf17-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="fdf17-110">Farklı [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), tümleştirme testleri sık bir veritabanı, dosya sistemi, ağ kaynaklarına veya web isteklerini ve yanıtlarını gibi uygulama altyapısı konuları içerir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="fdf17-111">Birim testleri fakes veya bu sorunları yerine sahte nesneler kullanır, ancak sistem bu sistemleriyle beklendiği gibi çalıştığını doğrulamak için tümleştirme testleri amacı budur.</span><span class="sxs-lookup"><span data-stu-id="fdf17-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="fdf17-112">Tümleştirme testleri büyük kod parçalarını çalışma olduğundan ve altyapı öğelerde dayandığından büyüklük birim testleri yavaş olma eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="fdf17-113">Birim testi ile aynı davranışı özellikle test edebilirsiniz, bu nedenle, onu yazarsanız, kaç tane tümleştirme testleri sınırlamak için bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf17-114">Bazı davranışı birim testi veya bir tümleştirme testi kullanılarak test edilebilir, neredeyse her zaman daha hızlı olması olacağından birim testi tercih eder.</span><span class="sxs-lookup"><span data-stu-id="fdf17-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="fdf17-115">Düzinelerce veya birçok farklı girişleri ile birim testleri yüzlerce ancak yalnızca en önemli senaryolar kapsayan tümleştirme testleri sayıda olabilir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="fdf17-116">Kendi yöntemleri mantık sınama, genellikle birim testleri etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="fdf17-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="fdf17-117">Örneğin ASP.NET Core veya bir veritabanı ile kendi framework içinde uygulamanızı nasıl çalıştığını test burada tümleştirme testleri oyuna gelen.</span><span class="sxs-lookup"><span data-stu-id="fdf17-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="fdf17-118">Bir satır veritabanına yazma ve geri okuma onaylamak için çok fazla tümleştirme testleri almaz.</span><span class="sxs-lookup"><span data-stu-id="fdf17-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="fdf17-119">Veri erişim kodunuzun her olası sıralamaya test gerekmez - uygulamanızın düzgün çalıştığını güvenirlik vermek için yeterli test etmek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="fdf17-120">Tümleştirme sınama ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="fdf17-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="fdf17-121">En fazla çalışma tümleştirme testleri ayarlanmış almak için bir test projesi oluşturmak, ASP.NET Core web projeniz için bir başvuru ekleyin ve bir test Çalıştırıcısı yüklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="fdf17-122">Bu işlem açıklanan [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) belgeleri testleri ve testleri ve test sınıfları adlandırma öneriler çalıştırma hakkında daha ayrıntılı yönergeler.</span><span class="sxs-lookup"><span data-stu-id="fdf17-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf17-123">Birim testleri ve tümleştirme Testlerinizi farklı projelere kullanarak ayırın.</span><span class="sxs-lookup"><span data-stu-id="fdf17-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="fdf17-124">Bu altyapı sorunları birim testlerinizi yanlışlıkla tanıtmak yok sağlamaya yardımcı olur ve kolayca hangi çalıştırmak için test kümesini seçmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fdf17-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="fdf17-125">Test ana</span><span class="sxs-lookup"><span data-stu-id="fdf17-125">The Test Host</span></span>

<span data-ttu-id="fdf17-126">ASP.NET Core tümleştirme test projeleri için eklenebilir ve gerçek web ana gerek kalmadan sunma test istekleri uygulamaları, ASP.NET Core konağa kullanılacak bir test ana içerir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="fdf17-127">Sağlanan örnek kullanacak şekilde yapılandırılmış bir tümleştirme test projesi içeren [xUnit](https://xunit.github.io) ve Test ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="fdf17-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="fdf17-128">Kullandığı `Microsoft.AspNetCore.TestHost` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="fdf17-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="fdf17-129">Bir kez `Microsoft.AspNetCore.TestHost` paketini projeye dahil, oluşturma ve yapılandırma kullanabileceksiniz bir `TestServer` testlerinizde.</span><span class="sxs-lookup"><span data-stu-id="fdf17-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="fdf17-130">Aşağıdaki sınama bir sitenin kök yapılan bir istek "Hello World!" döndürür doğrulamak nasıl gösterir</span><span class="sxs-lookup"><span data-stu-id="fdf17-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="fdf17-131">ve ASP.NET Core boş Web şablonu Visual Studio tarafından oluşturulan varsayılan karşı başarıyla çalıştırmalısınız.</span><span class="sxs-lookup"><span data-stu-id="fdf17-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="fdf17-132">Bu test Yerleştir Act Assert düzeni kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="fdf17-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="fdf17-133">Yerleştir adım örneği oluşturan oluşturucusunda yapılır `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="fdf17-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="fdf17-134">Yapılandırılan bir `WebHostBuilder` oluşturmak için kullanılan bir `TestHost`; Bu örnekte, `Configure` testin (SUT) altında sistem yönteminden `Startup` sınıfı iletilir `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fdf17-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="fdf17-135">Bu yöntem, istek ardışık düzenini yapılandırmak için kullanılan `TestServer` nasıl SUT sunucusunun yapılandırılması için aynı.</span><span class="sxs-lookup"><span data-stu-id="fdf17-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="fdf17-136">Test Act kısmı için bir istek yapıldığında `TestServer` örneği "/" yolu ve yanıt için bir dize olarak okunur.</span><span class="sxs-lookup"><span data-stu-id="fdf17-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="fdf17-137">Bu dize, "Hello World!" beklenen dizeyi ile karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="fdf17-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="fdf17-138">Eşleşirlerse, test geçirir; Aksi takdirde başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fdf17-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="fdf17-139">Artık işlevselliği denetimi asal web uygulaması aracılığıyla çalıştığını doğrulamak için birkaç ek tümleştirme testleri ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fdf17-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="fdf17-140">Değil gerçekten asal sayı denetleyicisi doğruluğunu ile bu testleri test etmeye çalıştığınız ancak bunun yerine, web uygulaması beklediğiniz yaptığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fdf17-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="fdf17-141">Güven verir birim testi kapsamı zaten `PrimeService`, burada görüldüğü gibi:</span><span class="sxs-lookup"><span data-stu-id="fdf17-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Test Gezgini](integration-testing/_static/test-explorer.png)

<span data-ttu-id="fdf17-143">Birim testleri hakkında daha fazla bilgiyi [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) makalesi.</span><span class="sxs-lookup"><span data-stu-id="fdf17-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="fdf17-144">MVC/Razor test tümleştirme</span><span class="sxs-lookup"><span data-stu-id="fdf17-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="fdf17-145">Razor görünümleri içeren test projeleri gerektiren `<PreserveCompilationContext>` ayarlanabilir true olarak *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fdf17-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="fdf17-146">Bu öğe eksik projeleri aşağıdakine benzer bir hata oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fdf17-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="fdf17-147">Ara yazılım kullanacak şekilde yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="fdf17-147">Refactoring to use middleware</span></span>

<span data-ttu-id="fdf17-148">Yeniden düzenleme davranışını değiştirmeden tasarımını geliştirmek için uygulamanın kodunu değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="fdf17-149">Bu Yardım değişikliklerden önceki ve sonraki sistemin davranışı aynı kalmasını sağlamak bu yana testleri, geçirme, bir paketi olduğunda ideal olarak yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fdf17-149">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="fdf17-150">Web uygulamasının denetleme mantığı asal uygulanır şekilde bakarak `Configure` yöntemi, bkz:</span><span class="sxs-lookup"><span data-stu-id="fdf17-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="fdf17-151">Bu kod çalışır, ancak nasıl işlevselliği bu tür bir ASP.NET Core uygulamada, bu bile kadar basit uygulamak istediğiniz gölgeden uzak olan.</span><span class="sxs-lookup"><span data-stu-id="fdf17-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="fdf17-152">Ne düşünün `Configure` yöntemi uygulamamız görünecektir gibi başka bir URL uç eklediğiniz her zaman bu kadar kodu ekleyin gerekirse!</span><span class="sxs-lookup"><span data-stu-id="fdf17-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="fdf17-153">Dikkate alınması gereken bir seçenek ekleme [MVC](xref:mvc/overview) prime denetimi işlemek için uygulama ve bir denetleyici oluşturma.</span><span class="sxs-lookup"><span data-stu-id="fdf17-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="fdf17-154">Ancak, şu anda yok varsayarak bir bit herhangi diğer MVC işlevselliği, overkill.</span><span class="sxs-lookup"><span data-stu-id="fdf17-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="fdf17-155">Ancak, ASP.NET Core avantajlarından yararlanmak [ara yazılımı](xref:fundamentals/middleware), hangi yardımcı olacak bize denetleme mantığı kendi sınıfında asal kapsüllemek ve daha iyi elde [sorunları ayrılması](http://deviq.com/separation-of-concerns/) içinde `Configure` yöntem.</span><span class="sxs-lookup"><span data-stu-id="fdf17-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="fdf17-156">Ara yazılım kullanan ara yazılım sınıfı bekliyor biçimde bir parametre olarak belirtilmesi için bir yola izin vermek istediğiniz bir `RequestDelegate` ve `PrimeCheckerOptions` kurucusu örneği.</span><span class="sxs-lookup"><span data-stu-id="fdf17-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="fdf17-157">İstek yolu bu ara yazılımın nedir eşleşmiyorsa beklediğiniz şekilde yapılandırılmış, sadece zincirde bir sonraki ara yazılım çağırın ve başka hiçbir şey yapma.</span><span class="sxs-lookup"><span data-stu-id="fdf17-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="fdf17-158">İçinde uygulama kodu kalan `Configure` olduğunu `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fdf17-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf17-159">Ara yazılım bağımlı olduğundan `PrimeService` service, ayrıca isteyen Oluşturucusu ile bu hizmet örneği.</span><span class="sxs-lookup"><span data-stu-id="fdf17-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="fdf17-160">Bu hizmeti aracılığıyla framework sağlayacak [bağımlılık ekleme](xref:fundamentals/dependency-injection), yapılandırılmış, örneğin varsayılarak `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fdf17-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="fdf17-161">Yolunu eşleştiğinde bu ara yazılım istek temsilci zincirde bir uç nokta gibi davranır, yoktur hiçbir çağrısına `_next.Invoke` ne zaman bu ara yazılım isteği işler.</span><span class="sxs-lookup"><span data-stu-id="fdf17-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="fdf17-162">Bu ara yazılım ve yapılandırma, kolaylaştırmak için oluşturulan bazı yararlı genişletme yöntemleri işlenmiş olan `Configure` yöntemi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="fdf17-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="fdf17-163">Bu yeniden düzenleme aşağıdaki, tüm tümleştirme testlerinizi geçtiğiniz beri web uygulaması halen önceki gibi çalışır durumda olduğundan emin olursunuz.</span><span class="sxs-lookup"><span data-stu-id="fdf17-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf17-164">Bir yeniden düzenleme tamamladıktan sonra testlerinizi geçirmek için kaynak denetimi değişikliklerinizi uygulamak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="fdf17-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="fdf17-165">Test güdümlü geliştirme, kapattığınızdan varsa [kırmızı yeşil düzenleme döngüyü yürütme eklemeyi düşünün](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="fdf17-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="fdf17-166">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fdf17-166">Resources</span></span>

* [<span data-ttu-id="fdf17-167">Birim testi</span><span class="sxs-lookup"><span data-stu-id="fdf17-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="fdf17-168">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="fdf17-168">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="fdf17-169">Test denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="fdf17-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
