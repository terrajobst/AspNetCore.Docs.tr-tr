---
title: "ASP.NET çekirdeği Modülü"
author: tdykstra
description: "ASP.NET çekirdeği Modülü (ANCM), IIS veya IIS Express ters proxy sunucusu olarak kullanacak Kestrel web sunucusu olanak sağlayan bir IIS Modülü tanıtır."
keywords: "ASP.NET Core, IIS, IIS Express,ASP.NET çekirdek modülü, UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d1f551dbde5f3dd6e71808154c2e5885d588d7c
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET çekirdeği modülü için giriş

Tarafından [zel Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris fillerin](https://github.com/Tratcher) 

ASP.NET çekirdeği Modülü (ANCM) ASP.NET Core uygulamaları IIS arkasında çalıştırmak olanak tanır (güvenlik, yönetilebilirlik ve çok daha fazla) iyi nedir için IIS kullanarak ve kullanarak [Kestrel](kestrel.md) (gerçekten hızlı olmasının en), iyi nedir için ve alma aynı anda hem teknolojilerden avantajları. **ANCM yalnızca Kestrel ile çalışır; WebListener ile uyumlu değil (ASP.NET Core içinde 1.x) veya HTTP.sys (içinde 2.x).** 

Desteklenen Windows sürümlerine:

* Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>ASP.NET çekirdeği Modülü'ne

ANCM IIS ardışık düzenine kanca oluşturur ve ASP.NET Core uygulama arka ucuna trafiğini yönlendiren yerel bir IIS modüldür. Windows kimlik doğrulaması gibi birçok diğer modüller, hala çalıştırmak için bir fırsat alın. ANCM istek için bir işleyici seçildiğinde ve işleyici eşlemesi uygulamada tanımlı denetimi yalnızca sürer *web.config* dosya.

ASP.NET Core uygulamaları bir işlem olarak çalıştırmak için IIS çalışan işlemini ayrı olduğundan ANCM yönetim işlem. İlk istek geldiğinde ve onu kilitlendiğinde yeniden başlatıldığında ANCM ASP.NET Core uygulama işlemini başlatır. Klasik ASP.NET uygulamaları temelde aynı davranışı budur çalışan işlem içi IIS'de ve WAS (Windows Etkinleştirme hizmeti) tarafından yönetilir.

IIS, ANCM ve ASP.NET Core uygulamaları arasındaki ilişkiyi gösteren diyagram aşağıdadır.

![ASP.NET çekirdeği Modülü](aspnet-core-module/_static/ancm.png)

İstekleri Web sunucusundan gelen ve bunları birincil bağlantı noktası (80) veya SSL bağlantı noktası (443) üzerinde IIS içine yönlendiren çekirdek modu Http.Sys sürücüsünü ulaştı. ANCM ASP.NET Core uygulama 80/443 numaralı bağlantı noktası değil uygulama için yapılandırılan HTTP bağlantı noktası isteklerini iletir.

ANCM gelen trafik için kestrel dinler.  ANCM başlangıçta ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [UseIISIntegration](#call-useiisintegration) yöntemi yapılandırır üzerinde dinlemek üzere `http://localhost:{port}`. ANCM değil, istekleri reddedecek şekilde ek denetimler vardır. (HTTPS üzerinden IIS tarafından alınan olsa bile istekleri HTTP üzerinden iletilir şekilde ANCM HTTPS iletme'yi desteklemiyor.)

Kestrel ANCM istekleri seçer ve bunları daha sonra bunları işler ve bunları olarak geçirdiği ASP.NET Core ara yazılım ardışık düzenini içine iter `HttpContext` uygulama mantığını örneklerine. Uygulamanın yanıtları sonra IIS, isteklerin başlatılan HTTP istemcisi geri hangi iter dön geçirilir.

ANCM birkaç diğer işlevleri de vardır:

* Ortam değişkenlerini ayarlar.
* Günlükleri `stdout` dosya depolama alanına çıktı.
* Windows kimlik doğrulama belirteçleri iletir.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>ASP.NET Core uygulamaları ANCM kullanma

Bu bölümde, bir IIS sunucusu ve ASP.NET Core uygulama ayarlama işlemine genel bakış sağlar. Ayrıntılı yönergeler için bkz: [IIS yayımlama](../../publishing/iis.md).

### <a name="install-ancm"></a>ANCM yükleyin


ASP.NET çekirdeği modülü IIS sunucularınızda ve IIS Express geliştirme makinelerde yüklü gerekir. Sunucular için ANCM dahil [.NET Core Windows Server barındırma paket](https://aka.ms/dotnetcore-2-windowshosting). Makinede zaten yüklü geliştirme makineler için Visual Studio otomatik olarak ANCM IIS ve IIS Express de yükler.

### <a name="install-the-iisintegration-nuget-package"></a>IISIntegration NuGet paketini yükleyin

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) paketinde ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) ve [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Metapackages birini kullanmıyorsanız, yükleme `Microsoft.AspNetCore.Server.IISIntegration` ayrı olarak. `IISIntegration` Uygulamanızı ayarlayın ANCM tarafından yayınlanan ortam değişkenleri okur birlikte çalışabilirlik paketi paketidir. Ortam değişkenleri Dinlemenin yapılacağı bağlantı noktası gibi yapılandırma bilgilerini sağlar. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Uygulamanızda yüklemek [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). `IISIntegration` Uygulamanızı ayarlayın ANCM tarafından yayınlanan ortam değişkenleri okur birlikte çalışabilirlik paketi paketidir. Ortam değişkenleri Dinlemenin yapılacağı bağlantı noktası gibi yapılandırma bilgilerini sağlar. 

---

### <a name="call-useiisintegration"></a>Çağrı UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

`UseIISIntegration` Genişletme yöntemi [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) IIS ile çalıştırdığınızda otomatik olarak çağrılır.

ASP.NET Core metapackages birini kullanmadığınız ve yüklemediniz `Microsoft.AspNetCore.Server.IISIntegration` paketi, bir çalışma zamanı hatası alın. Çağırırsanız `UseIISIntegration` paketi yüklü değilse açıkça bir derleme zamanı hatası alın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Uygulamanızın içinde `Main` yöntemi, çağrı `UseIISIntegration` genişletme yöntemi [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` Yöntemi ANCM ayarlar ortam değişkenleri ve onu görünen no-ops bulunamazsa değil. Bu davranış, geliştirme ve macOS veya Linux'ta test etme ve IIS çalıştıran bir sunucuya dağıtma gibi senaryoları kolaylaştırır. MacOS ya da Linux üzerinde çalışırken, Kestrel web sunucusu gibi davranan; Ancak uygulama IIS ortamına dağıtıldığında, otomatik olarak ANCM ve IIS kullanır.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Diğer bağlantı noktası bağlamaları ANCM bağlantı noktası bağlama geçersiz kılar

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

ANCM arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `UseIISIntegration` Yöntemi bu dinamik bir bağlantı noktası seçer ve Dinlemenin yapılacağı Kestrel yapılandırır `http://locahost:{dynamicPort}/`. Bu çağrı gibi diğer URL yapılandırmaları geçersiz kılar `UseUrls` veya [Kestrel'ın dinleme API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Bu nedenle, çağrı gerekmez `UseUrls` veya Kestrel'ın `Listen` ANCM kullandığınızda API. Çağırırsanız `UseUrls` veya `Listen`, Kestrel uygulama IIS olmadan çalıştırdığınızda, belirttiğiniz bağlantı noktasında dinler.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

ANCM arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur. `UseIISIntegration` Yöntemi bu dinamik bir bağlantı noktası seçer ve Dinlemenin yapılacağı Kestrel yapılandırır `http://locahost:{dynamicPort}/`. Bu çağrı gibi diğer URL yapılandırmaları geçersiz kılar `UseUrls`. Bu nedenle, çağrı gerekmez `UseUrls` ANCM kullandığınızda. Çağırırsanız `UseUrls`, Kestrel uygulama IIS olmadan çalıştırdığınızda, belirttiğiniz bağlantı noktasında dinler.

ASP.NET Core 1.0 çağırırsanız, `UseUrls`, çağrısından **önce** çağırmanız `UseIISIntegration` böylece ANCM yapılandırılan bağlantı noktası üzerine değildir. ANCM ayarı geçersiz kıldığından bu arama sırası ASP.NET Core 1.1 gerekli olmadığından `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Web.config dosyasında ANCM seçeneklerini yapılandırma

ASP.NET çekirdeği modülü için yapılandırması depolanır *Web.config* uygulamanın kök klasöründe yer alan dosya. Noktası başlangıç komut ve ASP.NET Core uygulamanızı başlatmak bağımsız değişkenler için bu dosyadaki ayarlar. Örnek Web.config kod ve yapılandırma seçenekleri hakkında yönergeler için bkz: [ASP.NET temel modül yapılandırma başvurusu](../../hosting/aspnet-core-module.md).

### <a name="run-with-iis-express-in-development"></a>IIS Express ile geliştirme çalıştırın

IIS Express ASP.NET Core şablonları tarafından tanımlanan varsayılan profili kullanarak Visual Studio tarafından başlatılabilir.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>HTTP protokolünü ve bir eşleşme belirteci proxy yapılandırması kullanır

ANCM arasında Kestrel oluşturulan proxy HTTP protokolünü kullanır. HTTP performansı en iyi duruma getirme ANCM ve Kestrel arasındaki trafiğin geri döngü adresine dışına ağ arabirimi gerçekleştiği kullanmaktır. Hiçbir riski ANCM ve sunucu dışına bir konumdan Kestrel arasındaki trafiğin gizli dinleme yoktur.

Bir eşleşme belirteci Kestrel tarafından alınan isteği IIS tarafından yönlendirilirken ve bazı başka bir kaynaktan geliyor kaydetmedi güvence altına almak için kullanılır. Eşleştirme belirteci oluşturulur ve bir ortam değişkeni ayarlayın (`ASPNETCORE_TOKEN`) ANCM tarafından. Eşleştirme belirteci de bir üstbilgisine ayarlayın (`MSAspNetCoreToken`) yönlendirilirken her istekte. IIS Ara denetimleri eşleştirme belirteci üstbilgi değeri ortam değişkeni değeri ile eşleşen onaylamak için aldığı isteyin. Belirteç değerleri eşleşirse, isteği günlüğe ve reddetti. Eşleştirme belirteci ortam değişkenine ve ANCM ve Kestrel arasındaki trafiğin sunucunun dışına bir konumdan erişilebilir değil. Eşleştirme belirteç değeri bilmeden bir saldırgan IIS Ara yazılımında denetimini atla istek gönderemez.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bu makale için örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET çekirdeği modülü kaynak kodu](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core modül yapılandırma başvurusu](../../hosting/aspnet-core-module.md)
* [IIS yayımlama](../../publishing/iis.md)
