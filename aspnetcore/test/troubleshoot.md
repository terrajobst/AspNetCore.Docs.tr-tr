---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: 1f53375a6aa504c989920a0bff83b400b7bd83ec
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208849"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="5f9ce-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="5f9ce-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="5f9ce-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f9ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f9ce-105">Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="5f9ce-106">NDC konferansı (Londra, 2018): ASP.NET Core uygulamalarında sorunları tanılama</span><span class="sxs-lookup"><span data-stu-id="5f9ce-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="5f9ce-107">ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="5f9ce-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="5f9ce-108">.NET core SDK'sı uyarıları</span><span class="sxs-lookup"><span data-stu-id="5f9ce-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="5f9ce-109">Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir</span><span class="sxs-lookup"><span data-stu-id="5f9ce-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="5f9ce-110">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="5f9ce-111">.NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="5f9ce-112">Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="5f9ce-114">Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="5f9ce-115">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="5f9ce-116">İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="5f9ce-117">32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="5f9ce-118">Yanlış sürümü indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="5f9ce-119">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="5f9ce-120">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="5f9ce-121">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="5f9ce-122">.NET Core SDK'sını birden çok konumda yüklenir</span><span class="sxs-lookup"><span data-stu-id="5f9ce-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="5f9ce-123">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="5f9ce-124">.NET Core SDK'sı, birden çok konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="5f9ce-125">Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="5f9ce-127">.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="5f9ce-128">Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="5f9ce-129">Tüm 32-bit .NET Core SDK'ları ve çalışma zamanları bu uyarıyı engellemek için kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-129">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="5f9ce-130">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="5f9ce-131">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="5f9ce-132">.NET çekirdek SDK algılanmadı</span><span class="sxs-lookup"><span data-stu-id="5f9ce-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="5f9ce-133">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="5f9ce-134">.NET çekirdek SDK algılanmadı, 'PATH' ortam değişkenine dahil olduklarından emin olun.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="5f9ce-136">Bu uyarı görüntülenir ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor (örneğin, `C:\Program Files\dotnet\` ve `C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="5f9ce-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="5f9ce-137">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-137">To resolve this problem:</span></span>

* <span data-ttu-id="5f9ce-138">Yükleme veya .NET Core SDK'sı yüklü olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="5f9ce-139">En son Yükleyicisi'nden elde [.NET indirir](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="5f9ce-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="5f9ce-140">Doğrulayın `PATH` ortam değişkeni SDK'sı yüklü olduğu konuma işaret eder.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="5f9ce-141">Yükleyici normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="5f9ce-142">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="5f9ce-142">Obtain data from an app</span></span>

<span data-ttu-id="5f9ce-143">Bir uygulama isteklerini yanıtlayabileceği ise, ara yazılımın kullanılması uygulamayı aşağıdaki veriler elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="5f9ce-144">İstek &ndash; yöntemi, şema, konak, pathbase, yol, sorgu dizesi, üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="5f9ce-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="5f9ce-145">Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="5f9ce-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="5f9ce-146">Kimlik &ndash; adı, görünen adı</span><span class="sxs-lookup"><span data-stu-id="5f9ce-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="5f9ce-147">Yapılandırma ayarları</span><span class="sxs-lookup"><span data-stu-id="5f9ce-147">Configuration settings</span></span>
* <span data-ttu-id="5f9ce-148">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="5f9ce-148">Environment variables</span></span>

<span data-ttu-id="5f9ce-149">Aşağıdaki yerleştirin [ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) başındaki kod `Startup.Configure` yöntemin istek işleme ardışık düzeni.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="5f9ce-150">Kod geliştirme ortamında yalnızca yürütülür emin olmak için bir ara yazılım çalıştırılmadan önce ortamı denetlenir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="5f9ce-151">Ortamı elde etmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5f9ce-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="5f9ce-152">Ekleme `IHostingEnvironment` içine `Startup.Configure` yöntemi ve onay yerel değişken ortamı.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="5f9ce-153">Aşağıdaki örnek kod, bu yaklaşım gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="5f9ce-154">Ortamı bir özelliğine atayın `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5f9ce-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="5f9ce-155">Özelliğini kullanarak ortam denetleyin (örneğin, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="5f9ce-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
