---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488747"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="84470-103">ASP.NET Core projelerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="84470-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="84470-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84470-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84470-105">Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:</span><span class="sxs-lookup"><span data-stu-id="84470-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="84470-106">NDC konferansı (Londra, 2018): ASP.NET Core uygulamalarında sorunları tanılama</span><span class="sxs-lookup"><span data-stu-id="84470-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="84470-107">ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="84470-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="84470-108">.NET core SDK'sı uyarıları</span><span class="sxs-lookup"><span data-stu-id="84470-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="84470-109">Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir</span><span class="sxs-lookup"><span data-stu-id="84470-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="84470-110">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="84470-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="84470-111">.NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="84470-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="84470-112">Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84470-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="84470-113">Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="84470-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="84470-114">Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="84470-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="84470-115">İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.</span><span class="sxs-lookup"><span data-stu-id="84470-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="84470-116">32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.</span><span class="sxs-lookup"><span data-stu-id="84470-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="84470-117">Yanlış sürümü indirildi ve yüklendi.</span><span class="sxs-lookup"><span data-stu-id="84470-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="84470-118">Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın.</span><span class="sxs-lookup"><span data-stu-id="84470-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="84470-119">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="84470-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="84470-120">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84470-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="84470-121">.NET Core SDK'sını birden çok konumda yüklenir</span><span class="sxs-lookup"><span data-stu-id="84470-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="84470-122">İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="84470-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="84470-123">.NET Core SDK'sı, birden çok konumda yüklenir.</span><span class="sxs-lookup"><span data-stu-id="84470-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="84470-124">Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="84470-124">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="84470-125">.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="84470-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="84470-126">Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="84470-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="84470-127">Tüm 32-bit .NET Core SDK'ları ve çalışma zamanları bu uyarıyı engellemek için kaldırın.</span><span class="sxs-lookup"><span data-stu-id="84470-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="84470-128">Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**.</span><span class="sxs-lookup"><span data-stu-id="84470-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="84470-129">Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84470-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="84470-130">.NET çekirdek SDK algılanmadı</span><span class="sxs-lookup"><span data-stu-id="84470-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="84470-131">Visual Studio **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="84470-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="84470-132">.NET çekirdek SDK algılanmadı, ortam değişkenine dahil oldukları olun `PATH`.</span><span class="sxs-lookup"><span data-stu-id="84470-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="84470-133">Yürütülürken bir `dotnet` komutu olarak bir uyarı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="84470-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="84470-134">Herhangi bir yüklü dotnet SDK'ları bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="84470-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="84470-135">Bu uyarılar görünür ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor.</span><span class="sxs-lookup"><span data-stu-id="84470-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="84470-136">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="84470-136">To resolve this problem:</span></span>

* <span data-ttu-id="84470-137">.NET Core SDK'sını yükleyin.</span><span class="sxs-lookup"><span data-stu-id="84470-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="84470-138">En son Yükleyicisi'nden elde [.NET indirir](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="84470-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="84470-139">Doğrulayın `PATH` ortam değişkeni, SDK'sı yüklü olduğu konuma işaret (`C:\Program Files\dotnet\` için 64-bit/x64 veya `C:\Program Files (x86)\dotnet\` 32-bit/x86 için).</span><span class="sxs-lookup"><span data-stu-id="84470-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="84470-140">SDK'sı Yükleyicisi normalde ayarlar `PATH`.</span><span class="sxs-lookup"><span data-stu-id="84470-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="84470-141">Her zaman aynı bit genişliği SDK ve çalışma zamanları, aynı makineye yükleyin.</span><span class="sxs-lookup"><span data-stu-id="84470-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="84470-142">.NET Core barındırma paketini yükledikten sonra SDK'sı eksik</span><span class="sxs-lookup"><span data-stu-id="84470-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="84470-143">Yükleme [.NET Core barındırma paket](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) değiştirir `PATH` .NET core'un 32-bit (x 86) sürümüne işaret edecek şekilde .NET Core çalışma zamanı yüklendiğinde (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="84470-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="84470-144">Bu SDK'ları eksik neden olduğunda 32-bit (x 86) .NET Core `dotnet` komutu kullanılır ([.NET Core SDK algılandı](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="84470-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="84470-145">Bu sorunu çözmek için taşıma `C:\Program Files\dotnet\` önce bir konuma `C:\Program Files (x86)\dotnet\` üzerinde `PATH`.</span><span class="sxs-lookup"><span data-stu-id="84470-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="84470-146">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="84470-146">Obtain data from an app</span></span>

<span data-ttu-id="84470-147">Bir uygulama isteklerini yanıtlayabileceği ise, ara yazılımın kullanılması uygulamayı aşağıdaki veriler elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="84470-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="84470-148">İstek &ndash; yöntemi, şema, konak, pathbase, yol, sorgu dizesi, üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="84470-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="84470-149">Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="84470-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="84470-150">Kimlik &ndash; adı, görünen adı</span><span class="sxs-lookup"><span data-stu-id="84470-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="84470-151">Yapılandırma ayarları</span><span class="sxs-lookup"><span data-stu-id="84470-151">Configuration settings</span></span>
* <span data-ttu-id="84470-152">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="84470-152">Environment variables</span></span>

<span data-ttu-id="84470-153">Aşağıdaki yerleştirin [ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) başındaki kod `Startup.Configure` yöntemin istek işleme ardışık düzeni.</span><span class="sxs-lookup"><span data-stu-id="84470-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="84470-154">Kod geliştirme ortamında yalnızca yürütülür emin olmak için bir ara yazılım çalıştırılmadan önce ortamı denetlenir.</span><span class="sxs-lookup"><span data-stu-id="84470-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="84470-155">Ortamı elde etmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="84470-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="84470-156">Ekleme `IHostingEnvironment` içine `Startup.Configure` yöntemi ve onay yerel değişken ortamı.</span><span class="sxs-lookup"><span data-stu-id="84470-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="84470-157">Aşağıdaki örnek kod, bu yaklaşım gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="84470-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="84470-158">Ortamı bir özelliğine atayın `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="84470-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="84470-159">Özelliğini kullanarak ortam denetleyin (örneğin, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="84470-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
