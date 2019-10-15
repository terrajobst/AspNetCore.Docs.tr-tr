---
title: ASP.NET Core Razor Pages yol ve uygulama kuralları
author: guardrex
description: Yönlendirme ve uygulama modeli sağlayıcısı kurallarının sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemenize nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c93f169c422d260f738faba4812861521f383e51
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914971"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>ASP.NET Core Razor Pages yol ve uygulama kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Razor Pages uygulamalarında sayfa yönlendirmeyi, bulmayı ve işlemeyi denetlemek için sayfa [yolu ve uygulama modeli sağlayıcısı kurallarını](xref:mvc/controllers/application-model#conventions) nasıl kullanacağınızı öğrenin.

Ayrı sayfalar için özel sayfa yolları yapılandırmanız gerektiğinde, bu konunun ilerleyen kısımlarında açıklanan [Addpageroute kuralına](#configure-a-page-route) sahip sayfalara yönlendirmeyi yapılandırın.

Bir sayfa yolu belirtmek, yol kesimleri eklemek veya bir rotaya parametre eklemek için, sayfanın `@page` yönergesini kullanın. Daha fazla bilgi için bkz. [özel rotalar](xref:razor-pages/index#custom-routes).

Yol kesimleri veya parametre adları olarak kullanılamayan ayrılmış sözcükler vardır. Daha fazla bilgi için bkz [. yönlendirme: Ayrılmış yönlendirme adları](xref:fundamentals/routing#reserved-routing-names).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

| Senaryo | Örnek gösterilmektedir... |
| -------- | --------------------------- |
| [Model kuralları](#model-conventions)<br><br>Kurallar. Add<ul><li>Ipageroutemodelconvention</li><li>IPageApplicationModelConvention</li><li>Ipagehandlermodelconvention</li></ul> | Uygulamanın sayfalarına bir yol şablonu ve üst bilgi ekleyin. |
| [Sayfa yolu eylem kuralları](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Bir klasördeki sayfalara ve tek bir sayfaya rota şablonu ekleyin. |
| [Sayfa modeli eylem kuralları](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtre fabrikası)</li></ul> | Bir klasördeki sayfalara üst bilgi ekleyin, tek bir sayfaya üst bilgi ekleyin ve bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) yapılandırarak uygulamanın sayfalarına üst bilgi ekleyin. |

Razor Pages kurallar, <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> `Startup` sınıfındaki hizmet koleksiyonuna öğesine <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> genişletme yöntemi kullanılarak eklenir ve yapılandırılır. Aşağıdaki kural örnekleri bu konunun ilerleyen kısımlarında açıklanmıştır:

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

## <a name="route-order"></a>Rota sırası

Rotalar bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> işlem için (rota eşleştirme) belirtir.

| Sipariş verme            | Davranış |
| :--------------: | -------- |
| -1               | Yol, diğer rotalar işlenmeden önce işlenir. |
| 0                | Sıra belirtilmemiş (varsayılan değer). Atama `Order` değil (`Order = null`), işleme için `Order` varsayılan yolu 0 (sıfır) olarak belirler. |
| 1, 2, &hellip; n | Yol işleme sırasını belirtir. |

Yol işleme, kurala göre belirlenir:

* Yollar sıralı sırada işlenir (-1, 0, 1, 2, &hellip; n).
* Yollar aynı `Order`olduğunda, en belirli yol önce daha az özel yollarla eşleştirilir.
* Aynı `Order` ve aynı parametre sayısına sahip rotalar bir istek URL 'siyle eşleşiyorsa, rotalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>öğesine eklendikleri sırada işlenir.

Mümkünse, belirlenen bir yol işleme sırasına bağlı olarak kullanmaktan kaçının. Genellikle Yönlendirme, URL eşleştirme ile doğru yolu seçer. İstekleri doğru yönlendirmek için yol `Order` özelliklerini ayarlamanız gerekiyorsa, uygulamanın yönlendirme şeması büyük olasılıkla istemciler için kafa karıştırıcı olur ve bakım için kırıcı olur. Uygulamanın yönlendirme şemasını basitleştirecek şekilde arama yapın. Örnek uygulama, tek bir uygulama kullanarak birkaç yönlendirme senaryosunu göstermek için açık bir yol işleme sırası gerektirir. Ancak, üretim uygulamalarında rota `Order` ayarlama uygulamalarından kaçınmaya çalışmalısınız.

Razor Pages yönlendirme ve MVC denetleyici yönlendirme bir uygulamayı paylaşır. MVC konularındaki yol sırasıyla ilgili bilgiler, denetleyici eylemlerine yönlendirme [sırasında mevcuttur: Öznitelik yollarını](xref:mvc/controllers/routing#ordering-attribute-routes)sıralama.

## <a name="model-conventions"></a>Model kuralları

Razor Pages için uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> [model kuralları](xref:mvc/controllers/application-model#conventions) eklemek için bir temsilci ekleyin.

### <a name="add-a-route-model-convention-to-all-pages"></a>Tüm sayfalara bir rota modeli kuralı ekleme

Sayfa <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> yönlendirme modeli oluşturma sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnek koleksiyonu oluşturmak ve eklemek için kullanın.

Örnek uygulama, uygulamadaki tüm `{globalTemplate?}` sayfalara bir rota şablonu ekler:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

İçin <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> olarak`1`ayarlanır. Bu, örnek uygulamada aşağıdaki yol eşleştirme davranışını sağlar:

* İçin `TheContactPage/{text?}` bir yol şablonu, konusuna daha sonra eklenir. İletişim sayfası yolu, `null` (`Order = 0`) varsayılan sırasına sahiptir, bu `{globalTemplate?}` nedenle yol şablonundan önce eşleşir.
* Konuya `{aboutTemplate?}` daha sonra bir yol şablonu eklenir. `{aboutTemplate?}` Şablonuna`Order` bir verilebilir .`2` Hakkında sayfası `/About/RouteDataValue`istendiğinde,`Order = 2` `RouteData.Values["aboutTemplate"]` `RouteData.Values["globalTemplate"]` özelliğinayarlanması`Order` nedeniyle "routedatavalue", (`Order = 1`) içine () yüklenir.
* Konuya `{otherPagesTemplate?}` daha sonra bir yol şablonu eklenir. `{otherPagesTemplate?}` Şablonuna`Order` bir verilebilir .`2` *Sayfalar/diğer sayfalar* klasöründeki herhangi `/OtherPages/Page1/RouteDataValue`bir sayfa bir yol parametresiyle (örneğin,) istendiğinde, "routedatavalue`Order = 2` `RouteData.Values["globalTemplate"]` "`Order = 1` `RouteData.Values["otherPagesTemplate"]`, `Order` özelliği.

Mümkün olan `Order` `Order = 0`yerlerde, ' yi ayarlayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

' Deki <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> `Startup.ConfigureServices`hizmet koleksiyonuna MVC eklendiğinde ekleme gibi Razor Pages seçenekleri eklenir. Örnek için bkz. [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

Örnekteki hakkında daha `localhost:5000/About/GlobalRouteValue` fazla bilgi isteyin ve sonucu inceleyin:

![Hakkında sayfası, GlobalRouteValue 'un rota segmentiyle istenir. İşlenen sayfa, yol veri değerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Tüm sayfalara uygulama modeli kuralı ekleme

Oluşturma <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> ve sayfa uygulama modeli oluşturma <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnek koleksiyonuna eklemek için kullanın.

Bu ve diğer kuralları konunun ilerleyen kısımlarında göstermek için, örnek uygulama bir `AddHeaderAttribute` sınıfı içerir. Sınıf Oluşturucusu bir `name` dize ve bir `values` dize dizisi kabul eder. Bu değerler, `OnResultExecuting` bir yanıt üst bilgisi ayarlamak için yönteminde kullanılır. Tam sınıf, konusunun ilerleyen kısımlarında [sayfa modeli eylem kuralları](#page-model-action-conventions) bölümünde gösterilir.

Örnek uygulama, uygulama içindeki `AddHeaderAttribute` tüm sayfalara `GlobalHeader`üst bilgi eklemek için sınıfını kullanır:

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

Örnekteki hakkında daha `localhost:5000/About` fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri GlobalHeader 'ın eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Tüm sayfalara bir işleyici modeli kuralı ekleme

Sayfa <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> işleyici modelinin oluşturulması sırasında uygulanan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> örnek koleksiyonu oluşturmak ve eklemek için kullanın.

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

Sayfa yollarının yapılandırılması için genişletilebilirlik noktaları sağlamak üzere <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> tasarlanan, çağıran kurallarından türetilen varsayılan yol modeli sağlayıcısı.

### <a name="folder-route-model-convention"></a>Klasör Yönlendirme modeli kuralı

Belirtilen <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> klasörün altındaki tüm sayfalar <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> üzerinde bir eylem çağıran bir eylem oluşturmak ve eklemek için kullanın.

Örnek uygulama, <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> *diğer sayfalar* klasöründeki sayfalara `{otherPagesTemplate?}` bir yol şablonu eklemek için kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

İçin <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> olarak`2`ayarlanır. Bu, tek bir rota değeri `{globalTemplate?}` sağlandığında, ( `1`konusunda daha önce, konusunda daha önce olan) şablonun ilk yol veri değeri konumu için öncelik verilmesini sağlar. *Sayfalar/otherpages* `/OtherPages/Page1/RouteDataValue`klasöründeki bir sayfa bir rota parametresi değeri (örneğin,) ile isteniyorsa, "routedatavalue `RouteData.Values["globalTemplate"]` "`Order = 1`, `RouteData.Values["otherPagesTemplate"]` `Order = 2` `Order` özelliği.

Mümkün olan `Order` `Order = 0`yerlerde, ' yi ayarlayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` isteyin ve sonucu inceleyin:

![Diğer sayfalar klasöründeki Sayfa1, GlobalRouteValue ve OtherPagesRouteValue yol kesimiyle istenir. İşlenen sayfa, yol veri değerlerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Sayfa yönlendirme modeli kuralı

Sayfa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> için belirtilen ada sahip bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> eylemi çağıran bir eylem oluşturmak ve eklemek için kullanın.

Örnek uygulama, hakkında `AddPageRouteModelConvention` sayfasına bir `{aboutTemplate?}` yol şablonu eklemek için kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

İçin <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> olarak`2`ayarlanır. Bu, tek bir rota değeri `{globalTemplate?}` sağlandığında, ( `1`konusunda daha önce, konusunda daha önce olan) şablonun ilk yol veri değeri konumu için öncelik verilmesini sağlar. Hakkında sayfasında `/About/RouteDataValue`bir yol parametresi değeri varsa, `RouteData.Values["aboutTemplate"]` `Order = 2` `RouteData.Values["globalTemplate"]` özelliğin`Order` ayarlanması nedeniyle "routedatavalue" (`Order = 1`) içine () yüklenir.

Mümkün olan `Order` `Order = 0`yerlerde, ' yi ayarlayın. Doğru yolu seçmek için yönlendirmeyi güvenin.

Örnekteki hakkında daha `localhost:5000/About/GlobalRouteValue/AboutRouteValue` fazla bilgi isteyin ve sonucu inceleyin:

![GlobalRouteValue ve AboutRouteValue için rota kesimlerle ilgili sayfa hakkında bilgi istenir. İşlenen sayfa, yol veri değerlerinin sayfanın OnGet yönteminde yakalandığını gösterir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Sayfa yollarını özelleştirmek için bir parametre transformatörü kullanın

ASP.NET Core tarafından oluşturulan sayfa yolları, bir parametre transformatörü kullanılarak özelleştirilebilir. Bir parametre transformatörü, `IOutboundParameterTransformer` parametrelerinin değerini uygular ve dönüştürür. Örneğin, özel `SlugifyParameterTransformer` bir parametre transformatörü `SubscriptionManagement` rota değerini olarak `subscription-management`değiştirir.

`PageRouteTransformerConvention` Sayfa yolu modeli kuralı, bir uygulamadaki otomatik olarak oluşturulan sayfa yollarının klasör ve dosya adı kesimlerine bir parametre transformatörü uygular. Örneğin, */Pages/subscriptionmanagement/viewAll.exe* konumundaki Razor Pages dosyasında, yolu ' dan `/SubscriptionManagement/ViewAll` `/subscription-management/view-all`' a yeniden yazılır.

`PageRouteTransformerConvention`yalnızca Razor Pages klasörü ve dosya adından gelen bir sayfa yolunun otomatik olarak oluşturulan segmentlerini dönüştürür. `@page` Yönergeyle eklenen yol parçalarını dönüştürmez. Kural, tarafından <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>eklenen yolları da dönüştürmez.

, `PageRouteTransformerConvention` İçinde`Startup.ConfigureServices`bir seçenek olarak kaydedilir:

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

Belirtilen <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> sayfa yolundaki sayfaya bir yol yapılandırmak için kullanın. Sayfa için oluşturulan bağlantılar belirtilen rotayı kullanır. `AddPageRoute`yolu `AddPageRouteModelConvention` oluşturmak için kullanır.

Örnek uygulama, *Contact. cshtml*için `/TheContactPage` bir yol oluşturur:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

İletişim sayfasına, varsayılan yolu `/Contact` üzerinden de erişilebilir.

Örnek uygulamanın kişi sayfasına özel yolu, isteğe bağlı `text` bir yol segmentine (`{text?}`) izin verir. Bu sayfa, ziyaretçinin `@page` `/Contact` yolunda sayfaya erişmesi durumunda bu isteğe bağlı segmenti de içerir:

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

İşlenmiş sayfadaki **kişi** bağlantısı IÇIN oluşturulan URL 'nin güncelleştirilmiş yolu yansıttığını unutmayın:

![Gezinti çubuğundaki örnek uygulama Iletişim bağlantısı](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML 'deki kişi bağlantısının inceleniyor href 'in '/theContact tpage ' olarak ayarlandığını belirtir](razor-pages-conventions/_static/contact-link-source.png)

Kendi normal rotasında `/Contact`, veya özel `/TheContactPage`rotada kişi sayfasını ziyaret edin. Ek `text` bir rota kesimi sağlarsanız, sayfada sağladığınız HTML kodlu segment görüntülenir:

![Microsoft Edge tarayıcı, URL 'de ' TextValue ' öğesinin isteğe bağlı ' text ' yol segmentini sağlamaya yönelik örnek. İşlenmiş sayfa ' metin ' segmenti değerini gösterir.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Sayfa modeli eylem kuralları

Uygulayan <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> varsayılan sayfa modeli sağlayıcısı, sayfa modellerini yapılandırmak için genişletilebilirlik noktaları sağlamak üzere tasarlanan kuralları çağırır. Bu kurallar sayfa bulma ve işleme senaryolarını oluştururken ve değiştirirken yararlıdır.

Bu bölümdeki örneklerde, örnek uygulama bir yanıt üst bilgisi uygulayan olan `AddHeaderAttribute` bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>sınıfı kullanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

Kurallar kullanılarak, örnek bir klasördeki tüm sayfalara ve tek bir sayfaya özniteliğin nasıl uygulanacağını gösterir.

**Klasör uygulama modeli kuralı**

Belirtilen <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> klasör altındaki tüm sayfalar için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> örneklere bir eylem çağıran bir eylem oluşturmak ve eklemek için kullanın.

Örnek, uygulamasının, uygulamanın `AddFolderApplicationModelConvention` *diğer sayfalar* klasörünün içindeki sayfalara bir `OtherPagesHeader`başlık ekleyerek kullanımını gösterir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

Örnekteki Sayfa1 sayfasını `localhost:5000/OtherPages/Page1` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Diğer sayfalar/Sayfa1 sayfasının yanıt üstbilgileri, OtherPagesHeader öğesinin eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Sayfa uygulama modeli kuralı**

Sayfa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> için belirtilen ada sahip bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> eylemi çağıran bir eylem oluşturmak ve eklemek için kullanın.

Örnek, hakkında sayfasına bir `AddPageApplicationModelConvention` `AboutHeader`başlık ekleyerek öğesinin kullanımını gösterir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

Örnekteki hakkında daha `localhost:5000/About` fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

**Filtre yapılandırma**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>Belirtilen filtreyi uygulamak için yapılandırır. Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama bir lambda ifadesinde bir filtrenin nasıl uygulanacağını gösterir, bu da bir filtre döndüren bir fabrika olarak arka planda uygulandı:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

Sayfa uygulama modeli, *diğer sayfalar* klasöründeki Page2 sayfasına yol açan parçaların göreli yolunu denetlemek için kullanılır. Koşul geçerse, bir üst bilgi eklenir. `EmptyFilter` Aksi takdirde, uygulanır.

`EmptyFilter`bir [eylem filtresidir](xref:mvc/controllers/filters#action-filters). Eylem filtreleri Razor Pages `EmptyFilter` tarafından yoksayıldığından, yol içermiyorsa `OtherPages/Page2`amacına göre hiçbir etkiye sahip değildir.

Örnek Page2 sayfasını `localhost:5000/OtherPages/Page2` isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![OtherPagesPage2Header, Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

**Filtre fabrikası yapılandırma**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>Tüm Razor Pages [filtre](xref:mvc/controllers/filters) uygulamak için belirtilen fabrikası yapılandırır.

Örnek uygulama, uygulamanın sayfalarına iki değerli bir başlık `FilterFactoryHeader`ekleyerek bir [filtre fabrikası](xref:mvc/controllers/filters#ifilterfactory) kullanmanın bir örneğini sağlar:

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

Örnekteki hakkında daha `localhost:5000/About` fazla bilgi isteyin ve sonucu görüntülemek için üst bilgileri inceleyin:

![Hakkında sayfasının yanıt üstbilgileri iki FilterFactoryHeader üst bilgisinin eklendiğini gösterir.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC filtreleri ve sayfa filtresi (ıpagefilter)

Razor Pages işleyici yöntemleri kullandığından, MVC [eylem filtreleri](xref:mvc/controllers/filters#action-filters) Razor Pages tarafından yok sayılır. Diğer MVC filtresi türleri şunları kullanabilirsiniz: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters)ve [sonuç](xref:mvc/controllers/filters#result-filters). Daha fazla bilgi için [Filtreler](xref:mvc/controllers/filters) konusuna bakın.

Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor Pages için geçerli bir filtredir. Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
