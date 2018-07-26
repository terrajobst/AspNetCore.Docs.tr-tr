---
title: ASP.NET Core yönlendirme
author: ardalis
description: Nasıl ASP.NET Core yönlendirme işlevini gelen bir isteğin bir rota işleyiciye eşleme için sorumlu olduğunu keşfedin.
ms.author: riande
ms.date: 07/25/2018
uid: fundamentals/routing
ms.openlocfilehash: 19265ac4d19915462c50628061600b1fde04694b
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254889"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Yönlendirme işlevini, gelen bir istek için bir rota işleyiciye eşleme sorumludur. Yollar ASP.NET uygulamasında tanımlandığı ve uygulama başlatıldığında yapılandırılmış. Bir rota isteğe bağlı olarak istekte bulunan URL'den değerleri ayıklayabilir ve bu değerler daha sonra istek işleme için kullanılabilir. ASP.NET uygulamasından yol bilgileri kullanarak, yönlendirme de rota işleyicilerine eşleyen URL üretmek için bir işlevdir. Bu nedenle, yönlendirme bir URL veya yol işleyicisi bilgilere göre bir belirli bir rota işleyiciye karşılık gelen URL göre bir rota işleyiciye bulabilirsiniz.

>[!IMPORTANT]
> Bu belge, düşük düzey ASP.NET Core yönlendirme kapsar. Bkz: ASP.NET Core MVC yönlendirme için [denetleyici eylemlerine yönlendirme](../mvc/controllers/routing.md)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme Temelleri

Yönlendirme kullanan *yollar* (uygulamaları [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) için:

* gelen istekler harita *rota işleyicileri*

* yanıtları kullanılan URL'leri oluştur

Genellikle, bir uygulamanın tek bir rota koleksiyonu vardır. Rota koleksiyonu, bir istek ulaştığında sırayla işlenir. Gelen istek, istek URL'si çağırarak eşleşen bir rota arar `RouteAsync` yöntemi her kullanılabilir yolda bir rota koleksiyonu. Aksine, bir yanıt (örneğin, yeniden yönlendirme veya bağlantıları) rota bilgilerine dayalı URL üretmek için yönlendirme kullanır ve bu nedenle sabit kodlu URL'lere bakım yardımcı olan zorunluluğundan.

Yönlendirme bağlı olduğu [ara yazılım](xref:fundamentals/middleware/index) tarafından işlem hattı `RouterMiddleware` sınıfı. [ASP.NET Core MVC](xref:mvc/overview) yapılandırmasına bir parçası olarak ara yazılım ardışık düzene yönlendirme ekler. Bir tek başına bileşeni olarak Yönlendirme kullanma hakkında bilgi edinmek için [yönlendirme ara yazılımın kullanılması](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>URL eşleştirme

URL ile eşleşen işlemdir hangi yönlendirme dağıtımları tarafından gelen bir istek için bir *işleyici*. Bu işlem genellikle ve URL yolundaki verilere bağlıdır, ancak istekteki herhangi bir veri göz önünde bulundurun şekilde genişletilebilir. İşleyicileri ayırmak için istekleri gönderme olanağı, büyüklüğü ve karmaşıklığı ile bir uygulama, ölçeklendirme için anahtardır.

Gelen istekleri girin `RouterMiddleware`, çağıran `RouteAsync` yöntemi dizideki her bir yol. `IRouter` Örneği seçer kullanılıp kullanılmayacağını *işlemek* ayarlayarak isteğin `RouteContext.Handler` NULL olmayan `RequestDelegate`. Bir rota isteği için bir işleyici olarak ayarlamışsa, işlem durdurur ve işleyici rota isteği işlemek için çağrılır. Tüm yollar çalıştı ve istek için işleyici yok bulunuyorsa, ara yazılım çağırır *sonraki* ve istek ardışık düzende sonraki ara yazılımı çağrılır.

Birincil Giriş `RouteAsync` olduğu `RouteContext.HttpContext` geçerli istekle ilişkili. `RouteContext.Handler` Ve `RouteContext.RouteData` bir rotayla eşleşen sonra ayarlanır çıkışları olan.

Bir eşleşme sırasında `RouteAsync` ayrıca özelliklerini ayarlayacak `RouteContext.RouteData` kadar gerçekleştirilen istek işleme göre uygun değerleri. Bir rotayla eşleşen bir istek varsa `RouteContext.RouteData` hakkında önemli durum bilgilerini içerecek *sonucu*.

`RouteData.Values` ' ın bir sözlük *rota değerleri* route'tan üretti. Bu değerler, genellikle URL belirteç oluşturma tarafından belirlenir ve kullanıcı girişi kabul edin veya uygulamadaki başka dispatching kararları vermek için kullanılabilir.

`RouteData.DataTokens`  eşlenen rotaya ilgili ek veriler, bir özellik paketi olur. `DataTokens` veri uygulama daha sonra yol tabanlı kararları her yol ile eşleşen ilişkilendirmeyi durumunu desteklemek için sağlanır. Bu değerler Geliştirici tanımlı ve yapmak **değil** herhangi bir şekilde yönlendirme davranışını etkiler. Ayrıca, veri belirteçleri için gizli olan değerleri aksine bir kolayca dönüştürülemez, gelen dizeleri ve rota değerleri, herhangi bir türde olabilir.

`RouteData.Routers` istek başarıyla eşleşen yer alan yolları bir listesidir. Yollar iç içe geçirilemez birbirine içinde ve `Routers` özelliği üzerinden bir eşleşme ile sonuçlandı yolların mantıksal ağaç yolu gösterir. Listedeki ilk öğe genellikle `Routers` rota koleksiyonu ve URL üretmek için kullanılmamalıdır. Son öğenin `Routers` eşleşen bir rota işleyicisi.

### <a name="url-generation"></a>URL üretimi

URL üretimi işlemdir hangi yönlendirerek, rota değerleri kümesine göre bir URL yolu oluşturabilirsiniz. Bu, işleyicileri ve bunlara erişim URL'leri arasında mantıksal bir ayrım sağlar.

URL üretimi için benzer bir süreçtir takip eder, ancak bu kullanıcı veya framework kodu çağırma ile başlayan `GetVirtualPath` yöntemi rota koleksiyonu. Her *rota* ardından gerekir, `GetVirtualPath` yöntemi null olmayan bir kadar sırayla çağrılır `VirtualPathData` döndürülür.

Birincil giriş için `GetVirtualPath` şunlardır:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Yollar, öncelikle tarafından sağlanan rota değerleri kullanın `Values` ve `AmbientValues` bir URL'yi oluşturmak mümkün olduğu ve hangi içerecek şekilde değerleri karar vermek için. `AmbientValues` Yönlendirme sistem geçerli istekle eşleşen dizinden üretilen rota değerlerini alır. Buna karşılık, `Values` geçerli işlem için istenen URL'yi oluşturmak nasıl belirten rota değerlerdir. `HttpContext` Hizmetleri ya da geçerli bağlam ile ilişkili ek veri almak bir rota ihtiyacı sağlanır.

İpucu: düşünün `Values` geçersiz kılmalar için bir dizi olarak `AmbientValues`. URL üretimi, rota değerleri için bağlantıları aynı yol ya da rota değerlerini kullanarak URL üretmek kolay hale getirmek için geçerli istek yeniden dener.

Çıkışı `GetVirtualPath` olduğu bir `VirtualPathData`. `VirtualPathData` Paralel olan `RouteData`; içerdiği `VirtualPath` çıkış URL'si ve rota tarafından ayarlanması bazı ek özellikler.

`VirtualPathData.VirtualPath` Özelliği içeren *sanal yol* rota tarafından üretilen. Gereksinimlerinize bağlı olarak, daha fazla yolu işlem gerekebilir. Örneğin, oluşturulan URL HTML işlemek istiyorsanız, uygulamanın taban yolu önüne ekleyin gerekir.

`VirtualPathData.Router` URL başarıyla oluşturuldu rota bir başvurudur.

`VirtualPathData.DataTokens` Özellikleri olan URL'yi oluşturulan yol ilgili ek veri sözlüğü. Bu, paralel, `RouteData.DataTokens`.

### <a name="creating-routes"></a>Yollar oluşturma

Yönlendirme sağlar `Route` sınıf öğesinin standart uygulaması olarak `IRouter`. `Route` kullanan *rota şablonu* karşı URL yolu eşleşecektir desenleri tanımlamak için sözdizimi, `RouteAsync` çağrılır. `Route` bir URL'yi oluşturmak için aynı rota şablonunu kullanır, `GetVirtualPath` çağrılır.

Çoğu uygulama çağırarak yollar oluşturur `MapRoute` veya benzer genişletme yöntemlerini tanımlanan `IRouteBuilder`. Bu yöntemlerin tümü, bir örneğini oluşturmak `Route` ve rota koleksiyona ekleyin.

Not: `MapRoute` bir rota işleyicisi parametresini - verdiği hızlı tepkilerden faydalanamamış yalnızca tarafından işlenen rotalar ekler `DefaultHandler`. Varsayılan işleyici olduğundan bir `IRouter`, istek işlememesi isteyebilirsiniz. Örneğin, ASP.NET MVC, genellikle yalnızca işleyen bir varsayılan işleyici eşleşen bir kullanılabilir denetleyici ve eylem istekleri gibi yapılandırılır. MVC için yönlendirme hakkında daha fazla bilgi için bkz: [denetleyici eylemleri için rota](../mvc/controllers/routing.md).

Bir örnek, bir `MapRoute` tipik bir ASP.NET MVC yönlendirme tanımı tarafından kullanılan arama:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yolu gibi eşleşecektir `/Products/Details/17` ve rota değerlerini ayıklamak `{ controller = Products, action = Details, id = 17 }`. Rota değerlerini ve URL yolunu kesimler halinde bölme ve bulunduğu her segment eşleşen belirlenir *parametre yol* adını, rota şablonu. Rota parametrelerine olarak adlandırılır. Parametre adı, küme ayraçları içine kapsayan tarafından tanımlı `{ }`.

Yukarıdaki şablonu URL yolu da eşleşen `/` ve değerleri oluşturur `{ controller = Home, action = Index }`. Çünkü böyle `{controller}` ve `{action}` rota parametrelerinin varsayılan değerleri olan ve `id` rota parametresinin isteğe bağlı. Bir eşittir `=` ardından gelen bir değer tarafından sonra rota parametresi adı parametresi için varsayılan bir değer tanımlar. Bir soru işareti `?` sonra rota parametre adı parametrenin isteğe bağlı olarak tanımlar. Rota parametrelerinin varsayılan bir değerle *her zaman* yol ile eşleşen - isteğe bağlı parametreler karşılık gelen hiçbir URL yol kesimi ise bir yönlendirme değeri üretmek kalmaz, bir yönlendirme değeri üretmek.

Bkz: [rota şablonu başvurusu](#route-template-reference) rota şablonu özellikleri ve söz dizimi eksiksiz bir açıklaması.

Bu örnek içeren bir *rota kısıtlaması*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon bir URL yolu gibi eşleşecektir `/Products/Details/17`, ama `/Products/Details/Apples`. Rota parametre tanımında `{id:int}` tanımlayan bir *rota kısıtlaması* için `id` yol parametresi. Rota kısıtlamalarını uygulamak `IRouteConstraint` ve onları doğrulamak için rota değerlerini denetleyin. Bu örnekte, rota değeri `id` tamsayıya dönüştürülebilir olmalıdır. Bkz: [rota kısıtlaması başvurusu](#route-constraint-reference) için framework tarafından sağlanan rota kısıtlamalarını daha ayrıntılı bir açıklaması.

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

İpucu: satır içi söz dizimi kısıtlamaları ve varsayılan değerleri tanımlamak için basit yollar için daha kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçlerini gibi özellikleri vardır.

Bu örnek, birkaç daha fazla özellik göstermektedir:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Bu şablon bir URL yolu gibi eşleşecektir `/Blog/All-About-Routing/Introduction` ve değerleri ayıklar `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Varsayılan rota değerleri için `controller` ve `action` olsa bile karşılık gelen hiçbir rota parametrelerinin şablonda rota tarafından üretilir. Varsayılan değerleri, rota şablonu belirtilebilir. `article` Rota parametresi olarak tanımlanmış olan bir *tümünü* yıldız görünümünü tarafından `*` rota parametre adından önce. Tümünü rota parametrelerinin URL yolu geri kalanında yakalama ve boş bir dize da eşleştirebilirsiniz.

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

<a name="id1"></a>

### <a name="url-generation"></a>URL üretimi

`Route` Sınıfı da gerçekleştirebilirsiniz URL üretimi bir dizi rota değerleri, rota şablonu ile birleştirerek. Bu mantıksal olarak URL yolu eşleşen ters işlemidir.

İpucu: URL üretimi daha iyi anlamak için oluşturmak ve ardından söz konusu URL rota şablonu nasıl BC hakkında düşünmek için hangi URL'yi düşünün. Hangi değerleri üretilen? Bu, URL üretimi nasıl çalıştığını kabaca eşdeğeridir `Route` sınıfı.

Bu örnek, temel bir ASP.NET MVC stili rota kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Rota değerleri ile `{ controller = Products, action = List }`, bu yönlendirme URL'sini oluşturur `/Products/List`. Rota değerleri, URL yolu oluşturmak karşılık gelen rota parametrelerinin yerini alır. Bu yana `id` isteğe bağlıdır parametre yol, bir değer yok sorun değil.

Rota değerleri ile `{ controller = Home, action = Index }`, bu yönlendirme URL'sini oluşturur `/`. Bu değerleri karşılık gelen segmentleri güvenli bir şekilde atlanabilir için sağlanan yol değerleri varsayılan değerleriyle eşleşir. Oluşturulan her iki URL bu rota tanımıyla gidiş dönüş olur ve URL'yi oluşturmak için kullanılan aynı rota değerlerini üretmek unutmayın.

İpucu: ASP.NET MVC kullanarak uygulama kullanması gereken `UrlHelper` doğrudan yönlendirme içine çağırmak yerine URL'lerini oluşturmak için.

URL oluşturma işlemi hakkında daha fazla ayrıntı için bkz. [url üretimi başvuru](#url-generation-reference).

## <a name="using-routing-middleware"></a>Yönlendirme ara yazılımın kullanılması

"Microsoft.AspNetCore.Routing" NuGet paketini ekleyin.

Hizmet kapsayıcısını yönlendirme ekleme *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Yollar yapılandırılmalıdır `Configure` yönteminde `Startup` sınıfı. Aşağıdaki örnek bu API'leri kullanır:

* `RouteBuilder`
* `Build`
* `MapGet`  Yalnızca HTTP GET isteklerini eşleşir
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

Aşağıdaki tabloda, belirli bir URI'leri ile yanıtlarını gösterir.

| URI | Yanıt  |
| ------- | -------- |
| /Package/Create/3  | Merhaba! Rota değerleri: [işlem oluşturun], [ID, 3] |
| / /-3 paketi/parçası  | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3] |
| / Paket / /-3 izleme / | Merhaba! Rota değerleri: [işlem, izleme], [ID, -3]  |
| / Package/izleme / | \<Eşleşme, kalan > |
| /Hello/Joe Al | Merhaba, ALi! |
| POST /hello/Joe | \<Geçiş, yalnızca eşleşen HTTP GET > |
| /Hello/Joe/Smith Al | \<Eşleşme, kalan > |

Tek bir yol yapılandırıyorsanız, çağrı `app.UseRouter` öğesinde geçen bir `IRouter` örneği. Çağrı gerekmez `RouteBuilder`.

Framework yolları gibi oluşturmak için genişletme yöntemleri sağlar:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Bu yöntemler gibi bazı `MapGet` gerektiren bir `RequestDelegate` sağlanacak. `RequestDelegate` Olarak kullanılacak *rota işleyiciye* zaman yol ile eşleşen. Rota işleyiciye kullanılacak bir ara yazılım ardışık düzenini yapılandırmak bu ailesindeki diğer yöntemleri sağlar. Varsa *harita* değil accept yöntemi bir işleyici gibi `MapRoute`, kullanacağı sonra `DefaultHandler`.

`Map[Verb]` Yöntemleri kısıtlamaları HTTP fiili yöntem adı için rota sınırlamak için kullanın. Örneğin, [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) ve [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Küme ayraçları içinde belirteçleri (`{ }`) tanımlamak *rota parametreleri* hangi bağlı rotanın eşleşmesi durumunda. İçindeki bir yol parçasının birden fazla rota parametresini tanımlayabilirsiniz, ancak bir değişmez değer ayrılmalıdır. Örneğin `{controller=Home}{action=Index}` arasında herhangi bir değişmez değer olduğundan geçerli bir yol olmazdı `{controller}` ve `{action}`. Bu rota parametrelerinin bir adı olmalıdır ve ek öznitelikler belirtmiş olabilirsiniz.

Metin rota parametrelerinin dışında (örneğin, `{id}`) ve yol ayırıcısı `/` URL metin eşleşmesi gerekir. Eşleşen metin büyük/küçük harfe ve URL yolunu kodu çözülmüş gösterimini göre. Değişmez değer rota parametresi sınırlayıcı eşleştirilecek `{` veya `}`, karakter tekrarlayarak kaçış (`{{` veya `}}`).

İsteğe bağlı dosya uzantısına sahip bir dosya adı yakalama girişimi URL desenleri ek hususlar vardır. Örneğin, şablonu kullanarak `files/{filename}.{ext?}` - hem `filename` ve `ext` var, her iki değer doldurulur. Yalnızca `filename` URL'nin yol eşleşmeleri var olduğundan sondaki nokta `.` isteğe bağlıdır. Aşağıdaki URL'ler bu rota eşleşmesi:

* `/files/myFile.txt`
* `/files/myFile`

Kullanabilirsiniz `*` karakter öneki olarak URI - rest'e bağlamak için bir rota parametresini için bu çağrılan bir *tümünü* parametresi. Örneğin, `blog/{*slug}` kullanmaya herhangi bir URL BC `/blog` ve aşağıdaki herhangi bir değer vardı (hangi atanmış için `slug` rota değeri). Genel parametreleri da boş dize eşleşebilir.

Rota parametrelerine sahip olabilir *varsayılan değerler*, ayrılmış parametre adından sonra varsayılan belirterek belirli bir `=`. Örneğin, `{controller=Home}` tanımlarsınız `Home` için varsayılan değer olarak `controller`. Hiçbir değer parametresi için URL'de varsa varsayılan değer kullanılır. Rota parametrelerinin varsayılan değerleri ek olarak, isteğe bağlı olabilir (ekleyerek belirtilen bir `?` parametre adı, giriş olarak sonuna `id?`). İsteğe bağlı arasındaki farkı ve "olan varsayılan" olan bir rota parametresini varsayılan değeri her zaman bir değer; üretir yalnızca sağlandığında isteğe bağlı parametresi bir değer içeriyor.

Rota parametrelerine URL'den bağlı rota değerle eşleşmelidir kısıtlamaları da olabilir. İki nokta ekleme `:` ve rota parametresi adını belirttikten sonra kısıtlama adı bir *satır içi kısıtlama* bir rota parametresini üzerinde. Kısıtlama bunlar ayraç içinde sağlanan bağımsız değişkenleri gerektiriyorsa `( )` sonra kısıtlama adı. Başka bir iki nokta üst üste ekleyerek, birden çok satır içi kısıtlamalarını belirtilebilir `:` ve kısıtlama adı. Kısıtlama adı geçirilir `IInlineConstraintResolver` bir örneğini oluşturmak için hizmet `IRouteConstraint` URL işlenmesinde kullanılacak. Örneğin, rota şablonu `blog/{article:minlength(10)}` belirtir `minlength` kısıtlaması bağımsız değişkeniyle `10`. Daha fazla açıklama rota kısıtlamalarını ve kısıtlamaları framework tarafından sağlanan bir listesi için bkz: [rota kısıtlaması başvurusu](#route-constraint-reference).

Aşağıdaki tabloda, bazı rota şablonlarının ve davranışları gösterir.

| Rota şablonu | Örnek URL eşleştirme | Notlar |
| -------- | -------- | ------- |
| Merhaba  | / Merhaba  | Yalnızca tek bir yol ile eşleşir `/hello` |
| {Page=Home} | / | Eşleşen ve ayarlar `Page` için `Home` |
| {Page=Home}  | / Contact  | Eşleşen ve ayarlar `Page` için `Contact` |
| {Denetleyici} / {eylem} / {id}? | / Ürünler/listesi | Eşlendiği `Products` denetleyicisi ve `List` eylemi |
| {Denetleyici} / {eylem} / {id}? | / Ürünler/Ayrıntılar/123  |  Eşlendiği `Products` denetleyicisi ve `Details` eylem.  `id` 123 için ayarlayın |
| {controller=Home}/{action=Index}/{id?} | /  |  Eşlendiği `Home` denetleyicisi ve `Index` yöntemi `id` göz ardı edilir. |

Şablon kullanarak genel yönlendirme en basit yaklaşımdır. Kısıtlamaları ve varsayılan rota şablonu dışında da belirtilebilir.

İpucu: Etkinleştirme [günlüğü](xref:fundamentals/logging/index) görmek için nasıl yönlendirme uygulamalarında gibi yerleşik `Route`, eşleşen istekleri.

## <a name="reserved-routing-names"></a>Yönlendirme ayrılmış adlar

Aşağıdaki anahtar sözcükler ayrılmış adları ve rota adları veya parametre kullanılamaz:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Rota kısıtlaması başvurusu

Rota kısıtlamalarını yürütme bir `Route` eşleşen gelen URL söz dizimini ve rota değerlerini ve URL yolunu simgeleştirilmiş. Rota kısıtlamalarını genellikle rota şablonu ile ilişkili yol değişkenlerinizin ve basit bir Evet/Hayır olup olmadığı kabul edilebilir değerdir hakkında karar olun. Bazı rota kısıtlamalarını isteğin yönlendirildiği olup olmadığını göz önünde bulundurun için rota değeri dışındaki verileri kullanın. Örneğin, `HttpMethodRouteConstraint` kabul edebilir veya kendi HTTP fiiline bağlı bir isteği reddetmek.

>[!WARNING]
> Kısıtlamaları kullanmaktan kaçının **giriş doğrulama**, bunu geçersiz giriş yapan bir 404 (bulunamadı) yerine uygun hata iletisiyle bir 400 sonuçlanır. Rota kısıtlamalarını için kullanılan **belirsizliğinin ortadan kaldırılmasını** arasında belirli bir rota için girişler değil doğrulamak için benzer yollar.

Aşağıdaki tabloda, bazı rota kısıtlamalarını ve bunların beklenen bir davranış gösterir.

| kısıtlama | Örnek | Örnek eşleşmeleri | Notlar |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Herhangi bir tamsayı ile eşleşir |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Eşleşme `true` veya `false` (büyük-küçük harf duyarsız) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Geçerli bir eşleşen `DateTime` değeri (uyarı sabit kültür - bakın) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir eşleşen `decimal` değeri (uyarı sabit kültür - bakın) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `double` değeri (uyarı sabit kültür - bakın) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `float` değeri (uyarı sabit kültür - bakın) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli bir eşleşen `Guid` değeri |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir eşleşen `long` değeri |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize en fazla 8 karakter uzunluğunda olmalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8, en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | En az 18 tamsayı değeri olmalıdır |
| `max(value)` | `{age:max(120)}` |  `91` | En fazla 120 tamsayı değeri olmalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | En az 18 ancak en fazla 120 tamsayı değeri olmalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize, bir veya daha fazla alfabetik karakter oluşmalıdır (`a`-`z`, büyük küçük harf duyarsız) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize, normal ifadeyle eşleşmesi gerekir (bkz. normal bir ifadeyi tanımlama hakkında ipuçları) |
| `required`  | `{name:required}` | `Rick` |  Parametresi olmayan değer URL oluşturma sırasında mevcut olduğunu uygulamak için kullanılan |

Birden çok, tek bir parametre için virgülle ayrılmış kısıtlamalar uygulanabilir. Örneğin, aşağıdaki kısıtlama parametresi için 1 veya daha büyük bir tamsayı değeri kısıtlar:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

>[!WARNING]
> URL'yi doğrulamak rota kısıtlamalarını CLR türüne dönüştürülebilir (gibi `int` veya `DateTime`) her zaman sabit kültür kullanan - URL yerelleştirilemeyen varsayılır. Framework tarafından sağlanan rota kısıtlamaları, rota değerleri depolanan değerleri değiştirmeyin. URL'den ayrıştırılmış tüm rota değerleri dize olarak depolanır. Örneğin, [Float rota kısıtlaması](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) rota değeri kayana dönüştürülecek deneyecek, ancak yalnızca bu dönüştürülebilir bir kayan nokta doğrulamak için kullanılan dönüştürülen değer.

## <a name="regular-expressions"></a>Normal ifadeler

ASP.NET Core framework ekler `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` normal ifade oluşturucusu. Bkz: [RegexOptions numaralandırma](/dotnet/api/system.text.regularexpressions.regexoptions) bu üyeleri bir açıklaması.

Normal ifadeler, sınırlayıcılar ve Yönlendirme ve C# dili tarafından kullanılan benzer belirteçleri kullanın. Normal ifade belirteçleri Atlanan gerekir. Örneğin, normal ifade kullanılacak `^\d{3}-\d{2}-\d{4}$` sahip olması gerekir, yönlendirme `\` olarak yazdığınız karakterler `\\` atlamak için C# kaynak dosyadaki `\` dize kaçış karakteri (kullanmadığınız sürece [verbatim dize değişmez değerleri](/dotnet/csharp/language-reference/keywords/string). `{` , `}` , ' [' Ve ']' karakter bunları yönlendirme parametresi sınırlayıcı karakter kaçış karakterinin yinelenmesiyle gerekir.  Aşağıdaki tabloda, normal bir ifade ve kaçan sürümünü gösterir.

| İfade               | Not |
| ----------------- | ------------ |
| `^\d{3}-\d{2}-\d{4}$` | Normal ifade |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Kaçış  |
| `^[a-z]{2}$` | Normal ifade |
| `^[[a-z]]{{2}}$` | Kaçış  |

Yönlendirme, kullanılan normal ifadeler ile genellikle başlayacak `^` karakter (eşleşme dizenin konumdan başlayarak) ve bitiş ile `$` karakter (eşleşme dizenin konumunu biten). `^` Ve `$` karakter tüm rota parametre değeri normal ifade eşleştirmesi sağlar. Olmadan `^` ve `$` karakterler normal ifade genellikle istemezsiniz olan dize içindeki herhangi bir alt dizenin eşleşir. Aşağıdaki tablo bazı örnekler gösterilmektedir ve neden aynı veya eşleştirilecek başarısız açıklar.

| İfade               | Dize | Eşleştirme | Yorum |
| ----------------- | ------------ |  ------------ |  ------------ |
| `[a-z]{2}` | Merhaba | Evet | alt dize eşleşmeleri |
| `[a-z]{2}` | 123abc456 | Evet | alt dize eşleşmeleri |
| `[a-z]{2}` | mz | Evet | ifadeyle eşleşiyor |
| `[a-z]{2}` | MZ | Evet | büyük/küçük harfe duyarlı değil |
| `^[a-z]{2}$` |  Merhaba | Yok | bkz: `^` ve `$` yukarıda |
| `^[a-z]{2}$` |  123abc456 | Yok | bkz: `^` ve `$` yukarıda |

Başvurmak [.NET Framework normal ifadelerinde](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) normal ifade söz dizimi hakkında daha fazla bilgi için.

Olası değerler bilinen bir dizi parametre sınırlamak için normal bir ifade kullanın. Örneğin `{action:regex(^(list|get|create)$)}` yalnızca eşleşen `action` yönlendirmek için değer `list`, `get`, veya `create`. Dize kısıtlamaları sözlük geçirilen "^ (liste | get | oluşturma) $" eşdeğer olacaktır. Bilinen kısıtlamalardan biri eşleşmiyor (hizalı şablon içinde değil) kısıtlamaları sözlükteki geçirilen kısıtlamalar da normal ifadeler şeklinde kabul edilir.

## <a name="url-generation-reference"></a>URL üretimi başvurusu

Aşağıdaki örnekte, rota değerleri sözlüğü belirtilen bir rotaya bir bağlantı oluşturmak gösterilir ve `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Yukarıdaki örnek sonunda oluşturulmuş `/package/create/123`.

İkinci parametresi `VirtualPathContext` oluşturucudur koleksiyonu *ortam değerlerini*. Ortam değerleri, belirli bir istek bağlamı içinde bir geliştirici belirtmelisiniz değerlerin sayısını sınırlayarak kolaylık sağlar. Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir. Açıksa Örneğin, bir ASP.NET MVC uygulamasında `About` eylemi `HomeController`, bağlanmak için denetleyici rota değeri belirtmeniz gerekmez `Index` eylem (ortam değerini `Home` kullanılır).

Bir parametre eşleşmeyen ortam değerleri yoksayılır ve ortam değerleri bir açıkça tarafından sağlanan değer, soldan sağa doğru giden geçersiz kıldığında da yoksayılır URL.

Açıkça sağlanan ancak, herhangi bir şey eşleşmeyen değerler sorgu dizesine eklenir. Rota şablonu kullanılırken sonucu aşağıdaki tabloda gösterilmiştir `{controller}/{action}/{id?}`.

| Ortam değerleri | Açık değerler | Sonuç |
| -------------   | -------------- | ------ |
| Denetleyici = "Home" | Eylem "Hakkında" = | `/Home/About` |
| Denetleyici = "Home" | Denetleyici = "Order", Eylem "Hakkında" = | `/Order/About` |
| Denetleyici "Home", renk = = "Red" | Eylem "Hakkında" = | `/Home/About` |
| Denetleyici = "Home" | Eylem = "Hakkında", renk = "Red" | `/Home/About?color=Red`

Bir rota parametresi gelmiyor varsayılan bir değer varsa ve bu değeri açıkça sağlanır, bu varsayılan değer eşleşmelidir. Örneğin:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Denetleyici ve eylem için eşleşen değerler sağlandığında bağlama oluşturmayı yalnızca bu yol için bir bağlantı oluşturur.
