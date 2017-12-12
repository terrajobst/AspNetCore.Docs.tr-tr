---
title: "ASP.NET Core test tümleştirme"
author: ardalis
description: "Bir uygulamanın bileşenleri düzgün emin olmak için test ASP.NET Core tümleştirme kullanmayı."
keywords: "ASP.NET Core, test, Razor tümleştirme"
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 155fd2f0663c6225531a4df6f323ebb30ab1ee73
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="67c84-104">ASP.NET Core test tümleştirme</span><span class="sxs-lookup"><span data-stu-id="67c84-104">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="67c84-105">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="67c84-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="67c84-106">Tümleştirme sınaması uygulamanın bileşenleri birlikte birleştirilen zaman düzgün çalışması sağlar.</span><span class="sxs-lookup"><span data-stu-id="67c84-106">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="67c84-107">ASP.NET Core destekler tümleştirme birim test çerçevelerini ve ağ yükü olmadan isteklerini işlemek için kullanılan bir yerleşik test web ana bilgisayarı kullanarak test etme.</span><span class="sxs-lookup"><span data-stu-id="67c84-107">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="67c84-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67c84-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="67c84-109">Tümleştirme sınaması giriş</span><span class="sxs-lookup"><span data-stu-id="67c84-109">Introduction to integration testing</span></span>

<span data-ttu-id="67c84-110">Tümleştirme testleri, bir uygulamanın farklı bölümlerini doğru bir şekilde birlikte çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="67c84-110">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="67c84-111">Farklı [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), tümleştirme testleri sık bir veritabanı, dosya sistemi, ağ kaynaklarına veya web isteklerini ve yanıtlarını gibi uygulama altyapısı konuları içerir.</span><span class="sxs-lookup"><span data-stu-id="67c84-111">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="67c84-112">Birim testleri fakes veya bu sorunları yerine sahte nesneler kullanır, ancak sistem bu sistemleriyle beklendiği gibi çalıştığını doğrulamak için tümleştirme testleri amacı budur.</span><span class="sxs-lookup"><span data-stu-id="67c84-112">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="67c84-113">Tümleştirme testleri büyük kod parçalarını çalışma olduğundan ve altyapı öğelerde dayandığından büyüklük birim testleri yavaş olma eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="67c84-113">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="67c84-114">Birim testi ile aynı davranışı özellikle test edebilirsiniz, bu nedenle, onu yazarsanız, kaç tane tümleştirme testleri sınırlamak için bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="67c84-114">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="67c84-115">Bazı davranışı birim testi veya bir tümleştirme testi kullanılarak test edilebilir, neredeyse her zaman daha hızlı olması olacağından birim testi tercih eder.</span><span class="sxs-lookup"><span data-stu-id="67c84-115">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="67c84-116">Düzinelerce veya birçok farklı girişleri ile birim testleri yüzlerce ancak yalnızca en önemli senaryolar kapsayan tümleştirme testleri sayıda olabilir.</span><span class="sxs-lookup"><span data-stu-id="67c84-116">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="67c84-117">Kendi yöntemleri mantık sınama, genellikle birim testleri etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="67c84-117">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="67c84-118">Örneğin ASP.NET Core veya bir veritabanı ile kendi framework içinde uygulamanızı nasıl çalıştığını test burada tümleştirme testleri oyuna gelen.</span><span class="sxs-lookup"><span data-stu-id="67c84-118">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="67c84-119">Bir satır veritabanına yazma ve geri okuma onaylamak için çok fazla tümleştirme testleri almaz.</span><span class="sxs-lookup"><span data-stu-id="67c84-119">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="67c84-120">Veri erişim kodunuzun her olası sıralamaya test gerekmez - uygulamanızın düzgün çalıştığını güvenirlik vermek için yeterli test etmek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="67c84-120">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="67c84-121">Tümleştirme sınama ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="67c84-121">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="67c84-122">En fazla çalışma tümleştirme testleri ayarlanmış almak için bir test projesi oluşturmak, ASP.NET Core web projeniz için bir başvuru ekleyin ve bir test Çalıştırıcısı yüklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="67c84-122">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="67c84-123">Bu işlem açıklanan [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) belgeleri testleri ve testleri ve test sınıfları adlandırma öneriler çalıştırma hakkında daha ayrıntılı yönergeler.</span><span class="sxs-lookup"><span data-stu-id="67c84-123">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="67c84-124">Birim testleri ve tümleştirme Testlerinizi farklı projelere kullanarak ayırın.</span><span class="sxs-lookup"><span data-stu-id="67c84-124">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="67c84-125">Bu altyapı sorunları birim testlerinizi yanlışlıkla tanıtmak yok sağlamaya yardımcı olur ve kolayca hangi çalıştırmak için test kümesini seçmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="67c84-125">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="67c84-126">Test ana</span><span class="sxs-lookup"><span data-stu-id="67c84-126">The Test Host</span></span>

<span data-ttu-id="67c84-127">ASP.NET Core tümleştirme test projeleri için eklenebilir ve gerçek web ana gerek kalmadan sunma test istekleri uygulamaları, ASP.NET Core konağa kullanılacak bir test ana içerir.</span><span class="sxs-lookup"><span data-stu-id="67c84-127">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="67c84-128">Sağlanan örnek kullanacak şekilde yapılandırılmış bir tümleştirme test projesi içeren [xUnit](https://xunit.github.io) ve Test ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="67c84-128">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="67c84-129">Kullandığı `Microsoft.AspNetCore.TestHost` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="67c84-129">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="67c84-130">Bir kez `Microsoft.AspNetCore.TestHost` paketini projeye dahil, oluşturma ve yapılandırma kullanabileceksiniz bir `TestServer` testlerinizde.</span><span class="sxs-lookup"><span data-stu-id="67c84-130">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="67c84-131">Aşağıdaki sınama bir sitenin kök yapılan bir istek "Hello World!" döndürür doğrulamak nasıl gösterir</span><span class="sxs-lookup"><span data-stu-id="67c84-131">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="67c84-132">ve ASP.NET Core boş Web şablonu Visual Studio tarafından oluşturulan varsayılan karşı başarıyla çalıştırmalısınız.</span><span class="sxs-lookup"><span data-stu-id="67c84-132">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="67c84-133">Bu test Yerleştir Act Assert düzeni kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="67c84-133">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="67c84-134">Yerleştir adım örneği oluşturan oluşturucusunda yapılır `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="67c84-134">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="67c84-135">Yapılandırılan bir `WebHostBuilder` oluşturmak için kullanılan bir `TestHost`; Bu örnekte, `Configure` testin (SUT) altında sistem yönteminden `Startup` sınıfı iletilir `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="67c84-135">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="67c84-136">Bu yöntem, istek ardışık düzenini yapılandırmak için kullanılan `TestServer` nasıl SUT sunucusunun yapılandırılması için aynı.</span><span class="sxs-lookup"><span data-stu-id="67c84-136">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="67c84-137">Test Act kısmı için bir istek yapıldığında `TestServer` örneği "/" yolu ve yanıt için bir dize olarak okunur.</span><span class="sxs-lookup"><span data-stu-id="67c84-137">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="67c84-138">Bu dize, "Hello World!" beklenen dizeyi ile karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="67c84-138">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="67c84-139">Eşleşirlerse, test geçirir; Aksi takdirde başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="67c84-139">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="67c84-140">Artık işlevselliği denetimi asal web uygulaması aracılığıyla çalıştığını doğrulamak için birkaç ek tümleştirme testleri ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="67c84-140">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="67c84-141">Değil gerçekten asal sayı denetleyicisi doğruluğunu ile bu testleri test etmeye çalıştığınız ancak bunun yerine, web uygulaması beklediğiniz yaptığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="67c84-141">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="67c84-142">Güven verir birim testi kapsamı zaten `PrimeService`, burada görüldüğü gibi:</span><span class="sxs-lookup"><span data-stu-id="67c84-142">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Test Gezgini](integration-testing/_static/test-explorer.png)

<span data-ttu-id="67c84-144">Birim testleri hakkında daha fazla bilgiyi [birim testi](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) makalesi.</span><span class="sxs-lookup"><span data-stu-id="67c84-144">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="67c84-145">MVC/Razor test tümleştirme</span><span class="sxs-lookup"><span data-stu-id="67c84-145">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="67c84-146">Razor görünümleri içeren test projeleri gerektiren `<PreserveCompilationContext>` ayarlanabilir true olarak *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="67c84-146">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="67c84-147">Bu öğe eksik projeleri aşağıdakine benzer bir hata oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67c84-147">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="67c84-148">Ara yazılım kullanacak şekilde yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="67c84-148">Refactoring to use middleware</span></span>

<span data-ttu-id="67c84-149">Yeniden düzenleme davranışını değiştirmeden tasarımını geliştirmek için uygulamanın kodunu değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="67c84-149">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="67c84-150">Bu Yardım değişikliklerden önceki ve sonraki sistemin davranışı aynı kalmasını sağlamak bu yana testleri, geçirme, bir paketi olduğunda ideal olarak yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="67c84-150">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="67c84-151">Web uygulamasının denetleme mantığı asal uygulanır şekilde bakarak `Configure` yöntemi, bkz:</span><span class="sxs-lookup"><span data-stu-id="67c84-151">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="67c84-152">Bu kod çalışır, ancak nasıl işlevselliği bu tür bir ASP.NET Core uygulamada, bu bile kadar basit uygulamak istediğiniz gölgeden uzak olan.</span><span class="sxs-lookup"><span data-stu-id="67c84-152">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="67c84-153">Ne düşünün `Configure` yöntemi uygulamamız görünecektir gibi başka bir URL uç eklediğiniz her zaman bu kadar kodu ekleyin gerekirse!</span><span class="sxs-lookup"><span data-stu-id="67c84-153">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="67c84-154">Dikkate alınması gereken bir seçenek ekleme [MVC](xref:mvc/overview) prime denetimi işlemek için uygulama ve bir denetleyici oluşturma.</span><span class="sxs-lookup"><span data-stu-id="67c84-154">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="67c84-155">Ancak, şu anda yok varsayarak bir bit herhangi diğer MVC işlevselliği, overkill.</span><span class="sxs-lookup"><span data-stu-id="67c84-155">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="67c84-156">Ancak, ASP.NET Core avantajlarından yararlanmak [ara yazılımı](xref:fundamentals/middleware), hangi yardımcı olacak bize denetleme mantığı kendi sınıfında asal kapsüllemek ve daha iyi elde [sorunları ayrılması](http://deviq.com/separation-of-concerns/) içinde `Configure` yöntem.</span><span class="sxs-lookup"><span data-stu-id="67c84-156">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="67c84-157">Ara yazılım kullanan ara yazılım sınıfı bekliyor biçimde bir parametre olarak belirtilmesi için bir yola izin vermek istediğiniz bir `RequestDelegate` ve `PrimeCheckerOptions` kurucusu örneği.</span><span class="sxs-lookup"><span data-stu-id="67c84-157">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="67c84-158">İstek yolu bu ara yazılımın nedir eşleşmiyorsa beklediğiniz şekilde yapılandırılmış, sadece zincirde bir sonraki ara yazılım çağırın ve başka hiçbir şey yapma.</span><span class="sxs-lookup"><span data-stu-id="67c84-158">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="67c84-159">İçinde uygulama kodu kalan `Configure` olduğunu `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="67c84-159">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="67c84-160">Ara yazılım bağımlı olduğundan `PrimeService` service, ayrıca isteyen Oluşturucusu ile bu hizmet örneği.</span><span class="sxs-lookup"><span data-stu-id="67c84-160">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="67c84-161">Bu hizmeti aracılığıyla framework sağlayacak [bağımlılık ekleme](xref:fundamentals/dependency-injection), yapılandırılmış, örneğin varsayılarak `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="67c84-161">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="67c84-162">Yolunu eşleştiğinde bu ara yazılım istek temsilci zincirde bir uç nokta gibi davranır, yoktur hiçbir çağrısına `_next.Invoke` ne zaman bu ara yazılım isteği işler.</span><span class="sxs-lookup"><span data-stu-id="67c84-162">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="67c84-163">Bu ara yazılım ve yapılandırma, kolaylaştırmak için oluşturulan bazı yararlı genişletme yöntemleri işlenmiş olan `Configure` yöntemi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="67c84-163">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="67c84-164">Bu yeniden düzenleme aşağıdaki, tüm tümleştirme testlerinizi geçtiğiniz beri web uygulaması halen önceki gibi çalışır durumda olduğundan emin olursunuz.</span><span class="sxs-lookup"><span data-stu-id="67c84-164">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="67c84-165">Bir yeniden düzenleme tamamladıktan sonra testlerinizi geçirmek için kaynak denetimi değişikliklerinizi uygulamak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="67c84-165">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="67c84-166">Test güdümlü geliştirme, kapattığınızdan varsa [kırmızı yeşil düzenleme döngüyü yürütme eklemeyi düşünün](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="67c84-166">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="67c84-167">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="67c84-167">Resources</span></span>

* [<span data-ttu-id="67c84-168">Birim testi</span><span class="sxs-lookup"><span data-stu-id="67c84-168">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="67c84-169">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="67c84-169">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="67c84-170">Test denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="67c84-170">Testing controllers</span></span>](xref:mvc/controllers/testing)
