---
title: "ASP.NET çekirdeği modülü yapılandırma başvurusu"
author: guardrex
description: "ASP.NET Core uygulamaları barındırmak için ASP.NET Core Modülü'nü yapılandırmak nasıl."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 5addaada33364d044d89359196bd1d316590c517
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET çekirdeği modülü yapılandırma başvurusu

Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu belge, ASP.NET Core uygulamaları barındırmak için ASP.NET Core Modülü'nü yapılandırma hakkında ayrıntılar sağlar. Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz: [ASP.NET Core Modülü'ne genel bakış](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Web.config aracılığıyla yapılandırma

ASP.NET çekirdeği modülü bir site veya uygulama yapılandırılır *web.config* dosya ve kendi `aspNetCore` yapılandırma bölümü içinde `system.webServer`. İşte bir örnek *web.config* dosya `Microsoft.NET.Sdk.Web` SDK projesi için yayımlandığında sağlayacaktır bir [framework bağımlı dağıtım](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) yer tutucuları olan `processPath` ve `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config* aşağıdaki örnek olduğu için bir [müstakil dağıtım](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) için [Azure App Service](https://azure.microsoft.com/services/app-service/). Daha fazla bilgi için bkz: [IIS ile Windows konakta](xref:host-and-deploy/iis/index). Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmasındaki ilgili önemli bir not için *web.config* alt uygulamalarında dosyaları.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore öğenin öznitelikleri

| Öznitelik | Açıklama |
| --- | --- |
| processPath | <p>Gerekli dize özniteliği.</p><p>HTTP isteklerini dinlemeye bir işlem başlatır yürütülebilir dosya yolu. Göreli yollar desteklenir. Yol ile başlıyorsa '.', yol site köküne göre değerlendirilir.</p><p>Varsayılan değer yoktur.</p> |
| bağımsız değişkenler | <p>İsteğe bağlı dize özniteliği.</p><p>Belirtilen yürütülebilir dosya bağımsız değişkenleri **processPath**.</p><p>Varsayılan değer boş bir dizedir.</p> |
| startupTimeLimit | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Modül yürütülebilir dosyanın bağlantı noktasını dinleyen bir işlemi başlatmak için bekleyeceği saniye cinsinden süre. Bu süre aşılırsa, modül işlemi sonlandırın. Modül yeni bir istek alırsa ve uygulamayı başlatmak gelmedikçe sonraki gelen istekleri üzerinde işlemini yeniden başlatmak denemeye devam edecek işlem yeniden başlatmaya çalışacak **rapidFailsPerMinute** numarası Son çalışırken dakika içinde sürelerinin.</p><p>Varsayılan değer 120'dir.</p> |
| shutdownTimeLimit | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Kendisi için modül bekleyecek dikkatlice kapatmak için yürütülebilir dosyası için saniye cinsinden süre olduğunda *app_offline.htm* dosya algılandı.</p><p>Varsayılan değer 10'dur.</p> |
| rapidFailsPerMinute | <p>İsteğe bağlı tamsayı özniteliği.</p><p>Belirtilen işlem sayısını belirtir **processPath** dakikada kilitlenmesine izin verilir. Bu sınır aşılırsa, modül dakikanın geri kalan işlemi başlatmayı durdurur.</p><p>Varsayılan değer 10'dur.</p> |
| requestTimeout | <p>İsteğe bağlı timespan özniteliği.</p><p>ASP.NET çekirdeği Modülü'için % ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt bekleyeceği süreyi belirtir.</p><p>Varsayılan değer "00: 02:00".</p><p>`requestTimeout` Yalnızca tam dakikalar içinde aksi 2 dakika olarak varsayılan olarak belirtilmelidir.</p> |
| stdoutLogEnabled | <p>İsteğe bağlı Boole öznitelik.</p><p>TRUE ise, **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyasının yönlendirilen **stdoutLogFile**.</p><p>Varsayılan değer false'tur.</p> |
| stdoutLogFile | <p>İsteğe bağlı dize özniteliği.</p><p>Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işleminden **processPath** günlüğe kaydedilir. Site köküne göre göreli yollardır. İle başlayan herhangi bir yol '.' da site köküne göre olacaktır ve diğer tüm yolları mutlak yollar olarak kabul edilir. Yolu sağlanan herhangi bir klasörde günlük dosyası oluşturmak modülün mevcut olmalıdır. İşlem kimliği, zaman damgası (*yyyyMdhms*) ve dosya uzantısı (*.log*) çizgiyle sınırlayıcıları son segmenti eklenen **stdoutLogFile** sağlanan.</p><p>Varsayılan değer `aspnetcore-stdout` şeklindedir.</p> |
| forwardWindowsAuthToken | doğru veya yanlış.</p><p>TRUE ise, belirteç istek başına bir üstbilgi 'MS-ASPNETCORE-WINAUTHTOKEN' % ASPNETCORE_PORT % üzerinde dinleme alt işlem iletilir. Bu belirteç istek başına CloseHandle çağırmak için işlem sorumluluğundadır.</p><p>Varsayılan değer true olur.</p> |
| disableStartUpErrorPage | doğru veya yanlış.</p><p>TRUE ise, **502.5 - işlem hatası** sayfa bastırılacak ve 502 durum kod sayfası yapılandırılan, *web.config* öncelikli olur.</p><p>Varsayılan değer false'tur.</p> |

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Belirtilen işlem için ortam değişkenleri belirttiğiniz ASP.NET Core modülü sağlar `processPath` bir veya daha fazla belirterek özniteliği `environmentVariable` alt öğelerinin bir `environmentVariables` koleksiyon öğesi altında `aspNetCore` öğesi. Ortam değişkenleri Bu bölümde ayarlama işlemi için ortam değişkenleri sistem önceliklidir.

Aşağıdaki örnekte, iki ortam değişkenlerini ayarlar. `ASPNETCORE_ENVIRONMENT` uygulamanın ortamına yapılandıracak `Development`. Bir geliştirici geçici olarak bu değer kümesinde *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) bir uygulama özel durum hata ayıklama sırasında yüklenecek. `CONFIG_DIR` bir kullanıcı tanımlı ortam değişkeni Geliştirici uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak için başlangıç değerini okuma yapacak kodu yazıldığı örneğidir.

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

## <a name="appofflinehtm"></a>app_offline.htm

Ada sahip bir dosya yerleştirirseniz *app_offline.htm* web uygulama dizini kökünde ASP.NET çekirdek modülü düzgün biçimde kapatılması uygulamayı deneyin ve gelen istekleri işlemeyi durdur. Uygulama hala çalıştırdıktan sonra `shutdownTimeLimit` numarası saniye olarak, ASP.NET Core modülü çalışan işlemi sonlandırılamadı.

Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü istekleri içeriğini göndererek yanıtlar *app_offline.htm* dosya. Bir kez *app_offline.htm* dosya kaldırılır, bir sonraki istekte isteklerine yanıt vermediğini uygulamayı yükler.

## <a name="start-up-error-page"></a>Başlatma hata sayfası

Arka uç işlem veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek şekilde başarısız başlatmak ASP.NET Core modülü başarısız olursa, bir HTTP 502.5 durum kod sayfasını görürsünüz. Bu sayfayı bastırmak ve varsayılan IIS 502 durum kod sayfasına dönmek için kullanmak `disableStartUpErrorPage` özniteliği. Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz: [HTTP hataları `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![502 durumu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Günlük oluşturma ve yeniden yönlendirme

ASP.NET çekirdeği modülü yönlendiren `stdout` ve `stderr` ayarlarsanız diske günlükleri `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi. Herhangi bir klasörde `stdoutLogFile` yolu günlük dosyası oluşturmak modülün var olmalıdır. Günlük dosyasını oluştururken bir zaman damgası ve dosya uzantısı otomatik olarak eklenir. İşlem geri dönüştürme/yeniden başlatma oluşmadığı sürece günlükleri, döndürülemez. Günlükleri tüketen disk alanını sınırla barındırma sorumluluğundadır. Kullanarak `stdout` günlük uygulama başlatma sorunlarını giderme ve genel uygulama günlüğü amaçları için değil yalnızca önerilir.

Günlük dosyası adı işlem Kimliğini (PID), zaman damgası ekleyerek oluşur (*yyyyMdhms*) ve dosya uzantısı (*.log*) son segmenti için `stdoutLogFile` yol (genellikle *stdout* ) alt çizgi tarafından ayrılmış. Örneğin, `stdoutLogFile` yolu bitiyor ile *stdout*, 8/10/2017 12:05:02 sırasında oluşturulan 10652 PID ile bir uygulama için bir günlük dosyası adına sahip *stdout_10652_20178101252.log*.

Örnek bir işte `aspNetCore` yapılandırır öğesi `stdout` günlüğü. `stdoutLogFile` Örnekte gösterilen yol Azure App Service için uygundur. Bir yerel yol veya ağ paylaşım yolu yerel günlüğe kaydetme için kabul edilebilir değil. Uygulama havuzu kullanıcı kimliği sağlanan yolun yazma izni olduğunu doğrulayın.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
Bkz: [web.config aracılığıyla yapılandırmaya](#configuration-via-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET çekirdeği modülü ile bir IIS paylaşılan yapılandırma

ASP.NET çekirdeği Modülü Yükleyicisi ayrıcalıklarıyla çalıştırır **sistem** hesabı. IIS paylaşılan yapılandırması tarafından kullanılan paylaşım yolu için izin sahip yerel sistem hesabı olmayan değiştirmek için bir erişim reddedildi hatası modülü ayarlarını yapılandırmak çalışırken yükleyici karşılaşır  *applicationHost.config* paylaşımda.

Desteklenmeyen geçici bir çözüm değildir paylaşılan IIS yapılandırmasını devre dışı bırakmak için yükleyiciyi çalıştırın, güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımı ve paylaşılan IIS yapılandırmasını yeniden etkinleştirin.

## <a name="module-schema-and-configuration-file-locations"></a>Modül, şema ve yapılandırmasını dosya konumları

### <a name="module"></a>Modül

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Şema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Yapılandırma

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Arayabilirsiniz *aspnetcore.dll* içinde *applicationHost.config* dosya. IIS Express, *applicationHost.config* dosyası varsayılan olarak mevcut olmaz. Dosyanın oluşturulma *{uygulama kökü}\.vs\config* hiçbir web uygulaması projesini Visual Studio çözümünde başlattığınızda.
