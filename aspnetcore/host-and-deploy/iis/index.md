---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2019
uid: host-and-deploy/iis/index
ms.openlocfilehash: 6ba4da913ef712ef897a4c8418263e3060ea85ac
ms.sourcegitcommit: e67356f5e643a5d43f6d567c5c998ce6002bdeb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004975"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Windows IIS üzerinde ASP.NET Core barındırma

Tarafından [Luke Latham](https://github.com/guardrex)

[Paket barındırma .NET Core'u yükleme](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki işletim sistemleri desteklenir:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla çağrılan WebListener), ııs'li bir ters proxy yapılandırması çalışmaz. Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).

Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.

## <a name="supported-platforms"></a>Desteklenen platformlar

(X86) 32 bit veya 64-bit (x 64) dağıtım desteklenir için yayımlanan uygulamalar. .NET Core SDK'sı sürece bir 32-bit (x86) 32-bit uygulamayla dağıtma uygulama:

* Bir 64-bit uygulamalar tarafından kullanılabilecek daha büyük sanal bellek adres alanı gerektirir.
* Daha büyük IIS yığın boyutu gerektirir.
* 64-bit yerel bağımlılıkları vardır.

64 bitlik bir uygulamayı yayımlamak için bir 64-bit (x 64) .NET Core SDK'sını kullanın. Bir 64 bit çalışma zamanı ana bilgisayar sisteminde mevcut olmalıdır.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="enable-the-iisintegration-components"></a>IISIntegration bileşenlerini etkinleştir

Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker range=">= aspnetcore-2.2"

**İşlem içi barındırma modeli**

`CreateDefaultBuilder` çağrıları `UseIIS` önyükleme yöntemi [CoreCLR](/dotnet/standard/glossary#coreclr) ve IIS çalışan işlemi uygulama barındırın (*w3wp.exe* veya *iisexpress.exe*). Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.

İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.

**İşlem dışı barındırma modeli**

IIS ile işlem dışı barındırmak için `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).

ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi. `UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`). Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`. Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:

* `UseUrls`
* [Kestrel'i'nın dinleme API](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))

Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir. Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirttiğiniz bağlantı noktalarındaki Kestrel dinlediği.

İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).

ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi. `UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`). Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`. Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:

* `UseUrls`
* [Kestrel'i'nın dinleme API](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))

Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir. Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.

::: moniker-end

Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/index#host).

### <a name="iis-options"></a>IIS seçenekleri

::: moniker range=">= aspnetcore-2.2"

**İşlem içi barındırma modeli**

IIS sunucusu seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil <xref:Microsoft.AspNetCore.Builder.IISServerOptions> içinde <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |
| `AllowSynchronousIO`           | `false` | Zaman uyumlu g/ç için izin verilip verilmediğini `HttpContext.Request` ve `HttpContext.Response`. |
| `MaxRequestBodySize`           | `30000000`  | Alır veya ayarlar için en fazla istek gövdesi boyutu `HttpRequest`. IIS'nin sınırına sahip olduğuna dikkat edin `maxAllowedContentLength` hangi işlenmeyecek önce `MaxRequestBodySize` kümesinde `IISServerOptions`. Değiştirme `MaxRequestBodySize` etkilemez `maxAllowedContentLength`. Artırmak için `maxAllowedContentLength`, bir giriş ekleyin *web.config* ayarlanacak `maxAllowedContentLength` daha yüksek bir değer. Daha fazla ayrıntı için [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration). |

**İşlem dışı barındırma modeli**

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**İşlem dışı barındırma modeli**

::: moniker-end

IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil <xref:Microsoft.AspNetCore.Builder.IISOptions> içinde <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Varsa `true`, IIS tümleştirme ara yazılımı ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu. |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |
| `ForwardClientCertificate`     | `true`  | Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır. Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Web.config dosyası

*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module). Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır. Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`). SDK'sı, proje dosyasının en üstüne ayarlanır:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Varsa bir *web.config* dosyası projedeki mevcut değil, dosyanın doğru oluşturulması *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:host-and-deploy/aspnet-core-module) ve taşınabilir [yayımlanmış çıktısı](xref:host-and-deploy/directory-structure).

Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı. Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.

*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir. ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.

Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Web.config dosyası konumu

Ayarlamak için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) doğru *web.config* dosya dağıtılan uygulamayı içerik kök yolda (genellikle uygulama temel yolu) mevcut olmalıdır. IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur. *Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.

Mevcut uygulamanın fiziksel yola gibi hassas dosyalar *\<derleme >. runtimeconfig.json*, *\<derleme > .xml* (XML belgeleri açıklamaları) ve *\<derleme >. deps.json*. Zaman *web.config* dosya varsa ve siteyi normalde başlatır, IIS bunların istenmesi halinde bu hassas dosyalar hizmet değil. Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.

***Web.config* doğru adlı, dağıtımdaki her zaman mevcut ve yukarı site normal başlangıç için yapılandırmak için dosya olmalıdır. Hiçbir zaman Kaldır *web.config* dosyasından bir üretim dağıtımı.**

### <a name="transform-webconfig"></a>Web.config’i dönüştürme

Dönüştürmeniz gerekirse *web.config* (örneğin, ortam değişkenlerini ayarlama, yapılandırma, profili veya ortama göre), bkz: yayımlama sırasında <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="iis-configuration"></a>IIS yapılandırması

**Windows Server işletim sistemleri**

Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.

1. Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**. Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)**.

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler. İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   **Windows kimlik doğrulaması (isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**. Seçin **Windows kimlik doğrulaması** özelliği. Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

   **WebSockets (isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**. Seçin **WebSocket Protokolü** özelliği. Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).

1. Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım. Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.

**Windows masaüstü işletim sistemleri**

Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.

1. Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).

1. Açık **Internet Information Services** düğümü. Açık **Web yönetimi araçları** düğümü.

1. İçin kutuyu **IIS Yönetim Konsolu**.

1. İçin kutuyu **World Wide Web Hizmetleri**.

1. Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.

   **Windows kimlik doğrulaması (isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**. Seçin **Windows kimlik doğrulaması** özelliği. Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

   **WebSockets (isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**. Seçin **WebSocket Protokolü** özelliği. Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).

1. IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Paket barındırma .NET Core'u yükleme

Yükleme *.NET Core barındırma paket* barındıran sistemde. .NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module). Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar. Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.

> [!IMPORTANT]
> Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır. IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.
>
> .NET Core 64-bit (x 64) sürümünü yükledikten sonra barındırma paket yüklü değilse, SDK'ları eksik görünebilir ([.NET Core SDK algılandı](xref:test/troubleshoot#no-net-core-sdks-were-detected)). Bu sorunu gidermek için bkz: <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.

### <a name="direct-download-current-version"></a>Doğrudan indirme (geçerli sürüm)

Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:

[Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Yükleyici önceki sürümleri

Yükleyici önceki bir sürümünü almak için:

1. Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).
1. Altında **.NET Core**, .NET Core sürümünüzü seçin.
1. İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.
1. Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.

> [!WARNING]
> Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir. Daha fazla bilgi için [Destek İlkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

### <a name="install-the-hosting-bundle"></a>Barındırma paket yükleme

1. Sunucuda yükleyiciyi çalıştırın. Aşağıdaki parametreleri, yükleyici komut kabuğunu yönetici olarak çalıştırılırken kullanılabilir:

   * `OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.
   * `OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.
   * `OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.
   * `OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları. 32-bit uygulamaları barındırma gerekmez, bildiğiniz durumlarda bu parametreyi kullanın. Hem 32 bit hem de 64-bit uygulamaları gelecekte barındıracak ihtimali varsa, yoksa bu parametreyi kullanın ve her iki çalışma zamanları yükleyin.
   * `OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; IIS paylaşılan yapılandırması kullanmak için denetimi devre dışı olduğunda paylaşılan yapılandırma (*applicationHost.config*) IIS yüklemesi ile aynı makinede olduğu. *Yalnızca, ASP.NET Core 2.2 veya üzeri barındırma Bundler yükleyicileri için de kullanılabilir.* Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.
1. Sistemi yeniden başlatın veya yürütme **net stop olan /y**çizgidir **net start w3svc** komut kabuğundan. Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.

> [!NOTE]
> IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin

Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin. Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Tercih edilen yöntem, Webpı kullanmaktır. Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.

## <a name="create-the-iis-site"></a>IIS sitesi oluştur

1. Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun. Bir uygulamanın dağıtım Düzen açıklanan [dizin yapısı](xref:host-and-deploy/directory-structure) konu.

1. IIS Yöneticisi'nde açın sunucu düğümünde **bağlantıları** paneli. Sağ **siteleri** klasör. Seçin **Web sitesi Ekle** bağlam menüsünde.

1. Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne. Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır. Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık bir ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

1. Sunucu düğümü altında seçin **uygulama havuzları**.

1. Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.

1. İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir. ASP.NET Core değil kullanan masaüstü CLR (.NET CLR) yüklemede de&mdash;çekirdek ortak dil çalışma zamanı (CoreCLR) .NET Core için çalışan işlemi uygulamayı barındırmak için önyüklenir. Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlı ancak önerilir.

1. *ASP.NET Core 2.2 veya üzeri*: Bir 64-bit (x64) için [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) kullanan [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model), 32 bit (x 86) işlemleri için uygulama havuzunu devre dışı bırakır.

   İçinde **eylemleri** kenar IIS Yöneticisi'nin > **uygulama havuzları**seçin **uygulama havuzu Varsayılanlarını Ayarla** veya **Gelişmiş ayarlar**. Bulun **etkinleştirme 32-Bit uygulamaları** ve değerine `False`. Bu ayar için dağıtılan uygulamaları etkilemez [barındırma işlemi çıkış](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip. Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.

**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**  
Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Uygulamayı barındıran sistemde oluşturduğunuz klasöre dağıtın. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı

Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin. Barındırma sağlayıcısı oluşturmak için bir yayımlama profili veya destek sağlar, bunların profilini indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı Visual Studio'nun dışında

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir. Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternatif Web dağıtımı

Uygulama barındırma sistemin el ile kopyalama, Xcopy, Robocopy ve PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.

IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.

## <a name="browse-the-website"></a>Web sitesine Gözat

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Kilitli dağıtım dosyaları

Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı. Dağıtım sırasında kilitli dosyalar üzerine yazılamaz. Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:

* Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki. Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir. Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.
* Silmek için PowerShell kullanma *app_offline.html* (PowerShell 5 veya üzeri gerekir):

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a>Veri koruma

[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere. Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management). Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.

Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:

* Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır. 
* Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir. 
* Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir. Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).

Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:

* **Veri koruma kayıt defteri anahtarları oluşturma**

  ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır. Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.

  Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir. Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur. Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.

  Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir. Varsayılan olarak, veri koruma anahtarları şifreli değildir. Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun. X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir. Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: Kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve kullanıcının uygulama çalıştığı tüm makinelerde kullanılabilir emin olun. Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.

* **Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**

  Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için. Ayarlama **kullanıcı profilini Yükle** için `True`. Ayarlandığında `True`, anahtarları kullanıcı profili dizinde depolanır ve kullanıcı hesabı için özel bir anahtarla DPAPI kullanılarak korunan. Anahtarlar için kaldı *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* klasör.

  Uygulama havuzunun [setProfileEnvironment özniteliği](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) etkinleştirilmiş olması da gerekir. Varsayılan değer olan `setProfileEnvironment` olduğu `true`. (Örneğin, Windows işletim sistemi), bazı senaryolarda `setProfileEnvironment` ayarlanır `false`. Kullanıcı profili dizinde anahtarları depolanmaz, beklenen:

  1. Gidin *%windir%/system32/inetsrv/config* klasör.
  1. Açık *applicationHost.config* dosya.
  1. Bulun `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesi.
  1. Onaylayın `setProfileEnvironment` özniteliği mevcut olmadığında, bunun varsayılan değeri için `true`, veya özniteliğin değerini açık olarak `true`.

* **Dosya sistemi anahtarı halkası deposu olarak kullanın**

  Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview). Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun. Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.

  IIS web grubunda kullanırken:

  * Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.
  * X X509 dağıtma her makine için sertifika. Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).

* **Veri koruma için makine genelinde bir ilke ayarlayın**

  Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için. Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.

## <a name="virtual-directories"></a>Sanal dizinler

[IIS sanal dizinlerinin](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ile ASP.NET Core uygulamaları desteklenmez. Bir uygulama olarak barındırılan bir [alt uygulama](#sub-applications).

## <a name="sub-applications"></a>Alt uygulamalar

ASP.NET Core uygulaması olarak barındırılan bir [IIS alt uygulama (uygulama içi sub)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications). Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.

::: moniker range="< aspnetcore-2.2"

Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir. Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.

Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici sub-uygulamanın açıkça Kaldır *web.config* dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Alt uygulama içindeki statik varlık bağlantılar tilde eğik çizgi kullanılmalıdır (`~/`) gösterimi. Tilde eğik çizgi gösterimi Tetikleyiciler bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) işlenmiş göreli bağlantısını için alt-uygulamanın pathbase önüne eklediğinizden. Alt uygulama için `/subapp_path`, bir görüntü ile bağlantılı `src="~/image.png"` olarak işlenen `src="/subapp_path/image.png"`. Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor. İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.

Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir. Kök uygulamanın statik dosya ara yazılımlarını kök uygulamanın varlığından hizmet dener [web kök](xref:fundamentals/index#web-root), hangi sonuçlanıyor bir *404 - Bulunamadı* yanıt statik varlık kök uygulama kullanılabilir değilse.

ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:

1. Alt uygulama için bir uygulama havuzu oluşturun. Ayarlama **.NET CLR sürümü** için **yönetilen kod yok** .NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR) değil Masaüstü CLR (.NET CLR) çalışan işlemindeki uygulamayı barındırmak için önyüklendiğinde olduğundan.

1. IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.

1. IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.

1. İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor. Seçin **Tamam**.

Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.

Barındırma modeli ve ASP.NET Core modülü yapılandırma işlem hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module> ve <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Web.config ile IIS yapılandırma

IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için. Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır. IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.

Daha fazla bilgi için [yapılandırma başvurusu için \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET içeren IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules). (IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama havuzları

::: moniker range=">= aspnetcore-2.2"

Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:

* İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.
* Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.

IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu. Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz. IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma. Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

## <a name="application-pool-identity"></a>Uygulama havuzu kimliği

Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar. IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır. IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur. Bu kimlik kullanarak kaynakları güvenliği sağlanabilir. Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.

IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:

1. Windows Gezgini'ni açın ve dizine gidin.

1. Sağ tıklatın ve dizin **özellikleri**.

1. Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.

1. Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.

1. ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan. Seçin **Adları Denetle** düğmesi. İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**. Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir. Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir. Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. **Tamam**’ı seçin.

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: Nesne adı "DefaultAppPool", "Adları denetle" seçtikten sonra nesne adları alanında gösterilir.](index/_static/select-users-or-groups-2.png)

1. Okuma &amp; Yürütme izinleri varsayılan verilmesi. Gerektiğinde ek izinler sağlar.

Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı. Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.

## <a name="http2-support"></a>HTTP/2 desteği

::: moniker range=">= aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:

* İşlem içi
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * TLS 1.2 veya sonraki bir bağlantı
* İşlem dışı
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.
  * TLS 1.2 veya sonraki bir bağlantı

Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`. Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.

İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:

* Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
* Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.
* Hedef çerçeve: HTTP/2 bağlantı beri işlem dışı dağıtımlar için geçerli değildir tamamen IIS tarafından işlenir.
* TLS 1.2 veya sonraki bir bağlantı

Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.

::: moniker-end

HTTP/2 varsayılan olarak etkindir. Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner. IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="cors-preflight-requests"></a>CORS denetim öncesi isteği

*Bu bölüm, yalnızca .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için geçerlidir.*

ASP.NET Core uygulaması için hedefleyen .NET Framework'IIS içinde varsayılan olarak OPTIONS istekleri uygulamaya geçirilen değildir. Uygulamanın IIS işleyicileri yapılandırma hakkında bilgi edinmek için *web.config* seçenekleri isteklerini iletmek için bkz: [ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme: CORS işleyişi](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a>Uygulama Başlatma modülü ve boşta kalma zaman aşımı

IIS içinde ASP.NET Core modülü sürüm 2 barındırıldığında:

* [Uygulama Başlatma modülü](#application-initialization-module) &ndash; uygulamanın barındırılan [işlem içi](xref:fundamentals/servers/index#in-process-hosting-model) veya [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model), bir çalışan işlemin yeniden veya sunucuda otomatik olarak başlatılacak şekilde yapılandırılabilir yeniden başlatın.
* [Boşta kalma zaman aşımı](#idle-timeout) &ndash; uygulamanın barındırılan [işlem içi](xref:fundamentals/servers/index#in-process-hosting-model) için zaman aşımı etkin olmadığı dönemler sırasında yapılandırılabilir.

### <a name="application-initialization-module"></a>Uygulama Başlatma modülü

*İşlemdeki barındırılan uygulamaları ve işlem dışı için geçerlidir.*

[IIS uygulama başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) uygulama havuzunu başlatır veya geri dönüştürüldüğünde, uygulamaya bir HTTP isteği gönderir, bir IIS özelliğidir. İstek, uygulamanın başlatılması tetikler. Varsayılan olarak, IIS uygulama kök URL'si için bir istek sorunları (`/`) uygulamayı başlatmak için (bkz [ek kaynaklar](#application-initialization-module-and-idle-timeout-additional-resources) yapılandırması hakkında daha fazla ayrıntı için).

IIS uygulama başlatma rolü özelliği etkin olduğunu doğrulayın:

Windows 7 veya üzeri Masaüstü sistemlerinde IIS yerel olarak kullanırken:

1. Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).
1. Açık **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.
1. Onay kutusunu seçin **uygulama başlatma**.

Windows Server 2008 R2 veya sonraki sürümlerde:

1. Açık **rol ve Özellik Ekle Sihirbazı**.
1. İçinde **rol hizmetlerini seçin** paneli, açık **uygulama geliştirme** düğümü.
1. Onay kutusunu seçin **uygulama başlatma**.

Site için uygulama başlatma modülü etkinleştirmek için aşağıdaki yaklaşımlardan birini kullanın:

* IIS Yöneticisi'ni kullanarak:

  1. Seçin **uygulama havuzları** içinde **bağlantıları** paneli.
  1. Uygulamanın uygulama havuzu listesi seçip sağ **Gelişmiş ayarlar**.
  1. Varsayılan **modunu Başlat** olduğu **OnDemand**. Ayarlama **başlangıç modu** için **AlwaysRunning**. **Tamam**’ı seçin.
  1. Açık **siteleri** düğümünde **bağlantıları** paneli.
  1. Uygulamayı sağ tıklayıp **Web sitesini Yönet** > **Gelişmiş ayarlar**.
  1. Varsayılan **önceden yükleme etkin** ayardır **False**. Ayarlama **önceden yükleme etkin** için **True**. **Tamam**’ı seçin.

* Kullanarak *web.config*, ekleme `<applicationInitialization>` öğeyle `doAppInitAfterRestart` kümesine `true` için `<system.webServer>` uygulamanın öğelerindeki *web.config* dosyası:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a>Boşta kalma zaman aşımı

*Yalnızca barındırılan uygulamaları işlem içinde geçerlidir.*

Uygulama boşa gelen önlemek için IIS Yöneticisi'ni kullanarak uygulama havuzunun boşta kalma zaman aşımını ayarlayın:

1. Seçin **uygulama havuzları** içinde **bağlantıları** paneli.
1. Uygulamanın uygulama havuzu listesi seçip sağ **Gelişmiş ayarlar**.
1. Varsayılan **boşta kalma zaman aşımı (dakika)** olduğu **20** dakika. Ayarlama **boşta kalma zaman aşımı (dakika)** için **0** (sıfır). **Tamam**’ı seçin.
1. Çalışan işlemi geri dönüştürün.

Barındırılan uygulamaları engellemek için [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model) gelen aşımından aşağıdaki yaklaşımlardan birini kullanın:

* Uygulamayı bir dış hizmetten çalışmasını sağlamak için ping yapın.
* Uygulamayı yalnızca arka plan Hizmetleri barındırıyorsa, IIS barındırma kaçının ve kullanan bir [ASP.NET Core uygulaması barındırmak üzere Windows hizmet](xref:host-and-deploy/windows-service).

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a>Uygulama Başlatma modülü ve boşta kalma zaman aşımı ek kaynaklar

* [IIS 8.0 uygulama başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* [Uygulama başlatma \<applicationInitialization >](/iis/configuration/system.webserver/applicationinitialization/).
* [İşlem modeli ayarları için bir uygulama havuzu \<processModel >](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a>IIS Yöneticiler için dağıtım kaynakları

IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.  
[IIS belgeleri](/iis)

.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.  
[.NET core uygulama dağıtımı](/dotnet/core/deploying/)

ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.  
[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module)

ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.  
[ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)

Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.  
[Dizin yapısı](xref:host-and-deploy/directory-structure)

ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.  
[IIS modülleri](xref:host-and-deploy/iis/troubleshoot)

ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.  
[Sorun giderme](xref:host-and-deploy/iis/troubleshoot)

Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.  
[Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot>
* [ASP.NET Core'a giriş](xref:index)
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Windows Server Teknik İçerik Kitaplığı](/windows-server/windows-server)
* [IIS HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
