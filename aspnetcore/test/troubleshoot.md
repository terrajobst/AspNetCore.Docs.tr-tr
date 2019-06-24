---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: test/troubleshoot
ms.openlocfilehash: bcec8a55a5111e1f3acf53ae2f57b45e6e609d25
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313681"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core projelerinde sorun giderme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC konferansı (Londra, 2018): ASP.NET Core uygulamalarında sorunları tanılama](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK'sı uyarıları

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>.NET Core SDK'yı 32-bit ve 64-bit sürümlerinin yüklü

İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK'sı hem 32-bit hem de 64 bit sürümlerinde yüklenir. Yalnızca yüklü 64-bit sürümleri şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.

Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir. Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:

* İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.
* 32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.
* Yanlış sürümü indirildi ve yüklendi.

Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK'sını birden çok konumda yüklenir

İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK'sı, birden çok konumda yüklenir. Yalnızca yüklü SDK'ları şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.

.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\* . Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.

Tüm 32-bit .NET Core SDK'ları ve çalışma zamanları bu uyarıyı engellemek için kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.

### <a name="no-net-core-sdks-were-detected"></a>.NET çekirdek SDK algılanmadı

* Visual Studio **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

  > .NET çekirdek SDK algılanmadı, ortam değişkenine dahil oldukları olun `PATH`.

* Yürütülürken bir `dotnet` komutu olarak bir uyarı görüntülenir:

  > Herhangi bir yüklü dotnet SDK'ları bulmak mümkün değildi.

Bu uyarılar görünür ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor. Bu sorunu çözmek için:

* .NET Core SDK'sını yükleyin. En son Yükleyicisi'nden elde [.NET indirir](https://dotnet.microsoft.com/download).
* Doğrulayın `PATH` ortam değişkeni, SDK'sı yüklü olduğu konuma işaret (`C:\Program Files\dotnet\` için 64-bit/x64 veya `C:\Program Files (x86)\dotnet\` 32-bit/x86 için). SDK'sı Yükleyicisi normalde ayarlar `PATH`. Her zaman aynı bit genişliği SDK ve çalışma zamanları, aynı makineye yükleyin.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>.NET Core barındırma paketini yükledikten sonra SDK'sı eksik

Yükleme [.NET Core barındırma paket](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) değiştirir `PATH` .NET core'un 32-bit (x 86) sürümüne işaret edecek şekilde .NET Core çalışma zamanı yüklendiğinde (`C:\Program Files (x86)\dotnet\`). Bu SDK'ları eksik neden olduğunda 32-bit (x 86) .NET Core `dotnet` komutu kullanılır ([.NET Core SDK algılandı](#no-net-core-sdks-were-detected)). Bu sorunu çözmek için taşıma `C:\Program Files\dotnet\` önce bir konuma `C:\Program Files (x86)\dotnet\` üzerinde `PATH`.

## <a name="obtain-data-from-an-app"></a>Bir uygulamadan veri alın

Bir uygulama isteklerini yanıtlayabileceği ise, ara yazılımın kullanılması uygulamayı aşağıdaki veriler elde edebilirsiniz:

* İstek &ndash; yöntemi, şema, konak, pathbase, yol, sorgu dizesi, üst bilgileri
* Bağlantı &ndash; uzak IP adresi, uzak bağlantı noktası, yerel IP adresi, yerel bağlantı noktası, istemci sertifikası
* Kimlik &ndash; adı, görünen adı
* Yapılandırma ayarları
* Ortam değişkenleri

Aşağıdaki yerleştirin [ara yazılım](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) başındaki kod `Startup.Configure` yöntemin istek işleme ardışık düzeni. Kod geliştirme ortamında yalnızca yürütülür emin olmak için bir ara yazılım çalıştırılmadan önce ortamı denetlenir.

Ortamı elde etmek için aşağıdaki yaklaşımlardan birini kullanın:

* Ekleme `IHostingEnvironment` içine `Startup.Configure` yöntemi ve onay yerel değişken ortamı. Aşağıdaki örnek kod, bu yaklaşım gösterilmektedir.

* Ortamı bir özelliğine atayın `Startup` sınıfı. Özelliğini kullanarak ortam denetleyin (örneğin, `if (Environment.IsDevelopment())`).

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
