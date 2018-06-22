---
title: ASP.NET Core 2.1 yenilikler nelerdir?
author: isaac2004
description: ASP.NET Core 2.1 yeni özellikler hakkında bilgi edinin.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: 98e867ec77a79102bc536fe5580c8796cf142feb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291653"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="db4aa-103">ASP.NET Core 2.1 yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="db4aa-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="db4aa-104">Bu makalede, ASP.NET Core 2.1 en önemli değişiklikleri ile ilgili belgelere bağlantılar vurgular.</span><span class="sxs-lookup"><span data-stu-id="db4aa-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="db4aa-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="db4aa-105">SignalR</span></span>

<span data-ttu-id="db4aa-106">SignalR ASP.NET Core 2.1 için yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="db4aa-107">ASP.NET Core SignalR bazı geliştirmeler içerir:</span><span class="sxs-lookup"><span data-stu-id="db4aa-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="db4aa-108">Basitleştirilmiş bir genişleme modeli.</span><span class="sxs-lookup"><span data-stu-id="db4aa-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="db4aa-109">Yeni bir JavaScript istemci hiçbir jQuery bağımlılığı.</span><span class="sxs-lookup"><span data-stu-id="db4aa-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="db4aa-110">Üzerinde MessagePack bağlı olan yeni sıkıştırılmış ikili protokolü.</span><span class="sxs-lookup"><span data-stu-id="db4aa-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="db4aa-111">Özel protokoller için destek.</span><span class="sxs-lookup"><span data-stu-id="db4aa-111">Support for custom protocols.</span></span>
* <span data-ttu-id="db4aa-112">Yeni bir akış yanıt modeli.</span><span class="sxs-lookup"><span data-stu-id="db4aa-112">A new streaming response model.</span></span>
* <span data-ttu-id="db4aa-113">Üzerinde tam WebSockets tabanlı istemciler için destek.</span><span class="sxs-lookup"><span data-stu-id="db4aa-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="db4aa-114">Daha fazla bilgi için bkz: [ASP.NET Core SignalR](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="db4aa-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="db4aa-115">Razor sınıf kitaplıkları</span><span class="sxs-lookup"><span data-stu-id="db4aa-115">Razor class libraries</span></span>

<span data-ttu-id="db4aa-116">ASP.NET Core 2.1 yapı ve Razor tabanlı UI kitaplığa ve birden çok projeler arasında paylaşmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="db4aa-117">Bir NuGet paketi paketlenen sınıf kitaplığı projesine Razor dosyalarıyla yeni Razor SDK'sı sağlar.</span><span class="sxs-lookup"><span data-stu-id="db4aa-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="db4aa-118">Görünümleri ve kitaplıkları sayfalarında otomatik olarak bulunur ve uygulama tarafından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="db4aa-119">Yapıda Razor derleme tümleştirerek:</span><span class="sxs-lookup"><span data-stu-id="db4aa-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="db4aa-120">Uygulama başlangıç zamanı önemli ölçüde daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="db4aa-121">Hızlı Razor görünümleri ve çalışma zamanında sayfalarına güncelleştirmelerin hala kullanılabilir bir yinelemeli geliştirme iş akışının parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="db4aa-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="db4aa-122">Daha fazla bilgi için bkz: [Razor sınıf kitaplığı proje kullanarak yeniden kullanılabilir kullanıcı Arabirimi oluşturma](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="db4aa-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="db4aa-123">Kimlik kullanıcı Arabirimi kitaplığı ve yapı iskelesi</span><span class="sxs-lookup"><span data-stu-id="db4aa-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="db4aa-124">ASP.NET Core 2.1 sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="db4aa-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="db4aa-125">Kimliği içeren uygulamaları kimlik Razor sınıf kitaplığı (RCL) bulunan kaynak koduna seçerek eklemek için yeni kimlik iskele kurucu uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db4aa-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="db4aa-126">Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db4aa-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="db4aa-127">Örneğin, kayıt için kullanılan kodu oluşturmak için iskele kurucu istemeniz.</span><span class="sxs-lookup"><span data-stu-id="db4aa-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="db4aa-128">Oluşturulan kod kimlik RCL aynı kodu daha önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="db4aa-129">Yapmak uygulamaları **değil** dahil kimlik doğrulama kimlik iskele kurucu RCL kimlik paketini eklemek için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="db4aa-130">Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="db4aa-131">Daha fazla bilgi için bkz: [İskele kimlik ASP.NET Core projelerinde](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="db4aa-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="db4aa-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="db4aa-132">HTTPS</span></span>

<span data-ttu-id="db4aa-133">Güvenlik ve gizlilik hakkında daha fazla odak ile web uygulamaları için HTTPS'yi etkinleştirme önemlidir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="db4aa-134">HTTPS zorlama Web'de katı giderek büyüyor.</span><span class="sxs-lookup"><span data-stu-id="db4aa-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="db4aa-135">HTTPS kullanmayan siteleri güvenli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="db4aa-136">Tarayıcılar (Chromium, Mozilla), web özellikleri güvenli bağlamından kullanılmalıdır zorlamak başladılar.</span><span class="sxs-lookup"><span data-stu-id="db4aa-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="db4aa-137">[GDPR](xref:security/gdpr) kullanıcı gizliliğini korumak için HTTPS kullanılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="db4aa-138">Üretimde HTTPS kullanarak kritik olsa da, geliştirme HTTPS kullanarak sorunları (örneğin, güvenli olmayan bağlantıları) dağıtımda önlenmesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="db4aa-139">ASP.NET Core 2.1 birkaç geliştirme HTTPS kullanmak üzere ve üretimde HTTPS yapılandırmak için daha kolay geliştirme içerir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="db4aa-140">Daha fazla bilgi için bkz: [zorunlu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="db4aa-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="db4aa-141">Üzerinde varsayılan olarak</span><span class="sxs-lookup"><span data-stu-id="db4aa-141">On by default</span></span>

<span data-ttu-id="db4aa-142">Güvenli Web sitesi geliştirme kolaylaştırmak için HTTPS artık varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="db4aa-143">Dinlediği Kestrel 2.1 başlayarak, `https://localhost:5001` yerel geliştirme sertifikası olduğunda mevcut.</span><span class="sxs-lookup"><span data-stu-id="db4aa-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="db4aa-144">Geliştirme sertifikası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="db4aa-144">A development certificate is created:</span></span>

* <span data-ttu-id="db4aa-145">SDK'yı ilk kez kullandığınızda, .NET Core SDK ilk çalıştırma deneyimi bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="db4aa-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="db4aa-146">Yeni kullanarak el ile `dev-certs` aracı.</span><span class="sxs-lookup"><span data-stu-id="db4aa-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="db4aa-147">Çalıştırma `dotnet dev-certs https --trust` sertifikaya güvenmelerini.</span><span class="sxs-lookup"><span data-stu-id="db4aa-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="db4aa-148">HTTPS yeniden yönlendirmesi ve zorlama</span><span class="sxs-lookup"><span data-stu-id="db4aa-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="db4aa-149">Web uygulamaları genellikle hem HTTP hem de HTTPS dinleme ancak HTTPS için tüm HTTP trafiği yeniden yönlendirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="db4aa-150">2.1 akıllıca yönlendiren özel HTTPS yeniden yönlendirmesi ara yazılım yapılandırma varlığına göre veya ilişkili sunucu bağlantı noktaları sunulan.</span><span class="sxs-lookup"><span data-stu-id="db4aa-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="db4aa-151">HTTPS kullanımı daha fazla zorunlu kullanarak [HTTP katı Aktarım güvenlik protokolü (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="db4aa-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="db4aa-152">HSTS daima HTTPS üzerinden siteye erişmek için tarayıcılar bildirir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="db4aa-153">ASP.NET Core 2.1 max yaşı, alt etki alanları, seçeneklerini destekler HSTS ara yazılımı ekler ve listeyi HSTS dağıtılacak.</span><span class="sxs-lookup"><span data-stu-id="db4aa-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="db4aa-154">Üretim için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="db4aa-154">Configuration for production</span></span>

<span data-ttu-id="db4aa-155">Üretimde HTTPS açıkça yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="db4aa-156">2.1 içinde HTTPS Kestrel için yapılandırmak için varsayılan yapılandırma şeması eklendi.</span><span class="sxs-lookup"><span data-stu-id="db4aa-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="db4aa-157">Uygulamalar kullanacak şekilde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="db4aa-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="db4aa-158">URL'leri dahil olmak üzere birden çok uç nokta.</span><span class="sxs-lookup"><span data-stu-id="db4aa-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="db4aa-159">Daha fazla bilgi için bkz: [Kestrel web sunucusu uygulaması: uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="db4aa-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="db4aa-160">Diskteki bir dosyanın veya bir sertifika deposundan HTTPS kullanmak üzere sertifika.</span><span class="sxs-lookup"><span data-stu-id="db4aa-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="db4aa-161">GDPR</span><span class="sxs-lookup"><span data-stu-id="db4aa-161">GDPR</span></span>

<span data-ttu-id="db4aa-162">ASP.NET Core API ve şablonları bazı karşılamak amacıyla sağlar [AB genel veri koruma düzenleme (GDPR)](https://www.eugdpr.org/) gereksinimleri.</span><span class="sxs-lookup"><span data-stu-id="db4aa-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="db4aa-163">Daha fazla bilgi için bkz: [GDPR destek ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="db4aa-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="db4aa-164">A [örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) nasıl kullanılacağını gösterir ve GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonları eklenen API'leri çoğunu test olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="db4aa-165">Tümleştirme testleri</span><span class="sxs-lookup"><span data-stu-id="db4aa-165">Integration tests</span></span>

<span data-ttu-id="db4aa-166">Yeni bir paket ölçeklendirerek oluşturma ve yürütme test sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="db4aa-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="db4aa-167">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) paketi aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="db4aa-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="db4aa-168">Bağımlılık dosyası kopyalar (*\*.deps*) test projesinin içine sınanan uygulamadan *bin* klasör.</span><span class="sxs-lookup"><span data-stu-id="db4aa-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="db4aa-169">Statik dosyalar ve sayfaları/görünümleri testleri ne zaman çalıştırılır böylece bulunur içerik kök sınanan uygulamanın proje kök dizinine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="db4aa-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="db4aa-170">Sağlar [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) sınanan uygulamayla önyükleme kolaylaştırmak için sınıf [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="db4aa-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="db4aa-171">Aşağıdaki sınama kullanır [xUnit](https://xunit.github.io/) dizin sayfası bir başarı durum kodu ile doğru Content-Type üstbilgisi yükler denetlemek için:</span><span class="sxs-lookup"><span data-stu-id="db4aa-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

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

<span data-ttu-id="db4aa-172">Daha fazla bilgi için bkz: [tümleştirme testleri](xref:test/integration-tests) konu.</span><span class="sxs-lookup"><span data-stu-id="db4aa-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresult"></a><span data-ttu-id="db4aa-173">[ApiController] ActionResult</span><span class="sxs-lookup"><span data-stu-id="db4aa-173">[ApiController], ActionResult</span></span>

<span data-ttu-id="db4aa-174">ASP.NET Core 2.1 temiz ve açıklayıcı web API oluşturma kolaylaştıran yeni programlama kuralları ekler.</span><span class="sxs-lookup"><span data-stu-id="db4aa-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="db4aa-175">`ActionResult<T>` Yeni bir tür bir uygulamayı hala yanıt türünü belirten sırasında bir yanıt türü veya (IActionResult için benzer şekilde), diğer herhangi bir eylem sonucu dönmek için izin vermek için eklenir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="db4aa-176">`[ApiController]` Özniteliği de olarak eklendiğinden Web API özel kuralları ve davranışları kabul yolu.</span><span class="sxs-lookup"><span data-stu-id="db4aa-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="db4aa-177">Daha fazla bilgi için bkz: [yapı Web API ile ASP.NET çekirdek](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="db4aa-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="db4aa-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="db4aa-178">IHttpClientFactory</span></span>

<span data-ttu-id="db4aa-179">ASP.NET Core 2.1 içeren yeni bir `IHttpClientFactory` yapılandırmak ve örnekleri kullanmak kolaylaştırır hizmet `HttpClient` uygulamalarda.</span><span class="sxs-lookup"><span data-stu-id="db4aa-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="db4aa-180">`HttpClient` zaten giden HTTP istekleri için birbirine işleyicileri için temsilci seçme kavramı vardır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="db4aa-181">Fabrika:</span><span class="sxs-lookup"><span data-stu-id="db4aa-181">The factory:</span></span>

* <span data-ttu-id="db4aa-182">Örnekleri kaydetme yapar `HttpClient` daha sezgisel adlandırılmış istemci başına.</span><span class="sxs-lookup"><span data-stu-id="db4aa-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="db4aa-183">Yeniden deneme, CircuitBreakers vb. için kullanılacak ilke Polly veren Polly işleyici uygular.</span><span class="sxs-lookup"><span data-stu-id="db4aa-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="db4aa-184">Daha fazla bilgi için bkz: [başlatmak HTTP isteklerini](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="db4aa-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="db4aa-185">Kestrel aktarım yapılandırma</span><span class="sxs-lookup"><span data-stu-id="db4aa-185">Kestrel transport configuration</span></span>

<span data-ttu-id="db4aa-186">ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda dayalı.</span><span class="sxs-lookup"><span data-stu-id="db4aa-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="db4aa-187">Daha fazla bilgi için bkz: [Kestrel web sunucusu uygulaması: aktarım yapılandırma](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="db4aa-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="db4aa-188">Genel ana bilgisayar Oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="db4aa-188">Generic host builder</span></span>

<span data-ttu-id="db4aa-189">Genel ana bilgisayar Oluşturucusu (`HostBuilder`) sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="db4aa-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="db4aa-190">Bu oluşturucu, HTTP isteklerini (ileti, arka plan görevleri, vs.) işleyen olmayan uygulamalar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="db4aa-191">Daha fazla bilgi için bkz: [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="db4aa-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="db4aa-192">Güncelleştirilmiş SPA şablonları</span><span class="sxs-lookup"><span data-stu-id="db4aa-192">Updated SPA templates</span></span>

<span data-ttu-id="db4aa-193">Angular, tepki, tek sayfa uygulaması şablonları ve tepki Redux ile standart proje yapıları kullanın ve her çerçevesi için sistemler oluşturmak için güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="db4aa-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="db4aa-194">Açısal şablonu Açısal CLI üzerinde temel alır ve tepki şablonları oluşturma tepki-uygulama üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="db4aa-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>
<span data-ttu-id="db4aa-195">Daha fazla bilgi için bkz: [ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma](xref:spa/index).</span><span class="sxs-lookup"><span data-stu-id="db4aa-195">For more information, see [Use the Single Page Application templates with ASP.NET Core](xref:spa/index).</span></span>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="db4aa-196">Razor sayfalarının Razor varlıkları için arama</span><span class="sxs-lookup"><span data-stu-id="db4aa-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="db4aa-197">2.1 içinde listelenen sırayla aşağıdaki dizinlerde Razor varlıklar (örneğin, düzenleri ve kısmi) Razor sayfalarının arayın:</span><span class="sxs-lookup"><span data-stu-id="db4aa-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="db4aa-198">Geçerli sayfa klasör.</span><span class="sxs-lookup"><span data-stu-id="db4aa-198">Current Pages folder.</span></span>
1. <span data-ttu-id="db4aa-199">*/Pages/paylaşılan /*</span><span class="sxs-lookup"><span data-stu-id="db4aa-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="db4aa-200">*/Views/paylaşılan /*</span><span class="sxs-lookup"><span data-stu-id="db4aa-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="db4aa-201">Bir alanı Razor sayfalarında</span><span class="sxs-lookup"><span data-stu-id="db4aa-201">Razor Pages in an area</span></span>

<span data-ttu-id="db4aa-202">Razor sayfalarının şimdi destek [alanları](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="db4aa-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="db4aa-203">Örneği alanları görmek için bireysel kullanıcı hesapları ile yeni bir Razor sayfalarının web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db4aa-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="db4aa-204">Bireysel kullanıcı hesapları ile Razor sayfalarının web uygulamasını içeren */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="db4aa-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="db4aa-205">2. 0 2.1 için geçirme</span><span class="sxs-lookup"><span data-stu-id="db4aa-205">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="db4aa-206">Bkz: [ASP.NET çekirdek 2.0 için 2.1 geçirmek](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="db4aa-206">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="db4aa-207">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="db4aa-207">Additional information</span></span>

<span data-ttu-id="db4aa-208">Değişiklikleri tam listesi için bkz: [ASP.NET Core 2.1 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="db4aa-208">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
