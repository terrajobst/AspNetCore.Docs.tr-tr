---
title: "Web server ASP.NET Core uygulamalarında"
author: tdykstra
description: "Web sunucuları Kestrel ve WebListener için ASP.NET Core tanıtır. Birini konusunda rehberlik sağlar ve ne zaman bir ters proxy sunucusu ile kullanılır."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 807e60e61d4ce4d5755987cffe65d130c9bbbd42
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Web server ASP.NET Core uygulamalarında

Tarafından [zel Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris fillerin](https://github.com/Tratcher)

Bir ASP.NET Core uygulama işlem içi HTTP sunucusu uygulamasını ile çalışır. HTTP istekleri ve bunları uygulamasına kümesi olarak ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) içine oluşan bir `HttpContext`.

ASP.NET Core iki sunucu uygulamaları gelir:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) platformlar arası HTTP sunucusu dayanır [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı.

* [HTTP.sys](httpsys.md) yalnızca Windows HTTP sunucu dayanır [Http.Sys çekirdek sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) platformlar arası HTTP sunucusu dayanır [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı.

* [WebListener](weblistener.md) yalnızca Windows HTTP sunucu dayanır [Http.Sys çekirdek sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>kestrel

Kestrel varsayılan ASP.NET Core yeni proje şablonları olarak dahil edilen web sunucusudur. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma &mdash; ile veya bir ters Ara sunucu olmadan &mdash; Kestrel yalnızca bir iç ağa açılırsa de kullanılabilir.

Ne zaman bir ters proxy ile Kestrel kullanılacağı hakkında daha fazla bilgi için bkz: [Kestrel giriş](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Uygulamanızı bir iç ağ yalnızca gelen istekleri kabul ederse, tek başına Kestrel kullanabilirsiniz.

![Kestrel, iç ağınız ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

Uygulamanızı internet kullanıma sunma, IIS, Nginx ya da Apache olarak kullanmalısınız bir *ters proxy sunucu*. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra aşağıdaki çizimde gösterildiği gibi iletir.

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Güvenlik (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu kullanmak için en önemli nedenidir. Kestrel 1.x sürümleri tamamlayıcı saldırılarına karşı savunma yoktur. Bu, içerir, ancak uygun için sınırlı zaman aşımları, boyut sınırları ve eşzamanlı bağlantı sınırlarını değil.

Ne zaman bir ters proxy ile Kestrel kullanılacağı hakkında daha fazla bilgi için bkz: [Kestrel giriş](kestrel.md).

---

IIS, Nginx ya da Apache Kestrel olmadan kullanamazsınız veya [özel sunucu uygulaması](#custom-servers). ASP.NET Core, böylece platformlar genelinde tutarlı bir şekilde davranabilir kendi işleminde çalışmak üzere tasarlanmıştır. IIS, Nginx ve Apache kendi başlatma işlemi ve ortam dikte; bunları kullanmak için doğrudan ASP.NET Core her biri gereksinimlerine uyarlamak gerekir. Bir web sunucusu uygulaması Kestrel gibi kullanarak ASP.NET Core üzerinde denetim başlatma işlemi ve ortam sağlar. Bu nedenle ASP.NET Core IIS, Nginx ya da Apache uyarlamak çalışırken yerine yalnızca bu web sunucularının Kestrel proxy istekleri için ayarlarsınız. Bu düzenlemenin sağlar, `Program.Main` ve `Startup` temelde dağıttığınız nerede olursa olsun aynı olması için sınıflar.

### <a name="iis-with-kestrel"></a>IIS Kestrel ile

ASP.NET Core için ters proxy IIS veya IIS Express kullandığınızda, ASP.NET Core uygulama ayrı bir işlemde için IIS çalışan işlemini çalıştırır. Ters proxy ilişki koordine etmek için özel bir IIS modül IIS işleminde çalışır.  Bu *ASP.NET Core Modülü*. Birincil ASP.NET Core modülünün ASP.NET Core uygulamayı başlatmak, onu kilitlendiğinde yeniden başlatın ve HTTP trafiği iletmek için işlevlerdir. Daha fazla bilgi için bkz: [ASP.NET Core Modülü](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx Kestrel ile

Nginx Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Nginx ile Linux konakta](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Kestrel Apache

Apache Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Linux Apache ile konakta](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core uygulamanızı Windows'ta çalıştırma, HTTP.sys alternatif Kestrel olur. HTTP.sys burada uygulamanızı internet kullanıma ve Kestrel desteklemiyor HTTP.sys özelliklerine ihtiyaç senaryoları için kullanabilirsiniz. 

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

HTTP.sys, yalnızca bir iç ağ için sunulan uygulamaları için de kullanılabilir. 

![HTTP.sys, iç ağınız ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

İç ağ senaryoları için Kestrel genellikle en iyi performans için önerilir; Ancak, bazı senaryolarda, yalnızca HTTP.sys sağlayan bir özellik kullanmak isteyebilirsiniz. HTTP.sys özellikler hakkında daha fazla bilgi için bkz: [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys ASP.NET Core WebListener adlı 1.x. Windows, WebListener uygulama için kullanabileceğiniz bir alternatiftir, ASP.NET Core çalıştırırsanız Internet ancak uygulamanıza kullanıma sunmak istediğiniz senaryolar IIS kullanamazsınız.

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

Kestrel desteklemiyor WebListener özelliklerine gereksinim duyarsanız WebListener de Kestrel yerine yalnızca bir iç ağa, sunulan uygulamaları için kullanılabilir. 

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

İç ağ senaryoları için Kestrel genellikle en iyi performans için önerilir, ancak bazı senaryolarda yalnızca WebListener sağlayan bir özellik kullanmak isteyebilirsiniz. WebListener özellikleri hakkında daha fazla bilgi için bkz: [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>ASP.NET Core sunucu altyapısı ile ilgili notlar

[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) Bulunan `Startup` sınıfı `Configure` yöntemi çıkarır `ServerFeatures` türündeki özelliği [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel ve WebListener kullanıma yalnızca tek bir özellik olan [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı sunucu uygulamaları ek işlevsellik kullanıma.

`IServerAddressesFeature`Sunucu uygulaması çalışma zamanında bağlı hangi bağlantı noktasının öğrenmek için kullanılabilir.

## <a name="custom-servers"></a>Özel sunucuları

Yerleşik sunucuları ihtiyaçlarınızı karşılamıyorsa, özel sunucu uygulaması oluşturabilirsiniz. [.NET (OWIN) kılavuzu için açık Web arabirimi](../owin.md) nasıl yazılacağını göstermektedir bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması. En azından desteklemeniz gereken ancak yalnızca uygulamanız gereken, özellik arabirimleri uygulamak ücretsiz [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [IIS ile kestrel](aspnet-core-module.md)
- [Nginx ile Linux’ta barındırma](xref:host-and-deploy/linux-nginx)
- [Apache ile Linux’ta barındırma](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [IIS ile kestrel](aspnet-core-module.md)
- [Nginx ile Linux’ta barındırma](xref:host-and-deploy/linux-nginx)
- [Apache ile Linux’ta barındırma](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
