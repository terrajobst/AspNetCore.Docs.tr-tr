---
title: İçinde ASP.NET Core Razor sayfalar yol ve uygulama kuralları
author: guardrex
description: Nasıl yol ve uygulama modeli sağlayıcısı kuralları sayfası denetimi yönlendirme, bulma ve işleme yardımcı keşfedin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/12/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 13fd6c156afd5ab62739b09296a929120ce3450f
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348539"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>İçinde ASP.NET Core Razor sayfalar yol ve uygulama kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Sayfa kullanmayı öğrenin [rota ve uygulama sağlayıcısı kuralları modeli](xref:mvc/controllers/application-model#conventions) sayfasına yönlendirme, bulma ve Razor sayfaları uygulamalarda işleme denetlemek için.

Özel sayfa yolları her bir sayfayı yapılandırmanız gerektiğinde sahip sayfalar için yönlendirmeyi yapılandırma [AddPageRoute kuralı](#configure-a-page-route) bu konunun ilerleyen bölümlerinde açıklanmıştır.

Bir sayfa yolu belirtin, yol kesimleri ekleyin veya bir rota için parametreleri eklemek için sayfanın kullanın `@page` yönergesi. Daha fazla bilgi için [özel yollar](xref:razor-pages/index#custom-routes).

Yol segmentlerini veya parametre adları kullanılamaz, ayrılmış sözcükler vardır. Daha fazla bilgi için [yönlendirme: Yönlendirme adları ayrılmış](xref:fundamentals/routing#reserved-routing-names).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"

| Senaryo | Örnek gösterir... |
| -------- | --------------------------- |
| [Model kuralları](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Bir uygulamanın sayfaları için bir rota şablonu ve üst bilgi ekleyin. |
| [Rota eylem kuralları sayfası](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Bir rota şablonu, bir klasördeki sayfalara ve tek bir sayfaya ekleyin. |
| [Sayfa modeli eylem kuralları](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtresi Fabrika)</li></ul> | Bir klasördeki sayfalara üstbilgi ekleme, tek bir sayfaya bir üst bilgi ekleme ve yapılandırma bir [filtre Fabrika](xref:mvc/controllers/filters#ifilterfactory) uygulamanın sayfaları için bir başlık eklemek için. |
| [Varsayılan sayfa uygulama modeli sağlayıcısı](#replace-the-default-page-app-model-provider) | İşleyici adları için kurallar için varsayılan sayfa modeli sağlayıcısı değiştirin. |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Senaryo | Örnek gösterir... |
| -------- | --------------------------- |
| [Model kuralları](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Bir uygulamanın sayfaları için bir rota şablonu ve üst bilgi ekleyin. |
| [Rota eylem kuralları sayfası](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Bir rota şablonu, bir klasördeki sayfalara ve tek bir sayfaya ekleyin. |
| [Sayfa modeli eylem kuralları](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtresi Fabrika)</li></ul> | Bir klasördeki sayfalara üstbilgi ekleme, tek bir sayfaya bir üst bilgi ekleme ve yapılandırma bir [filtre Fabrika](xref:mvc/controllers/filters#ifilterfactory) uygulamanın sayfaları için bir başlık eklemek için. |
| [Varsayılan sayfa uygulama modeli sağlayıcısı](#replace-the-default-page-app-model-provider) | İşleyici adları için kurallar için varsayılan sayfa modeli sağlayıcısı değiştirin. |

::: moniker-end

Razor sayfaları kuralları eklenir ve kullanılarak yapılandırılan [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) genişletme yöntemi için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) hizmet koleksiyonu üzerinde `Startup` sınıfı. Aşağıdaki kural örnekleri, bu konunun ilerleyen bölümlerinde açıklanmıştır:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Rota sırası

Rota belirtme bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> (yol ile eşleşen) işlemek için.

| Sırası            | Davranış |
| :--------------: | -------- |
| -1               | Rotanın diğer yollar işlenmeden önce işlenir. |
| 0                | Sipariş belirtilmediyse (varsayılan değer). Atama yok `Order` (`Order = null`) rota varsayılanları `Order` işleme için 0 (sıfır). |
| 1, 2, &hellip; n | Rota işlem sırasını belirtir. |

Yönlendirme işlemi kurala göre belirlenir:

* Yollar, sıralı olarak işlenir (-1, 0, 1, 2 &hellip; n).
* Yollar olduğunda aynı `Order`en belirli bir yol ilk less yazımına özgü yol tarafından izlenen eşleşir.
* Zaman aynı yollar `Order` ve aynı parametre sayısıyla istek URL'si, yollar, eklemiş sırayla işlenir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Mümkünse, bağlı olarak belirlenen rota işleme sipariş kaçının. Genellikle, yönlendirme ile URL ile eşleşen doğru yolu seçer. Rota ayarlamanız gerekirse `Order` uygulamanın yönlendirme düzeni, büyük olasılıkla istemcilere kafa karıştırıcı ve kırılgan korumak için yönlendirmek için özellikler doğru ister. Uygulamanın yönlendirme şeması basitleştirmek arama yapın. Örnek uygulama, tek bir uygulama kullanarak çeşitli Yönlendirme senaryoları göstermek için sipariş işleme açık bir yol gerekiyor. Ancak, uygulama ayarı rotanın önlemek denemelidir `Order` üretim uygulamalarında.

Yönlendirme razor sayfaları ve MVC denetleyicisi yönlendirme paylaşım uygulaması. Rota sırası MVC konularında bilgi şu adreste [denetleyici eylemlerine yönlendirme: öznitelik rotaları sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Model kuralları

Bir temsilci eklemek [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) eklemek için [model kuralları](xref:mvc/controllers/application-model#conventions) Razor sayfaları için geçerlidir.

### <a name="add-a-route-model-convention-to-all-pages"></a>Tüm sayfalar için bir yol modeli Kuralı Ekle

Kullanım [kuralları](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) oluşturmak ve eklemek için bir [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) koleksiyonuna [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) sayfa rota modeli sırasında uygulanan örnekleri Yapı.

Örnek uygulamayı ekler bir `{globalTemplate?}` uygulamadaki tüm sayfalar için rota şablonu:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ayarlanır `1`. Bu örnek uygulamada davranışı aşağıdaki yol sağlar:

* Bir rota şablonu için `TheContactPage/{text?}` konusunda daha sonra eklenir. İlgili kişi sayfası yol varsayılan sıralamasını sahip `null` (`Order = 0`), önce eşleşecek şekilde `{globalTemplate?}` rota şablonu.
* Bir `{aboutTemplate?}` rota şablonu konusunda daha sonra eklenir. `{aboutTemplate?}` Şablon verildiğinde bir `Order` , `2`. Ne zaman hakkında sayfası istenen adresindeki `/About/RouteDataValue`, "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.
* Bir `{otherPagesTemplate?}` rota şablonu konusunda daha sonra eklenir. `{otherPagesTemplate?}` Şablon verildiğinde bir `Order` , `2`. Ne zaman tüm sayfa içinde *sayfaları/OtherPages* klasörü ile bir rota parametresini istenen (örneğin, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.

Mümkün olduğunda, ayarlamamanız `Order`, içindeki sonuçlar `Order = 0`. Doğru yol seçmek için yönlendirme kullanır.

Razor sayfaları seçenekleri ekleme gibi [kuralları](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), MVC hizmet koleksiyona eklendiğinde, eklenen `Startup.ConfigureServices`. Bir örnek için bkz. [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About/GlobalRouteValue` ve sonucu inceleyin:

![Hakkında sayfası GlobalRouteValue ile bir yol kesimi istenir. Rota veri değeri sayfanın OnGet yöntemi yakalanır işlenen sayfada gösterilir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Tüm sayfalar için bir uygulama modeli Kuralı Ekle

Kullanım [kuralları](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) oluşturmak ve eklemek için bir [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) koleksiyonuna [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) sayfa uygulama modeli sırasında uygulanan örnekleri Yapı.

Bu ve diğer kuralları daha sonra bu konudaki göstermek için örnek uygulamayı içeren bir `AddHeaderAttribute` sınıfı. Sınıf oluşturucu kabul eden bir `name` dize ve `values` dize dizisi. Bu değerler kullanılır, `OnResultExecuting` yöntemi yanıt üst bilgisini ayarlayın. Tam sınıf gösterilen [sayfa modeli eylem kuralları](#page-model-action-conventions) konunun ilerleyen bölümlerinde.

Örnek uygulama kullandığı `AddHeaderAttribute` üst bilgi eklemek için sınıfı `GlobalHeader`, uygulamadaki tüm sayfalar için:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri hakkında sayfasının GlobalHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

### <a name="add-a-handler-model-convention-to-all-pages"></a>Tüm sayfalar için bir işleyici modeli Kuralı Ekle

Kullanım [kuralları](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) oluşturmak ve eklemek için bir [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) koleksiyonuna [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) sayfa işleyici modeli sırasında uygulanan örnekleri Yapı.

```csharp
public class GlobalPageHandlerModelConvention
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a>Rota eylem kuralları sayfası

Öğesinden türetilen varsayılan rota model sağlayıcısı [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) sayfa yolları yapılandırmak için genişletilebilirlik noktaları sağlamak için tasarlanmış kuralları çağırır.

### <a name="folder-route-model-convention"></a>Klasör yolu model kuralı

Kullanım [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) oluşturmak ve eklemek için bir [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , çağıran bir eylem üzerinde [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) tüm sayfalar için Belirtilen klasör.

Örnek uygulama kullandığı `AddFolderRouteModelConvention` eklemek için bir `{otherPagesTemplate?}` sayfaları için rota şablonu *OtherPages* klasörü:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ayarlanır `2`. Bu şablonu sağlar `{globalTemplate?}` (önceki konuya ayarlamak `1`) için tek yol değeri sağlandığında konumu ilk rota veri değeri öncelik verilir. Bir sayfa varsa *sayfaları/OtherPages* klasör yolu bir parametre istenen (örneğin, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.

Mümkün olduğunda, ayarlamamanız `Order`, içindeki sonuçlar `Order = 0`. Doğru yol seçmek için yönlendirme kullanır.

Örnek kullanıcının Sayfa1 sayfanın istek `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ve sonucu inceleyin:

![OtherPages klasöründe Sayfa1 GlobalRouteValue ve OtherPagesRouteValue yol kesimini ile istenir. Rota veri değerleri sayfa OnGet yönteminde yakalanır işlenen sayfada gösterilir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Sayfa yolu model kuralı

Kullanım [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) oluşturmak ve eklemek için bir [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , çağıran bir eylem üzerinde [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) belirtilen sayfa için adı.

Örnek uygulama kullandığı `AddPageRouteModelConvention` eklemek için bir `{aboutTemplate?}` hakkında sayfasına rota şablonu:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ayarlanır `2`. Bu şablonu sağlar `{globalTemplate?}` (önceki konuya ayarlamak `1`) için tek yol değeri sağlandığında konumu ilk rota veri değeri öncelik verilir. Bir rota parametresi değer ile hakkında sayfası istenirse, `/About/RouteDataValue`, "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.

Mümkün olduğunda, ayarlamamanız `Order`, içindeki sonuçlar `Order = 0`. Doğru yol seçmek için yönlendirme kullanır.

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ve sonucu inceleyin:

![Sayfa hakkında GlobalRouteValue ve AboutRouteValue için yol kesimleri ile istenir. Rota veri değerleri sayfa OnGet yönteminde yakalanır işlenen sayfada gösterilir.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Sayfa yollar özelleştirmek için bir parametre transformer kullanma

ASP.NET Core tarafından oluşturulan sayfa yollar, bir parametre transformer kullanma özelleştirilebilir. Bir parametre transformer uygulayan `IOutboundParameterTransformer` ve parametre değerine dönüştürür. Örneğin, bir özel `SlugifyParameterTransformer` parametre transformer değişiklikleri `SubscriptionManagement` yönlendirmek için değer `subscription-management`.

`PageRouteTransformerConvention` Sayfa rota modeli kuralı bir uygulamayı otomatik olarak oluşturulan sayfa yollar klasör ve dosya adı kesimlerini parametresi transformer uygular. Örneğin, Razor sayfaları dosya */Pages/SubscriptionManagement/ViewAll.cshtml* gelen yeniden yolunu gerekir `/SubscriptionManagement/ViewAll` için `/subscription-management/view-all`.

`PageRouteTransformerConvention` Razor sayfaları klasör ve dosya adı gelirler otomatik olarak oluşturulan bir sayfa yolu parçalarını yalnızca dönüştürür. Yol kesimleri ile eklenen dönüştürmez `@page` yönergesi. Kuralı tarafından eklenen rotaları da dönüştürmez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

`PageRouteTransformerConvention` Bir seçenek olarak kayıtlı `Startup.ConfigureServices`:

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

## <a name="configure-a-page-route"></a>Bir sayfa yolunu Yapılandır

Kullanım [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) belirtilen sayfa yolunda bir sayfa için bir yol yapılandırmak için. Oluşturulan Bağlantılar sayfasını, belirtilen rota kullanın. `AddPageRoute` kullanan `AddPageRouteModelConvention` rota oluşturmak için.

Örnek uygulama için bir yol oluşturur `/TheContactPage` için *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

İlgili kişi sayfası aynı zamanda adresinden ulaşılabilir `/Contact` aracılığıyla varsayılan yolu.

İlgili kişi sayfası örnek uygulamanın özel yol için bir isteğe bağlı sağlar `text` yol kesimi (`{text?}`). Sayfa Ayrıca bu isteğe bağlı bir segmente içerir, `@page` ziyaretçi sayfasına erişen durumunda yönerge kendi `/Contact` rota:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

URL için oluşturulan Not **kişi** işlenen sayfanın bağlantısını yansıtan güncelleştirilmiş yolu:

![Gezinti çubuğunda, uygulama kişi bağlantısı örneği](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML kişi bağlantıya inceleyerek gösterir href ayarlamak ' / TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

İlgili kişi sayfasını ya da kendi sıradan rota ziyaret edin `/Contact`, ya da özel rota `/TheContactPage`. Ek bir sağlarsanız `text` yol kesimi sayfada gösterilir HTML ile kodlanmış segment sağlamanız:

![URL'deki 'MetinDeğeri' bir isteğe bağlı 'text' yol kesimi sağlama edge tarayıcı örneği. İşlenen sayfada 'text' segmenti değeri gösterilir.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Sayfa modeli eylem kuralları

Uygulayan varsayılan sayfa modeli sağlayıcısı [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) sayfa modelleri yapılandırmak için genişletilebilirlik noktaları sağlamak için tasarlanmış kuralları çağırır. Bu kuralları oluştururken ve sayfa bulma ve işleme senaryoları değiştirme yararlı olur.

Bu bölümdeki örnekler için örnek uygulamayı kullanan bir `AddHeaderAttribute` sınıfının bir [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), bir yanıt üstbilgisi, geçerli:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Kuralları kullanarak örnek bir klasördeki tüm sayfalar için ve tek sayfalı bir öznitelik uygulamak nasıl gösterir.

**Klasör uygulama modeli kuralı**

Kullanım [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) oluşturmak ve eklemek için bir [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , çağıran bir eylem üzerinde [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) örnekler Belirtilen klasör altındaki tüm sayfalar sağlar.

Örnek, kullanımını gösterir. `AddFolderApplicationModelConvention` bir üst bilgisi ekleyerek `OtherPagesHeader`, içinde sayfalara *OtherPages* uygulama klasörü:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Örnek kullanıcının Sayfa1 sayfanın istek `localhost:5000/OtherPages/Page1` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri OtherPages/Sayfa1 sayfanın OtherPagesHeader eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Sayfa uygulama modeli kuralı**

Kullanım [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) oluşturmak ve eklemek için bir [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , çağıran bir eylem üzerinde [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) sayfası Belirtilen ada sahip.

Örnek, kullanımını gösterir. `AddPageApplicationModelConvention` bir üst bilgisi ekleyerek `AboutHeader`, hakkında sayfası:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri hakkında sayfasının AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

**Bir filtre yapılandırın**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) belirtilen filtre uygulamak için yapılandırır. Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama, Sahne Arkası filtre döndüren bir Fabrika olarak uygulanan bir lambda ifadesinde bir filtre uygulamak gösterilmektedir:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Sayfa uygulama modeli Page2 sayfasına yol kesimleri için göreli yolu denetlemek için kullanılan *OtherPages* klasör. Koşul başarılı olursa, bir üst bilgi eklenir. Aksi takdirde, `EmptyFilter` uygulanır.

`EmptyFilter` olan bir [eylem filtresi](xref:mvc/controllers/filters#action-filters). Eylem filtreleri Razor sayfaları tarafından göz ardı edilir beri `EmptyFilter` no-yolu içermiyor beklendiği gibi ops `OtherPages/Page2`.

Örnek kullanıcının Page2 sayfanın istek `localhost:5000/OtherPages/Page2` ve sonucu görüntülemek için üst bilgileri inceleyin:

![OtherPagesPage2Header Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

**Bir filtre fabrikayı yapılandırma**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) uygulamak için belirtilen Üreteç yapılandırır [filtreleri](xref:mvc/controllers/filters) tüm Razor sayfaları için.

Örnek uygulamasını kullanarak bir örnek sağlar. bir [filtre Fabrika](xref:mvc/controllers/filters#ifilterfactory) bir üst bilgisi ekleyerek `FilterFactoryHeader`, uygulamanın sayfaları için iki değerleriyle:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![İki FilterFactoryHeader üst bilgileri eklendiğinden hakkında sayfasının yanıt üst bilgileri gösterin.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Varsayılan sayfa uygulama modeli sağlayıcısı değiştirin

Razor sayfaları kullanan `IPageApplicationModelProvider` arabirimi oluşturmak için bir [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). İşleyici bulma ve işlenmesi için kendi uygulama mantığını sağlamak için varsayılan model sağlayıcısının devralabilir. Varsayılan uygulama ([başvuru kaynağı](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) için kuralları oluşturur *adlandırılmamış* ve *adlı* aşağıda açıklanan adlandırma, işleyici.

**Varsayılan adlandırılmamış işleyici yöntemleri**

İşleyici yöntemleri HTTP fiilleri ("adlandırılmamış" işleyici yöntemler) için bir kural uygulayın: `On<HTTP verb>[Async]` (ekleme `Async` isteğe bağlıdır, ancak zaman uyumsuz yöntemler için önerilen).

| Adsız işleyicisi yöntemi     | Çalışma                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Sayfa durumu başlatın.     |
| `OnPost`/`OnPostAsync`     | POST isteklerini işler.          |
| `OnDelete`/`OnDeleteAsync` | DELETE isteklerini işlemek&#8224;. |
| `OnPut`/`OnPutAsync`       | PUT isteklerini işleyecek&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Düzeltme eki isteklerini işlemek&#8224;.  |

&#8224;Sayfaya API çağrıları yapmak için kullanılır.

**Varsayılan adlandırılmış işleyici yöntemleri**

Benzer bir kural ("adlı işleyici yöntemleri") geliştiricisi tarafından sağlanan işleyici yöntemleri uygulayın. HTTP fiili veya HTTP fiili arasında işleyicisi adı görüntülenir ve `Async`: `On<HTTP verb><handler name>[Async]` (ekleme `Async` isteğe bağlıdır, ancak zaman uyumsuz yöntemler için önerilen). Örneğin, iletileri işleyen yöntemler aşağıdaki tabloda gösterilen adlandırma alabilir.

| Örnek adlı işleyici yöntemi             | Örnek işlemi        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Bir ileti alın.        |
| `OnPostMessage`/`OnPostMessageAsync`     | Bir ileti gönderin.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | İleti silme&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Bir ileti YERLEŞTİRME&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Bir ileti düzeltme eki&#8224;.  |

&#8224;Sayfaya API çağrıları yapmak için kullanılır.

**İşleyicisi yöntem adları özelleştirme**

Adsız ve adlandırılmış işleyici yöntemleri adlı şeklini değiştirmek tercih ettiğiniz varsayılır. Alternatif bir adlandırma şeması, yöntem adları "On" ile başlayan önlemek ve HTTP fiili belirlemek için ilk sözcük kesimi kullanmaktır. Diğer değişiklikler, yaptığınız gibi fiiller için silme dönüştürme, PUT ve POST için düzeltme eki. Yöntem adları aşağıdaki tabloda gösterildiği gibi bir düzeni sağlar.

| İşleyicisi yöntemi                       | Çalışma                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Sayfa durumu başlatın.     |
| `Post`/`PostAsync`                   | POST isteklerini işler.          |
| `Delete`/`DeleteAsync`               | DELETE isteklerini işlemek&#8224;. |
| `Put`/`PutAsync`                     | PUT isteklerini işleyecek&#8224;.    |
| `Patch`/`PatchAsync`                 | Düzeltme eki isteklerini işlemek&#8224;.  |
| `GetMessage`                         | Bir ileti alın.              |
| `PostMessage`/`PostMessageAsync`     | Bir ileti gönderin.                |
| `DeleteMessage`/`DeleteMessageAsync` | Silmek için bir ileti gönderin.      |
| `PutMessage`/`PutMessageAsync`       | Koymak için bir ileti gönderin.         |
| `PatchMessage`/`PatchMessageAsync`   | Düzeltme eki ileti gönderin.       |

&#8224;Sayfaya API çağrıları yapmak için kullanılır.

Bu düzen oluşturmak için devralınan `DefaultPageApplicationModelProvider` sınıf ve geçersiz kılma [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) çözümlemek için özel mantığı sağlamak yöntemi [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) işleyici adları. Örnek uygulamayı nasıl yapıldığını gösterir, `CustomPageApplicationModelProvider` sınıfı:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Sınıfın önemli noktalar şunlardır:

* Sınıfının devraldığı `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` HTTP fiili belirlemek için bir işleyici işler (`httpMethod`) ve adlandırılmış işleyici adı (`handlerName`) oluştururken `PageHandlerModel`.
  * Bir `Async` sonek göz ardı edilir, varsa.
  * Büyük/küçük harf, HTTP fiili yöntem adından ayrıştırmak için kullanılır.
  * Zaman yöntem adı (olmadan `Async`) olduğu HTTP fiili adına eşit, yok adlı işleyici yok. `handlerName` Ayarlanır `null`, ve yöntem adı `Get`, `Post`, `Delete`, `Put`, veya `Patch`.
  * Zaman yöntem adı (olmadan `Async`) HTTP fiili adın uzun bir adlandırılmış işleyici. `handlerName` Ayarlanır `<method name (less 'Async', if present)>`. Örneğin, hem "GetMessage" ve "GetMessageAsync" işleyicisi adı "GetMessage" yield.
  * SİLME, PUT ve PATCH HTTP fiilleri GÖNDERİYE dönüştürülür.

Kayıt `CustomPageApplicationModelProvider` içinde `Startup` sınıfı:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

Sayfa modeli *Index.cshtml.cs* sıradan işleyicisi yöntemi adlandırma kurallarını uygulama sayfaları için nasıl değiştirilir gösterir. "" Ön eki ile Razor sayfaları kullanılan adlandırmayla ilgili sıradan kaldırıldı. Sayfa durumu başlatan yöntem artık adlı `Get`. Bu kural, herhangi bir sayfa modeli herhangi birinin sayfaların açarsanız uygulama boyunca kullanılan görebilirsiniz.

Diğer yöntemlerin her biri kendi işleme tanımlayan HTTP fiili ile başlayın. İle başlayan iki yöntem `Delete` normalde DELETE HTTP fiilleri ancak mantık olarak kabul edilir `TryParseHandlerMethod` fiili POST hem işleyicileri için açıkça ayarlar.

Unutmayın `Async` arasında isteğe bağlı olduğu `DeleteAllMessages` ve `DeleteMessageAsync`. Hem zaman uyumsuz yöntemler oldukları ancak kullanmayı da tercih edebilirsiniz `Async` veya sonek; bunu yapmanızı öneririz. `DeleteAllMessages` Burada tanıtım amacıyla kullanılır, ancak bu tür bir yöntem adı öneririz `DeleteAllMessagesAsync`. İşleme etkilemez örneğe ait uygulama, ancak kullanarak `Async` çağrılar zaman uyumsuz bir yöntem olduğunu kullanıma sonek.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Sağlanan işleyicisi adlarını not edin *Index.cshtml* eşleşen `DeleteAllMessages` ve `DeleteMessageAsync` işleyici yöntemleri:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` işleyicisi yöntem adı içerisinde `DeleteMessageAsync` tarafından dışarı factored `TryParseHandlerMethod` yöntemi POST isteğinin işleyici eşlemesi. `asp-page-handler` Adını `DeleteMessage` işleyici yöntemine eşleşen `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC filtreleri ve sayfa filtresi (IPageFilter)

MVC [eylem filtrelerini](xref:mvc/controllers/filters#action-filters) Razor sayfaları işleyici yöntemleri kullandığından Razor sayfaları tarafından göz ardı edilir. Diğer türleri MVC filtrelerinin kullanabilmeniz için kullanılabilir: [yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters), ve [sonucu](xref:mvc/controllers/filters#result-filters). Daha fazla bilgi için [filtreleri](xref:mvc/controllers/filters) konu.

Sayfa filtresi ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) Razor sayfaları için geçerli bir filtredir. Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).

## <a name="additional-resources"></a>Ek kaynaklar

* [Razor sayfaları yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization)
