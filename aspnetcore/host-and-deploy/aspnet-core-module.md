---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4a360023cc7fab2f066d490f7f368fc35815703a
ms.sourcegitcommit: 4b00e77f9984ce76356e829cfe7f75f0f61a7a8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "67500455"
---
# <a name="aspnet-core-module"></a>ASP.NET Core Modülü

, [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris](https://github.com/Tratcher)No, [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [adatın kotalik](https://github.com/jkotalik)ve [Luke Latham](https://github.com/guardrex) tarafından

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:

* `w3wp.exe` [İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin () içinde bir ASP.NET Core uygulaması barındırın.
* Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

İşlem içinde barındırırken, modül IIS HTTP sunucusu (`IISHttpServer`) olarak adlandırılan IIS için işlem içi sunucu uygulamasını kullanır.

İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir. Modül, [http. sys](xref:fundamentals/servers/httpsys)ile uyumsuzdur.

## <a name="hosting-models"></a>Barındırma modelleri

### <a name="in-process-hosting-model"></a>İşlem içi barındırma modeli

Uygulamayı işlem içi barındırma için yapılandırmak için, `<AspNetCoreHostingModel>` özelliği uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma ile `OutOfProcess`ayarlanır) ekleyin:

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.

Özellik dosyada yoksa, varsayılan değer olur `OutOfProcess`. `<AspNetCoreHostingModel>`

Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:

* Kestrel Server yerine IIS`IISHttpServer`http sunucusu () kullanılır [](xref:fundamentals/servers/kestrel) . İşlem içi için [createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) aşağıdakileri öğesine çağırır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> :

  * `IISHttpServer`Kaydolun.
  * ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
  * Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

* [RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.

* Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor. Uygulama başına bir uygulama havuzu kullanın.

* Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

* Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.

* El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`. Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.

* İstemci bağlantısını keser algılanır. [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.

* ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *c:\Windows\System32\inetsrv* için, *W3wp. exe*).

  Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Çağrı `SetCurrentDirectory` yöntemi. Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.

* İşlem içi barındırma sırasında, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> bir kullanıcıyı başlatmak için dahili olarak çağrılmaz. Bu nedenle, <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir uygulama varsayılan olarak etkinleştirilmez. Talepleri bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamayla dönüştürürken, kimlik doğrulama hizmetleri <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> Ekle ' yi çağırın:

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a>İşlem dışı barındırma modeli

Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:

* `<AspNetCoreHostingModel>` Özelliği belirtmeyin. Özellik dosyada yoksa, varsayılan değer olur `OutOfProcess`. `<AspNetCoreHostingModel>`
* `<AspNetCoreHostingModel>` Özelliğin değerini olarak `OutOfProcess` ayarlayın (işlem içi barındırma ile `InProcess`ayarlanır):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

[](xref:fundamentals/servers/kestrel) IIS HTTP Server (`IISHttpServer`) yerine Kestrel Server kullanılır.

İşlem dışı için [createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) aşağıdakileri öğesine çağırır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> :

* ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
* Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

İşlem dışı barındırma sırasında, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> bir kullanıcıyı başlatmak için dahili olarak çağrılmaz. Bu nedenle, <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir uygulama varsayılan olarak etkinleştirilmez. Talepleri bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamayla dönüştürürken, kimlik doğrulama hizmetleri <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> Ekle ' yi çağırın:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
    services.AddAuthentication(IISDefaults.AuthenticationScheme);
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="hosting-model-changes"></a>Barındırma modeli değişiklikleri

Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.

Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler. Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.

### <a name="process-name"></a>İşlem adı

`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core modülü, Web isteklerini arka uca ASP.NET Core uygulamalarına iletmek için IIS ardışık düzenine takılan yerel bir IIS modülüdür.

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

Modül yalnızca Kestrel ile birlikte kullanılabilir. Modül, [http. sys](xref:fundamentals/servers/httpsys)ile uyumsuzdur.

ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini de işler. Modül, ilk istek ulaştığında ASP.NET Core App işlemini başlatır ve kilitlenirse uygulamayı yeniden başlatır. Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen IIS 'de işlem içinde çalışan ASP.NET 4. x uygulamaları ile görüldüğü aynı davranıştır.

Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve bir uygulama arasındaki ilişki gösterilmektedir:

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır. Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS). Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.

Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu dinleyecek `http://localhost:{port}`şekilde yapılandırır. Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir. Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.

Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örnek olarak geçirir. IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir. Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.

::: moniker-end

Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz <xref:host-and-deploy/iis/modules>.

ASP.NET Core modülü de şunları yapabilir:

* Çalışan işlem için ortam değişkenlerini ayarlayın.
* Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.
* Windows kimlik doğrulama belirteçlerini ilet.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core modülünü yüklemek ve kullanmak

ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.

Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
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
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.

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

IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

::: moniker range=">= aspnetcore-2.2"

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `hostingModel` | <p>İsteğe bağlı dize özniteliği.</p><p>Barındırma modeli işlemdeki belirtir (`InProcess`) veya işlem dışı (`OutOfProcess`).</p> | `OutOfProcess` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</p><p>&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</p><p>Ayar `processesPerApplication` önerilmez. Bu öznitelik gelecek bir sürümde kaldırılacak.</p> | Varsayılan: `1`<br>En küçük: `1`<br>En fazla: `100`&dagger; |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p><p>İşlem içi barındırma ile desteklenmez.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p><p>İşlem içi barındırmak için geçerli değildir. İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</p><p>Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır. Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</p> | Varsayılan: `00:02:00`<br>En küçük: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</p><p>0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</p> | Varsayılan: `120`<br>En küçük: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir. Site köküne göreli yollardır. İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</p><p>Ayar `processesPerApplication` önerilmez. Bu öznitelik gelecek bir sürümde kaldırılacak.</p> | Varsayılan: `1`<br>En küçük: `1`<br>En fazla: `100` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p> | Varsayılan: `00:02:00`<br>En küçük: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</p> | Varsayılan: `10`<br>En küçük: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</p><p>0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</p> | Varsayılan: `120`<br>En küçük: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir. Site köküne göreli yollardır. İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır. Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

::: moniker range=">= aspnetcore-3.0"

Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği. Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi. Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği. Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.

> [!WARNING]
> Bu bölümde ayarlanan ortam değişkenleri, aynı ada sahip sistem ortam değişkenleri ile çakışıyor. Bir ortam değişkeni hem *Web. config* dosyasında hem de Windows 'un sistem düzeyinde ayarlandıysa, *Web. config* dosyasındaki değer sistem ortam değişkeni değerine (örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`) eklenerek, uygulamayı engelleyen başlangıç.

::: moniker-end

Aşağıdaki örnek, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`. Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek. `CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği Publish profile ( *. pubxml*) veya proje dosyasına dahil etmek de vardır. Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.

## <a name="app_offlinehtm"></a>app_offline.htm

Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır. Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.

Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya. Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.

::: moniker range=">= aspnetcore-2.2"

Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

::: moniker-end

## <a name="start-up-error-page"></a>Başlangıç hata sayfası

::: moniker range=">= aspnetcore-2.2"

İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.

Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.

Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.

Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.

Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği. Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür. Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği. Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yönlendirme

ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır. `stdoutLogFile` Yoldaki tüm klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).

Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece. Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.

Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir. Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın. ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur ( *.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış. Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.

::: moniker range=">= aspnetcore-2.2"

Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar. Başlatma işleminden sonra tüm ek günlükler atılır.

::: moniker-end

Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır. Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir. Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
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

ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir. Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Yoldaki tüm klasörler (önceki örnekteki*Günlükler* ), günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`<handlerSetting>` Değer için belirtilen yoldaki klasörler (önceki örnekteki*Günlükler* ) modül tarafından otomatik olarak oluşturulmaz ve dağıtımda önceden var olmalıdır. Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

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

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a>Yığın boyutunu değiştirme

Yönetilen yığın boyutunu bayt cinsinden `stackSize` ayarı kullanarak yapılandırın. Varsayılan boyut bayt 'tır `1048576` (1 MB).

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.

::: moniker range=">= aspnetcore-2.2"

*Yalnızca işlem dışı barındırmak için geçerlidir.*

::: moniker-end

Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır. HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır. Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.

Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır. Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından. Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte. IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek. Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi. Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil. Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma

ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır. Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.

::: moniker range=">= aspnetcore-2.2"

IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, şu şekilde `OPT_NO_SHARED_CONFIG_CHECK` `1`ayarlanan parametre ile birlikte ASP.NET Core barındırma paketi yükleyicisini çalıştırın:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

::: moniker-end

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

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

::: moniker-end

**IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

::: moniker-end

### <a name="schema"></a>Şema

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

::: moniker range=">= aspnetcore-2.2"

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

::: moniker-end

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml

::: moniker-end

### <a name="configuration"></a>Yapılandırma

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {APPLICATION root}\\. vs\config\applicationhost,config

* *iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/iis/index>
* [ASP.NET Core Module GitHub deposu (başvuru kaynağı)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
