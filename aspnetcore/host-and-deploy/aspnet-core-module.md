---
title: ASP.NET çekirdeği modülü yapılandırma başvurusu
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için ASP.NET Core modülü yapılandırmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483753"
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET çekirdeği modülü yapılandırma başvurusu

Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu belge, ASP.NET Core uygulamaları barındırmak için ASP.NET Core Modülü'nü yapılandırma hakkında yönergeler sağlar. Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz: [ASP.NET Core Modülü'ne genel bakış](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Web.config ile yapılandırma

ASP.NET çekirdeği modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.

Aşağıdaki *web.config* dosyası için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için ASP.NET Core Modülü'nü yapılandırır:

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

Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

İçin bir uygulama dağıtıldığında [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolu ayarlanmış `\\?\%home%\LogFiles\stdout`. Yolun stdout günlüklerine kaydeder *LogFiles* otomatik olarak konumdur klasörü hizmeti tarafından oluşturuldu.

Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmasındaki ilgili önemli bir not için *web.config* alt uygulamaları dosyalarında.

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

::: moniker range="<= aspnetcore-2.0"
| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya bağımsız değişkenleri **processPath**.</p>| |
| `disableStartUpErrorPage` | doğru veya yanlış.</p><p>TRUE ise, **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durum kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | doğru veya yanlış.</p><p>TRUE ise, belirteç istek başına bir üstbilgi 'MS-ASPNETCORE-WINAUTHTOKEN' % ASPNETCORE_PORT % üzerinde dinleme alt işlem iletilir. Bu belirteç istek başına CloseHandle çağırmak için işlem sorumluluğundadır.</p> | `true` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP isteklerini dinlemeye bir işlem başlatır yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göre değerlendirilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakikada kilitlenmesine izin verilir. Bu sınır aşılırsa dakikanın geri kalan işlem başlatma modül durdurur.</p> | `10` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET çekirdeği Modülü ' % ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core Modülü'nü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika olarak varsayılan olarak belirtilmelidir.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatmak için yürütülebilir dosyası için bekleyeceği saniye cinsinden süre olduğunda *app_offline.htm* dosya algılandı.</p> | `10` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül yürütülebilir dosyanın bağlantı noktasını dinleyen bir işlemi başlatmak için bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Modül yeni bir istek alırsa ve uygulamayı başlatmak gelmedikçe gelen istekler işlemi yeniden başlatmaya devam işlemini yeniden dener **rapidFailsPerMinute** son kez sayısı dakika alınıyor.</p> | `120` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boole öznitelik.</p><p>TRUE ise, **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyasına yeniden yönlendiriliyor **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işleminden **processPath** kaydedilir. Site köküne göre göreli yollardır. İle başlayan herhangi bir yol `.` olan göre sitenin kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Yolu sağlanan herhangi bir klasörde günlük dosyası oluşturmak modülün mevcut olmalıdır. Alt çizgi sınırlayıcıları, zaman damgası, işlem kimliği ve dosya uzantısını kullanarak (*.log*) kesimini son eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018 19:41:32 1934 bir işlem kimlikli adresindeki kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Öznitelik | Açıklama | Varsayılan |
| --------- | ----------- | :-----: |
| `arguments` | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya bağımsız değişkenleri **processPath**.</p>| |
| `disableStartUpErrorPage` | doğru veya yanlış.</p><p>TRUE ise, **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durum kod sayfası yapılandırılan *web.config* önceliklidir.</p> | `false` |
| `forwardWindowsAuthToken` | doğru veya yanlış.</p><p>TRUE ise, belirteç istek başına bir üstbilgi 'MS-ASPNETCORE-WINAUTHTOKEN' % ASPNETCORE_PORT % üzerinde dinleme alt işlem iletilir. Bu belirteç istek başına CloseHandle çağırmak için işlem sorumluluğundadır.</p> | `true` |
| `processPath` | <p>Gerekli dize özniteliği.</p><p>HTTP isteklerini dinlemeye bir işlem başlatır yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa `.`, yolun site köküne göre değerlendirilir.</p> | |
| `rapidFailsPerMinute` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakikada kilitlenmesine izin verilir. Bu sınır aşılırsa dakikanın geri kalan işlem başlatma modül durdurur.</p> | `10` |
| `requestTimeout` | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET çekirdeği Modülü ' % ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt bekleyeceği süreyi belirtir.</p><p>ASP.NET Core 2.1 veya sonraki sürüm ile birlikte gelen ASP.NET Core Modülü'nü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül düzgün biçimde kapatmak için yürütülebilir dosyası için bekleyeceği saniye cinsinden süre olduğunda *app_offline.htm* dosya algılandı.</p> | `10` |
| `startupTimeLimit` | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül yürütülebilir dosyanın bağlantı noktasını dinleyen bir işlemi başlatmak için bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırır. Modül yeni bir istek alırsa ve uygulamayı başlatmak gelmedikçe gelen istekler işlemi yeniden başlatmaya devam işlemini yeniden dener **rapidFailsPerMinute** son kez sayısı dakika alınıyor.</p> | `120` |
| `stdoutLogEnabled` | <p>İsteğe bağlı Boole öznitelik.</p><p>TRUE ise, **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyasına yeniden yönlendiriliyor **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işleminden **processPath** kaydedilir. Site köküne göre göreli yollardır. İle başlayan herhangi bir yol `.` olan göre sitenin kök ve diğer tüm yolları mutlak yollar olarak kabul edilir. Yolu sağlanan herhangi bir klasörde günlük dosyası oluşturmak modülün mevcut olmalıdır. Alt çizgi sınırlayıcıları, zaman damgası, işlem kimliği ve dosya uzantısını kullanarak (*.log*) kesimini son eklenen **stdoutLogFile** yolu. Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018 19:41:32 1934 bir işlem kimlikli adresindeki kaydedildiğinde klasör.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği. Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi olan bir `environmentVariables` koleksiyon öğesi. Bu bölümde ayarlamak ortam değişkenleri ortam değişkenleri sistem önceliklidir.

Aşağıdaki örnekte, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT` uygulamanın ortamına yapılandırır `Development`. Bir geliştirici geçici olarak bu değer kümesinde *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) bir uygulama özel durum hata ayıklama sırasında yüklenecek. `CONFIG_DIR` bir kullanıcı tanımlı ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kodu yazıldığı örneğidir.

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
> Yalnızca kümesi `ASPNETCORE_ENVIRONMENT` envirnonment değişkenine `Development` hazırlama ve Internet gibi güvenilmeyen ağlara erişilemez sunucuları test etme.

## <a name="appofflinehtm"></a>app_offline.htm

Bir dosya varsa adıyla *app_offline.htm* algılandığında, bir uygulamanın kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün biçimde kapatılması çalışır. Uygulama tanımlı saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.

Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriğini göndererek yanıt *app_offline.htm* dosya. Zaman *app_offline.htm* dosya kaldırılır, uygulamayı bir sonraki istekte başlatır.

## <a name="start-up-error-page"></a>Başlatma hata sayfası

Arka uç işlem veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek şekilde başarısız başlatmak ASP.NET Core modülü başarısız olursa bir *502.5 işlem hatası* durum kod sayfası görünür. Bu sayfayı bastırmak ve varsayılan IIS 502 durum kod sayfasına dönmek için kullanmak `disableStartUpErrorPage` özniteliği. Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz: [HTTP hataları `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 işlem hatası durumu kod sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yeniden yönlendirme

ASP.NET çekirdeği modülü diske stdout ve stderr günlüklerini yönlendiren `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır. Herhangi bir klasörde `stdoutLogFile` yolu günlük dosyası oluşturmak modülün var olmalıdır. Uygulama havuzu günlükleri yazıldığı konuma yazma erişimi olması gerekir (kullanmak `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).

İşlem geri dönüştürme/yeniden başlatma oluşmadığı sürece günlükleri Döndürülmüş değil. Günlükleri tüketen disk alanını sınırla barındırma sorumluluğundadır.

Yalnızca'yi stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için önerilir. Stdout günlük genel uygulama günlüğü amaçlar için kullanmayın. Bir ASP.NET Core uygulamada rutin günlüğü için günlük dosyası boyutu sınırlar ve günlükleri döndüğü bir günlük kitaplığını kullanın. Daha fazla bilgi için bkz: [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).

Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir. Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yol (genellikle *stdout*) alt çizgi tarafından ayrılmış. Varsa `stdoutLogFile` yolu bitiyor ile *stdout*, 2/5/2018 19:42: 32'de oluşturulan 1934 PID ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.

Aşağıdaki örnek `aspNetCore` öğesi stdout günlüğü için Azure App Service içinde barındırılan bir uygulamayı yapılandırır. Bir yerel yol veya ağ paylaşım yolu yerel günlüğe kaydetme için kabul edilebilir değil. Uygulama havuzu kullanıcı kimliği sağlanan yolun yazma izni olduğunu doğrulayın.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Bkz: [web.config yapılandırmayla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolünü ve bir eşleşme belirteci proxy yapılandırması kullanır

ASP.NET çekirdeği modülü arasında Kestrel oluşturulan proxy HTTP protokolünü kullanır. HTTP performansı iyileştirme modülü ve Kestrel arasındaki trafiğin geri döngü adresine dışına ağ arabirimi gerçekleştiği kullanmaktır. Modül ve sunucunun dışına bir konumdan Kestrel arasındaki trafiğin gizli dinleme hiçbir riski vardır.

Bir eşleşme belirteci Kestrel tarafından alınan isteği IIS tarafından yönlendirilirken ve bazı başka bir kaynaktan geliyor kaydetmedi güvence altına almak için kullanılır. Eşleştirme belirteci oluşturulur ve bir ortam değişkeni ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından. Eşleştirme belirteci de bir üstbilgisine ayarlayın (`MSAspNetCoreToken`) yönlendirilirken her istekte. IIS Ara denetimleri eşleştirme belirteci üstbilgi değeri ortam değişkeni değeri ile eşleşen onaylamak için aldığı isteyin. Belirteç değerleri eşleşirse, isteği günlüğe ve reddetti. Eşleştirme belirteci ortam değişkenine ve modül ve Kestrel arasındaki trafiğin sunucunun dışına bir konumdan erişilebilir değil. Eşleştirme belirteç değeri bilmeden bir saldırgan IIS Ara yazılımında denetimini atla istek gönderemez.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET çekirdeği modülü ile bir IIS paylaşılan yapılandırma

ASP.NET çekirdeği Modülü Yükleyicisi ayrıcalıklarıyla çalıştırır **sistem** hesabı. IIS paylaşılan yapılandırması tarafından kullanılan paylaşım yolu için izin sahip yerel sistem hesabı olmayan değiştirmek için bir erişim reddedildi hatası modülü ayarlarını yapılandırmak çalışırken yükleyici isabetler *applicationHost.config* paylaşımda. IIS paylaşılan yapılandırması kullanırken, aşağıdaki adımları izleyin:

1. IIS paylaşılan yapılandırması devre dışı bırakın.
1. Yükleyiciyi çalıştırın.
1. Güncelleştirilmiş verme *applicationHost.config* dosya paylaşımına.
1. IIS paylaşılan yapılandırma yeniden etkinleştirin.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Modül sürümü ve barındırma Paket Yükleyici günlükleri

Yüklü ASP.NET Core modülünün sürümünü belirlemek için:

1. Barındıran sistemde gidin *%windir%\System32\inetsrv*.
1. Bulun *aspnetcore.dll* dosya.
1. Dosyaya sağ tıklayın ve seçin **özellikleri** bağlam menüsünde.
1. Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.

Modül için barındırma Paket Yükleyici günlüklerini konumunda bulunan *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosya adında *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırmasını dosya konumları

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

Dosyalar için arama yaparak bulunabilir *aspnetcore.dll* içinde *applicationHost.config* dosya. IIS Express, *applicationHost.config* dosyası varsayılan olarak mevcut olmaz. Dosyanın oluşturulma  *\<application_root >\\.vs\\config* Visual Studio çözümünde hiçbir web uygulaması projesi başlatırken.
