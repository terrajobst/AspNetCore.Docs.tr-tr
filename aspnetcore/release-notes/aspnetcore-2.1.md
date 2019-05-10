---
title: ASP.NET Core 2.1 yenilikler nelerdir?
author: isaac2004
description: ASP.NET Core 2.1 yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/30/2019
uid: aspnetcore-2.1
ms.openlocfilehash: 359f961db768b9048427c8ab296ee3e035879408
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086401"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="17d45-103">ASP.NET Core 2.1 yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="17d45-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="17d45-104">Bu makalede, ASP.NET Core 2.1 ile ilgili belgelere bağlantılar en önemli değişiklikleri vurgular.</span><span class="sxs-lookup"><span data-stu-id="17d45-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="17d45-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="17d45-105">SignalR</span></span>

<span data-ttu-id="17d45-106">SignalR, ASP.NET Core 2.1 için yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="17d45-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="17d45-107">ASP.NET Core SignalR geliştirmeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="17d45-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="17d45-108">Basitleştirilmiş bir ölçek genişletme modeli.</span><span class="sxs-lookup"><span data-stu-id="17d45-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="17d45-109">JQuery bağımlılığı olmayan ile yeni bir JavaScript istemci.</span><span class="sxs-lookup"><span data-stu-id="17d45-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="17d45-110">MessagePack üzerinde bağlı olan yeni sıkıştırılmış ikili protokolü.</span><span class="sxs-lookup"><span data-stu-id="17d45-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="17d45-111">Özel protokoller için destek.</span><span class="sxs-lookup"><span data-stu-id="17d45-111">Support for custom protocols.</span></span>
* <span data-ttu-id="17d45-112">Yeni akış yanıt modeli.</span><span class="sxs-lookup"><span data-stu-id="17d45-112">A new streaming response model.</span></span>
* <span data-ttu-id="17d45-113">Üzerinde tam WebSockets tabanlı istemciler için destek.</span><span class="sxs-lookup"><span data-stu-id="17d45-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="17d45-114">Daha fazla bilgi için [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="17d45-114">For more information, see [ASP.NET Core SignalR](xref:signalr/introduction).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="17d45-115">Razor sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="17d45-115">Razor class libraries</span></span>

<span data-ttu-id="17d45-116">ASP.NET Core 2.1 oluşturmak ve Razor tabanlı UI kitaplığa ve birden çok projede paylaşmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="17d45-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="17d45-117">Sınıf kitaplığı projesine bir NuGet paketi paketlenen Razor dosyalarıyla yeni Razor SDK'sı sağlar.</span><span class="sxs-lookup"><span data-stu-id="17d45-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="17d45-118">Görünümlere ve sayfalara kitaplıklarında otomatik olarak bulunur ve uygulama tarafından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="17d45-119">Razor derleme yapıda tümleştirerek:</span><span class="sxs-lookup"><span data-stu-id="17d45-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="17d45-120">Uygulama başlatma süresini önemli ölçüde daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="17d45-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="17d45-121">Razor görünümleri ve çalışma zamanında sayfalarına hızlı güncelleştirmeler hala kullanılabilir bir yinelemeli geliştirme iş akışının parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="17d45-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="17d45-122">Daha fazla bilgi için [Razor sınıf kitaplığı projesi kullanarak yeniden kullanılabilir kullanıcı Arabirimi oluşturma](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="17d45-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="17d45-123">Kimlik kullanıcı Arabirimi kitaplığı ve yapı iskelesi</span><span class="sxs-lookup"><span data-stu-id="17d45-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="17d45-124">ASP.NET Core 2.1 sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="17d45-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="17d45-125">Kimlik içeren uygulamaları seçmeli olarak kimlik Razor sınıf kitaplığı (RCL) yer alan kaynak kodu eklemek için yeni kimlik destek uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17d45-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="17d45-126">Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17d45-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="17d45-127">Örneğin, kayıt için kullanılan kod üretmek için iskele kurucu toplamasını.</span><span class="sxs-lookup"><span data-stu-id="17d45-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="17d45-128">Oluşturulan kod aynı kimlik RCL kodda daha önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="17d45-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="17d45-129">Yapan uygulamalar **değil** dahil kimlik doğrulaması kimlik iskele kurucu RCL kimlik paketini eklemek için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="17d45-130">Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="17d45-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="17d45-131">Daha fazla bilgi için [İskele kimlik ASP.NET Core projelerinde](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="17d45-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="17d45-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="17d45-132">HTTPS</span></span>

<span data-ttu-id="17d45-133">Güvenlik ve gizlilik hakkında daha fazla odaklanma ile web apps için HTTPS'yi etkinleştirme önemlidir.</span><span class="sxs-lookup"><span data-stu-id="17d45-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="17d45-134">HTTPS zorlama Web'de giderek katı gelmektedir.</span><span class="sxs-lookup"><span data-stu-id="17d45-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="17d45-135">HTTPS kullanmayın siteleri güvenli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="17d45-136">Web özellikleri güvenli bağlamından kullanılmalıdır zorlamak tarayıcı (Chromium, Mozilla) başlıyor.</span><span class="sxs-lookup"><span data-stu-id="17d45-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="17d45-137">[GDPR](xref:security/gdpr) kullanıcı gizliliğini korumak için HTTPS kullanılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="17d45-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="17d45-138">HTTPS kullanarak geliştirme, üretimde HTTPS kullanarak kritik olsa da, (örneğin, güvenli bağlantılar) dağıtım sorunlarını engellemeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="17d45-139">ASP.NET Core 2.1 geliştirme HTTPS kullanmak için ve HTTPS üretimde yapılandırmak için daha kolay geliştirmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="17d45-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="17d45-140">Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="17d45-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="17d45-141">Üzerinde varsayılan olarak</span><span class="sxs-lookup"><span data-stu-id="17d45-141">On by default</span></span>

<span data-ttu-id="17d45-142">Güvenli Web sitesi geliştirme kolaylaştırmak için HTTPS artık varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="17d45-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="17d45-143">Dinlediği Kestrel 2.1 itibaren `https://localhost:5001` yerel geliştirme sertifikası olduğunda mevcut.</span><span class="sxs-lookup"><span data-stu-id="17d45-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="17d45-144">Bir geliştirme sertifikası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="17d45-144">A development certificate is created:</span></span>

* <span data-ttu-id="17d45-145">SDK'yı ilk kez kullandığınızda, .NET Core SDK'yı ilk kez çalıştırma deneyimi bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="17d45-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="17d45-146">El ile kullanarak yeni `dev-certs` aracı.</span><span class="sxs-lookup"><span data-stu-id="17d45-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="17d45-147">Çalıştırma `dotnet dev-certs https --trust` sertifikasına güvenmek için.</span><span class="sxs-lookup"><span data-stu-id="17d45-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="17d45-148">HTTPS yeniden yönlendirmesi ve zorlama</span><span class="sxs-lookup"><span data-stu-id="17d45-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="17d45-149">Web uygulamaları, genellikle hem HTTP hem de HTTPS üzerinde dinleme yapar ancak ardından tüm HTTP trafiğini HTTPS'ye yönlendirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="17d45-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="17d45-150">2.1, akıllı bir şekilde yönlendiren özel HTTPS yeniden yönlendirmesi ara yazılım yapılandırma varlığını temel alarak veya ilişkili sunucusu bağlantı noktaları sunulmuş.</span><span class="sxs-lookup"><span data-stu-id="17d45-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="17d45-151">HTTPS kullanımı daha fazla zorunlu kullanarak [HTTP katı Aktarım güvenlik protokolü (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="17d45-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="17d45-152">Her zaman HTTPS üzerinden erişmesini tarayıcılar HSTS bildirir.</span><span class="sxs-lookup"><span data-stu-id="17d45-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="17d45-153">ASP.NET Core 2.1 en yüksek yaş, alt etki alanlarını, seçeneklerini destekler HSTS ara yazılım ekler ve liste HSTS dağıtılacak.</span><span class="sxs-lookup"><span data-stu-id="17d45-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="17d45-154">Üretim için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17d45-154">Configuration for production</span></span>

<span data-ttu-id="17d45-155">Üretim ortamında, HTTPS açıkça yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="17d45-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="17d45-156">2.1 içinde Kestrel HTTPS yapılandırmak için varsayılan yapılandırma şeması eklendi.</span><span class="sxs-lookup"><span data-stu-id="17d45-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="17d45-157">Uygulamaları kullanmak için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="17d45-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="17d45-158">URL'leri dahil olmak üzere birden fazla uç nokta.</span><span class="sxs-lookup"><span data-stu-id="17d45-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="17d45-159">Daha fazla bilgi için [Kestrel web sunucusu uygulaması: Uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="17d45-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="17d45-160">HTTPS disk üzerindeki bir dosyadan veya bir sertifika deposu için kullanılacak sertifika.</span><span class="sxs-lookup"><span data-stu-id="17d45-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="17d45-161">GDPR</span><span class="sxs-lookup"><span data-stu-id="17d45-161">GDPR</span></span>

<span data-ttu-id="17d45-162">ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri.</span><span class="sxs-lookup"><span data-stu-id="17d45-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="17d45-163">Daha fazla bilgi için [GDPR desteği de ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="17d45-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="17d45-164">A [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) nasıl kullanılacağını gösterir ve GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonlarına eklenen API'leri çoğunu sınamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="17d45-164">A [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="17d45-165">Tümleştirme testleri</span><span class="sxs-lookup"><span data-stu-id="17d45-165">Integration tests</span></span>

<span data-ttu-id="17d45-166">Yeni bir paket ölçeklendirerek oluşturma ve yürütme test sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="17d45-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="17d45-167">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) paket, aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="17d45-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="17d45-168">Bağımlılık dosya kopyalar (*\*.deps*) test projesinin içinde test edilmiş uygulamasından *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="17d45-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="17d45-169">İçerik kök test edilen uygulamanın proje kök dizinine ayarlar, böylece zaman testler yürütülmeden statik dosyalar ve sayfalar/görünümler bulunur.</span><span class="sxs-lookup"><span data-stu-id="17d45-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="17d45-170">Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) önyükleme ile test edilen uygulamayı kolaylaştırmak için sınıf [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="17d45-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="17d45-171">Aşağıdaki test kullandığı [xUnit](https://xunit.github.io/) dizin sayfası doğru Content-Type üst bilgisiyle bir başarı durum kodu ile yüklendiğini kontrol etmek için:</span><span class="sxs-lookup"><span data-stu-id="17d45-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

<span data-ttu-id="17d45-172">Daha fazla bilgi için [tümleştirme testleri](xref:test/integration-tests) konu.</span><span class="sxs-lookup"><span data-stu-id="17d45-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="17d45-173">[ApiController] ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="17d45-173">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="17d45-174">ASP.NET Core 2.1 temiz ve açıklayıcı web API'lerini de oluşturabilirler daha kolay hale getirmek ve programlama yeni kurallar ekler.</span><span class="sxs-lookup"><span data-stu-id="17d45-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="17d45-175">`ActionResult<T>` Yeni bir tür, hala yanıt türü belirten sırasında yanıt türü veya (IActionResult için benzer şekilde), diğer herhangi bir eylem sonucu döndürmek uygulama izin verecek şekilde eklenir.</span><span class="sxs-lookup"><span data-stu-id="17d45-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="17d45-176">`[ApiController]` Özniteliği de olarak eklendiğinden, Web API özel kuralları ve davranışları için iyileştirilmiş şekilde.</span><span class="sxs-lookup"><span data-stu-id="17d45-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="17d45-177">Daha fazla bilgi için [ASP.NET Core Web API yapı](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="17d45-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="17d45-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="17d45-178">IHttpClientFactory</span></span>

<span data-ttu-id="17d45-179">ASP.NET Core 2.1 içeren yeni bir `IHttpClientFactory` yapılandırmak ve örneklerini kullanma kolaylaştırması hizmet `HttpClient` uygulamalarında.</span><span class="sxs-lookup"><span data-stu-id="17d45-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="17d45-180">`HttpClient` Giden HTTP istekleri için birbirine işleyicileri temsilci olarak görevlendirme kavramı zaten sahip.</span><span class="sxs-lookup"><span data-stu-id="17d45-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="17d45-181">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="17d45-181">The factory:</span></span>

* <span data-ttu-id="17d45-182">' In örneğini kaydetme yapar `HttpClient` daha sezgisel adlandırılmış istemci başına.</span><span class="sxs-lookup"><span data-stu-id="17d45-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="17d45-183">Yeniden deneme CircuitBreakers, vb. için kullanılacak ilkeleri Polly izin veren bir Polly işleyici uygular.</span><span class="sxs-lookup"><span data-stu-id="17d45-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="17d45-184">Daha fazla bilgi için [HTTP isteklerini başlatma](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="17d45-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="17d45-185">Kestrel'i aktarım yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17d45-185">Kestrel transport configuration</span></span>

<span data-ttu-id="17d45-186">ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre.</span><span class="sxs-lookup"><span data-stu-id="17d45-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="17d45-187">Daha fazla bilgi için [Kestrel web sunucusu uygulaması: Aktarım yapılandırma](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="17d45-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="17d45-188">Genel konak Oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="17d45-188">Generic host builder</span></span>

<span data-ttu-id="17d45-189">Genel konak Oluşturucusu (`HostBuilder`) eklendi.</span><span class="sxs-lookup"><span data-stu-id="17d45-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="17d45-190">Bu oluşturucu, HTTP isteklerini (ileti, arka plan görevleri, vs.) mıdl'ye işleme uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="17d45-191">Daha fazla bilgi için [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="17d45-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="17d45-192">Güncelleştirilmiş SPA şablonları</span><span class="sxs-lookup"><span data-stu-id="17d45-192">Updated SPA templates</span></span>

<span data-ttu-id="17d45-193">Tek sayfalı uygulama şablonları için Angular, React ve Redux ile React standart proje yapılarını kullanın ve her bir çerçeve için sistemleri oluşturmak için güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="17d45-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="17d45-194">Angular CLI Angular şablonu temel alır ve React şablonlar oluşturma react-uygulama temel alır.</span><span class="sxs-lookup"><span data-stu-id="17d45-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>

<span data-ttu-id="17d45-195">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="17d45-195">For more information, see:</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="17d45-196">Razor sayfaları için Razor varlıkları arama</span><span class="sxs-lookup"><span data-stu-id="17d45-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="17d45-197">2.1 içinde listelenen sırayla aşağıdaki dizinlerde Razor varlıklar (örneğin, düzenler ve kısmi bölümleri) Razor sayfaları arayın:</span><span class="sxs-lookup"><span data-stu-id="17d45-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="17d45-198">Geçerli sayfa klasör.</span><span class="sxs-lookup"><span data-stu-id="17d45-198">Current Pages folder.</span></span>
1. <span data-ttu-id="17d45-199">*/Pages/paylaşılan /*</span><span class="sxs-lookup"><span data-stu-id="17d45-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="17d45-200">*/Views/paylaşılan /*</span><span class="sxs-lookup"><span data-stu-id="17d45-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="17d45-201">Bir alanı, Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="17d45-201">Razor Pages in an area</span></span>

<span data-ttu-id="17d45-202">Razor sayfaları artık destek [alanları](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="17d45-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="17d45-203">Alanların bir örnek için bireysel kullanıcı hesapları ile yeni bir Razor sayfaları web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17d45-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="17d45-204">Bireysel kullanıcı hesapları ile Razor sayfaları web uygulaması içeren */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="17d45-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="mvc-compatibility-version"></a><span data-ttu-id="17d45-205">MVC uyumluluk sürümü</span><span class="sxs-lookup"><span data-stu-id="17d45-205">MVC compatibility version</span></span>

<span data-ttu-id="17d45-206"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="17d45-206">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="17d45-207">Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="17d45-207">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="17d45-208">2.1 için 2. 0'ı geçirme</span><span class="sxs-lookup"><span data-stu-id="17d45-208">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="17d45-209">Bkz: [ASP.NET Core 2.0 için 2.1 geçiş](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="17d45-209">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="17d45-210">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="17d45-210">Additional information</span></span>

<span data-ttu-id="17d45-211">Değişikliklerin tam listesi için bkz. [ASP.NET Core 2.1 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="17d45-211">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
