---
title: ASP.NET Core Module yapılandırma başvurusu
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0d167f779f9dcae6b0d946dce5e341793daf43bf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091021"
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core Module yapılandırma başvurusu

Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu belge, ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırma hakkında yönergeler sağlar. Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz [ASP.NET Core modülü genel bakış](xref:fundamentals/servers/aspnet-core-module).

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a>Barındırma modeli

.NET Core 2.2 veya sonraki sürümlerde çalışan uygulamalarda, modül ters proxy (giden işlem) barındırmak için karşılaştırıldığında geliştirilmiş performans için bir işlem içi barındırma modelini destekler. Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.

Procsess barındırma katılımı olan mevcut uygulamalar için ancak [yeni dotnet](/dotnet/core/tools/dotnet-new) işlemdeki tüm IIS ve IIS Express senaryoları için barındırma modelini varsayılan şablonları.

İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreModuleHostingModel>` özellik değerini içeren uygulamanın proje dosyasına `inprocess` (işlem dışı barındırma ile ayarlanır `outofprocess`):

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:

* [Kestrel sunucu](xref:fundamentals/servers/kestrel) kullanılmaz. Özel bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> uygulaması `IISHttpServer` uygulamanın sunucusu işlevi görür.

* [RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.

* Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor. Uygulama başına bir uygulama havuzu kullanın.

* Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

* Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.

* El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`. Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.

* İstemci bağlantısını keser algılanır. [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.

### <a name="hosting-model-changes"></a>Barındırma modeli değişiklikleri

Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.

Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler. Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.

### <a name="process-name"></a>İşlem adı

`Process.GetCurrentProcess().ProcessName` raporları `w3wp` (işlem içi) veya `dotnet` (giden işlem).

::: moniker-end

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.

Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`. Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.

Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmanız için ilgili önemli bir not için *web.config* alt uygulamalarında dosyaları.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

::: moniker range=">= aspnetcore-2.2"

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `hostingModel` | <p>İsteğe bağlı dize özniteliği.</p><p>Barındırma modeli işlemdeki belirtir (`inprocess`) veya işlem dışı (`outofprocess`).</p> | `outofprocess` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</p><p>&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</p> | Varsayılan: `1`<br>En küçük: `1`<br>En fazla: `100`&dagger; |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p><p>İşlem içi barındırma ile desteklenmez.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p><p>İşlem içi barındırmak için geçerli değildir. İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</p> | Varsayılan: `00:02:00`<br>En küçük: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</p><p>0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</p> | Varsayılan: `120`<br>En küçük: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir. Site köküne göreli yollardır. İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır. Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</p> | Varsayılan: `1`<br>En küçük: `1`<br>En fazla: `100` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p> | Varsayılan: `00:02:00`<br>En küçük: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</p><p>0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</p> | Varsayılan: `120`<br>En küçük: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir. Site köküne göreli yollardır. İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır. Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</p> | Varsayılan: `1`<br>En küçük: `1`<br>En fazla: `100` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</p> | Varsayılan: `00:02:00`<br>En küçük: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</p> | Varsayılan: `120`<br>En küçük: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir. Site köküne göreli yollardır. İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır. Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği. Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi bir `environmentVariables` koleksiyon öğesi. Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.

Aşağıdaki örnek, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`. Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek. `CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.

## <a name="appofflinehtm"></a>app_offline.htm

Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır. Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.

Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya. Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.

::: moniker range=">= aspnetcore-2.2"

Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

::: moniker-end

## <a name="start-up-error-page"></a>Başlangıç hata sayfası

::: moniker range=">= aspnetcore-2.2"

*Yalnızca işlem dışı barındırmak için geçerlidir.*

::: moniker-end

Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 işlem hatası* durumu kod sayfası görünür. Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği. Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yönlendirme

ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır. Herhangi bir klasörde `stdoutLogFile` yolu, modül günlük dosyası oluşturmak için sırayla bulunmalıdır. Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).

Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece. Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.

Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir. Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın. ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış. Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.

Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır. Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir. Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a>Gelişmiş tanılama günlükleri

ASP.NET Core modülü sağlayan gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir. Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.

Düzeyleri (en az sırası için en ayrıntılı):

* HATA
* UYARI
* BİLGİLERİ
* TRACE

(Birden fazla konumda izin verilir) konumları:

* KONSOLU
* OLAY GÜNLÜĞÜ
* DOSYA

İşleyici ayarları ortam değişkenlerini de sağlanabilir:

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu. (Varsayılan: *aspnetcore debug.log*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.

> [!WARNING]
> Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın. Günlüğünün boyutu sınırlı değildir. Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.

::: moniker-end

Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.

::: moniker range=">= aspnetcore-2.2"

*Yalnızca işlem dışı barındırmak için geçerlidir.*

::: moniker-end

Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır. HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır. Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.

Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır. Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından. Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MSAspNetCoreToken`) proxy kullanan her istekte. IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek. Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi. Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil. Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma

ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **sistem** hesabı. IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken İsabetleri *applicationHost.config* paylaşımda. IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Modül sürümü ve paket barındırma yükleyici günlükleri

Yüklü ASP.NET Core modülü sürümünü belirlemek için:

1. Barındıran sistemde gidin *%windir%\System32\inetsrv*.
1. Bulun *aspnetcore.dll* dosya.
1. Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.
1. Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.

Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırma dosyası konumları

### <a name="module"></a>Modül

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

### <a name="schema"></a>Şema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Yapılandırma

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Dosyalar için arama yaparak bulunabilir *aspnetcore.dll* içinde *applicationHost.config* dosya. IIS Express için *applicationHost.config* dosya varsayılan olarak mevcut olmaz. Dosyanın oluşturulma  *\<application_root >\\.vs\\config* Visual Studio çözümü içinde herhangi bir web uygulaması projesine başlatırken.
