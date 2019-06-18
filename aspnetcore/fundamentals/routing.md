---
title: ASP.NET Core yönlendirme
author: rick-anderson
description: Nasıl ASP.NET Core yönlendirme uç noktaları için eşleme isteği için uç nokta seçici bir URI'leri ve dispatching gelen istekleri sorumludur keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: fundamentals/routing
ms.openlocfilehash: 71cb7215651a263e588531c9be644326c0b6eda6
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167091"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

Bu konuda 1.1 sürümü için indirme [ASP.NET Core (sürüm 1.1, PDF) yönlendirme](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Yönlendirme uç noktaları için eşleme isteği için uç nokta seçici bir URI'leri ve dispatching gelen istekleri sorumludur. Yollar uygulamada tanımlı ve uygulama başlatıldığında yapılandırılmış. Bir rota isteğe bağlı olarak istekte bulunan URL'den değerleri ayıklayabilir ve bu değerler daha sonra istek işleme için kullanılabilir. Uygulamadan rota bilgilerini kullanarak, yönlendirme de için uç nokta Seçici eşleyen URL üretmek kuramıyor.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

ASP.NET Core 2.2 içinde son Yönlendirme senaryoları kullanmak için belirtin [uyumluluk sürümü](xref:mvc/compatibility-version) için MVC, kayıt hizmetleri `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> Seçeneği yönlendirme dahili uç nokta tabanlı mantıksal kullanması gerekip gerekmediğini belirler veya <xref:Microsoft.AspNetCore.Routing.IRouter>-ASP.NET Core 2.1 veya daha önce mantığı. Uyumluluk sürümü 2.2 veya üzeri olarak ayarlandığında, varsayılan değer: `true`. Değerine `false` önceki yönlendirme mantığı kullanmak için:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Daha fazla bilgi için <xref:Microsoft.AspNetCore.Routing.IRouter>-yönlendirme bağlı olarak, bkz: [ASP.NET Core 2.1 sürümü, bu konunun](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yönlendirme, yol işleyicisi ve bir gelen istekleri gönderme eşleştirme isteğinde URI'ler için sorumludur. Yollar uygulamada tanımlı ve uygulama başlatıldığında yapılandırılmış. Bir rota isteğe bağlı olarak istekte bulunan URL'den değerleri ayıklayabilir ve bu değerler daha sonra istek işleme için kullanılabilir. Yapılandırılan yollar uygulamasından kullanarak yönlendirme rota işleyicilerine eşleyen URL üretmek kuramıyor.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

ASP.NET Core 2.1 son Yönlendirme senaryoları kullanmak için belirtin [uyumluluk sürümü](xref:mvc/compatibility-version) için MVC, kayıt hizmetleri `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

::: moniker-end

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirme kapsar. ASP.NET Core MVC yönlendirme hakkında daha fazla bilgi için bkz: <xref:mvc/controllers/routing>. Razor sayfaları yönlendirme kuralları hakkında daha fazla bilgi için bkz: <xref:razor-pages/razor-pages-conventions>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme Temelleri

Çoğu uygulama, URL'leri okunaklı ve anlamlı olacak şekilde basit ve açıklayıcı bir yönlendirme düzeni seçmeniz gerekir. Varsayılan geleneksel yolu `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* Kullanıcı Arabirimi tabanlı uygulamalar için kullanışlı bir başlangıç noktası var.

Geliştiriciler genellikle ekleme ek kısa yolları yüksek trafik alanlara kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç) bir uygulamanın [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya Geleneksel rotaları dedicated.

Web API'leri öznitelik yönlendirme uygulamanın işlevselliğini bir kaynak kümesi olarak model için işlemleri burada HTTP fiilleri tarafından temsil edilir kullanmanız gerekir. Başka bir deyişle, aynı mantıksal kaynak üzerinde birçok işlemler (örneğin, GET, POST) aynı URL'yi kullanır. Öznitelik yönlendirmeyi dikkatli bir şekilde bir API uygulamasının genel bir uç nokta düzenini tasarlamak için nelere ihtiyacınız olduğunu denetim düzeyi sağlar.

Razor sayfaları uygulamaları kullanma varsayılan adlandırılmış kaynaklarında sunmak için yönlendirme geleneksel *sayfaları* uygulama klasörü. Ek kuralları kullanılabilir Razor sayfaları yönlendirme davranışı özelleştirmenize olanak sağlar. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamayı birbirine bağlamak için URL'leri kodlamak olmadan geliştirilecek okumasına izin verir. Temel bir yönlendirme yapılandırmayla başlatma ve uygulamanın kaynak Düzen belirlendikten sonra yolları değiştirmek için bu destek sağlar.

::: moniker range=">= aspnetcore-2.2"

Yönlendirme kullanan *uç noktaları* (`Endpoint`) mantıksal uç noktaları bir uygulamada temsil etmek için.

Bir uç nokta istekleri ve rastgele meta veri koleksiyonu işlemek için bir temsilci tanımlar. Meta veriler, ilkeleri ve yapılandırma için her bir uç noktasının iliştirilmiş temelinde kullanılan uygulama geniş kapsamlı kritik konular olur.

Yönlendirme sistem aşağıdaki özelliklere sahiptir:

* Rota şablonu söz dizimi, parçalanmış rota parametrelerinin rotalarla tanımlamak için kullanılır.
* Geleneksel stili ve öznitelik stili uç nokta yapılandırması izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> bir URL parametresi için belirtilen uç nokta kısıtlaması geçerli bir değer içerip içermediğini belirlemek için kullanılır.
* MVC/Razor sayfaları gibi uygulama modelleri, tahmin edilebilir bir uygulama Yönlendirme senaryoları biri olan uç noktaları, tüm kaydedin.
* Yönlendirme uygulama ara yazılım ardışık istenen her yerde yönlendirme kararlarını verir.
* Yönlendirme Ara yazılımdan sonra görünen bir ara yazılım, belirli bir istek URI'si için uç nokta karar yönlendirme Ara sonucunu inceleyebilirsiniz.
* Tüm uç noktalar için ara yazılım ardışık düzenini herhangi bir yerindeki uygulamasında numaralandırma mümkündür.
* Uygulama yönlendirme (örneğin, yeniden yönlendirme veya bağlantıları) uç noktası bilgilerini temel alarak URL'lerini oluşturmak ve bu nedenle bakım yardımcı olan URL'ler, sabit kodlanmış önlemek için kullanabilirsiniz.
* URL üretimi rastgele genişletilebilirlik desteği adreslerinde dayanır:

  * Bağlantı oluşturucu API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) herhangi bir yerde kullanarak çözülebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) URL'lerini oluşturmak için.
  * Bağlantı oluşturucu API DI kullanılabilir olmayan <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> URL'ler oluşturma yöntemleri sunar.

> [!NOTE]
> Uç noktası ASP.NET Core 2.2 yönlendirme sürümle birlikte, uç nokta bağlama MVC/Razor sayfaları Eylemler ve sayfaları ile sınırlıdır. Uç nokta bağlama genişletmeleri özellikleri gelecekteki sürümlerde planlanmış.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yönlendirme kullanan *yollar* (uygulamaları <xref:Microsoft.AspNetCore.Routing.IRouter>) için:

* Gelen istekler harita *rota işleyicileri*.
* Yanıtları kullanılan URL'leri oluşturur.

Varsayılan olarak, bir uygulama rota tek bir koleksiyonu vardır. Bir istek ulaştığında, koleksiyon yolları koleksiyonda mevcut olduğunu sırada işlenir. Framework çağırarak gelen istek URL rota koleksiyondaki eşleştirmeyi dener <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> koleksiyondaki her bir yol yöntemi. Yanıt, yönlendirme (örneğin, yeniden yönlendirme veya bağlantıları) rota bilgilerine dayalı URL'lerini oluşturmak ve bu nedenle bakım yardımcı olan URL'ler, sabit kodlanmış önlemek için kullanabilirsiniz.

Yönlendirme sistem aşağıdaki özelliklere sahiptir:

* Rota şablonu söz dizimi, parçalanmış rota parametrelerinin rotalarla tanımlamak için kullanılır.
* Geleneksel stili ve öznitelik stili uç nokta yapılandırması izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> bir URL parametresi için belirtilen uç nokta kısıtlaması geçerli bir değer içerip içermediğini belirlemek için kullanılır.
* MVC/Razor sayfaları gibi uygulama modelleri Yönlendirme senaryoları, tahmin edilebilir bir uygulamasına sahipseniz, yolların kaydedin.
* Yanıt, yönlendirme (örneğin, yeniden yönlendirme veya bağlantıları) rota bilgilerine dayalı URL'lerini oluşturmak ve bu nedenle bakım yardımcı olan URL'ler, sabit kodlanmış önlemek için kullanabilirsiniz.
* URL üretimi rastgele genişletilebilirlik desteği yolları üzerinde temel alır. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> URL'ler oluşturma yöntemleri sunar.

::: moniker-end

Yönlendirme bağlı olduğu [ara yazılım](xref:fundamentals/middleware/index) tarafından işlem hattı <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı. [ASP.NET Core MVC](xref:mvc/overview) yapılandırmasına bir parçası olarak ara yazılım ardışık düzene yönlendirme ekler ve MVC Razor sayfaları uygulamalarında yönlendirilmesini sağlar. Bir tek başına bileşeni olarak Yönlendirme öğrenmek için bkz [kullanın yönlendirme ara yazılım](#use-routing-middleware) bölümü.

### <a name="url-matching"></a>URL eşleştirme

::: moniker range=">= aspnetcore-2.2"

URL ile eşleşen işlemdir hangi yönlendirme dağıtımları tarafından gelen bir istek için bir *uç nokta*. Bu işlem URL yolunda verileri temel alır, ancak istekteki herhangi bir veri dikkate alınması gereken genişletilebilir. İşleyicileri ayırmak için istekleri gönderme olanağı, büyüklüğü ve karmaşıklığı ile bir uygulama, ölçeklendirme için anahtardır.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Bir yönlendirme ara yazılımı yürütüldüğünde, bir uç nokta ayarlar (`Endpoint`) ve rota değerleri için bir özellik üzerinde <xref:Microsoft.AspNetCore.Http.HttpContext>. Geçerli istek için:

* Çağırma `HttpContext.GetEndpoint` uç noktasını alır.
* `HttpRequest.RouteValues` Rota değerleri koleksiyonunu alır.

Yönlendirme Ara yazılımdan sonra çalışan bir ara yazılım, uç nokta bakın ve eylemi gerçekleştirin. Örneğin, bir yetkilendirme ara yazılımı, uç noktanın bir yetkilendirme ilkesi için meta veri koleksiyonu sorgulayın. Seçili uç noktanın temsilci, tüm ara yazılımların istek işleme ardışık düzeninde yürütülür sonra çağrılır.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Uç nokta akışındaki yönlendirme sistemi için tüm dispatching kararlarını sorumludur. Ara yazılım seçilen uç noktasına göre ilkeleri uygulayan olduğundan, gönderme etkileyebilecek herhangi bir karar veya güvenlik ilkeleri uygulamayı, yönlendirme sistemin içine yapılan önemlidir.

Uç nokta temsilci ne zaman çalıştırılır, özelliklerini [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) şimdiye kadarki gerçekleştirilen istek işleme göre uygun değerlere ayarlayın.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

URL ile eşleşen işlemdir hangi yönlendirme dağıtımları tarafından gelen bir istek için bir *işleyici*. Bu işlem URL yolunda verileri temel alır, ancak istekteki herhangi bir veri dikkate alınması gereken genişletilebilir. İşleyicileri ayırmak için istekleri gönderme olanağı, büyüklüğü ve karmaşıklığı ile bir uygulama, ölçeklendirme için anahtardır.

Gelen istekleri girin <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, çağıran <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemi dizideki her bir yol. <xref:Microsoft.AspNetCore.Routing.IRouter> Örneği seçer kullanılıp kullanılmayacağını *işlemek* ayarlayarak isteğin [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) NULL olmayan <xref:Microsoft.AspNetCore.Http.RequestDelegate>. İstek için bir işleyici bir rotayı ayarlar, işlemenin durması yönlendirmek ve işleyicisi isteği işlemek için çağrılır. İsteği işlemek için hiçbir yol işleyicisi bulunursa, ara yazılım istek ardışık düzende sonraki ara yazılımı için devre dışı istek uygulamalı.

Birincil Giriş <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> olduğu [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) geçerli istekle ilişkili. [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) ve [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) olduğunu çıkışları bir yol ile eşleşen sonra ayarlayın.

Çağıran bir eşleşme <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> özelliklerini de ayarlar [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) şimdiye kadarki gerçekleştirilen istek işleme göre uygun değerleri.

::: moniker-end

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) sözlüğü olan *rota değerleri* route'tan üretti. Bu değerler, genellikle URL belirteç oluşturma tarafından belirlenir ve kullanıcı girişi kabul etmek veya daha fazla dispatching kararlar uygulaması içinde için kullanılabilir.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) olan bir özellik paketi eşlenen rotaya ilgili ek veriler. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> Uygulama kararlar verebilen her yol ile ilişkilendirmeyi durumu verilerini destekleyecek şekilde sağlanan eşleşen hangi rotalar. Bu değerler Geliştirici tanımlı ve yapmak **değil** herhangi bir şekilde yönlendirme davranışını etkiler. Ayrıca, değerleri, gizli [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) aksine herhangi bir türde olabilir [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), gelen dizeleri ve dönüştürülebilir olmalıdır.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) isteği başarıyla eşleşen yer alan yolları bir listesidir. Yollar başka içinde yuvalanabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> Özelliği üzerinden bir eşleşme ile sonuçlandı yolların mantıksal ağaç yolu gösterir. Genel olarak, listedeki ilk öğe <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> rota koleksiyonu ve URL üretmek için kullanılmamalıdır. Son öğenin <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> eşleşen bir rota işleyicisi.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>URL üretimi LinkGenerator ile

::: moniker range=">= aspnetcore-2.2"

URL üretimi işlemdir hangi yönlendirerek, rota değerleri kümesine göre bir URL yolu oluşturabilirsiniz. Bu, uç noktaları ve erişim URL'leri arasında mantıksal bir ayrım sağlar.

Bağlantı oluşturucu API uç noktası yönlendirme içerir (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> DI alınan bir singleton hizmetidir. API, bir yürütme isteği bağlamı dışında kullanılabilir. MVC'ın <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ve dayanan senaryoları <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, gibi [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML yardımcıları ve [eylem sonuçlarını](xref:mvc/controllers/actions), özellikleri oluşturma bağlantı sağlamak için bağlantı oluşturucuyu kullanın.

Bağlantı oluşturucu kavramı tarafından desteklenen bir *adresi* ve *adres düzenleri*. Adres düzeni bağlantı oluşturma için değerlendirilmesi gereken uç noktalarını belirleyen bir yoludur. Örneğin, rota adı ve rota değerleri senaryoları çok sayıda kullanıcı MVC/Razor sayfaları alışkın olduğunuz adres düzeni uygulanır.

Bağlantı oluşturucunun MVC/Razor sayfaları Eylemler ve sayfaları aşağıdaki uzantı yöntemleri aracılığıyla bağlantı oluşturabilirsiniz:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Bu yöntemlerin bir aşırı içeren bağımsız değişkenleri kabul eden `HttpContext`. Bu yöntemler işlevsel olarak eşdeğerdir `Url.Action` ve `Url.Page` ancak daha fazla esneklik ve seçenek sunar.

`GetPath*` Yöntemleri en çok benzeyen `Url.Action` ve `Url.Page` içeren mutlak bir yol içeren bir URI hazırlanmasının. `GetUri*` Yöntemleri her zaman bir düzen ve konak içeren mutlak bir URI oluşturur. Kabul yöntemleri bir `HttpContext` yürütme isteği bağlamında bir URI oluşturmayı. Ortam rota değerleri, temel URL yolu, Düzen ve konak yürütülen istek, geçersiz kılınmadığı sürece kullanılır.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> sahip bir adres olarak adlandırılır. Bir URI oluşturma iki adımda gerçekleştirilir:

1. Bir adres bir adres ile aynı uç noktalar listesine bağlanır.
1. Her uç noktanın `RoutePattern` sağlanan değerler eşleşen bir rota deseni bulunana kadar değerlendirilir. Elde edilen çıkış, bağlantı oluşturucuya sağlanan ve döndürülen diğer URI bölümleri ile birleştirilir.

Tarafından sağlanan yöntemleri <xref:Microsoft.AspNetCore.Routing.LinkGenerator> adresi herhangi bir türü için standart bağlantı oluşturma özellikleri destekler. Bağlantı oluşturucu kullanmanın en kolay yolu, işlemleri belirli adres türü için genişletme yöntemleri aracılığıyladır.

| Genişletme yöntemi   | Açıklama                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Bir URI ile sağlanan değerlerine göre mutlak bir yol oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Sağlanan değerlere dayalı bir mutlak URI oluşturur.             |

> [!WARNING]
> Aşağıdaki arama etkilerini dikkat <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemleri:
>
> * Kullanım `GetUri*` genişletme yöntemleri dikkatle doğrulamaz bir uygulama yapılandırmasında `Host` , gelen istek üst bilgisi. Varsa `Host` değil, gelen istek üst bilgisi doğrulandı, güvenilmeyen istek girişi, URI'de bir görünüm/sayfasında istemcisine gönderilebilir. Tüm üretim uygulamaları doğrulamak için sunucu yapılandırmanızı öneririz `Host` bilinen geçerli değerler karşı başlığı.
>
> * Kullanım <xref:Microsoft.AspNetCore.Routing.LinkGenerator> Ara yazılımında birlikte dikkatle `Map` veya `MapWhen`. `Map*` bağlantı oluşturma çıkışını etkiler yürütülen istek temel yolunu değiştirir. Tüm <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API'leri izin temel yolu belirtme. Her zaman geri almak için boş bir temel yol belirtmeniz `Map*`kullanıcının bağlantı nesil etkiler.

## <a name="differences-from-earlier-versions-of-routing"></a>Önceki sürümlerini yönlendirme arasındaki farklar

Uç noktası yönlendirme içinde ASP.NET Core 2.2 veya sonraki bir sürümü ile ASP.NET Core yönlendirme önceki sürümleri arasında bazı farklar mevcuttur:

* Uç noktası yönlendirme sistemi desteklemiyor <xref:Microsoft.AspNetCore.Routing.IRouter>-genişletilebilirlik dan devralan dahil olmak üzere, temel <xref:Microsoft.AspNetCore.Routing.Route>.

* Uç nokta yönlendirmeyi desteklemiyor [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Kullanım 2.1 [uyumluluk sürümü](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) uyumluluk dolgu kullanmaya devam etmek için.

* Uç noktası yönlendirme için oluşturulan bir URI'leri büyük küçük harfleri farklı bir davranış geleneksel yollar kullanırken sahiptir.

  Aşağıdaki varsayılan rota şablonu göz önünde bulundurun:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Aşağıdaki yol kullanarak bir eylem giden bir bağlantı oluşturmak varsayalım:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  İle <xref:Microsoft.AspNetCore.Routing.IRouter>-yönlendirme bağlı olarak, bu kod bir URI'sini oluşturur `/blog/ReadPost/17`, belirtilen rota değerin büyük küçük harfleri dikkate alır. ASP.NET Core 2.2 veya sonraki bir sürümde uç noktası yönlendirme üretir `/Blog/ReadPost/17` ("Blog" büyük harfle). Uç noktası yönlendirme sağlar `IOutboundParameterTransformer` bu davranışı genel özelleştirme veya URL'leri eşleme için farklı kuralları uygulamak için kullanılan arabirim.

  Daha fazla bilgi için [parametre transformer başvurusu](#parameter-transformer-reference) bölümü.

* Geleneksel rotalarla MVC/Razor sayfaları tarafından kullanılan bağlantı oluşturma, bir denetleyici/eylem veya yok sayfasına bağlantı çalışırken farklı davranır.

  Aşağıdaki varsayılan rota şablonu göz önünde bulundurun:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Aşağıdaki varsayılan şablonu kullanarak bir eylem giden bir bağlantı oluşturmak varsayalım:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  İle `IRouter`-yönlendirme bağlı olarak, sonuç her zaman, `/Blog/ReadPost/17`bile `BlogController` mevcut değil veya yok bir `ReadPost` eylem yöntemi. ASP.NET Core 2.2 veya sonraki bir sürümde uç noktası yönlendirme beklendiği gibi üretir `/Blog/ReadPost/17` eylem yöntemi varsa. *Ancak, uç noktası yönlendirme eylemini mevcut değilse boş bir dize oluşturur.* Kavramsal olarak, uç noktası yönlendirme eylemi mevcut değilse uç nokta var olduğunu varsaymaz.

* Bağlantı oluşturma *ortam değeri geçersiz kılma algoritması* uç noktası yönlendirme ile kullanıldığında farklı davranır.

  *Ortam değeri geçersiz kılma* hangi rota değerleri (ortam değerler) şu anda yürütülen istek bağlantı oluşturma işlemlerinde kullanılabilir karar algoritmasıdır. Her zaman geleneksel yönlendirme ek rota değerleri için farklı bir eylem bağlanırken geçersiz kılındı. Öznitelik yönlendirme, ASP.NET Core 2.2 sürümünü önce bu davranışı yoktu. ASP.NET Core önceki sürümlerinde aynı yol parametre adları kullanan başka bir eylem bağlantıları bağlantı oluşturma hataları sonuçlandı. ASP.NET Core 2.2 veya sonraki sürümlerde, her iki biçimi yönlendirme için başka bir eylem bağlarken değerleri geçersiz.

  Aşağıdaki örnek ASP.NET Core 2.1 veya daha önce göz önünde bulundurun. Başka bir eylem (veya başka bir sayfa) bağlanırken, rota değerleri istenmeyen şekilde yeniden kullanılabilir.

  İçinde */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  İçinde */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  URI ise `/Store/Product/18` bağlantı ASP.NET Core 2.1 veya daha önce oluşturulan tarafından Store/bilgileri sayfasında `@Url.Page("/Login")` olduğu `/Login/18`. `id` 18 değerini yeniden kullanılabilir, bağlantı hedef tamamen farklı bir uygulamanın parçası olmasına rağmen. `id` Rota değeri bağlamında `/Login` sayfası büyük olasılıkla bir kullanıcı kimliği değerini depolama ürün kimliği değeri geçersiz.

  Uç noktasında yönlendirme ile ASP.NET Core 2.2 veya üzeri sonucudur `/Login`. Ortam değerleri bağlı hedef farklı bir eylem veya sayfası olduğunda yeniden değildir.

* Gidiş dönüşü rota parametresi söz dizimi: Eğik olmayan kodlanmış bir yıldız işareti çift kullanırken (`**`) genel parametresi söz dizimi.

  Bağlantı oluşturma sırasında bir çift yıldız yakalanan değer yönlendirme sistem kodlar (`**`) genel parametresi (örneğin, `{**myparametername}`) eğik hariç. Catch-tüm çift yıldız işareti ile desteklenen `IRouter`-ASP.NET Core 2.2 veya sonraki bir sürümde yönlendirme tabanlı.

  ASP.NET Core'nın önceki sürümlerinde yıldız genel parametre sözdizimi (`{*myparametername}`) desteklenen kalır ve eğik kodlanır.

  | yol              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (eğik çizgi kodlanmış)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ara yazılım örneği

Aşağıdaki örnekte, bir ara yazılım kullanan <xref:Microsoft.AspNetCore.Routing.LinkGenerator> listeleyen bir eylem yöntemine giden bağlantı oluşturmak için API ürünleri depolayın. Bir sınıf ve arama ekleyerek bağlantı oluşturucuyu kullanarak `GenerateLink` bir uygulamada herhangi bir sınıf için kullanılabilir.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

URL üretimi işlemdir hangi yönlendirerek, rota değerleri kümesine göre bir URL yolu oluşturabilirsiniz. Bu yol işleyicisi ve bunlara erişim URL'leri arasında mantıksal bir ayrım sağlar.

URL üretimi, benzer bir süreçtir takip eder, ancak kullanıcı veya framework kodu çağırma ile başlar <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> yöntemi rota koleksiyonu. Her *rota* sahip kendi <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> yöntemi null olmayan bir kadar sırayla çağrılır <xref:Microsoft.AspNetCore.Routing.VirtualPathData> döndürülür.

Birincil giriş için <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> şunlardır:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Yollar, öncelikle tarafından sağlanan rota değerleri kullanın <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> ve <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> bir URL'yi oluşturmak mümkün olup olmadığını ve hangi içerecek şekilde değerleri karar vermek için. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> Geçerli isteğe eşleştirmeden üretilen rota değerlerini alır. Buna karşılık, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> geçerli işlem için istenen URL'yi oluşturmak nasıl belirten rota değerlerdir. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> Olasılığına yol Hizmetleri ya da geçerli bağlam ile ilişkili ek veri edinmelidir sağlanır.

> [!TIP]
> Düşünün [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) geçersiz kılmalar için bir dizi olarak [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). URL üretimi, rota değerleri için bağlantıları aynı yol ya da rota değerlerini kullanarak URL üretmek için geçerli istek yeniden dener.

Çıkışı <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> olduğu bir <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> Paralel olan <xref:Microsoft.AspNetCore.Routing.RouteData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> içeren <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> çıkış URL'si ve rota tarafından ayarlanması bazı ek özellikler.

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) özelliği içeren *sanal yol* rota tarafından üretilen. Gereksinimlerinize bağlı olarak, daha fazla yolu işlem gerekebilir. Oluşturulan URL HTML işlemek istiyorsanız, uygulamanın taban yolu önüne ekleyin.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) URL başarıyla oluşturuldu rota bir başvurudur.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) özellikleri olan URL'yi oluşturulan yol ilgili ek veri sözlüğü. Bu, paralel, [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

::: moniker-end

### <a name="create-routes"></a>Yollar oluşturma

::: moniker range="< aspnetcore-2.2"

Yönlendirme sağlar <xref:Microsoft.AspNetCore.Routing.Route> sınıf öğesinin standart uygulaması olarak <xref:Microsoft.AspNetCore.Routing.IRouter>. <xref:Microsoft.AspNetCore.Routing.Route> kullanan *rota şablonu* karşı URL yolu eşleştirilecek desenleri tanımlamak için sözdizimi, <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> çağrılır. <xref:Microsoft.AspNetCore.Routing.Route> bir URL'yi oluşturmak için aynı rota şablonunu kullanır, <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> çağrılır.

::: moniker-end

Çoğu uygulama çağırarak yollar oluşturma <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> veya benzer genişletme yöntemlerini tanımlanan <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Herhangi bir <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> genişletme yöntemleri öğesinin bir örneğini oluşturamıyor <xref:Microsoft.AspNetCore.Routing.Route> ve rota koleksiyona ekleyin.

::: moniker range=">= aspnetcore-2.2"

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> bir rota işleyicisi parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> yalnızca tarafından işlenen rotalar ekler <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. MVC yönlendirme hakkında daha fazla bilgi için bkz: <xref:mvc/controllers/routing>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> bir rota işleyicisi parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> yalnızca tarafından işlenen rotalar ekler <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Varsayılan işleyici bir `IRouter`, ve işleyici isteği işlememesi. Örneğin, yalnızca işleyen bir varsayılan işleyici eşleşen bir kullanılabilir denetleyici ve eylem istekleri gibi ASP.NET Core MVC genellikle yapılandırılır. MVC yönlendirme hakkında daha fazla bilgi için bkz: <xref:mvc/controllers/routing>.

::: moniker-end

Aşağıdaki kod örneği, örneğidir bir <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> tipik bir ASP.NET Core MVC yönlendirme tanımı tarafından kullanılan arama:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon, eşleşen bir URL yolu ve rota değerlerini ayıklar. Örneğin, yol `/Products/Details/17` aşağıdaki rota değerlerini oluşturur: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri ve URL yolunu kesimler halinde bölme ve her bir kesim ile eşleşen tarafından belirlenir *parametre yol* adını, rota şablonu. Rota parametrelerine olarak adlandırılır. Parametre adı, küme ayraçları içine kapsayan tarafından tanımlanan parametrelerin `{ ... }`.

Yukarıdaki şablonu URL yolu da eşleşen `/` ve değerler üretmek `{ controller = Home, action = Index }`. Bu durum oluşur `{controller}` ve `{action}` rota parametrelerinin varsayılan değerlere sahip olur ve `id` rota parametresinin isteğe bağlı. Bir eşittir işareti (`=`) rota parametresi adı parametresi için varsayılan bir değer tanımlar sonra bir değer tarafından izlenen. Bir soru işareti (`?`) sonra isteğe bağlı bir parametre rota parametresi adını tanımlar.

Rota parametrelerinin varsayılan bir değerle *her zaman* rota eşleştiğinde bir yönlendirme değeri üretir. İsteğe bağlı parametreler, karşılık gelen hiçbir URL yol kesimi ise bir yönlendirme değeri üretmediği. Bkz: [rota şablonu başvurusu](#route-template-reference) bölümünü kapsamlı bir açıklama için rota şablonu senaryoları ve söz dizimi.

Aşağıdaki örnekte, rota parametre tanımında `{id:int}` tanımlayan bir [rota kısıtlaması](#route-constraint-reference) için `id` parametre yol:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon bir URL yolu gibi eşleşen `/Products/Details/17` ama `/Products/Details/Apples`. Rota kısıtlamalarını uygulamak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> ve onları doğrulamak için rota değerlerini denetleyin. Bu örnekte, rota değeri `id` tamsayıya dönüştürülebilir olmalıdır. Bkz: [rota kısıtlaması başvurusu](#route-constraint-reference) rota kısıtlamaları framework tarafından sağlanan bir açıklaması için.

Ek aşırı yüklemeleri <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> değerleri kabul edin `constraints`, `dataTokens`, ve `defaults`. Bu parametre normal kullanım, burada parametre adları anonim tür eşleşen özellik adlarını rota anonim olarak belirlenmiş bir nesne geçirmektir.

Aşağıdaki <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> örnekler eşdeğer yollar oluşturur:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Satır içi söz dizimi kısıtlamaları ve varsayılan değerleri tanımlamak için basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçlerini gibi senaryoları vardır.

Aşağıdaki örnek, bazı ek senaryoları göstermektedir:

::: moniker range=">= aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablonda bir URL yolu gibi eşleşen `/Blog/All-About-Routing/Introduction` ve değerleri ayıklar `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Varsayılan rota değerleri için `controller` ve `action` olsa bile karşılık gelen hiçbir rota parametrelerinin şablonda rota tarafından üretilir. Varsayılan değerleri, rota şablonu belirtilebilir. `article` Rota parametresi olarak tanımlanmış olan bir *tümünü* çift yıldız görünümünü tarafından (`**`) rota parametre adından önce. Tümünü rota parametrelerinin, URL yolu geri kalanında yakalayın ve boş bir dize da eşleştirebilirsiniz.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablonda bir URL yolu gibi eşleşen `/Blog/All-About-Routing/Introduction` ve değerleri ayıklar `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Varsayılan rota değerleri için `controller` ve `action` olsa bile karşılık gelen hiçbir rota parametrelerinin şablonda rota tarafından üretilir. Varsayılan değerleri, rota şablonu belirtilebilir. `article` Rota parametresi olarak tanımlanmış olan bir *tümünü* görünümüne göre bir yıldız işareti (`*`) rota parametre adından önce. Tümünü rota parametrelerinin, URL yolu geri kalanında yakalayın ve boş bir dize da eşleştirebilirsiniz.

::: moniker-end

Aşağıdaki örnek, rota kısıtlamaları ve veri belirteçleri ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablonda bir URL yolu gibi eşleşen `/en-US/Products/5` ve değerleri ayıklar `{ controller = Products, action = Details, id = 5 }` ve veri belirteçleri `{ locale = en-US }`.

![Yerel Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Rota sınıfı URL üretimi

<xref:Microsoft.AspNetCore.Routing.Route> Sınıfı da gerçekleştirebilirsiniz URL üretimi bir dizi rota değerleri, rota şablonu ile birleştirerek. Bu mantıksal olarak URL yolu eşleşen ters işlemidir.

> [!TIP]
> URL üretimi daha iyi anlamak için oluşturmak ve ardından söz konusu URL rota şablonu nasıl BC hakkında düşünmek için hangi URL'yi düşünün. Hangi değerleri üretilen? Bu, URL üretimi nasıl çalıştığını kabaca eşdeğeridir <xref:Microsoft.AspNetCore.Routing.Route> sınıfı.

Aşağıdaki örnek, genel bir ASP.NET Core MVC varsayılan rota kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Rota değerleri ile `{ controller = Products, action = List }`, URL `/Products/List` oluşturulur. Rota değerleri, URL yolu oluşturmak karşılık gelen rota parametrelerinin yerini alır. Bu yana `id` isteğe bağlıdır rota parametresi, URL için bir değer olmadan başarıyla oluşturulur `id`.

Rota değerleri ile `{ controller = Home, action = Index }`, URL `/` oluşturulur. Belirtilen rota değerleri varsayılan değerleriyle eşleşir ve varsayılan değerlere karşılık gelen segmentleri güvenli bir şekilde göz ardı edilir.

Aşağıdaki rota tanımını ile hem oluşturulan URL'leri gidiş dönüş (`/Home/Index` ve `/`) URL'yi oluşturmak için kullanılan aynı rota değerlerini üretir.

> [!NOTE]
> ASP.NET Core MVC kullanarak uygulama kullanması gereken <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> doğrudan yönlendirme içine çağırmak yerine URL'lerini oluşturmak için.

URL üretimi hakkında daha fazla bilgi için bkz. [Url üretimi başvuru](#url-generation-reference) bölümü.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılım kullanın

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) uygulamanın proje dosyasında.

Hizmet kapsayıcısını yönlendirme ekleme `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yollar yapılandırılmalıdır `Startup.Configure` yöntemi. Örnek uygulama aşağıdaki API'leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Yalnızca HTTP GET isteklerini eşleşir.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Aşağıdaki tablo, belirli bir URI'leri yanıtları gösterir.

| URI                    | Yanıt                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Merhaba! Rota değerleri: [işlem oluşturun], [ID, 3] |
| `/package/track/-3`    | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3] |
| `/package/track/-3/`   | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3] |
| `/package/track/`      | Talep düştüğünde aracılığıyla, eşleşme yok.              |
| `GET /hello/Joe`       | Merhaba, ALi!                                          |
| `POST /hello/Joe`      | İstek geçmez, yalnızca HTTP GET eşleşir. |
| `GET /hello/Joe/Smith` | Talep düştüğünde aracılığıyla, eşleşme yok.              |

::: moniker range="< aspnetcore-2.2"

Tek bir yol yapılandırıyorsanız, çağrı <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> öğesinde geçen bir `IRouter` örneği. Kullanmanız gerekmez <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

::: moniker-end

Çerçeve rotalar oluşturma için genişletme yöntemleri kümesini sunar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

::: moniker range="< aspnetcore-2.2"

Listelenen bazı yöntemler gibi <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, gerekli bir <xref:Microsoft.AspNetCore.Http.RequestDelegate>. <xref:Microsoft.AspNetCore.Http.RequestDelegate> Olarak kullanılan *rota işleyiciye* zaman yol ile eşleşen. Bu serideki diğer yöntemleri rota işleyiciye bir ara yazılım ardışık düzenini kullanmak için yapılandırma sağlar. Varsa `Map*` değil accept yöntemi bir işleyici gibi <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, kullandığı <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

::: moniker-end

`Map[Verb]` Yöntemleri kısıtlamaları HTTP fiili yöntem adı için rota sınırlamak için kullanın. Örneğin, <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Belirteçleri kaşlı ayraçlar içinde (`{ ... }`) tanımlamak *rota parametreleri* rotanın eşleşmesi durumunda bağlıdır. İçindeki bir yol parçasının birden fazla rota parametresini tanımlayabilirsiniz, ancak bir değişmez değer ayrılmalıdır. Örneğin, `{controller=Home}{action=Index}` arasında herhangi bir değişmez değer olduğundan geçerli bir yol değil `{controller}` ve `{action}`. Bu rota parametrelerinin bir adı olmalıdır ve ek öznitelikler belirtmiş olabilirsiniz.

Metin rota parametrelerinin dışında (örneğin, `{id}`) ve yol ayırıcısı `/` URL metin eşleşmesi gerekir. Eşleşen metin büyük/küçük harfe ve URL yolunu kodu çözülmüş gösterimini göre. Değişmez değer rota parametresi sınırlayıcı eşleştirmek için (`{` veya `}`), sınırlayıcı karakter tekrarlayarak kaçış (`{{` veya `}}`).

İsteğe bağlı dosya uzantısına sahip bir dosya adı yakalama girişimi URL desenleri ek hususlar vardır. Örneğin, şablonu düşünün `files/{filename}.{ext?}`. Hem de değerleri `filename` ve `ext` var, her iki değerler doldurulur. İçin bir değer `filename` URL'nin yol eşleşmeleri var olduğundan sondaki nokta (`.`) isteğe bağlıdır. Aşağıdaki URL'ler bu rotanın eşleşmesi:

* `/files/myFile.txt`
* `/files/myFile`

::: moniker range=">= aspnetcore-2.2"

Bir yıldız işareti kullanabilirsiniz (`*`) veya çift yıldız işareti (`**`) URI rest'e bağlamak için bir rota parametresini öneki olarak. Bu adlı bir *tümünü* parametreleri. Örneğin, `blog/{**slug}` ile başlayan herhangi bir URL ile eşleşen `/blog` ve öğesine atanan bu, aşağıdaki herhangi bir değere sahip `slug` rota değeri. Genel parametreleri da boş dize eşleşebilir.

Yol, yol ayırıcı dahil olmak üzere, bir URL'yi oluşturmak için kullanıldığında, yakalama-all parametresini uygun karakterleri kaçırır. (`/`) karakter. Örneğin, yol `foo/{*path}` rota değerleri ile `{ path = "my/path" }` oluşturur `foo/my%2Fpath`. Atlanan eğik unutmayın. Gidiş dönüş yol ayırıcı karakter kullanın `**` rota parametresinin öneki. Rota `foo/{**path}` ile `{ path = "my/path" }` oluşturur `foo/my/path`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yıldız işareti kullanabilirsiniz (`*`) URI rest'e bağlamak için bir rota parametresini öneki olarak. Bu adlı bir *tümünü* parametresi. Örneğin, `blog/{*slug}` ile başlayan herhangi bir URL ile eşleşen `/blog` ve öğesine atanan bu, aşağıdaki herhangi bir değere sahip `slug` rota değeri. Genel parametreleri da boş dize eşleşebilir.

Yol, yol ayırıcı dahil olmak üzere, bir URL'yi oluşturmak için kullanıldığında, yakalama-all parametresini uygun karakterleri kaçırır. (`/`) karakter. Örneğin, yol `foo/{*path}` rota değerleri ile `{ path = "my/path" }` oluşturur `foo/my%2Fpath`. Atlanan eğik unutmayın.

::: moniker-end

Rota parametrelerine sahip olabilir *varsayılan değerler* belirlenmiş bir eşittir işareti ile ayrılmış parametre adının varsayılan değeri belirterek (`=`). Örneğin, `{controller=Home}` tanımlar `Home` için varsayılan değer olarak `controller`. Hiçbir değer parametresi için URL'de varsa varsayılan değer kullanılır. Rota parametrelerine yapılan isteğe bağlı bir soru işareti eklenerek (`?`) parametre adı, giriş olarak sonuna `id?`. İsteğe bağlı değerler ve varsayılan rota parametrelerini arasındaki fark, bir rota parametresini varsayılan değeri her zaman bir değer üretir olan&mdash;yalnızca bir değer istek URL'si sağlandığında isteğe bağlı parametresi bir değere sahip.

Rota parametrelerine bağlı URL'den rota değerle eşleşmelidir kısıtlamaları olabilir. İki nokta ekleme (`:`) ve rota parametresi adını belirttikten sonra kısıtlama adı bir *satır içi kısıtlama* bir rota parametresini üzerinde. Kısıtlama bağımsız değişkenleri gerektiriyorsa, parantez içine alınmış (`(...)`) sonra kısıtlama adı. Başka bir iki nokta üst üste ekleyerek, birden çok satır içi kısıtlamalarını belirtilebilir (`:`) ve kısıtlama adı.

Kısıtlama adı ve bağımsız değişkenler geçirilir <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> bir örneğini oluşturmak için hizmet <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> URL işlenmesinde kullanılacak. Örneğin, rota şablonu `blog/{article:minlength(10)}` belirtir bir `minlength` kısıtlaması bağımsız değişkeniyle `10`. Rota kısıtlamalarını ve kısıtlamaları framework tarafından sağlanan listesi hakkında daha fazla bilgi için bkz. [rota kısıtlaması başvurusu](#route-constraint-reference) bölümü.

::: moniker range=">= aspnetcore-2.2"

Rota parametrelerine bağlantılar ve eşleşen eylemleri ve URL'leri sayfalara oluştururken bir parametrenin değeri dönüştürme parametresi dönüştürücüler, da olabilir. Kısıtlamalar gibi parametre dönüştürücüler satır içi bir rota parametresini bir iki nokta üst üste ekleyerek eklenebilir (`:`) ve rota parametre adından sonra transformer adı. Örneğin, rota şablonu `blog/{article:slugify}` belirtir bir `slugify` dönüştürücü. Parametre dönüştürücüler hakkında daha fazla bilgi için bkz. [parametre transformer başvurusu](#parameter-transformer-reference) bölümü.

::: moniker-end

Aşağıdaki tabloda örnek rota şablonlarının ve davranışları gösterir.

| Rota şablonu                           | Örnek eşleşen URI'si    | İstek URI'si&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca tek bir yol ile eşleşen `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Eşleşen ve ayarlar `Page` için `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Eşleşen ve ayarlar `Page` için `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Eşlendiği `Products` denetleyicisi ve `List` eylem.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Eşlendiği `Products` denetleyicisi ve `Details` eylem (`id` ayarlamak için 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Eşlendiği `Home` denetleyicisi ve `Index` yöntemi (`id` göz ardı edilir).        |

Şablon kullanarak genel yönlendirme en basit yaklaşımdır. Kısıtlamaları ve varsayılan rota şablonu dışında da belirtilebilir.

> [!TIP]
> Etkinleştirme [günlüğü](xref:fundamentals/logging/index) görmek için nasıl yerleşik uygulamaları gibi yönlendirme <xref:Microsoft.AspNetCore.Routing.Route>, eşleşen istekleri.

## <a name="reserved-routing-names"></a>Yönlendirme ayrılmış adlar

Aşağıdaki anahtar sözcükler ayrılmış adları ve rota adları veya parametre kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Rota kısıtlaması başvurusu

Rota kısıtlamalarını bir eşleşme için gelen URL oluştu ve URL yolunu, rota değerlerini simgeleştirilmiş yürütün. Rota kısıtlamalarını genellikle rota şablonu ile ilişkili yol değişkenlerinizin ve Evet olun/yok veya yüklememe değer kabul edilebilir bir karardır. Bazı rota kısıtlamalarını isteğin yönlendirildiği olup olmadığını göz önünde bulundurun için rota değeri dışındaki verileri kullanın. Örneğin, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> kabul edebilir veya kendi HTTP fiiline bağlı bir isteği reddetmek. Kısıtlamaları bağlantı oluşturma ve yönlendirme isteklerinde kullanılır.

> [!WARNING]
> Kısıtlamaları kullanmayın **giriş doğrulama**. Kısıtlamalar için kullanılıyorsa **giriş doğrulama**, geçersiz giriş sonuçları bir *404 - Bulunamadı* yanıt yerine bir *400 - bozuk istek* uygun bir hata iletisi ile. Rota kısıtlamalarını alışkın olduğunuz **belirsizliğinin ortadan kaldırılmasını** girişleri için belirli bir yol değil doğrulamak için benzer yollar.

Aşağıdaki tabloda örnek rota kısıtlamalarını ve bunların beklenen bir davranış gösterir.

| kısıtlama | Örnek | Örnek eşleşmeleri | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayı ile eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | Eşleşme `true` veya `false` (büyük-küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli bir eşleşen `DateTime` değeri (uyarı sabit kültür - bakın) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir eşleşen `decimal` değeri (uyarı sabit kültür - bakın) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `double` değeri (uyarı sabit kültür - bakın) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `float` değeri (uyarı sabit kültür - bakın) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli bir eşleşen `Guid` değeri |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir eşleşen `long` değeri |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize en fazla 8 karakter uzunluğunda olmalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8, en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | En az 18 tamsayı değeri olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | En fazla 120 tamsayı değeri olmalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | En az 18 ancak en fazla 120 tamsayı değeri olmalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize, bir veya daha fazla alfabetik karakter oluşmalıdır (`a`-`z`, büyük küçük harf duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmesi gerekir (bkz. normal bir ifadeyi tanımlama hakkında ipuçları) |
| `required` | `{name:required}` | `Rick` | Parametresi olmayan değer URL oluşturma sırasında mevcut olduğunu uygulamak için kullanılan |

Birden çok, tek bir parametre için virgülle ayrılmış kısıtlamalar uygulanabilir. Örneğin, aşağıdaki kısıtlama parametresi için 1 veya daha büyük bir tamsayı değeri kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın. Framework tarafından sağlanan rota kısıtlamaları, rota değerleri depolanan değerleri değiştirmeyin. URL'den ayrıştırılmış tüm rota değerleri dize olarak depolanır. Örneğin, `float` rota değeri kayana dönüştürülecek kısıtlaması çalışır, ancak dönüştürülen değeri yalnızca, dönüştürülebilir bir kayan nokta doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core framework ekler `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` normal ifade oluşturucusu. Bkz: <xref:System.Text.RegularExpressions.RegexOptions> bu üyeleri bir açıklaması.

Normal ifadeler, sınırlayıcılar ve Yönlendirme ve C# dili tarafından kullanılan benzer belirteçleri kullanın. Normal ifade belirteçleri Atlanan gerekir. Normal ifade kullanılacak `^\d{3}-\d{2}-\d{4}$` yönlendirme, deyim olmalıdır `\` (tek ters eğik çizgi) karakter dizesi olarak sağlanan `\\` (çift ters eğik çizgi) karakterleri C# kaçışiçinkaynakdosyası`\` dize kaçış karakteri (kullanmadığınız sürece [verbatim dize değişmez değerleri](/dotnet/csharp/language-reference/keywords/string)). Yönlendirme parametresi sınırlayıcı karakter kaçış (`{`, `}`, `[`, `]`), ifadedeki karakter çift (`{{`, `}`, `[[`, `]]`). Aşağıdaki tabloda, normal bir ifade ve kaçan sürümünü gösterir.

| Normal ifade    | Atlanan bir normal ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Sık sık yönlendirme kullanılan normal ifadeler ile giriş işaretini Başlat (`^`) karakteri ve dizenin başlangıç konumunu eşleştirin. Deyimler genellikle dolar işareti ile Bitiş (`$`) karakteri ve eşleşme dizenin sonunda. `^` Ve `$` karakter tüm rota parametre değeri normal ifade eşleştirmesi sağlar. Olmadan `^` ve `$` karakterler, normal bir ifadeyle eşleşen genellikle istenmeyen dize içindeki herhangi bir alt dize. Aşağıdaki tabloda, örnekler ve neden aynı veya eşleştirilecek başarısız açıklar.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Merhaba     | Evet   | alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | ifadeyle eşleşiyor    |
| `[a-z]{2}`   | MZ        | Evet   | büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | Merhaba     | Hayır    | bkz: `^` ve `$` yukarıda |
| `^[a-z]{2}$` | 123abc456 | Hayır    | bkz: `^` ve `$` yukarıda |

Normal ifade söz dizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadelerinde](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Olası değerler bilinen bir dizi parametre sınırlamak için normal bir ifade kullanın. Örneğin, `{action:regex(^(list|get|create)$)}` yalnızca eşleşen `action` yönlendirmek için değer `list`, `get`, veya `create`. Dize kısıtlamaları sözlük geçirilen `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biri eşleşmiyor (hizalı şablon içinde değil) kısıtlamaları sözlükteki geçirilen kısıtlamalar da normal ifadeler şeklinde kabul edilir.

## <a name="custom-route-constraints"></a>Özel rota kısıtlamaları

Yerleşik rota kısıtlamalara ek olarak, özel rota kısıtlamalarını uygulayarak oluşturulabilir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> Arabirimi içeren tek bir yöntem `Match`, döndüren `true` kısıtlaması gerçekleştiyse ve `false` Aksi takdirde.

Özel kullanılacak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, rota kısıtlama türü uygulama kaydedilmelidir <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> uygulamanın service kapsayıcısında. A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kısıtlama anahtarlarını haritalar rota bir sözlük <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> kısıtlamalar doğrulayan uygulamalar. Bir uygulamanın <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> içinde güncelleştirilebilir `Startup.ConfigureServices` parçası olarak ya da bir [Hizmetleri. AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrı yapma veya yapılandırmayı <xref:Microsoft.AspNetCore.Routing.RouteOptions> ile doğrudan `services.Configure<RouteOptions>`. Örneğin:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Kısıtlama sonra yolları kısıtlama türü kaydı sırasında belirtilen adı kullanarak her zamanki şekilde uygulanabilir. Örneğin:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Parametre transformer başvurusu

Parametre dönüştürücüler:

* Bir bağlantı oluşturulurken yürütme bir <xref:Microsoft.AspNetCore.Routing.Route>.
* Uygulama `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Kullanılarak yapılandırılmış <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Parametrenin rota değeri alın ve yeni bir dize değerine dönüştürün.
* Oluşturulan bağlantısına dönüştürülen değer kullanılmasına neden.

Örneğin, bir özel `slugify` parametresi transformer yol deseninde `blog\{article:slugify}` ile `Url.Action(new { article = "MyTestArticle" })` oluşturur `blog\my-test-article`.

Yol deseninde bir parametre transformer kullanmak için yapılandırmanız ilk kullanarak <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> içinde `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Parametre dönüştürücüler, burada bir uç nokta çözümler URI dönüştürmek için framework tarafından kullanılır. Örneğin, ASP.NET Core MVC eşleştirmek için kullanılan rota değeri dönüştürmek için parametre dönüştürücüler kullanan bir `area`, `controller`, `action`, ve `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Önceki yol, bir eylem ile `SubscriptionManagementController.GetAll()` URI'si ile eşleşen `/subscription-management/get-all`. Bir parametre transformer giden bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıkarır `/subscription-management/get-all`.

ASP.NET Core, bir parametre dönüştürücüler ile oluşturulan yolları kullanarak için API kuralları sağlar:

* ASP.NET Core MVC sahip `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı. Bu kural, uygulamadaki tüm öznitelik rotaları belirtilen parametre transformer uygular. Bunlar gibi parametre transformer öznitelik rotası belirteçleri dönüştürür. Daha fazla bilgi için [belirteç değiştirme özelleştirmek için bir parametre transformer kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor sayfaları `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralı. Bu kural belirtilen parametre transformer otomatik olarak keşfedildiğini Razor sayfaları için geçerlidir. Parametre dönüştürücü, klasör ve dosya adı parçaları Razor sayfaları yolların dönüştürür. Daha fazla bilgi için [sayfa yollar özelleştirmek için bir parametre transformer kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>URL üretimi başvurusu

Aşağıdaki örnek, rota değerleri sözlüğü belirtilen bir rotaya bir bağlantı oluşturmak gösterilir ve <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> Önceki sonunda oluşturulan örnek `/package/create/123`. Sözlük sağlayan `operation` ve `id` rota değerleri "İzleme paketi yol" şablonunun `package/{operation}/{id}`. Örnek kodda Ayrıntılar için bkz [kullanım yönlendirme ara yazılım](#use-routing-middleware) bölüm veya [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

İkinci parametresi <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> oluşturucudur koleksiyonu *ortam değerlerini*. Ortam değerlerini, bunların değerleri bir geliştirici, bir istek bağlamı içinde belirtmelisiniz sayısını sınırlamak kullanışlıdır. Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir. Bir ASP.NET Core MVC uygulamasının `About` eylemi `HomeController`, bağlanmak için denetleyici rota değeri belirtmeniz gerekmez `Index` eylem&mdash;ortam değerini `Home` kullanılır.

Bir parametre eşleşmeyen ortam değerleri yok sayılır. Ortam değerler, ortam değeri açıkça belirtilen bir değer geçersiz kıldığında da dikkate alınmaz. Eşleşen, soldan sağa doğru gerçekleşir URL.

Açıkça sağlanan değerler ancak yol kesimini eşleşmiyor sorgu dizesine eklenir. Rota şablonu kullanılırken sonucu aşağıdaki tabloda gösterilmiştir `{controller}/{action}/{id?}`.

| Ortam değerleri                     | Açık değerler                        | Sonuç                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| Denetleyici = "Home"                | Eylem "Hakkında" =                       | `/Home/About`           |
| Denetleyici = "Home"                | Denetleyici = "Order", Eylem "Hakkında" = | `/Order/About`          |
| Denetleyici "Home", renk = = "Red" | Eylem "Hakkında" =                       | `/Home/About`           |
| Denetleyici = "Home"                | Eylem = "Hakkında", renk = "Red"        | `/Home/About?color=Red` |

Bir rota parametresi gelmiyor varsayılan bir değer varsa ve bu değeri açıkça sağlanan varsayılan değer eşleşmesi gerekir:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Bağlantı oluşturma için eşleşen değerleri değiştiğinde bu yol için bir bağlantı yalnızca oluşturur `controller` ve `action` sağlanır.

## <a name="complex-segments"></a>Karmaşık segmentleri

Karmaşık bir segment (örneğin `[Route("/x{token}y")]`) değişmez değerlerden soldan sağa'kurmak doyumsuz olmayan bir yolla eşleşen tarafından işlenir. Bkz: [bu kodu](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ilişkin ayrıntılı bir açıklaması için karmaşık segmentler eşleştirilir. [Kod örneği](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler iyi bir açıklama sağlar.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->
