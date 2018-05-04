---
title: ASP.NET çekirdek yönlendirme
author: ardalis
description: ASP.NET Core yönlendirme işlevini nasıl gelen istek yönlendirme işleyicisine eşlemek için sorumlu olduğu bulur.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: 2e1257639ec41f657093439c5245b50adbad34dc
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="routing-in-aspnet-core"></a>ASP.NET çekirdek yönlendirme

Tarafından [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Gelen istek yönlendirme işleyicisine eşlemek için yönlendirme işlevi sorumludur. Yollar ASP.NET uygulamasında tanımlanır ve uygulama başladığında yapılandırılmış. Bir rota, istekte bulunan URL'den değerleri isteğe bağlı olarak ayıklayabilirsiniz ve bu değerleri daha sonra isteği işleme için kullanılabilir. ASP.NET Uygulama yolu bilgileri kullanarak, yönlendirme işlevi de rota işleyicilere eşleme URL'leri oluşturabilir. Bu nedenle, yönlendirme bir URL veya yol işleyicisi bilgilere göre belirli bir rota işleyicisi karşılık gelen URL göre bir rota işleyiciye bulabilirsiniz.

>[!IMPORTANT]
> Bu belge yönlendirme düşük düzey ASP.NET Core kapsar. ASP.NET Core MVC yönlendirme için bkz: [rota denetleyici eylemleri için](../mvc/controllers/routing.md)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Yönlendirme Temelleri

Yönlendirme kullanır *yollar* (uygulamaları [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) için:

* gelen istekleri eşleme *rota işleyicileri*

* yanıtları kullanılan URL üretmek

Genellikle, bir uygulamanın tek bir rota koleksiyonu yok. Bir istek ulaştığında rota koleksiyonu sırada işlenir. Gelen istek çağırarak istek URL'si eşleşen bir yol arar `RouteAsync` rota koleksiyonu içinde kullanılabilir her rotada yöntemi. Bunun aksine, bir yanıt rota bilgilerine dayalı URL (örneğin, yeniden yönlendirme veya bağlantılar) üretmek için yönlendirme kullanın ve bu nedenle sabit kodlu URL'lere Bakımı yardımcı olan olmamasına özen gösterin.

Yönlendirme bağlı [ara yazılımı](xref:fundamentals/middleware/index) tarafından kanal `RouterMiddleware` sınıfı. [ASP.NET Core MVC](xref:mvc/overview) yapılandırmasına bir parçası olarak ara yazılım ardışık düzene yönlendirme ekler. Bir tek başına bileşeni olarak Yönlendirme kullanma hakkında bilgi edinmek için bkz: [yönlendirme ara yazılımı kullanarak](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>URL eşleştirme

URL ile eşleşen işlemidir tarafından hangi yönlendirme gönderileri gelen bir istek için bir *işleyici*. Bu işlem genellikle URL yolunu verilerde bağlıdır, ancak istekteki herhangi bir veri dikkate alınması gereken genişletilmiş. İşleyicileri ayırmak için istekleri gönderme olanağı, boyutu ve karmaşıklığı bir uygulamanın Ölçeklendirmesi anahtardır.

Gelen istekleri girin `RouterMiddleware`, çağıran `RouteAsync` dizisindeki her rotada yöntemi. `IRouter` Örneği seçer kullanılıp kullanılmayacağını *işlemek* ayarlayarak isteği `RouteContext.Handler` NULL olmayan `RequestDelegate`. Bir rota isteği için bir işleyici ayarlarsa, işlem durur ve işleyici rota isteğini işlemek için çağrılır. Tüm yollar denedi ve istek için hiçbir işleyici bulunamadı, ara yazılım çağırır *sonraki* ve istek ardışık düzende sonraki ara yazılımı çağrılır.

İçin birincil giriş `RouteAsync` olan `RouteContext.HttpContext` geçerli istekle ilişkilendirilmiş. `RouteContext.Handler` Ve `RouteContext.RouteData` bir rotayla eşleşen sonra ayarlanacak çıkışları şunlardır.

Bir eşleşme sırasında `RouteAsync` özelliklerini de ayarlayacaktır `RouteContext.RouteData` kadar gerçekleştirilen istek işlemenin göre uygun değerlere. Yol bir istek eşleşirse `RouteContext.RouteData` hakkındaki önemli durum bilgileri içeren *sonuç*.

`RouteData.Values` bir dictionary'si *rota değerleri* rotadaki üretilen. Bu değerler genellikle URL tokenizing tarafından belirlenir ve kullanıcı girişi kabul etmek veya daha fazla dağıtırken kararları uygulama için kullanılabilir.

`RouteData.DataTokens`  eşlenen rotaya ilgili ek veriler bir özellik paketi değil. `DataTokens` uygulama daha sonra rota dayalı kararlar şekilde her yol verilerle eşleşen özellikle görüntüyle durumunu desteklemek için sağlanır. Bu değerler geliştirici tarafından tanımlanan ve yapmak **değil** herhangi bir şekilde yönlendirme davranışını etkiler. Ayrıca, veri belirteçleri gizli değerleri dizeler gelen ve giden kolayca dönüştürülebilir olmalıdır rota değerleri aksine herhangi bir türde olabilir.

`RouteData.Routers` istek başarıyla eşleşen yer aldığı rotaları listesidir. Yollar iç içe geçirilemez birbirine içinde ve `Routers` özellik mantıksal ağacının bir eşleşme ile sonuçlandı yolların üzerinden yolu gösterir. Listedeki ilk öğe genellikle `Routers` rota koleksiyonu ve URL üretmek için kullanılmalıdır. Son öğenin `Routers` eşleşen rota işleyicisi.

### <a name="url-generation"></a>URL oluşturma

URL oluşturma işlemidir hangi yönlendirerek rota değerleri kümesine göre bir URL yolu oluşturabilirsiniz. Bu, işleyicileri ve bunlara erişim URL'leri arasında mantıksal ayrım sağlar.

URL üretmek için benzer bir yinelemeli işlem takip eder, ancak bu çağırma kullanıcı ya da framework kodu ile başlayan `GetVirtualPath` yöntemi rota koleksiyonu. Her *rota* sonra olacaktır, `GetVirtualPath` yöntemi null olmayan bir kadar sırayla adlı `VirtualPathData` döndürülür.

Birincil girdi için `GetVirtualPath` şunlardır:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Yollar öncelikle tarafından sağlanan rota değerleri kullanmak `Values` ve `AmbientValues` bir URL'yi oluşturmak mümkün olduğu ve ne eklenecek değerleri karar vermek için. `AmbientValues` Yönlendirme sistem geçerli istekle eşleşen üretilen rota değerleri kümesidir. Buna karşılık, `Values` geçerli işlem için istenen URL'yi oluşturmak nasıl belirtin rota değerlerdir. `HttpContext` Hizmetleri ya da geçerli bağlamla ilişkili ek veri almak bir yol ihtiyacı kalmadığında sağlanır.

İpucu: düşünmek `Values` geçersiz kılmalar için bir dizi olarak `AmbientValues`. URL üretmek için bağlantıları aynı yol ya da rota değerlerini kullanarak URL üretmek kolaylaştırmak için geçerli istek rota değerleri yeniden dener.

Çıktısını `GetVirtualPath` olan bir `VirtualPathData`. `VirtualPathData` paralel olarak olan `RouteData`; içerdiği `VirtualPath` rota tarafından ayarlanması bazı ek özellikler yanı sıra çıkış URL.

`VirtualPathData.VirtualPath` Özelliği içeren *sanal yol* rota tarafından üretilen. Gereksinimlerinize bağlı olarak yol daha fazla işlem gerekebilir. Örneğin, HTML oluşturulan URL'yi oluşturmak istiyorsanız, uygulamanın taban yolu başına gerekir.

`VirtualPathData.Router` Başarıyla URL oluşturulan rota başvurudur.

`VirtualPathData.DataTokens` Özellikleri olan bir URL oluşturulan rota ilgili ek veri sözlüğü. Paralel olarak budur `RouteData.DataTokens`.

### <a name="creating-routes"></a>Yollar oluşturma

Yönlendirme sağlar `Route` sınıf standart uygulaması olarak `IRouter`. `Route` kullanan *rota şablonu* karşı URL yolu ile eşleşir desenlerini tanımlamak için sözdizimi zaman `RouteAsync` olarak adlandırılır. `Route` aynı yol şablonu bir URL oluşturmak için kullanacağı zaman `GetVirtualPath` olarak adlandırılır.

Çoğu uygulama çağırarak yollar oluşturacak `MapRoute` veya tanımlanan benzer genişletme yöntemleri `IRouteBuilder`. Bu yöntemlerin örneğini oluşturacak `Route` ve rota koleksiyonuna ekleyin.

Not: `MapRoute` bir rota işleyicisi parametresini - almaz yalnızca tarafından işlenen rotalar ekler `DefaultHandler`. Varsayılan işleyici olduğundan bir `IRouter`, istek işleme değil karar verebilirsiniz. Örneğin, yalnızca işleme bir varsayılan işleyici eşleşen bir kullanılabilir denetleyici ve eylem istekleri gibi ASP.NET MVC genellikle yapılandırılır. MVC için yönlendirme hakkında daha fazla bilgi için bkz: [denetleyici eylemleri için rota](../mvc/controllers/routing.md).

Bu bir örnektir bir `MapRoute` tipik bir ASP.NET MVC rota tanımı tarafından kullanılan çağrısı:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Bu şablon bir URL yolu gibi eşleşir `/Products/Details/17` ve rota değerlerini ayıklayın `{ controller = Products, action = Details, id = 17 }`. Rota değerlerini ve URL yolunu kesimler halinde bölme ve her segment ile eşleşen tarafından belirlenen *parametresi rota* rota şablonu adı. Rota parametrelerine olarak adlandırılır. Parametre adı kaşlı ayraç içinde kapsayan tarafından tanımlı `{ }`.

Yukarıdaki şablonu URL yolunu eşleşen `/` ve değerleri msgıd `{ controller = Home, action = Index }`. Çünkü böyle `{controller}` ve `{action}` rota parametrelerinin varsayılan değerlere sahip ve `id` rota parametresinin isteğe bağlı. Bir eşittir `=` oturum ve ardından bir değerle sonra rota parametresi adı parametresi için varsayılan bir değer tanımlar. Bir soru işareti `?` sonra rota parametresi adı parametre isteğe bağlı olarak tanımlar. Rota parametrelerinin varsayılan bir değerle *her zaman* bir yönlendirme değeri üretmek rotayla eşleşen - isteğe bağlı parametreler hiçbir karşılık gelen URL yol kesimi ise bir yönlendirme değeri üretmek olmaz olduğunda.

Bkz: [rota şablon başvurusu](#route-template-reference) rota şablonu özellikleri ve sözdizimi kapsamlı açıklaması.

Bu örnek içeren bir *rota kısıtlaması*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Bu şablon bir URL yolu gibi eşleşir `/Products/Details/17`, ama `/Products/Details/Apples`. Rota parametresi tanımı `{id:int}` tanımlayan bir *rota kısıtlaması* için `id` parametre rota. Rota kısıtlamalarını uygulamak `IRouteConstraint` ve onları doğrulamak için rota değerleri inceleyin. Bu örnekte rota değeri `id` tamsayıya dönüştürülebilir olmalıdır. Bkz: [rota kısıtlaması başvurusu](#route-constraint-reference) framework tarafından sağlanan rota kısıtlamalarını daha ayrıntılı bir açıklaması için.

Ek aşırı `MapRoute` değerleri kabul `constraints`, `dataTokens`, ve `defaults`. Bu ek parametreler, `MapRoute` türü olarak tanımlanmış `object`. Bu parametre tipik kullanım, burada anonim tür eşlemesi özellik adlarını parametre adları yönlendirmek anonim olarak yazılmış bir nesne geçirmektir.

Aşağıdaki iki örnek eşdeğer yollar oluşturun:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

İpucu: kısıtlamaları ve Varsayılanları tanımlamak için satır içi sözdizimi basit yollar için daha kullanışlı olabilir. Ancak, satır içi sözdizimi tarafından desteklenmeyen veri belirteçleri gibi özellikler vardır.

Bu örnekte birkaç daha fazla özelliği gösterilmektedir:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Bu şablon bir URL yolu gibi eşleşir `/Blog/All-About-Routing/Introduction` ve değerlerini ayıklar `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Varsayılan rota değerleri için `controller` ve `action` olsa bile karşılık gelen hiçbir rota parametrelerinin şablonda rota tarafından üretilir. Varsayılan değerler rota şablonu belirtilebilir. `article` Rota parametresi olarak tanımlanmış bir *catch-tüm* yıldız görünümünü tarafından `*` rota parametresi adından önce. Catch tüm rota parametrelerinin URL yolunu kalanı yakalamak ve boş dize de eşleştirebilirsiniz.

Bu örnek, rota kısıtlamaları ve veri belirteçleri ekler:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Bu şablon bir URL yolu gibi eşleşir `/Products/5` ve değerlerini ayıklar `{ controller = Products, action = Details, id = 5 }` ve veri belirteçleri `{ locale = en-US }`.

![Yerel öğeler Windows belirteçleri](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>URL oluşturma

`Route` Sınıfı ayrıca gerçekleştirebilir URL oluşturma, rota şablonuyla bir rota değerleri kümesi birleştirerek. Bu mantıksal URL yolunu eşleşen ters işlemidir.

İpucu: URL nesil daha iyi anlamak için oluşturmak ve bu URL rota şablonu nasıl eşleşir hakkında düşünmek için istediğiniz hangi URL düşünün. Ne değerleri üretilen? Bu, URL nesil nasıl çalıştığını kabaca eşdeğerdir `Route` sınıfı.

Bu örnek, bir temel ASP.NET MVC stili rota kullanır:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Rota değerleri ile `{ controller = Products, action = List }`, bu yol URL'si oluşturur `/Products/List`. Rota değerlerini ve URL yolunu oluşturmak karşılık gelen rota parametrelerini yerine kullanılır. Bu yana `id` isteğe parametresi yol, bir değere sahip olmayan herhangi bir sorun olduğunu.

Rota değerleri ile `{ controller = Home, action = Index }`, bu yol URL'si oluşturur `/`. Bu değerlere karşılık gelen kesimleri güvenle yoksayılabilir şekilde sağlanan rota değerleri varsayılan değerleri aynı. Oluşturulan her iki URL'leri bu rota tanımıyla gidiş dönüş olur ve URL'yi oluşturmak için kullanılan aynı rota değerleri üretmek unutmayın.

İpucu: ASP.NET MVC kullanarak bir uygulama kullanması gereken `UrlHelper` doğrudan yönlendirme içine çağırmak yerine URL üretmek için.

URL oluşturma işlemi hakkında daha fazla ayrıntı için bkz: [url oluşturma başvurusu](#url-generation-reference).

## <a name="using-routing-middleware"></a>Yönlendirme ara yazılım kullanma

"Microsoft.AspNetCore.Routing" NuGet paketini ekleyin.

Hizmet kapsayıcısında yönlendirme eklemek *haline*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Yollar yapılandırılmalıdır `Configure` yönteminde `Startup` sınıfı. Aşağıdaki örnekte bu API'leri kullanır:

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

Aşağıdaki tabloda verilen URI'ler yanıtları gösterir.

| URI | Yanıt  |
| ------- | -------- |
| /Package/Create/3  | Merhaba! Rota değerleri: [işlemi, oluşturmak], [kimliği, 3] |
| / /-3 Paket/İzle  | Merhaba! Rota değerleri: [işlem, izleme], [kimliği, -3] |
| / paketini / /-3 izlemek / | Merhaba! Rota değerleri: [işlem, izleme], [kimliği, -3]  |
| /Package/İzle / | \<Eşleşme aracılığıyla, kalan > |
| /Hello/Joe Al | Merhaba, CAN! |
| POST /hello/Joe | \<Atlayabilir, HTTP GET yalnızca eşleşen > |
| /Hello/Joe/Smith Al | \<Eşleşme aracılığıyla, kalan > |

Tek bir yol yapılandırıyorsanız, çağrı `app.UseRouter` tümleştirilmesidir bir `IRouter` örneği. Çağrı gerekmez `RouteBuilder`.

Framework yolları gibi oluşturmak için genişletme yöntemleri kümesini sağlar:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Bu yöntemlerin gibi bazı `MapGet` gerektiren bir `RequestDelegate` sağlanmalıdır. `RequestDelegate` Olarak kullanılan *rota işleyiciye* zaman rotayla eşleşen. Bu ailedeki diğer yöntemleri, yol işleyicisi olarak kullanılacak bir ara yazılım ardışık düzenini yapılandırma olanak verir. Varsa *harita* değil accept yöntemi bir işleyici gibi `MapRoute`, kullanacağı sonra `DefaultHandler`.

`Map[Verb]` Yöntemleri kısıtlamaları yöntem adı HTTP fiili rotaya sınırlamak için kullanın. Örneğin, [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) ve [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Rota şablonu başvurusu

Süslü ayraçlar içinde belirteçleri (`{ }`) tanımlamak *rota parametreleri* hangi bağlı rotanın eşleşmesi durumunda. Birden fazla yol parametresi bir yol kesimi tanımlayabilirsiniz, ancak bir hazır değer ile ayrılmalıdır. Örneğin `{controller=Home}{action=Index}` arasında değişmez değer olduğundan geçerli bir rota olmayacaktır `{controller}` ve `{action}`. Bu rota parametrelerinin bir adı olması gerekir ve ek öznitelikler belirtmiş olabilirsiniz.

Rota parametrelerine dışında düz metin (örneğin, `{id}`) ve yol ayırıcısı `/` URL metinde eşleşmesi gerekir. Eşleşen metin URL'leri yolu kodu çözülmüş gösterimini dayalı ve büyük küçük harfe duyarsızdır. Değişmez değer rota parametresi sınırlayıcı eşleşecek şekilde `{` veya `}`, karakteri tekrarlayarak kaçış (`{{` veya `}}`).

Bir isteğe bağlı bir dosya uzantılı bir dosya adı yakalama girişimi URL desenlerini dikkat edilecek diğer noktalar vardır. Örneğin, şablonu kullanarak `files/{filename}.{ext?}` - her ikisi de `filename` ve `ext` var, her iki değerin doldurulur. Yalnızca `filename` URL, yol eşleşmeleri var olduğundan, sonundaki nokta `.` isteğe bağlıdır. Aşağıdaki URL'ler bu rota eşleşir:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

Kullanabileceğiniz `*` öneki olarak karakter - URI geri kalanı için bağlamak için bir rota parametresini için bu çağrılır bir *catch tüm* parametresi. Örneğin, `blog/{*slug}` kullanmaya herhangi bir URL eşleşir `/blog` ve sonrasında herhangi bir değer (hangi atanabilir için `slug` rota değeri). Catch tüm parametreleri da boş dize eşleştirebilirsiniz.

Rota parametrelerine sahip olabilir *varsayılan değerlerin*, ayrılmış parametre adı sonra varsayılan belirterek belirli bir `=`. Örneğin, `{controller=Home}` tanımlarsınız `Home` için varsayılan değer olarak `controller`. Hiçbir değer parametresi için URL'de varsa, varsayılan değer kullanılır. Rota parametrelerinin varsayılan değerleri ek olarak, isteğe bağlı olabilir (ekleyerek belirtilen bir `?` parametre adı olarak da sonuna `id?`). İsteğe bağlı arasındaki farkı ve "olan varsayılan" bir rota parametresini varsayılan bir değerle her zaman bir değer; üretir yalnızca sağlandığında isteğe bağlı bir parametre bir değere sahip.

Rota parametrelerine URL'den bağlı rota değeriyle eşleşmelidir kısıtlamaları da sahip olabilirsiniz. İki nokta ekleme `:` ve rota parametresi adı belirttikten sonra kısıtlama adı bir *satır içi kısıtlama* bir rota parametresini üzerinde. Kısıtlama olanlar ayraç içinde sağlanan bağımsız değişkenleri gerektiriyorsa `( )` sonra kısıtlama adı. Başka bir iki nokta üst üste ekleyerek, birden çok satır içi kısıtlama belirtilebilir `:` ve kısıtlama adı. Kısıtlama adı geçirilir `IInlineConstraintResolver` bir örneğini oluşturmak için hizmet `IRouteConstraint` URL işlenirken kullanılacak. Örneğin, rota şablonu `blog/{article:minlength(10)}` belirtir `minlength` kısıtlaması bağımsız değişkeni ile `10`. Daha fazla açıklama rota kısıtlamaları ve framework tarafından sağlanan kısıtlamaları bir listesi için bkz [rota kısıtlaması başvurusu](#route-constraint-reference).

Aşağıdaki tabloda bazı rota şablonlarının ve davranışlarını gösterir.

| Rota şablonu | Örnek URL eşleştirme | Notlar |
| -------- | -------- | ------- |
| Merhaba  | / Merhaba  | Yalnızca tek bir yol ile eşleşir `/hello` |
| {Page=Home} | / | Eşleşen ve ayarlar `Page` için `Home` |
| {Page=Home}  | / Başvurun  | Eşleşen ve ayarlar `Page` için `Contact` |
| {controller} / {action} / {id?} | / Ürünler/listesi | Eşlendiği `Products` denetleyicisi ve `List` eylem |
| {controller} / {action} / {id?} | / Ürünler/Ayrıntılar/123  |  Eşlendiği `Products` denetleyicisi ve `Details` eylem.  `id` için 123 ayarlayın |
| {controller=Home}/{action=Index}/{id?} | /  |  Eşlendiği `Home` denetleyicisi ve `Index` yöntemi; `id` göz ardı edilir. |

Bir şablon kullanarak genellikle yönlendirme en basit yaklaşımdır. Ayrıca kısıtlamaları ve varsayılan rota şablonu dışında belirtilebilir.

İpucu: Etkinleştirmek [günlüğü](xref:fundamentals/logging/index) görmek için nasıl yönlendirme uygulamalarında gibi yerleşik `Route`, eşleşen istekleri.

## <a name="route-constraint-reference"></a>Rota kısıtlaması başvurusu

Rota kısıtlamalarını yürütme bir `Route` gelen URL söz dizimini eşleşen ve rota değerleri URL yolunu simgeleştirilmiş. Rota kısıtlamalarını genellikle rota şablonu ilişkili rota değeri inceleyin ve Evet/Hayır olsun veya olmasın kabul edilebilir değerdir hakkında karar basit yapın. Bazı rota kısıtlamalarını istek yönlendirilen olup olmadığını düşünün için rota değeri dışında verileri kullanın. Örneğin, `HttpMethodRouteConstraint` kabul veya kendi HTTP fiiline bağlı bir istek reddedebilirsiniz.

>[!WARNING]
> Kısıtlamalarını kullanmaktan kaçının **giriş doğrulama**, bunu anlamına gelir, geçersiz giriş yapmak bir 404 (bulunamadı) yerine 400 uygun bir hata iletisi ile sonuçlanır. Rota kısıtlamalarını için kullanılan **belirsizliğini ortadan kaldırmak** belirli bir rota girişleri değil doğrulanacak benzer yolları arasında.

Aşağıdaki tabloda bazı rota kısıtlamaları ve bunların beklenen bir davranış gösterir.

| kısıtlama | Örnek | Örnek eşleşiyor | Notlar |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Herhangi bir tamsayı eşleşir |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Eşleşme `true` veya `false` (büyük küçük harf duyarlı) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Geçerli bir eşleşen `DateTime` değeri (sabit kültür - uyarı bakın) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Geçerli bir eşleşen `decimal` değeri (sabit kültür - uyarı bakın) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `double` değeri (sabit kültür - uyarı bakın) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Geçerli bir eşleşen `float` değeri (sabit kültür - uyarı bakın) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Geçerli bir eşleşen `Guid` değeri |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Geçerli bir eşleşen `long` değeri |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Dize en az 4 karakter olmalıdır |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Dize en fazla 8 karakter olmalıdır |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Dize tam olarak 12 karakter uzunluğunda olmalıdır |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Dize en az 8, en fazla 16 karakter uzunluğunda olmalıdır |
| `min(value)` | `{age:min(18)}` | `19` | En az 18 tamsayı değeri olmalıdır |
| `max(value)` | `{age:max(120)}` |  `91` | En fazla 120 tamsayı değeri olmalıdır |
| `range(min,max)` | `{age:range(18,120)}` | `91` | En az 18 ancak en fazla 120 tamsayı değeri olmalıdır |
| `alpha` | `{name:alpha}` | `Rick` | Dize, bir veya daha fazla alfabetik karakterlerden oluşmalıdır (`a`-`z`, büyük küçük harf duyarlı) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Dize normal ifade eşleşmesi gerekir (normal ifade tanımlama hakkında ipuçları bakın) |
| `required`  | `{name:required}` | `Rick` |  URL oluşturma sırasında bir parametre olmayan değer bulunduğunu uygulamak için kullanılan |

>[!WARNING]
> URL'yi doğrulayın rota kısıtlamalarını CLR türüne dönüştürülebilir (gibi `int` veya `DateTime`) her zaman sabit kültür kullanın - URL yerelleştirilemeyen olduğu varsayılır. Sağlanan framework rota kısıtlamalarını rota değerleri depolanan değerleri değiştirmeyin. URL'den Ayrıştırılan tüm rota değerleri, dize olarak depolanır. Örneğin, [Float rota kısıtlaması](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) rota değeri kayana dönüştürülecek deneyecek, ancak dönüştürülen değer yalnızca onu dönüştürülebilir kayana doğrulamak için kullanılır.

## <a name="regular-expressions"></a>Normal ifadeler 

ASP.NET Core framework ekler `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` normal ifade oluşturucusu. Bkz: [RegexOptions numaralandırma](/dotnet/api/system.text.regularexpressions.regexoptions) bu üyeleri açıklaması.

Normal ifadeler sınırlayıcıları ve Yönlendirme ve C# dili tarafından kullanılan benzer belirteçleri kullanın. Normal ifade belirteçleri kaçış uygulanmalıdır. Örneğin, normal ifade kullanmak için `^\d{3}-\d{2}-\d{4}$` olması gerekiyor, yönlendirme `\` olarak yazdığınız karakterler `\\` kaçınmak için C# kaynak dosyasında `\` dize kaçış karakteri (kullanmadığınız sürece [verbatim dize değişmez değerleri](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string). `{` , `}` , ' [' Ve ']' karakter yönlendirme parametre sınırlayıcı karakter kaçınmak için Katlama tarafından kaçış gerekir.  Aşağıdaki tabloda, normal bir ifade ve atlanan sürümünü gösterir.

| İfade               | Not |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Normal ifade |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Atlanan  |
| `^[a-z]{2}$` | Normal ifade |
| `^[[a-z]]{{2}}$` | Atlanan  |

Normal ifadeler yönlendirmeye kullanılan ile genellikle başlayacak `^` karakter (eşleşme dizesinin konumdan başlayarak) ve ile biten `$` karakter (dize konumunu bitiş eşleşir). `^` Ve `$` karakterleri tüm rota parametre değeri normal ifade eşleştirmesi sağlar. Olmadan `^` ve `$` karakter normal ifade istemezsiniz görülür dize içinde herhangi bir alt dize eşleşir. Aşağıdaki tablo bazı örnekler gösterir ve neden aynı veya eşleştirilecek başarısız açıklanmaktadır.

| İfade               | Dize | Eşleştirme | Yorum |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | Merhaba | Evet | eşleşmelerini |
| `[a-z]{2}` | 123abc456 | Evet | eşleşmelerini |
| `[a-z]{2}` | mz | Evet | ifadesiyle eşleşiyor |
| `[a-z]{2}` | MZ | Evet | değil büyük küçük harfe duyarlı |
| `^[a-z]{2}$` |  Merhaba | Yok | bkz: `^` ve `$` yukarıda |
| `^[a-z]{2}$` |  123abc456 | Yok | bkz: `^` ve `$` yukarıda |

Başvurmak [.NET Framework normal ifadeleri](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) normal ifade sözdizimi hakkında daha fazla bilgi için.

Olası değerler bilinen bir dizi parametre sınırlamak için normal ifade kullanın. Örneğin `{action:regex(^(list|get|create)$)}` yalnızca eşleşen `action` yönlendirmek için değer `list`, `get`, veya `create`. Dize kısıtlamaları sözlük aktarılırsa "^ (listesi | get | oluşturun) $" eşdeğer olacaktır. Bilinen kısıtlamalardan biri eşleşmeyen (hizalı şablon içinde değil) kısıtlamaları sözlükte geçirilen kısıtlamaları da normal ifadeler olarak kabul edilir.

## <a name="url-generation-reference"></a>URL oluşturma başvurusu

Aşağıdaki örnek, rota değerleri sözlüğü verilen yolu bir bağlantı oluşturmak nasıl gösterir ve `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Yukarıdaki örnek sonunda oluşturulmuş `/package/create/123`.

İkinci parametre `VirtualPathContext` Oluşturucusu koleksiyonudur *ortam değerleri*. Ortam değerleri, belirli bir istek bağlamı içinde bir geliştirici belirtmelisiniz değerlerin sayısını sınırlayarak kolaylık sağlar. Geçerli isteğin geçerli rota değerleri bağlantı oluşturma için ortam değerleri olarak kabul edilir. İçinde olup olmadığını Örneğin, bir ASP.NET MVC uygulamasında `About` eylemi `HomeController`, bağlantı sağlamak için denetleyici rota değeri belirtmeniz gerekmez `Index` eylemi (ortam değeri `Home` kullanılır).

Bir parametre eşleşmeyen ortam değerleri yoksayılır ve ortam değerleri de bir açıkça tarafından sağlanan değer, soldan sağa giderek değiştirdiğinde gözardı URL.

Açıkça sağlanır, ancak, herhangi bir şey eşleşmeyen değerleri sorgu dizesine eklenir. Rota şablonu kullanırken aşağıdaki tabloda sonuç gösterilmektedir `{controller}/{action}/{id?}`.

| Ortam değerleri | Açık değerler | Sonuç |
| -------------   | -------------- | ------ |
| Denetleyici = "Home" | Eylem "About" = | `/Home/About` |
| Denetleyici = "Home" | Denetleyici "Order", Eylem = "About" = | `/Order/About` |
| Denetleyici "Home", color = "Red" = | Eylem "About" = | `/Home/About` |
| Denetleyici = "Home" | Eylem = "Hakkında", renk = "Red" | `/Home/About?color=Red`

Bir rota parametresi karşılık gelmiyor varsayılan bir değeri yok ve bu değeri açıkça sağlanır, varsayılan değer eşleşmesi gerekir. Örneğin:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Denetleyici ve eylem için eşleşen değerler sağlandığında bağlantı oluşturma yalnızca bu yol için bir bağlantı oluşturur.
