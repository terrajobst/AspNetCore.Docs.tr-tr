---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalardaki istekleri yönlendirme ve gezinti bağlantısı bileşeni hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/routing
ms.openlocfilehash: 76266aedd4655161f1f50a8beb0936660d452912
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999821"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Blazor yönlendirme

Tarafından [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

İsteklerin nasıl yönlendirileceğini ve Blazor uygulamalarında gezinti bağlantıları oluşturmak için `NavLink` bileşeninin nasıl kullanılacağını öğrenin.

## <a name="aspnet-core-endpoint-routing-integration"></a>Uç nokta yönlendirme tümleştirmesi ASP.NET Core

Blazor sunucusu [ASP.NET Core uç nokta yönlendirme](xref:fundamentals/routing)ile tümleşiktir. ASP.NET Core bir uygulama, `Startup.Configure` ' de `MapBlazorHub` ile etkileşimli bileşenlere yönelik gelen bağlantıları kabul edecek şekilde yapılandırılmıştır:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

En yaygın yapılandırma, tüm istekleri Razor sayfasına yönlendirmesidir ve bu, Blazor Server uygulamasının sunucu tarafı bölümü için ana bilgisayar görevi görür. Kural gereği, *ana bilgisayar* sayfası genellikle *_host. cshtml*olarak adlandırılır. Ana bilgisayar dosyasında belirtilen yol, yol eşleştirilirken düşük bir öncelik ile çalıştığından bir *geri dönüş yolu* olarak adlandırılır. Geri dönüş yolu, diğer yollar eşleşmediği zaman kabul edilir. Bu, uygulamanın Blazor sunucu uygulamasıyla kesintiye uğramadan diğer denetleyicileri ve sayfaları kullanmasına izin verir.

## <a name="route-templates"></a>Rota şablonları

@No__t-0 bileşeni, belirtilen bir rota ile her bileşene yönlendirmeyi sağlar. @No__t-0 bileşeni *app. Razor* dosyasında görünür:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

@No__t-1 yönergesi içeren bir *. Razor* dosyası derlendiğinde, oluşturulan sınıf, yol şablonunu belirten bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> olarak sunulur.

Çalışma zamanında `RouteView` bileşeni:

* @No__t-1 ' den istenen parametrelerle birlikte `RouteData` alır.
* Belirtilen parametreleri kullanarak belirtilen bileşeni düzeniyle (veya isteğe bağlı bir varsayılan düzende) işler.

İsteğe bağlı olarak, düzen belirtmeyen bileşenler için kullanılacak düzen sınıfıyla birlikte `DefaultLayout` parametresini belirtebilirsiniz. Varsayılan Blazor şablonları `MainLayout` bileşenini belirtir. *Mainlayout. Razor* , şablon projenin *paylaşılan* klasöründedir. Düzenler hakkında daha fazla bilgi için bkz. <xref:blazor/layouts>.

Birden çok yol şablonu, bir bileşene uygulanabilir. Aşağıdaki bileşen `/BlazorRoute` ve `/DifferentBlazorRoute` isteklerine yanıt verir:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> URL 'Lerin doğru şekilde çözülmesi için, uygulamanın, *Wwwroot/index.html* dosyası (Blazor WebAssembly) veya *Pages/_host. cshtml* dosyasında (Blazor Server), `href` özniteliğinde belirtilen uygulama temel yolu ile bir `<base>` etiketi (`<base href="/">`) içermesi gerekir. Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>İçerik bulunamadığında özel içerik sağla

@No__t-0 bileşeni, istenen rota için içerik bulunmazsa uygulamanın özel içerik belirtmesini sağlar.

*App. Razor* dosyasında, `Router` bileşeninin `NotFound` Şablon parametresinde özel içerik ayarlayın:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

@No__t-0 etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir. @No__t-0 içeriğine varsayılan bir düzen uygulamak için bkz. <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Birden çok derlemeden bileşenlere rota

@No__t-1 bileşeninin yönlendirilebilir bileşenleri ararken dikkate alınması için ek derlemeler belirtmek üzere `AdditionalAssemblies` parametresini kullanın. Belirtilen derlemeler @no__t -0-belirtilen derlemeye ek olarak değerlendirilir. Aşağıdaki örnekte, `Component1`, başvurulan bir sınıf kitaplığında tanımlanan yönlendirilebilir bir bileşendir. Aşağıdaki `AdditionalAssemblies` örneği, `Component1` için yönlendirme desteğiyle sonuçlanır:

< yönlendirici AppAssembly = "typeof (program). Derleme "AdditionalAssemblies =" New [] {typeof (Component1). Bütünleştirilmiş kod} >... </Router>

## <a name="route-parameters"></a>Rota parametreleri

Yönlendirici, karşılık gelen bileşen parametrelerini aynı ada (büyük/küçük harfe duyarsız) doldurmak için yol parametrelerini kullanır:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

ASP.NET Core 3,0 ' deki Blazor uygulamaları için isteğe bağlı parametreler desteklenmez. Önceki örnekte iki `@page` yönergesi uygulanır. İlki, bir parametre olmadan bileşene gezinmesine izin verir. İkinci `@page` yönergesi `{text}` yol parametresini alır ve değeri `Text` özelliğine atar.

## <a name="route-constraints"></a>Yol kısıtlamaları

Yol kısıtlaması bir yönlendirme segmentinde bir bileşene tür eşleştirmeyi zorlar.

Aşağıdaki örnekte, `Users` bileşeninin yolu yalnızca şu durumlarda eşleşir:

* İstek URL 'sinde bir `Id` yol kesimi var.
* @No__t-0 segmenti bir tamsayıdır (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Aşağıdaki tabloda gösterilen yol kısıtlamaları mevcuttur. Sabit kültür ile eşleşen yol kısıtlamaları için daha fazla bilgi için tablonun altındaki uyarıya bakın.

| Kısıtlaması | Örnek           | Örnek eşleşmeler                                                                  | Bilmesi<br>culture<br>eşleştirme |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Hayır                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Evet                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Evet                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Evet                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Evet                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Hayır                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Evet                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Evet                              |

> [!WARNING]
> URL 'YI doğrulayan ve bir CLR türüne (`int` veya `DateTime`) dönüştürülen yol kısıtlamaları her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL 'nin yerelleştirilemeyen olduğunu varsayar.

### <a name="routing-with-urls-that-contain-dots"></a>Noktalar içeren URL 'lerle yönlendirme

Blazor Server uygulamalarında, *_Host. cshtml* içindeki varsayılan yol `/` ' dir (`@page "/"`). URL bir dosya isteyecek şekilde göründüğünden, nokta (`.`) içeren bir istek URL 'SI varsayılan yol tarafından eşleşmiyor. Bir Blazor uygulaması mevcut olmayan bir statik dosya için *404-Found* yanıtı döndürür. Bir nokta içeren yolları kullanmak için *_Host. cshtml* 'yi aşağıdaki yol şablonuyla yapılandırın:

```cshtml
@page "/{**path}"
```

@No__t-0 şablonu şunları içerir:

* Çift yıldız işareti (`**`), eğik çizgi (`/`) kodlaması olmadan birden çok klasör sınırlarındaki *yolu yakalar.*
* @No__t-0 rota parametresi adı.

Daha fazla bilgi için bkz. <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Gezinti bağlantısı bileşeni

Gezinti bağlantıları oluştururken, HTML köprü öğelerinin yerine `NavLink` bileşeni kullanın (`<a>`). @No__t-0 bileşeni, `href` ' in geçerli URL ile eşleşip eşleşmediğini temel alan `active` CSS sınıfına geçiş yaparken, bir `<a>` öğesi gibi davranır. @No__t-0 sınıfı, bir kullanıcının hangi sayfanın etkin sayfa olduğunu anlamasına yardımcı olur.

Aşağıdaki `NavMenu` bileşeni, `NavLink` bileşenlerinin nasıl kullanılacağını gösteren bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğu oluşturur:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

@No__t-2 öğesinin `Match` özniteliğine atayabilmeniz için iki `NavLinkMatch` seçeneği vardır:

* `NavLinkMatch.All` &ndash; `NavLink` geçerli URL ile eşleştiğinde etkin olur.
* `NavLinkMatch.Prefix` (*varsayılan*) &ndash;, geçerli URL 'nin herhangi bir önekiyle eşleştiğinde `NavLink` etkin olur.

Yukarıdaki örnekte, `NavLink` `href=""` giriş URL 'siyle eşleşir ve yalnızca uygulamanın varsayılan temel yol URL 'sindeki `active` CSS sınıfını alır (örneğin, `https://localhost:5001/`). İkinci `NavLink`, Kullanıcı `MyComponent` ön ekine sahip herhangi bir URL 'YI ziyaret ettiğinde `active` sınıfını alır (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).

Ek `NavLink` bileşen öznitelikleri, işlenen tutturucu etiketine geçirilir. Aşağıdaki örnekte, `NavLink` bileşeni `target` özniteliğini içerir:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Aşağıdaki HTML biçimlendirmesi işlenir:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>URI ve gezinti durumu yardımcıları

URI ile çalışmak için `Microsoft.AspNetCore.Components.NavigationManager` kullanın ve C# kodda gezinme yapın. `NavigationManager`, aşağıdaki tabloda gösterilen olay ve yöntemleri sağlar.

| Üyesi | Açıklama |
| ------ | ----------- |
| `Uri` | Geçerli mutlak URI 'yi alır. |
| `BaseUri` | Mutlak bir URI oluşturmak için göreli URI yollarına eklenebilir olan temel URI 'yi (sondaki eğik çizgiyle birlikte) alır. Genellikle `BaseUri`, belgenin *Wwwroot/index.html* (Blazor WebAssembly) veya *Pages/_host. cshtml* (Blazor Server) içindeki `<base>` öğesinde `href` özniteliğine karşılık gelir. |
| `NavigateTo` | Belirtilen URI 'ye gider. @No__t-0 `true` ise:<ul><li>İstemci tarafı yönlendirme atlanır.</li><li>Bu tarayıcı, URI 'nin normalde istemci tarafı yönlendirici tarafından işlenip işlenmediğini sunucudan yeni sayfayı yüklemeye zorlanır.</li></ul> |
| `LocationChanged` | Gezinti konumu değiştiğinde harekete gelen bir olay. |
| `ToAbsoluteUri` | Göreli bir URI 'yi mutlak bir URI 'ye dönüştürür. |
| `ToBaseRelativePath` | Bir temel URI (örneğin, daha önce `GetBaseUri` tarafından döndürülen bir URI) verildiğinde, mutlak bir URI 'yi taban URI ön ekine göre bir URI 'ye dönüştürür. |

Aşağıdaki bileşen, düğme seçildiğinde uygulamanın `Counter` bileşenine gider:

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
