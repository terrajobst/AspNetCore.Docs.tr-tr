---
title: ASP.NET Core projeleri sorunlarını giderme
author: Rick-Anderson
description: ASP.NET Core projelerle uyarıları ve hataları anlayın ve sorun giderin.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: b434af2dd046045836d2f6f7f7b7b2d57699bedc
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308277"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="7ede5-103">ASP.NET Core projeleri sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="7ede5-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="7ede5-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ede5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ede5-105">Aşağıdaki bağlantılar sorun giderme kılavuzunu sağlar:</span><span class="sxs-lookup"><span data-stu-id="7ede5-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="7ede5-106">NDC Konferansı (Londra, 2018): ASP.NET Core uygulamalardaki sorunları tanılama</span><span class="sxs-lookup"><span data-stu-id="7ede5-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="7ede5-107">ASP.NET blogu: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="7ede5-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="7ede5-108">.NET Core SDK uyarılar</span><span class="sxs-lookup"><span data-stu-id="7ede5-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="7ede5-109">.NET Core SDK hem 32-bit hem de 64-bit sürümleri yüklenir</span><span class="sxs-lookup"><span data-stu-id="7ede5-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="7ede5-110">**Yeni proje** iletişim kutusunda ASP.NET Core için aşağıdaki uyarıyı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7ede5-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7ede5-111">.NET Core SDK hem 32-bit hem de 64-bit sürümleri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="7ede5-112">Yalnızca\\' C: Program Files\\DotNet\\SDK\\' konumunda yüklü olan 64 bitlik sürümlerden alınan şablonlar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="7ede5-113">Bu uyarı, [.NET Core SDK](https://www.microsoft.com/net/download/all) hem 32-bit (x86) hem de 64-bit (x64) sürümleri yüklendiğinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="7ede5-114">Her iki sürümün de sık yüklenebileceği yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7ede5-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="7ede5-115">İlk olarak 32 bitlik bir makine kullanarak .NET Core SDK yükleyicisini indirdiniz, ancak bu dosyayı bir 64 bit makineye kopyaladınız ve bu makinede yüklediniz.</span><span class="sxs-lookup"><span data-stu-id="7ede5-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="7ede5-116">32 bit .NET Core SDK başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="7ede5-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="7ede5-117">Yanlış sürüm indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="7ede5-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="7ede5-118">Bu uyarıyı engellemek için 32 bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="7ede5-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7ede5-119">**Denetim Masası** > **Programlar ve Özellikler** > 'den Kaldır**bir programı kaldırma veya değiştirme**.</span><span class="sxs-lookup"><span data-stu-id="7ede5-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7ede5-120">Uyarının neden oluştuğunu ve etkilerini anladıysanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ede5-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="7ede5-121">.NET Core SDK birden çok konuma yüklendi</span><span class="sxs-lookup"><span data-stu-id="7ede5-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="7ede5-122">**Yeni proje** iletişim kutusunda ASP.NET Core için aşağıdaki uyarıyı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7ede5-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7ede5-123">.NET Core SDK birden çok konuma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="7ede5-124">Yalnızca\\' C: Program Files\\DotNet\\SDK\\' konumunda yüklü SDK 'lardan şablonlar görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="7ede5-125">*C:\\Program Files\\DotNet\\SDKdışındabirdizindeenazbir.NETCoreSDKyüklemenizolduğundabuiletiyigörürsünüz.\\*</span><span class="sxs-lookup"><span data-stu-id="7ede5-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="7ede5-126">Genellikle bu, .NET Core SDK MSI yükleyicisi yerine Kopyala/Yapıştır kullanılarak bir makineye dağıtıldığında meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="7ede5-127">Bu uyarıyı engellemek için tüm 32-bit .NET Core SDK 'larını ve çalışma zamanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="7ede5-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="7ede5-128">**Denetim Masası** > **Programlar ve Özellikler** > 'den Kaldır**bir programı kaldırma veya değiştirme**.</span><span class="sxs-lookup"><span data-stu-id="7ede5-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7ede5-129">Uyarının neden oluştuğunu ve etkilerini anladıysanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ede5-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="7ede5-130">.NET Core SDK 'Ları algılanmadı</span><span class="sxs-lookup"><span data-stu-id="7ede5-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="7ede5-131">ASP.NET Core için Visual Studio **Yeni proje** iletişim kutusunda aşağıdaki uyarıyı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7ede5-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="7ede5-132">.NET Core SDK 'Ları algılanmadı, ortam değişkenine `PATH`dahil olduklarından emin olun.</span><span class="sxs-lookup"><span data-stu-id="7ede5-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="7ede5-133">Bir `dotnet` komut yürütürken, uyarı şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="7ede5-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="7ede5-134">Yüklü olan DotNet SDK 'Ları bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="7ede5-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="7ede5-135">Bu uyarılar, ortam değişkeni `PATH` makinede herhangi bir .NET Core SDK 'sı üzerine işaret etmez görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="7ede5-136">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="7ede5-136">To resolve this problem:</span></span>

* <span data-ttu-id="7ede5-137">.NET Core SDK 'i yükler.</span><span class="sxs-lookup"><span data-stu-id="7ede5-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="7ede5-138">[.Net Indirmelerinde](https://dotnet.microsoft.com/download)en son yükleyiciyi edinin.</span><span class="sxs-lookup"><span data-stu-id="7ede5-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="7ede5-139">Ortam değişkeninin SDK 'nın yüklü olduğu konuma işaret ettiğini doğrulayın (`C:\Program Files\dotnet\` 64 bit/x64 veya `C:\Program Files (x86)\dotnet\` 32 bit/x86 için). `PATH`</span><span class="sxs-lookup"><span data-stu-id="7ede5-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="7ede5-140">SDK yükleyicisi normalde ' i ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="7ede5-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="7ede5-141">Aynı bit genişliği SDK 'larını ve çalışma zamanlarını aynı makineye her zaman yükler.</span><span class="sxs-lookup"><span data-stu-id="7ede5-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="7ede5-142">.NET Core barındırma paketi yüklendikten sonra SDK eksik</span><span class="sxs-lookup"><span data-stu-id="7ede5-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="7ede5-143">.NET Core [barındırma](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) paketinin yüklenmesi, `PATH` .NET Core 'un 32-bit (x86) sürümünü (`C:\Program Files (x86)\dotnet\`) işaret etmek üzere .NET Core çalışma zamanını yüklerken değiştirir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="7ede5-144">Bu, 32-bit (x86) .NET Core `dotnet` komutu kullanıldığında ([.NET Core SDK 'ları algılanmadığında](#no-net-core-sdks-were-detected)) eksik SDK 'lara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="7ede5-145">Bu sorunu çözmek için, öncesinde `C:\Program Files\dotnet\` `C:\Program Files (x86)\dotnet\` `PATH`bir konuma geçin.</span><span class="sxs-lookup"><span data-stu-id="7ede5-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="7ede5-146">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="7ede5-146">Obtain data from an app</span></span>

<span data-ttu-id="7ede5-147">Bir uygulama isteklere yanıt veriyorsa, ara yazılım kullanarak uygulamadan aşağıdaki verileri alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7ede5-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="7ede5-148">İstek &ndash; yöntemi, düzen, ana bilgisayar, pathbase, yol, sorgu dizesi, üst bilgiler</span><span class="sxs-lookup"><span data-stu-id="7ede5-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="7ede5-149">Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="7ede5-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="7ede5-150">Kimlik &ndash; adı, görünen ad</span><span class="sxs-lookup"><span data-stu-id="7ede5-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="7ede5-151">Yapılandırma ayarları</span><span class="sxs-lookup"><span data-stu-id="7ede5-151">Configuration settings</span></span>
* <span data-ttu-id="7ede5-152">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="7ede5-152">Environment variables</span></span>

<span data-ttu-id="7ede5-153">Aşağıdaki [Ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) kodunu `Startup.Configure` metodun istek işleme işlem hattının başına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ede5-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="7ede5-154">Bu ortam, kodun yalnızca geliştirme ortamında yürütülmesini sağlamak için, ara yazılım çalıştırılmadan önce denetlenir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="7ede5-155">Ortamı edinmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7ede5-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="7ede5-156">Metodunuyöntemine`Startup.Configure` ekleyin ve yerel değişkenle ortamı kontrol edin. `IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7ede5-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="7ede5-157">Aşağıdaki örnek kodda bu yaklaşım gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7ede5-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="7ede5-158">Ortamı `Startup` sınıfındaki bir özelliğe atayın.</span><span class="sxs-lookup"><span data-stu-id="7ede5-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="7ede5-159">Özelliğini kullanarak ortamı denetleyin (örneğin, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="7ede5-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
