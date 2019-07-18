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
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core projeleri sorunlarını giderme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Aşağıdaki bağlantılar sorun giderme kılavuzunu sağlar:

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC Konferansı (Londra, 2018): ASP.NET Core uygulamalardaki sorunları tanılama](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET blogu: ASP.NET Core performans sorunlarını giderme](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET Core SDK uyarılar

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>.NET Core SDK hem 32-bit hem de 64-bit sürümleri yüklenir

**Yeni proje** iletişim kutusunda ASP.NET Core için aşağıdaki uyarıyı görebilirsiniz:

> .NET Core SDK hem 32-bit hem de 64-bit sürümleri yüklenir. Yalnızca\\' C: Program Files\\DotNet\\SDK\\' konumunda yüklü olan 64 bitlik sürümlerden alınan şablonlar görüntülenir.

Bu uyarı, [.NET Core SDK](https://www.microsoft.com/net/download/all) hem 32-bit (x86) hem de 64-bit (x64) sürümleri yüklendiğinde görüntülenir. Her iki sürümün de sık yüklenebileceği yaygın nedenler şunlardır:

* İlk olarak 32 bitlik bir makine kullanarak .NET Core SDK yükleyicisini indirdiniz, ancak bu dosyayı bir 64 bit makineye kopyaladınız ve bu makinede yüklediniz.
* 32 bit .NET Core SDK başka bir uygulama tarafından yüklendi.
* Yanlış sürüm indirildi ve yüklendi.

Bu uyarıyı engellemek için 32 bit .NET Core SDK kaldırın. **Denetim Masası** > **Programlar ve Özellikler** > 'den Kaldır**bir programı kaldırma veya değiştirme**. Uyarının neden oluştuğunu ve etkilerini anladıysanız, uyarıyı yoksayabilirsiniz.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK birden çok konuma yüklendi

**Yeni proje** iletişim kutusunda ASP.NET Core için aşağıdaki uyarıyı görebilirsiniz:

> .NET Core SDK birden çok konuma yüklenir. Yalnızca\\' C: Program Files\\DotNet\\SDK\\' konumunda yüklü SDK 'lardan şablonlar görüntülenir.

*C:\\Program Files\\DotNet\\SDKdışındabirdizindeenazbir.NETCoreSDKyüklemenizolduğundabuiletiyigörürsünüz.\\* Genellikle bu, .NET Core SDK MSI yükleyicisi yerine Kopyala/Yapıştır kullanılarak bir makineye dağıtıldığında meydana gelir.

Bu uyarıyı engellemek için tüm 32-bit .NET Core SDK 'larını ve çalışma zamanlarını kaldırın. **Denetim Masası** > **Programlar ve Özellikler** > 'den Kaldır**bir programı kaldırma veya değiştirme**. Uyarının neden oluştuğunu ve etkilerini anladıysanız, uyarıyı yoksayabilirsiniz.

### <a name="no-net-core-sdks-were-detected"></a>.NET Core SDK 'Ları algılanmadı

* ASP.NET Core için Visual Studio **Yeni proje** iletişim kutusunda aşağıdaki uyarıyı görebilirsiniz:

  > .NET Core SDK 'Ları algılanmadı, ortam değişkenine `PATH`dahil olduklarından emin olun.

* Bir `dotnet` komut yürütürken, uyarı şöyle görünür:

  > Yüklü olan DotNet SDK 'Ları bulmak mümkün değildi.

Bu uyarılar, ortam değişkeni `PATH` makinede herhangi bir .NET Core SDK 'sı üzerine işaret etmez görüntülenir. Bu sorunu çözmek için:

* .NET Core SDK 'i yükler. [.Net Indirmelerinde](https://dotnet.microsoft.com/download)en son yükleyiciyi edinin.
* Ortam değişkeninin SDK 'nın yüklü olduğu konuma işaret ettiğini doğrulayın (`C:\Program Files\dotnet\` 64 bit/x64 veya `C:\Program Files (x86)\dotnet\` 32 bit/x86 için). `PATH` SDK yükleyicisi normalde ' i ayarlar `PATH`. Aynı bit genişliği SDK 'larını ve çalışma zamanlarını aynı makineye her zaman yükler.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>.NET Core barındırma paketi yüklendikten sonra SDK eksik

.NET Core [barındırma](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) paketinin yüklenmesi, `PATH` .NET Core 'un 32-bit (x86) sürümünü (`C:\Program Files (x86)\dotnet\`) işaret etmek üzere .NET Core çalışma zamanını yüklerken değiştirir. Bu, 32-bit (x86) .NET Core `dotnet` komutu kullanıldığında ([.NET Core SDK 'ları algılanmadığında](#no-net-core-sdks-were-detected)) eksik SDK 'lara yol açabilir. Bu sorunu çözmek için, öncesinde `C:\Program Files\dotnet\` `C:\Program Files (x86)\dotnet\` `PATH`bir konuma geçin.

## <a name="obtain-data-from-an-app"></a>Bir uygulamadan veri alın

Bir uygulama isteklere yanıt veriyorsa, ara yazılım kullanarak uygulamadan aşağıdaki verileri alabilirsiniz:

* İstek &ndash; yöntemi, düzen, ana bilgisayar, pathbase, yol, sorgu dizesi, üst bilgiler
* Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası
* Kimlik &ndash; adı, görünen ad
* Yapılandırma ayarları
* Ortam değişkenleri

Aşağıdaki [Ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) kodunu `Startup.Configure` metodun istek işleme işlem hattının başına yerleştirin. Bu ortam, kodun yalnızca geliştirme ortamında yürütülmesini sağlamak için, ara yazılım çalıştırılmadan önce denetlenir.

Ortamı edinmek için aşağıdaki yaklaşımlardan birini kullanın:

* Metodunuyöntemine`Startup.Configure` ekleyin ve yerel değişkenle ortamı kontrol edin. `IHostingEnvironment` Aşağıdaki örnek kodda bu yaklaşım gösterilmektedir.

* Ortamı `Startup` sınıfındaki bir özelliğe atayın. Özelliğini kullanarak ortamı denetleyin (örneğin, `if (Environment.IsDevelopment())`).

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
