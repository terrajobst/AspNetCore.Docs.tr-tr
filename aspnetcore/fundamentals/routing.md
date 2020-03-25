---
title: ASP.NET Core yönlendirme
author: rick-anderson
description: HTTP isteklerinden eşleşen ve yürütülebilir uç noktalara gönderen ASP.NET Core yönlendirmenin nasıl sorumlu olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 3/25/2020
uid: fundamentals/routing
ms.openlocfilehash: 69d8aa65084d4d2ee13a8ca0e8e275f4344d08c5
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242671"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

[Ryan şimdi ak](https://github.com/rynowak), [Kirk Larkabağı](https://twitter.com/serpent5)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Yönlendirme, gelen HTTP isteklerini eşleştirmekten ve bu istekleri uygulamanın yürütülebilir uç noktalarına gönderen sorumludur. [Uç noktalar](#endpoint) , uygulamanın yürütülebilir istek işleme kodu birimleridir. Uç noktalar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Uç nokta eşleştirme işlemi, isteğin URL 'sindeki değerleri çıkarabilir ve bu değerleri istek işleme için sağlayabilir. Uygulamanın uç nokta bilgilerini kullanarak, yönlendirme, uç noktalarıyla eşlenen URL 'Ler de oluşturabilir.

Uygulamalar, kullanarak yönlendirmeyi yapılandırabilir:

- Denetleyiciler
- Razor Pages
- SignalR
- gRPC Hizmetleri
- [Sistem durumu denetimleri](xref:host-and-deploy/health-checks)gibi uç nokta özellikli [Ara yazılım](xref:fundamentals/middleware/index) .
- Yönlendirme ile kayıtlı temsilciler ve Lambdalar.

Bu belgede ASP.NET Core yönlendirmenin alt düzey ayrıntıları ele alınmaktadır. Yönlendirmeyi yapılandırma hakkında bilgi için:

* Denetleyiciler için bkz. <xref:mvc/controllers/routing>.
* Razor Pages kuralları için bkz. <xref:razor-pages/razor-pages-conventions>.

Bu belgede açıklanan uç nokta yönlendirme sistemi, ASP.NET Core 3,0 ve üzeri için geçerlidir. <xref:Microsoft.AspNetCore.Routing.IRouter>temel alan önceki yönlendirme sistemi hakkında daha fazla bilgi için aşağıdaki yaklaşımlardan birini kullanarak ASP.NET Core 2,1 sürümünü seçin:

* Önceki sürümün sürüm Seçicisi.
* [ASP.NET Core 2,1 yönlendirme](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)öğesini seçin.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Bu belgenin karşıdan yükleme örnekleri, belirli bir `Startup` sınıfı tarafından etkinleştirilir. Belirli bir örneği çalıştırmak için *program.cs* öğesini istenen `Startup` sınıfını çağırmak üzere değiştirin.

## <a name="routing-basics"></a>Yönlendirme temelleri

Tüm ASP.NET Core şablonları oluşturulan koda yönlendirmeyi içerir. Yönlendirme `Startup.Configure`içindeki [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine kaydedilir.

Aşağıdaki kod, yönlendirmenin temel bir örneğini göstermektedir:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

Yönlendirme, <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> ve <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>tarafından kaydedilmiş bir ara yazılım kullanır:

* `UseRouting`, ara yazılım ardışık düzenine eşleşen rota ekler. Bu ara yazılım, uygulamada tanımlanan uç nokta kümesine bakar ve isteğe bağlı olarak [en iyi eşleşmeyi](#urlm) seçer.
* `UseEndpoints`, uç nokta yürütmeyi ara yazılım ardışık düzenine ekler. Seçili uç noktayla ilişkili temsilciyi çalıştırır.

Önceki örnekte, [Mapget](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*) yöntemi kullanılarak kod uç noktasına tek bir *yol* dahildir:

* Bir HTTP `GET` isteği `/`kök URL 'sine gönderildiğinde:
  * Gösterilen istek temsilcisi yürütülüyor.
  * `Hello World!` HTTP yanıtına yazılır. Varsayılan olarak, `/` kök URL 'SI `https://localhost:5001/`.
* İstek yöntemi `GET` yoksa veya kök URL `/`değilse, hiçbir yol eşleşmesi ve HTTP 404 döndürülür.

### <a name="endpoint"></a>Uç Noktası

<a name="endpoint"></a>

`MapGet` yöntemi bir **uç noktayı**tanımlamak için kullanılır. Uç nokta şöyle olabilir:

* URL ve HTTP yöntemiyle eşleştirerek seçilir.
* , Temsilcisi çalıştırılarak yürütülür.

Uygulama tarafından eşleştirilecek ve çalıştırılabilen uç noktalar `UseEndpoints`olarak yapılandırılır. Örneğin, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>ve [benzer yöntemler](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions) , istek temsilcilere yönlendirme sistemine bağlanır.
ASP.NET Core Framework özelliklerini yönlendirme sistemine bağlamak için ek yöntemler kullanılabilir:
- [Razor Pages için MapRazorPages](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)
- [Denetleyiciler için MapControllers](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- [SignalR için MapHub\<THub >](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*) 
- [GRPC için MapGrpcService\<TService >](xref:grpc/aspnetcore)

Aşağıdaki örnekte, daha karmaşık bir yol şablonuyla yönlendirme gösterilmektedir:

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

Dize `/hello/{name:alpha}` bir **yol şablonudur**. Uç noktanın nasıl eşleştirileceği yapılandırmak için kullanılır. Bu durumda, şablon eşleşir:

* `/hello/Ryan` gibi bir URL
* `/hello/` ve ardından alfabetik karakterlerden oluşan bir dizi ile başlayan herhangi bir URL yolu.  `:alpha` yalnızca alfabetik karakterlerle eşleşen bir rota kısıtlaması uygular. [Yol kısıtlamaları](#route-constraint-reference) bu belgenin ilerleyen kısımlarında açıklanmıştır.

URL yolunun ikinci segmenti `{name:alpha}`:

* `name` parametresine bağlanır.
* Yakalanır ve [HttpRequest. RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)'da depolanır.

Bu belgede açıklanan uç nokta yönlendirme sistemi ASP.NET Core 3,0 itibariyle yenidir. Ancak, ASP.NET Core tüm sürümleri aynı yol şablonu özellikleri ve yol kısıtlamaları kümesini destekler.

Aşağıdaki örnek, [durum denetimleri](xref:host-and-deploy/health-checks) ve yetkilendirmeyle yönlendirmeyi gösterir:

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[](~/includes/MTcomments.md)]

Yukarıdaki örnekte nasıl yapılacağı gösterilmektedir:

* Yetkilendirme ara yazılımı yönlendirmeyle birlikte kullanılabilir.
* Uç noktalar, yetkilendirme davranışını yapılandırmak için kullanılabilir.

<xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*> çağrısı bir sistem durumu denetimi uç noktası ekler. Bu çağrıda <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> zincirleme bir yetkilendirme ilkesini uç noktaya iliştirir.

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> ve <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> çağırmak, kimlik doğrulama ve yetkilendirme ara yazılımını ekler. Bu ara yazılım <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> ve `UseEndpoints` arasına yerleştirilir, böylece şunları yapabilirsiniz:

* Hangi uç noktanın `UseRouting`tarafından seçili olduğunu görün.
* Uç noktaya gönderilmeden önce bir yetkilendirme ilkesi uygulayın <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.

<a name="metadata"></a>

### <a name="endpoint-metadata"></a>Uç nokta meta verileri

Yukarıdaki örnekte, iki uç nokta bulunur, ancak yalnızca sistem durumu denetimi uç noktasına bağlı bir yetkilendirme ilkesi vardır. İstek, sistem durumu denetim uç noktasıyla eşleşiyorsa, `/healthz`bir yetkilendirme denetimi gerçekleştirilir. Bu, uç noktalara eklenen ek verilere sahip olduğunu gösterir. Bu ek verilere uç nokta **meta verileri**denir:

* Meta veriler, yönlendirme kullanan ara yazılım tarafından işlenebilir.
* Meta veriler herhangi bir .NET türü olabilir.

## <a name="routing-concepts"></a>Yönlendirme kavramları

Yönlendirme sistemi güçlü **uç nokta** kavramı ekleyerek, ara yazılım ardışık düzeninin üst kısmında oluşturulur. Uç noktalar, yönlendirme, yetkilendirme ve herhangi bir sayıda ASP.NET Core sistemi açısından birbirinden farklı olan uygulama işlevselliğinin birimlerini temsil eder.

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a>ASP.NET Core uç noktası tanımı

ASP.NET Core uç noktası:

* Yürütülebilir: <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>sahip.
* Genişletilebilir: [meta veri](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*) koleksiyonu vardır.
* Seçilebilir: Isteğe bağlı olarak [yönlendirme bilgileri](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)vardır.
* Numaralandırılabilir: [' dan <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>](xref:fundamentals/dependency-injection)alma uç noktaları koleksiyonu listelenebilir.

Aşağıdaki kod, geçerli istekle eşleşen uç noktanın nasıl alınacağını ve inceleneceği gösterilmektedir:

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

Seçili ise bitiş noktası `HttpContext`alınabilir. Özellikleri incelenebilir. Uç nokta nesneleri sabittir ve oluşturulduktan sonra değiştirilemez. Uç noktanın en yaygın türü bir <xref:Microsoft.AspNetCore.Routing.RouteEndpoint>. `RouteEndpoint`, yönlendirme sistemi tarafından seçilme olanağı sağlayan bilgiler içerir.

Yukarıdaki kod, [uygulama. Kullanım](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*) , çevrimiçi bir [Ara yazılım](xref:fundamentals/middleware/index)yapılandırır.

<a name="mt"></a>

Aşağıdaki kod, işlem hattında `app.Use` çağrıldığı yere bağlı olarak bir uç nokta olmayabilir:

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

Bu önceki örnek, bir uç noktanın seçili olup olmadığını gösteren `Console.WriteLine` deyimlerini ekler. Netlik açısından örnek, belirtilen `/` uç noktasına bir görünen ad atar.

Bu kodu `/` bir URL 'SI ile çalıştırmak şunları gösterir:

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

Bu kodu başka bir URL ile çalıştırmak şunları görüntüler:

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

Bu çıkış şunları gösterir:

* `UseRouting` çağrılmadan önce uç nokta her zaman null olur.
* Bir eşleşme bulunursa, uç nokta `UseRouting` ve <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>arasında boş değildir.
* Bir eşleşme bulunduğunda `UseEndpoints` ara yazılım **terminaldir** . [Terminal ara yazılımı](#tm) bu belgede daha sonra tanımlanır.
* `UseEndpoints` sonrasında ara yazılım, yalnızca eşleşme bulunamadığında yürütülür.

`UseRouting` ara yazılımı, uç noktayı geçerli bağlama eklemek için [Setendpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*) yöntemini kullanır. `UseRouting` ara yazılımını özel mantık ile değiştirmek ve uç noktaları kullanmanın avantajlarını almaya devam etmek mümkündür. Uç noktalar, ara yazılım gibi alt düzey bir temel 'tür ve yönlendirme uygulamasıyla birlikte aktarılmaz. Çoğu uygulamanın, `UseRouting` özel mantık ile değiştirme gereksinimi yoktur.

`UseEndpoints` ara yazılımı, `UseRouting` ara yazılım ile birlikte kullanılmak üzere tasarlanmıştır. Bir uç noktayı yürütmek için çekirdek mantık karmaşık değildir. Uç noktayı almak için <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> kullanın ve sonra <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> özelliğini çağırın.

Aşağıdaki kod, ara yazılımların yönlendirmeyi nasıl etkileyebileceğini veya tepki vermesini gösterir:

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

Yukarıdaki örnekte iki önemli kavram gösterilmektedir:

* Ara yazılım, yönlendirmenin çalıştığı verileri değiştirmek için `UseRouting` önce çalışabilir.
    * Genellikle, yönlendirme öncesinde görüntülenen ara yazılım, <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>, <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>veya <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>gibi bir istek özelliğini değiştirir.
* Ara yazılım, uç nokta yürütülmeden önce yönlendirmenin sonuçlarını işlemek için `UseRouting` ile <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> arasında çalışabilir.
    * `UseRouting` ve `UseEndpoints`arasında çalışan ara yazılım:
      * Genellikle uç noktaları anlamak için meta verileri inceler.
      * Genellikle `UseAuthorization` ve `UseCors`tarafından gerçekleştirilen güvenlik kararlarını verir.
    * Ara yazılım ve meta verilerin birleşimi, ilkelerin uç nokta başına yapılandırılmasını sağlar.

Yukarıdaki kodda, uç nokta başına ilkeleri destekleyen özel bir ara yazılım örneği gösterilmektedir. Ara yazılım, gizli verilere erişim *denetim günlüğünü* konsola yazar. Ara yazılım, `AuditPolicyAttribute` meta verileri ile bir uç noktayı *denetlemek* üzere yapılandırılabilir. Bu örnek, yalnızca hassas olarak işaretlenen bitiş noktalarının denetlendiği bir *katılım* modelini gösterir. Örneğin, güvenli olarak işaretlenmemiş her şeyi denetlemek için bu mantığı ters olarak tanımlamak mümkündür. Uç nokta meta veri sistemi esnektir. Bu mantık, kullanım örneğine uygun herhangi bir şekilde tasarlanabilir.

Önceki örnek kod, uç noktaların temel kavramlarını göstermek için tasarlanmıştır. **Örnek, üretim kullanımı için tasarlanmamıştır**. *Denetim günlüğü* ara yazılımı 'nın daha kapsamlı bir sürümü şöyle olacaktır:

* Bir dosya veya veritabanında oturum açın.
* Kullanıcı, IP adresi, hassas bitiş noktasının adı ve daha fazlası gibi ayrıntıları dahil edin.

Denetim ilkesi meta verileri `AuditPolicyAttribute`, denetleyiciler ve SignalR gibi sınıf tabanlı çerçevelerle daha kolay kullanılmak üzere bir `Attribute` olarak tanımlanır. *Kod yolu*kullanırken:

* Meta veriler bir Oluşturucu API 'siyle birlikte eklenir.
* Sınıf tabanlı çerçeveler, uç noktalar oluştururken karşılık gelen Yöntem ve sınıftaki tüm öznitelikleri içerir.

Meta veri türleri için en iyi uygulamalar, bunları arabirimler ya da öznitelikler olarak tanımlamaktır. Arabirimler ve öznitelikler kod yeniden kullanılmasına izin verir. Meta veri sistemi esnektir ve herhangi bir kısıtlama vermez.

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a>Terminal ara yazılımını ve yönlendirmeyi karşılaştırma

Aşağıdaki kod örneği, yönlendirmeyi kullanarak ara yazılım kullanarak karşıtlıkları:

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

`Approach 1:` ile gösterilen ara yazılım stili, **Terminal ara yazılımı**. Bu, eşleşen bir işlem yaptığı için Terminal ara yazılımı olarak adlandırılır:

* Yukarıdaki örnekteki eşleştirme işlemi, ara yazılım için `Path == "/"` ve yönlendirme için `Path == "/Movie"`.
* Bir eşleşme başarılı olduğunda, `next` ara yazılımı çağırmak yerine bazı işlevleri yürütür ve döndürür.

Bu, aramayı sonlandırdığı, bazı işlevleri yürüttüğünde ve sonra döndürdüğünden, Terminal ara yazılım ara yazılımı olarak adlandırılır.

Terminal ara yazılımını ve yönlendirmeyi karşılaştırma:
* Her iki yaklaşım da işleme ardışık düzeninin sonlandırılmasını sağlar:
    * Ara yazılım, `next`çağırmak yerine işlem hattını döndürerek sonlandırır.
    * Uç noktalar her zaman terminaldir.
* Terminal ara yazılımı, işlem hattındaki ara yazılımı rastgele bir yerde konumlandırmayı sağlar:
    * Uç noktalar <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>konumunda yürütülür.
* Terminal ara yazılımı, rastgele kodun, ara yazılımın ne zaman eşleştiğini belirlemesine izin verir:
    * Özel yol eşleştirme kodu ayrıntılı ve doğru yazılması zor olabilir.
    * Yönlendirme, tipik uygulamalar için doğrudan çözümler sağlar. Çoğu uygulama özel yol eşleştirme kodu gerektirmez.
* `UseAuthorization` ve `UseCors`gibi ara yazılım içeren uç noktalar arabirimi.
    * `UseAuthorization` veya `UseCors` sahip bir Terminal ara yazılımı kullanmak, yetkilendirme sistemiyle el ile arabirim oluşturma gerektirir.

Bir [uç nokta](#endpoint) her ikisini de tanımlar:

* İstekleri işlemek için bir temsilci.
* Rastgele meta veri koleksiyonu. Meta veriler, her uç noktaya eklenen ilkelere ve yapılandırmaya göre çapraz kesme sorunları uygulamak için kullanılır.

Terminal ara yazılımı etkin bir araç olabilir, ancak şunları gerektirebilir:

* Önemli miktarda kodlama ve test.
* İstenen esneklik düzeyine ulaşmak için diğer sistemlerle el ile tümleştirme.

Terminal ara yazılımı yazmadan önce yönlendirme ile tümleştirmeyi düşünün.

[Eşleme](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) veya <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> tümleşen mevcut Terminal ara yazılımı, genellikle bir yönlendirme duyarlı uç noktaya açılabilir. [Maphealthdenetimleri](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16) , yönlendirici-Ware için bir model gösterir:
* <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>bir genişletme yöntemi yazın.
* <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>kullanarak iç içe geçmiş bir ara yazılım işlem hattı oluşturun.
* Yeni işlem hattına ara yazılım ekleyin. Bu durumda <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>.
* ara yazılım ardışık düzenini bir <xref:Microsoft.AspNetCore.Http.RequestDelegate><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>.
* `Map` çağırın ve yeni ara yazılım ardışık düzenini sağlayın.
* Uzantı yönteminden `Map` tarafından sunulan Oluşturucu nesnesini döndürün.

Aşağıdaki kod, [Maphealthdenetimlerinin](xref:host-and-deploy/health-checks)kullanımını gösterir:

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

Yukarıdaki örnek, Oluşturucu nesnesinin neden önemli olduğunu gösterir. Oluşturucu nesnesini döndürmek, uygulama geliştiricisinin uç nokta için yetkilendirme gibi ilkeleri yapılandırmasına izin verir. Bu örnekte, sistem durumu denetimleri ara yazılımı yetkilendirme sistemiyle doğrudan tümleştirme gerektirmez.

Meta veri sistemi, Terminal ara yazılımı kullanan genişletilebilirlik yazarları tarafından karşılaşılan sorunlara yanıt olarak oluşturulmuştur. Her bir ara yazılım, yetkilendirme sistemiyle kendi tümleştirmesini uygulamak için sorunlu olur.

<a name="urlm"></a>

### <a name="url-matching"></a>URL eşleştirme

* , Yönlendirmenin bir [uç noktaya](#endpoint)gelen istekle eşleştiği işlemdir.
* URL yolu ve üst bilgilerindeki verileri temel alır.
* İstekteki verileri göz önünde bulundurmanız için genişletilebilir.

Bir yönlendirme ara yazılımı yürütüldüğünde bir `Endpoint` ayarlar ve geçerli istekten <xref:Microsoft.AspNetCore.Http.HttpContext> bir [istek özelliğine](xref:fundamentals/request-features) yönlendirir:

* [HttpContext. GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>) çağrısı uç noktasını alır.
* `HttpRequest.RouteValues` yol değerlerinin koleksiyonunu alır.

Yönlendirme ara yazılımı [, çalışan ara yazılım uç](xref:fundamentals/middleware/index) noktasını inceleyebilir ve işlem yapabilir. Örneğin, bir yetkilendirme ara yazılımı, bir yetkilendirme ilkesi için bitiş noktasının meta veri koleksiyonunu sorgulanamıyor. İstek işleme ardışık düzeninde bulunan tüm ara yazılım yürütüldükten sonra, seçilen uç noktanın temsilcisi çağrılır.

Uç nokta yönlendirmesinde yönlendirme sistemi tüm gönderme kararlarından sorumludur. Ara yazılım, seçili uç noktaya göre ilkeler uyguladığı için şu önemlidir:

* Güvenlik ilkelerinin dağıtımını etkileyebilecek herhangi bir karar, yönlendirme sisteminin içinde yapılır.

> [!WARNING]
> Geriye dönük uyumluluk için, bir denetleyici veya Razor Pages uç noktası temsilcisi yürütüldüğünde, [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özellikleri, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlanır.
>
> `RouteContext` türü gelecekteki bir sürümde kullanımdan kalkacaktır:
>
> * `RouteData.Values` `HttpRequest.RouteValues`geçirin.
> * Uç nokta meta verilerinden [ıdatatokensmetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) almak için `RouteData.DataTokens` geçirin.

URL eşleştirme, yapılandırılabilir bir aşamalar kümesinde çalışır. Her aşamada, çıktı bir eşleşme kümesidir. Eşleşmeler kümesi bir sonraki aşamadan sonra daha dar olabilir. Yönlendirme uygulama, eşleşen uç noktalar için bir işlem sırası garantisi vermez. **Tüm** olası eşleşmeler aynı anda işlenir. URL ile eşleşen aşamalar aşağıdaki sırada oluşur. ASP.NET Core:

1. URL yolunu uç noktalar kümesine ve bunların yol şablonlarına göre işler ve **Tüm** eşleşmeleri toplar.
1. Önceki listeyi alır ve yol kısıtlamalarında başarısız olan eşleşmeleri kaldırır.
1. Önceki listeyi alır ve [Matcherpolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy) örnekleri kümesi başarısız olan eşleşmeleri kaldırır.
1. Önceki listeden son bir karar vermek için [Endpointselector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) kullanır.

Uç noktaların listesi şunlara göre önceliklendirilir:

* [Routeendpoint. Order](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)
* [Yol şablonu önceliği](#rtp)

Tüm eşleşen uç noktalar, <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> ulaşılıncaya kadar her aşamada işlenir. `EndpointSelector` son aşamadır. En iyi eşleşme ile eşleştirmelerle en yüksek öncelikli uç noktayı seçer. En iyi eşleşme ile aynı önceliğe sahip başka eşleşmeler varsa, belirsiz eşleşme özel durumu oluşur.

Yol önceliği, daha yüksek öncelikli olarak verilen **daha belirli** bir yol şablonuna göre hesaplanır. Örneğin, şablonları `/hello` ve `/{message}`göz önünde bulundurun:

* Her ikisi de `/hello`URL yoluyla eşleşir.
* `/hello` daha özeldir ve bu nedenle daha yüksek önceliğe sahiptir.

Genel olarak, rota önceliği, uygulamada kullanılan URL şemaları türleri için en iyi eşleşmeyi seçme konusunda iyi bir iş olur. Belirsizliği önlemek için <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order> kullanın.

Yönlendirme tarafından sunulan genişletilebilirlik türleri nedeniyle, yönlendirme sisteminin belirsiz yolların önünde işlem yapmak mümkün değildir. `/{message:alpha}` ve `/{message:int}`yol şablonları gibi bir örnek düşünün:

* `alpha` kısıtlaması yalnızca alfabetik karakterlerle eşleşir.
* `int` kısıtlaması yalnızca sayılarla eşleşir.
* Bu şablonlar aynı rota önceliğine sahiptir, ancak her iki eşleşen tek URL yoktur.
* Yönlendirme sistemi başlangıçta bir belirsizlik hatası bildirse, bu geçerli kullanım durumunu engeller.

> [!WARNING]
>
> <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> içindeki işlemlerin sırası, yönlendirme davranışını tek bir özel durumla etkilemez. <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ve <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>, çağrıldığı sıraya göre uç noktalarına otomatik olarak bir sipariş değeri atar. Bu, yönlendirme sistemi olmadan, eski yönlendirme uygulamalarıyla aynı garantilere sahip olmayan denetleyicilerin uzun süreli davranışlarına benzetir.
>
> Yönlendirmenin eski uygulamasında, yolların işlendiği sıraya bağımlılığı olan yönlendirme genişletilebilirliği uygulamak mümkündür. ASP.NET Core 3,0 ve üzeri için uç nokta yönlendirme:
> 
> * Bir yol kavramı yoktur.
> * Sıralama garantisi sağlamaz. Tüm uç noktalar aynı anda işlenir.
>
> Bu, eski yönlendirme sistemini kullanarak çıkdıysanız, [Yardım için bir GitHub sorunu açın](https://github.com/dotnet/aspnetcore/issues).

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a>Yol şablonu önceliği ve uç nokta seçim sırası

[Yol şablonu önceliği](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16) , her yol şablonuna, ne kadar belirli bir değere göre bir değer atayan bir sistemdir. Yol şablonu önceliği:

* Ortak durumlarda uç noktaların sırasını ayarlama ihtiyacını önler.
* Yönlendirme davranışının ortak Sense beklentilerini eşleştirmeye çalışır.

Örneğin, şablonları `/Products/List` ve `/Products/{id}`göz önünde bulundurun. `/Products/List`, URL yolu `/Products/List`için `/Products/{id}` daha iyi bir eşleşme olduğunu varsaymak mantıklı olacaktır. `/List`, değişmez değer segmentinin parametre kesiminin `/{id}`daha iyi önceliğe sahip olduğu kabul edildiği için geçerlidir.

Önceliğin nasıl kullanılacağına ilişkin ayrıntılar, yönlendirme şablonlarının nasıl tanımlandığınıza bağlıdır:

* Daha fazla kesimli şablonlar daha belirgin olarak değerlendirilir.
* Değişmez değerli bir kesim, bir parametre segmentinden daha belirgin olarak değerlendirilir.
* Kısıtlaması olan bir parametre segmenti, olmadan birden fazla olarak değerlendirilir.
* Karmaşık bir kesim, kısıtlama içeren bir parametre segmenti olarak kabul edilir.
* Tüm parametreleri yakala en az özgüdür.

Tam değerler başvurusu için [GitHub 'da kaynak koda](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189) bakın.

<a name="lg"></a>

### <a name="url-generation-concepts"></a>URL oluşturma kavramları

URL oluşturma:

* , Yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturmalarına yönelik işlemdir.
* Uç noktalar ve bunlara erişen URL 'Ler arasında bir mantıksal ayrım sağlar.

Endpoint Routing <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'sini içerir. `LinkGenerator`, [dı](xref:fundamentals/dependency-injection)tarafından kullanılabilen bir tek hizmettir. `LinkGenerator` API 'SI yürütülen bir istek bağlamı dışında kullanılabilir. [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>kullanan [Mvc. ıurlhelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper) ve senaryolar, bağlantı oluşturma ÖZELLIKLERI sağlamak için dahili olarak `LinkGenerator` API 'sini kullanır.

Bağlantı Oluşturucu, bir **Adres** ve **Adres şemaları**kavramıyla desteklenir. Adres şeması, bağlantı oluşturma için göz önünde bulundurmanız gereken uç noktaları belirlemenin bir yoludur. Örneğin, yol adı ve yol değerleri senaryoları birçok kullanıcı denetleyicilerden öğrenmekte ve Razor Pages bir adres düzeni olarak uygulanır.

Bağlantı Oluşturucu, aşağıdaki uzantı yöntemleri aracılığıyla denetleyicilere ve Razor Pages bağlanabilir:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Bu yöntemlerin aşırı yüklemeleri, `HttpContext`içeren bağımsız değişkenleri kabul eder. Bu yöntemler, [URL. Action](xref:System.Web.Mvc.UrlHelper.Action*) ve [URL. Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)ile işlevsel olarak eşdeğerdir, ancak ek esneklik ve seçenekler sunar.

`GetPath*` yöntemleri, `Url.Action` ve `Url.Page`benzerdir ve bu, mutlak bir yol içeren bir URI oluşturur. `GetUri*` Yöntemler her zaman bir düzen ve konak içeren mutlak bir URI oluşturur. Bir `HttpContext` kabul eden yöntemler, yürütülmekte olan istek bağlamında bir URI oluşturur. [Ortam](#ambient) yolu DEĞERLERI, URL taban yolu, şeması ve yürütülen istekten ana bilgisayar, geçersiz kılınmadıkça kullanılır.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> bir adresle çağrılır. URI oluşturma iki adımda gerçekleşir:

1. Adres, adresle eşleşen bir uç nokta listesine bağlanır.
1. Her uç noktanın <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>, sağlanan değerlerle eşleşen bir rota deseninin bulunana kadar değerlendirilir. Elde edilen çıktı, bağlantı oluşturucuya sağlanan diğer URI parçalarıyla birleştirilir ve döndürülür.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> tarafından sunulan yöntemler, herhangi bir tür adresin standart bağlantı oluşturma özelliklerini destekler. Bağlantı oluşturucuyu kullanmanın en kolay yolu, belirli bir adres türü için işlem gerçekleştiren genişletme yöntemlerine yöneliktir:

| Genişletme yöntemi | Açıklama |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Belirtilen değerleri temel alarak mutlak bir yola sahip bir URI oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Belirtilen değerleri temel alarak mutlak bir URI oluşturur.             |

> [!WARNING]
> <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemlerinin çağrılmasının aşağıdaki etkilerine dikkat edin:
>
> * Gelen isteklerin `Host` üstbilgisini doğrulayan bir uygulama yapılandırmasında `GetUri*` uzantısı yöntemlerini dikkatle kullanın. Gelen isteklerin `Host` üstbilgisi doğrulanmaz, güvenilir olmayan istek girdisi bir görünüm veya sayfadaki URI 'Ler içinde istemciye geri gönderilebilir. Tüm üretim uygulamalarının, `Host` üst bilgisini bilinen geçerli değerlere karşı doğrulamak için kendi sunucusunu yapılandırmasını öneririz.
>
> * `Map` veya `MapWhen`birlikte ara yazılım ile birlikte <xref:Microsoft.AspNetCore.Routing.LinkGenerator> kullanın. `Map*`, yürütülen isteğin temel yolunu değiştirir ve bu da bağlantı oluşturma çıktısını etkiler. Tüm <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'Leri temel yol belirtmeye izin verir. Bağlantı oluşturmada `Map*` etkisini geri almak için boş bir temel yol belirtin.

### <a name="middleware-example"></a>Ara yazılım örneği

Aşağıdaki örnekte, bir ara yazılım, mağaza ürünlerini listeleyen bir eylem yöntemine bağlantı oluşturmak için <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'sini kullanır. Ekleme tarafından bir sınıfa olan ve `GenerateLink` çağıran bağlantı Oluşturucuyu kullanmak, bir uygulamadaki tüm sınıflar için kullanılabilir:

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a>Rota şablonu başvurusu

`{}` içindeki belirteçler, yol eşleştirildiği takdirde bağlanan rota parametrelerini tanımlar. Yol segmentinde birden fazla yol parametresi tanımlanabilir, ancak yol parametreleri bir sabit değer ile ayrılmalıdır. Örneğin, `{controller}` ve `{action}`arasında değişmez değer olmadığından `{controller=Home}{action=Index}` geçerli bir yol değil.  Rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametreleri dışında (örneğin, `{id}`) ve yol ayırıcı `/` URL içindeki metinle eşleşmesi gerekir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Sabit yol parametresi sınırlayıcısından `{` veya `}`eşleştirmek için, karakteri tekrarlayarak sınırlayıcıdan kaçış. Örneğin `{{` veya `}}`.

Yıldız `*` veya çift yıldız `**`:

* URI 'nin geri kalanına bağlamak için bir yol parametresinin öneki olarak kullanılabilir.
* , **Catch-all** parametreleri olarak adlandırılır. Örneğin, `blog/{**slug}`:
  * `/blog` ile başlayan ve bundan sonraki bir değere sahip olan herhangi bir URI ile eşleşir.
  * `/blog` aşağıdaki değer, [başlık](https://developer.mozilla.org/docs/Glossary/Slug) yolu değerine atanır.

Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı `/` karakterler de dahil olmak üzere bir URL oluşturmak için, catch-all parametresi uygun karakterleri de çıkar. Örneğin yol `foo/{*path}` rota değerleriyle `{ path = "my/path" }` `foo/my%2Fpath`üretir. Atlanan eğik çizgiye göz önünde edin. Yol ayırıcı karakterlerini geri döndürmek için `**` Route parametresi önekini kullanın. `{ path = "my/path" }` ile yol `foo/{**path}` `foo/my/path`üretir.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, `files/{filename}.{ext?}`şablonu düşünün. `filename` ve `ext` değerlerinin her ikisi de varsa, her iki değer de doldurulur. URL 'de yalnızca `filename` için bir değer varsa, sondaki `.` isteğe bağlı olduğu için yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

Yol parametrelerinin, parametre adından sonra varsayılan değer belirtilerek bir eşittir işareti (`=`) ile ayrıldıktan sonra belirlenen **varsayılan değerleri** olabilir. Örneğin, `Home` `controller`için varsayılan değer olarak `{controller=Home}` tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, parametre adının sonuna bir soru işareti (`?`) eklenerek isteğe bağlı olarak yapılır. Örneğin, `id?`. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark şudur:

* Varsayılan değere sahip bir yol parametresi her zaman bir değer üretir.
* İsteğe bağlı bir parametre yalnızca istek URL 'SI tarafından bir değer sağlandığında bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra `:` ve kısıtlama adı eklemek, bir rota parametresinde bir satır içi kısıtlamayı belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez içine alınır `(...)`. Birden çok *satır içi kısıtlama* , başka bir `:` ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılacak bir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> örneği oluşturmak için <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}`, `10`bağımsız değişkeniyle bir `minlength` kısıtlaması belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Rota parametrelerinin ayrıca parametre dönüştürücüler de olabilir. Parametre dönüştürücüler, URL 'Ler için bağlantılar ve eşleşen eylemler ve sayfalar oluştururken parametrenin değerini dönüştürür. LIKE kısıtlamaları, yol parametre adından sonra bir `:` ve transformatör adı ekleyerek bir rota parametresine satır içi eklenebilir. Örneğin, yol şablonu `blog/{article:slugify}` bir `slugify` transformatörü belirtir. Parametre dönüştürücüler hakkında daha fazla bilgi için bkz. [Parameter transformatör başvurusu](#parameter-transformer-reference) bölümü.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir:

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca `/hello`tek yol ile eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | `Home``Page` eşleşir ve ayarlar.                                         |
| `{Page=Home}`                            | `/Contact`              | `Contact``Page` eşleşir ve ayarlar.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` denetleyicisi ve `List` eylemiyle eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | `Products` denetleyicisi ve `Details` eylemiyle eşlenir`id`, 123 olarak ayarlanır. |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | `Home` denetleyicisi ve `Index` yöntemi ile eşlenir. `id` yoksayıldı.        |
| `{controller=Home}/{action=Index}/{id?}` | `/Products`         | `Products` denetleyicisi ve `Index` yöntemi ile eşlenir. `id` yoksayıldı.        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

### <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler, en çok [greksuz olmayan](#greedy) bir şekilde, sabit değer sınırlayıcılarını sağdan sola eşleştirerek işlenir. Örneğin, `[Route("/a{b}c{d}")]` karmaşık bir kesimdir.
Karmaşık segmentler, onları başarılı bir şekilde kullanmak için anlaşılması gereken belirli bir şekilde çalışır. Bu bölümdeki örnek, sınırlayıcı metnin parametre değerleri içinde görünmemesinin neden karmaşık segmentlerin gerçekten iyi şekilde çalıştığını gösterir. Bir [Regex](/dotnet/standard/base-types/regular-expressions) kullanarak ve daha karmaşık durumlar için değerleri el ile ayıklamanız gerekir.

Bu, yönlendirmenin `/a{b}c{d}` şablon ile gerçekleştirdiği adımların bir özetidir ve URL yolu `/abcd`. `|`, algoritmanın nasıl çalıştığını görselleştirmenize yardımcı olmak için kullanılır:

* İlk sabit değer, sağdan sola `c`. `/abcd`, sağdan aranmıştır ve `/ab|c|d`bulur.
* Sağ taraftaki her şey (`d`) `{d}`yol parametresi ile eşleştirilir.
* Sonraki değişmez değer, sağdan sola `a`. `/ab|c|d` ayrıldığımızda başlayan `a` `/|a|b|c|d`bulunur.
* Sağdaki değer (`b`), artık `{b}`rota parametresiyle eşleştirilir.
* Kalan bir metin yok ve kalan yol şablonu yok, bu nedenle bu bir eşleşmedir.

Aynı şablon `/a{b}c{d}` ve `/aabcd`URL yolu kullanılarak negatif bir durum örneği aşağıda verilmiştir. `|`, algoritmanın nasıl çalıştığını görselleştirmeye yardımcı olmak için kullanılır. Bu durum, aynı algoritma tarafından açıklanan bir eşleşme değildir:
* İlk sabit değer, sağdan sola `c`. `/aabcd`, sağdan aranmıştır ve `/aab|c|d`bulur.
* Sağ taraftaki her şey (`d`) `{d}`yol parametresi ile eşleştirilir.
* Sonraki değişmez değer, sağdan sola `a`. `/aab|c|d` ayrıldığımızda başlayan `a` `/a|a|b|c|d`bulunur.
* Sağdaki değer (`b`), artık `{b}`rota parametresiyle eşleştirilir.
* Bu noktada, `a`kalan metin vardır ancak algoritmanın ayrıştırılacak yol şablonu kalmadı, bu nedenle bu bir eşleşme değildir.

Eşleşen algoritma [greyumsuz](#greedy)olmadığından:

* Her adımda mümkün olan en küçük metin miktarıyla eşleşir.
* Sınırlayıcı değerin parametre değerleri içinde göründüğü herhangi bir durum, eşleşmemelidir.

Normal ifadeler, eşleşen davranışları üzerinde çok daha fazla denetim sağlar.

<a name="greedy"></a>

Aynı zamanda [yavaş eşleşme](https://wikipedia.org/wiki/Regular_expression#Lazy_matching)olarak da bilinen Greedy eşleştirmesi, olası en büyük dizeyle eşleşir. Doyumsuz olmayan dize, olası en küçük dizeyle eşleşir.

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Yol kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda doğru veya yanlış bir karar vermez. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint>, HTTP fiiline bağlı olarak bir isteği kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> Giriş doğrulaması için kısıtlamaları kullanmayın. Giriş doğrulaması için kısıtlamalar kullanılıyorsa, geçersiz giriş sonuçları `404` bulunamayan bir Yanıt ile sonuçlanır. Geçersiz giriş, uygun bir hata iletisiyle `400` hatalı bir Istek üretmelidir. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların belirsizliğini ortadan kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve beklenen davranışlar gösterilmektedir:

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya `false`eşleşir. Büyük/küçük harf duyarsız |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Sabit kültürün geçerli bir `DateTime` değeriyle eşleşir. Önceki uyarıya bakın. |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Sabit kültürün geçerli bir `decimal` değeriyle eşleşir. Önceki uyarıya bakın.|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `double` değeriyle eşleşir. Önceki uyarıya bakın.|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `float` değeriyle eşleşir. Önceki uyarıya bakın.|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | Geçerli bir `Guid` değeriyle eşleşir |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir `long` değeriyle eşleşir |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | Dize 8 karakterden uzun olmamalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 ve en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri 120 ' ten fazla olmamalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 olmalı ancak 120 ' ten fazla olmamalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır, `a`-`z` ve büyük/küçük harfe duyarsız olmalıdır. |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir. Normal ifade tanımlama hakkında ipuçlarına bakın. |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır |

[!INCLUDE[](~/includes/regex.md)]

Birden çok, iki nokta üst üste sınırlı kısıtlama, tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'YI doğrulayan ve CLR türüne dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Örneğin, CLR türüne dönüştürme `int` veya `DateTime`. Bu kısıtlamalar, URL 'nin yerelleştirilebilir olmadığını varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

### <a name="regular-expressions-in-constraints"></a>Kısıtlamalarda normal ifadeler

[!INCLUDE[](~/includes/regex.md)]

Normal ifadeler `regex(...)` Route kısıtlaması kullanılarak satır içi kısıtlamalar olarak belirtilebilir. <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> ailesindeki metotlar ayrıca Kısıtlamaların bir nesne sabit değerini de kabul eder. Bu form kullanılıyorsa, dize değerleri normal ifadeler olarak yorumlanır.

Aşağıdaki kod, bir satır içi Regex kısıtlaması kullanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

Aşağıdaki kod, bir Regex kısıtlaması belirtmek için bir nesne değişmez değeri kullanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. Bir satır içi kısıtlamadaki normal ifade `^\d{3}-\d{2}-\d{4}$` kullanmak için aşağıdakilerden birini kullanın:

* Dizedeki `\` karakterleri, `\` String kaçış karakterinden kaçınmak için C# kaynak dosyada `\\` karakter olarak değiştirin.
* Tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string).

Yönlendirme parametresi sınırlayıcı karakterlerinin `{`, `}`, `[`, `]`, ifadedeki karakterleri çift tırnak içine almak için `{{`, `}}`, `[[`, `]]`. Aşağıdaki tabloda, bir normal ifade ve bunun kaçış sürümü gösterilmektedir:

| Normal ifade    | Kaçan normal ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle `^` karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler genellikle `$` karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır:

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | herkese     | Hayır    | Yukarıdaki `^` ve `$` bakın |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Yukarıdaki `^` ve `$` bakın |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. Örneğin `{action:regex(^(list|get|create)$)}`, yalnızca `action` Route değeri `list`, `get`veya `create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğünde geçirilen kısıtlamalar, normal ifadeler olarak da değerlendirilir. Bilinen kısıtlamaların biriyle eşleşmeyen bir şablon içinde geçirilen kısıtlamalar normal ifadeler olarak değerlendirilmez.

### <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Özel yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak oluşturulabilir. `IRouteConstraint` arabirimi, kısıtlama karşılandıysanız `true` döndüren <xref:System.Web.Routing.IRouteConstraint.Match*>içerir ve aksi takdirde `false`.

Özel yol kısıtlamaları nadiren gereklidir. Özel bir yol kısıtlaması uygulamadan önce, model bağlama gibi alternatifleri göz önünde bulundurun.

Özel bir `IRouteConstraint`kullanmak için yol kısıtlama türü, uygulamanın hizmet kapsayıcısında <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kayıtlı olmalıdır. `ConstraintMap`, yol kısıtlama anahtarlarını bu kısıtlamaları doğrulayan `IRouteConstraint` uygulamalarla eşleyen bir sözlüktür. Bir uygulamanın `ConstraintMap`, bir hizmetin parçası olarak `Startup.ConfigureServices` güncelleştirilebilen olabilir [. AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya <xref:Microsoft.AspNetCore.Routing.RouteOptions> doğrudan `services.Configure<RouteOptions>`ile yapılandırma. Örneğin:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

Önceki kısıtlama aşağıdaki kodda uygulanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[Mydisplayrouteınfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x/RoutingSample/Extensions/ControllerContextExtensions.cs) yöntemi [örnek karşıdan yüklemeye](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) dahildir ve yönlendirme bilgilerini görüntülemek için kullanılır.

`MyCustomConstraint` uygulanması `0` bir yönlendirme parametresine uygulanmasını engeller:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

Yukarıdaki kod:

* Yolun `{id}` kesimindeki `0` engeller.
* , Özel bir kısıtlamayı uygulamaya yönelik temel bir örnek sağlamak için gösterilmektedir. Bu, bir üretim uygulamasında kullanılmamalıdır.

Aşağıdaki kod, `0` içeren `id` işlenmesini önlemek için daha iyi bir yaklaşımdır:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

Yukarıdaki kod `MyCustomConstraint` yaklaşımına göre aşağıdaki avantajlara sahiptir:

* Özel bir kısıtlama gerektirmez.
* Yol parametresi `0`içerdiğinde daha açıklayıcı bir hata döndürür.

## <a name="parameter-transformer-reference"></a>Parametre transformatörü başvurusu

Parametre dönüştürücüler:

* <xref:Microsoft.AspNetCore.Routing.LinkGenerator>kullanarak bağlantı oluştururken yürütün.
* <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>uygulayın.
* <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>kullanılarak yapılandırılır.
* Parametrenin yol değerini alın ve yeni bir dize değerine dönüştürün.
* Oluşturulan bağlantıda Dönüştürülmüş değerin kullanılmasına neden olacak.

Örneğin, `Url.Action(new { article = "MyTestArticle" })` ile `blog\{article:slugify}` yol düzeninde özel `slugify` parametresi transformatörü `blog\my-test-article`üretir.

Aşağıdaki `IOutboundParameterTransformer` uygulamasını göz önünde bulundurun:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

Bir parametre transformatörü bir yol düzeninde kullanmak için, `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kullanarak yapılandırın:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

ASP.NET Core Framework, bir uç noktanın çözümlediği URI 'yi dönüştürmek için parametre dönüştürücüler kullanır. Örneğin, parametre dönüştürücüler bir `area`, `controller`, `action`ve `page`eşleştirmek için kullanılan rota değerlerini dönüştürür.

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Önceki yol şablonuyla eylem `SubscriptionManagementController.GetAll`, URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıkışları `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla parametre dönüştürücüler kullanmak için API kuralları sağlar:

* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC kuralı, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API kuralını kullanır. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

<a name="ugr"></a>

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Bu bölüm, URL oluşturma tarafından uygulanan algoritmanın başvurusunu içerir. Uygulamada, URL oluşturmaya yönelik çoğu karmaşık örnek, denetleyiciler veya Razor Pages kullanır. Daha fazla bilgi için bkz. [denetleyicilerde yönlendirme](xref:mvc/controllers/routing) .

URL oluşturma işlemi [Linkgenerator. GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*) veya benzer bir yöntem çağrısıyla başlar. Yöntemi, bir adres, bir rota değerleri kümesi ve isteğe bağlı olarak `HttpContext`geçerli istek hakkında bilgi ile birlikte sağlanır.

İlk adım, adresin türüyle eşleşen bir [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) kullanarak bir aday uç nokta kümesini çözümlemek için adresi kullanmaktır.

Adres kümesi tarafından bir aday kümesini bulduktan sonra, uç noktalar bir URL oluşturma işlemi başarılı olana kadar sıralanır ve işlenir. URL oluşturma belirsizlikleri için Denetim **yapmaz** , döndürülen ilk sonuç son sonuçdur.

### <a name="troubleshooting-url-generation-with-logging"></a>Günlüğe kaydetme ile URL oluşturma sorunlarını giderme

URL oluşturma sorunlarını gidermeye yönelik ilk adım, `Microsoft.AspNetCore.Routing` günlük düzeyini `TRACE`olarak ayarlıyor. `LinkGenerator`, işleme hakkında birçok ayrıntıyı günlüğe kaydeder ve bu da sorunları gidermek için yararlı olabilir.

URL oluşturma hakkında ayrıntılı bilgi için bkz. [URL oluşturma başvurusu](#ugr) .

### <a name="addresses"></a>Adresler

Adresler, bağlantı oluşturucuya bir çağrıyı bir aday uç noktası kümesine bağlamak için kullanılan URL oluşturma kavramıdır.

Adresler, varsayılan olarak iki uygulama ile birlikte gelen Genişletilebilir bir kavramdır:

* Şu adres olarak *uç nokta adı* (`string`) kullanılıyor:
    * MVC 'nin yol adına benzer işlevsellik sağlar.
    * <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> meta veri türünü kullanır.
    * Belirtilen dizeyi tüm kayıtlı uç noktaların meta verilerinde çözer.
    * Birden fazla uç nokta aynı adı kullanıyorsa, başlangıçta bir özel durum oluşturur.
    * Denetleyiciler ve Razor Pages dışında genel amaçlı kullanım için önerilir.
* Adres olarak *yol değerlerini* (<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>) kullanma:
    * Denetleyicilere benzer işlevler sağlar ve eski URL üretimini Razor Pages.
    * Genişletmek ve hata ayıklamak için çok karmaşık.
    * `IUrlHelper`, etiket yardımcıları, HTML Yardımcıları, eylem sonuçları vb. tarafından kullanılan uygulamayı sağlar.

Adres şemasının rolü, rastgele ölçütlere göre adres ve eşleşen uç noktalar arasındaki ilişkiyi oluşturmak için kullanılır:

* Uç nokta adı şeması, temel bir sözlük araması gerçekleştirir.
* Yol değerleri şeması, set algoritmasının karmaşık bir en iyi alt kümesine sahiptir.

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a>Çevresel değerler ve açık değerler

Yönlendirme geçerli istekten, geçerli isteğin rota değerlerine erişir `HttpContext.Request.RouteValues`. Geçerli istekle ilişkili değerler **çevresel değerler**olarak adlandırılır. Netlik amacı doğrultusunda, belgeler yöntemlere geçirilen yol değerlerini **açık değerler**olarak ifade eder.

Aşağıdaki örnek, ortam değerlerini ve açık değerleri gösterir. Geçerli istekten ve açık değerlerden çevresel değerler sağlar: `{ id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

Yukarıdaki kod:

* `/Widget/Index/17` döndürür
* [Dı](xref:fundamentals/dependency-injection)aracılığıyla <xref:Microsoft.AspNetCore.Routing.LinkGenerator> alır.

Aşağıdaki kod hiçbir çevresel değer ve açık değer sağlamaz: `{ controller = "Home", action = "Subscribe", id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

Önceki yöntem `/Home/Subscribe/17` döndürür

`WidgetController` aşağıdaki kod `/Widget/Subscribe/17`döndürür:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

Aşağıdaki kod, denetleyiciyi geçerli istekteki çevresel değerlerden ve açık değerlerde sağlar: `{ action = "Edit", id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

Önceki kodda:

* `/Gadget/Edit/17` döndürülür.
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url> <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>alır.
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
bir eylem yöntemi için mutlak bir yol içeren bir URL oluşturur. URL, belirtilen `action` adı ve `route` değerlerini içerir.

Aşağıdaki kod, geçerli istekten ve açık değerlerden çevresel değerler sağlar: `{ page = "./Edit, id = 17, }`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

`url`, Razor Düzenle sayfası aşağıdaki sayfa yönergesini içerdiğinde Yukarıdaki kod `/Edit/17` olarak ayarlanır:

 `@page "{id:int}"`

Düzenleme sayfası `"{id:int}"` yol şablonu içermiyorsa, `url` `/Edit?id=17`.

MVC 'nin <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> davranışı, burada açıklanan kurallara ek olarak bir karmaşıklık katmanı ekler:

* `IUrlHelper` her zaman geçerli istekten çevresel değerler olarak yol değerlerini sağlar.
* [Iurlhelper. Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*) , geliştirici tarafından geçersiz kılınmadığı sürece, her zaman geçerli `action` ve `controller` rota değerlerini açık değerler olarak kopyalar.
* [Iurlhelper. Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*) , geçersiz kılınmadığı sürece geçerli `page` rota değerini açık bir değer olarak kopyalar. <!--by the user-->
* `IUrlHelper.Page` her zaman geçersiz kılınmadığı sürece açık değerler olarak geçerli `handler` rota değerini `null` geçersiz kılar.

MVC kendi kurallarını takip ettiğinden, kullanıcılar genellikle çevresel değerlerinin davranış ayrıntılarına göre görünür. Geçmiş ve uyumluluk nedenleriyle, `action`, `controller`, `page`ve `handler` gibi bazı rota değerlerinin kendi özel durum davranışları vardır.

`LinkGenerator.GetPathByAction` ve `LinkGenerator.GetPathByPage` tarafından sunulan eşdeğer işlevsellik, uyumluluk için `IUrlHelper` Bu bozuklukları çoğaltır.

### <a name="url-generation-process"></a>URL oluşturma işlemi

Aday uç noktalar kümesi bulunduğunda, URL oluşturma algoritması:

* Son noktaları yinelemeli olarak işler.
* İlk başarılı sonucu döndürür.

Bu işlemdeki ilk adıma **Rota değeri ınvalidation**adı verilir.  Rota değeri geçersiz kılma, yönlendirmenin çevresel değerlerden hangi rota değerlerinin kullanılması gerektiğini ve göz ardı edileceğine yönelik bir işlemdir. Her ortam değeri, açık değerlerle birlikte değerlendirilir ve yok sayılır.

Ortam değerlerinin rolünü düşünmenin en iyi yolu, bazı yaygın durumlarda, yazma uygulama geliştiricilerinin kaydedilmesini denedikleri değerlerdir. Geleneksel olarak, ortam değerlerinin yararlı olduğu senaryolar MVC ile ilgilidir:

* Aynı denetleyicideki başka bir eyleme bağlantı sırasında, denetleyicinin adının belirtilmesi gerekmez.
* Aynı alandaki başka bir denetleyiciye bağlanırken, alan adının belirtilmesi gerekmez.
* Aynı eylem yöntemine bağlanırken, rota değerlerinin belirtilmesi gerekmez.
* Uygulamanın başka bir bölümüne bağlanırken, uygulamanın o bölümünde anlamı olmayan rota değerlerini yürütmek istemezsiniz.

`null` döndüren `LinkGenerator` veya `IUrlHelper` çağrıları genellikle yol değeri doğrulaması anlaşılmadığında oluşur. Yolun sorunu çözüp çözmediğini görmek için daha fazla yol değeri belirtip yol değerlerini geçersiz kılma sorunlarını giderin.

Rota değeri geçersiz kılma, uygulamanın URL şeması, soldan sağa doğru oluşturulmuş bir hiyerarşi ile hiyerarşik olduğu varsayımıyla birlikte çalışıyor. Bunun uygulamada nasıl çalıştığı hakkında sezgisel bir fikir almak için temel denetleyici yönlendirme şablonunu `{controller}/{action}/{id?}` göz önünde bulundurun. Bir değer **değişikliği** , sağda görünen tüm rota değerlerini **geçersiz kılar** . Bu, hiyerarşiyle ilgili varsayımını yansıtır. Uygulamanın `id`için bir ortam değeri varsa ve işlem `controller`için farklı bir değer belirtiyorsa:

* `id`, `{controller}` `{id?}`solunda olduğundan yeniden kullanılamayacak.

Bu ilkeyi gösteren bazı örnekler:

* Açık değerler `id`için bir değer içeriyorsa, `id` ortam değeri yok sayılır. `controller` ve `action` 'in çevresel değerleri kullanılabilir.
* Açık değerler `action`için bir değer içeriyorsa, `action` için herhangi bir çevresel değer yok sayılır. `controller` için çevresel değerler kullanılabilir. `action` için açık değer `action`ortam değerinden farklıysa `id` değeri kullanılmaz.  `action` için açık değer `action`ortam değeri ile aynıysa `id` değeri kullanılabilir.
* Açık değerler `controller`için bir değer içeriyorsa, `controller` için herhangi bir çevresel değer yok sayılır. `controller` için açık değer `controller`ortam değerinden farklıysa, `action` ve `id` değerleri kullanılmaz. `controller` için açık değer `controller`ortam değeri ile aynıysa, `action` ve `id` değerleri kullanılabilir.

Bu işlem, öznitelik yollarının ve adanmış geleneksel yolların varlığı tarafından daha karmaşıktır. `{controller}/{action}/{id?}` gibi denetleyici geleneksel yolları yol parametreleri kullanarak bir hiyerarşi belirtir. [Özel geleneksel yollar](xref:mvc/controllers/routing#dcr) ve denetleyicilere yönelik [öznitelik yolları](xref:mvc/controllers/routing#ar) ve Razor Pages için:

* Yol değerlerinin bir hiyerarşisi vardır.
* Bunlar şablonda görünmez.

Bu gibi durumlarda, URL oluşturma **gerekli değerler** kavramını tanımlar. Denetleyiciler ve Razor Pages tarafından oluşturulan bitiş noktaları, rota değeri geçersiz kılma çalışmasına izin veren gerekli değerlere sahip.

Ayrıntı olarak yol değeri geçersiz kılma algoritması:

* Gerekli değer adları, yol parametreleriyle birleştirilir ve soldan sağa işlenir.
* Her parametre için çevresel değer ve açık değer karşılaştırılır:
    * Çevresel değer ve açık değer aynıysa işlem devam eder.
    * Çevresel değer varsa ve açık değer değilse, URL oluşturulurken çevresel değer kullanılır.
    * Çevresel değer yoksa ve açık değer ise, ortam değerini ve sonraki tüm ortam değerlerini reddedin.
    * Çevresel değer ve açık değer varsa ve iki değer farklıysa, ortam değerini ve sonraki tüm ortam değerlerini reddedin.

Bu noktada, URL oluşturma işlemi yol kısıtlamalarını değerlendirmek için hazırlayın. Kabul edilen değerler kümesi, kısıtlamalara sunulan parametre varsayılan değerleriyle birleştirilir. Kısıtlamaların tümü başarılı olursa işlem devam eder.

Ardından, **kabul edilen değerler** yol şablonunu genişletmek için kullanılabilir. Yol şablonu işlenir:

* Soldan sağa.
* Her parametrenin kabul edilen değeri değiştirildi.
* Aşağıdaki özel durumlar ile:
  * Kabul edilen değerlerde bir değer eksikse ve parametrenin varsayılan değeri varsa, varsayılan değer kullanılır.
  * Kabul edilen değerlerde bir değer eksikse ve parametresi isteğe bağlıdır, işleme devam eder.
  * Eksik bir isteğe bağlı parametrenin sağında herhangi bir rota parametresinin değeri varsa, işlem başarısız olur.
  * <!-- review default-valued parameters optional parameters --> Ardışık varsayılan değerli parametreler ve isteğe bağlı parametreler mümkün olduğunda daraltılır.

Yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda `{controller}/{action}/{id?}`yol şablonu kullanılırken sonuç gösterilmektedir.

| Çevresel değerler                     | Açık değerler                        | Sonuç                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| denetleyici = "giriş"                | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | denetleyici = "Order", Action = "hakkında" | `/Order/About`          |
| denetleyici = "giriş", renk = "kırmızı" | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | Action = "hakkında", color = "Red"        | `/Home/About?color=Red` |

### <a name="problems-with-route-value-invalidation"></a>Rota değeri geçersiz kılma sorunları

ASP.NET Core 3,0 itibariyle, önceki ASP.NET Core sürümlerde kullanılan bazı URL oluşturma şemaları, URL oluşturma ile iyi çalışmaz. ASP.NET Core takım, gelecekteki sürümlerde bu ihtiyaçları karşılamak için özellik eklemeyi planlıyor. Şimdilik en iyi çözüm eski yönlendirmeyi kullanmaktır.

Aşağıdaki kod, yönlendirme tarafından desteklenmeyen bir URL oluşturma şemasına bir örnek gösterir.

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

Yukarıdaki kodda `culture` Route parametresi yerelleştirme için kullanılır. Bunun için `culture` parametresi her zaman çevresel değer olarak kabul edilir. Ancak, gerekli değerlerin çalışma biçimi nedeniyle `culture` parametresi bir çevresel değer olarak kabul edilmez:

* `"default"` Route şablonunda, `culture` Route parametresi `controller`solunda bulunur, bu nedenle `controller` yapılan değişiklikler `culture`geçersiz kılmaz.
* `"blog"` Route şablonunda, `culture` Route parametresi, gerekli değerlerde görüntülenen `controller`sağında kabul edilir.

## <a name="configuring-endpoint-metadata"></a>Uç nokta meta verilerini yapılandırma

Aşağıdaki bağlantılar, uç nokta meta verilerini yapılandırma hakkında bilgi sağlar:

* [Uç nokta yönlendirme ile CORS 'yi etkinleştirme](xref:security/cors#enable-cors-with-endpoint-routing)
* Özel bir `[MinimumAgeAuthorize]` özniteliği kullanan [ıauthorizationpolicyprovider örneği](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [[Yetkilendir] özniteliğiyle test kimlik doğrulaması](xref:security/authentication/identity#test-identity)
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* [[Yetkilendir] özniteliğiyle düzeni seçme](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)
* [[Yetkilendir] özniteliği kullanılarak ilkeler uygulanıyor](xref:security/authorization/policies#applying-policies-to-mvc-controllers)
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a>RequireHost ile yollarla eşleşen ana bilgisayar

<xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>, belirtilen Konağı gerektiren rotaya bir kısıtlama uygular. `RequireHost` veya [[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute) parametresi şu olabilir:

* Ana bilgisayar: `www.domain.com`, tüm bağlantı noktaları ile `www.domain.com` eşleşir.
* Joker karakterle ana bilgisayar: `*.domain.com`, tüm bağlantı noktaları üzerinde `www.domain.com`, `subdomain.domain.com`veya `www.subdomain.domain.com` eşleşir.
* Bağlantı noktası: `*:5000`, bağlantı noktası 5000 ile herhangi bir konak ile eşleşir.
* Konak ve bağlantı noktası: `www.domain.com:5000` veya `*.domain.com:5000`, ana bilgisayar ve bağlantı noktasıyla eşleşir.

`RequireHost` veya `[Host]`kullanılarak birden çok parametre belirtilebilir. Kısıtlama, parametrelerden herhangi biri için geçerli olan konaklarla eşleşir. Örneğin, `[Host("domain.com", "*.domain.com")]` `domain.com`, `www.domain.com`ve `subdomain.domain.com`eşleşir.

Aşağıdaki kod, rotada belirtilen ana bilgisayarı gerektirmek için `RequireHost` kullanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

Aşağıdaki kod, belirtilen konaklardan herhangi birini gerektirmek için denetleyicinin `[Host]` özniteliğini kullanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

`[Host]` özniteliği hem denetleyici hem de eylem yöntemine uygulandığında:

* Eylem üzerindeki özniteliği kullanılır.
* Denetleyici özniteliği yok sayılır.

## <a name="performance-guidance-for-routing"></a>Yönlendirme için performans Kılavuzu

Yönlendirmenin çoğu, performansı artırmak için ASP.NET Core 3,0 ' de güncelleştirildi.

Bir uygulamada performans sorunları olduğunda, yönlendirme genellikle sorun olarak şüpheli. Yönlendirme nedeni, denetleyiciler ve Razor Pages gibi çerçevelerin, kendi günlük iletilerinde çerçeve içinde harcanan süreyi raporlamadır. Denetleyiciler tarafından bildirilen zaman ve isteğin toplam süresi arasında önemli bir fark olduğunda:

* Geliştiriciler, uygulamanın kodunu sorunun kaynağı olarak ortadan kaldırır.
* Yönlendirmenin neden olduğunu varsaymak yaygındır.

Yönlendirme, binlerce uç nokta kullanılarak performansa göre test edilir. Tipik bir uygulamanın çok büyük bir performans sorunuyla karşılaşmasının pek olası olması normaldir. Yavaş yönlendirme performansının en yaygın temel nedenlerinden biri genellikle hatalı davranan özel bir ara yazılım olur.

Aşağıdaki kod örneğinde, gecikme kaynağını daraltmak için temel bir teknik gösterilmektedir:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

Zaman yönlendirme:

* Yukarıdaki kodda gösterilen zamanlama ara yazılımı kopyasıyla her bir ara yazılımı ayırma.
* Zamanlama verilerinin kodla ilişkilendirilmesi için benzersiz bir tanımlayıcı ekleyin.

Bu, örneğin `10ms`çok önemli olduğunda gecikmeyi daraltmak için temel bir yoldur.  `Time 1` `Time 2` çıkarmak `UseRouting` ara yazılımı içinde harcanan süreyi raporlar.

Aşağıdaki kod, önceki zamanlama koduna yönelik daha küçük bir yaklaşım kullanır:

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a>Potansiyel olarak pahalı yönlendirme özellikleri

Aşağıdaki listede, temel yol şablonlarıyla karşılaştırıldığında görece maliyetli yönlendirme özellikleri hakkında bazı Öngörüler sunulmaktadır:

* Normal ifadeler: karmaşık ifadeler yazmak veya az miktarda giriş ile uzun süre çalışma süresi olması mümkündür.

* Karmaşık segmentler (`{x}-{y}-{z}`): 
  * , Normal bir URL yolu segmentini ayrıştırmaktan önemli ölçüde daha pahalıdır.
  * Daha fazla sayıda alt dizenin ayrılması sonucunu elde edin.
  * Karmaşık kesim mantığı ASP.NET Core 3,0 yönlendirme performansı güncelleştirmesinde güncelleştirilmedi.

* Zaman uyumlu veri erişimi: birçok karmaşık uygulamanın, yönlendirmenin bir parçası olarak veritabanı erişimi vardır. ASP.NET Core 2,2 ve önceki yönlendirme, veritabanı erişim yönlendirmesini desteklemek için doğru genişletilebilirlik noktaları sağlamayabilir. Örneğin, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>ve <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> zaman uyumludur. <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> ve <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> gibi genişletilebilirlik noktaları zaman uyumsuzdur.

## <a name="guidance-for-library-authors"></a>Kitaplık yazarları için kılavuz

Bu bölüm, yönlendirmenin üzerine binen üst düzey kitaplık yazarları için yönergeler içerir. Bu ayrıntılar, uygulama geliştiricilerinin yönlendirmeyi genişleten kitaplıkları ve çerçeveleri kullanarak iyi bir deneyim aldığından emin olmak için tasarlanmıştır.

### <a name="define-endpoints"></a>Uç noktaları tanımlama

URL eşleştirme için yönlendirmeyi kullanan bir çerçeve oluşturmak için, <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>en üstünde yapı yapan bir kullanıcı deneyimi tanımlayarak başlayın.

<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>üstünde derleme **yapın** . Bu, kullanıcıların kendi çatısını diğer ASP.NET Core özellikleriyle karışıklık olmadan oluşturma olanağı sağlar. Her ASP.NET Core şablonu yönlendirme içerir. Yönlendirmenin mevcut olduğunu ve kullanıcılara tanıdık olduğunu varsayın.

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

<xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>uygulayan `MapMyFramework(...)` çağrısından bir Sealed somut **türü döndürün.** Çoğu Framework `Map...` yöntemi bu kalıbı izler. `IEndpointConventionBuilder` arabirimi:

* Meta verilerin bir kısmını bilme olanağı tanır.
* , Çeşitli uzantı yöntemleriyle hedeflenmelidir.

Kendi türünü bildirmek, çerçeveye özgü işlevselliği oluşturucuya eklemenize olanak tanır. Çerçeve tarafından tanımlanan bir oluşturucuyu sarmadan ve çağrıları iletecek.

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

Kendi <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>yazmayı **düşünün** . `EndpointDataSource`, uç nokta koleksiyonunu bildirmek ve güncelleştirmek için alt düzey temel değer. `EndpointDataSource`, denetleyiciler ve Razor Pages tarafından kullanılan güçlü bir API 'dir.

Yönlendirme testlerinin, güncelleştirme olmayan bir veri kaynağına ilişkin [temel bir örneği](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17) vardır.

Varsayılan olarak bir `EndpointDataSource` **kaydetmeyi denemeyin** . Kullanıcıların çatısını <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>kaydetmesini isteyin. Yönlendirmenin felseı, varsayılan olarak hiçbir şey dahil değildir ve bu `UseEndpoints` uç noktaların kaydedileceği yerdir.

### <a name="creating-routing-integrated-middleware"></a>Yönlendirme ile tümleşik ara yazılım oluşturma

Meta veri türlerini arabirim olarak tanımlamayı **düşünün** .

Meta veri türlerini sınıflar ve yöntemlerde öznitelik olarak kullanmayı **mümkün hale getirir** .

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

Denetleyiciler ve Razor Pages gibi çerçeveler, meta veri özniteliklerini türlere ve yöntemlere uygulamayı destekler. Meta veri türleri bildirirseniz:

* Bunları [öznitelik](/dotnet/csharp/programming-guide/concepts/attributes/)olarak erişilebilir yapın.
* Çoğu Kullanıcı öznitelikleri uygulamayla tanıdık gelecektir.

Bir arabirim olarak bir meta veri türü bildirmek, başka bir esneklik katmanı ekler:

* Arabirimler birleştirilebilir.
* Geliştiriciler, birden çok ilkeyi birleştiren kendi türlerini bildirebilir.

Aşağıdaki örnekte gösterildiği gibi meta verileri **geçersiz kılmak mümkün hale gelir** :

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

Bu yönergeleri izlemek için en iyi yol, **işaretleyici meta verileri**tanımlamayı önleyemaktır:

* Yalnızca meta veri türünün varolup olmadığını göz atın.
* Meta verilerde bir özellik tanımlayın ve özelliği denetleyin.

Meta veri koleksiyonu sıralanmıştır ve önceliğe göre geçersiz kılmayı destekler. Denetleyiciler söz konusu olduğunda, eylem yöntemindeki meta veriler en özeldir.

Ara yazılımı, yönlendirme olmadan ve ile **yararlı hale getirin** .

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

Bu yönergeye örnek olarak `UseAuthorization` ara yazılımını göz önünde bulundurun. Yetkilendirme ara yazılımı, bir geri dönüş ilkesi geçirmenize olanak sağlar. <!-- shown where?  (shown here) --> Belirtilmişse, geri dönüş ilkesi her ikisi için de geçerlidir:

* Belirli bir ilke olmadan uç noktalar.
* Bir uç noktasıyla eşleşmeyen istekler.

Bu, yetkilendirme ara yazılımını yönlendirme bağlamı dışında yararlı hale getirir. Yetkilendirme ara yazılımı geleneksel ara yazılım programlama için kullanılabilir.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Yönlendirme, istek URI 'Lerini uç noktalarla eşleştirmekten ve gelen istekleri bu uç noktalara gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yönlendirme bilgilerini kullanarak, yönlendirme, uç noktalarıyla eşlenen URL 'Ler de oluşturabilir.

ASP.NET Core 2,2 ' de en son yönlendirme senaryolarını kullanmak için `Startup.ConfigureServices`MVC Hizmetleri kaydının [Uyumluluk sürümünü](xref:mvc/compatibility-version) belirtin:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> seçeneği, yönlendirmenin, ASP.NET Core 2,1 veya önceki bir sürümünün uç nokta tabanlı mantık veya <xref:Microsoft.AspNetCore.Routing.IRouter>tabanlı mantığını dahili olarak kullanması gerektiğini belirler. Uyumluluk sürümü 2,2 veya üzeri bir değere ayarlandığında, varsayılan değer `true`olur. Önceki yönlendirme mantığını kullanmak için değeri `false` olarak ayarlayın:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<xref:Microsoft.AspNetCore.Routing.IRouter>tabanlı yönlendirme hakkında daha fazla bilgi için [Bu konunun ASP.NET Core 2,1 sürümüne](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)bakın.

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında daha fazla bilgi için bkz. <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz. <xref:razor-pages/razor-pages-conventions>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, genellikle [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda uygulamanın yüksek trafikte daha fazla terse yolu ekler. Özelleştirilmiş durumlar örnekleri, blog ve eticaret uç noktaları içerir.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, örneğin, al ve POST gibi birçok işlemin aynı mantıksal kaynakta aynı URL 'YI kullanması anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, bir uygulamadaki mantıksal uç noktaları temsil etmek için *uç noktaları* (`Endpoint`) kullanır.

Bir uç nokta, istekleri işlemek için bir temsilci ve rastgele meta veri koleksiyonunu tanımlar. Meta veriler, her bir uç noktaya eklenen ilkelere ve yapılandırmaya bağlı olarak çapraz kesme sorunları uygulamak için kullanılır.

Yönlendirme sistemi aşağıdaki özelliklere sahiptir:

* Yol şablonu sözdizimi, simgeleştirilmiş yol parametrelerine sahip yolları tanımlamak için kullanılır.
* Geleneksel stil ve öznitelik stili uç nokta yapılandırmasına izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, bir URL parametresinin belirli bir uç nokta kısıtlaması için geçerli bir değer içerip içermediğini belirlemekte kullanılır.
* MVC/Razor Pages gibi uygulama modelleri, yönlendirme senaryolarının öngörülebilir bir uygulaması olan tüm uç noktalarını kaydeder.
* Yönlendirme gerçekleştirme, yönlendirme kararlarını, ara yazılım ardışık düzeninde istediğiniz yere getirir.
* Bir yönlendirme ara yazılımı, belirli bir istek URI 'SI için yönlendirme ara yazılımı uç noktası kararının sonucunu inceleyebilir.
* Uygulamadaki tüm uç noktaları, ara yazılım ardışık düzeninde herhangi bir yerde listelemek mümkündür.
* Bir uygulama, uç nokta bilgilerine göre URL 'Ler oluşturmak için (örneğin, yeniden yönlendirme veya bağlantılar için) yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.
* URL oluşturma, rastgele genişletilebilirliği destekleyen adreslere dayalıdır:

  * Bağlantı Oluşturucu API 'SI (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>), [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanılarak URL 'ler oluşturmak için her yerde çözülebilir.
  * Bağlantı Oluşturucu API 'sinin DI aracılığıyla kullanılamadığı durumlarda <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> URL 'Leri derlemek için yöntemler sunar.

> [!NOTE]
> ASP.NET Core 2,2 ' de uç nokta yönlendirme yayını ile, uç nokta bağlama MVC/Razor Pages eylemleri ve sayfaları ile sınırlıdır. Uç nokta bağlama yeteneklerinin genişletmeleri, gelecek sürümlerde planlanmaktadır.

Yönlendirme, <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı tarafından, [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *uç noktaya*gelen isteği gönderdiği işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Uç nokta yönlendirmesinde yönlendirme sistemi tüm gönderme kararlarından sorumludur. Ara yazılım, seçili uç noktaya göre ilkeler uyguladığı için, yönlendirme sisteminde güvenlik ilkelerinin dağıtımını etkileyebilecek veya uygulamanın uygulanmasını etkileyebilecek herhangi bir kararın olması önemlidir.

Uç nokta temsilcisi yürütüldüğünde, [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özellikleri, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlanır.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>, uygulamanın hangi yolun eşleştiğini temel alarak kararlar verebilmesi için durum verilerinin her bir rota ile ilişkilendirilmesini desteklemek amacıyla sağlanır. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>LinkGenerator ile URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, uç noktalarınız ve bunlara erişen URL 'Ler arasında mantıksal bir ayrım sağlar.

Uç nokta yönlendirme bağlantı Oluşturucu API 'sini (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) içerir. <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, [dı](xref:fundamentals/dependency-injection)'dan alınabilecek bir tek hizmettir. API, yürütülen bir istek bağlamı dışında kullanılabilir. MVC 'nin [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>kullanan <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ve senaryoları, bağlantı oluşturma yetenekleri sağlamak için bağlantı oluşturucuyu kullanır.

Bağlantı Oluşturucu, bir *Adres* ve *Adres şemaları*kavramıyla desteklenir. Adres şeması, bağlantı oluşturma için göz önünde bulundurmanız gereken uç noktaları belirlemenin bir yoludur. Örneğin, çok sayıda kullanıcının yol adı ve yol değerleri senaryoları, MVC/Razor Pages tarafından tanıdık bir adres düzeni olarak uygulanır.

Bağlantı Oluşturucu, aşağıdaki genişletme yöntemleri aracılığıyla MVC/Razor Pages eylemlerine ve sayfalarına bağlanabilir:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Bu yöntemlerin aşırı yüklemesi `HttpContext`içeren bağımsız değişkenleri kabul eder. Bu yöntemler `Url.Action` ve `Url.Page` işlevsel olarak eşdeğerdir ancak ek esneklik ve seçenekler sunar.

`GetPath*` yöntemleri, `Url.Action` ve `Url.Page`, mutlak bir yol içeren bir URI oluşturmak için en çok benzerdir. `GetUri*` Yöntemler her zaman bir düzen ve konak içeren mutlak bir URI oluşturur. Bir `HttpContext` kabul eden yöntemler, yürütülmekte olan istek bağlamında bir URI oluşturur. Ortam yolu değerleri, URL taban yolu, şeması ve yürütülen istekten ana bilgisayar, geçersiz kılınmadıkça kullanılır.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> bir adresle çağrılır. URI oluşturma iki adımda gerçekleşir:

1. Adres, adresle eşleşen bir uç nokta listesine bağlanır.
1. Her uç noktanın `RoutePattern`, sağlanan değerlerle eşleşen bir rota deseninin bulunana kadar değerlendirilir. Elde edilen çıktı, bağlantı oluşturucuya sağlanan diğer URI parçalarıyla birleştirilir ve döndürülür.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> tarafından sunulan yöntemler, herhangi bir tür adresin standart bağlantı oluşturma özelliklerini destekler. Bağlantı oluşturucuyu kullanmanın en kolay yolu, belirli bir adres türü için işlem gerçekleştiren genişletme yöntemlerine yöneliktir.

| Genişletme yöntemi   | Açıklama                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Belirtilen değerleri temel alarak mutlak bir yola sahip bir URI oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Belirtilen değerleri temel alarak mutlak bir URI oluşturur.             |

> [!WARNING]
> <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemlerinin çağrılmasının aşağıdaki etkilerine dikkat edin:
>
> * Gelen isteklerin `Host` üstbilgisini doğrulayan bir uygulama yapılandırmasında `GetUri*` uzantısı yöntemlerini dikkatle kullanın. Gelen isteklerin `Host` üstbilgisi doğrulandıktan sonra, güvenilir olmayan istek girişi, bir görünüm/sayfada URI 'Ler halinde istemciye geri gönderilebilir. Tüm üretim uygulamalarının, `Host` üst bilgisini bilinen geçerli değerlere karşı doğrulamak için kendi sunucusunu yapılandırmasını öneririz.
>
> * `Map` veya `MapWhen`birlikte ara yazılım ile birlikte <xref:Microsoft.AspNetCore.Routing.LinkGenerator> kullanın. `Map*`, yürütülen isteğin temel yolunu değiştirir ve bu da bağlantı oluşturma çıktısını etkiler. Tüm <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'Leri temel yol belirtmeye izin verir. `Map*`bağlantı oluşturmada etkilerini geri almak için her zaman boş bir temel yol belirtin.

## <a name="differences-from-earlier-versions-of-routing"></a>Yönlendirmenin önceki sürümlerinden farklılıklar

ASP.NET Core 2,2 veya üzeri ve daha önceki yönlendirme sürümlerindeki ASP.NET Core uç nokta yönlendirmesi arasında birkaç fark vardır:

* Uç nokta yönlendirme sistemi, <xref:Microsoft.AspNetCore.Routing.Route>devralma dahil <xref:Microsoft.AspNetCore.Routing.IRouter>tabanlı genişletilebilirliği desteklemez.

* Uç nokta yönlendirme [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)desteklemez. Uyumluluk Shim 'yi kullanmaya devam etmek için 2,1 [Uyumluluk sürümünü](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) kullanın.

* Uç nokta yönlendirmesinde, geleneksel yollar kullanılırken oluşturulan URI 'lerin büyük küçük harfleri için farklı davranış vardır.

  Aşağıdaki varsayılan yol şablonunu göz önünde bulundurun:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Aşağıdaki rotayı kullanarak bir eyleme bağlantı oluşturduğunuzu varsayalım:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <xref:Microsoft.AspNetCore.Routing.IRouter>tabanlı yönlendirme ile bu kod, `/blog/ReadPost/17`URI 'sini oluşturur ve bu da, belirtilen yol değerinin büyük küçük harflerini üretir. ASP.NET Core 2,2 veya sonraki bir sürümde uç nokta yönlendirme `/Blog/ReadPost/17` ("blog" büyük harfli) oluşturur. Endpoint Routing, bu davranışı küresel olarak özelleştirmek veya URL eşleme için farklı kurallar uygulamak üzere kullanılabilecek `IOutboundParameterTransformer` arabirimini sağlar.

  Daha fazla bilgi için bkz. [Parameter transformatör başvurusu](#parameter-transformer-reference) bölümü.

* Standart yollarla MVC/Razor Pages tarafından kullanılan bağlantı oluşturma, mevcut olmayan bir denetleyiciye/eyleme veya sayfaya bağlantı kurmaya çalışırken farklı şekilde davranır.

  Aşağıdaki varsayılan yol şablonunu göz önünde bulundurun:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Aşağıdaki ile varsayılan şablonu kullanarak bir eyleme bağlantı oluşturduğunuzu varsayalım:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  `IRouter`tabanlı yönlendirme sayesinde, `BlogController` var olmasa veya bir `ReadPost` Action yöntemine sahip olmasa bile sonuç daima `/Blog/ReadPost/17`. Beklenen şekilde, işlem yöntemi varsa ASP.NET Core 2,2 veya sonraki bir sürümde uç nokta yönlendirmesi `/Blog/ReadPost/17` üretir. *Ancak, uç nokta yönlendirme, eylem yoksa boş bir dize oluşturur.* Kavramsal olarak, uç nokta yönlendirme, eylem mevcut değilse uç noktanın var olduğunu varsaymaz.

* Bağlantı oluşturma *çevresel değeri ınvalidation algoritması* , uç nokta yönlendirme ile kullanıldığında farklı davranır.

  *Çevresel değer ınvalidation* , şu anda yürütülmekte olan isteğin (çevresel değerler) hangi rota değerlerinin bağlantı oluşturma işlemlerinde kullanılabileceğini belirleyen algoritmadır. Geleneksel yönlendirme, farklı bir eyleme bağlanılırken ek yol değerlerini her zaman geçersiz kıldı. ASP.NET Core 2,2 sürümünden önce öznitelik yönlendirmesinde bu davranış yoktu. ASP.NET Core 'nin önceki sürümlerinde, aynı yol parametresi adlarını kullanan başka bir eyleme bağlantılar, bağlantı oluşturma hatalarıyla sonuçlandı. ASP.NET Core 2,2 veya sonraki sürümlerde, yönlendirme biçimlerinin her ikisi de başka bir eyleme bağlanırken değerleri geçersiz kılar.

  ASP.NET Core 2,1 veya önceki sürümlerde aşağıdaki örneği göz önünde bulundurun. Başka bir eyleme (veya başka bir sayfaya) bağlanırken, yol değerleri istenmeyen yollarla yeniden kullanılabilir.

  */Pages/Store/Product.exe*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  */Pages/Login.exe*içinde:

  ```cshtml
  @page "{id?}"
  ```

  URI, ASP.NET Core 2,1 veya önceki sürümlerde `/Store/Product/18`, `@Url.Page("/Login")` tarafından Store/Info sayfasında oluşturulan bağlantı `/Login/18`. 18 ' in `id` değeri, bağlantı hedefi uygulamanın tamamen farklı bir parçası olsa bile yeniden kullanılır. `/Login` sayfasının bağlamındaki `id` yol değeri büyük olasılıkla bir depolama ürün KIMLIĞI değeri değil, bir kullanıcı KIMLIĞI değeridir.

  ASP.NET Core 2,2 veya sonraki bir sürümü ile Endpoint Routing, sonuç `/Login`. Bağlantılı hedef farklı bir eylem veya sayfa olduğunda çevresel değerler yeniden kullanılmaz.

* Gidiş dönüşü yol parametresi sözdizimi: çift yıldız (`**`) catch-all parametre sözdizimi kullanılırken eğik çizgiler kodlanmaz.

  Bağlantı oluşturma sırasında, yönlendirme sistemi, eğik çizgiler dışında bir çift yıldız (`**`) catch-all parametresinde (örneğin, `{**myparametername}`) yakalanan değeri kodlar. Çift yıldız yakalama, ASP.NET Core 2,2 veya sonraki sürümlerde `IRouter`tabanlı yönlendirme ile desteklenir.

  ASP.NET Core (`{*myparametername}`) önceki sürümlerindeki tek yıldız catch-all parametre sözdizimi desteklenmeye devam eder ve eğik çizgi kodlandı.

  | Yol              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (eğik çizgi kodlanmış)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ara yazılım örneği

Aşağıdaki örnekte, bir ara yazılım, mağaza ürünlerini listeleyen bir eylem yöntemine bağlantı oluşturmak için <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'sini kullanır. Ekleme tarafından bir sınıfa olan ve `GenerateLink` çağıran bağlantı Oluşturucuyu kullanmak, bir uygulamadaki herhangi bir sınıfta kullanılabilir.

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

### <a name="create-routes"></a>Yolları oluşturma

Çoğu uygulama, <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>tanımlanan benzer uzantı yöntemlerinden birini veya <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> çağırarak yollar oluşturur. <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> uzantısı yöntemlerinden herhangi biri bir <xref:Microsoft.AspNetCore.Routing.Route> örneği oluşturur ve bunu yol koleksiyonuna ekler.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> yol işleyicisi parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>, yalnızca <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>tarafından işlenen rotaları ekler. MVC 'de yönlendirme hakkında daha fazla bilgi için bkz. <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, tipik bir ASP.NET Core MVC yol tanımı tarafından kullanılan <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> çağrısının bir örneğidir:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yoluyla eşleşir ve yol değerlerini ayıklar. Örneğin, yol `/Products/Details/17` şu yol değerlerini üretir: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri, URL yolunun kesimlere bölünüyor ve rota şablonundaki *yol parametresi* adı ile her bir segmenti eşleştirerek belirlenir. Yol parametreleri olarak adlandırılır. Küme ayraçları içinde parametre adı eklenerek tanımlanan parametreler `{ ... }`.

Yukarıdaki şablon, URL yolu `/` de eşleştirebilir ve `{ controller = Home, action = Index }`değerler üretecektir. Bu, `{controller}` ve `{action}` yol parametrelerinin varsayılan değerleri olduğu ve `id` Route parametresinin isteğe bağlı olduğu için oluşur. Eşittir işareti (`=`), yol parametresi adı parametre için varsayılan bir değer tanımladıktan sonra bir değer izler. Yol parametre adından sonra bir soru işareti (`?`) isteğe bağlı bir parametre tanımlar.

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. İsteğe bağlı parametreler, karşılık gelen bir URL yol kesimi yoksa rota değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

Aşağıdaki örnekte, yol parametresi tanımı `{id:int}` `id` Route parametresi için bir [yol kısıtlaması](#route-constraint-reference) tanımlıyor:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon `/Products/Details/17` gibi bir URL yoluyla eşleşir ancak `/Products/Details/Apples`. Yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> uygular ve yol değerlerini inceleyerek onları doğrular. Bu örnekte, yol değeri `id` bir tamsayıya dönüştürülebilir olmalıdır. Framework tarafından sunulan yol kısıtlamalarının açıklaması için bkz. [route-Constraint-Reference](#route-constraint-reference) .

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> ek aşırı yüklemeleri `constraints`, `dataTokens`ve `defaults`için değer kabul eder. Bu parametrelerin tipik kullanımı, anonim türdeki özellik adlarının yol parametre adlarıyla eşleşen anonim olarak yazılmış bir nesneyi geçirmektir.

Aşağıdaki <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> örnekleri eşdeğer yollar oluşturur:

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
> Kısıtlamaları ve Varsayılanları tanımlamaya yönelik satır içi sözdizimi basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi senaryolar vardır.

Aşağıdaki örnek birkaç ek senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablon `/Blog/All-About-Routing/Introduction` gibi bir URL yoluyla eşleşir ve `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`değerleri ayıklar. `controller` ve `action` için varsayılan yol değerleri, şablonda karşılık gelen hiçbir yol parametresi olmasa bile yol tarafından üretilir. Varsayılan değerler yol şablonunda belirtilebilir. `article` Route parametresi, yol parametre adından önce bir çift yıldız işareti (`**`) görünümüne göre *catch-all* olarak tanımlanır. Catch-all yol parametreleri, URL yolunun kalanını yakalar ve boş dizeyle de aynı olabilir.

Aşağıdaki örnek yol kısıtlamalarını ve veri belirteçlerini ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablon `/en-US/Products/5` gibi bir URL yoluyla eşleşir ve `{ controller = Products, action = Details, id = 5 }` ve veri belirteçleri `{ locale = en-US }`değerleri ayıklar.

![Yereller Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Yol sınıfı URL 'SI oluşturma

<xref:Microsoft.AspNetCore.Routing.Route> sınıfı, yol değerlerini bir kümesini rota şablonuyla birleştirerek URL oluşturmayı da gerçekleştirebilir. Bu, URL yolunu eşleştirmenin mantıksal bir işlemdir.

> [!TIP]
> URL oluşturmayı daha iyi anlamak için, oluşturmak istediğiniz URL 'YI düşünün ve sonra bir yönlendirme şablonunun bu URL ile nasıl eşleşeceğini düşünün. Hangi değerler üretilemidir? Bu, URL oluşturma 'nın <xref:Microsoft.AspNetCore.Routing.Route> sınıfında nasıl çalıştığına ilişkin kaba bir eştir.

Aşağıdaki örnek genel ASP.NET Core MVC varsayılan yolunu kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Yol değerleri `{ controller = Products, action = List }`, `/Products/List` URL 'SI oluşturulur. Yol değerleri, URL yolunu biçimlendirmek için karşılık gelen yol parametrelerinin yerine kullanılır. `id`, isteğe bağlı bir yol parametresi olduğundan, `id`için bir değer olmadan URL başarıyla oluşturulur.

Yol değerleri `{ controller = Home, action = Index }`, `/` URL 'SI oluşturulur. Belirtilen yol değerleri varsayılan değerlerle eşleşir ve varsayılan değerlere karşılık gelen segmentler güvenle atlanır.

Her iki URL de aşağıdaki yol tanımıyla (`/Home/Index` ve `/`) bir gidiş dönüş, URL oluşturmak için kullanılan rota değerlerini oluşturur.

> [!NOTE]
> ASP.NET Core MVC kullanan bir uygulama doğrudan yönlendirmeye çağırmak yerine URL oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> kullanmalıdır.

URL oluşturma hakkında daha fazla bilgi için [URL oluşturma başvurusu](#url-generation-reference) bölümüne bakın.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılımı kullanma

Uygulamanın proje dosyasındaki [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) öğesine başvurun.

`Startup.ConfigureServices`' de hizmet kapsayıcısına yönlendirme ekleyin:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; yalnızca HTTP GET istekleriyle eşleşir.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Aşağıdaki tabloda verilen URI 'Ler ile ilgili yanıtlar gösterilmektedir.

| URI                    | Yanıt                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Herkese! Rota değerleri: [işlem, oluşturma], [kimlik, 3] |
| `/package/track/-3`    | Herkese! Rota değerleri: [işlem, izleme], [kimlik,-3] |
| `/package/track/-3/`   | Herkese! Rota değerleri: [işlem, izleme], [kimlik,-3] |
| `/package/track/`      | İstek, eşleşme olmadan üzerinden yapılır.              |
| `GET /hello/Joe`       | Merhaba, ali!                                          |
| `POST /hello/Joe`      | İstek üzerinden geçer, yalnızca HTTP GET ile eşleşir. |
| `GET /hello/Joe/Smith` | İstek, eşleşme olmadan üzerinden yapılır.              |

Çerçeve, yollar oluşturmak için bir genişletme yöntemleri kümesi sağlar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

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

`Map[Verb]` yöntemleri, yöntem adındaki HTTP fiili ile rotayı sınırlandırmak için kısıtlamaları kullanır. Örneğin, bkz. <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları içindeki belirteçler (`{ ... }`), yol eşleştirildiği takdirde bağlanan *Rota parametrelerini* tanımlar. Bir yol segmentinde birden fazla yol parametresi tanımlayabilirsiniz, ancak bunların bir değişmez değer ile ayrılması gerekir. Örneğin, `{controller}` ve `{action}`arasında değişmez değer olmadığından `{controller=Home}{action=Index}` geçerli bir yol değil. Bu rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametreleri dışında (örneğin, `{id}`) ve yol ayırıcı `/` URL içindeki metinle eşleşmesi gerekir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Değişmez değer yol parametresi sınırlayıcısından (`{` veya `}`) eşleştirmek için, karakteri (`{{` veya `}}`) tekrarlayarak sınırlayıcıdan kaçış.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, `files/{filename}.{ext?}`şablonu düşünün. `filename` ve `ext` değerlerinin her ikisi de varsa, her iki değer de doldurulur. URL 'de yalnızca `filename` için bir değer varsa, bitiş dönemi (`.`) isteğe bağlı olduğundan yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

URI 'nin geri kalanına bağlamak için bir yol parametresinin öneki olarak bir yıldız işareti (`*`) veya çift yıldız işareti (`**`) kullanabilirsiniz. Bunlara *catch-all* parametreleri denir. Örneğin, `blog/{**slug}` `/blog` ile başlayan ve ondan sonraki bir değere sahip olan `slug` yol değerine atanan tüm URI ile eşleşir. Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı (`/`) karakterleri de dahil olmak üzere bir URL oluşturmak için bir yol kullanıldığında, catch-all parametresi uygun karakterleri çıkar. Örneğin yol `foo/{*path}` rota değerleriyle `{ path = "my/path" }` `foo/my%2Fpath`üretir. Atlanan eğik çizgiye göz önünde edin. Yol ayırıcı karakterlerini geri döndürmek için `**` Route parametresi önekini kullanın. `{ path = "my/path" }` ile yol `foo/{**path}` `foo/my/path`üretir.

Yol parametrelerinin, parametre adından sonra varsayılan değer belirtilerek bir eşittir işareti (`=`) ile ayrıldıktan sonra belirlenen *varsayılan değerleri* olabilir. Örneğin, `Home` `controller`için varsayılan değer olarak `{controller=Home}` tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, `id?`gibi parametre adının sonuna bir soru işareti (`?`) eklenerek isteğe bağlı olarak yapılır. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark, varsayılan değere sahip bir yol parametresinin her zaman bir değer ürettiğinden&mdash;isteğe bağlı bir parametre yalnızca istek URL 'SI tarafından bir değer sağlandığında bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra bir iki nokta üst üste (`:`) ve kısıtlama adının eklenmesi, bir yol parametresi üzerinde bir *satır içi kısıtlamayı* belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez (`(...)`) içine alınır. Birden çok satır içi kısıtlama, başka bir iki nokta (`:`) ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılacak bir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> örneği oluşturmak için <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}`, `10`bağımsız değişkeniyle bir `minlength` kısıtlaması belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Yol parametrelerinde Ayrıca, bağlantı oluştururken ve URL 'Ler ile eşleşen eylemler ve sayfalar için bir parametrenin değerini dönüştüren parametre dönüştürücüler bulunabilir. LIKE kısıtlamaları, yol parametre adından sonra iki nokta üst üste (`:`) ve transformatör adı eklenerek, parametre dönüştürücüleri bir rota parametresine eklenebilir. Örneğin, yol şablonu `blog/{article:slugify}` bir `slugify` transformatörü belirtir. Parametre dönüştürücüler hakkında daha fazla bilgi için bkz. [Parameter transformatör başvurusu](#parameter-transformer-reference) bölümü.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir.

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca `/hello`tek yol ile eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | `Home``Page` eşleşir ve ayarlar.                                         |
| `{Page=Home}`                            | `/Contact`              | `Contact``Page` eşleşir ve ayarlar.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` denetleyicisi ve `List` eylemiyle eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | `Products` denetleyicisi ve `Details` eylemine eşlenir (`id` 123 olarak ayarlanır). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | `Home` denetleyicisi ve `Index` yöntemiyle eşlenir (`id` yok sayılır).        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

> [!TIP]
> <xref:Microsoft.AspNetCore.Routing.Route>, istekleri eşleştirme gibi yerleşik yönlendirme uygulamalarının nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

## <a name="reserved-routing-names"></a>Ayrılmış yönlendirme adları

Aşağıdaki anahtar sözcükler ayrılmış isimlerdir ve yol adları veya parametreler olarak kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Rota kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda bir Evet/Hayır kararı getirir. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint>, HTTP fiiline bağlı olarak bir isteği kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> **Giriş doğrulaması**için kısıtlamaları kullanmayın. **Giriş doğrulaması**için kısıtlamalar kullanılıyorsa, doğru bir hata iletisine sahip *400-Bad isteği* yerine *404-* olmayan bir Yanıt ile geçersiz giriş oluşur. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların **belirsizliğini ortadan** kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve bunların beklenen davranışları gösterilmektedir.

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir. |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya ' false ile eşleşir. Büyük/küçük harf duyarsız. |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Sabit kültürün geçerli bir `DateTime` değeriyle eşleşir. Önceki uyarıya bakın.|
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Sabit kültürün geçerli bir `decimal` değeriyle eşleşir. Önceki uyarıya bakın.|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `double` değeriyle eşleşir. Önceki uyarıya bakın.|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `float` değeriyle eşleşir. Önceki uyarıya bakın.|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli bir `Guid` değeriyle eşleşir. |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir `long` değeriyle eşleşir. |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır. |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | Dizede en fazla 8 karakter vardır. |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 olmalı ve en fazla 16 karakter uzunluğunda olmalıdır. |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır. |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri üst sınırı 120. |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 ve en fazla 120 olmalıdır. |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır `a`-`z`.  Büyük/küçük harf duyarsız. |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir. Normal ifade tanımlama hakkında ipuçlarına bakın. |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır. |

Birden çok, iki nokta üst üste sınırlı kısıtlama tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'YI doğrulayan ve bir CLR türüne (örneğin `int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. Yönlendirme içinde `^\d{3}-\d{2}-\d{4}$` normal ifade kullanmak için:

* İfade, dizede, kaynak kodundaki karakterlerin `\\` çift ters eğik çizgi olarak belirtilen tek ters eğik çizgi `\` olmalıdır.
* Normal ifade, `\` dize kaçış karakterini atlamak için `\\` olmalıdır.
* Normal ifade, tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanılırken `\\` gerektirmez.

Yönlendirme parametresi sınırlayıcı karakterlerinin `{`, `}`, `[`, `]`ve ifadedeki karakterleri `{{`, `}`, `[[`, `]]`olarak tırnak içine almak için. Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir:

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka `^` karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler, genellikle dolar işareti `$` karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | herkese     | Hayır    | Yukarıdaki `^` ve `$` bakın |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Yukarıdaki `^` ve `$` bakın |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. Örneğin `{action:regex(^(list|get|create)$)}`, yalnızca `action` Route değeri `list`, `get`veya `create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğüne (bir şablon içinde satır içi değil) geçirilen kısıtlamalar da normal ifadeler olarak kabul edilir.

## <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Yerleşik yol kısıtlamalarına ek olarak, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak özel yol kısıtlamaları oluşturulabilir. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi, kısıtlama karşılandıysanız `true` döndüren `Match`tek bir yöntem içerir ve aksi takdirde `false`.

Özel bir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>kullanmak için, yol kısıtlama türü uygulamanın hizmet kapsayıcısında uygulamanın <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kayıtlı olmalıdır. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>, yol kısıtlama anahtarlarını bu kısıtlamaları doğrulayan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> uygulamalarla eşleyen bir sözlüktür. Bir uygulamanın <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>, bir hizmetin parçası olarak `Startup.ConfigureServices` güncelleştirilebilen olabilir [. AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya <xref:Microsoft.AspNetCore.Routing.RouteOptions> doğrudan `services.Configure<RouteOptions>`ile yapılandırma. Örneğin:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Kısıtlama daha sonra, kısıtlama türü kaydedilirken belirtilen ad kullanılarak yollara her zamanki şekilde uygulanabilir. Örneğin:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Parametre transformatörü başvurusu

Parametre dönüştürücüler:

* <xref:Microsoft.AspNetCore.Routing.Route>için bağlantı oluştururken yürütün.
* `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`uygulayın.
* <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>kullanılarak yapılandırılır.
* Parametrenin yol değerini alın ve yeni bir dize değerine dönüştürün.
* Oluşturulan bağlantıda Dönüştürülmüş değerin kullanılmasına neden olacak.

Örneğin, `Url.Action(new { article = "MyTestArticle" })` ile `blog\{article:slugify}` yol düzeninde özel `slugify` parametresi transformatörü `blog\my-test-article`üretir.

Bir parametre transformatörü bir yol düzeninde kullanmak için, önce `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kullanarak yapılandırın:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Parametre dönüştürücüler, bir uç noktanın çözümlediği URI 'yi dönüştürmek için çerçevesi tarafından kullanılır. Örneğin, ASP.NET Core MVC, `area`, `controller`, `action`ve `page`eşleşecek şekilde kullanılan rota değerini dönüştürmek için parametre dönüştürücüler kullanır.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Önceki yol ile eylem `SubscriptionManagementController.GetAll`, URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıkışları `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla bir parametre dönüştürücüler kullanmak için API kuralları sağlar:

* ASP.NET Core MVC 'nin `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı vardır. Bu kural, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralına sahiptir. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin bir sözlüğü ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>için bir yol bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Yukarıdaki örnek sonunda oluşturulan <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> `/package/create/123`. Sözlük, "paket yolunu Izle" şablonunun `package/{operation}/{id}``operation` ve `id` yol değerlerini sağlar. Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> oluşturucusunun ikinci parametresi bir *ortam değerleri*koleksiyonudur. Ortam değerleri, bir geliştiricinin bir istek bağlamı içinde belirtmesi gereken değer sayısını sınırlandırdığından kullanım için uygundur. Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir. ASP.NET Core MVC uygulamasının `HomeController``About` eyleminde,&mdash;ortam değeri `Home` `Index` eyleme bağlamak için denetleyici yolu değerini belirtmeniz gerekmez.

Bir parametreyle eşleşmeyen çevresel değerler yok sayılır. Ayrıca, açıkça sağlanmış bir değer çevresel değeri geçersiz kıldığında çevresel değerler de yoksayılır. Eşleştirme, URL 'de soldan sağa doğru gerçekleşir.

Açık olarak sağlanmış ancak yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda `{controller}/{action}/{id?}`yol şablonu kullanılırken sonuç gösterilmektedir.

| Çevresel değerler                     | Açık değerler                        | Sonuç                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| denetleyici = "giriş"                | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | denetleyici = "Order", Action = "hakkında" | `/Order/About`          |
| denetleyici = "giriş", renk = "kırmızı" | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | Action = "hakkında", color = "Red"        | `/Home/About?color=Red` |

Bir rotada bir parametreye karşılık gelen bir varsayılan değer varsa ve bu değer açıkça sağlanmışsa, varsayılan değerle eşleşmelidir:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Bağlantı oluşturma yalnızca `controller` ve `action` için eşleşen değerler sağlandığında bu yol için bir bağlantı oluşturur.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin `[Route("/x{token}y")]`), sabit değerli olmayan değişmez değerler ile sağdan sola eşleştirilirken işlenir. Karmaşık segmentlerin nasıl eşleştirileceği hakkında ayrıntılı bir açıklama için [bu koda](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) bakın. [Kod örneği](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler hakkında iyi bir açıklama sağlar.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yönlendirme, istek URI 'Lerini rota işleyicileriyle eşleştirmekten ve gelen istekleri gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yapılandırılan yollar kullanıldığında, yönlendirme, yol işleyicileriyle eşlenen URL 'Ler oluşturabilir.

ASP.NET Core 2,1 ' de en son yönlendirme senaryolarını kullanmak için `Startup.ConfigureServices`MVC Hizmetleri kaydının [Uyumluluk sürümünü](xref:mvc/compatibility-version) belirtin:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında daha fazla bilgi için bkz. <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz. <xref:razor-pages/razor-pages-conventions>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, <xref:Microsoft.AspNetCore.Routing.IRouter> yollar uygulamalarını kullanır:

* Gelen istekleri *Rota işleyicilerine*eşleyin.
* Yanıtlarda kullanılan URL 'Leri oluşturun.

Varsayılan olarak, bir uygulama tek bir yollar koleksiyonuna sahiptir. Bir istek geldiğinde koleksiyondaki yollar, koleksiyonda var olan sırada işlenir. Çerçeve, koleksiyondaki her bir rotada <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemini çağırarak, gelen istek URL 'sini koleksiyondaki bir yola eşleştirmeye çalışır. Yanıt, yönlendirme bilgilerine göre URL (örneğin, yeniden yönlendirme veya bağlantılar için) oluşturmak için yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.

Yönlendirme sistemi aşağıdaki özelliklere sahiptir:

* Yol şablonu sözdizimi, simgeleştirilmiş yol parametrelerine sahip yolları tanımlamak için kullanılır.
* Geleneksel stil ve öznitelik stili uç nokta yapılandırmasına izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, bir URL parametresinin belirli bir uç nokta kısıtlaması için geçerli bir değer içerip içermediğini belirlemekte kullanılır.
* MVC/Razor Pages gibi uygulama modelleri, yönlendirme senaryolarının öngörülebilir bir uygulaması olan tüm yollarını kaydeder.
* Yanıt, yönlendirme bilgilerine göre URL (örneğin, yeniden yönlendirme veya bağlantılar için) oluşturmak için yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.
* URL oluşturma, rastgele genişletilebilirliği destekleyen yollara dayalıdır. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, URL 'Leri derlemek için yöntemler sunar.
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
Yönlendirme, <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı tarafından `[middleware](xref:fundamentals/middleware/index)` işlem hattına bağlanır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *işleyiciye*gelen istek gönderdiğine yönelik işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Gelen istekler, sırayla her bir rotada <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemini çağıran <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>girer. <xref:Microsoft.AspNetCore.Routing.IRouter> örneği, [Routecontext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) değerini null olmayan bir <xref:Microsoft.AspNetCore.Http.RequestDelegate>ayarlayarak isteğin *işleneceğini* belirler. Bir yol istek için bir işleyici ayarlarsa, yol işleme duraklar ve işleyici isteği işlemek için çağrılır. İsteği işlemek için yol işleyicisi bulunmazsa, ara yazılım istek ardışık düzeninde sonraki bir ara yazılım için isteği kapatır.

<xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> birincil giriş, geçerli istekle ilişkili [Routecontext. HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) ' dir. [Routecontext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) ve [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) , bir yol eşleştirdikten sonra çıkış olarak ayarlanır.

<xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> çağıran bir eşleşme de [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özelliklerinin özelliklerini, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlar.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>, uygulamanın hangi yolun eşleştiğini temel alarak kararlar verebilmesi için durum verilerinin her bir rota ile ilişkilendirilmesini desteklemek amacıyla sağlanır. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation"></a>URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, yönlendirme işleyicileri ve bunlara erişen URL 'Ler arasındaki mantıksal bir ayrım sağlar.

URL oluşturma benzer bir yinelemeli işlem izler, ancak yol koleksiyonunun <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> metoduna çağıran kullanıcı veya çerçeve kodu ile başlar. Her *rotada* null olmayan bir <xref:Microsoft.AspNetCore.Routing.VirtualPathData> döndürülene kadar <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> yöntemi sırayla çağırılır.

<xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> birincil girişleri şunlardır:

* [VirtualPathContext. HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext. Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext. AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Rotalar öncelikle <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> ve <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> tarafından sunulan yol değerlerini kullanarak bir URL oluşturma ve hangi değerlerin dahil edileceğini belirleyin. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues>, geçerli istekle eşleşmeden üretilmiş olan rota değerleri kümesidir. Buna karşılık <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values>, geçerli işlem için istenen URL 'nin nasıl oluşturulacağını belirten rota değerlerdir. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>, bir yolun geçerli bağlamla ilişkili hizmetleri veya ek verileri alması durumunda sağlanır.

> [!TIP]
> Virtualpathcontext [. Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) öğesini [Virtualpathcontext. AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)için bir geçersiz kılma kümesi olarak düşünün. URL oluşturma, aynı rota veya yol değerlerini kullanan bağlantılar için URL 'Ler oluşturmak üzere geçerli istekten yol değerlerini yeniden kullanmayı dener.

<xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> çıkışı bir <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData>, <xref:Microsoft.AspNetCore.Routing.RouteData>paraleldir. <xref:Microsoft.AspNetCore.Routing.VirtualPathData>, çıkış URL 'SI için <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> ve yol tarafından ayarlanması gereken bazı ek özellikleri içerir.

[VirtualPathData. VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) özelliği, yol tarafından üretilen *sanal yolu* içerir. Gereksinimlerinize bağlı olarak, yolu daha fazla işlem yapmanız gerekebilir. Oluşturulan URL 'YI HTML 'de işlemek istiyorsanız, uygulamanın temel yolunu ekleyin.

[VirtualPathData. Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) , URL 'yi başarıyla oluşturmuş olan yola bir başvurudur.

[VirtualPathData. DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) özellikleri, URL 'yi oluşturan rotayla ilgili ek verilerin bir sözlüğüdür. Bu, [RouteData. Datatoken](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*)'ların paraleldir.

### <a name="create-routes"></a>Yolları oluşturma

Yönlendirme, <xref:Microsoft.AspNetCore.Routing.IRouter>standart uygulama olarak <xref:Microsoft.AspNetCore.Routing.Route> sınıfını sağlar. <xref:Microsoft.AspNetCore.Routing.Route>, <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> çağrıldığında URL yoluyla eşleştirilecek desenleri tanımlamak için *yol şablonu* sözdizimini kullanır. <xref:Microsoft.AspNetCore.Routing.Route>, <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> çağrıldığında bir URL oluşturmak için aynı yol şablonunu kullanır.

Çoğu uygulama, <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>tanımlanan benzer uzantı yöntemlerinden birini veya <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> çağırarak yollar oluşturur. <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> uzantısı yöntemlerinden herhangi biri bir <xref:Microsoft.AspNetCore.Routing.Route> örneği oluşturur ve bunu yol koleksiyonuna ekler.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> yol işleyicisi parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>, yalnızca <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>tarafından işlenen rotaları ekler. Varsayılan işleyici bir `IRouter`ve işleyici isteği işleyemeyebilir. Örneğin, ASP.NET Core MVC genellikle yalnızca kullanılabilir bir denetleyici ve eylemle eşleşen istekleri işleyen varsayılan bir işleyici olarak yapılandırılır. MVC 'de yönlendirme hakkında daha fazla bilgi için bkz. <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, tipik bir ASP.NET Core MVC yol tanımı tarafından kullanılan <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> çağrısının bir örneğidir:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yoluyla eşleşir ve yol değerlerini ayıklar. Örneğin, yol `/Products/Details/17` şu yol değerlerini üretir: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri, URL yolunun kesimlere bölünüyor ve rota şablonundaki *yol parametresi* adı ile her bir segmenti eşleştirerek belirlenir. Yol parametreleri olarak adlandırılır. Küme ayraçları içinde parametre adı eklenerek tanımlanan parametreler `{ ... }`.

Yukarıdaki şablon, URL yolu `/` de eşleştirebilir ve `{ controller = Home, action = Index }`değerler üretecektir. Bu, `{controller}` ve `{action}` yol parametrelerinin varsayılan değerleri olduğu ve `id` Route parametresinin isteğe bağlı olduğu için oluşur. Eşittir işareti (`=`), yol parametresi adı parametre için varsayılan bir değer tanımladıktan sonra bir değer izler. Yol parametre adından sonra bir soru işareti (`?`) isteğe bağlı bir parametre tanımlar.

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. İsteğe bağlı parametreler, karşılık gelen bir URL yol kesimi yoksa rota değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

Aşağıdaki örnekte, yol parametresi tanımı `{id:int}` `id` Route parametresi için bir [yol kısıtlaması](#route-constraint-reference) tanımlıyor:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon `/Products/Details/17` gibi bir URL yoluyla eşleşir ancak `/Products/Details/Apples`. Yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> uygular ve yol değerlerini inceleyerek onları doğrular. Bu örnekte, yol değeri `id` bir tamsayıya dönüştürülebilir olmalıdır. Framework tarafından sunulan yol kısıtlamalarının açıklaması için bkz. [route-Constraint-Reference](#route-constraint-reference) .

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> ek aşırı yüklemeleri `constraints`, `dataTokens`ve `defaults`için değer kabul eder. Bu parametrelerin tipik kullanımı, anonim türdeki özellik adlarının yol parametre adlarıyla eşleşen anonim olarak yazılmış bir nesneyi geçirmektir.

Aşağıdaki <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> örnekleri eşdeğer yollar oluşturur:

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
> Kısıtlamaları ve Varsayılanları tanımlamaya yönelik satır içi sözdizimi basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi senaryolar vardır.

Aşağıdaki örnek birkaç ek senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablon `/Blog/All-About-Routing/Introduction` gibi bir URL yoluyla eşleşir ve `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`değerleri ayıklar. `controller` ve `action` için varsayılan yol değerleri, şablonda karşılık gelen hiçbir yol parametresi olmasa bile yol tarafından üretilir. Varsayılan değerler yol şablonunda belirtilebilir. `article` Route parametresi, yol parametre adından önce bir yıldız işareti (`*`) görünümüne göre *catch-all* olarak tanımlanır. Catch-all yol parametreleri, URL yolunun kalanını yakalar ve boş dizeyle de aynı olabilir.

Aşağıdaki örnek yol kısıtlamalarını ve veri belirteçlerini ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablon `/en-US/Products/5` gibi bir URL yoluyla eşleşir ve `{ controller = Products, action = Details, id = 5 }` ve veri belirteçleri `{ locale = en-US }`değerleri ayıklar.

![Yereller Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Yol sınıfı URL 'SI oluşturma

<xref:Microsoft.AspNetCore.Routing.Route> sınıfı, yol değerlerini bir kümesini rota şablonuyla birleştirerek URL oluşturmayı da gerçekleştirebilir. Bu, URL yolunu eşleştirmenin mantıksal bir işlemdir.

> [!TIP]
> URL oluşturmayı daha iyi anlamak için, oluşturmak istediğiniz URL 'YI düşünün ve sonra bir yönlendirme şablonunun bu URL ile nasıl eşleşeceğini düşünün. Hangi değerler üretilemidir? Bu, URL oluşturma 'nın <xref:Microsoft.AspNetCore.Routing.Route> sınıfında nasıl çalıştığına ilişkin kaba bir eştir.

Aşağıdaki örnek genel ASP.NET Core MVC varsayılan yolunu kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Yol değerleri `{ controller = Products, action = List }`, `/Products/List` URL 'SI oluşturulur. Yol değerleri, URL yolunu biçimlendirmek için karşılık gelen yol parametrelerinin yerine kullanılır. `id`, isteğe bağlı bir yol parametresi olduğundan, `id`için bir değer olmadan URL başarıyla oluşturulur.

Yol değerleri `{ controller = Home, action = Index }`, `/` URL 'SI oluşturulur. Belirtilen yol değerleri varsayılan değerlerle eşleşir ve varsayılan değerlere karşılık gelen segmentler güvenle atlanır.

Her iki URL de aşağıdaki yol tanımıyla (`/Home/Index` ve `/`) bir gidiş dönüş, URL oluşturmak için kullanılan rota değerlerini oluşturur.

> [!NOTE]
> ASP.NET Core MVC kullanan bir uygulama doğrudan yönlendirmeye çağırmak yerine URL oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> kullanmalıdır.

URL oluşturma hakkında daha fazla bilgi için [URL oluşturma başvurusu](#url-generation-reference) bölümüne bakın.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılımı kullanma

Uygulamanın proje dosyasındaki [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) öğesine başvurun.

`Startup.ConfigureServices`' de hizmet kapsayıcısına yönlendirme ekleyin:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; yalnızca HTTP GET istekleriyle eşleşir.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Aşağıdaki tabloda verilen URI 'Ler ile ilgili yanıtlar gösterilmektedir.

| URI                    | Yanıt                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Herkese! Rota değerleri: [işlem, oluşturma], [kimlik, 3] |
| `/package/track/-3`    | Herkese! Rota değerleri: [işlem, izleme], [kimlik,-3] |
| `/package/track/-3/`   | Herkese! Rota değerleri: [işlem, izleme], [kimlik,-3] |
| `/package/track/`      | İstek, eşleşme olmadan üzerinden yapılır.              |
| `GET /hello/Joe`       | Merhaba, ali!                                          |
| `POST /hello/Joe`      | İstek üzerinden geçer, yalnızca HTTP GET ile eşleşir. |
| `GET /hello/Joe/Smith` | İstek, eşleşme olmadan üzerinden yapılır.              |

Tek bir yol yapılandırıyorsanız, bir `IRouter` örneğinde geçen <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> çağırın. <xref:Microsoft.AspNetCore.Routing.RouteBuilder>kullanmanız gerekmez.

Çerçeve, yollar oluşturmak için bir genişletme yöntemleri kümesi sağlar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

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

<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>gibi listelenen yöntemlerin bazıları <xref:Microsoft.AspNetCore.Http.RequestDelegate>gerektirir. Yol eşleştiğinde *yol işleyicisi* olarak <xref:Microsoft.AspNetCore.Http.RequestDelegate> kullanılır. Bu ailedeki diğer yöntemler, yönlendirme işleyicisi olarak kullanılmak üzere bir ara yazılım ardışık düzeni yapılandırmaya olanak tanır. `Map*` yöntemi <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>gibi bir işleyiciyi kabul etmez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>kullanır.

`Map[Verb]` yöntemleri, yöntem adındaki HTTP fiili ile rotayı sınırlandırmak için kısıtlamaları kullanır. Örneğin, bkz. <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları içindeki belirteçler (`{ ... }`), yol eşleştirildiği takdirde bağlanan *Rota parametrelerini* tanımlar. Bir yol segmentinde birden fazla yol parametresi tanımlayabilirsiniz, ancak bunların bir değişmez değer ile ayrılması gerekir. Örneğin, `{controller}` ve `{action}`arasında değişmez değer olmadığından `{controller=Home}{action=Index}` geçerli bir yol değil. Bu rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametreleri dışında (örneğin, `{id}`) ve yol ayırıcı `/` URL içindeki metinle eşleşmesi gerekir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Değişmez değer yol parametresi sınırlayıcısından (`{` veya `}`) eşleştirmek için, karakteri (`{{` veya `}}`) tekrarlayarak sınırlayıcıdan kaçış.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, `files/{filename}.{ext?}`şablonu düşünün. `filename` ve `ext` değerlerinin her ikisi de varsa, her iki değer de doldurulur. URL 'de yalnızca `filename` için bir değer varsa, bitiş dönemi (`.`) isteğe bağlı olduğundan yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

Yıldız işaretini (`*`) URI 'nin geri kalanına bağlamak için bir yol parametresinin öneki olarak kullanabilirsiniz. Buna *catch-all* parametresi denir. Örneğin, `blog/{*slug}` `/blog` ile başlayan ve ondan sonraki bir değere sahip olan `slug` yol değerine atanan tüm URI ile eşleşir. Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı (`/`) karakterleri de dahil olmak üzere bir URL oluşturmak için bir yol kullanıldığında, catch-all parametresi uygun karakterleri çıkar. Örneğin yol `foo/{*path}` rota değerleriyle `{ path = "my/path" }` `foo/my%2Fpath`üretir. Atlanan eğik çizgiye göz önünde edin.

Yol parametrelerinin, parametre adından sonra varsayılan değer belirtilerek bir eşittir işareti (`=`) ile ayrıldıktan sonra belirlenen *varsayılan değerleri* olabilir. Örneğin, `Home` `controller`için varsayılan değer olarak `{controller=Home}` tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, `id?`gibi parametre adının sonuna bir soru işareti (`?`) eklenerek isteğe bağlı olarak yapılır. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark, varsayılan değere sahip bir yol parametresinin her zaman bir değer ürettiğinden&mdash;isteğe bağlı bir parametre yalnızca istek URL 'SI tarafından bir değer sağlandığında bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra bir iki nokta üst üste (`:`) ve kısıtlama adının eklenmesi, bir yol parametresi üzerinde bir *satır içi kısıtlamayı* belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez (`(...)`) içine alınır. Birden çok satır içi kısıtlama, başka bir iki nokta (`:`) ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılacak bir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> örneği oluşturmak için <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}`, `10`bağımsız değişkeniyle bir `minlength` kısıtlaması belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir.

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca `/hello`tek yol ile eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | `Home``Page` eşleşir ve ayarlar.                                         |
| `{Page=Home}`                            | `/Contact`              | `Contact``Page` eşleşir ve ayarlar.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` denetleyicisi ve `List` eylemiyle eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | `Products` denetleyicisi ve `Details` eylemine eşlenir (`id` 123 olarak ayarlanır). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | `Home` denetleyicisi ve `Index` yöntemiyle eşlenir (`id` yok sayılır).        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

> [!TIP]
> <xref:Microsoft.AspNetCore.Routing.Route>, istekleri eşleştirme gibi yerleşik yönlendirme uygulamalarının nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Rota kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda bir Evet/Hayır kararı getirir. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint>, HTTP fiiline bağlı olarak bir isteği kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> **Giriş doğrulaması**için kısıtlamaları kullanmayın. **Giriş doğrulaması**için kısıtlamalar kullanılıyorsa, doğru bir hata iletisine sahip *400-Bad isteği* yerine *404-* olmayan bir Yanıt ile geçersiz giriş oluşur. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların **belirsizliğini ortadan** kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve bunların beklenen davranışları gösterilmektedir.

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya `false` eşleşir (büyük/küçük harfe duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Sabit kültürün geçerli bir `DateTime` değeriyle eşleşir. Önceki uyarıya bakın.|
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Sabit kültürün geçerli bir `decimal` değeriyle eşleşir. Önceki uyarıya bakın.|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `double` değeriyle eşleşir. Önceki uyarıya bakın.|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Sabit kültürün geçerli bir `float` değeriyle eşleşir. Önceki uyarıya bakın.|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli bir `Guid` değeriyle eşleşir |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir `long` değeriyle eşleşir |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize 8 karakterden uzun olmamalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 ve en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri 120 ' ten fazla olmamalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 olmalı ancak 120 ' ten fazla olmamalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır (`a`-`z`, büyük/küçük harfe duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir (normal ifade tanımlama hakkında ipuçlarına bakın) |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır |

Birden çok, iki nokta üst üste sınırlı kısıtlama tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'YI doğrulayan ve bir CLR türüne (örneğin `int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. `^\d{3}-\d{2}-\d{4}$` normal ifade ' i yönlendirmede kullanmak için, ifadede, C# kaynak `\` dosyadaki `\\` (çift ters eğik çizgi) karakter olarak dize içinde belirtilen `\` (tek ters eğik çizgi) karakterleri olmalıdır (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `[`, `]`) atlamak için, ifadedeki karakterleri çift (`{{`, `}`, `[[`, `]]`). Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | herkese     | Hayır    | Yukarıdaki `^` ve `$` bakın |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Yukarıdaki `^` ve `$` bakın |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. Örneğin `{action:regex(^(list|get|create)$)}`, yalnızca `action` Route değeri `list`, `get`veya `create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğüne (bir şablon içinde satır içi değil) geçirilen kısıtlamalar da normal ifadeler olarak kabul edilir.

## <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Yerleşik yol kısıtlamalarına ek olarak, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak özel yol kısıtlamaları oluşturulabilir. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi, kısıtlama karşılandıysanız `true` döndüren `Match`tek bir yöntem içerir ve aksi takdirde `false`.

Özel bir <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>kullanmak için, yol kısıtlama türü uygulamanın hizmet kapsayıcısında uygulamanın <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> kayıtlı olmalıdır. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>, yol kısıtlama anahtarlarını bu kısıtlamaları doğrulayan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> uygulamalarla eşleyen bir sözlüktür. Bir uygulamanın <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>, bir hizmetin parçası olarak `Startup.ConfigureServices` güncelleştirilebilen olabilir [. AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya <xref:Microsoft.AspNetCore.Routing.RouteOptions> doğrudan `services.Configure<RouteOptions>`ile yapılandırma. Örneğin:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Kısıtlama daha sonra, kısıtlama türü kaydedilirken belirtilen ad kullanılarak yollara her zamanki şekilde uygulanabilir. Örneğin:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin bir sözlüğü ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>için bir yol bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Yukarıdaki örnek sonunda oluşturulan <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> `/package/create/123`. Sözlük, "paket yolunu Izle" şablonunun `package/{operation}/{id}``operation` ve `id` yol değerlerini sağlar. Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> oluşturucusunun ikinci parametresi bir *ortam değerleri*koleksiyonudur. Ortam değerleri, bir geliştiricinin bir istek bağlamı içinde belirtmesi gereken değer sayısını sınırlandırdığından kullanım için uygundur. Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir. ASP.NET Core MVC uygulamasının `HomeController``About` eyleminde,&mdash;ortam değeri `Home` `Index` eyleme bağlamak için denetleyici yolu değerini belirtmeniz gerekmez.

Bir parametreyle eşleşmeyen çevresel değerler yok sayılır. Ayrıca, açıkça sağlanmış bir değer çevresel değeri geçersiz kıldığında çevresel değerler de yoksayılır. Eşleştirme, URL 'de soldan sağa doğru gerçekleşir.

Açık olarak sağlanmış ancak yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda `{controller}/{action}/{id?}`yol şablonu kullanılırken sonuç gösterilmektedir.

| Çevresel değerler                     | Açık değerler                        | Sonuç                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| denetleyici = "giriş"                | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | denetleyici = "Order", Action = "hakkında" | `/Order/About`          |
| denetleyici = "giriş", renk = "kırmızı" | Action = "hakkında"                       | `/Home/About`           |
| denetleyici = "giriş"                | Action = "hakkında", color = "Red"        | `/Home/About?color=Red` |

Bir rotada bir parametreye karşılık gelen bir varsayılan değer varsa ve bu değer açıkça sağlanmışsa, varsayılan değerle eşleşmelidir:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Bağlantı oluşturma yalnızca `controller` ve `action` için eşleşen değerler sağlandığında bu yol için bir bağlantı oluşturur.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin `[Route("/x{token}y")]`), sabit değerli olmayan değişmez değerler ile sağdan sola eşleştirilirken işlenir. Karmaşık segmentlerin nasıl eşleştirileceği hakkında ayrıntılı bir açıklama için [bu koda](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) bakın. [Kod örneği](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler hakkında iyi bir açıklama sağlar.


::: moniker-end
