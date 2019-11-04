---
title: ASP.NET Core yönlendirme
author: rick-anderson
description: İstek URI 'Lerini uç nokta seçiclerine eşleyerek ve gelen istekleri uç noktalara gönderen ASP.NET Core yönlendirmenin nasıl sorumlu olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: fundamentals/routing
ms.openlocfilehash: be4493cc927bd5437a2c9dab00b6a555756195bb
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416132"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

[Ryan şimdi ak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Yönlendirme, istek URI 'Lerini uç noktalarla eşleştirmekten ve gelen istekleri bu uç noktalara gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yönlendirme bilgilerini kullanarak, yönlendirme, uç noktalarıyla eşlenen URL 'Ler de oluşturabilir.

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında daha fazla bilgi için bkz. <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz. <xref:razor-pages/razor-pages-conventions>.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, bir uygulamadaki mantıksal uç noktaları temsil etmek için *uç noktaları* (`Endpoint`) kullanır.

Bir uç nokta, istekleri işlemek için bir temsilci ve rastgele meta veri koleksiyonunu tanımlar. Meta veriler, her uç noktaya eklenen ilkelere ve yapılandırmaya göre çapraz kesme sorunları uygulamak için kullanılır.

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
> Uç nokta bağlama, MVC/Razor Pages eylemleri ve sayfalarıyla sınırlıdır. Uç nokta bağlama yeteneklerinin genişletmeleri, gelecek sürümlerde planlanmaktadır.

Yönlendirme, <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı tarafından, [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *uç noktaya*gelen isteği gönderdiği işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Bir yönlendirme ara yazılımı yürütüldüğünde, bir uç nokta (`Endpoint`) ayarlar ve değerleri <xref:Microsoft.AspNetCore.Http.HttpContext>bir özelliğe yönlendirir. Geçerli istek için:

* Çağırma `HttpContext.GetEndpoint` bitiş noktasını alır.
* `HttpRequest.RouteValues` yol değerlerinin koleksiyonunu alır.

Yönlendirme ara yazılımı ile çalışan ara yazılım, uç noktayı görebilir ve işlem yapabilir. Örneğin, bir yetkilendirme ara yazılımı, bir yetkilendirme ilkesi için bitiş noktasının meta veri koleksiyonunu sorgulanamıyor. İstek işleme ardışık düzeninde bulunan tüm ara yazılım yürütüldükten sonra, seçilen uç noktanın temsilcisi çağrılır.

Uç nokta yönlendirmesinde yönlendirme sistemi tüm gönderme kararlarından sorumludur. Ara yazılım, seçili uç noktaya göre ilkeler uyguladığı için, yönlendirme sisteminde güvenlik ilkelerinin dağıtımını etkileyebilecek veya uygulamanın uygulanmasını etkileyebilecek herhangi bir kararın olması önemlidir.

Uç nokta temsilcisi yürütüldüğünde, [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özellikleri, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlanır.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>, uygulamanın hangi yolun eşleştiğini temel alarak kararlar verebilmesi için durum verilerinin her bir rota ile ilişkilendirilmesini desteklemek amacıyla sağlanır. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>LinkGenerator ile URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, uç noktalarınız ve bunlara erişen URL 'Ler arasında mantıksal bir ayrım sağlar.

Uç nokta yönlendirme bağlantı Oluşturucu API 'sini (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) içerir. <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, DI 'dan alınabilecek bir tek hizmettir. API, yürütülen bir istek bağlamı dışında kullanılabilir. MVC 'nin [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>kullanan <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ve senaryoları, bağlantı oluşturma yetenekleri sağlamak için bağlantı oluşturucuyu kullanır.

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

| Genişletme yöntemi | Açıklama |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Belirtilen değerleri temel alarak mutlak bir yola sahip bir URI oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Belirtilen değerleri temel alarak mutlak bir URI oluşturur.             |

> [!WARNING]
> <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemlerinin çağrılmasının aşağıdaki etkilerine dikkat edin:
>
> * Gelen isteklerin `Host` üstbilgisini doğrulayan bir uygulama yapılandırmasında `GetUri*` uzantısı yöntemlerini dikkatle kullanın. Gelen isteklerin `Host` üstbilgisi doğrulandıktan sonra, güvenilir olmayan istek girişi, bir görünüm/sayfada URI 'Ler halinde istemciye geri gönderilebilir. Tüm üretim uygulamalarının, `Host` üst bilgisini bilinen geçerli değerlere karşı doğrulamak için kendi sunucusunu yapılandırmasını öneririz.
>
> * `Map` veya `MapWhen`birlikte ara yazılım ile birlikte <xref:Microsoft.AspNetCore.Routing.LinkGenerator> kullanın. `Map*`, yürütülen isteğin temel yolunu değiştirir ve bu da bağlantı oluşturma çıktısını etkiler. Tüm <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 'Leri temel yol belirtmeye izin verir. `Map*`bağlantı oluşturmada etkilerini geri almak için her zaman boş bir temel yol belirtin.

## <a name="endpoint-routing-differences-from-earlier-versions-of-routing"></a>Önceki yönlendirme sürümlerinden uç nokta yönlendirme farkları

ASP.NET Core 2,2 ' den önceki yönlendirmenin uç nokta yönlendirme ve sürümleri arasında birkaç fark vardır:

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

  | Yolu              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
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

### <a name="create-routes"></a>Rotalar oluştur

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

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

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

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; yalnızca HTTP GET istekleriyle eşleşir.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Aşağıdaki tabloda verilen URI 'Ler ile ilgili yanıtlar gösterilmektedir.

| URI                    | Yanıtıyla                                          |
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
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya `false` eşleşir (büyük/küçük harfe duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli bir `DateTime` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir `decimal` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir `double` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir `float` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
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
> URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. `^\d{3}-\d{2}-\d{4}$` normal ifade ' i yönlendirmede kullanmak için, ifadede, C# kaynak `\` dosyadaki `\\` (çift ters eğik çizgi) karakter olarak belirtilen `\` (tek ters eğik çizgi) karakterlere sahip olması gerekir. karakter (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `[`, `]`) atlamak için, ifadedeki karakterleri çift (`{{`, `}`, `[[`, `]]`). Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | MZ        | Evet   | Eşleşen ifadesi    |
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

Önceki yol ile eylem `SubscriptionManagementController.GetAll()`, URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıkışları `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla bir parametre dönüştürücüler kullanmak için API kuralları sağlar:

* ASP.NET Core MVC 'nin `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı vardır. Bu kural, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralına sahiptir. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin bir sözlüğü ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>için bir yol bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Yukarıdaki örnek sonunda oluşturulan <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> `/package/create/123`. Sözlük, "paket yolunu Izle" şablonunun `package/{operation}/{id}``operation` ve `id` yol değerlerini sağlar. Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

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
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

## <a name="configuring-endpoint-metadata"></a>Uç nokta meta verilerini yapılandırma

Aşağıdaki bağlantılar, uç nokta meta verilerini yapılandırma hakkında bilgi sağlar:

* [Uç nokta yönlendirme ile CORS 'yi etkinleştirme](xref:security/cors#enable-cors-with-endpoint-routing)
* Özel bir `[MinimumAgeAuthorize]` özniteliği kullanan [ıauthorizationpolicyprovider örneği](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [[Yetkilendir] özniteliğiyle test kimlik doğrulaması](xref:security/authentication/identity#test-identity)
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* [[Yetkilendir] özniteliğiyle düzeni seçme](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)
* [[Yetkilendir] özniteliği kullanılarak ilkeler uygulanıyor](xref:security/authorization/policies#applying-policies-to-mvc-controllers)
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a>RequireHost ile yollarla eşleşen ana bilgisayar

`RequireHost`, belirtilen Konağı gerektiren rotaya bir kısıtlama uygular. `RequireHost` veya `[Host]` parametresi şu olabilir:

* Ana bilgisayar: `www.domain.com` (herhangi bir bağlantı noktasıyla `www.domain.com` eşleşir)
* Joker karakterle ana bilgisayar: `*.domain.com` (herhangi bir bağlantı noktasında `www.domain.com`, `subdomain.domain.com`veya `www.subdomain.domain.com` eşleşir)
* Bağlantı noktası: `*:5000` (herhangi bir konakla bağlantı noktası 5000 ile eşleşir)
* Konak ve bağlantı noktası: `www.domain.com:5000`, `*.domain.com:5000` (ana bilgisayar ve bağlantı noktasıyla eşleşir)

`RequireHost` veya `[Host]`kullanılarak birden çok parametre belirtilebilir. Kısıtlama, parametrelerden herhangi biri için geçerli olan konaklara göre eşleşir. Örneğin, `[Host("domain.com", "*.domain.com")]` `domain.com`, `www.domain.com`veya `subdomain.domain.com`eşleştirecektir.

Aşağıdaki kod, rotada belirtilen ana bilgisayarı gerektirmek için `RequireHost` kullanır:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi Contoso!"))
            .RequireHost("contoso.com");
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi AdventureWorks!"))
            .RequireHost("adventure-works.com");
        endpoints.MapHealthChecks("/healthz").RequireHost("*:8080");
    });
}
```

Aşağıdaki kod, denetleyicide belirtilen Konağı gerektirmek için `[Host]` özniteliğini kullanır:

```csharp
[Host("contoso.com", "adventure-works.com")]
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    [Host("example.com:8080")]
    public IActionResult Privacy()
    {
        return View();
    }

}
```

`[Host]` özniteliği hem denetleyici hem de eylem yöntemine uygulandığında:

* Eylem üzerindeki özniteliği kullanılır.
* Denetleyici özniteliği yok sayılır.

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

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

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

Uç nokta yönlendirme bağlantı Oluşturucu API 'sini (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) içerir. <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, DI 'dan alınabilecek bir tek hizmettir. API, yürütülen bir istek bağlamı dışında kullanılabilir. MVC 'nin [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>kullanan <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ve senaryoları, bağlantı oluşturma yetenekleri sağlamak için bağlantı oluşturucuyu kullanır.

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

  | Yolu              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
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

### <a name="create-routes"></a>Rotalar oluştur

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

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

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

| URI                    | Yanıtıyla                                          |
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
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya `false` eşleşir (büyük/küçük harfe duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli bir `DateTime` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir `decimal` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir `double` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir `float` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
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
> URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. `^\d{3}-\d{2}-\d{4}$` normal ifade ' i yönlendirmede kullanmak için, ifadede, C# kaynak `\` dosyadaki `\\` (çift ters eğik çizgi) karakter olarak belirtilen `\` (tek ters eğik çizgi) karakterlere sahip olması gerekir. karakter (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `[`, `]`) atlamak için, ifadedeki karakterleri çift (`{{`, `}`, `[[`, `]]`). Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | MZ        | Evet   | Eşleşen ifadesi    |
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

Önceki yol ile eylem `SubscriptionManagementController.GetAll()`, URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıkışları `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla bir parametre dönüştürücüler kullanmak için API kuralları sağlar:

* ASP.NET Core MVC 'nin `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı vardır. Bu kural, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralına sahiptir. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin bir sözlüğü ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>için bir yol bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Yukarıdaki örnek sonunda oluşturulan <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> `/package/create/123`. Sözlük, "paket yolunu Izle" şablonunun `package/{operation}/{id}``operation` ve `id` yol değerlerini sağlar. Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

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
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
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

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, *yolları* kullanır (<xref:Microsoft.AspNetCore.Routing.IRouter>uygulamalar):

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

Yönlendirme, <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı tarafından, [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

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

### <a name="create-routes"></a>Rotalar oluştur

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

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

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

| URI                    | Yanıtıyla                                          |
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
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | `true` veya `false` eşleşir (büyük/küçük harfe duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli bir `DateTime` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir `decimal` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir `double` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir `float` değeriyle eşleşir (Sabit kültür içinde, bkz. uyarı) |
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
> URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlaması yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal ifade oluşturucusuna `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ekler. Bu üyelerin açıklaması için bkz. <xref:System.Text.RegularExpressions.RegexOptions>.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. `^\d{3}-\d{2}-\d{4}$` normal ifade ' i yönlendirmede kullanmak için, ifadede, C# kaynak `\` dosyadaki `\\` (çift ters eğik çizgi) karakter olarak belirtilen `\` (tek ters eğik çizgi) karakterlere sahip olması gerekir. karakter (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `[`, `]`) atlamak için, ifadedeki karakterleri çift (`{{`, `}`, `[[`, `]]`). Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` ve `$` karakterler, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` ve `$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | MZ        | Evet   | Eşleşen ifadesi    |
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

Yukarıdaki örnek sonunda oluşturulan <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> `/package/create/123`. Sözlük, "paket yolunu Izle" şablonunun `package/{operation}/{id}``operation` ve `id` yol değerlerini sağlar. Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

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
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
