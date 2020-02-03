---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: 146a204509856186a2696b770cae2249d348fa34
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726830"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Windows IIS üzerinde ASP.NET Core barındırma

[Luke Latham](https://github.com/guardrex) tarafından

ASP.NET Core uygulamasını bir IIS sunucusuna yayımlamaya yönelik bir öğretici deneyimi için, bkz. <xref:tutorials/publish-to-iis>.

[.NET Core barındırma paketi 'ni yükler](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki işletim sistemleri desteklenmektedir:

::: moniker range=">= aspnetcore-3.0"

* Windows 7 veya üzeri
* Windows Server 2012 R2 veya üzeri

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

::: moniker-end

[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eskiden webListener olarak adlandırılır), IIS ile ters proxy yapılandırmasında çalışmaz. [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)kullanın.

Azure 'da barındırma hakkında bilgi için bkz. <xref:host-and-deploy/azure-apps/index>.

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

`CreateDefaultBuilder`, [CoreCLR](/dotnet/standard/glossary#coreclr) 'yi önyüklemek ve uygulamayı IIS çalışan işleminin (*W3wp. exe* veya *iisexpress. exe*) içinde barındırmak için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> metodunu çağırarak bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> örneği ekler. Performans testleri, bir .NET Core uygulamasını işlem içinde barındıran uygulamanın, uygulamayı işlem dışı ve [Kestrel](xref:fundamentals/servers/kestrel) sunucusuna proxy alma isteklerinin barındırılmasına kıyasla önemli ölçüde daha yüksek istek aktarım hızı sağladığını gösterir.

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

`CreateDefaultBuilder`, Web sunucusu olarak [Kestrel](xref:fundamentals/servers/kestrel) sunucusunu yapılandırır ve [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module)için temel yolu ve bağlantı noktasını yapılandırarak IIS tümleştirmesini mümkün yapar.

ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemini çağırır. `UseIISIntegration`, Kestrel 'yi localhost IP adresinde (`127.0.0.1`) dinamik bağlantı noktasında dinleyecek şekilde yapılandırır. Dinamik bağlantı noktası 1234 ise, Kestrel `127.0.0.1:1234`dinler. Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:

* `UseUrls`
* [Kestrel 'in dinleme API 'SI](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Yapılandırma](xref:fundamentals/configuration/index) (veya [komut satırı--URL seçeneği](xref:fundamentals/host/web-host#override-configuration))

Modül kullanılırken `UseUrls` veya Kestrel 'un `Listen` API 'sine yapılan çağrılar gerekli değildir. `UseUrls` veya `Listen` çağrılırsa, Kestrel yalnızca uygulamayı IIS olmadan çalıştırırken belirtilen bağlantı noktasını dinler.

::: moniker-end

ASP.NET Core modülü yapılandırma kılavuzu için bkz. <xref:host-and-deploy/aspnet-core-module>.

Barındırma hakkında daha fazla bilgi için bkz. [ASP.NET Core ana bilgisayar](xref:fundamentals/index#host).

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
| `AutomaticAuthentication`      | `true`  | `true`, IIS sunucusu, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar. `false`, sunucu yalnızca `HttpContext.User` için bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir. `AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir. Daha fazla bilgi için bkz. [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |
| `AllowSynchronousIO`           | `false` | `HttpContext.Request` ve `HttpContext.Response`için zaman uyumlu GÇ izin verilip verilmeyeceğini belirtir. |
| `MaxRequestBodySize`           | `30000000`  | `HttpRequest`için en büyük istek gövdesi boyutunu alır veya ayarlar. IIS 'nin `IISServerOptions``MaxRequestBodySize` ayarlamadan önce işlenecek sınır `maxAllowedContentLength` sahip olduğunu unutmayın. `MaxRequestBodySize` değiştirmek `maxAllowedContentLength`etkilemez. `maxAllowedContentLength`artırmak için, *Web. config* dosyasına daha yüksek bir değere `maxAllowedContentLength` ayarlamak üzere bir giriş ekleyin. Daha fazla ayrıntı için bkz. [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration). |

**İşlem dışı barındırma modeli**

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | `true`, IIS sunucusu, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar. `false`, sunucu yalnızca `HttpContext.User` için bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir. `AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir. Daha fazla bilgi için bkz. [Windows kimlik doğrulaması](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**İşlem dışı barındırma modeli**

::: moniker-end

IIS seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISOptions> için bir hizmet yapılandırması ekleyin. Aşağıdaki örnek, uygulamanın `HttpContext.Connection.ClientCertificate`doldurmasını engeller:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Seçenek                         | Varsayılan | Ayar |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | `true`, [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components) [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar. `false`, ara yazılım yalnızca `HttpContext.User` bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir. `AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir. Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konusuna bakın. |
| `AuthenticationDisplayName`    | `null`  | Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar. |
| `ForwardClientCertificate`     | `true`  | `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üst bilgisi varsa `HttpContext.Connection.ClientCertificate` doldurulur. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Iletilen üstbilgiler ara yazılımını yapılandıran [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components)ve ASP.NET Core modülü, DÜZENI (http/https) ve isteğin KAYNAKLANDıĞı uzak IP adresini iletecek şekilde yapılandırılmıştır. Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz. [proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Web.config dosyası

*Web. config* dosyası [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yapılandırır. *Web. config* dosyası oluşturma, dönüştürme ve yayımlama, proje yayımlandığında bir MSBuild hedefi (`_TransformWebConfig`) tarafından işlenir. Bu hedef, Web SDK hedeflerinde (`Microsoft.NET.Sdk.Web`) bulunur. SDK'sı, proje dosyasının en üstüne ayarlanır:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Bir *Web. config* dosyası projede yoksa, dosya ASP.NET Core modülünü yapılandırmak Için doğru *processPath* ve *bağımsız değişkenlerle* oluşturulur ve [yayımlanan çıktıya](xref:host-and-deploy/directory-structure)taşınır.

Projede bir *Web. config* dosyası varsa, dosya ASP.NET Core modülünü yapılandırmak ve yayımlanan çıktıya taşınmak Için doğru *processPath* ve *bağımsız değişkenlerle* birlikte dönüştürülür. Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.

*Web. config* dosyası, etkin IIS modüllerini DENETLEYEN ek IIS yapılandırma ayarları verebilir. ASP.NET Core uygulamalarla istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz. [IIS modules](xref:host-and-deploy/iis/modules) konusu.

Web SDK 'sının *Web. config* dosyasını dönüştürmasını engellemek için, proje dosyasında **\<ıstransformwebconfigdisabled >** özelliğini kullanın:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Web SDK 'sının dosyayı dönüştürmesiyle devre dışı bırakıldığında, *processPath* ve *bağımsız değişkenler* geliştirici tarafından el ile ayarlanmalıdır. Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

### <a name="webconfig-file-location"></a>Web.config dosyası konumu

[ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) doğru bir şekilde ayarlamak için, *Web. config* dosyası dağıtılan uygulamanın [içerik kök](xref:fundamentals/index#content-root) yolunda (genellikle uygulama temel yolu) bulunmalıdır. IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur. Web Dağıtımı kullanarak birden çok uygulama yayımlamayı etkinleştirmek için uygulamanın kökünde *Web. config* dosyası gereklidir.

Uygulamanın fiziksel yolunda ( *\<derlemesi >. runtimeconfig. JSON*, *\<bütünleştirilmiş kod >. xml* (XML belge açıklamaları) ve *\<derlemesi >. Deps. JSON*gibi hassas dosyalar bulunur. *Web. config* dosyası mevcut olduğunda ve site normal olarak başlatıldığında, istenirse IIS bu hassas dosyaları sunmaz. *Web. config* dosyası eksikse, yanlış şekilde adlandırılmış veya site normal başlatma için YAPıLANDıRıMıŞSA IIS hassas dosyalara genel olarak sunabilir.

***Web. config* dosyasının her zaman dağıtımda mevcut olması, doğru şekilde adlandırılması ve siteyi normal başlangıç için yapılandırabiliyor olması gerekir. *Web. config* dosyasını bir üretim dağıtımından hiçbir şekilde kaldırmayın.**

### <a name="transform-webconfig"></a>Web.config’i dönüştürme

Yayımlama sırasında *Web. config* ' i dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz. <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="iis-configuration"></a>IIS yapılandırması

**Windows Server işletim sistemleri**

**Web sunucusu (IIS)** sunucu rolünü etkinleştirin ve rol hizmetleri oluşturun.

1. **Yönet** menüsündeki **rol ve özellik ekleme** sihirbazı ' nı veya **Sunucu Yöneticisi**bağlantısındaki bağlantıyı kullanın. **Sunucu rolleri** adımında, **Web sunucusu (IIS)** kutusunu işaretleyin.

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. **Özellikler** adımından sonra, Web sunucusu (IIS) için **rol hizmetleri** adımı yüklenir. İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   **Windows kimlik doğrulaması (Isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için şu düğümleri genişletin: **Web sunucusu** > **güvenliği**. **Windows kimlik doğrulama** özelliğini seçin. Daha fazla bilgi için bkz. [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).

   **WebSockets (Isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için şu düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**. **WebSocket protokolü** özelliğini seçin. Daha fazla bilgi için bkz. [WebSockets](xref:fundamentals/websockets).

1. Web sunucusu rolü ve hizmetlerini yüklemek için **onay** adımına ilerleyin. **Web sunucusu (IIS)** rolü yüklendikten sonra sunucu/IIS yeniden başlatması gerekli değildir.

**Windows masaüstü işletim sistemleri**

**IIS Yönetim Konsolu** ve **World Wide Web hizmetlerini**etkinleştirin.

1.  > programlar ve Özellikler > **Programlar** **ve Özellikler** ' **e gidin > ** **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).

1. **Internet Information Services** düğümünü açın. **Web yönetimi araçları** düğümünü açın.

1. **IIS Yönetim Konsolu**kutusunu işaretleyin.

1. **World Wide Web Hizmetleri**kutusunu işaretleyin.

1. **World Wide Web Hizmetleri** için varsayılan özellikleri kabul edın veya IIS özelliklerini özelleştirin.

   **Windows kimlik doğrulaması (Isteğe bağlı)**  
   Windows kimlik doğrulamasını etkinleştirmek için şu düğümleri genişletin: **World Wide Web hizmetleri** > **güvenlik**. **Windows kimlik doğrulama** özelliğini seçin. Daha fazla bilgi için bkz. [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).

   **WebSockets (Isteğe bağlı)**  
   WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir. WebSockets etkinleştirmek için şu düğümleri genişletin: **World Wide Web hizmetleri** > **uygulama geliştirme özellikleri**. **WebSocket protokolü** özelliğini seçin. Daha fazla bilgi için bkz. [WebSockets](xref:fundamentals/websockets).

1. IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Paket barındırma .NET Core'u yükleme

*.NET Core barındırma paketi* 'ni barındırma sistemine yükler. Paket, .NET Core çalışma zamanı, .NET Core kitaplığı ve [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)de yüklüyor. Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.

> [!IMPORTANT]
> Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır. IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.
>
> .NET Core 'un 64 bit (x64) sürümünü yükledikten sonra barındırma paketi yüklenirse, SDK 'lar eksik gibi görünebilir ([hiçbir .NET Core SDK 'sı algılanmadı](xref:test/troubleshoot#no-net-core-sdks-were-detected)). Sorunu çözmek için bkz. <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.

### <a name="direct-download-current-version"></a>Doğrudan indirme (geçerli sürüm)

Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:

[Geçerli .NET Core barındırma paketi yükleyicisi (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Yükleyici önceki sürümleri

Yükleyici önceki bir sürümünü almak için:

1. [.Net indirme arşivleri](https://www.microsoft.com/net/download/archives)' ne gidin.
1. **.NET Core**altında .NET Core sürümünü seçin.
1. **Uygulamaları Çalıştır-çalışma zamanı** sütununda, Istenen .NET Core çalışma zamanı sürümünün satırını bulun.
1. **Çalışma zamanı & barındırma paketi** bağlantısını kullanarak yükleyiciyi indirin.

> [!WARNING]
> Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir. Daha fazla bilgi için bkz. [destek ilkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

### <a name="install-the-hosting-bundle"></a>Barındırma paket yükleme

1. Sunucuda yükleyiciyi çalıştırın. Aşağıdaki parametreler, yükleyiciyi yönetici komut kabuğu 'ndan çalıştırırken kullanılabilir:

   * ASP.NET Core modülünü yüklemeyi `OPT_NO_ANCM=1` &ndash; atlayın.
   * .NET Core çalışma zamanını yüklemeyi `OPT_NO_RUNTIME=1` &ndash; atlayın. Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.
   * ASP.NET paylaşılan çerçevesini yüklemeyi `OPT_NO_SHAREDFX=1` &ndash; atlayın (ASP.NET Runtime). Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.
   * `OPT_NO_X86=1` &ndash; x86 çalışma zamanları yüklemeyi atlayın. 32 bitlik uygulamalar barındırmayabildiğinizi bildiğiniz durumlarda bu parametreyi kullanın. Gelecekte 32-bit ve 64 bit uygulamaları barındırabilmeniz gereken herhangi bir şansınız varsa, bu parametreyi kullanmayın ve her iki çalışma zamanını da yüklemeyin.
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
> IIS paylaşılan Yapılandırması hakkında bilgi için bkz. [IIS paylaşılan yapılandırması ile ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin

Uygulamaları [Web dağıtımı](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later)olan sunuculara dağıttığınızda, en son Web dağıtımı sürümünü sunucuya yüklersiniz. Web Dağıtımı yüklemek için [Web Platformu Yükleyicisi 'ni (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) kullanın veya doğrudan [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=43717)'nden bir yükleyici edinin. Tercih edilen yöntem, Webpı kullanmaktır. Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.

## <a name="create-the-iis-site"></a>IIS sitesi oluştur

1. Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun. Aşağıdaki adımda, klasörün yolu, uygulamanın fiziksel yolu olarak IIS 'ye sağlanır. Uygulamanın dağıtım klasörü ve dosya düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.

1. IIS Yöneticisi 'nde, **Bağlantılar** panelinde sunucunun düğümünü açın. **Siteler** klasörüne sağ tıklayın. Bağlamsal menüden **Web sitesi Ekle** ' yi seçin.

1. Bir **site adı** belirtin ve **fiziksel yolu** uygulamanın dağıtım klasörüne ayarlayın. **Bağlama** yapılandırmasını sağlayın ve **Tamam**' ı seçerek Web sitesini oluşturun:

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Üst düzey joker karakter bağlamaları (`http://*:80/` ve `http://+:80` **) kullanılmamalıdır.** Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık bir ana bilgisayar adları kullanın. Alt etki alanı joker karakteri bağlama (örneğin, `*.mysub.com`), tüm üst etki alanını (güvenlik açığı olan `*.com`aksine) kontrol ediyorsanız bu güvenlik riskine sahip değildir. Daha fazla bilgi için bkz. [rfc7230 Section-5,4](https://tools.ietf.org/html/rfc7230#section-5.4) .

1. Sunucu düğümünün altında **uygulama havuzları**' nı seçin.

1. Sitenin uygulama havuzuna sağ tıklayın ve bağlam menüsünden **temel ayarlar** ' ı seçin.

1. **Uygulama havuzunu Düzenle** penceresinde, **.NET CLR sürümünü** **yönetilen kod olmadan**ayarlayın:

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir. ASP.NET Core, masaüstü CLR 'nin (.NET CLR) yüklenmemesine&mdash;.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı çalışan işlemde barındırmak için önyüklenir. **.NET CLR sürümünün** **yönetilen kod olmadan** ayarlanması isteğe bağlıdır, ancak önerilir.

1. *ASP.NET Core 2,2 veya üzeri*: [işlem içi barındırma modelini](#in-process-hosting-model)kullanan 64 bit (x64) [tabanlı bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) için, 32-bit (x86) işlemleri için uygulama havuzunu devre dışı bırakın.

   IIS Yöneticisi > **uygulama havuzlarının** **Eylemler** kenar çubuğunda, **uygulama havuzu varsayılanlarını** veya **Gelişmiş ayarları**ayarla ' yı seçin. **32 bitlik uygulamaları etkinleştir** ' i bulun ve değeri `False`olarak ayarlayın. Bu ayar [, işlem dışı barındırma](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)için dağıtılan uygulamaları etkilemez.

1. İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.

   Uygulama havuzunun varsayılan kimliği (**Işlem modeli** > **kimliği**), **applicationpokaydentity** 'den başka bir kimliğe değiştiyse, yeni kimliğin uygulamanın klasörüne, veritabanına ve diğer gerekli kaynaklara erişmek için gerekli izinlere sahip olduğunu doğrulayın. Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.

**Windows kimlik doğrulama yapılandırması (Isteğe bağlı)**  
Daha fazla bilgi için bkz. [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Uygulamayı [IIS sitesi oluşturma](#create-the-iis-site) bölümünde oluşturulan IIS **fiziksel yol** klasörüne dağıtın. [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtım için önerilen mekanizmadır, ancak uygulamayı projenin *Publish* klasöründen barındırma sisteminin dağıtım klasörüne taşımak için çeşitli seçenekler mevcuttur.

### <a name="web-deploy-with-visual-studio"></a>Visual Studio ile Web dağıtımı

Web Dağıtımı kullanılacak bir yayımlama profili oluşturma hakkında bilgi edinmek için bkz. [ASP.NET Core App Deployment Için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konusuna bakın. Barındırma sağlayıcısı, bir yayımlama profili veya oluşturma desteği sağlıyorsa, profilini indirin ve Visual Studio **Yayımlama** iletişim kutusunu kullanarak içeri aktarın:

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web dağıtımı Visual Studio'nun dışında

[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) , komut satırından Visual Studio dışında da kullanılabilir. Daha fazla bilgi için bkz. [Web Dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternatif Web dağıtımı

Uygulamayı barındırma sistemine taşımak için el ile kopyalama, [xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy)veya [PowerShell](/powershell/)gibi çeşitli yöntemlerden birini kullanın.

IIS 'ye dağıtım ASP.NET Core hakkında daha fazla bilgi için, [IIS yöneticileri Için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümüne bakın.

## <a name="browse-the-website"></a>Web sitesine Gözat

Uygulama barındırma sistemine dağıtıldıktan sonra, uygulamanın genel uç noktalarından birine bir istek oluşturun.

Aşağıdaki örnekte site, **bağlantı noktası** `80``www.mysite.com` IIS **ana bilgisayar adına** bağlıdır. `http://www.mysite.com`bir istek yapıldı:

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Kilitli dağıtım dosyaları

Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı. Dağıtım sırasında kilitli dosyalar üzerine yazılamaz. Kilitli dosyaları bir dağıtımda serbest bırakmak için aşağıdaki yaklaşımlardan **birini** kullanarak uygulama havuzunu durdurun:

* Proje dosyasında Web Dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` kullanın. Bir *app_offline. htm* dosyası Web uygulaması dizininin köküne yerleştirilir. Dosya olduğunda, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatır ve dağıtım sırasında *app_offline. htm* dosyasına hizmet verir. Daha fazla bilgi için [ASP.NET Core modülü yapılandırma başvurusuna](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)bakın.
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

[ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction) , kimlik doğrulamasında kullanılan ara yazılımlar dahil olmak üzere birkaç ASP.NET Core [middlewares](xref:fundamentals/middleware/index)tarafından kullanılır. Veri koruma API 'Leri Kullanıcı kodu tarafından çağrılmasa bile, kalıcı bir şifreleme [anahtarı deposu](xref:security/data-protection/implementation/key-management)oluşturmak için veri koruma bir dağıtım betiği veya Kullanıcı kodu ile yapılandırılmalıdır. Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.

Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:

* Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır. 
* Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir. 
* Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir. Bu, [CSRF belirteçlerini](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata)içerebilir.

Anahtar halkasını sürdürmek için IIS altındaki veri korumasını yapılandırmak için aşağıdaki yaklaşımlardan **birini** kullanın:

* **Veri koruma kayıt defteri anahtarları oluşturma**

  ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır. Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.

  Tek başına, Web grubu olmayan IIS yüklemeleri için [Data Protection provision-AutoGenKeys. ps1 PowerShell betiği](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) , bir ASP.NET Core uygulamasıyla kullanılan her uygulama havuzu için kullanılabilir. Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur. Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.

  Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir. Varsayılan olarak, veri koruma anahtarları şifreli değildir. Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun. X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir. Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun. Ayrıntılar için bkz. [ASP.NET Core veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) .

* **Kullanıcı profilini yüklemek için IIS uygulama havuzunu yapılandırma**

  Bu ayar, uygulama havuzunun **Gelişmiş ayarları** altındaki **işlem modeli** bölümünde bulunur. **Kullanıcı profilini** `True`için Yükle ' ye ayarlayın. `True`olarak ayarlandığında anahtarlar Kullanıcı profili dizininde depolanır ve Kullanıcı hesabına özgü bir anahtarla DPAPI kullanılarak korunur. Anahtarlar *% LocalAppData%/ASP.exe. net/DataProtection-Keys* klasörüne kalıcıdır.

  Uygulama havuzunun [Setprofileenvironment özniteliğinin](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) de etkinleştirilmesi gerekir. `setProfileEnvironment` varsayılan değeri `true`. Bazı senaryolarda (örneğin, Windows işletim sistemi), `setProfileEnvironment` `false`olarak ayarlanır. Anahtarlar beklenen şekilde Kullanıcı profili dizininde depolanmıyorsa:

  1. *% Windir%/system32/inetsrv/config* klasörüne gidin.
  1. *ApplicationHost. config* dosyasını açın.
  1. `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesini bulun.
  1. `setProfileEnvironment` özniteliğinin mevcut olmadığını, bu değerin varsayılan olarak `true`veya özniteliğin değerini `true`olarak ayarlamış olduğunu doğrulayın.

* **Dosya sistemini anahtar halka deposu olarak kullanma**

  [Dosya sistemini anahtar halka deposu olarak kullanmak](xref:security/data-protection/configuration/overview)için uygulama kodunu ayarlayın. Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun. Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.

  IIS web grubunda kullanırken:

  * Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.
  * X X509 dağıtma her makine için sertifika. [Kodda veri korumasını](xref:security/data-protection/configuration/overview)yapılandırın.

* **Veri koruma için makineye özel bir ilke ayarlama**

  Veri koruma sistemi, veri koruma API 'Lerini kullanan tüm uygulamalar için varsayılan [makine genelinde bir ilke](xref:security/data-protection/configuration/machine-wide-policy) ayarlamak için sınırlı destek içerir. Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.

## <a name="virtual-directories"></a>Sanal dizinler

[IIS sanal dizinleri](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ASP.NET Core uygulamalarla desteklenmez. Bir uygulama, bir [alt uygulama](#sub-applications)olarak barındırılabilir.

## <a name="sub-applications"></a>Alt uygulamalar

ASP.NET Core bir uygulama, bir [IIS alt uygulaması (alt uygulama)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)olarak barındırılabilir. Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.

::: moniker range="< aspnetcore-2.2"

Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir. Modül bir alt uygulamanın *Web. config* dosyasına bir işleyici olarak eklenirse, alt uygulamaya gözatmaya çalışılırken hatalı yapılandırma dosyasına başvuran *500,19 iç sunucu hatası* alınır.

Aşağıdaki örnek, bir ASP.NET Core alt uygulaması için yayımlanmış bir *Web. config* dosyası gösterir:

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

Bir ASP.NET Core uygulamasının altında bir non-ASP.NET Core alt uygulaması barındırırken, alt uygulamanın *Web. config* dosyasında devralınan işleyiciyi açıkça kaldırın:

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

Alt uygulama içindeki statik varlık bağlantıları, tilde işareti (`~/`) gösterimini kullanmalıdır. Tilde işareti gösterimi, alt uygulamanın pathbase 'i işlenmiş göreli bağlantıya eklemek için bir [etiket yardımcısını](xref:mvc/views/tag-helpers/intro) tetikler. `/subapp_path`bir alt uygulama için, `src="~/image.png"` ile bağlantılı bir görüntü `src="/subapp_path/image.png"`olarak işlenir. Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor. İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.

Statik bir varlığın `src` özniteliği mutlak bir yola ayarlandıysa (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase olmadan işlenir. Kök uygulamanın statik dosya ara yazılımı, statik varlık kök uygulamadan kullanılabilir olmadığı takdirde *404-bulunamayan* bir Yanıt ile sonuçlanır. Bu, kök uygulamanın [Web kökünden](xref:fundamentals/index#web-root)varlık sunmaya çalışır.

ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:

1. Alt uygulama için bir uygulama havuzu oluşturun. .NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı masaüstü CLR (.NET CLR) değil, çalışan işlemde barındırmak için önyüklenirken **.NET CLR sürümünü** **yönetilen kod olmadan** ayarlayın.

1. IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.

1. IIS Yöneticisi 'ndeki alt uygulama klasörüne sağ tıklayın ve **uygulamaya Dönüştür**' ü seçin.

1. **Uygulama Ekle** iletişim kutusunda, alt uygulama için oluşturduğunuz uygulama havuzunu atamak üzere **uygulama havuzunun** **Seç** düğmesini kullanın. **Tamam**’ı seçin.

Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.

İşlem içi barındırma modeli ve ASP.NET Core modülünü yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Web.config ile IIS yapılandırma

IIS yapılandırması, ASP.NET Core modüllü ASP.NET Core uygulamalar için işlevsel olan IIS senaryoları için *Web. config* 'in `<system.webServer>` bölümünden etkilenir. Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır. IIS sunucu düzeyinde dinamik sıkıştırma kullanmak üzere yapılandırıldıysa, uygulamanın *Web. config* dosyasındaki `<urlCompression>` öğesi, ASP.NET Core bir uygulama için devre dışı bırakabilir.

Daha fazla bilgi için aşağıdaki konulara bakın:

* [\<System. webServer > için yapılandırma başvurusu](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

Yalıtılmış uygulama havuzlarında çalışan ayrı uygulamalara yönelik ortam değişkenlerini ayarlamak için (IIS 10,0 veya üzeri için desteklenir), IIS başvuru belgelerindeki [ortam değişkenleri \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.

## <a name="configuration-sections-of-webconfig"></a>Web.config yapılandırma bölümlerini

*Web. config* dosyasındaki ASP.NET 4. x uygulamalarının yapılandırma bölümleri yapılandırma için ASP.NET Core uygulamalar tarafından kullanılmaz:

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır. Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Uygulama havuzları

::: moniker range=">= aspnetcore-2.2"

Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:

* İşlem içi barındırma &ndash; uygulamalar ayrı uygulama havuzlarında çalıştırmak için gereklidir.
* İşlem dışı barındırma &ndash;, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmayı öneririz.

IIS **Web sitesi ekleme** iletişim kutusu varsayılan olarak uygulama başına tek bir uygulama havuzu olur. Bir **site adı** sağlandığında, metin otomatik olarak **uygulama havuzu** metin kutusuna aktarılır. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz. IIS **Web sitesi ekleme** iletişim kutusu varsayılan olarak bu yapılandırmaya sahiptir. Bir **site adı** sağlandığında, metin otomatik olarak **uygulama havuzu** metin kutusuna aktarılır. Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.

::: moniker-end

## <a name="application-pool-identity"></a>Uygulama havuzu kimliği

Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar. IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır. Uygulama havuzunun **Gelişmiş ayarlar** altındaki IIS Yönetim Konsolu 'Nda, **kimliğin** **applicationpokaydentity**kullanılacak şekilde ayarlandığından emin olun:

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur. Bu kimlik kullanarak kaynakları güvenliği sağlanabilir. Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.

IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:

1. Windows Gezgini'ni açın ve dizine gidin.

1. Dizine sağ tıklayıp **Özellikler**' i seçin.

1. **Güvenlik** sekmesinde, **Düzenle** düğmesini ve ardından **Ekle** düğmesini seçin.

1. **Konumlar** düğmesini seçin ve sistemin seçili olduğundan emin olun.

1. **IIS AppPool\\< app_pool_name >** **Seçilecek nesne adlarını girin** alanına girin. **Adları denetle** düğmesini seçin. *DefaultAppPool* için **IIS APPPOOL\DefaultAppPool**kullanarak adları denetleyin. **Adları denetle** düğmesi seçildiğinde, nesne adları alanında bir **DefaultAppPool** değeri gösterilir. Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir. Nesne adı denetlenirken **app_pool_name > biçim\\< IIS AppPool** 'ı kullanın.

   ![Uygulama klasörü için Kullanıcı veya Grup Seç iletişim kutusu: "ad denetle" seçmeden önce nesne adları alanında "DefaultAppPool" uygulama havuzu adı "IIS AppPool\" eklenir.](index/_static/select-users-or-groups-1.png)

1. **Tamam**’ı seçin.

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. Okuma &amp; yürütme izinleri varsayılan olarak verilmelidir. Gerektiğinde ek izinler sağlar.

Erişim, **ıacl 'ler** Aracı kullanılarak bir komut isteminde de verilebilir. Örnek olarak *DefaultAppPool* kullanarak, aşağıdaki komut kullanılır:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Daha fazla bilgi için [ıcacl 'ler](/windows-server/administration/windows-commands/icacls) konusuna bakın.

## <a name="http2-support"></a>HTTP/2 desteği

::: moniker range=">= aspnetcore-2.2"

[Http/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki IIS dağıtım senaryolarında ASP.NET Core desteklenir:

* İşlem içi
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * TLS 1.2 veya sonraki bir bağlantı
* İşlem dışı
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * Herkese açık uç sunucu bağlantıları HTTP/2 kullanır, ancak [Kestrel sunucusuyla](xref:fundamentals/servers/kestrel) ters proxy bağlantısı http/1.1 kullanır.
  * TLS 1.2 veya sonraki bir bağlantı

Bir HTTP/2 bağlantısı oluşturulduğunda, bir işlem içi dağıtım için, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`. Bir HTTP/2 bağlantısı oluşturulduğunda, bir işlem dışı dağıtımı için, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/1.1`.

İşlem içi ve işlem dışı barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Http/2](https://httpwg.org/specs/rfc7540.html) , aşağıdaki temel gereksinimleri karşılayan işlem dışı dağıtımlar için desteklenir:

* Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
* Herkese açık uç sunucu bağlantıları HTTP/2 kullanır, ancak [Kestrel sunucusuyla](xref:fundamentals/servers/kestrel) ters proxy bağlantısı http/1.1 kullanır.
* Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.
* TLS 1.2 veya sonraki bir bağlantı

Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/1.1`.

::: moniker-end

HTTP/2 varsayılan olarak etkindir. Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner. IIS dağıtımları ile HTTP/2 yapılandırması hakkında daha fazla bilgi için bkz. [IIS 'de http/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

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
  1. Varsayılan **Başlangıç modu** **OnDemand**' dir. **Başlangıç modunu** **AlwaysRunning**olarak ayarlayın. **Tamam**’ı seçin.
  1. **Bağlantılar** panelinde **siteler** düğümünü açın.
  1. Uygulamaya sağ tıklayın ve **Gelişmiş ayarlar**> **Web sitesini Yönet** ' i seçin.
  1. Varsayılan **önyükleme etkin** ayarı **false**şeklindedir. **Önyükleme etkin** ' i **true**olarak ayarlayın. **Tamam**’ı seçin.

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

### <a name="idle-timeout"></a>Boşta Kalma Süresi Zaman Aşımı

*Yalnızca işlem sırasında barındırılan uygulamalar için geçerlidir.*

Uygulamanın çalışmasını engellemek için, IIS Yöneticisi 'Ni kullanarak uygulama havuzunun boşta kalma zaman aşımını ayarlayın:

1. **Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.
1. Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.
1. Varsayılan **boşta kalma süresi (dakika)** **20** dakikadır. **Boşta kalma zaman aşımını (dakika)** **0** (sıfır) olarak ayarlayın. **Tamam**’ı seçin.
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

* [IIS belgeleri](/iis)
* [IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [.NET Core uygulama dağıtımı](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot>
* <xref:index>
* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Windows Server teknik içerik kitaplığı](/windows-server/windows-server)
* [IIS üzerinde HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
