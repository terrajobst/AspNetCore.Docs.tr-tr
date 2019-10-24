---
title: ASP.NET Core Razor Pages yol ve uygulama kuralları
author: guardrex
description: Yönlendirme ve uygulama modeli sağlayıcısı kurallarının sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemenize nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779211"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core Razor Pages yol ve uygulama kuralları

[Luke Latham](https://github.com/guardrex) tarafından

Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.

Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.

Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametreler eklemek için, sayfanın `@page` yönergesini kullanın. Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).

Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır. Daha fazla bilgi için bkz. [Yönlendirme: ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

| Senaryo | Örnek gösterilmektedir... |
| -------- | --------------------------- |
| [Model kuralları](#model-conventions)<br><br>Kurallar. Add<ul><li>Ipageroutemodelconvention</li><li>Ipageapplicationmodelconvention</li><li>Ipagehandlermodelconvention</li></ul> | Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin. |
| [Sayfa yolu eylem kuralları](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin. |
| [Sayfa modeli eylem kuralları](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</li></ul> | Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin. |

Razor Pages kuralları, `Startup` sınıfında hizmet koleksiyonuna <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> uzantısı yöntemi kullanılarak eklenir ve yapılandırılır. Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a>Rota sırası

Rotalar işleme için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> belirtir (rota eşleştirme).

| Siparişi            | Davranış |
| :--------------: | -------- |
| -1               | Yol, diğer rotalar işlenmeden önce işlenir. |
| 0                | Sıra belirtilmemiş (varsayılan değer). @No__t_0 atanmazsa (`Order = null`), yolun işlenmek üzere 0 (sıfır) olarak `Order`. |
| 1, 2, &hellip; n | Yol işleme sırasını belirtir. |

Yol işleme, kurala göre belirlenir:

* Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).
* Yolların aynı `Order` olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.
* Aynı `Order` ve aynı parametre sayısı ile rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> eklendiği sırada işlenir.

Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının. Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer. İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakımını yapmak için kırıcı olur. Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın. Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir. Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışın.

Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır. MVC konularındaki yol sırasıyla ilgili bilgiler, [Denetleyici eylemlerine yönlendirme sırasında mevcuttur: öznitelik yollarını sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Model kuralları

Razor Pages için uygulanan [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> için bir temsilci ekleyin.

### <a name="add-a-route-model-convention-to-all-pages"></a>Tüm sayfalara bir rota modeli kuralı ekleme

Sayfa yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.

Örnek uygulama, uygulamadaki tüm sayfalara bir `{globalTemplate?}` Route şablonu ekler:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `1` olarak ayarlanmıştır. Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:

* @No__t_0 için bir yol şablonu konuya daha sonra eklenir. Iletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu nedenle `{globalTemplate?}` Route şablonundan önce eşleşir.
* Konunun ilerleyen kısımlarında `{aboutTemplate?}` yol şablonu eklenir. @No__t_0 şablonuna `2` `Order` verilir. @No__t_0 sayfası istendiğinde, `Order = 2` özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.
* Konunun ilerleyen kısımlarında `{otherPagesTemplate?}` yol şablonu eklenir. @No__t_0 şablonuna `2` `Order` verilir. *Sayfalar/diğer sayfalar* klasöründeki herhangi bir sayfa bir yol parametresiyle (örneğin, `/OtherPages/Page1/RouteDataValue`) istendiğinde, `Order = 2` özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.

Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

@No__t_0 ekleme gibi Razor Pages seçenekler, MVC `Startup.ConfigureServices` hizmet koleksiyonuna eklendiğinde eklenir. Örnek için bkz. [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

@No__t_0 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir. İşlenen sayfa, yol veri değerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Tüm sayfalara uygulama modeli kuralı ekleme

@No__t_0 kullanarak, sayfa uygulama modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ekleyin.

Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir. Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder. Bu değerler, yanıt üst bilgisini ayarlamak için `OnResultExecuting` yönteminde kullanılır. Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.

Örnek uygulama, uygulamadaki tüm sayfalara bir başlık, `GlobalHeader` eklemek için `AddHeaderAttribute` sınıfını kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Tüm sayfalara bir işleyici modeli kuralı ekleme

Sayfa işleyicisi modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnekleri koleksiyonuna bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> kullanın.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a>Sayfa yolu eylem kuralları

@No__t_0 türetilen varsayılan yol modeli sağlayıcısı, sayfa yollarını yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır.

### <a name="folder-route-model-convention"></a>Klasör Yönlendirme modeli kuralı

Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanın.

Örnek uygulama, *diğer sayfalar* klasöründeki sayfalara bir `{otherPagesTemplate?}` yol şablonu eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2` olarak ayarlanmıştır. Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1` olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar. *Sayfalar/otherpages* klasöründeki bir sayfa bir yol parametresi değeri (örneğin, `/OtherPages/Page1/RouteDataValue`) ile isteniyorsa, `Order = 2` özelliğinin ayarlanması nedeniyle "routedatavalue", `RouteData.Values["otherPagesTemplate"]` (`Order`) değil `RouteData.Values["globalTemplate"]` (`Order = 1`) olarak yüklenir.

Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir. İşlenen sayfa, yol veri değerlerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Sayfa yönlendirme modeli kuralı

Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> kullanın.

Örnek uygulama, hakkında sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için `AddPageRouteModelConvention` kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

@No__t_1 için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği `2` olarak ayarlanmıştır. Bu, tek bir rota değeri sağlandığında `{globalTemplate?}` (konuda daha önce `1` olarak ayarlanan) şablonunun ilk yol veri değeri konumu için öncelik verilmesini sağlar. @No__t_0 sayfasında yol parametresi değeri varsa, `Order = 2` özelliğinin ayarlanması nedeniyle "RouteDataValue" `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order`) değil.

Mümkün olan yerlerde, `Order` `Order = 0` sonuç olarak ayarlanmayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

@No__t_0 'de örneğin hakkında sayfasını isteyin ve sonucu inceleyin:

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir. İşlenen sayfa, yol veri değerlerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Sayfa yollarını özelleştirmek için bir parametre transformatörü kullanın

ASP.NET Core tarafından oluşturulan sayfa yolları, bir parametre transformatörü kullanılarak özelleştirilebilir. Bir parametre transformatörü `IOutboundParameterTransformer` uygular ve parametrelerin değerini dönüştürür. Örneğin, özel bir `SlugifyParameterTransformer` parametresi transformatörü `SubscriptionManagement` Route değerini `subscription-management` olarak değiştirir.

@No__t_0 Page Route model kuralı, bir uygulamadaki otomatik olarak oluşturulan sayfa yollarının klasör ve dosya adı kesimlerine bir parametre transformatörü uygular. Örneğin, */Pages/subscriptionmanagement/viewAll.exe* konumundaki Razor Pages dosyasında yol `/SubscriptionManagement/ViewAll` `/subscription-management/view-all` olarak yeniden yazılabilir.

`PageRouteTransformerConvention`, yalnızca Razor Pages klasöründen ve dosya adından gelen bir sayfa yolunun otomatik olarak oluşturulan segmentlerini dönüştürür. @No__t_0 yönergesiyle eklenen yol kesimlerini dönüştürmez. Kural, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> eklenen yolları da dönüştürmez.

@No__t_0, `Startup.ConfigureServices` bir seçenek olarak kaydedilir:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a>Sayfa yolu yapılandırma

Belirtilen sayfa yolundaki bir sayfaya bir yol yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> kullanın. Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır. `AddPageRoute`, yolu oluşturmak için `AddPageRouteModelConvention` kullanır.

Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

Iletişim sayfasına, varsayılan yolu aracılığıyla `/Contact` de erişilebilir.

Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` yol segmentine (`{text?}`) izin verir. Bu sayfa, ziyaretçinin `/Contact` rotasında sayfaya erişmesi durumunda `@page` yönergesinde bu isteğe bağlı segmenti de içerir:

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

@No__t_0, kendi sıradan yönlendirmekte olan kişi sayfasını ziyaret edin, veya özel yol `/TheContactPage`. Ek bir `text` yol kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek. İşlenmiş sayfa ' metin ' segmenti değerini gösterir.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Sayfa modeli eylem kuralları

@No__t_0 uygulayan varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır. Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.

Bu bölümdeki örneklerde örnek uygulama, yanıt üst bilgisini uygulayan bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> `AddHeaderAttribute` sınıfı kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.

**Klasör uygulama modeli kuralı**

Belirtilen klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklerine bir eylem çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> kullanın.

Örnek, uygulamanın *diğer sayfalar* klasörünün içindeki sayfalara bir başlık, `OtherPagesHeader` ekleyerek `AddFolderApplicationModelConvention` kullanımını gösterir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Sayfa uygulama modeli kuralı**

Belirtilen ada sahip sayfanın <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> bir eylemi çağıran bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oluşturmak ve eklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> kullanın.

Örnek, hakkında sayfasına `AboutHeader` bir başlık ekleyerek `AddPageApplicationModelConvention` kullanımını gösterir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

**Filtre yapılandırma**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen filtreyi uygulamak üzere yapılandırır. Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır. Koşul geçerse, bir üst bilgi eklenir. Aksi takdirde, `EmptyFilter` uygulanır.

`EmptyFilter` bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters). Eylem filtreleri Razor Pages tarafından yoksayıldığından, yolun `OtherPages/Page2` içermiyorsa `EmptyFilter` hiçbir etkisi yoktur.

@No__t_0 'de örneğin Page2 sayfasını isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

**Filtre fabrikası yapılandırma**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>, belirtilen fabrikayı tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulayacak şekilde yapılandırır.

Örnek uygulama, uygulamanın sayfalarına iki değer içeren `FilterFactoryHeader` bir üst bilgi ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanılmasına bir örnek sağlar:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

*AddHeaderWithFactory.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

@No__t_0 sırasında örneğin hakkında daha fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC filtreleri ve sayfa filtresi (ıpagefilter)

Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır. Diğer MVC filtresi türleri şunlardır: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters). Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.

Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir. Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
