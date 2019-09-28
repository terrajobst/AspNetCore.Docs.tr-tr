---
title: ASP.NET Core yönlendirme
author: rick-anderson
description: İstek URI 'Lerini uç nokta seçiclerine eşleyerek ve gelen istekleri uç noktalara gönderen ASP.NET Core yönlendirmenin nasıl sorumlu olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: fundamentals/routing
ms.openlocfilehash: c8037d79c79c5b7eb3b99d9724aa3e5361f92b8c
ms.sourcegitcommit: 5d25a7f22c50ca6fdd0f8ecd8e525822e1b35b7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2019
ms.locfileid: "71482038"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

[Ryan şimdi ak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Yönlendirme, istek URI 'Lerini uç noktalarla eşleştirmekten ve gelen istekleri bu uç noktalara gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yönlendirme bilgilerini kullanarak, yönlendirme, uç noktalarıyla eşlenen URL 'Ler de oluşturabilir.

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında bilgi için bkz <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz <xref:razor-pages/razor-pages-conventions>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, bir uygulamadaki`Endpoint`mantıksal uç noktaları temsil etmek için *uç noktaları* () kullanır.

Bir uç nokta, istekleri işlemek için bir temsilci ve rastgele meta veri koleksiyonunu tanımlar. Meta veriler, her uç noktaya eklenen ilkelere ve yapılandırmaya göre çapraz kesme sorunları uygulamak için kullanılır.

Yönlendirme sistemi aşağıdaki özelliklere sahiptir:

* Yol şablonu sözdizimi, simgeleştirilmiş yol parametrelerine sahip yolları tanımlamak için kullanılır.
* Geleneksel stil ve öznitelik stili uç nokta yapılandırmasına izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir URL parametresinin belirli bir uç nokta kısıtlaması için geçerli bir değer içerip içermediğini belirlemekte kullanılır.
* MVC/Razor Pages gibi uygulama modelleri, yönlendirme senaryolarının öngörülebilir bir uygulaması olan tüm uç noktalarını kaydeder.
* Yönlendirme gerçekleştirme, yönlendirme kararlarını, ara yazılım ardışık düzeninde istediğiniz yere getirir.
* Bir yönlendirme ara yazılımı, belirli bir istek URI 'SI için yönlendirme ara yazılımı uç noktası kararının sonucunu inceleyebilir.
* Uygulamadaki tüm uç noktaları, ara yazılım ardışık düzeninde herhangi bir yerde listelemek mümkündür.
* Bir uygulama, uç nokta bilgilerine göre URL 'Ler oluşturmak için (örneğin, yeniden yönlendirme veya bağlantılar için) yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.
* URL oluşturma, rastgele genişletilebilirliği destekleyen adreslere dayalıdır:

  * Bağlantı Oluşturucu API 'si (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>), URL oluşturmak için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanılarak her yerde çözülebilir.
  * Bağlantı Oluşturucu API 'sinin dı <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> aracılığıyla kullanılamadığı durumlarda URL 'leri derlemek için yöntemler sunar.

> [!NOTE]
> Uç nokta bağlama, MVC/Razor Pages eylemleri ve sayfalarıyla sınırlıdır. Uç nokta bağlama yeteneklerinin genişletmeleri, gelecek sürümlerde planlanmaktadır.

Yönlendirme, sınıfı tarafından <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> bulunan [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *uç noktaya*gelen isteği gönderdiği işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Bir yönlendirme ara yazılımı yürütüldüğünde, bir uç nokta (`Endpoint`) ve yollar değerlerini ' <xref:Microsoft.AspNetCore.Http.HttpContext>de bir özelliğe ayarlar. Geçerli istek için:

* Çağırma `HttpContext.GetEndpoint` bitiş noktasını alır.
* `HttpRequest.RouteValues`yol değerlerinin koleksiyonunu alır.

Yönlendirme ara yazılımı ile çalışan ara yazılım, uç noktayı görebilir ve işlem yapabilir. Örneğin, bir yetkilendirme ara yazılımı, bir yetkilendirme ilkesi için bitiş noktasının meta veri koleksiyonunu sorgulanamıyor. İstek işleme ardışık düzeninde bulunan tüm ara yazılım yürütüldükten sonra, seçilen uç noktanın temsilcisi çağrılır.

Uç nokta yönlendirmesinde yönlendirme sistemi tüm gönderme kararlarından sorumludur. Ara yazılım, seçili uç noktaya göre ilkeler uyguladığı için, yönlendirme sisteminde güvenlik ilkelerinin dağıtımını etkileyebilecek veya uygulamanın uygulanmasını etkileyebilecek herhangi bir kararın olması önemlidir.

Uç nokta temsilcisi yürütüldüğünde, [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özellikleri, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlanır.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>Her rotayla durum verilerinin ilişkilendirilmesini desteklemek için sağlanır, böylece uygulama hangi yolun eşleştiğini temel alarak kararlar alabilir. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> Özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, içindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. İçindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>LinkGenerator ile URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, uç noktalarınız ve bunlara erişen URL 'Ler arasında mantıksal bir ayrım sağlar.

Endpoint Routing, bağlantı Oluşturucu API 'SI (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) içerir. <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, DI 'den alınabilecek bir tek hizmettir. API, yürütülen bir istek bağlamı dışında kullanılabilir. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi mvc 'nin ve senaryolarına yönelik senaryolar <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, bağlantı oluşturma özellikleri sağlamak için bağlantı oluşturucuyu kullanır.

Bağlantı Oluşturucu, bir *Adres* ve *Adres şemaları*kavramıyla desteklenir. Adres şeması, bağlantı oluşturma için göz önünde bulundurmanız gereken uç noktaları belirlemenin bir yoludur. Örneğin, çok sayıda kullanıcının yol adı ve yol değerleri senaryoları, MVC/Razor Pages tarafından tanıdık bir adres düzeni olarak uygulanır.

Bağlantı Oluşturucu, aşağıdaki genişletme yöntemleri aracılığıyla MVC/Razor Pages eylemlerine ve sayfalarına bağlanabilir:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Bu yöntemlerin aşırı yüklemesi, `HttpContext`içeren bağımsız değişkenleri kabul eder. Bu yöntemler işlevsel olarak `Url.Action` `Url.Page` eşdeğerdir, ancak ek esneklik ve seçenekler sunar.

Yöntemler `GetPath*` , mutlak bir yol içeren `Url.Action` bir `Url.Page` URI oluşturabilen ve ' a benzerdir. `GetUri*` Yöntemler her zaman bir düzen ve konak içeren mutlak bir URI oluşturur. Bir `HttpContext` öğesini kabul eden yöntemler, yürütülmekte olan istek bağlamında bir URI oluşturur. Ortam yolu değerleri, URL taban yolu, şeması ve yürütülen istekten ana bilgisayar, geçersiz kılınmadıkça kullanılır.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator>bir adresle çağırılır. URI oluşturma iki adımda gerçekleşir:

1. Adres, adresle eşleşen bir uç nokta listesine bağlanır.
1. Her uç nokta `RoutePattern` , sağlanan değerlerle eşleşen bir yol deseninin bulunana kadar değerlendirilir. Elde edilen çıktı, bağlantı oluşturucuya sağlanan diğer URI parçalarıyla birleştirilir ve döndürülür.

Tarafından <xref:Microsoft.AspNetCore.Routing.LinkGenerator> sunulan yöntemler, herhangi bir adres türü için standart bağlantı oluşturma yeteneklerini destekler. Bağlantı oluşturucuyu kullanmanın en kolay yolu, belirli bir adres türü için işlem gerçekleştiren genişletme yöntemlerine yöneliktir.

| Genişletme yöntemi | Açıklama |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Belirtilen değerleri temel alarak mutlak bir yola sahip bir URI oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Belirtilen değerleri temel alarak mutlak bir URI oluşturur.             |

> [!WARNING]
> Çağırma <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemlerinin aşağıdaki etkilerine dikkat edin:
>
> * Gelen `GetUri*` isteklerin `Host` üstbilgisini doğrulayan bir uygulama yapılandırmasında uzantı yöntemlerini dikkatle kullanın. Gelen isteklerin `Host` üstbilgisi doğrulandıktan sonra, güvenilir olmayan istek girişi, bir görünüm/sayfada URI 'ler halinde istemciye geri gönderilebilir. Tüm üretim uygulamalarının, `Host` üst bilgisini bilinen geçerli değerlere karşı doğrulamak için kendi sunucusunu yapılandırmasını öneririz.
>
> * Veya <xref:Microsoft.AspNetCore.Routing.LinkGenerator> ile`MapWhen`birlikte ara yazılım içinde dikkatli kullanın. `Map` `Map*`yürütülen isteğin temel yolunu değiştirir ve bu da bağlantı oluşturma çıktısını etkiler. <xref:Microsoft.AspNetCore.Routing.LinkGenerator> Tüm API 'ler temel yol belirtilmesine izin verir. Bağlantı oluşturma işlemi geri almak `Map*`için her zaman boş bir temel yol belirtin.

## <a name="differences-from-earlier-versions-of-routing"></a>Yönlendirmenin önceki sürümlerinden farklılıklar

ASP.NET Core 2,2 ' den önceki yönlendirmenin uç nokta yönlendirme ve sürümleri arasında birkaç fark vardır:

* Uç nokta yönlendirme sistemi, <xref:Microsoft.AspNetCore.Routing.IRouter> <xref:Microsoft.AspNetCore.Routing.Route>devralma dahil olmak üzere tabanlı genişletilebilirliği desteklemez.

* Uç nokta yönlendirme [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)desteklemez. Uyumluluk Shim 'yi kullanmaya devam etmek`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`için 2,1 [Uyumluluk sürümünü](xref:mvc/compatibility-version) () kullanın.

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

  Tabanlı <xref:Microsoft.AspNetCore.Routing.IRouter>yönlendirme ile, bu kod, belirtilen rota değerinin büyük `/blog/ReadPost/17`küçük harflerini belirten bir URI oluşturur. ASP.NET Core 2,2 veya üzeri bir nokta yönlendirme ( `/Blog/ReadPost/17` "blog" büyük harfli) üretir. Endpoint Routing, `IOutboundParameterTransformer` bu davranışı genel olarak özelleştirmek veya URL eşleme için farklı kurallar uygulamak üzere kullanılabilecek arabirimi sağlar.

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

  Tabanlı `IRouter`yönlendirme ile, mevcut olmasa veya bir `ReadPost` eylem `/Blog/ReadPost/17`yöntemine sahip olmasalar bile `BlogController` sonuç her zaman olur. Beklendiği gibi, ASP.NET Core 2,2 veya sonraki bir sürümde uç nokta `/Blog/ReadPost/17` yönlendirme eylem yöntemi varsa üretir. *Ancak, uç nokta yönlendirme, eylem yoksa boş bir dize oluşturur.* Kavramsal olarak, uç nokta yönlendirme, eylem mevcut değilse uç noktanın var olduğunu varsaymaz.

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

  URI `/Store/Product/18` ASP.NET Core 2,1 veya daha önceki bir sürümdeyse, tarafından `@Url.Page("/Login")` Store/Info sayfasında oluşturulan bağlantı olur `/Login/18`. 16 `id` değeri, bağlantı hedefi uygulamanın tamamen farklı bir parçası olsa da yeniden kullanılır. Sayfabağlamındaki`/Login` yol değeri büyük olasılıkla bir mağaza ürün kimliği değeri değil, bir kullanıcı kimliği değeridir. `id`

  ASP.NET Core 2,2 veya sonraki bir sürümü ile Endpoint Routing de sonuç olur `/Login`. Bağlantılı hedef farklı bir eylem veya sayfa olduğunda çevresel değerler yeniden kullanılmaz.

* Gidiş dönüşü yol parametresi sözdizimi: Çift yıldız (`**`) catch-all parametre sözdizimi kullanılırken eğik çizgiler kodlanmaz.

  Bağlantı oluşturma sırasında, yönlendirme sistemi, eğik çizgiler hariç, bir çift yıldız (`**`) catch-all parametresinde (örneğin, `{**myparametername}`) yakalanan değeri kodlar. Çift yıldız yakalama, ASP.NET Core 2,2 veya üzeri sürümlerde tabanlı `IRouter`yönlendirme ile desteklenir.

  ASP.NET Core (`{*myparametername}`) öğesinin önceki sürümlerindeki tek yıldız catch-all parametre sözdizimi desteklenmeye devam eder ve eğik çizgi kodlandı.

  | Yolu              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts`(eğik çizgi kodlandı)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ara yazılım örneği

Aşağıdaki örnekte, bir ara yazılım, mağaza ürünlerini <xref:Microsoft.AspNetCore.Routing.LinkGenerator> listeleyen bir eylem yöntemine bağlantı oluşturmak için API 'yi kullanır. Bağlantı Oluşturucuyu bir sınıfa ekleme ve çağırarak `GenerateLink` bir uygulamadaki herhangi bir sınıf için kullanılabilir.

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

Çoğu uygulama, ' de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>tanımlanan benzer uzantı yöntemlerinden birini çağırarak veya arayarak yollar oluşturur. Uzantı yöntemlerinden herhangi biri bir <xref:Microsoft.AspNetCore.Routing.Route> örneği oluşturur ve bunu yol koleksiyonuna ekler. <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yol işleyici parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yalnızca tarafından <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>işlenen rotaları ekler. MVC 'de yönlendirme hakkında daha fazla bilgi için bkz <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, tipik bir ASP.NET Core MVC yol <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> tanımı tarafından kullanılan bir çağrının örneğidir:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yoluyla eşleşir ve yol değerlerini ayıklar. Örneğin, yol `/Products/Details/17` şu yol değerlerini oluşturur: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri, URL yolunun kesimlere bölünüyor ve rota şablonundaki *yol parametresi* adı ile her bir segmenti eşleştirerek belirlenir. Yol parametreleri olarak adlandırılır. Küme ayraçları `{ ... }`içinde parametre adının çevreleme tarafından tanımlanan parametreler.

Yukarıdaki şablon URL yoluyla `/` da eşleştirebilir ve değerleri `{ controller = Home, action = Index }`üretebilir. Bu durum, `{controller}` ve `{action}` rota parametrelerinin varsayılan değerleri olduğu ve `id` Route parametresinin isteğe bağlı olması nedeniyle oluşur. Bir eşittir işareti (`=`), yol parametre adından sonra gelen bir değer, parametre için varsayılan bir değer tanımlar. Yol parametre adından sonra`?`bir soru işareti () isteğe bağlı bir parametre tanımlar.

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

Aşağıdaki örnekte, Route parametresi tanımı `{id:int}` `id` yol parametresi için bir [yol kısıtlaması](#route-constraint-reference) tanımlar:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon, gibi `/Products/Details/17` `/Products/Details/Apples`bir URL yoluyla eşleşir. Yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> , yol değerlerini uygulayıp onları doğrulamak için inceler. Bu örnekte, yol değeri `id` bir tamsayıya dönüştürülebilir olmalıdır. Framework tarafından sunulan yol kısıtlamalarının açıklaması için bkz. [route-Constraint-Reference](#route-constraint-reference) .

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> `constraints`, Ve`defaults`içindeğer kabul etmenin ek aşırı yüklemeleri. `dataTokens` Bu parametrelerin tipik kullanımı, anonim türdeki özellik adlarının yol parametre adlarıyla eşleşen anonim olarak yazılmış bir nesneyi geçirmektir.

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
> Kısıtlamaları ve Varsayılanları tanımlamaya yönelik satır içi sözdizimi basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi senaryolar vardır.

Aşağıdaki örnek birkaç ek senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablon, benzer `/Blog/All-About-Routing/Introduction` bir URL yoluyla eşleşir ve değerleri `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`ayıklar. `controller` Ve`action` için varsayılan yol değerleri, şablonda karşılık gelen hiçbir yol parametresi olmasa bile rota tarafından üretilir. Varsayılan değerler yol şablonunda belirtilebilir. Route parametresi, yol parametre adından önce bir çift yıldız işareti (`**`) görünümüne göre *catch-all* olarak tanımlanır. `article` Catch-all yol parametreleri, URL yolunun kalanını yakalar ve boş dizeyle de aynı olabilir.

Aşağıdaki örnek yol kısıtlamalarını ve veri belirteçlerini ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablon, benzer `/en-US/Products/5` bir URL yoluyla eşleşir ve değerleri `{ controller = Products, action = Details, id = 5 }` ve veri belirteçlerini `{ locale = en-US }`ayıklar.

![Yereller Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Yol sınıfı URL 'SI oluşturma

Sınıf <xref:Microsoft.AspNetCore.Routing.Route> , yol değerlerini bir kümesini rota şablonuyla birleştirerek URL oluşturma işlemi de gerçekleştirebilir. Bu, URL yolunu eşleştirmenin mantıksal bir işlemdir.

> [!TIP]
> URL oluşturmayı daha iyi anlamak için, oluşturmak istediğiniz URL 'YI düşünün ve sonra bir yönlendirme şablonunun bu URL ile nasıl eşleşeceğini düşünün. Hangi değerler üretilemidir? Bu, URL oluşturma işlevinin <xref:Microsoft.AspNetCore.Routing.Route> sınıfında nasıl çalıştığına ilişkin kaba bir eştir.

Aşağıdaki örnek genel ASP.NET Core MVC varsayılan yolunu kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Yol değerleriyle `{ controller = Products, action = List }`, URL `/Products/List` oluşturulur. Yol değerleri, URL yolunu biçimlendirmek için karşılık gelen yol parametrelerinin yerine kullanılır. , İsteğe bağlı bir yol parametresi `id` olduğundan,URL'siiçinbirdeğerolmadanbaşarıylaoluşturulur.`id`

Yol değerleriyle `{ controller = Home, action = Index }`, URL `/` oluşturulur. Belirtilen yol değerleri varsayılan değerlerle eşleşir ve varsayılan değerlere karşılık gelen segmentler güvenle atlanır.

Her iki URL de aşağıdaki yol tanımıyla (`/Home/Index` ve `/`) birlikte gidiş dönüş, URL oluşturmak için kullanılan rota değerlerini oluşturur.

> [!NOTE]
> ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> kullanan bir uygulama, doğrudan yönlendirmeye çağırmak yerine URL 'ler oluşturmak için kullanılmalıdır.

URL oluşturma hakkında daha fazla bilgi için [URL oluşturma başvurusu](#url-generation-reference) bölümüne bakın.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılımı kullanma

Uygulamanın proje dosyasındaki [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) öğesine başvurun.

İçindeki `Startup.ConfigureServices`hizmet kapsayıcısına yönlendirme ekle:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash; Yalnızca http get isteklerini eşleştirir.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

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

Framework, yollar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>) oluşturmak için bir genişletme yöntemleri kümesi sağlar:

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

`Map[Verb]` Yöntemler, yöntem adındaki http fiili ile rotayı sınırlandırmak için kısıtlamalar kullanır. Örneğin, bkz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> . ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları (`{ ... }`) içindeki belirteçler, yol eşleştirildiği takdirde bağlanan *Rota parametrelerini* tanımlar. Bir yol segmentinde birden fazla yol parametresi tanımlayabilirsiniz, ancak bunların bir değişmez değer ile ayrılması gerekir. Örneğin, `{controller=Home}{action=Index}` ve `{controller}` arasında`{action}`değişmez değer olmadığından geçerli bir yol değil. Bu rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametrelerinden (örneğin, `{id}`) ve yol ayırıcısından `/` farklı bir metin, URL içindeki metinle eşleşmelidir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Bir sabit yol`{` parametresi sınırlayıcısından (veya `}`) eşleştirmek için, karakteri (`{{` veya `}}`) tekrarlayarak sınırlayıcıdan kaçış.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, şablonu `files/{filename}.{ext?}`göz önünde bulundurun. Hem hem de `filename` `ext` için değerler olduğunda her iki değer de doldurulur. URL 'de yalnızca bir değeri `filename` varsa, sondaki nokta (`.`) isteğe bağlı olduğundan yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

URI 'nin geri kalanına bağlamak için`*`bir yol parametresinin öneki olarak`**`bir yıldız işareti () veya çift yıldız işareti () kullanabilirsiniz. Bunlara *catch-all* parametreleri denir. Örneğin, `blog/{**slug}` ile `/blog` başlayan ve bunu izleyen bir değere sahip olan ve `slug` yol değerine atanan herhangi bir URI ile eşleşir. Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı (`/`) karakterleri de dahil olmak üzere bir URL oluşturmak için, catch-all parametresi uygun karakterleri de çıkar. Örneğin, yol değerlerini `foo/{*path}` `{ path = "my/path" }` içeren yol oluşturulur `foo/my%2Fpath`. Atlanan eğik çizgiye göz önünde edin. Yol ayırıcı karakterlerini yuvarlaklaştırmak için `**` rota parametresi önekini kullanın. İle `foo/{**path}` Rota`{ path = "my/path" }` oluşturulur .`foo/my/path`

Yol parametreleri, parametre adından sonra bir eşittir işaretiyle (`=`) ayırarak varsayılan değer belirtilerek belirlenmiş *varsayılan değerlere* sahip olabilir. Örneğin, `{controller=Home}` için `Home` varsayılan`controller`değer olarak tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, içinde`?` `id?`olduğu gibi parametre adının sonuna bir soru işareti () eklenerek isteğe bağlı olarak yapılır. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark, varsayılan değere sahip bir yol parametresinin her zaman bir değer&mdash;ürettiğinden, isteğe bağlı bir parametre yalnızca istek URL 'si tarafından bir değer sağlandığı zaman bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra`:`bir iki nokta () ve kısıtlama adı eklemek, bir rota parametresinde bir *satır içi kısıtlamayı* belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez (`(...)`) içine alınır. Birden çok satır içi kısıtlama, başka bir iki nokta (`:`) ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılmak <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> üzere bir örneği oluşturmak için hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}` bağımsız değişkenle `10`bir `minlength` kısıtlama belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Yol parametrelerinde Ayrıca, bağlantı oluştururken ve URL 'Ler ile eşleşen eylemler ve sayfalar için bir parametrenin değerini dönüştüren parametre dönüştürücüler bulunabilir. LIKE kısıtlamaları, yol parametre adından sonra iki nokta üst üste (`:`) ve transformatör adı eklenerek, parametre dönüştürücüleri bir rota parametresine eklenebilir. Örneğin, yol şablonu `blog/{article:slugify}` bir `slugify` transformatör belirtir. Parametre dönüştürücüler hakkında daha fazla bilgi için bkz. [Parameter transformatör başvurusu](#parameter-transformer-reference) bölümü.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir.

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca tek bir yolla `/hello`eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | İle eşleşir ve `Page` ayarlanır `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | İle eşleşir ve `Page` ayarlanır `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` Denetleyiciye ve`List` eyleme eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Denetleyici ve `Details` eyleme eşlenir (`id` 123 olarak ayarlanır). `Products` |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Denetleyiciye ve `Index` yönteme eşlenir (`id` yok sayılır). `Home`        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

> [!TIP]
> İstekleri eşleme gibi yerleşik yönlendirme uygulamalarının <xref:Microsoft.AspNetCore.Routing.Route>nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

## <a name="reserved-routing-names"></a>Ayrılmış yönlendirme adları

Aşağıdaki anahtar sözcükler ayrılmış isimlerdir ve yol adları veya parametreler olarak kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Rota kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda bir Evet/Hayır kararı getirir. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> bir isteği http fiiline bağlı olarak kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> **Giriş doğrulaması**için kısıtlamaları kullanmayın. **Giriş doğrulaması**için kısıtlamalar kullanılıyorsa, doğru bir hata iletisine sahip *400-Bad isteği* yerine *404-* olmayan bir Yanıt ile geçersiz giriş oluşur. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların **belirsizliğini ortadan** kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve bunların beklenen davranışları gösterilmektedir.

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | Eşleşiyor `true` veya`false` (büyük/küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli `DateTime` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli `decimal` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli `double` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli `float` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli `Guid` bir değerle eşleşir |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli `long` bir değerle eşleşir |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize 8 karakterden uzun olmamalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 ve en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri 120 ' ten fazla olmamalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 olmalı ancak 120 ' ten fazla olmamalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır (`a`-`z`büyük/küçük harfe duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir (normal ifade tanımlama hakkında ipuçlarına bakın) |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır |

Birden çok, iki nokta üst üste sınırlı kısıtlama tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'yi doğrulayan ve bir clr türüne ( `int` veya `DateTime`gibi) dönüştürülen yol kısıtlamaları, her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlama yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ifade oluşturucusuna ekler. Bu <xref:System.Text.RegularExpressions.RegexOptions> üyelerin açıklaması için bkz.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. Yönlendirmenin normal ifadesini `^\d{3}-\d{2}-\d{4}$` kullanmak için, ifade `\` , C# kaynak dosyadaki dize olarak (çift ters eğik çizgi) karakter olarak `\\` belirtilmelidir ve `\` dize kaçış karakteri (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `]` ,)atlamak`]]`için, ifadedeki karakterleri (`{{` ,,,)çift.`}` `[[` `[` Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler, genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` Ve`$` karakterleri, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` Ve`$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | Herkese     | Hayır    | Bkz `^` . `$` ve üzeri |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Bkz `^` . `$` ve üzeri |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. `{action:regex(^(list|get|create)$)}` Örneğin, yalnızca `action` rota değeri `list`, `get`, veya`create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğüne (bir şablon içinde satır içi değil) geçirilen kısıtlamalar da normal ifadeler olarak kabul edilir.

## <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Yerleşik yol kısıtlamalarına ek olarak, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak özel yol kısıtlamaları oluşturulabilir. Arabirim, kısıtlama `Match` `true` karşılanıp`false` Aksi takdirde döndüren tek bir yöntemi içerir. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>

Özel <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir kullanmak için yol kısıtlama türü, uygulamanın uygulamanın hizmet kapsayıcısında kayıtlı <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> olması gerekir. , Yol kısıtlama <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> anahtarlarını bu kısıtlamaları doğrulayan uygulamalarla eşleyen bir sözlüktür. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> Bir uygulama <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> , hizmetlerin bir parçası olarak `Startup.ConfigureServices` ' de güncelleştirilir [. Addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya doğrudan ile <xref:Microsoft.AspNetCore.Routing.RouteOptions> `services.Configure<RouteOptions>`yapılandırma. Örneğin:

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

* Bir <xref:Microsoft.AspNetCore.Routing.Route>bağlantısını oluştururken yürütün.
* Uygulayın `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Kullanılarak <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>yapılandırılır.
* Parametrenin yol değerini alın ve yeni bir dize değerine dönüştürün.
* Oluşturulan bağlantıda Dönüştürülmüş değerin kullanılmasına neden olacak.

Örneğin, `slugify` `blog\my-test-article`yol `blog\{article:slugify}` düzenindebirözel`Url.Action(new { article = "MyTestArticle" })` parametre transformatörü oluşturur.

Bir parametre transformatörü bir yol düzeninde kullanmak için, önce ' de <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> `Startup.ConfigureServices`kullanarak bunu yapılandırın:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Parametre dönüştürücüler, bir uç noktanın çözümlediği URI 'yi dönüştürmek için çerçevesi tarafından kullanılır. `area`Örneğin, ASP.NET Core MVC `controller` `action`,,, ve `page`ile eşleşecek şekilde kullanılan rota değerini dönüştürmek için parametre dönüştürücüler kullanır.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Önceki yol ile, eylem `SubscriptionManagementController.GetAll()` URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıktılar `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla bir parametre dönüştürücüler kullanmak için API kuralları sağlar:

* ASP.NET Core MVC 'nin `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı vardır. Bu kural, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralına sahiptir. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>sözlüğünün bir sözlüğüne verilen bir yolun bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Önceki örnek sonunda <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>oluşturulan. `/package/create/123` Sözlük, " `operation` paketyolunu`id` izle" şablonunun ve rota değerlerini sağlar. `package/{operation}/{id}` Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> Oluşturucunun ikinci parametresi bir *ortam değerleri*koleksiyonudur. Ortam değerleri, bir geliştiricinin bir istek bağlamı içinde belirtmesi gereken değer sayısını sınırlandırdığından kullanım için uygundur. Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir. ASP.NET Core `About` MVC uygulamasının `Index` &mdash;öğesinin `HomeController`eyleminde, ortam değeri`Home` kullanılan eyleme bağlamak için denetleyici yol değerini belirtmeniz gerekmez.

Bir parametreyle eşleşmeyen çevresel değerler yok sayılır. Ayrıca, açıkça sağlanmış bir değer çevresel değeri geçersiz kıldığında çevresel değerler de yoksayılır. Eşleştirme, URL 'de soldan sağa doğru gerçekleşir.

Açık olarak sağlanmış ancak yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda, yol şablonu `{controller}/{action}/{id?}`kullanılırken sonuç gösterilmektedir.

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

Bağlantı oluşturma yalnızca ve `controller` `action` için eşleşen değerler sağlandığında bu yol için bir bağlantı oluşturur.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin `[Route("/x{token}y")]`), sabit değerli olmayan değişmez değer ile sağdan sola eşleştirilirken işlenir. Karmaşık segmentlerin nasıl eşleştirileceği hakkında ayrıntılı bir açıklama için [bu koda](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) bakın. [Kod örneği](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler hakkında iyi bir açıklama sağlar.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Yönlendirme, istek URI 'Lerini uç noktalarla eşleştirmekten ve gelen istekleri bu uç noktalara gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yönlendirme bilgilerini kullanarak, yönlendirme, uç noktalarıyla eşlenen URL 'Ler de oluşturabilir.

ASP.NET Core 2,2 ' de en son yönlendirme senaryolarını kullanmak için, ' de `Startup.ConfigureServices`MVC Hizmetleri kaydının [Uyumluluk sürümünü](xref:mvc/compatibility-version) belirtin:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Bu seçenek yönlendirmenin ASP.NET Core 2,1 veya önceki bir sürümünün uç nokta tabanlı Logic <xref:Microsoft.AspNetCore.Routing.IRouter>mi yoksa tabanlı mantığını mı kullanması gerektiğini belirler. <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> Uyumluluk sürümü 2,2 veya üzeri bir değere ayarlandığında, varsayılan değer olur `true`. Önceki yönlendirme mantığını kullanmak `false` için değeri olarak ayarlayın:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Tabanlı yönlendirme hakkında <xref:Microsoft.AspNetCore.Routing.IRouter>daha fazla bilgi için [Bu konunun ASP.NET Core 2,1 sürümüne](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)bakın.

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında bilgi için bkz <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz <xref:razor-pages/razor-pages-conventions>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme, bir uygulamadaki`Endpoint`mantıksal uç noktaları temsil etmek için *uç noktaları* () kullanır.

Bir uç nokta, istekleri işlemek için bir temsilci ve rastgele meta veri koleksiyonunu tanımlar. Meta veriler, her bir uç noktaya eklenen ilkelere ve yapılandırmaya bağlı olarak çapraz kesme sorunları uygulamak için kullanılır.

Yönlendirme sistemi aşağıdaki özelliklere sahiptir:

* Yol şablonu sözdizimi, simgeleştirilmiş yol parametrelerine sahip yolları tanımlamak için kullanılır.
* Geleneksel stil ve öznitelik stili uç nokta yapılandırmasına izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir URL parametresinin belirli bir uç nokta kısıtlaması için geçerli bir değer içerip içermediğini belirlemekte kullanılır.
* MVC/Razor Pages gibi uygulama modelleri, yönlendirme senaryolarının öngörülebilir bir uygulaması olan tüm uç noktalarını kaydeder.
* Yönlendirme gerçekleştirme, yönlendirme kararlarını, ara yazılım ardışık düzeninde istediğiniz yere getirir.
* Bir yönlendirme ara yazılımı, belirli bir istek URI 'SI için yönlendirme ara yazılımı uç noktası kararının sonucunu inceleyebilir.
* Uygulamadaki tüm uç noktaları, ara yazılım ardışık düzeninde herhangi bir yerde listelemek mümkündür.
* Bir uygulama, uç nokta bilgilerine göre URL 'Ler oluşturmak için (örneğin, yeniden yönlendirme veya bağlantılar için) yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.
* URL oluşturma, rastgele genişletilebilirliği destekleyen adreslere dayalıdır:

  * Bağlantı Oluşturucu API 'si (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>), URL oluşturmak için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanılarak her yerde çözülebilir.
  * Bağlantı Oluşturucu API 'sinin dı <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> aracılığıyla kullanılamadığı durumlarda URL 'leri derlemek için yöntemler sunar.

> [!NOTE]
> ASP.NET Core 2,2 ' de uç nokta yönlendirme yayını ile, uç nokta bağlama MVC/Razor Pages eylemleri ve sayfaları ile sınırlıdır. Uç nokta bağlama yeteneklerinin genişletmeleri, gelecek sürümlerde planlanmaktadır.

Yönlendirme, sınıfı tarafından <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> bulunan [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *uç noktaya*gelen isteği gönderdiği işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Uç nokta yönlendirmesinde yönlendirme sistemi tüm gönderme kararlarından sorumludur. Ara yazılım, seçili uç noktaya göre ilkeler uyguladığı için, yönlendirme sisteminde güvenlik ilkelerinin dağıtımını etkileyebilecek veya uygulamanın uygulanmasını etkileyebilecek herhangi bir kararın olması önemlidir.

Uç nokta temsilcisi yürütüldüğünde, [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özellikleri, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlanır.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>Her rotayla durum verilerinin ilişkilendirilmesini desteklemek için sağlanır, böylece uygulama hangi yolun eşleştiğini temel alarak kararlar alabilir. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> Özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, içindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. İçindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>LinkGenerator ile URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, uç noktalarınız ve bunlara erişen URL 'Ler arasında mantıksal bir ayrım sağlar.

Endpoint Routing, bağlantı Oluşturucu API 'SI (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) içerir. <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, DI 'den alınabilecek bir tek hizmettir. API, yürütülen bir istek bağlamı dışında kullanılabilir. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), HTML Yardımcıları ve [eylem sonuçları](xref:mvc/controllers/actions)gibi mvc 'nin ve senaryolarına yönelik senaryolar <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, bağlantı oluşturma özellikleri sağlamak için bağlantı oluşturucuyu kullanır.

Bağlantı Oluşturucu, bir *Adres* ve *Adres şemaları*kavramıyla desteklenir. Adres şeması, bağlantı oluşturma için göz önünde bulundurmanız gereken uç noktaları belirlemenin bir yoludur. Örneğin, çok sayıda kullanıcının yol adı ve yol değerleri senaryoları, MVC/Razor Pages tarafından tanıdık bir adres düzeni olarak uygulanır.

Bağlantı Oluşturucu, aşağıdaki genişletme yöntemleri aracılığıyla MVC/Razor Pages eylemlerine ve sayfalarına bağlanabilir:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Bu yöntemlerin aşırı yüklemesi, `HttpContext`içeren bağımsız değişkenleri kabul eder. Bu yöntemler işlevsel olarak `Url.Action` `Url.Page` eşdeğerdir, ancak ek esneklik ve seçenekler sunar.

Yöntemler `GetPath*` , mutlak bir yol içeren `Url.Action` bir `Url.Page` URI oluşturabilen ve ' a benzerdir. `GetUri*` Yöntemler her zaman bir düzen ve konak içeren mutlak bir URI oluşturur. Bir `HttpContext` öğesini kabul eden yöntemler, yürütülmekte olan istek bağlamında bir URI oluşturur. Ortam yolu değerleri, URL taban yolu, şeması ve yürütülen istekten ana bilgisayar, geçersiz kılınmadıkça kullanılır.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator>bir adresle çağırılır. URI oluşturma iki adımda gerçekleşir:

1. Adres, adresle eşleşen bir uç nokta listesine bağlanır.
1. Her uç nokta `RoutePattern` , sağlanan değerlerle eşleşen bir yol deseninin bulunana kadar değerlendirilir. Elde edilen çıktı, bağlantı oluşturucuya sağlanan diğer URI parçalarıyla birleştirilir ve döndürülür.

Tarafından <xref:Microsoft.AspNetCore.Routing.LinkGenerator> sunulan yöntemler, herhangi bir adres türü için standart bağlantı oluşturma yeteneklerini destekler. Bağlantı oluşturucuyu kullanmanın en kolay yolu, belirli bir adres türü için işlem gerçekleştiren genişletme yöntemlerine yöneliktir.

| Genişletme yöntemi   | Açıklama                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Belirtilen değerleri temel alarak mutlak bir yola sahip bir URI oluşturur. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Belirtilen değerleri temel alarak mutlak bir URI oluşturur.             |

> [!WARNING]
> Çağırma <xref:Microsoft.AspNetCore.Routing.LinkGenerator> yöntemlerinin aşağıdaki etkilerine dikkat edin:
>
> * Gelen `GetUri*` isteklerin `Host` üstbilgisini doğrulayan bir uygulama yapılandırmasında uzantı yöntemlerini dikkatle kullanın. Gelen isteklerin `Host` üstbilgisi doğrulandıktan sonra, güvenilir olmayan istek girişi, bir görünüm/sayfada URI 'ler halinde istemciye geri gönderilebilir. Tüm üretim uygulamalarının, `Host` üst bilgisini bilinen geçerli değerlere karşı doğrulamak için kendi sunucusunu yapılandırmasını öneririz.
>
> * Veya <xref:Microsoft.AspNetCore.Routing.LinkGenerator> ile`MapWhen`birlikte ara yazılım içinde dikkatli kullanın. `Map` `Map*`yürütülen isteğin temel yolunu değiştirir ve bu da bağlantı oluşturma çıktısını etkiler. <xref:Microsoft.AspNetCore.Routing.LinkGenerator> Tüm API 'ler temel yol belirtilmesine izin verir. Bağlantı oluşturma işlemi geri almak `Map*`için her zaman boş bir temel yol belirtin.

## <a name="differences-from-earlier-versions-of-routing"></a>Yönlendirmenin önceki sürümlerinden farklılıklar

ASP.NET Core 2,2 veya üzeri ve daha önceki yönlendirme sürümlerindeki ASP.NET Core uç nokta yönlendirmesi arasında birkaç fark vardır:

* Uç nokta yönlendirme sistemi, <xref:Microsoft.AspNetCore.Routing.IRouter> <xref:Microsoft.AspNetCore.Routing.Route>devralma dahil olmak üzere tabanlı genişletilebilirliği desteklemez.

* Uç nokta yönlendirme [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)desteklemez. Uyumluluk Shim 'yi kullanmaya devam etmek`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`için 2,1 [Uyumluluk sürümünü](xref:mvc/compatibility-version) () kullanın.

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

  Tabanlı <xref:Microsoft.AspNetCore.Routing.IRouter>yönlendirme ile, bu kod, belirtilen rota değerinin büyük `/blog/ReadPost/17`küçük harflerini belirten bir URI oluşturur. ASP.NET Core 2,2 veya üzeri bir nokta yönlendirme ( `/Blog/ReadPost/17` "blog" büyük harfli) üretir. Endpoint Routing, `IOutboundParameterTransformer` bu davranışı genel olarak özelleştirmek veya URL eşleme için farklı kurallar uygulamak üzere kullanılabilecek arabirimi sağlar.

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

  Tabanlı `IRouter`yönlendirme ile, mevcut olmasa veya bir `ReadPost` eylem `/Blog/ReadPost/17`yöntemine sahip olmasalar bile `BlogController` sonuç her zaman olur. Beklendiği gibi, ASP.NET Core 2,2 veya sonraki bir sürümde uç nokta `/Blog/ReadPost/17` yönlendirme eylem yöntemi varsa üretir. *Ancak, uç nokta yönlendirme, eylem yoksa boş bir dize oluşturur.* Kavramsal olarak, uç nokta yönlendirme, eylem mevcut değilse uç noktanın var olduğunu varsaymaz.

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

  URI `/Store/Product/18` ASP.NET Core 2,1 veya daha önceki bir sürümdeyse, tarafından `@Url.Page("/Login")` Store/Info sayfasında oluşturulan bağlantı olur `/Login/18`. 16 `id` değeri, bağlantı hedefi uygulamanın tamamen farklı bir parçası olsa da yeniden kullanılır. Sayfabağlamındaki`/Login` yol değeri büyük olasılıkla bir mağaza ürün kimliği değeri değil, bir kullanıcı kimliği değeridir. `id`

  ASP.NET Core 2,2 veya sonraki bir sürümü ile Endpoint Routing de sonuç olur `/Login`. Bağlantılı hedef farklı bir eylem veya sayfa olduğunda çevresel değerler yeniden kullanılmaz.

* Gidiş dönüşü yol parametresi sözdizimi: Çift yıldız (`**`) catch-all parametre sözdizimi kullanılırken eğik çizgiler kodlanmaz.

  Bağlantı oluşturma sırasında, yönlendirme sistemi, eğik çizgiler hariç, bir çift yıldız (`**`) catch-all parametresinde (örneğin, `{**myparametername}`) yakalanan değeri kodlar. Çift yıldız yakalama, ASP.NET Core 2,2 veya üzeri sürümlerde tabanlı `IRouter`yönlendirme ile desteklenir.

  ASP.NET Core (`{*myparametername}`) öğesinin önceki sürümlerindeki tek yıldız catch-all parametre sözdizimi desteklenmeye devam eder ve eğik çizgi kodlandı.

  | Yolu              | İle oluşturulan bağlantı<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts`(eğik çizgi kodlandı)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ara yazılım örneği

Aşağıdaki örnekte, bir ara yazılım, mağaza ürünlerini <xref:Microsoft.AspNetCore.Routing.LinkGenerator> listeleyen bir eylem yöntemine bağlantı oluşturmak için API 'yi kullanır. Bağlantı Oluşturucuyu bir sınıfa ekleme ve çağırarak `GenerateLink` bir uygulamadaki herhangi bir sınıf için kullanılabilir.

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

Çoğu uygulama, ' de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>tanımlanan benzer uzantı yöntemlerinden birini çağırarak veya arayarak yollar oluşturur. Uzantı yöntemlerinden herhangi biri bir <xref:Microsoft.AspNetCore.Routing.Route> örneği oluşturur ve bunu yol koleksiyonuna ekler. <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yol işleyici parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yalnızca tarafından <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>işlenen rotaları ekler. MVC 'de yönlendirme hakkında daha fazla bilgi için bkz <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, tipik bir ASP.NET Core MVC yol <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> tanımı tarafından kullanılan bir çağrının örneğidir:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yoluyla eşleşir ve yol değerlerini ayıklar. Örneğin, yol `/Products/Details/17` şu yol değerlerini oluşturur: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri, URL yolunun kesimlere bölünüyor ve rota şablonundaki *yol parametresi* adı ile her bir segmenti eşleştirerek belirlenir. Yol parametreleri olarak adlandırılır. Küme ayraçları `{ ... }`içinde parametre adının çevreleme tarafından tanımlanan parametreler.

Yukarıdaki şablon URL yoluyla `/` da eşleştirebilir ve değerleri `{ controller = Home, action = Index }`üretebilir. Bu durum, `{controller}` ve `{action}` rota parametrelerinin varsayılan değerleri olduğu ve `id` Route parametresinin isteğe bağlı olması nedeniyle oluşur. Bir eşittir işareti (`=`), yol parametre adından sonra gelen bir değer, parametre için varsayılan bir değer tanımlar. Yol parametre adından sonra`?`bir soru işareti () isteğe bağlı bir parametre tanımlar.

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

Aşağıdaki örnekte, Route parametresi tanımı `{id:int}` `id` yol parametresi için bir [yol kısıtlaması](#route-constraint-reference) tanımlar:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon, gibi `/Products/Details/17` `/Products/Details/Apples`bir URL yoluyla eşleşir. Yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> , yol değerlerini uygulayıp onları doğrulamak için inceler. Bu örnekte, yol değeri `id` bir tamsayıya dönüştürülebilir olmalıdır. Framework tarafından sunulan yol kısıtlamalarının açıklaması için bkz. [route-Constraint-Reference](#route-constraint-reference) .

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> `constraints`, Ve`defaults`içindeğer kabul etmenin ek aşırı yüklemeleri. `dataTokens` Bu parametrelerin tipik kullanımı, anonim türdeki özellik adlarının yol parametre adlarıyla eşleşen anonim olarak yazılmış bir nesneyi geçirmektir.

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
> Kısıtlamaları ve Varsayılanları tanımlamaya yönelik satır içi sözdizimi basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi senaryolar vardır.

Aşağıdaki örnek birkaç ek senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablon, benzer `/Blog/All-About-Routing/Introduction` bir URL yoluyla eşleşir ve değerleri `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`ayıklar. `controller` Ve`action` için varsayılan yol değerleri, şablonda karşılık gelen hiçbir yol parametresi olmasa bile rota tarafından üretilir. Varsayılan değerler yol şablonunda belirtilebilir. Route parametresi, yol parametre adından önce bir çift yıldız işareti (`**`) görünümüne göre *catch-all* olarak tanımlanır. `article` Catch-all yol parametreleri, URL yolunun kalanını yakalar ve boş dizeyle de aynı olabilir.

Aşağıdaki örnek yol kısıtlamalarını ve veri belirteçlerini ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablon, benzer `/en-US/Products/5` bir URL yoluyla eşleşir ve değerleri `{ controller = Products, action = Details, id = 5 }` ve veri belirteçlerini `{ locale = en-US }`ayıklar.

![Yereller Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Yol sınıfı URL 'SI oluşturma

Sınıf <xref:Microsoft.AspNetCore.Routing.Route> , yol değerlerini bir kümesini rota şablonuyla birleştirerek URL oluşturma işlemi de gerçekleştirebilir. Bu, URL yolunu eşleştirmenin mantıksal bir işlemdir.

> [!TIP]
> URL oluşturmayı daha iyi anlamak için, oluşturmak istediğiniz URL 'YI düşünün ve sonra bir yönlendirme şablonunun bu URL ile nasıl eşleşeceğini düşünün. Hangi değerler üretilemidir? Bu, URL oluşturma işlevinin <xref:Microsoft.AspNetCore.Routing.Route> sınıfında nasıl çalıştığına ilişkin kaba bir eştir.

Aşağıdaki örnek genel ASP.NET Core MVC varsayılan yolunu kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Yol değerleriyle `{ controller = Products, action = List }`, URL `/Products/List` oluşturulur. Yol değerleri, URL yolunu biçimlendirmek için karşılık gelen yol parametrelerinin yerine kullanılır. , İsteğe bağlı bir yol parametresi `id` olduğundan,URL'siiçinbirdeğerolmadanbaşarıylaoluşturulur.`id`

Yol değerleriyle `{ controller = Home, action = Index }`, URL `/` oluşturulur. Belirtilen yol değerleri varsayılan değerlerle eşleşir ve varsayılan değerlere karşılık gelen segmentler güvenle atlanır.

Her iki URL de aşağıdaki yol tanımıyla (`/Home/Index` ve `/`) birlikte gidiş dönüş, URL oluşturmak için kullanılan rota değerlerini oluşturur.

> [!NOTE]
> ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> kullanan bir uygulama, doğrudan yönlendirmeye çağırmak yerine URL 'ler oluşturmak için kullanılmalıdır.

URL oluşturma hakkında daha fazla bilgi için [URL oluşturma başvurusu](#url-generation-reference) bölümüne bakın.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılımı kullanma

Uygulamanın proje dosyasındaki [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) öğesine başvurun.

İçindeki `Startup.ConfigureServices`hizmet kapsayıcısına yönlendirme ekle:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash; Yalnızca http get isteklerini eşleştirir.
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

Framework, yollar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>) oluşturmak için bir genişletme yöntemleri kümesi sağlar:

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

`Map[Verb]` Yöntemler, yöntem adındaki http fiili ile rotayı sınırlandırmak için kısıtlamalar kullanır. Örneğin, bkz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> . ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları (`{ ... }`) içindeki belirteçler, yol eşleştirildiği takdirde bağlanan *Rota parametrelerini* tanımlar. Bir yol segmentinde birden fazla yol parametresi tanımlayabilirsiniz, ancak bunların bir değişmez değer ile ayrılması gerekir. Örneğin, `{controller=Home}{action=Index}` ve `{controller}` arasında`{action}`değişmez değer olmadığından geçerli bir yol değil. Bu rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametrelerinden (örneğin, `{id}`) ve yol ayırıcısından `/` farklı bir metin, URL içindeki metinle eşleşmelidir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Bir sabit yol`{` parametresi sınırlayıcısından (veya `}`) eşleştirmek için, karakteri (`{{` veya `}}`) tekrarlayarak sınırlayıcıdan kaçış.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, şablonu `files/{filename}.{ext?}`göz önünde bulundurun. Hem hem de `filename` `ext` için değerler olduğunda her iki değer de doldurulur. URL 'de yalnızca bir değeri `filename` varsa, sondaki nokta (`.`) isteğe bağlı olduğundan yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

URI 'nin geri kalanına bağlamak için`*`bir yol parametresinin öneki olarak`**`bir yıldız işareti () veya çift yıldız işareti () kullanabilirsiniz. Bunlara *catch-all* parametreleri denir. Örneğin, `blog/{**slug}` ile `/blog` başlayan ve bunu izleyen bir değere sahip olan ve `slug` yol değerine atanan herhangi bir URI ile eşleşir. Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı (`/`) karakterleri de dahil olmak üzere bir URL oluşturmak için, catch-all parametresi uygun karakterleri de çıkar. Örneğin, yol değerlerini `foo/{*path}` `{ path = "my/path" }` içeren yol oluşturulur `foo/my%2Fpath`. Atlanan eğik çizgiye göz önünde edin. Yol ayırıcı karakterlerini yuvarlaklaştırmak için `**` rota parametresi önekini kullanın. İle `foo/{**path}` Rota`{ path = "my/path" }` oluşturulur .`foo/my/path`

Yol parametreleri, parametre adından sonra bir eşittir işaretiyle (`=`) ayırarak varsayılan değer belirtilerek belirlenmiş *varsayılan değerlere* sahip olabilir. Örneğin, `{controller=Home}` için `Home` varsayılan`controller`değer olarak tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, içinde`?` `id?`olduğu gibi parametre adının sonuna bir soru işareti () eklenerek isteğe bağlı olarak yapılır. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark, varsayılan değere sahip bir yol parametresinin her zaman bir değer&mdash;ürettiğinden, isteğe bağlı bir parametre yalnızca istek URL 'si tarafından bir değer sağlandığı zaman bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra`:`bir iki nokta () ve kısıtlama adı eklemek, bir rota parametresinde bir *satır içi kısıtlamayı* belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez (`(...)`) içine alınır. Birden çok satır içi kısıtlama, başka bir iki nokta (`:`) ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılmak <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> üzere bir örneği oluşturmak için hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}` bağımsız değişkenle `10`bir `minlength` kısıtlama belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Yol parametrelerinde Ayrıca, bağlantı oluştururken ve URL 'Ler ile eşleşen eylemler ve sayfalar için bir parametrenin değerini dönüştüren parametre dönüştürücüler bulunabilir. LIKE kısıtlamaları, yol parametre adından sonra iki nokta üst üste (`:`) ve transformatör adı eklenerek, parametre dönüştürücüleri bir rota parametresine eklenebilir. Örneğin, yol şablonu `blog/{article:slugify}` bir `slugify` transformatör belirtir. Parametre dönüştürücüler hakkında daha fazla bilgi için bkz. [Parameter transformatör başvurusu](#parameter-transformer-reference) bölümü.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir.

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca tek bir yolla `/hello`eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | İle eşleşir ve `Page` ayarlanır `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | İle eşleşir ve `Page` ayarlanır `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` Denetleyiciye ve`List` eyleme eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Denetleyici ve `Details` eyleme eşlenir (`id` 123 olarak ayarlanır). `Products` |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Denetleyiciye ve `Index` yönteme eşlenir (`id` yok sayılır). `Home`        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

> [!TIP]
> İstekleri eşleme gibi yerleşik yönlendirme uygulamalarının <xref:Microsoft.AspNetCore.Routing.Route>nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

## <a name="reserved-routing-names"></a>Ayrılmış yönlendirme adları

Aşağıdaki anahtar sözcükler ayrılmış isimlerdir ve yol adları veya parametreler olarak kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Rota kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda bir Evet/Hayır kararı getirir. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> bir isteği http fiiline bağlı olarak kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> **Giriş doğrulaması**için kısıtlamaları kullanmayın. **Giriş doğrulaması**için kısıtlamalar kullanılıyorsa, doğru bir hata iletisine sahip *400-Bad isteği* yerine *404-* olmayan bir Yanıt ile geçersiz giriş oluşur. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların **belirsizliğini ortadan** kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve bunların beklenen davranışları gösterilmektedir.

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | Eşleşiyor `true` veya`false` (büyük/küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli `DateTime` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli `decimal` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli `double` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli `float` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli `Guid` bir değerle eşleşir |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli `long` bir değerle eşleşir |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize 8 karakterden uzun olmamalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 ve en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri 120 ' ten fazla olmamalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 olmalı ancak 120 ' ten fazla olmamalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır (`a`-`z`büyük/küçük harfe duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir (normal ifade tanımlama hakkında ipuçlarına bakın) |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır |

Birden çok, iki nokta üst üste sınırlı kısıtlama tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'yi doğrulayan ve bir clr türüne ( `int` veya `DateTime`gibi) dönüştürülen yol kısıtlamaları, her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlama yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ifade oluşturucusuna ekler. Bu <xref:System.Text.RegularExpressions.RegexOptions> üyelerin açıklaması için bkz.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. Yönlendirmenin normal ifadesini `^\d{3}-\d{2}-\d{4}$` kullanmak için, ifade `\` , C# kaynak dosyadaki dize olarak (çift ters eğik çizgi) karakter olarak `\\` belirtilmelidir ve `\` dize kaçış karakteri (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `]` ,)atlamak`]]`için, ifadedeki karakterleri (`{{` ,,,)çift.`}` `[[` `[` Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler, genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` Ve`$` karakterleri, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` Ve`$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | Herkese     | Hayır    | Bkz `^` . `$` ve üzeri |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Bkz `^` . `$` ve üzeri |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. `{action:regex(^(list|get|create)$)}` Örneğin, yalnızca `action` rota değeri `list`, `get`, veya`create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğüne (bir şablon içinde satır içi değil) geçirilen kısıtlamalar da normal ifadeler olarak kabul edilir.

## <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Yerleşik yol kısıtlamalarına ek olarak, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak özel yol kısıtlamaları oluşturulabilir. Arabirim, kısıtlama `Match` `true` karşılanıp`false` Aksi takdirde döndüren tek bir yöntemi içerir. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>

Özel <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir kullanmak için yol kısıtlama türü, uygulamanın uygulamanın hizmet kapsayıcısında kayıtlı <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> olması gerekir. , Yol kısıtlama <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> anahtarlarını bu kısıtlamaları doğrulayan uygulamalarla eşleyen bir sözlüktür. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> Bir uygulama <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> , hizmetlerin bir parçası olarak `Startup.ConfigureServices` ' de güncelleştirilir [. Addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya doğrudan ile <xref:Microsoft.AspNetCore.Routing.RouteOptions> `services.Configure<RouteOptions>`yapılandırma. Örneğin:

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

* Bir <xref:Microsoft.AspNetCore.Routing.Route>bağlantısını oluştururken yürütün.
* Uygulayın `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Kullanılarak <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>yapılandırılır.
* Parametrenin yol değerini alın ve yeni bir dize değerine dönüştürün.
* Oluşturulan bağlantıda Dönüştürülmüş değerin kullanılmasına neden olacak.

Örneğin, `slugify` `blog\my-test-article`yol `blog\{article:slugify}` düzenindebirözel`Url.Action(new { article = "MyTestArticle" })` parametre transformatörü oluşturur.

Bir parametre transformatörü bir yol düzeninde kullanmak için, önce ' de <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> `Startup.ConfigureServices`kullanarak bunu yapılandırın:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Parametre dönüştürücüler, bir uç noktanın çözümlediği URI 'yi dönüştürmek için çerçevesi tarafından kullanılır. `area`Örneğin, ASP.NET Core MVC `controller` `action`,,, ve `page`ile eşleşecek şekilde kullanılan rota değerini dönüştürmek için parametre dönüştürücüler kullanır.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Önceki yol ile, eylem `SubscriptionManagementController.GetAll()` URI `/subscription-management/get-all`ile eşleştirilir. Bir parametre transformatörü bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. Örneğin, `Url.Action("GetAll", "SubscriptionManagement")` çıktılar `/subscription-management/get-all`.

ASP.NET Core, oluşturulan yollarla bir parametre dönüştürücüler kullanmak için API kuralları sağlar:

* ASP.NET Core MVC 'nin `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı vardır. Bu kural, uygulamadaki tüm öznitelik yollarına belirtilen bir parametre transformatörü uygular. Parametre transformatörü, öznitelik yol belirteçlerini değiştirildiklerinde dönüştürür. Daha fazla bilgi için bkz. [belirteç değişimini özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API kuralına sahiptir. Bu kural, belirtilen bir parametre transformatörü otomatik olarak keşfedilen Razor Pages uygular. Parametre transformatörü Razor Pages yolların klasör ve dosya adı segmentlerini dönüştürür. Daha fazla bilgi için bkz. [sayfa yollarını özelleştirmek için bir parametre transformatörü kullanma](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, yol değerlerinin ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>sözlüğünün bir sözlüğüne verilen bir yolun bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Önceki örnek sonunda <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>oluşturulan. `/package/create/123` Sözlük, " `operation` paketyolunu`id` izle" şablonunun ve rota değerlerini sağlar. `package/{operation}/{id}` Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> Oluşturucunun ikinci parametresi bir *ortam değerleri*koleksiyonudur. Ortam değerleri, bir geliştiricinin bir istek bağlamı içinde belirtmesi gereken değer sayısını sınırlandırdığından kullanım için uygundur. Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir. ASP.NET Core `About` MVC uygulamasının `Index` &mdash;öğesinin `HomeController`eyleminde, ortam değeri`Home` kullanılan eyleme bağlamak için denetleyici yol değerini belirtmeniz gerekmez.

Bir parametreyle eşleşmeyen çevresel değerler yok sayılır. Ayrıca, açıkça sağlanmış bir değer çevresel değeri geçersiz kıldığında çevresel değerler de yoksayılır. Eşleştirme, URL 'de soldan sağa doğru gerçekleşir.

Açık olarak sağlanmış ancak yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda, yol şablonu `{controller}/{action}/{id?}`kullanılırken sonuç gösterilmektedir.

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

Bağlantı oluşturma yalnızca ve `controller` `action` için eşleşen değerler sağlandığında bu yol için bir bağlantı oluşturur.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin `[Route("/x{token}y")]`), sabit değerli olmayan değişmez değer ile sağdan sola eşleştirilirken işlenir. Karmaşık segmentlerin nasıl eşleştirileceği hakkında ayrıntılı bir açıklama için [bu koda](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) bakın. [Kod örneği](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler hakkında iyi bir açıklama sağlar.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Yönlendirme, istek URI 'Lerini rota işleyicileriyle eşleştirmekten ve gelen istekleri gönderen sorumludur. Yollar uygulamada tanımlanır ve uygulama başlatıldığında yapılandırılır. Yol, isteğe bağlı olarak istekte bulunan URL 'den değerleri ayıklayabilir ve bu değerler, istek işleme için kullanılabilir. Uygulamadan yapılandırılan yollar kullanıldığında, yönlendirme, yol işleyicileriyle eşlenen URL 'Ler oluşturabilir.

ASP.NET Core 2,1 ' de en son yönlendirme senaryolarını kullanmak için, ' de `Startup.ConfigureServices`MVC Hizmetleri kaydının [Uyumluluk sürümünü](xref:mvc/compatibility-version) belirtin:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirmeyi içerir. ASP.NET Core MVC yönlendirme hakkında bilgi için bkz <xref:mvc/controllers/routing>. Razor Pages 'de yönlendirme kuralları hakkında bilgi için bkz <xref:razor-pages/razor-pages-conventions>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme temelleri

Çoğu uygulama, URL 'Lerin okunabilir ve anlamlı olması için temel ve açıklayıcı bir yönlendirme şeması seçmelidir. Varsayılan geleneksel yol `{controller=Home}/{action=Index}/{id?}`:

* Temel ve açıklayıcı bir yönlendirme düzenini destekler.
* , UI tabanlı uygulamalar için kullanışlı bir başlangıç noktasıdır.

Geliştiriciler, [öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing) veya adanmış geleneksel yollar kullanarak özel durumlarda (örneğin, blog ve e-ticaret uç noktaları) bir uygulamanın yüksek trafik bölümlerine daha fazla terse yolu ekler.

Web API 'Leri, uygulamanın işlevselliğini HTTP fiilleri tarafından temsil edilen bir kaynak kümesi olarak modellemek için öznitelik yönlendirmeyi kullanmalıdır. Bu, aynı mantıksal kaynaktaki birçok işlemin (örneğin, GET, POST) aynı URL 'YI kullanacağı anlamına gelir. Öznitelik yönlendirme, bir API 'nin Genel uç nokta yerleşimini dikkatle tasarlamak için gereken bir denetim düzeyi sağlar.

Razor Pages uygulamalar, bir uygulamanın *Sayfalar* klasöründe adlandırılmış kaynaklara hizmeti sağlamak için varsayılan geleneksel yönlendirmeyi kullanır. Razor Pages yönlendirme davranışını özelleştirmenizi sağlayan ek kurallar mevcuttur. Daha fazla bilgi için bkz. <xref:razor-pages/index> ve <xref:razor-pages/razor-pages-conventions>.

URL oluşturma desteği, uygulamanın, uygulamayı birbirine bağlamak için sabit kodlama URL 'Leri olmadan geliştirilebilmesine izin verir. Bu destek, temel bir yönlendirme yapılandırmasıyla başlayıp uygulamanın kaynak düzeni belirlendikten sonra yolların değiştirilmesini sağlar.

Yönlendirme *yolları* (uygulamaları <xref:Microsoft.AspNetCore.Routing.IRouter>) şu amaçlarla kullanılır:

* Gelen istekleri *Rota işleyicilerine*eşleyin.
* Yanıtlarda kullanılan URL 'Leri oluşturun.

Varsayılan olarak, bir uygulama tek bir yollar koleksiyonuna sahiptir. Bir istek geldiğinde koleksiyondaki yollar, koleksiyonda var olan sırada işlenir. Çerçeve koleksiyondaki her bir rotada yöntemini çağırarak, <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> bir gelen istek URL 'sini koleksiyondaki bir yola eşleştirmeye çalışır. Yanıt, yönlendirme bilgilerine göre URL (örneğin, yeniden yönlendirme veya bağlantılar için) oluşturmak için yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.

Yönlendirme sistemi aşağıdaki özelliklere sahiptir:

* Yol şablonu sözdizimi, simgeleştirilmiş yol parametrelerine sahip yolları tanımlamak için kullanılır.
* Geleneksel stil ve öznitelik stili uç nokta yapılandırmasına izin verilir.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir URL parametresinin belirli bir uç nokta kısıtlaması için geçerli bir değer içerip içermediğini belirlemekte kullanılır.
* MVC/Razor Pages gibi uygulama modelleri, yönlendirme senaryolarının öngörülebilir bir uygulaması olan tüm yollarını kaydeder.
* Yanıt, yönlendirme bilgilerine göre URL (örneğin, yeniden yönlendirme veya bağlantılar için) oluşturmak için yönlendirmeyi kullanabilir ve bu sayede bakım yapılmasına yardımcı olan sabit kodlanmış URL 'Lerden kaçınabilirsiniz.
* URL oluşturma, rastgele genişletilebilirliği destekleyen yollara dayalıdır. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>URL 'Ler oluşturmak için yöntemler sunar.

Yönlendirme, sınıfı tarafından <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> bulunan [Ara yazılım](xref:fundamentals/middleware/index) ardışık düzenine bağlıdır. [ASP.NET Core MVC](xref:mvc/overview) , yapılandırma kapsamında bir ara yazılım ardışık düzenine yönlendirme ekler ve MVC ve Razor Pages uygulamalarında yönlendirmeyi işler. Tek başına bileşen olarak yönlendirmeyi nasıl kullanacağınızı öğrenmek için [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümüne bakın.

### <a name="url-matching"></a>URL eşleştirme

URL eşleştirme, yönlendirmenin bir *işleyiciye*gelen istek gönderdiğine yönelik işlemdir. Bu işlem, URL yolundaki verileri temel alır, ancak istekteki verileri göz önünde bulundurmanız için genişletilebilir. Ayrı işleyicilere istek gönderme özelliği, bir uygulamanın boyutunu ve karmaşıklığını ölçeklendirmeye yönelik anahtardır.

Gelen istekler, sırayla <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>her bir yoldaki <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemi çağıran öğesini girer. Örnek, [routecontext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) değerini null <xref:Microsoft.AspNetCore.Http.RequestDelegate>olmayan olarak ayarlayarak isteğin işleneceğini seçer. <xref:Microsoft.AspNetCore.Routing.IRouter> Bir yol istek için bir işleyici ayarlarsa, yol işleme duraklar ve işleyici isteği işlemek için çağrılır. İsteği işlemek için yol işleyicisi bulunmazsa, ara yazılım istek ardışık düzeninde sonraki bir ara yazılım için isteği kapatır.

' Ye <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> birincil giriş, geçerli istekle ilişkili [routecontext. HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) ' dir. [Routecontext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) ve [Routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) , bir yol eşleştirdikten sonra çıkış olarak ayarlanır.

Çağıran <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> bir eşleşme de [routecontext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) özelliklerinin özelliklerini, bu nedenle yapılan istek işlemeye bağlı olarak uygun değerlere ayarlar.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) , rotada oluşturulan *yol değerlerinin* bir sözlüğüdür. Bu değerler genellikle URL 'YI simgeleştirerek belirlenir ve Kullanıcı girişini kabul etmek ya da uygulama içinde daha fazla kararlar almak için kullanılabilir.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) , eşleşen rotayla ilgili ek verilerin bir özellik çantadır. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*>Her rotayla durum verilerinin ilişkilendirilmesini desteklemek için sağlanır, böylece uygulama hangi yolun eşleştiğini temel alarak kararlar alabilir. Bu değerler, geliştirici tarafından tanımlanır ve yönlendirme davranışını herhangi bir **şekilde etkilemez.** Ayrıca, [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) içinde bulunan değerler her türlü türden olabilir. Bu, [veri](xref:Microsoft.AspNetCore.Routing.RouteData.Values)dizeleri arasında dönüştürülebilir olmalıdır.

[RouteData. yönlendiriciler](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) , isteği başarıyla eşleştirirken geçen yolların bir listesidir. Yollar bir diğerinin içinde iç içe olabilir. <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> Özelliği, bir eşleşme ile sonuçlanan yolların mantıksal ağacı aracılığıyla yolu yansıtır. Genellikle, içindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> ilk öğe yol koleksiyonudur ve URL oluşturma için kullanılmalıdır. İçindeki <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> son öğe, eşleşen yol işleyicisidir.

<a name="lg"></a>

### <a name="url-generation"></a>URL oluşturma

URL oluşturma, yönlendirmenin bir yol değerleri kümesine göre bir URL yolu oluşturabileceği işlemdir. Bu, yönlendirme işleyicileri ve bunlara erişen URL 'Ler arasındaki mantıksal bir ayrım sağlar.

URL oluşturma benzer bir yinelemeli işlem izler, ancak yol koleksiyonu <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> metoduna çağıran kullanıcı veya çerçeve kodu ile başlar. Her *yol* <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> , null olmayan bir değer <xref:Microsoft.AspNetCore.Routing.VirtualPathData> döndürülene kadar metodu sırayla çağırılır.

Birincil girdileri <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> :

* [VirtualPathContext. HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext. Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext. AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Yollar birincil olarak, tarafından <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> belirtilen yol değerlerini kullanır ve bir URL oluşturma ve hangi değerlerin dahil edileceğini belirlemek için. , <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> Geçerli istekle eşleştirmeden üretilmiş olan rota değerleri kümesidir. Buna karşılık, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> geçerli işlem için istenen URL 'nin nasıl oluşturulacağını belirten rota değerlerdir. , <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> Bir yolun geçerli bağlamla ilişkili hizmetleri veya ek verileri alması durumunda sağlanır.

> [!TIP]
> Virtualpathcontext [. Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) öğesini [Virtualpathcontext. AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)için bir geçersiz kılma kümesi olarak düşünün. URL oluşturma, aynı rota veya yol değerlerini kullanan bağlantılar için URL 'Ler oluşturmak üzere geçerli istekten yol değerlerini yeniden kullanmayı dener.

Çıkışı <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> bir <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData>, öğesinin <xref:Microsoft.AspNetCore.Routing.RouteData>bir paraleldir. <xref:Microsoft.AspNetCore.Routing.VirtualPathData>çıkış URL 'si için ve yol tarafından ayarlanması gereken bazı ek özellikler içerir. <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>

[VirtualPathData. VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) özelliği, yol tarafından üretilen *sanal yolu* içerir. Gereksinimlerinize bağlı olarak, yolu daha fazla işlem yapmanız gerekebilir. Oluşturulan URL 'YI HTML 'de işlemek istiyorsanız, uygulamanın temel yolunu ekleyin.

[VirtualPathData. Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) , URL 'yi başarıyla oluşturmuş olan yola bir başvurudur.

[VirtualPathData. DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) özellikleri, URL 'yi oluşturan rotayla ilgili ek verilerin bir sözlüğüdür. Bu, [RouteData. Datatoken](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*)'ların paraleldir.

### <a name="create-routes"></a>Rotalar oluştur

Yönlendirme <xref:Microsoft.AspNetCore.Routing.Route> sınıfı sınıfının standart <xref:Microsoft.AspNetCore.Routing.IRouter>uygulamasını sağlar. <xref:Microsoft.AspNetCore.Routing.Route>çağrıldığında URL yoluyla <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> eşleştirilecek desenleri tanımlamak için *yol şablonu* sözdizimini kullanır. <xref:Microsoft.AspNetCore.Routing.Route>çağrıldığında bir URL <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> oluşturmak için aynı rota şablonunu kullanır.

Çoğu uygulama, ' de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>tanımlanan benzer uzantı yöntemlerinden birini çağırarak veya arayarak yollar oluşturur. Uzantı yöntemlerinden herhangi biri bir <xref:Microsoft.AspNetCore.Routing.Route> örneği oluşturur ve bunu yol koleksiyonuna ekler. <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yol işleyici parametresini kabul etmez. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*>yalnızca tarafından <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>işlenen rotaları ekler. Varsayılan işleyici bir `IRouter`' dir ve işleyici isteği işleyemeyebilir. Örneğin, ASP.NET Core MVC genellikle yalnızca kullanılabilir bir denetleyici ve eylemle eşleşen istekleri işleyen varsayılan bir işleyici olarak yapılandırılır. MVC 'de yönlendirme hakkında daha fazla bilgi için bkz <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, tipik bir ASP.NET Core MVC yol <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> tanımı tarafından kullanılan bir çağrının örneğidir:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yoluyla eşleşir ve yol değerlerini ayıklar. Örneğin, yol `/Products/Details/17` şu yol değerlerini oluşturur: `{ controller = Products, action = Details, id = 17 }`.

Rota değerleri, URL yolunun kesimlere bölünüyor ve rota şablonundaki *yol parametresi* adı ile her bir segmenti eşleştirerek belirlenir. Yol parametreleri olarak adlandırılır. Küme ayraçları `{ ... }`içinde parametre adının çevreleme tarafından tanımlanan parametreler.

Yukarıdaki şablon URL yoluyla `/` da eşleştirebilir ve değerleri `{ controller = Home, action = Index }`üretebilir. Bu durum, `{controller}` ve `{action}` rota parametrelerinin varsayılan değerleri olduğu ve `id` Route parametresinin isteğe bağlı olması nedeniyle oluşur. Bir eşittir işareti (`=`), yol parametre adından sonra gelen bir değer, parametre için varsayılan bir değer tanımlar. Yol parametre adından sonra`?`bir soru işareti () isteğe bağlı bir parametre tanımlar.

Yol parametreleri varsayılan değer ile *her zaman* rota değeri oluşturur. Karşılık gelen bir URL yol kesimi yoksa isteğe bağlı parametreler yol değeri oluşturmaz. Yol şablonu senaryolarının ve sözdiziminin kapsamlı bir açıklaması için [yol şablonu başvurusu](#route-template-reference) bölümüne bakın.

Aşağıdaki örnekte, Route parametresi tanımı `{id:int}` `id` yol parametresi için bir [yol kısıtlaması](#route-constraint-reference) tanımlar:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon, gibi `/Products/Details/17` `/Products/Details/Apples`bir URL yoluyla eşleşir. Yol kısıtlamaları <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> , yol değerlerini uygulayıp onları doğrulamak için inceler. Bu örnekte, yol değeri `id` bir tamsayıya dönüştürülebilir olmalıdır. Framework tarafından sunulan yol kısıtlamalarının açıklaması için bkz. [route-Constraint-Reference](#route-constraint-reference) .

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> `constraints`, Ve`defaults`içindeğer kabul etmenin ek aşırı yüklemeleri. `dataTokens` Bu parametrelerin tipik kullanımı, anonim türdeki özellik adlarının yol parametre adlarıyla eşleşen anonim olarak yazılmış bir nesneyi geçirmektir.

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
> Kısıtlamaları ve Varsayılanları tanımlamaya yönelik satır içi sözdizimi basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi senaryolar vardır.

Aşağıdaki örnek birkaç ek senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Önceki şablon, benzer `/Blog/All-About-Routing/Introduction` bir URL yoluyla eşleşir ve değerleri `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`ayıklar. `controller` Ve`action` için varsayılan yol değerleri, şablonda karşılık gelen hiçbir yol parametresi olmasa bile rota tarafından üretilir. Varsayılan değerler yol şablonunda belirtilebilir. Route parametresi, yol parametre adından önce bir yıldız işareti (`*`) görünümüne göre *catch-all* olarak tanımlanır. `article` Catch-all yol parametreleri, URL yolunun kalanını yakalar ve boş dizeyle de aynı olabilir.

Aşağıdaki örnek yol kısıtlamalarını ve veri belirteçlerini ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Önceki şablon, benzer `/en-US/Products/5` bir URL yoluyla eşleşir ve değerleri `{ controller = Products, action = Details, id = 5 }` ve veri belirteçlerini `{ locale = en-US }`ayıklar.

![Yereller Windows belirteçleri](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Yol sınıfı URL 'SI oluşturma

Sınıf <xref:Microsoft.AspNetCore.Routing.Route> , yol değerlerini bir kümesini rota şablonuyla birleştirerek URL oluşturma işlemi de gerçekleştirebilir. Bu, URL yolunu eşleştirmenin mantıksal bir işlemdir.

> [!TIP]
> URL oluşturmayı daha iyi anlamak için, oluşturmak istediğiniz URL 'YI düşünün ve sonra bir yönlendirme şablonunun bu URL ile nasıl eşleşeceğini düşünün. Hangi değerler üretilemidir? Bu, URL oluşturma işlevinin <xref:Microsoft.AspNetCore.Routing.Route> sınıfında nasıl çalıştığına ilişkin kaba bir eştir.

Aşağıdaki örnek genel ASP.NET Core MVC varsayılan yolunu kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Yol değerleriyle `{ controller = Products, action = List }`, URL `/Products/List` oluşturulur. Yol değerleri, URL yolunu biçimlendirmek için karşılık gelen yol parametrelerinin yerine kullanılır. , İsteğe bağlı bir yol parametresi `id` olduğundan,URL'siiçinbirdeğerolmadanbaşarıylaoluşturulur.`id`

Yol değerleriyle `{ controller = Home, action = Index }`, URL `/` oluşturulur. Belirtilen yol değerleri varsayılan değerlerle eşleşir ve varsayılan değerlere karşılık gelen segmentler güvenle atlanır.

Her iki URL de aşağıdaki yol tanımıyla (`/Home/Index` ve `/`) birlikte gidiş dönüş, URL oluşturmak için kullanılan rota değerlerini oluşturur.

> [!NOTE]
> ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> kullanan bir uygulama, doğrudan yönlendirmeye çağırmak yerine URL 'ler oluşturmak için kullanılmalıdır.

URL oluşturma hakkında daha fazla bilgi için [URL oluşturma başvurusu](#url-generation-reference) bölümüne bakın.

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılımı kullanma

Uygulamanın proje dosyasındaki [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) öğesine başvurun.

İçindeki `Startup.ConfigureServices`hizmet kapsayıcısına yönlendirme ekle:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Yolların `Startup.Configure` yönteminde yapılandırılması gerekir. Örnek uygulama aşağıdaki API 'Leri kullanır:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash; Yalnızca http get isteklerini eşleştirir.
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

Tek bir yol yapılandırıyorsanız, bir <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> `IRouter` örneği geçirme çağrısı yapın. Kullanmanız <xref:Microsoft.AspNetCore.Routing.RouteBuilder>gerekmez.

Framework, yollar (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>) oluşturmak için bir genişletme yöntemleri kümesi sağlar:

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

Gibi listelenen yöntemlerin <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>bazıları için <xref:Microsoft.AspNetCore.Http.RequestDelegate>gerekir. Yol eşleştiğinde yol işleyicisi olarak kullanılır. <xref:Microsoft.AspNetCore.Http.RequestDelegate> Bu ailedeki diğer yöntemler, yönlendirme işleyicisi olarak kullanılmak üzere bir ara yazılım ardışık düzeni yapılandırmaya olanak tanır. Yöntemi gibi bir işleyiciyi <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>kabul etmez, öğesini kullanır <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. `Map*`

`Map[Verb]` Yöntemler, yöntem adındaki http fiili ile rotayı sınırlandırmak için kısıtlamalar kullanır. Örneğin, bkz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> . ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları (`{ ... }`) içindeki belirteçler, yol eşleştirildiği takdirde bağlanan *Rota parametrelerini* tanımlar. Bir yol segmentinde birden fazla yol parametresi tanımlayabilirsiniz, ancak bunların bir değişmez değer ile ayrılması gerekir. Örneğin, `{controller=Home}{action=Index}` ve `{controller}` arasında`{action}`değişmez değer olmadığından geçerli bir yol değil. Bu rota parametrelerinin bir adı olmalı ve ek öznitelikler belirtilmiş olabilir.

Yol parametrelerinden (örneğin, `{id}`) ve yol ayırıcısından `/` farklı bir metin, URL içindeki metinle eşleşmelidir. Metin eşleştirme, büyük/küçük harfe duyarsızdır ve URL yolunun kodu çözülmüş gösterimine göre yapılır. Bir sabit yol`{` parametresi sınırlayıcısından (veya `}`) eşleştirmek için, karakteri (`{{` veya `}}`) tekrarlayarak sınırlayıcıdan kaçış.

İsteğe bağlı bir dosya uzantısına sahip bir dosya adı yakalamaya deneyen URL desenlerinin ek konuları vardır. Örneğin, şablonu `files/{filename}.{ext?}`göz önünde bulundurun. Hem hem de `filename` `ext` için değerler olduğunda her iki değer de doldurulur. URL 'de yalnızca bir değeri `filename` varsa, sondaki nokta (`.`) isteğe bağlı olduğundan yol eşleşir. Aşağıdaki URL 'Ler bu rota ile eşleşiyor:

* `/files/myFile.txt`
* `/files/myFile`

URI 'nin geri kalanına bağlamak için`*`bir yol parametresinin öneki olarak yıldız işareti () kullanabilirsiniz. Buna *catch-all* parametresi denir. Örneğin, `blog/{*slug}` ile `/blog` başlayan ve bunu izleyen bir değere sahip olan ve `slug` yol değerine atanan herhangi bir URI ile eşleşir. Catch-all parametreleri boş dizeyle de aynı olabilir.

Yol ayırıcısı (`/`) karakterleri de dahil olmak üzere bir URL oluşturmak için, catch-all parametresi uygun karakterleri de çıkar. Örneğin, yol değerlerini `foo/{*path}` `{ path = "my/path" }` içeren yol oluşturulur `foo/my%2Fpath`. Atlanan eğik çizgiye göz önünde edin.

Yol parametreleri, parametre adından sonra bir eşittir işaretiyle (`=`) ayırarak varsayılan değer belirtilerek belirlenmiş *varsayılan değerlere* sahip olabilir. Örneğin, `{controller=Home}` için `Home` varsayılan`controller`değer olarak tanımlar. Parametresi için URL 'de hiçbir değer yoksa varsayılan değer kullanılır. Yol parametreleri, içinde`?` `id?`olduğu gibi parametre adının sonuna bir soru işareti () eklenerek isteğe bağlı olarak yapılır. İsteğe bağlı değerler ve varsayılan yol parametreleri arasındaki fark, varsayılan değere sahip bir yol parametresinin her zaman bir değer&mdash;ürettiğinden, isteğe bağlı bir parametre yalnızca istek URL 'si tarafından bir değer sağlandığı zaman bir değere sahip olur.

Rota parametrelerinin URL 'den bağlanan rota değeriyle eşleşmesi gereken kısıtlamaları olabilir. Yol parametre adından sonra`:`bir iki nokta () ve kısıtlama adı eklemek, bir rota parametresinde bir *satır içi kısıtlamayı* belirtir. Kısıtlama bağımsız değişkenler gerektiriyorsa, kısıtlama adından sonra parantez (`(...)`) içine alınır. Birden çok satır içi kısıtlama, başka bir iki nokta (`:`) ve kısıtlama adı eklenerek belirtilebilir.

Kısıtlama adı ve bağımsız değişkenler, URL işlemede kullanılmak <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> üzere bir örneği oluşturmak için hizmetine geçirilir. Örneğin, yol şablonu `blog/{article:minlength(10)}` bağımsız değişkenle `10`bir `minlength` kısıtlama belirtir. Yol kısıtlamaları ve Framework tarafından sunulan kısıtlamaların bir listesi hakkında daha fazla bilgi için, [route kısıtlama başvurusu](#route-constraint-reference) bölümüne bakın.

Aşağıdaki tabloda örnek yol şablonları ve bunların davranışları gösterilmektedir.

| Rota şablonu                           | Örnek eşleşen URI    | İstek URI 'SI&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Yalnızca tek bir yolla `/hello`eşleşir.                                     |
| `{Page=Home}`                            | `/`                     | İle eşleşir ve `Page` ayarlanır `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | İle eşleşir ve `Page` ayarlanır `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | `Products` Denetleyiciye ve`List` eyleme eşlenir.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Denetleyici ve `Details` eyleme eşlenir (`id` 123 olarak ayarlanır). `Products` |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Denetleyiciye ve `Index` yönteme eşlenir (`id` yok sayılır). `Home`        |

Bir şablon kullanmak genellikle yönlendirmeye en basit yaklaşımdır. Kısıtlamalar ve varsayılanlar, yol şablonu dışında da belirtilebilir.

> [!TIP]
> İstekleri eşleme gibi yerleşik yönlendirme uygulamalarının <xref:Microsoft.AspNetCore.Routing.Route>nasıl yapıldığını görmek için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

## <a name="reserved-routing-names"></a>Ayrılmış yönlendirme adları

Aşağıdaki anahtar sözcükler ayrılmış isimlerdir ve yol adları veya parametreler olarak kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Yol kısıtlama başvurusu

Yol kısıtlamaları, gelen URL 'de bir eşleşme meydana geldiğinde ve URL yolu yol değerlerinde simgeleştirilir yürütülür. Rota kısıtlamaları genellikle yol şablonu aracılığıyla ilişkili rota değerini inceler ve değerin kabul edilebilir olup olmadığı konusunda bir Evet/Hayır kararı getirir. Bazı rota kısıtlamaları, isteğin yönlendirilip yönlendirilmeyeceğini göz önünde bulundurmanız için yol değeri dışındaki verileri kullanır. Örneğin, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> bir isteği http fiiline bağlı olarak kabul edebilir veya reddedebilir. Kısıtlamalar, yönlendirme isteklerinde ve bağlantı oluşturmada kullanılır.

> [!WARNING]
> **Giriş doğrulaması**için kısıtlamaları kullanmayın. **Giriş doğrulaması**için kısıtlamalar kullanılıyorsa, doğru bir hata iletisine sahip *400-Bad isteği* yerine *404-* olmayan bir Yanıt ile geçersiz giriş oluşur. Yol kısıtlamaları, belirli bir rota için girdileri doğrulamak üzere değil, benzer yolların **belirsizliğini ortadan** kaldırmak için kullanılır.

Aşağıdaki tabloda örnek yol kısıtlamaları ve bunların beklenen davranışları gösterilmektedir.

| kısıtlama | Örnek | Örnek eşleşmeler | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Herhangi bir tamsayıyla eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | Eşleşiyor `true` veya`false` (büyük/küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Geçerli `DateTime` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli `decimal` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli `double` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli `float` bir değerle eşleşir (Sabit kültür içinde, bkz. uyarı) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli `Guid` bir değerle eşleşir |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli `long` bir değerle eşleşir |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize 8 karakterden uzun olmamalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8 ve en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | Tamsayı değeri en az 18 olmalıdır |
| `max(value)` | `{age:max(120)}` | `91` | Tamsayı değeri 120 ' ten fazla olmamalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Tamsayı değeri en az 18 olmalı ancak 120 ' ten fazla olmamalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize bir veya daha fazla alfabetik karakterden oluşmalıdır (`a`-`z`büyük/küçük harfe duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmelidir (normal ifade tanımlama hakkında ipuçlarına bakın) |
| `required` | `{name:required}` | `Rick` | URL oluşturma sırasında parametre olmayan bir değerin mevcut olduğunu zorlamak için kullanılır |

Birden çok, iki nokta üst üste sınırlı kısıtlama tek bir parametreye uygulanabilir. Örneğin, aşağıdaki kısıtlama bir parametreyi 1 veya daha büyük bir tamsayı değeriyle kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> URL 'yi doğrulayan ve bir clr türüne ( `int` veya `DateTime`gibi) dönüştürülen yol kısıtlamaları, her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar. Framework tarafından sunulan yol kısıtlamaları, yol değerlerinde depolanan değerleri değiştirmez. URL 'den Ayrıştırılan tüm rota değerleri dizeler olarak depolanır. Örneğin, `float` kısıtlama yol değerini bir float öğesine dönüştürmeye çalışır, ancak dönüştürülen değer yalnızca bir float öğesine dönüştürülebileceğini doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core Framework, normal `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ifade oluşturucusuna ekler. Bu <xref:System.Text.RegularExpressions.RegexOptions> üyelerin açıklaması için bkz.

Normal ifadeler, C# yönlendirme ve dil tarafından kullanılanlarla aynı sınırlayıcıları ve belirteçleri kullanır. Normal ifade belirteçlerinin atlanmalıdır. Yönlendirmenin normal ifadesini `^\d{3}-\d{2}-\d{4}$` kullanmak için, ifade `\` , C# kaynak dosyadaki dize olarak (çift ters eğik çizgi) karakter olarak `\\` belirtilmelidir ve `\` dize kaçış karakteri (tam [dize sabit değerleri](/dotnet/csharp/language-reference/keywords/string)kullanmadıkça). Yönlendirme parametresi sınırlayıcı karakterlerini (`{`, `}`, `]` ,)atlamak`]]`için, ifadedeki karakterleri (`{{` ,,,)çift.`}` `[[` `[` Aşağıdaki tabloda, bir normal ifade ve kaçan sürümü gösterilmektedir.

| Normal ifade    | Kaçan normal Ifade     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Yönlendirmelerde kullanılan normal ifadeler, genellikle şapka işareti (`^`) karakteriyle başlar ve dizenin başlangıç konumuyla eşleşir. İfadeler, genellikle dolar işareti (`$`) karakteriyle biter ve dizenin sonuyla eşleşir. `^` Ve`$` karakterleri, normal ifadenin tüm yol parametresi değeri ile eşleştiğinden emin olun. `^` Ve`$` karakterleri olmadan normal ifade, dize içindeki herhangi bir alt dizeden eşleşir ve bu genellikle istenmeyen bir ifadedir. Aşağıdaki tabloda örnekler verilmektedir ve bunların eşleşmesinin neden eşleşmediği veya eşleşmemesi açıklanmaktadır.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Herkese     | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | Alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | Eşleşen ifadesi    |
| `[a-z]{2}`   | MZ        | Evet   | Büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` | Herkese     | Hayır    | Bkz `^` . `$` ve üzeri |
| `^[a-z]{2}$` | 123abc456 | Hayır    | Bkz `^` . `$` ve üzeri |

Normal ifade sözdizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadeler](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Bir parametreyi bilinen olası değerler kümesiyle kısıtlamak için, normal bir ifade kullanın. `{action:regex(^(list|get|create)$)}` Örneğin, yalnızca `action` rota değeri `list`, `get`, veya`create`ile eşleşir. Kısıtlama sözlüğüne geçirilirse dize `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biriyle eşleşmeyen kısıtlama sözlüğüne (bir şablon içinde satır içi değil) geçirilen kısıtlamalar da normal ifadeler olarak kabul edilir.

## <a name="custom-route-constraints"></a>Özel yol kısıtlamaları

Yerleşik yol kısıtlamalarına ek olarak, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> arabirimi uygulayarak özel yol kısıtlamaları oluşturulabilir. Arabirim, kısıtlama `Match` `true` karşılanıp`false` Aksi takdirde döndüren tek bir yöntemi içerir. <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>

Özel <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>bir kullanmak için yol kısıtlama türü, uygulamanın uygulamanın hizmet kapsayıcısında kayıtlı <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> olması gerekir. , Yol kısıtlama <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> anahtarlarını bu kısıtlamaları doğrulayan uygulamalarla eşleyen bir sözlüktür. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> Bir uygulama <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> , hizmetlerin bir parçası olarak `Startup.ConfigureServices` ' de güncelleştirilir [. Addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) çağrısı veya doğrudan ile <xref:Microsoft.AspNetCore.Routing.RouteOptions> `services.Configure<RouteOptions>`yapılandırma. Örneğin:

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

Aşağıdaki örnek, yol değerlerinin ve bir <xref:Microsoft.AspNetCore.Routing.RouteCollection>sözlüğünün bir sözlüğüne verilen bir yolun bağlantısının nasıl oluşturulacağını gösterir.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Önceki örnek sonunda <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>oluşturulan. `/package/create/123` Sözlük, " `operation` paketyolunu`id` izle" şablonunun ve rota değerlerini sağlar. `package/{operation}/{id}` Ayrıntılar için, [yönlendirme ara yazılımı kullanma](#use-routing-middleware) bölümünde veya [örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)örnek koda bakın.

<xref:Microsoft.AspNetCore.Routing.VirtualPathContext> Oluşturucunun ikinci parametresi bir *ortam değerleri*koleksiyonudur. Ortam değerleri, bir geliştiricinin bir istek bağlamı içinde belirtmesi gereken değer sayısını sınırlandırdığından kullanım için uygundur. Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir. ASP.NET Core `About` MVC uygulamasının `Index` &mdash;öğesinin `HomeController`eyleminde, ortam değeri`Home` kullanılan eyleme bağlamak için denetleyici yol değerini belirtmeniz gerekmez.

Bir parametreyle eşleşmeyen çevresel değerler yok sayılır. Ayrıca, açıkça sağlanmış bir değer çevresel değeri geçersiz kıldığında çevresel değerler de yoksayılır. Eşleştirme, URL 'de soldan sağa doğru gerçekleşir.

Açık olarak sağlanmış ancak yolun bir segmentiyle eşleşmeyen değerler sorgu dizesine eklenir. Aşağıdaki tabloda, yol şablonu `{controller}/{action}/{id?}`kullanılırken sonuç gösterilmektedir.

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

Bağlantı oluşturma yalnızca ve `controller` `action` için eşleşen değerler sağlandığında bu yol için bir bağlantı oluşturur.

## <a name="complex-segments"></a>Karmaşık segmentler

Karmaşık segmentler (örneğin `[Route("/x{token}y")]`), sabit değerli olmayan değişmez değer ile sağdan sola eşleştirilirken işlenir. Karmaşık segmentlerin nasıl eşleştirileceği hakkında ayrıntılı bir açıklama için [bu koda](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) bakın. [Kod örneği](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) ASP.NET Core tarafından kullanılmaz, ancak karmaşık segmentler hakkında iyi bir açıklama sağlar.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
