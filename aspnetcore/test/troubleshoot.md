---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: test/troubleshoot
ms.openlocfilehash: 7a3361970bde2b8761c76884fc1905957d075c5c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450781"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="ab37a-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="ab37a-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="ab37a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab37a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab37a-105">Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="ab37a-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="ab37a-106">NDC konferansı (Londra, 2018): ASP.NET Core uygulamaları tanılama sorunları</span><span class="sxs-lookup"><span data-stu-id="ab37a-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="ab37a-107">ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="ab37a-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="ab37a-108">.NET core SDK'sı uyarıları</span><span class="sxs-lookup"><span data-stu-id="ab37a-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="ab37a-109">Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir</span><span class="sxs-lookup"><span data-stu-id="ab37a-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="ab37a-110">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ab37a-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="ab37a-111">.NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="ab37a-112">Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="ab37a-114">Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="ab37a-115">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ab37a-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="ab37a-116">İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="ab37a-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="ab37a-117">32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="ab37a-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="ab37a-118">Yanlış sürümü indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="ab37a-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="ab37a-119">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="ab37a-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="ab37a-120">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="ab37a-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="ab37a-121">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab37a-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="ab37a-122">.NET Core SDK'sını birden çok konumda yüklenir</span><span class="sxs-lookup"><span data-stu-id="ab37a-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="ab37a-123">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ab37a-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="ab37a-124">.NET Core SDK'sı, birden çok konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="ab37a-125">Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="ab37a-127">.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="ab37a-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="ab37a-128">Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="ab37a-129">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="ab37a-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="ab37a-130">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="ab37a-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="ab37a-131">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ab37a-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="ab37a-132">.NET çekirdek SDK algılanmadı</span><span class="sxs-lookup"><span data-stu-id="ab37a-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="ab37a-133">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ab37a-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="ab37a-134">.NET çekirdek SDK algılanmadı, 'PATH' ortam değişkenine dahil olduklarından emin olun.</span><span class="sxs-lookup"><span data-stu-id="ab37a-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="ab37a-136">Bu uyarı görüntülenir ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor.</span><span class="sxs-lookup"><span data-stu-id="ab37a-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="ab37a-137">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="ab37a-137">To resolve this problem:</span></span>

* <span data-ttu-id="ab37a-138">Yükleme veya .NET Core SDK'sı yüklü olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ab37a-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="ab37a-139">Doğrulayın `PATH` ortam değişkeni, SDK'ın yüklendiği konumun işaret eder.</span><span class="sxs-lookup"><span data-stu-id="ab37a-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="ab37a-140">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="ab37a-140">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="ab37a-141">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="ab37a-141">Obtain data from an app</span></span>

<span data-ttu-id="ab37a-142">Bir uygulama isteklerini yanıtlayabileceği ise, ara yazılımın kullanılması uygulamayı aşağıdaki veriler elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ab37a-142">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="ab37a-143">İstek &ndash; yöntemi, şema, konak, pathbase, yol, sorgu dizesi, üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="ab37a-143">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="ab37a-144">Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="ab37a-144">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="ab37a-145">Kimlik &ndash; adı, görünen adı</span><span class="sxs-lookup"><span data-stu-id="ab37a-145">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="ab37a-146">Yapılandırma ayarları</span><span class="sxs-lookup"><span data-stu-id="ab37a-146">Configuration settings</span></span>
* <span data-ttu-id="ab37a-147">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="ab37a-147">Environment variables</span></span>

<span data-ttu-id="ab37a-148">Aşağıdaki yerleştirin [ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) başındaki kod `Startup.Configure` yöntemin istek işleme ardışık düzeni.</span><span class="sxs-lookup"><span data-stu-id="ab37a-148">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="ab37a-149">Kod geliştirme ortamında yalnızca yürütülür emin olmak için bir ara yazılım çalıştırılmadan önce ortamı denetlenir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-149">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="ab37a-150">Ortamı elde etmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="ab37a-150">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="ab37a-151">Ekleme `IHostingEnvironment` içine `Startup.Configure` yöntemi ve onay yerel değişken ortamı.</span><span class="sxs-lookup"><span data-stu-id="ab37a-151">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="ab37a-152">Aşağıdaki örnek kod, bu yaklaşım gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ab37a-152">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="ab37a-153">Ortamı bir özelliğine atayın `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ab37a-153">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="ab37a-154">Özelliğini kullanarak ortam denetleyin (örneğin, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="ab37a-154">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
