---
title: ASP.NET Core yönlendirme
author: ardalis
description: Nasıl ASP.NET Core yönlendirme işlevini gelen bir isteğin bir rota işleyiciye eşleme için sorumlu olduğunu keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: d9ba96c7b2abd35b1b13c84814bf3f776e8d8731
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861063"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Yönlendirme işlevini, gelen bir istek için bir rota işleyiciye eşleme sorumludur. Yollar uygulamada tanımlı ve uygulama başlatıldığında yapılandırılmış. Bir rota isteğe bağlı olarak istekte bulunan URL'den değerleri ayıklayabilir ve bu değerler daha sonra istek işleme için kullanılabilir. Uygulamadan rota bilgilerini kullanarak, yönlendirme de rota işleyicilerine eşleyen URL üretmek için bir işlevdir. Bu nedenle, yönlendirme bir URL veya yol işleyicisi bilgilere göre bir belirli bir rota işleyiciye karşılık gelen URL göre bir rota işleyiciye bulabilirsiniz.

> [!IMPORTANT]
> Bu belge, alt düzey ASP.NET Core yönlendirme kapsar. ASP.NET Core MVC yönlendirme hakkında daha fazla bilgi için bkz: <xref:mvc/controllers/routing>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme Temelleri

Yönlendirme kullanan *yollar* (uygulamaları <xref:Microsoft.AspNetCore.Routing.IRouter>) için:

* Gelen istekler harita *rota işleyicileri*.
* Yanıtları kullanılan URL'leri oluşturur.

Genellikle, bir uygulamanın tek bir rota koleksiyonu vardır. Rota koleksiyonu, bir istek ulaştığında sırayla işlenir. Gelen istek, istek URL'si çağırarak eşleşen bir rota arar <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemi her kullanılabilir yolda bir rota koleksiyonu. Aksine, bir yanıt yönlendirme (örneğin, yeniden yönlendirme veya bağlantıları) rota bilgilerine dayalı URL'lerini oluşturmak ve bu nedenle sabit kodlu URL'lere bakım yardımcı olan önlemek için kullanabilirsiniz.

Yönlendirme bağlı olduğu [ara yazılım](xref:fundamentals/middleware/index) tarafından işlem hattı <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> sınıfı. [ASP.NET Core MVC](xref:mvc/overview) yapılandırmasına bir parçası olarak ara yazılım ardışık düzene yönlendirme ekler. Bir tek başına bileşeni olarak Yönlendirme kullanma hakkında bilgi edinmek için [kullanım yönlendirme ara yazılım](#use-routing-middleware) bölümü.

### <a name="url-matching"></a>URL eşleştirme

URL ile eşleşen işlemdir hangi yönlendirme dağıtımları tarafından gelen bir istek için bir *işleyici*. Bu işlem URL yolunda verileri temel alır, ancak istekteki herhangi bir veri dikkate alınması gereken genişletilebilir. İşleyicileri ayırmak için istekleri gönderme olanağı, büyüklüğü ve karmaşıklığı ile bir uygulama, ölçeklendirme için anahtardır.

Gelen istekleri girin `RouterMiddleware`, çağıran <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> yöntemi dizideki her bir yol. <xref:Microsoft.AspNetCore.Routing.IRouter> Örneği seçer kullanılıp kullanılmayacağını *işlemek* ayarlayarak isteğin [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) NULL olmayan <xref:Microsoft.AspNetCore.Http.RequestDelegate>. İstek için bir işleyici bir rotayı ayarlar, işlemenin durması yönlendirmek ve işleyicisi isteği işlemek için çağrılır. Tüm yollar çalıştı ve istek için işleyici yok bulunuyorsa, ara yazılım çağırır *sonraki*, ve istek ardışık düzende sonraki ara yazılımı çağrılır.

Birincil Giriş `RouteAsync` olduğu [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) geçerli istekle ilişkili. `RouteContext.Handler` Ve [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) olduğunu çıkışları bir rotayla eşleşen sonra ayarlayın.

Bir eşleşme sırasında `RouteAsync` özelliklerini de ayarlar `RouteContext.RouteData` şimdiye kadarki gerçekleştirilen istek işleme göre uygun değerleri. Bir rotayla eşleşen bir istek varsa `RouteContext.RouteData` hakkında önemli durum bilgilerini içeren *sonucu*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) sözlüğü olan *rota değerleri* route'tan üretti. Bu değerler, genellikle URL belirteç oluşturma tarafından belirlenir ve kullanıcı girişi kabul etmek veya daha fazla dispatching kararlar uygulaması içinde için kullanılabilir.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) olan bir özellik paketi eşlenen rotaya ilgili ek veriler. `DataTokens` uygulama daha sonra kararlar verebilen her yol ile ilişkilendirmeyi durumu verilerini destekleyecek şekilde sağlanan eşleşen hangi rotalar. Bu değerler Geliştirici tanımlı ve yapmak **değil** herhangi bir şekilde yönlendirme davranışını etkiler. Ayrıca, veri belirteçleri için gizli olan değerleri aksine bir kolayca dönüştürülemez, gelen dizeleri ve rota değerleri, herhangi bir türde olabilir.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) isteği başarıyla eşleşen yer alan yolları bir listesidir. Yollar başka içinde yuvalanabilir. `Routers` Özelliği üzerinden bir eşleşme ile sonuçlandı yolların mantıksal ağaç yolu gösterir. Genel olarak, listedeki ilk öğe `Routers` rota koleksiyonu ve URL üretmek için kullanılmamalıdır. Son öğenin `Routers` eşleşen bir rota işleyicisi.

### <a name="url-generation"></a>URL üretimi

URL üretimi işlemdir hangi yönlendirerek, rota değerleri kümesine göre bir URL yolu oluşturabilirsiniz. Bu, işleyicileri ve bunlara erişim URL'leri arasında mantıksal bir ayrım sağlar.

URL üretimi, benzer bir süreçtir takip eder, ancak kullanıcı veya framework kodu çağırma ile başlar <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> yöntemi rota koleksiyonu. Her *rota* ardından sahip kendi `GetVirtualPath` yöntemi null olmayan bir kadar sırayla çağrılır <xref:Microsoft.AspNetCore.Routing.VirtualPathData> döndürülür.

Birincil giriş için `GetVirtualPath` şunlardır:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Yollar, öncelikle tarafından sağlanan rota değerleri kullanın `Values` ve `AmbientValues` bir URL'yi oluşturmak mümkün olduğu ve hangi içerecek şekilde değerleri karar vermek için. `AmbientValues` Yönlendirme sistem geçerli istekle eşleşen dizinden üretilen rota değerlerini alır. Buna karşılık, `Values` geçerli işlem için istenen URL'yi oluşturmak nasıl belirten rota değerlerdir. `HttpContext` Hizmetleri ya da geçerli bağlam ile ilişkili ek veri almak bir rota ihtiyacı sağlanır.

> [!TIP]
> Düşünün [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) geçersiz kılmalar için bir dizi olarak [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). URL üretimi, rota değerleri için bağlantıları aynı yol ya da rota değerlerini kullanarak URL üretmek kolay hale getirmek için geçerli istek yeniden dener.

Çıkışı `GetVirtualPath` olduğu bir `VirtualPathData`. `VirtualPathData` Paralel olan `RouteData`. `VirtualPathData` içeren `VirtualPath` çıkış URL'si ve rota tarafından ayarlanması bazı ek özellikler.

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) özelliği içeren *sanal yol* rota tarafından üretilen. Gereksinimlerinize bağlı olarak, daha fazla yolu işlem gerekebilir. Oluşturulan URL HTML işlemek istiyorsanız, uygulamanın taban yolu önüne ekleyin.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) URL başarıyla oluşturuldu rota bir başvurudur.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) özellikleri olan URL'yi oluşturulan yol ilgili ek veri sözlüğü. Bu, paralel, [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Yollar oluşturma

Yönlendirme sağlar <xref:Microsoft.AspNetCore.Routing.Route> sınıf öğesinin standart uygulaması olarak <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` kullanan *rota şablonu* karşı URL yolu eşleşen desenleri tanımlamak için sözdizimi, <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> çağrılır. `Route` bir URL'yi oluşturmak için aynı rota şablonunu kullanır, `GetVirtualPath` çağrılır.

Çoğu uygulama çağırarak yollar oluşturma <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> veya benzer genişletme yöntemlerini tanımlanan <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Bu yöntemlerin tümü, bir örneğini oluşturmak <xref:Microsoft.AspNetCore.Routing.Route> ve rota koleksiyona ekleyin.

`MapRoute` bir rota işleyicisi parametre almaz. `MapRoute` yalnızca tarafından işlenen rotalar ekler <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Varsayılan işleyici olduğundan bir `IRouter`, istek işlememesi isteyebilirsiniz. Örneğin, yalnızca işleyen bir varsayılan işleyici eşleşen bir kullanılabilir denetleyici ve eylem istekleri gibi ASP.NET Core MVC genellikle yapılandırılır. MVC için yönlendirme hakkında daha fazla bilgi için bkz: <xref:mvc/controllers/routing>.

Aşağıdaki kod örneği, örneğidir bir `MapRoute` tipik bir ASP.NET Core MVC yönlendirme tanımı tarafından kullanılan arama:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yolu gibi eşleşen `/Products/Details/17` ve rota değerlerini ayıklar `{ controller = Products, action = Details, id = 17 }`. Rota değerlerini ve URL yolunu kesimler halinde bölme ve her bir kesim ile eşleşen belirlenir *parametre yol* adını, rota şablonu. Rota parametrelerine olarak adlandırılır. Parametre adı, küme ayraçları içine kapsayan tarafından tanımlı `{ ... }`.

Yukarıdaki şablonu URL yolu da eşleşen `/` ve değerleri oluşturur `{ controller = Home, action = Index }`. Bu durum oluşur `{controller}` ve `{action}` rota parametrelerinin varsayılan değerlere sahip olur ve `id` rota parametresinin isteğe bağlı. Bir eşittir `=` ardından gelen bir değer tarafından sonra rota parametresi adı parametresi için varsayılan bir değer tanımlar. Bir soru işareti `?` sonra rota parametre adı parametrenin isteğe bağlı olarak tanımlar. Rota parametrelerinin varsayılan bir değerle *her zaman* rota eşleştiğinde bir yönlendirme değeri üretir. İsteğe bağlı parametreler, karşılık gelen hiçbir URL yol kesimi ise bir yönlendirme değeri üretmediği.

Bkz: [rota şablonu başvurusu](#route-template-reference) rota şablonu özellikleri ve söz dizimi eksiksiz bir açıklaması.

Bu örnek içeren bir *rota kısıtlaması*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon bir URL yolu gibi eşleşen `/Products/Details/17` ama `/Products/Details/Apples`. Rota parametre tanımında `{id:int}` tanımlayan bir [rota kısıtlaması](#route-constraint-reference) için `id` yol parametresi. Rota kısıtlamalarını uygulamak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> ve onları doğrulamak için rota değerlerini denetleyin. Bu örnekte, rota değeri `id` tamsayıya dönüştürülebilir olmalıdır. Bkz: [rota kısıtlaması başvurusu](#route-constraint-reference) için framework tarafından sağlanan rota kısıtlamalarını daha ayrıntılı bir açıklaması.

Ek aşırı yüklemeleri `MapRoute` değerleri kabul edin `constraints`, `dataTokens`, ve `defaults`. Bu ek parametreleri `MapRoute` türü olarak tanımlanmış `object`. Bu parametre normal kullanım, burada parametre adları anonim tür eşleşen özellik adlarını rota anonim olarak belirlenmiş bir nesne geçirmektir.

Aşağıdaki iki örnek eşdeğer rotalar oluşturun:

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
> Satır içi söz dizimi kısıtlamaları ve varsayılan değerleri tanımlamak için basit yollar için kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçlerini gibi özellikleri vardır.

Aşağıdaki örnek, birkaç daha fazla senaryoyu göstermektedir:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Bu şablon bir URL yolu gibi eşleşen `/Blog/All-About-Routing/Introduction` ve değerleri ayıklar `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Varsayılan rota değerleri için `controller` ve `action` olsa bile karşılık gelen hiçbir rota parametrelerinin şablonda rota tarafından üretilir. Varsayılan değerleri, rota şablonu belirtilebilir. `article` Rota parametresi olarak tanımlanmış olan bir *tümünü* yıldız görünümünü tarafından `*` rota parametre adından önce. Tümünü rota parametrelerinin, URL yolu geri kalanında yakalayın ve boş bir dize da eşleştirebilirsiniz.

Bu örnek, rota kısıtlamaları ve veri belirteçleri ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Bu şablon bir URL yolu gibi eşleşen `/en-US/Products/5` ve değerleri ayıklar `{ controller = Products, action = Details, id = 5 }` ve veri belirteçleri `{ locale = en-US }`.

![Yerel Windows belirteçleri](routing/_static/tokens.png)

### <a name="url-generation"></a>URL üretimi

`Route` Sınıfı da gerçekleştirebilirsiniz URL üretimi bir dizi rota değerleri, rota şablonu ile birleştirerek. Bu mantıksal olarak URL yolu eşleşen ters işlemidir.

> [!TIP]
> URL üretimi daha iyi anlamak için oluşturmak ve ardından söz konusu URL rota şablonu nasıl BC hakkında düşünmek için hangi URL'yi düşünün. Hangi değerleri üretilen? Bu, URL üretimi nasıl çalıştığını kabaca eşdeğeridir `Route` sınıfı.

Bu örnek, temel bir ASP.NET Core MVC stili rota kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Rota değerleri ile `{ controller = Products, action = List }`, bu yolun bir URL oluşturur `/Products/List`. Rota değerleri, URL yolu oluşturmak karşılık gelen rota parametrelerinin yerini alır. Bu yana `id` isteğe bağlıdır parametre yol, bir değer yok sorun değil.

Rota değerleri ile `{ controller = Home, action = Index }`, bu yolun bir URL oluşturur `/`. Sağlanan yol değerleri varsayılan değerleriyle eşleşir, böylece bu değerlere karşılık gelen segmentleri güvenli bir şekilde atlanabilir. Her iki URL'leri, bu rota tanımını gidiş dönüş oluşturulan ve URL'yi oluşturmak için kullanılan aynı rota değerlerini üretir.

> [!TIP]
> ASP.NET Core MVC kullanarak uygulama kullanması gereken <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> doğrudan yönlendirme içine çağırmak yerine URL'lerini oluşturmak için.

URL üretimi hakkında daha fazla bilgi için bkz: [url üretimi başvuru](#url-generation-reference).

## <a name="use-routing-middleware"></a>Yönlendirme ara yazılım kullanın

::: moniker range=">= aspnetcore-2.1"

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) uygulamanın proje dosyasında.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) uygulamanın proje dosyasında.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Başvuru [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) uygulamanın proje dosyasında.

::: moniker-end

Hizmet kapsayıcısını yönlendirme ekleme `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Yollar yapılandırılmalıdır `Startup.Configure` yöntemi. Örnek uygulamayı bu API'leri kullanır:

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; Yalnızca HTTP GET isteklerini eşleşir.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

Aşağıdaki tablo, belirli bir URI'leri yanıtları gösterir.

| URI                  | Yanıt                                          |
| -------------------- | ------------------------------------------------- |
| /Package/Create/3    | Merhaba! Rota değerleri: [işlem oluşturun], [ID, 3] |
| / /-3 paketi/parçası    | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3] |
| / Paket / /-3 izleme /   | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3] |
| / Package/izleme /      | &lt;Eşleşme, kalan&gt;                    |
| /Hello/Joe Al       | Merhaba, ALi!                                          |
| POST /hello/Joe      | &lt;Geçiş, yalnızca eşleşen HTTP GET&gt;       |
| /Hello/Joe/Smith Al | &lt;Eşleşme, kalan&gt;                    |

Tek bir yol yapılandırıyorsanız, çağrı <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> öğesinde geçen bir `IRouter` örneği. Kullanmanız gerekmez <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Çerçeve yollarını gibi oluşturmak için genişletme yöntemleri sağlar:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Bu yöntemlerden bazıları gibi `MapGet`, gerekli bir `RequestDelegate` sağlanacak. `RequestDelegate` Olarak kullanılan *rota işleyiciye* zaman yol ile eşleşen. Bu serideki diğer yöntemleri rota işleyiciye bir ara yazılım ardışık düzenini kullanmak için yapılandırma sağlar. Varsa *harita* değil accept yöntemi bir işleyici gibi `MapRoute`, ardından bunu <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

`Map[Verb]` Yöntemleri kısıtlamaları HTTP fiili yöntem adı için rota sınırlamak için kullanın. Örneğin, <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> ve <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Belirteçleri kaşlı ayraçlar içinde (`{ ... }`) tanımlamak *rota parametreleri* rotanın eşleşmesi durumunda bağlıdır. İçindeki bir yol parçasının birden fazla rota parametresini tanımlayabilirsiniz, ancak bir değişmez değer ayrılmalıdır. Örneğin, `{controller=Home}{action=Index}` arasında herhangi bir değişmez değer olduğundan geçerli bir yol değil `{controller}` ve `{action}`. Bu rota parametrelerinin bir adı olmalıdır ve ek öznitelikler belirtmiş olabilirsiniz.

Metin rota parametrelerinin dışında (örneğin, `{id}`) ve yol ayırıcısı `/` URL metin eşleşmesi gerekir. Eşleşen metin büyük/küçük harfe ve URL yolunu kodu çözülmüş gösterimini göre. Değişmez değer rota parametresi sınırlayıcı eşleştirilecek `{` veya `}`, karakter tekrarlayarak kaçış (`{{` veya `}}`).

İsteğe bağlı dosya uzantısına sahip bir dosya adı yakalama girişimi URL desenleri ek hususlar vardır. Örneğin, şablonu düşünün `files/{filename}.{ext?}`. Her ikisi de `filename` ve `ext` var, her iki değerler doldurulur. Yalnızca `filename` URL'nin yol eşleşmeleri var olduğundan sondaki nokta `.` isteğe bağlıdır. Aşağıdaki URL'ler bu rotanın eşleşmesi:

* `/files/myFile.txt`
* `/files/myFile`

Kullanabileceğiniz `*` URI rest'e bağlamak için bir rota parametresini öneki olarak karakter. Bu adlı bir *tümünü* parametresi. Örneğin, `blog/{*slug}` ile başlayan herhangi bir URL ile eşleşen `/blog` ve aşağıdaki herhangi bir değer vardı (için atanan `slug` rota değeri). Genel parametreleri da boş dize eşleşebilir.

::: moniker range=">= aspnetcore-2.2"

Yol, yol ayırıcı dahil olmak üzere, bir URL'yi oluşturmak için kullanıldığında, yakalama-all parametresini uygun karakterleri kaçırır. (`/`) karakter. Örneğin, yol `foo/{*path}` rota değerleri ile `{ path = "my/path" }` oluşturur `foo/my%2Fpath`. Atlanan eğik unutmayın. Gidiş dönüş yol ayırıcı karakter kullanın `**` rota parametresinin öneki. Rota `foo/{**path}` ile `{ path = "my/path" }` oluşturur `foo/my/path`.

::: moniker-end

Rota parametrelerine sahip olabilir *varsayılan değerler*, belirlenen bir eşittir işaretiyle ayırarak parametre adından sonra varsayılan belirterek (`=`). Örneğin, `{controller=Home}` tanımlar `Home` için varsayılan değer olarak `controller`. Hiçbir değer parametresi için URL'de varsa varsayılan değer kullanılır. Varsayılan değerler yanı sıra, rota parametrelerinin bir soru işareti eklenerek belirtilen isteğe bağlı olabilir (`?`) parametre adı, giriş olarak sonuna `id?`. İsteğe bağlı değerler ve varsayılan rota parametrelerini arasındaki fark, bir rota parametresini varsayılan değeri her zaman bir değer üreten olan; yalnızca bir değer istek URL'si sağlandığında isteğe bağlı parametresi bir değer içeriyor.

::: moniker range=">= aspnetcore-2.2"

Rota parametrelerine URL'den bağlı rota değerle eşleşmelidir kısıtlamaları olabilir. İki nokta ekleme (`:`) ve rota parametresi adını belirttikten sonra kısıtlama adı bir *satır içi kısıtlama* bir rota parametresini üzerinde. Kısıtlama bağımsız değişkenleri gerektiriyorsa, parantez içine alınmış `( )` sonra kısıtlama adı. Başka bir iki nokta üst üste ekleyerek, birden çok satır içi kısıtlamalarını belirtilebilir (`:`) ve kısıtlama adı. Kısıtlama adı ve bağımsız değişkenler geçirilir <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> bir örneğini oluşturmak için hizmet <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> URL işlenmesinde kullanılacak. Kısıtlama Oluşturucusu Hizmetleri gerektiriyorsa, bağımlılık ekleme'nın uygulama Hizmetleri'nden çözümlenen. Örneğin, rota şablonu `blog/{article:minlength(10)}` belirtir bir `minlength` kısıtlaması bağımsız değişkeniyle `10`. Rota kısıtlamalarını ve kısıtlamaları framework tarafından sağlanan bir listesi hakkında daha fazla bilgi için bkz. [rota kısıtlaması başvurusu](#route-constraint-reference) bölümü.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Rota parametrelerine URL'den bağlı rota değerle eşleşmelidir kısıtlamaları olabilir. İki nokta ekleme (`:`) ve rota parametresi adını belirttikten sonra kısıtlama adı bir *satır içi kısıtlama* bir rota parametresini üzerinde. Kısıtlama bağımsız değişkenleri gerektiriyorsa, parantez içine alınmış `( )` sonra kısıtlama adı. Başka bir iki nokta üst üste ekleyerek, birden çok satır içi kısıtlamalarını belirtilebilir (`:`) ve kısıtlama adı. Kısıtlama adı ve bağımsız değişkenler geçirilir <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> bir örneğini oluşturmak için hizmet <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> URL işlenmesinde kullanılacak. Örneğin, rota şablonu `blog/{article:minlength(10)}` belirtir bir `minlength` kısıtlaması bağımsız değişkeniyle `10`. Rota kısıtlamalarını ve kısıtlamaları framework tarafından sağlanan bir listesi hakkında daha fazla bilgi için bkz. [rota kısıtlaması başvurusu](#route-constraint-reference) bölümü.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Rota parametrelerine bağlantılar ve eşleşen eylemleri ve sayfaları için bir URI'leri oluştururken bir parametrenin değeri dönüştürme parametresi dönüştürücüler, da olabilir. Kısıtlamalar gibi parametre dönüştürücüler satır içi bir rota parametresini bir iki nokta üst üste ekleyerek eklenebilir (`:`) ve rota parametre adından sonra transformer adı. Örneğin, rota şablonu `blog/{article:slugify}` belirtir bir `slugify` dönüştürücü.

::: moniker-end

Aşağıdaki tabloda, bazı rota şablonlarının ve davranışları gösterir.

| Rota şablonu                         | Örnek URL eşleştirme  | Notlar                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| Merhaba                                  | / Merhaba                | Yalnızca tek bir yol ile eşleşir `/hello`                                  |
| {Page=Home}                            | /                     | Eşleşen ve ayarlar `Page` için `Home`                                      |
| {Page=Home}                            | / Contact              | Eşleşen ve ayarlar `Page` için `Contact`                                   |
| {Denetleyici} / {eylem} / {id}?            | / Ürünler/listesi        | Eşlendiği `Products` denetleyicisi ve `List` eylemi                       |
| {Denetleyici} / {eylem} / {id}?            | / Ürünler/Ayrıntılar/123 |  Eşlendiği `Products` denetleyicisi ve `Details` eylem.  `id` 123 için ayarlayın |
| {controller=Home}/{action=Index}/{id?} | /                     |  Eşlendiği `Home` denetleyicisi ve `Index` yöntemi `id` göz ardı edilir.       |

Şablon kullanarak genel yönlendirme en basit yaklaşımdır. Kısıtlamaları ve varsayılan rota şablonu dışında da belirtilebilir.

> [!TIP]
> Etkinleştirme [günlüğü](xref:fundamentals/logging/index) görmek için nasıl yönlendirme uygulamalarında gibi yerleşik `Route`, eşleşen istekleri.

## <a name="reserved-routing-names"></a>Yönlendirme ayrılmış adlar

Aşağıdaki anahtar sözcükler ayrılmış adları ve rota adları veya parametre kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Rota kısıtlaması başvurusu

Rota kısıtlamalarını yürütme bir `Route` eşleşen gelen URL söz dizimini ve rota değerlerini ve URL yolunu simgeleştirilmiş. Rota kısıtlamalarını genellikle rota şablonu ile ilişkili yol değişkenlerinizin ve Evet olun/yok veya yüklememe değer kabul edilebilir bir karardır. Bazı rota kısıtlamalarını isteğin yönlendirildiği olup olmadığını göz önünde bulundurun için rota değeri dışındaki verileri kullanın. Örneğin, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> kabul edebilir veya kendi HTTP fiiline bağlı bir isteği reddetmek.

> [!WARNING]
> Kısıtlamaları kullanmaktan kaçının **giriş doğrulama** çünkü bunu geçersiz giriş yapılması sonuçlanır bir *404 - Bulunamadı* yanıt yerine bir *400 - bozuk istek* ile bir uygun bir hata iletisi. Rota kısıtlamalarını alışkın olduğunuz **belirsizliğinin ortadan kaldırılmasını** arasında belirli bir rota için girişler değil doğrulamak için benzer yollar.

Aşağıdaki tabloda, bazı rota kısıtlamalarını ve bunların beklenen bir davranış gösterir.

| kısıtlama | Örnek | Örnek eşleşmeleri | Notlar |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Herhangi bir tamsayı ile eşleşir |
| `bool` | `{active:bool}` | `true`, `FALSE` | Eşleşme `true` veya `false` (büyük-küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Geçerli bir eşleşen `DateTime` değeri (uyarı sabit kültür - bakın) |
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
| `required` | `{name:required}` | `Rick` |  Parametresi olmayan değer URL oluşturma sırasında mevcut olduğunu uygulamak için kullanılan |

Birden çok, tek bir parametre için virgülle ayrılmış kısıtlamalar uygulanabilir. Örneğin, aşağıdaki kısıtlama parametresi için 1 veya daha büyük bir tamsayı değeri kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın. Framework tarafından sağlanan rota kısıtlamaları, rota değerleri depolanan değerleri değiştirmeyin. URL'den ayrıştırılmış tüm rota değerleri dize olarak depolanır. Örneğin, `float` rota değeri kayana dönüştürülecek kısıtlaması çalışır, ancak dönüştürülen değeri yalnızca, dönüştürülebilir bir kayan nokta doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core framework ekler `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` normal ifade oluşturucusu. Bkz: <xref:System.Text.RegularExpressions.RegexOptions> bu üyeleri bir açıklaması.

Normal ifadeler, sınırlayıcılar ve Yönlendirme ve C# dili tarafından kullanılan benzer belirteçleri kullanın. Normal ifade belirteçleri Atlanan gerekir. Normal ifade kullanmanızı `^\d{3}-\d{2}-\d{4}$` yönlendirme, deyim olmalıdır `\` olarak yazdığınız karakterler `\\` atlamak için C# kaynak dosyadaki `\` dize kaçış karakteri (kullanmadığınız sürece [verbatim dizesi değişmez değerler](/dotnet/csharp/language-reference/keywords/string). `{`, `}`, `[`, Ve `]` karakter yönlendirme parametresi sınırlayıcı karakter kaçış katlayarak konulmalıdır. Aşağıdaki tabloda, normal bir ifade ve kaçan sürümünü gösterir.

| İfade            | Kaçış                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Sık sık yönlendirme kullanılan normal ifadeler ile başlayıp `^` karakter (eşleşme dizenin konumdan başlayarak) ve bitiş ile `$` karakter (eşleşme dizenin konumunu biten). `^` Ve `$` karakter tüm rota parametre değeri normal ifade eşleştirmesi sağlar. Olmadan `^` ve `$` karakterler, normal bir ifadeyle eşleşen genellikle istenmeyen dize içindeki herhangi bir alt dize. Aşağıdaki tablo bazı örnekler gösterilmektedir ve neden aynı veya eşleştirilecek başarısız açıklar.

| İfade   | Dize    | Eşleştirme | Yorum               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | Merhaba     | Evet   | alt dize eşleşmeleri     |
| `[a-z]{2}`   | 123abc456 | Evet   | alt dize eşleşmeleri     |
| `[a-z]{2}`   | mz        | Evet   | ifadeyle eşleşiyor    |
| `[a-z]{2}`   | MZ        | Evet   | büyük/küçük harfe duyarlı değil    |
| `^[a-z]{2}$` |  Merhaba    | Hayır    | bkz: `^` ve `$` yukarıda |
| `^[a-z]{2}$` | 123abc456 | Hayır    | bkz: `^` ve `$` yukarıda |

Normal ifade söz dizimi hakkında daha fazla bilgi için bkz. [.NET Framework normal ifadelerinde](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Olası değerler bilinen bir dizi parametre sınırlamak için normal bir ifade kullanın. Örneğin, `{action:regex(^(list|get|create)$)}` yalnızca eşleşen `action` yönlendirmek için değer `list`, `get`, veya `create`. Dize kısıtlamaları sözlük geçirilen `^(list|get|create)$` eşdeğerdir. Bilinen kısıtlamalardan biri eşleşmiyor (hizalı şablon içinde değil) kısıtlamaları sözlükteki geçirilen kısıtlamalar da normal ifadeler şeklinde kabul edilir.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Parametre transformer başvurusu

Parametre dönüştürücüler yürütmek için bir bağlantı oluştururken bir `Route`. Parametre dönüştürücüler parametrenin rota değeri alın ve yeni bir dize değerine dönüştürün. Oluşturulan bağlantısına dönüştürülen değer kullanılır. Örneğin, bir özel `slugify` parametresi transformer yol deseninde `blog\{article:slugify}` ile `Url.Action(new { article = "MyTestArticle" })` oluşturur `blog\my-test-article`. Parametre dönüştürücüler uygulamak `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer` ve kullanarak yapılandırılmış <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.

Parametre dönüştürücüler çerçevelerine bir uç nokta çözümler yapılacağı URI dönüştürmek için de kullanılır. Örneğin, ASP.NET Core MVC eşleştirmek için kullanılan rota değeri dönüştürmek için parametre dönüştürücüler kullanan bir `area`, `controller`, `action`, ve `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Önceki yol, bir eylem ile `SubscriptionManagementController.GetAll()` URI'si ile eşleşen `/subscription-management/get-all`. Bir parametre transformer giden bir bağlantı oluşturmak için kullanılan rota değerlerini değiştirmez. `Url.Action("GetAll", "SubscriptionManagement")` çıkaran `/subscription-management/get-all`.

ASP.NET Core MVC ile birlikte sunulan `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API kuralı. Kuralı belirtilen parametre transformer uygulamadaki tüm öznitelik rota belirteçler için geçerlidir.

::: moniker-end

## <a name="url-generation-reference"></a>URL üretimi başvurusu

Aşağıdaki örnek, rota değerleri sözlüğü belirtilen bir rotaya bir bağlantı oluşturmak gösterilir ve <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

`VirtualPath` Önceki sonunda oluşturulan örnek `/package/create/123`. Sözlük sağlayan `operation` ve `id` rota değerleri "İzleme paketi yol" şablonunun `package/{operation}/{id}`. Örnek kodda Ayrıntılar için bkz [kullanım yönlendirme ara yazılım](#use-routing-middleware) bölüm veya [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

İkinci parametresi `VirtualPathContext` oluşturucudur koleksiyonu *ortam değerlerini*. Ortam değerleri, belirli bir istek bağlamı içinde bir geliştirici belirtmelisiniz değerlerin sayısını sınırlayarak kolaylık sağlar. Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir. Bir ASP.NET Core MVC uygulaması açıksa `About` eylemi `HomeController`, bağlanmak için denetleyici rota değeri belirtmeniz gerekmez `Index` eylem&mdash;ortam değerini `Home` kullanılır.

Bir parametre eşleşmeyen ortam değerleri yoksayılır ve ortam değerleri açıkça belirtilen bir değer, soldan sağa doğru giden geçersiz kıldığında da yoksayılır URL.

Açıkça sağlanan ancak, herhangi bir şey eşleşmeyen değerler sorgu dizesine eklenir. Rota şablonu kullanılırken sonucu aşağıdaki tabloda gösterilmiştir `{controller}/{action}/{id?}`.

| Ortam değerleri                | Açık değerler                   | Sonuç                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| Denetleyici = "Home"             | Eylem "Hakkında" =                    | `/Home/About`           |
| Denetleyici = "Home"             | Denetleyici = "Order", Eylem "Hakkında" = | `/Order/About`          |
| Denetleyici "Home", renk = = "Red" | Eylem "Hakkında" =                    | `/Home/About`           |
| Denetleyici = "Home"             | Eylem = "Hakkında", renk = "Red"        | `/Home/About?color=Red` |

Bir rota parametresi gelmiyor varsayılan bir değer varsa ve bu değeri açıkça sağlanan varsayılan değer eşleşmesi gerekir:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Denetleyici ve eylem için eşleşen değerler sağlandığında bağlama oluşturmayı yalnızca bu yol için bir bağlantı oluşturur.
