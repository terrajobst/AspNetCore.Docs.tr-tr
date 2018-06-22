---
title: Web server ASP.NET Core uygulamalarında
author: rick-anderson
description: Kestrel ve HTTP.sys web sunucuları için ASP.NET Core bulur. Bir sunucu seçmek nasıl ve ne zaman bir ters proxy sunucusu kullanmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: bb0331d7201d4e979e6c6524cbf630280c4eaeb6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274449"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Web server ASP.NET Core uygulamalarında

Tarafından [zel Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris fillerin](https://github.com/Tratcher)

Bir ASP.NET Core uygulama işlem içi HTTP sunucusu uygulamasını ile çalışır. HTTP istekleri ve bunları uygulamaya kümesi ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir [HttpContext](/dotnet/api/system.web.httpcontext).

ASP.NET Core iki sunucu uygulamaları gelir:

* [Kestrel](xref:fundamentals/servers/kestrel) platformlar arası HTTP sunucusu, varsayılan ASP.NET çekirdeği için olur.
* [HTTP.sys](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP sunucu API'sini](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) ASP.NET Core içinde 1.x.)

## <a name="kestrel"></a>kestrel

Kestrel ASP.NET Core proje şablonlarını dahil varsayılan web sunucusudur.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tek başına veya birlikte kestrel kullanılabilir bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma&mdash;ile veya bir ters Ara sunucu olmadan&mdash;geçerli ve desteklenen bir barındırma yapılandırması ASP.NET Core 2.0 veya sonraki uygulamalar içindir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Uygulamayı yalnızca bir iç ağ gelen istekleri kabul ederse, Kestrel kendisi tarafından kullanılabilir.

![Kestrel iç ağ ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

Uygulamanın Internet'e açık değilse, IIS, Nginx ya da Apache olarak Kestrel kullanmalısınız bir *ters proxy sunucu*. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra aşağıdaki çizimde gösterildiği gibi iletir:

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Güvenlik (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu kullanmak için en önemli nedenidir. Kestrel 1.x sürümleri Internet üzerinden saldırılara karşı korumak için önemli güvenlik özellikleri yok. Bu, içerir, ancak sınırlı, uygun zaman aşımı, istek boyutu sınırları ve eş zamanlı bağlantı sınırlarını değil.

Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

IIS, Nginx ve Apache Kestrel kullanılamaz veya [özel sunucu uygulaması](#custom-servers). ASP.NET Core, böylece platformlar genelinde tutarlı bir şekilde davranabilir kendi işleminde çalışmak üzere tasarlanmıştır. IIS, Nginx ve Apache kendi başlangıç yordamı ve ortam dikte. Bu sunucu teknolojileri kullanmak için doğrudan ASP.NET Core her sunucu gereksinimlerine uyarlamak gerekir. Kestrel gibi bir web sunucusu uygulaması kullanarak ASP.NET Core farklı sunucu teknolojileri üzerinde barındırılan ortam ve başlatma işlemi denetime sahiptir.

### <a name="iis-with-kestrel"></a>IIS Kestrel ile

Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) için IIS çalışan işlemini ayrı bir işlemde ASP.NET Core uygulama ASP.NET Core için ters Ara sunucu çalışır. IIS işleminde [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ters proxy ilişki düzenler. Birincil ASP.NET Core modülünün ASP.NET Core uygulamayı başlatın, onu kilitlendiğinde uygulamayı yeniden başlatın ve uygulamayı HTTP trafiği iletmek için işlevlerdir. Daha fazla bilgi için bkz: [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx Kestrel ile

Nginx Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Nginx ile Linux konakta](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Kestrel Apache

Apache Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Linux Apache ile konakta](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core uygulamaları Windows çalıştırırsanız, HTTP.sys bir Kestrel alternatifidir. Kestrel genellikle en iyi performans için önerilir. HTTP.sys, burada uygulama Internet erişimine açıktır ve gerekli özellikleri HTTP.sys ancak değil Kestrel tarafından desteklenen senaryolarda kullanılabilir. HTTP.sys özellikleri hakkında daha fazla bilgi için bkz: [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

HTTP.sys, yalnızca bir iç ağ için sunulan uygulamaları için de kullanılabilir. 

![HTTP.sys iç ağ ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys adlandırılan [WebListener](xref:fundamentals/servers/weblistener) ASP.NET Core içinde 1.x. ASP.NET Core uygulamaları Windows çalıştırırsanız WebListener IIS burada ana bilgisayar uygulamaları için kullanılabilir değil senaryoları için alternatiftir.

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

WebListener de Kestrel yerine yalnızca bir iç ağ için sunulan uygulamaları için WebListener ancak değil Kestrel tarafından desteklenen özellikler gerekli olursa kullanılabilir. WebListener özellikleri hakkında daha fazla bilgi için bkz: [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener iç ağ ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core sunucu altyapısı

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) bulunan `Startup.Configure` yöntemi çıkarır [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) türündeki özelliği [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel ve HTTP.sys (ASP.NET Core WebListener 1.x) yalnızca tek bir özelliği her kullanıma [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı sunucu uygulamaları ek işlevsellik kullanıma.

`IServerAddressesFeature` Sunucu uygulaması çalışma zamanında bağlı hangi bağlantı noktasının öğrenmek için kullanılabilir.

## <a name="custom-servers"></a>Özel sunucuları

Yerleşik sunucuları uygulamanın gereksinimlerini karşılamıyorsa, özel sunucu uygulaması oluşturulabilir. [.NET (OWIN) kılavuzu için açık Web arabirimi](xref:fundamentals/owin) nasıl yazılacağını göstermektedir bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması. En az olsa uygulaması, uygulamanın kullandığı özelliği arabirimleri gerektiren yalnızca [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) desteklenmesi gerekir.

## <a name="server-startup"></a>Sunucu başlatma

Kullanırken [Visual Studio](https://www.visualstudio.com/vs/), [Mac için Visual Studio](https://www.visualstudio.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/), uygulama tarafından tümleşik geliştirme ortamı (olarak başlatıldığında sunucu başlatılır IDE). Windows Visual Studio'da, başlatma profilleri uygulama ve sunucu ile ya da başlatmak için kullanılabilir [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) veya Konsolu. Visual Studio Code uygulama ve sunucu tarafından başlatılan [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), CoreCLR hata ayıklayıcı etkinleştirir. Mac için Visual Studio kullanarak uygulama ve sunucu tarafından başlatılan [Mono yumuşak modu hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Proje klasöründeki bir komut isteminden bir uygulama başlatılırken [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) uygulama ve sunucu (Kestrel ve yalnızca HTTP.sys) başlatır. Yapılandırması tarafından belirtilen `-c|--configuration` ayarlandığından seçeneği `Debug` (varsayılan) veya `Release`. Başlatma profilleri varsa bir *launchSettings.json* dosya, kullanın `--launch-profile <NAME>` başlatma profili ayarlama seçeneği (örneğin, `Development` veya `Production`). Daha fazla bilgi için bkz: [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) ve [.NET Core dağıtım paketleme](/dotnet/core/build/distribution-packaging) Konular.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kestrel](xref:fundamentals/servers/kestrel)
* [IIS ile kestrel](xref:fundamentals/servers/aspnet-core-module)
* [Nginx ile Linux’ta barındırma](xref:host-and-deploy/linux-nginx)
* [Apache ile Linux’ta barındırma](xref:host-and-deploy/linux-apache)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET 1.x çekirdek bkz [WebListener](xref:fundamentals/servers/weblistener))
