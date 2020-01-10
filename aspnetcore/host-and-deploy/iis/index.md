---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: 8e0475e3e18688c7d4344661826290d15a2443c0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829198"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Windows IIS üzerinde ASP.NET Core barındırma

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamasını bir IIS sunucusuna yayımlamaya yönelik bir öğretici deneyimi için, bkz. <xref:tutorials/publish-to-iis>.

[Paket barındırma .NET Core'u yükleme](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Supported operating systems

Aşağıdaki işletim sistemleri desteklenir:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eskiden webListener olarak adlandırılır), IIS ile ters proxy yapılandırmasında çalışmaz. Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).

Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.

Sorun giderme kılavuzu için bkz. <xref:test/troubleshoot>.

## <a name="supported-platforms"></a>Desteklenen platformlar

32-bit (x86) veya 64 bit (x64) dağıtımı için yayımlanan uygulamalar desteklenir. Uygulama dışında 32 bitlik bir uygulamayı 32 bit (x86) .NET Core SDK dağıtma:

* 64 bitlik bir uygulama için kullanılabilir daha büyük sanal bellek adres alanını gerektirir.
* Daha büyük IIS yığın boyutunu gerektirir.
* 64 bitlik yerel bağımlılıklara sahiptir.

64 bit uygulama yayımlamak için 64 bit (x64) .NET Core SDK kullanın. Ana bilgisayar sisteminde 64 bit çalışma zamanı bulunmalıdır.

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-models"></a>Barındırma modelleri

### <a name="in-process-hosting-model"></a>İşlem içi barındırma modeli

ASP.NET Core bir uygulama, işlem içi barındırma kullanarak IIS çalışan işlemiyle aynı işlemde çalışır. İşlem içi barındırma, istek dışı barındırmak için gelişmiş performans sağlar çünkü istekler, giden ağ trafiği ile aynı makineye geri döndürülen bir ağ arabirimidir. IIS, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)ile işlem yönetimini işler.

[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module):

* Uygulama başlatmayı gerçekleştirir.
  * [CoreCLR](/dotnet/standard/glossary#coreclr)'yi yükler.
  * `Program.Main`çağırır.
* IIS yerel isteğinin ömrünü işler.

İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.

Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve süreçte barındırılan bir uygulama arasındaki ilişki gösterilmektedir:

![İşlem içi barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-inprocess.png)

Web 'den çekirdek modu HTTP. sys sürücüsüne bir istek ulaşır. Sürücü, yerel isteği Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS). ASP.NET Core modülü yerel isteği alır ve IIS HTTP sunucusuna (`IISHttpServer`) geçirir. IIS HTTP sunucusu, isteği yerelden yönetilene dönüştüren bir IIS için işlem içi sunucu uygulamasıdır.

IIS HTTP sunucusu isteği işlediğinde, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir. Uygulamanın yanıtı IIS HTTP sunucusu aracılığıyla IIS 'e geri geçirilir. IIS yanıtı, isteği başlatan istemciye gönderir.

İşlem içi barındırma, mevcut uygulamalar için kabul ediyor, ancak tüm IIS ve IIS Express senaryoları için varsayılan [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar, işlem içi barındırma modeli için varsayılan olarak kullanılır.

`CreateDefaultBuilder`, [CoreCLR](/dotnet/standard/glossary#coreclr) 'yi önyüklemek ve uygulamayı IIS çalışan işleminin (*W3wp. exe* veya *iisexpress. exe*) içinde barındırmak için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> metodunu çağırarak bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> örneği ekler. Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!NOTE]
> Tek bir dosya yürütülebilir dosyası olarak yayınlanan uygulamalar, işlem içi barındırma modeliyle yüklenemez.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="out-of-process-hosting-model"></a>İşlem dışı barındırma modeli

ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, ASP.NET Core modülü işlem yönetimini işler. Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır. Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.

Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:

![İşlem dışı barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-outofprocess.png)

İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır. Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS). Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.

Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> uzantısı sunucuyu `http://localhost:{PORT}`dinlemek üzere yapılandırır. Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir. Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.

Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir. IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir. Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core, varsayılan, platformlar arası bir HTTP sunucusu olan [Kestrel Server](xref:fundamentals/servers/kestrel)ile birlikte gönderilir.

[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)kullanırken, uygulama IIS çalışan işleminden (*işlem dışı*) [Kestrel sunucusu](xref:fundamentals/servers/index#kestrel)ile ayrı bir işlemde çalışır.

ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini işler. Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır. Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.

Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:

![ASP.NET Core Modülü](index/_static/ancm-outofprocess.png)

İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır. Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS). Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.

Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu `http://localhost:{port}`dinlemek üzere yapılandırır. Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir. Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.

Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir. IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir. Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.

`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).

ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi. `UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`). Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`. Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:

* `UseUrls`
* [Kestrel'i'nın dinleme API](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))

Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir. Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.

::: moniker-end

ASP.NET Core modülü yapılandırma kılavuzu için bkz. <xref:host-and-deploy/aspnet-core-module>.

Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/index#host).

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="enable-the-iisintegration-components"></a>IISIntegration bileşenlerini etkinleştir

::: moniker range=">= aspnetcore-3.0"

`CreateHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırın:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#default-builder-settings>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`CreateWebHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırın:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#set-up-a-host>.

::: moniker-end

### <a name="iis-options"></a>IIS seçenekleri

::: moniker range=">= aspnetcore-2.2"

**İşlem içi barındırma modeli**

IIS sunucu seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISServerOptions> için bir hizmet yapılandırması ekleyin. Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:

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
| `AllowSynchronousIO`           | `false` | `HttpContext.Request` ve `HttpContext.Response`için zaman uyumlu GÇ izin verilip verilmeyeceğini belirtir. |
| `MaxRequestBodySize`           | `30000000`  | `HttpRequest`için en büyük istek gövdesi boyutunu alır veya ayarlar. IIS 'nin `IISServerOptions``MaxRequestBodySize` ayarlamadan önce işlenecek sınır `maxAllowedContentLength` sahip olduğunu unutmayın. `MaxRequestBodySize` değiştirmek `maxAllowedContentLength`etkilemez. `maxAllowedContentLength`artırmak için, *Web. config* dosyasına daha yüksek bir değere `maxAllowedContentLength` ayarlamak üzere bir giriş ekleyin. Daha fazla ayrıntı için bkz. [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration). |

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

IIS seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISOptions> için bir hizmet yapılandırması ekleyin. Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | `true`, [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components) [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar. Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`. Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi. Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu. |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |
| `ForwardClientCertificate`     | `true`  | Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Iletilen üstbilgiler ara yazılımını yapılandıran [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components)ve ASP.NET Core modülü, DÜZENI (http/https) ve isteğin KAYNAKLANDıĞı uzak IP adresini iletecek şekilde yapılandırılmıştır. Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Web.config dosyası

*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module). Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır. Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`). SDK'sı, proje dosyasının en üstüne ayarlanır:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Bir *Web. config* dosyası projede yoksa, dosya ASP.NET Core modülünü yapılandırmak Için doğru *processPath* ve *bağımsız değişkenlerle* oluşturulur ve [yayımlanan çıktıya](xref:host-and-deploy/directory-structure)taşınır.

Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı. Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.

*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir. ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.

Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir. Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

### <a name="webconfig-file-location"></a>Web.config dosyası konumu

[ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) doğru bir şekilde ayarlamak için, *Web. config* dosyası dağıtılan uygulamanın [içerik kök](xref:fundamentals/index#content-root) yolunda (genellikle uygulama temel yolu) bulunmalıdır. IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur. *Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.

Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*. *Web. config* dosyası mevcut olduğunda ve site normal olarak başlatıldığında, istenirse IIS bu hassas dosyaları sunmaz. Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.

***Web. config* dosyasının her zaman dağıtımda mevcut olması, doğru şekilde adlandırılması ve siteyi normal başlangıç için yapılandırabiliyor olması gerekir. *Web. config* dosyasını bir üretim dağıtımından hiçbir şekilde kaldırmayın.**

### <a name="transform-webconfig"></a>Web.config’i dönüştürme

Yayımlama sırasında *Web. config* ' i dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz. <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="iis-configuration"></a>IIS yapılandırması

**Windows Server işletim sistemleri**

Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.

1. Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**. Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)** .

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

Yükleme *.NET Core barındırma paket* barındıran sistemde. .NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module). Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.

> [!IMPORTANT]
> Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır. IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.
>
> .NET Core 'un 64 bit (x64) sürümünü yükledikten sonra barındırma paketi yüklenirse, SDK 'lar eksik gibi görünebilir ([hiçbir .NET Core SDK 'sı algılanmadı](xref:test/troubleshoot#no-net-core-sdks-were-detected)). Sorunu çözmek için bkz. <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.

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

1. Sunucuda yükleyiciyi çalıştırın. Aşağıdaki parametreler, yükleyiciyi yönetici komut kabuğu 'ndan çalıştırırken kullanılabilir:

   * `OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.
   * `OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor. Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.
   * `OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor. Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.
   * `OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları. 32 bitlik uygulamalar barındırmayabildiğinizi bildiğiniz durumlarda bu parametreyi kullanın. Gelecekte 32-bit ve 64 bit uygulamaları barındırabilmeniz gereken herhangi bir şansınız varsa, bu parametreyi kullanmayın ve her iki çalışma zamanını da yüklemeyin.
   * `OPT_NO_SHARED_CONFIG_CHECK=1` &ndash;, paylaşılan yapılandırma (*ApplicationHost. config*) IIS yüklemesiyle aynı MAKINELI olduğunda IIS paylaşılan yapılandırması kullanma denetimini devre dışı bırakır. *Yalnızca ASP.NET Core 2,2 veya sonraki bir sürümü Paketcisi yükleyicilerini barındırmak için kullanılabilir.* Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.
1. Sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:

   ```console
   net stop was /y
   net start w3svc
   ```
   Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için geri iletme davranışını benimsemez. Yeni bir barındırma paketi yükleyerek paylaşılan çerçeveyi yükselttikten sonra, sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:

```console
net stop was /y
net start w3svc
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Barındırma paketini yüklerken IIS 'deki bireysel siteleri el ile durdurmanız gerekli değildir. Barındırılan uygulamalar (IIS siteleri) IIS yeniden başlatıldığında yeniden başlatılır. Uygulamalar, [uygulama başlatma modülünden](#application-initialization-module-and-idle-timeout)da dahil olmak üzere ilk isteklerini alırken yeniden başlatılır.

ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için ileri sarma davranışını benimseme. IIS ile IIS tarafından barındırılan uygulamalar IIS ile yeniden başlatıldığında, ilk isteklerini alırken başvurulan paketlerinin en son düzeltme eki sürümleriyle yüklenir. IIS yeniden başlatılırsa, uygulamalar yeniden başlatılır ve çalışan işlemlerine geri dönüştürüldüğünde geri alma davranışını gösterir ve ilk isteklerini alırlar.

::: moniker-end

> [!NOTE]
> IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin

Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin. Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Tercih edilen yöntem, Webpı kullanmaktır. Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.

## <a name="create-the-iis-site"></a>IIS sitesi oluştur

1. Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun. Aşağıdaki adımda, klasörün yolu, uygulamanın fiziksel yolu olarak IIS 'ye sağlanır. Uygulamanın dağıtım klasörü ve dosya düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.

1. IIS Yöneticisi 'nde, **Bağlantılar** panelinde sunucunun düğümünü açın. Sağ **siteleri** klasör. Seçin **Web sitesi Ekle** bağlam menüsünde.

1. Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne. Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır. Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık bir ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

1. Sunucu düğümü altında seçin **uygulama havuzları**.

1. Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.

1. İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir. ASP.NET Core, masaüstü CLR 'nin (.NET CLR) yüklenmemesine&mdash;.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı çalışan işlemde barındırmak için önyüklenir. **.NET CLR sürümünün** **yönetilen kod olmadan** ayarlanması isteğe bağlıdır, ancak önerilir.

1. *ASP.NET Core 2.2 veya üzeri*: için bir 64 bit (x 64) [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) kullanan [işlem içi barındırma modeli](#in-process-hosting-model), 32 bit (x 86) işlemleri için uygulama havuzunu devre dışı bırakır.

   IIS Yöneticisi > **uygulama havuzlarının** **Eylemler** kenar çubuğunda, **uygulama havuzu varsayılanlarını** veya **Gelişmiş ayarları**ayarla ' yı seçin. Bulun **etkinleştirme 32-Bit uygulamaları** ve değerine `False`. Bu ayar için dağıtılan uygulamaları etkilemez [barındırma işlemi çıkış](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip. Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.

**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**  
Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Uygulamayı [IIS sitesi oluşturma](#create-the-iis-site) bölümünde oluşturulan IIS **fiziksel yol** klasörüne dağıtın. [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtım için önerilen mekanizmadır, ancak uygulamayı projenin *Publish* klasöründen barındırma sisteminin dağıtım klasörüne taşımak için çeşitli seçenekler mevcuttur.

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı

Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin. Barındırma sağlayıcısı, bir yayımlama profili veya oluşturma desteği sağlıyorsa, profilini indirin ve Visual Studio **Yayımlama** iletişim kutusunu kullanarak içeri aktarın:

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı Visual Studio'nun dışında

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir. Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternatif Web dağıtımı

Uygulamayı barındırma sistemine taşımak için el ile kopyalama, [xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy)veya [PowerShell](/powershell/)gibi çeşitli yöntemlerden birini kullanın.

IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.

## <a name="browse-the-website"></a>Web sitesine Gözat

Uygulama barındırma sistemine dağıtıldıktan sonra, uygulamanın genel uç noktalarından birine bir istek oluşturun.

Aşağıdaki örnekte site, **bağlantı noktası** `80``www.mysite.com` IIS **ana bilgisayar adına** bağlıdır. `http://www.mysite.com`bir istek yapıldı:

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Kilitli dağıtım dosyaları

Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı. Dağıtım sırasında kilitli dosyalar üzerine yazılamaz. Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:

* Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki. Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir. Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya. Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.
* *App_offline. htm* dosyasını bırakmak için PowerShell kullanın (PowerShell 5 veya üzerini gerektirir):

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

  Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir. Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur. Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.

  Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir. Varsayılan olarak, veri koruma anahtarları şifreli değildir. Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun. X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir. Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun. Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.

* **Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**

  Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için. **Kullanıcı profilini** `True`için Yükle ' ye ayarlayın. `True`olarak ayarlandığında anahtarlar Kullanıcı profili dizininde depolanır ve Kullanıcı hesabına özgü bir anahtarla DPAPI kullanılarak korunur. Anahtarlar *% LocalAppData%/ASP.exe. net/DataProtection-Keys* klasörüne kalıcıdır.

  Uygulama havuzunun [Setprofileenvironment özniteliğinin](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) de etkinleştirilmesi gerekir. `setProfileEnvironment` varsayılan değeri `true`. Bazı senaryolarda (örneğin, Windows işletim sistemi), `setProfileEnvironment` `false`olarak ayarlanır. Anahtarlar beklenen şekilde Kullanıcı profili dizininde depolanmıyorsa:

  1. *% Windir%/system32/inetsrv/config* klasörüne gidin.
  1. *ApplicationHost. config* dosyasını açın.
  1. `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesini bulun.
  1. `setProfileEnvironment` özniteliğinin mevcut olmadığını, bu değerin varsayılan olarak `true`veya özniteliğin değerini `true`olarak ayarlamış olduğunu doğrulayın.

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

Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir. Kök uygulamanın statik dosya ara yazılımı, statik varlık kök uygulamadan kullanılabilir olmadığı takdirde *404-bulunamayan* bir Yanıt ile sonuçlanır. Bu, kök uygulamanın [Web kökünden](xref:fundamentals/index#web-root)varlık sunmaya çalışır.

ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:

1. Alt uygulama için bir uygulama havuzu oluşturun. .NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı masaüstü CLR (.NET CLR) değil, çalışan işlemde barındırmak için önyüklenirken **.NET CLR sürümünü** **yönetilen kod olmadan** ayarlayın.

1. IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.

1. IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.

1. İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor. Seçin **Tamam**.

Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.

İşlem içi barındırma modeli ve ASP.NET Core modülünü yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Web.config ile IIS yapılandırma

IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için. Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır. IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.

Daha fazla bilgi için aşağıdaki konulara bakın:

* [\<System. webServer > için yapılandırma başvurusu](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama Havuzları

::: moniker range=">= aspnetcore-2.2"

Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:

* İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.
* Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.

IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu. Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz. IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma. Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

## <a name="application-pool-identity"></a>Uygulama Havuzu Kimliği

Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar. IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır. IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur. Bu kimlik kullanarak kaynakları güvenliği sağlanabilir. Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.

IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:

1. Windows Gezgini'ni açın ve dizine gidin.

1. Sağ tıklatın ve dizin **özellikleri**.

1. Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.

1. Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.

1. ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan. Seçin **Adları Denetle** düğmesi. İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**. Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir. Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir. Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. Seçin **Tamam**.

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

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

İşlem içi ve işlem dışı barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:

* Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
* Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.
* Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.
* TLS 1.2 veya sonraki bir bağlantı

Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.

::: moniker-end

HTTP/2 varsayılan olarak etkindir. Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner. IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="cors-preflight-requests"></a>CORS ön denetim istekleri

*Bu bölüm, yalnızca .NET Framework hedefleyen ASP.NET Core uygulamalar için geçerlidir.*

.NET Framework hedefleyen ASP.NET Core bir uygulama için, Seçenekler istekleri uygulamaya varsayılan olarak IIS 'de aktarılmaz. *Web. config* DOSYASıNDAKI uygulama IIS işleyicilerini seçenek isteklerini geçecek şekilde yapılandırma hakkında bilgi edinmek için bkz. [ASP.NET Web API 2 ' de çapraz kaynak ISTEKLERINI etkinleştirme: CORS 'nin nasıl çalıştığı](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a>Uygulama başlatma modülü ve boşta kalma zaman aşımı

ASP.NET Core modülü sürüm 2 tarafından IIS 'de barındırıldığında:

* [Uygulama başlatma modülü](#application-initialization-module) &ndash; uygulamanın barındırılan veya [işlem dışı](#out-of-process-hosting-model) bir [çalışan işlem yeniden](#in-process-hosting-model) başlatma veya sunucu yeniden başlatma üzerinde otomatik olarak başlayacak şekilde yapılandırılabilir.
* [Boşta kalma zaman aşımı](#idle-timeout) &ndash; uygulamanın barındırılan, [işlem](#in-process-hosting-model) yapılmayan dönemler sırasında zaman aşımına uğramaması için yapılandırılabilir.

### <a name="application-initialization-module"></a>Uygulama başlatma modülü

*İşlem içi ve işlem dışı barındırılan uygulamalar için geçerlidir.*

[IIS uygulaması başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) , uygulama havuzu başlatıldığında veya geri DÖNÜŞTÜRÜLDÜĞÜNDE uygulamaya http isteği gönderen bir IIS özelliğidir. İstek, uygulamayı başlatmak üzere tetikler. Varsayılan olarak IIS, uygulamayı başlatmak için uygulamanın kök URL 'SI (`/`) için bir istek yayınlar (yapılandırma hakkında daha fazla bilgi için [ek kaynaklara](#application-initialization-module-and-idle-timeout-additional-resources) bakın).

IIS uygulama başlatma rolü özelliğinin etkin olduğunu doğrulayın:

IIS 'yi yerel olarak kullanırken Windows 7 veya üzeri masaüstü sistemlerinde:

1. > programlar ve Özellikler > **Programlar** **ve Özellikler** ' **e gidin >** **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).
1. **Uygulama geliştirme özellikleri**> **Internet Information Services** > **World Wide Web Hizmetleri** 'ni açın.
1. **Uygulama başlatma**onay kutusunu seçin.

Windows Server 2008 R2 veya sonraki sürümlerde:

1. **Rol ve Özellik Ekleme Sihirbazı 'nı**açın.
1. **Rol hizmetlerini Seç** panelinde, **uygulama geliştirme** düğümünü açın.
1. **Uygulama başlatma**onay kutusunu seçin.

Site için uygulama başlatma modülünü etkinleştirmek üzere aşağıdaki yaklaşımlardan birini kullanın:

* IIS Yöneticisi 'Ni kullanma:

  1. **Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.
  1. Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.
  1. Varsayılan **Başlangıç modu** **OnDemand**' dir. **Başlangıç modunu** **AlwaysRunning**olarak ayarlayın. Seçin **Tamam**.
  1. **Bağlantılar** panelinde **siteler** düğümünü açın.
  1. Uygulamaya sağ tıklayın ve **Gelişmiş ayarlar**> **Web sitesini Yönet** ' i seçin.
  1. Varsayılan **önyükleme etkin** ayarı **false**şeklindedir. **Önyükleme etkin** ' i **true**olarak ayarlayın. Seçin **Tamam**.

* *Web. config*kullanarak, uygulamanın *Web. config* dosyasındaki `<system.webServer>` öğelerine `true` `doAppInitAfterRestart` ayarlanmış `<applicationInitialization>` öğesini ekleyin:

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

### <a name="idle-timeout"></a>Boşta Kalma Zaman Aşımı

*Yalnızca işlem sırasında barındırılan uygulamalar için geçerlidir.*

Uygulamanın çalışmasını engellemek için, IIS Yöneticisi 'Ni kullanarak uygulama havuzunun boşta kalma zaman aşımını ayarlayın:

1. **Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.
1. Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.
1. Varsayılan **boşta kalma süresi (dakika)** **20** dakikadır. **Boşta kalma zaman aşımını (dakika)** **0** (sıfır) olarak ayarlayın. Seçin **Tamam**.
1. Çalışan işlemini geri dönüştür.

[İşlem dışı](#out-of-process-hosting-model) barındırılan uygulamaların zaman aşımına uğramasını engellemek için aşağıdaki yaklaşımlardan birini kullanın:

* Uygulamayı çalışır durumda tutmak için bir dış hizmetten ping yapın.
* Uygulama yalnızca arka plan hizmetleri barındırıyorsa, IIS barındırmaktan kaçının ve [ASP.NET Core uygulamasını barındırmak için bir Windows hizmeti](xref:host-and-deploy/windows-service)kullanın.

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a>Uygulama başlatma modülü ve boşta kalma zaman aşımı ek kaynakları

* [IIS 8,0 uygulama başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* [Uygulama başlatma \<applicationınitialization >](/iis/configuration/system.webserver/applicationinitialization/).
* [Bir uygulama havuzu Için Işlem modeli ayarları \<processModel >](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a>IIS Yöneticiler için dağıtım kaynakları

IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.  
[IIS belgeleri](/iis)

.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.  
[.NET core uygulama dağıtımı](/dotnet/core/deploying/)

Yapılandırma kılavuzu dahil ASP.NET Core modülü hakkında bilgi edinin.  
<xref:host-and-deploy/aspnet-core-module>

Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.  
[Dizin yapısı](xref:host-and-deploy/directory-structure)

ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.  
[IIS modülleri](xref:host-and-deploy/iis/modules)

ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.  
[Sorun giderme](xref:test/troubleshoot-azure-iis)

Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.  
[Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot>
* [ASP.NET Core'a giriş](xref:index)
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Windows Server Teknik İçerik Kitaplığı](/windows-server/windows-server)
* [IIS HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
