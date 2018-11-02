---
title: ASP.NET Core Web sunucu uygulamalarında
author: rick-anderson
description: ASP.NET Core için web sunucuları Kestrel ve HTTP.sys keşfedin. Bir sunucu seçin ve ne zaman bir ters proxy sunucusu kullanmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 6b6ebbe9d31d571ea470fba0989d622dcf6e68af
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758212"
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core Web sunucu uygulamalarında

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris Ross](https://github.com/Tratcher)

ASP.NET Core uygulaması bir işlemde HTTP sunucusu uygulamasını ile çalışır. HTTP istekleri ve bunları uygulamaya kümesi ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir <xref:Microsoft.AspNetCore.Http.HttpContext>.

ASP.NET Core aşağıdaki sunucu uygulamaları ile birlikte gelir:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel'i](xref:fundamentals/servers/kestrel) platformlar arası bir HTTP sunucusu, varsayılan ASP.NET Core için olan.
* `IISHttpServer` ile kullanılan [işlem içi barındırma modeli](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Windows üzerinde.
* [HTTP.sys](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [Kestrel'i](xref:fundamentals/servers/kestrel) platformlar arası bir HTTP sunucusu, varsayılan ASP.NET Core için olan.
* [HTTP.sys](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.)

::: moniker-end

## <a name="kestrel"></a>Kestrel'i

Kestrel'i ASP.NET Core proje şablonları dahil varsayılan web sunucusudur.

::: moniker range=">= aspnetcore-2.0"

Tek başına veya birlikte kestrel kullanılabilir bir *ters Ara sunucu*IIS, Ngınx veya Apache gibi. Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Uygulamayı yalnızca bir iç ağ gelen istekleri kabul ederse Kestrel kendisi tarafından kullanılabilir.

![Kestrel'i iç ağa ile doğrudan iletişim kurar.](kestrel/_static/kestrel-to-internal.png)

Uygulamayı Internet erişimine açıktır, IIS, Ngınx veya Apache olarak Kestrel kullanmalısınız bir *ters Ara sunucu*. Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alıp Kestrel için aşağıdaki diyagramda gösterildiği gibi bazı ön işleme sonra ileten:

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Internet güvenlik olduğundan doğrudan sunulan genel kullanıma yönelik uç sunucusu dağıtımları için ters Ara sunucu kullanmak için en önemli nedeni. Kestrel'i 1.x sürümlerini Internet'ten saldırılarına karşı korumak için önemli güvenlik özelliklerine sahip değilsiniz. Bu, içerir, ancak bunlarla sınırlı uygun bir zaman aşımı, istek boyutu sınırları ve eş zamanlı bağlantı sınırları değildir.

Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

IIS, Ngınx ve Apache Kestrel kullanılamaz veya [özel sunucu uygulaması](#custom-servers). ASP.NET Core platformlar arasında tutarlı bir şekilde davranabilir böylece kendi işlem içinde çalıştırmak için tasarlanmıştır. IIS, Ngınx ve Apache kendi başlangıç yordamı ve ortam gerektirir. Bu sunucu teknolojileri kullanmak için doğrudan, ASP.NET Core her bir sunucunun gereksinimlerine uyum sağlamak gerekir. Kestrel'i gibi bir web sunucusu uygulaması kullanarak ASP.NET Core başlatma işlemi ve farklı sunucu teknolojileri üzerinde barındırıldığında ortam üzerinde denetimi yoktur.

### <a name="iis-with-kestrel"></a>IIS ile Kestrel

::: moniker range=">= aspnetcore-2.2"

Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), ASP.NET Core uygulaması ya da IIS çalışan işlemi ile aynı işlemde çalışan ( *işlem içi* barındırma modeli) veya ayrı bir işlemde IIS çalışan işlemi ( *işlem dışı* barındırma modeli).

[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlem içi IIS Http sunucusu ya da işlem dışı Kestrel sunucusu arasında yerel IIS istekleri işleyen yerel bir IIS modülüdür. Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) IIS çalışan işleminden ayrı bir işlemde ASP.NET Core uygulaması ASP.NET Core için ters Ara sunucu çalışır. IIS işleminde [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ters proxy ilişki düzenler. ASP.NET Core modülü temel işlevleri, ASP.NET Core uygulaması başlatın, bu kilitlendiğinde uygulamayı yeniden başlatın ve uygulama HTTP trafiği ilettiği üzeresiniz. Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

### <a name="nginx-with-kestrel"></a>Ngınx Kestrel ile

Ngınx Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Nginx ile Linux'ta barındırma](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Kestrel'i Apache

Apache Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Apache ile Linux'ta barındırma](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Windows üzerinde çalışan ASP.NET Core uygulamaları, HTTP.sys Kestrel alternatif olur. Kestrel'i genellikle en iyi performans için önerilir. HTTP.sys burada uygulama Internet erişimine açıktır ve gerekli özellikleri HTTP.sys ancak değil Kestrel tarafından desteklenen senaryolarda kullanılabilir. HTTP.sys hakkında daha fazla bilgi için bkz: [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys doğrudan Internet ile iletişim kurar.](httpsys/_static/httpsys-to-internet.png)

HTTP.sys, yalnızca iç ağa sunulan uygulamalar için de kullanılabilir.

![HTTP.sys iç ağa ile doğrudan iletişim kurar.](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x. Windows üzerinde ASP.NET Core uygulamaları çalıştırırsanız, WebListener burada IIS ana bilgisayar uygulamaları için kullanılabilir senaryolar için alternatiftir.

![Weblistener doğrudan Internet ile iletişim kurar.](weblistener/_static/weblistener-to-internet.png)

WebListener de Kestrel yerine yalnızca bir iç ağ için sunulan uygulamalar için özellikleri WebListener ancak değil Kestrel tarafından desteklenen gerekirse kullanılabilir. WebListener hakkında daha fazla bilgi için bkz: [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener iç ağa ile doğrudan iletişim kurar.](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core sunucu altyapısı

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) bulunan `Startup.Configure` yöntemi kullanıma sunan [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) türünün özelliği [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel ve HTTP.sys (WebListener içinde ASP.NET Core 1.x) yalnızca tek bir özelliği her kullanıma [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı bir sunucu uygulamaları, ek işlevler sunarsınız.

`IServerAddressesFeature` Sunucu Uygulama çalışma zamanında bağlı bağlantı noktasını bulmak için kullanılabilir.

## <a name="custom-servers"></a>Özel sunucuları

Özel sunucu uygulaması, yerleşik sunucuları uygulamanın gereksinimleri karşılamıyorsanız oluşturulabilir. [.NET (OWIN) kılavuzu için açık Web arabirimi](xref:fundamentals/owin) nasıl yazılacağını gösteren bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması. Uygulamanın kullandığı özellik arabirimler uygulama, en az olsa gerektirir. yalnızca [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) desteklenmesi gerekir.

## <a name="server-startup"></a>Sunucu başlangıç

Kullanırken [Visual Studio](https://www.visualstudio.com/vs/), [Mac için Visual Studio](https://www.visualstudio.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/), sunucu uygulama tarafından tümleşik geliştirme ortamı (olarak başlatıldığında başlatılır IDE). Windows üzerinde Visual Studio'da başlatma profilleri uygulama ve sunucu ile başlatmak için kullanılabilir [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) veya Konsolu. Visual Studio Code'da uygulama ve sunucu tarafından başlatılan [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), CoreCLR hata ayıklayıcı etkinleştirir. Mac için Visual Studio kullanarak, uygulama ve sunucu tarafından başlatılan [Mono modu geçici hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Proje klasöründeki bir komut isteminden bir uygulamayı başlatırken [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulama ve sunucu (Kestrel ve yalnızca HTTP.sys) başlatır. Yapılandırması tarafından belirtilen `-c|--configuration` ayarlandığından seçeneği `Debug` (varsayılan) veya `Release`. Başlatma profili varsa bir *launchSettings.json* dosya, kullanın `--launch-profile <NAME>` başlatma profili ayarlamak için seçeneği (örneğin, `Development` veya `Production`). Daha fazla bilgi için [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) ve [.NET Core dağıtımı paketleme](/dotnet/core/build/distribution-packaging) konuları.

## <a name="http2-support"></a>HTTP/2 desteği

[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki dağıtım senaryolarında desteklenir:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * İşletim sistemi
    * Windows Server 2012 R2/Windows 8.1 veya üzeri
    * Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)
    * HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.
  * Hedef çerçeve: .NET Core 2.2 veya üzeri
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 veya üzeri
  * Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.
* [IIS (işlem içi)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * Hedef çerçeve: .NET Core 2.2 veya üzeri
* [IIS (giden işlem)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * HTTP/2 genel kullanıma yönelik uç sunucu bağlantılarını kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.
  * Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 veya üzeri
  * Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.
* [IIS (giden işlem)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri
  * HTTP/2 genel kullanıma yönelik uç sunucu bağlantılarını kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.
  * Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.

::: moniker-end

Bir HTTP/2 bağlantı kullanmalısınız [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) ve TLS 1.2 veya sonraki bir sürümü. Daha fazla bilgi için sunucu dağıtım senaryolarınız için ilgili konulara bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x sürümüne bkz <xref:fundamentals/servers/weblistener>)
