---
title: ASP.NET Core Modülü
author: rick-anderson
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 298d424557600735668217e1ef07ace606dac60b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667302"
---
# <a name="aspnet-core-module"></a>ASP.NET Core Modülü

, [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris](https://github.com/Tratcher)No, [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti)ve ka [kotalık](https://github.com/jkotalik)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:

* [İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.
* Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.

İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir. Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.

## <a name="hosting-models"></a>Barındırma modelleri

### <a name="in-process-hosting-model"></a>İşlem içi barındırma modeli

Uygulamalar, işlem içi barındırma modelinde varsayılan olarak ASP.NET Core.

Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:

* [Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır. İşlem içi için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> çağırır:

  * `IISHttpServer`kaydedin.
  * ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
  * Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

* [RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.

* Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor. Uygulama başına bir uygulama havuzu kullanın.

* [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

* Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.

* İstemci bağlantısını keser algılanır. İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.

* ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).

  Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs). `SetCurrentDirectory` yöntemini çağırın. <xref:System.IO.Directory.GetCurrentDirectory*> sonraki çağrılar, uygulamanın dizinini sağlar.

* İşlem içi barındırma sırasında, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak çağrılmaz. Bu nedenle, her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulama varsayılan olarak etkinleştirilmez. Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulamayla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:

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
  
  * [Web paketi (tek dosya) dağıtımları](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) desteklenmez.

### <a name="out-of-process-hosting-model"></a>İşlem dışı barındırma modeli

Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, `<AspNetCoreHostingModel>` özelliğinin değerini proje dosyasında ( *. csproj*) `OutOfProcess` olarak ayarlayın:

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

İşlem içi barındırma, varsayılan değer olan `InProcess`olarak ayarlanır.

`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.

[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).

İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> çağırır:

* ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
* Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

### <a name="hosting-model-changes"></a>Barındırma modeli değişiklikleri

`hostingModel` ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.

Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler. Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.

### <a name="process-name"></a>İşlem adı

`Process.GetCurrentProcess().ProcessName` rapor `w3wp`/`iisexpress` (işlem içi) veya `dotnet` (işlem dışı).

Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.

ASP.NET Core modülü de şunları yapabilir:

* Çalışan işlem için ortam değişkenlerini ayarlayın.
* Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.
* Windows kimlik doğrulama belirteçlerini ilet.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core modülünü yüklemek ve kullanmak

ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.

Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<xref:System.Configuration.SectionInformation.InheritInChildApplications*> özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.

Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır. Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.

IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</p> | |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `hostingModel` | <p>İsteğe bağlı dize özniteliği.</p><p>Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</p><p>işlem içi barındırma Için &dagger;, değer `1`sınırlıdır.</p><p>`processesPerApplication` ayarlama önerilmez. Bu öznitelik gelecek bir sürümde kaldırılacak.</p> | Varsayılan: `1`<br>Min: `1`<br>En fazla: `100`&dagger; |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol `.`başlıyorsa, yol site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p><p>İşlem içi barındırma ile desteklenmez.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p><p>İşlem içi barındırmak için geçerli değildir. İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</p><p>Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır. Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</p> | Varsayılan: `00:02:00`<br>Min: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</p><p>0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</p> | Varsayılan: `120`<br>Min: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir. Site köküne göreli yollardır. `.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir. Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir. `.\logs\stdout` bir değer olarak sağlandıysa, bir örnek stdout stdout_20180205194132_1934 günlüğü, bir işlem 1934 KIMLIĞI olan 2/5/2018 ' de 19:41:32 ' de kaydedildiğinde *Günlükler* klasöründe *günlüğe* kaydedilir.</p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir. Bir `<environmentVariables>` Collection öğesinin `<environmentVariable>` Child öğesiyle bir ortam değişkeni belirtin. Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.

Aşağıdaki örnek, *Web. config*dosyasında iki ortam değişkenini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır. Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir. `CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir. Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.

## <a name="app_offlinehtm"></a>app_offline.htm

Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır. Uygulama, `shutdownTimeLimit`belirtilen saniye sayısından sonra çalışmaya devam ediyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.

*App_offline. htm* dosyası mevcut olsa da, ASP.NET Core modülü *app_offline. htm* dosyasının içeriğini geri göndererek isteklere yanıt verir. *App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.

Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

## <a name="start-up-error-page"></a>Başlangıç hata sayfası

İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.

ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.

ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.

ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.

Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın. Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yönlendirme

`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir. `stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).

Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece. Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.

Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.

Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın. ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur. `stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosya adı *stdout_20180205194132_1934. log*' u içerir.

`stdoutLogEnabled` false ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır. Başlatma işleminden sonra tüm ek günlükler atılır.

Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır. Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar. `%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.

Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.

Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).

## <a name="enhanced-diagnostic-logs"></a>Gelişmiş tanılama günlükleri

ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir. `<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

Yoldaki tüm klasörler (önceki örnekteki*Günlükler* ), günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).

Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.

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

* hata ayıklama günlük dosyasının &ndash; yolunu `ASPNETCORE_MODULE_DEBUG_FILE`. (Varsayılan: *aspnetcore-Debug. log*)
* Hata ayıklama düzeyi ayarını &ndash; `ASPNETCORE_MODULE_DEBUG`.

> [!WARNING]
> Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.** Günlüğünün boyutu sınırlı değildir. Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.

*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .

## <a name="modify-the-stack-size"></a>Yığın boyutunu değiştirme

*Yalnızca işlem içi barındırma modeli kullanılırken geçerlidir.*

Yönetilen yığın boyutunu, *Web. config*dosyasındaki bayt cinsinden `stackSize` ayarını kullanarak yapılandırın. Varsayılan boyut `1048576` bayttır (1 MB).

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.

*Yalnızca işlem dışı barındırma için geçerlidir.*

Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır. Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.

Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır. Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır. Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır. IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek. Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi. Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil. Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma

ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır. Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.

IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Modül sürümü ve paket barındırma yükleyici günlükleri

Yüklü ASP.NET Core modülü sürümünü belirlemek için:

1. Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.
1. *Aspnetcore. dll* dosyasını bulun.
1. Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.
1. **Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.

Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırma dosyası konumları

### <a name="module"></a>Modül

**IIS (X86/AMD64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

* %ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

**IIS Express (X86/AMD64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

* %ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Şema

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Yapılandırma

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config

* *ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:

* [İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.
* Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.

İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir. Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.

## <a name="hosting-models"></a>Barındırma modelleri

### <a name="in-process-hosting-model"></a>İşlem içi barındırma modeli

Bir uygulamayı işlem içi barındırma için yapılandırmak için, `<AspNetCoreHostingModel>` özelliğini uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma `OutOfProcess`olarak ayarlanır) olarak ekleyin:

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.

`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.

`<AspNetCoreHostingModel>` özelliği dosyasında yoksa, varsayılan değer `OutOfProcess`.

Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:

* [Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır. İşlem içi için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> çağırır:

  * `IISHttpServer`kaydedin.
  * ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
  * Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

* [RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.

* Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor. Uygulama başına bir uygulama havuzu kullanın.

* [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

* Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.

* İstemci bağlantısını keser algılanır. İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.

* ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).

  Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). `SetCurrentDirectory` yöntemini çağırın. <xref:System.IO.Directory.GetCurrentDirectory*> sonraki çağrılar, uygulamanın dizinini sağlar.

* İşlem içi barındırma sırasında, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak çağrılmaz. Bu nedenle, her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulama varsayılan olarak etkinleştirilmez. Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulamayla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:

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

* `<AspNetCoreHostingModel>` özelliğini belirtmeyin. `<AspNetCoreHostingModel>` özelliği dosyasında yoksa, varsayılan değer `OutOfProcess`.
* `<AspNetCoreHostingModel>` özelliğinin değerini `OutOfProcess` olarak ayarlayın (işlem içi barındırma, `InProcess`ile ayarlanır):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Değer büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.

[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).

İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> çağırır:

* ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.
* Konağı, başlatma hatalarını yakalamak üzere yapılandırın.

### <a name="hosting-model-changes"></a>Barındırma modeli değişiklikleri

`hostingModel` ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.

Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler. Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.

### <a name="process-name"></a>İşlem adı

`Process.GetCurrentProcess().ProcessName` rapor `w3wp`/`iisexpress` (işlem içi) veya `dotnet` (işlem dışı).

Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.

ASP.NET Core modülü de şunları yapabilir:

* Çalışan işlem için ortam değişkenlerini ayarlayın.
* Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.
* Windows kimlik doğrulama belirteçlerini ilet.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core modülünü yüklemek ve kullanmak

ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.

Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<xref:System.Configuration.SectionInformation.InheritInChildApplications*> özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.

Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır. Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.

IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</p> | |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `hostingModel` | <p>İsteğe bağlı dize özniteliği.</p><p>Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</p><p>işlem içi barındırma Için &dagger;, değer `1`sınırlıdır.</p><p>`processesPerApplication` ayarlama önerilmez. Bu öznitelik gelecek bir sürümde kaldırılacak.</p> | Varsayılan: `1`<br>Min: `1`<br>En fazla: `100`&dagger; |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol `.`başlıyorsa, yol site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p><p>İşlem içi barındırma ile desteklenmez.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p><p>İşlem içi barındırmak için geçerli değildir. İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</p><p>Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır. Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</p> | Varsayılan: `00:02:00`<br>Min: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</p><p>0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</p> | Varsayılan: `120`<br>Min: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir. Site köküne göreli yollardır. `.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir. Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir. `.\logs\stdout` bir değer olarak sağlandıysa, bir örnek stdout stdout_20180205194132_1934 günlüğü, bir işlem 1934 KIMLIĞI olan 2/5/2018 ' de 19:41:32 ' de kaydedildiğinde *Günlükler* klasöründe *günlüğe* kaydedilir.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir. Bir `<environmentVariables>` Collection öğesinin `<environmentVariable>` Child öğesiyle bir ortam değişkeni belirtin. Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.

Aşağıdaki örnek, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır. Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir. `CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir. Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.

## <a name="app_offlinehtm"></a>app_offline.htm

Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır. Uygulama, `shutdownTimeLimit`belirtilen saniye sayısından sonra çalışmaya devam ediyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.

*App_offline. htm* dosyası mevcut olsa da, ASP.NET Core modülü *app_offline. htm* dosyasının içeriğini geri göndererek isteklere yanıt verir. *App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.

Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak. Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.

## <a name="start-up-error-page"></a>Başlangıç hata sayfası

İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.

ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.

ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.

ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.

Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın. Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yönlendirme

`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir. `stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).

Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece. Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.

Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.

Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın. ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur. `stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosya adı *stdout_20180205194132_1934. log*' u içerir.

`stdoutLogEnabled` false ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır. Başlatma işleminden sonra tüm ek günlükler atılır.

Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır. Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar. `%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.

Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).

## <a name="enhanced-diagnostic-logs"></a>Gelişmiş tanılama günlükleri

ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir. `<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

`<handlerSetting>` değerine (önceki örnekteki*Günlükler* ) belirtilen yoldaki klasörler, otomatik olarak modül tarafından oluşturulmaz ve dağıtımda önceden var olmalıdır. Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).

Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.

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

* hata ayıklama günlük dosyasının &ndash; yolunu `ASPNETCORE_MODULE_DEBUG_FILE`. (Varsayılan: *aspnetcore-Debug. log*)
* Hata ayıklama düzeyi ayarını &ndash; `ASPNETCORE_MODULE_DEBUG`.

> [!WARNING]
> Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.** Günlüğünün boyutu sınırlı değildir. Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.

*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.

*Yalnızca işlem dışı barındırma için geçerlidir.*

Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır. Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.

Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır. Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır. Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır. IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek. Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi. Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil. Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma

ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır. Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.

IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Modül sürümü ve paket barındırma yükleyici günlükleri

Yüklü ASP.NET Core modülü sürümünü belirlemek için:

1. Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.
1. *Aspnetcore. dll* dosyasını bulun.
1. Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.
1. **Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.

Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırma dosyası konumları

### <a name="module"></a>Modül

**IIS (X86/AMD64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

* %ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

**IIS Express (X86/AMD64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

* %ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Şema

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Yapılandırma

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config

* *ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.

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

Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu `http://localhost:{port}`dinlemek üzere yapılandırır. Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir. Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.

Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir. IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir. Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.

Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.

ASP.NET Core modülü de şunları yapabilir:

* Çalışan işlem için ortam değişkenlerini ayarlayın.
* Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.
* Windows kimlik doğrulama belirteçlerini ilet.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>ASP.NET Core modülünü yüklemek ve kullanmak

ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.

Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:

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

Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:

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

Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır. Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.

IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</p>| |
| `disableStartUpErrorPage` | <p>İsteğe bağlı Boolean özniteliği.</p><p>Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | <p>İsteğe bağlı Boolean özniteliği.</p><p>TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir. İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</p> | `true` |
| `processesPerApplication` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</p><p>`processesPerApplication` ayarlama önerilmez. Bu öznitelik gelecek bir sürümde kaldırılacak.</p> | Varsayılan: `1`<br>Min: `1`<br>En fazla: `100` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol `.`başlıyorsa, yol site köküne göreli olarak kabul edilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir. Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `100` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p> | Varsayılan: `00:02:00`<br>Min: `00:00:00`<br>En fazla: `360:00:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</p> | Varsayılan: `10`<br>Min: `0`<br>En fazla: `600` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</p><p>0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</p> | Varsayılan: `120`<br>Min: `0`<br>En fazla: `3600` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boolean özniteliği.</p><p>True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir. Site köküne göreli yollardır. `.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir. Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır. Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir. `.\logs\stdout` bir değer olarak sağlandıysa, bir örnek stdout stdout_20180205194132_1934 günlüğü, bir işlem 1934 KIMLIĞI olan 2/5/2018 ' de 19:41:32 ' de kaydedildiğinde *Günlükler* klasöründe *günlüğe* kaydedilir.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir. Bir `<environmentVariables>` Collection öğesinin `<environmentVariable>` Child öğesiyle bir ortam değişkeni belirtin.

> [!WARNING]
> Bu bölümde ayarlanan ortam değişkenleri, aynı ada sahip sistem ortam değişkenleri ile çakışıyor. Bir ortam değişkeni hem *Web. config* dosyasında hem de Windows 'un sistem düzeyinde ayarlandıysa, *Web. config* dosyasındaki değer, uygulamanın başlamasını önleyen sistem ortam değişkeni değerine (örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`) eklenir.

Aşağıdaki örnek, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır. Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir. `CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.

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

> [!WARNING]
> Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.

## <a name="app_offlinehtm"></a>app_offline.htm

Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır. Uygulama, `shutdownTimeLimit`belirtilen saniye sayısından sonra çalışmaya devam ediyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.

*App_offline. htm* dosyası mevcut olsa da, ASP.NET Core modülü *app_offline. htm* dosyasının içeriğini geri göndererek isteklere yanıt verir. *App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.

## <a name="start-up-error-page"></a>Başlangıç hata sayfası

ASP.NET Core modülü arka uç işlemini başlatamaz veya arka uç işlemi başlatılır, ancak yapılandırılmış bağlantı noktasında dinleme başarısız olursa, *502,5-Işlem hatası* durum kodu sayfası görüntülenir. Bu sayfayı bastırın ve varsayılan IIS 502 durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın. Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yönlendirme

`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir. `stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur. Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).

Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece. Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.

Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.

Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın. ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur. `stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosya adı *stdout_20180205194132_1934. log*' u içerir.

Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır. Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar. `%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.

Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.

Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.

Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır. Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.

Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır. Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır. Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır. IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek. Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi. Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil. Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma

ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır. Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.

IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.
1. IIS paylaşılan yapılandırması yeniden etkinleştirin.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Modül sürümü ve paket barındırma yükleyici günlükleri

Yüklü ASP.NET Core modülü sürümünü belirlemek için:

1. Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.
1. *Aspnetcore. dll* dosyasını bulun.
1. Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.
1. **Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.

Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırma dosyası konumları

### <a name="module"></a>Modül

**IIS (X86/AMD64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (X86/AMD64):**

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

* Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config

* *ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* [ASP.NET Core modül başvuru kaynağı (ana dal)](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2) &ndash; belirli bir sürümü seçmek için **dal** açılan listesini kullanın (örneğin, `release/3.1`).
* <xref:host-and-deploy/iis/modules>
