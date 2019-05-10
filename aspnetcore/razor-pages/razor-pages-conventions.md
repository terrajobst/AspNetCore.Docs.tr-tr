---
title: İçinde ASP.NET Core Razor sayfalar yol ve uygulama kuralları
author: guardrex
description: Nasıl yol ve uygulama modeli sağlayıcısı kuralları sayfası denetimi yönlendirme, bulma ve işleme yardımcı keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 4e07b5803adbce94982584212fa65afbfd427b64
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899738"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>İçinde ASP.NET Core Razor sayfalar yol ve uygulama kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Sayfa kullanmayı öğrenin [rota ve uygulama sağlayıcısı kuralları modeli](xref:mvc/controllers/application-model#conventions) sayfasına yönlendirme, bulma ve Razor sayfaları uygulamalarda işleme denetlemek için.

Özel sayfa yolları her bir sayfayı yapılandırmanız gerektiğinde sahip sayfalar için yönlendirmeyi yapılandırma [AddPageRoute kuralı](#configure-a-page-route) bu konunun ilerleyen bölümlerinde açıklanmıştır.

Bir sayfa yolu belirtin, yol kesimleri ekleyin veya bir rota için parametreleri eklemek için sayfanın kullanın `@page` yönergesi. Daha fazla bilgi için [özel yollar](xref:razor-pages/index#custom-routes).

Yol segmentlerini veya parametre adları kullanılamaz, ayrılmış sözcükler vardır. Daha fazla bilgi için [yönlendirme: Yönlendirme adları ayrılmış](xref:fundamentals/routing#reserved-routing-names).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

| Senaryo | Örnek gösterir... |
| -------- | --------------------------- |
| [Model kuralları](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Bir uygulamanın sayfaları için bir rota şablonu ve üst bilgi ekleyin. |
| [Rota eylem kuralları sayfası](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Bir rota şablonu, bir klasördeki sayfalara ve tek bir sayfaya ekleyin. |
| [Sayfa modeli eylem kuralları](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (filtre sınıfı, lambda ifadesi veya filtresi Fabrika)</li></ul> | Bir klasördeki sayfalara üstbilgi ekleme, tek bir sayfaya bir üst bilgi ekleme ve yapılandırma bir [filtre Fabrika](xref:mvc/controllers/filters#ifilterfactory) uygulamanın sayfaları için bir başlık eklemek için. |

Razor sayfaları kuralları eklenir ve kullanılarak yapılandırılan <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> genişletme yöntemi için <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> hizmet koleksiyonu üzerinde `Startup` sınıfı. Aşağıdaki kural örnekleri, bu konunun ilerleyen bölümlerinde açıklanmıştır:

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

| Sipariş verme            | Davranış |
| :--------------: | -------- |
| -1               | Rotanın diğer yollar işlenmeden önce işlenir. |
| 0                | Sipariş belirtilmediyse (varsayılan değer). Atama yok `Order` (`Order = null`) rota varsayılanları `Order` işleme için 0 (sıfır). |
| 1, 2, &hellip; n | Rota işlem sırasını belirtir. |

Yönlendirme işlemi kurala göre belirlenir:

* Yollar, sıralı olarak işlenir (-1, 0, 1, 2 &hellip; n).
* Yollar olduğunda aynı `Order`en belirli bir yol ilk less yazımına özgü yol tarafından izlenen eşleşir.
* Zaman aynı yollar `Order` ve aynı parametre sayısıyla istek URL'si, yollar, eklemiş sırayla işlenir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Mümkünse, bağlı olarak belirlenen rota işleme sipariş kaçının. Genellikle, yönlendirme ile URL ile eşleşen doğru yolu seçer. Rota ayarlamanız gerekirse `Order` uygulamanın yönlendirme düzeni, büyük olasılıkla istemcilere kafa karıştırıcı ve kırılgan korumak için yönlendirmek için özellikler doğru ister. Uygulamanın yönlendirme şeması basitleştirmek arama yapın. Örnek uygulama, tek bir uygulama kullanarak çeşitli Yönlendirme senaryoları göstermek için sipariş işleme açık bir yol gerekiyor. Ancak, uygulama ayarı rotanın önlemek denemelidir `Order` üretim uygulamalarında.

Yönlendirme razor sayfaları ve MVC denetleyicisi yönlendirme paylaşım uygulaması. Rota sırası MVC konularında bilgi şu adreste [denetleyici eylemlerine yönlendirme: Öznitelik rotaları sıralama](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Model kuralları

Bir temsilci eklemek <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> eklemek için [model kuralları](xref:mvc/controllers/application-model#conventions) Razor sayfaları için geçerlidir.

### <a name="add-a-route-model-convention-to-all-pages"></a>Tüm sayfalar için bir yol modeli Kuralı Ekle

Kullanım <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> koleksiyonuna <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> sayfasına yönlendirme sırasında uygulanan örnek model oluşturma.

Örnek uygulamayı ekler bir `{globalTemplate?}` uygulamadaki tüm sayfalar için rota şablonu:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ayarlanır `1`. Bu örnek uygulamada davranışı aşağıdaki yol sağlar:

* Bir rota şablonu için `TheContactPage/{text?}` konusunda daha sonra eklenir. İlgili kişi sayfası yol varsayılan sıralamasını sahip `null` (`Order = 0`), önce eşleşecek şekilde `{globalTemplate?}` rota şablonu.
* Bir `{aboutTemplate?}` rota şablonu konusunda daha sonra eklenir. `{aboutTemplate?}` Şablon verildiğinde bir `Order` , `2`. Ne zaman hakkında sayfası istenen adresindeki `/About/RouteDataValue`, "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["aboutTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.
* Bir `{otherPagesTemplate?}` rota şablonu konusunda daha sonra eklenir. `{otherPagesTemplate?}` Şablon verildiğinde bir `Order` , `2`. Ne zaman tüm sayfa içinde *sayfaları/OtherPages* klasörü ile bir rota parametresini istenen (örneğin, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.

Mümkün olduğunda, ayarlamamanız `Order`, içindeki sonuçlar `Order = 0`. Doğru yol seçmek için yönlendirme kullanır.

Razor sayfaları seçenekleri ekleme gibi <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, MVC hizmet koleksiyona eklendiğinde, eklenen `Startup.ConfigureServices`. Bir örnek için bkz. [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About/GlobalRouteValue` ve sonucu inceleyin:

![Hakkında sayfası GlobalRouteValue ile bir yol kesimi istenir. Rota veri değeri sayfanın OnGet yöntemi yakalanır işlenen sayfada gösterilir.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Tüm sayfalar için bir uygulama modeli Kuralı Ekle

Kullanım <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> koleksiyonuna <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> sayfasında uygulama modeli oluşturma sırasında uygulanan örnekleri.

Bu ve diğer kuralları daha sonra bu konudaki göstermek için örnek uygulamayı içeren bir `AddHeaderAttribute` sınıfı. Sınıf oluşturucu kabul eden bir `name` dize ve `values` dize dizisi. Bu değerler kullanılır, `OnResultExecuting` yöntemi yanıt üst bilgisini ayarlayın. Tam sınıf gösterilen [sayfa modeli eylem kuralları](#page-model-action-conventions) konunun ilerleyen bölümlerinde.

Örnek uygulama kullandığı `AddHeaderAttribute` üst bilgi eklemek için sınıfı `GlobalHeader`, uygulamadaki tüm sayfalar için:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri hakkında sayfasının GlobalHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Tüm sayfalar için bir işleyici modeli Kuralı Ekle

Kullanım <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> koleksiyonuna <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> sırasında uygulanan örnek işleyici modeli oluşturma sayfası.

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>Rota eylem kuralları sayfası

Öğesinden türetilen varsayılan rota model sağlayıcısı <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> sayfa yolları yapılandırmak için genişletilebilirlik noktaları sağlamak için tasarlanmış kuralları çağırır.

### <a name="folder-route-model-convention"></a>Klasör yolu model kuralı

Kullanım <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , çağıran bir eylem üzerinde <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> belirtilen klasör altındaki tüm sayfalar için.

Örnek uygulama kullandığı <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> eklemek için bir `{otherPagesTemplate?}` sayfaları için rota şablonu *OtherPages* klasörü:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Özelliği <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ayarlanır `2`. Bu şablonu sağlar `{globalTemplate?}` (önceki konuya ayarlamak `1`) için tek yol değeri sağlandığında konumu ilk rota veri değeri öncelik verilir. Bir sayfa varsa *sayfaları/OtherPages* klasör yolu bir parametre istenen (örneğin, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" içine yüklenir `RouteData.Values["globalTemplate"]` (`Order = 1`) ve `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ayarı nedeniyle `Order` özelliği.

Mümkün olduğunda, ayarlamamanız `Order`, içindeki sonuçlar `Order = 0`. Doğru yol seçmek için yönlendirme kullanır.

Örnek kullanıcının Sayfa1 sayfanın istek `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ve sonucu inceleyin:

![OtherPages klasöründe Sayfa1 GlobalRouteValue ve OtherPagesRouteValue yol kesimini ile istenir. Rota veri değerleri sayfa OnGet yönteminde yakalanır işlenen sayfada gösterilir.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Sayfa yolu model kuralı

Kullanım <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , çağıran bir eylem üzerinde <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> sayfanın belirtilen ada sahip.

Örnek uygulama kullandığı `AddPageRouteModelConvention` eklemek için bir `{aboutTemplate?}` hakkında sayfasına rota şablonu:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

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

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> belirtilen sayfa yolunda bir sayfa için bir yol yapılandırmak için. Oluşturulan Bağlantılar sayfasını, belirtilen rota kullanın. `AddPageRoute` kullanan `AddPageRouteModelConvention` rota oluşturmak için.

Örnek uygulama için bir yol oluşturur `/TheContactPage` için *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

İlgili kişi sayfası aynı zamanda adresinden ulaşılabilir `/Contact` aracılığıyla varsayılan yolu.

İlgili kişi sayfası örnek uygulamanın özel yol için bir isteğe bağlı sağlar `text` yol kesimi (`{text?}`). Sayfa Ayrıca bu isteğe bağlı bir segmente içerir, `@page` ziyaretçi sayfasına erişen durumunda yönerge kendi `/Contact` rota:

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

URL için oluşturulan Not **kişi** işlenen sayfanın bağlantısını yansıtan güncelleştirilmiş yolu:

![Gezinti çubuğunda, uygulama kişi bağlantısı örneği](razor-pages-conventions/_static/contact-link.png)

![İşlenmiş HTML kişi bağlantıya inceleyerek gösterir href ayarlamak ' / TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

İlgili kişi sayfasını ya da kendi sıradan rota ziyaret edin `/Contact`, ya da özel rota `/TheContactPage`. Ek bir sağlarsanız `text` yol kesimi sayfada gösterilir HTML ile kodlanmış segment sağlamanız:

![URL'deki 'MetinDeğeri' bir isteğe bağlı 'text' yol kesimi sağlama edge tarayıcı örneği. İşlenen sayfada 'text' segmenti değeri gösterilir.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Sayfa modeli eylem kuralları

Uygulayan varsayılan sayfa modeli sağlayıcısı <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> sayfa modelleri yapılandırmak için genişletilebilirlik noktaları sağlamak için tasarlanmış kuralları çağırır. Bu kuralları oluştururken ve sayfa bulma ve işleme senaryoları değiştirme yararlı olur.

Bu bölümdeki örnekler için örnek uygulamayı kullanan bir `AddHeaderAttribute` sınıfının bir <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, bir yanıt üstbilgisi, geçerli:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

Kuralları kullanarak örnek bir klasördeki tüm sayfalar için ve tek sayfalı bir öznitelik uygulamak nasıl gösterir.

**Klasör uygulama modeli kuralı**

Kullanım <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , çağıran bir eylem üzerinde <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> belirtilen klasör altındaki tüm sayfalar için örnekleri.

Örnek, kullanımını gösterir. `AddFolderApplicationModelConvention` bir üst bilgisi ekleyerek `OtherPagesHeader`, içinde sayfalara *OtherPages* uygulama klasörü:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

Örnek kullanıcının Sayfa1 sayfanın istek `localhost:5000/OtherPages/Page1` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri OtherPages/Sayfa1 sayfanın OtherPagesHeader eklendiğini gösterir.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Sayfa uygulama modeli kuralı**

Kullanım <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> oluşturmak ve eklemek için bir <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , çağıran bir eylem üzerinde <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> sayfanın belirtilen ada sahip.

Örnek, kullanımını gösterir. `AddPageApplicationModelConvention` bir üst bilgisi ekleyerek `AboutHeader`, hakkında sayfası:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![Yanıt Üstbilgileri hakkında sayfasının AboutHeader eklendiğini gösterir.](razor-pages-conventions/_static/about-page-about-header.png)

**Bir filtre yapılandırın**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Belirtilen filtre uygulamak için yapılandırır. Bir filtre sınıfı uygulayabilirsiniz, ancak örnek uygulama, Sahne Arkası filtre döndüren bir Fabrika olarak uygulanan bir lambda ifadesinde bir filtre uygulamak gösterilmektedir:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

Sayfa uygulama modeli Page2 sayfasına yol kesimleri için göreli yolu denetlemek için kullanılan *OtherPages* klasör. Koşul başarılı olursa, bir üst bilgi eklenir. Aksi takdirde, `EmptyFilter` uygulanır.

`EmptyFilter` olan bir [eylem filtresi](xref:mvc/controllers/filters#action-filters). Eylem filtreleri Razor sayfaları tarafından göz ardı edilir beri `EmptyFilter` no-yolu içermiyor beklendiği gibi ops `OtherPages/Page2`.

Örnek kullanıcının Page2 sayfanın istek `localhost:5000/OtherPages/Page2` ve sonucu görüntülemek için üst bilgileri inceleyin:

![OtherPagesPage2Header Page2 için yanıta eklenir.](razor-pages-conventions/_static/page2-filter-header.png)

**Bir filtre fabrikayı yapılandırma**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Belirtilen Üreteç uygulamak için yapılandırır [filtreleri](xref:mvc/controllers/filters) tüm Razor sayfaları için.

Örnek uygulamasını kullanarak bir örnek sağlar. bir [filtre Fabrika](xref:mvc/controllers/filters#ifilterfactory) bir üst bilgisi ekleyerek `FilterFactoryHeader`, uygulamanın sayfaları için iki değerleriyle:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Örnek kullanıcının hakkında sayfası, istek `localhost:5000/About` ve sonucu görüntülemek için üst bilgileri inceleyin:

![İki FilterFactoryHeader üst bilgileri eklendiğinden hakkında sayfasının yanıt üst bilgileri gösterin.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>MVC filtreleri ve sayfa filtresi (IPageFilter)

MVC [eylem filtrelerini](xref:mvc/controllers/filters#action-filters) Razor sayfaları işleyici yöntemleri kullandığından Razor sayfaları tarafından göz ardı edilir. MVC filtre diğer türleri kullanabilmeniz için kullanılabilir: [Yetkilendirme](xref:mvc/controllers/filters#authorization-filters), [özel durum](xref:mvc/controllers/filters#exception-filters), [kaynak](xref:mvc/controllers/filters#resource-filters), ve [sonucu](xref:mvc/controllers/filters#result-filters). Daha fazla bilgi için [filtreleri](xref:mvc/controllers/filters) konu.

Sayfa filtresi (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) Razor sayfaları için geçerli bir filtredir. Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
