---
title: ASP.NET Core Blazor yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/routing
ms.openlocfilehash: d2f0ce608d7368871f508754d7bbe4f75cc9701f
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538525"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Blazor yönlendirme

Tarafından [Luke Latham](https://github.com/guardrex)

İstek yönlendirme hakkında ve nasıl kullanılacağını öğrenin `NavLink` Blazor uygulamalarda gezinme bağlantıları oluşturmak için bileşen.

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core uç noktası yönlendirme tümleştirmesi

Sunucu tarafı Blazor bütünleştirilmiştir [ASP.NET Core uç noktası yönlendirme](xref:fundamentals/routing). ASP.NET Core uygulaması ile etkileşimli bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapBlazorHub` içinde `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Rota şablonlarının

`Router` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır. `Router` Bileşeni görünür *App.razor* dosyası:

Blazor sunucu tarafı uygulamasında:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

Bir Blazor istemci-tarafı uygulaması:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Olduğunda bir *.razor* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın sağlanan bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu. Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve istenen URL ile eşleşen bir rota şablonuyla bileşeni işler.

Bir bileşenin birden çok yol şablonu uygulanabilir. Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* (Blazor istemci-tarafı) dosya veya *Pages/_Host.cshtml* dosyası (Blazor sunucu-tarafı) ile uygulama temel yolu Belirtilen `href` özniteliği (`<base href="/">`). Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>İçerik bulunamadığında, özel içerik sağlayın

`Router` Bileşen, uygulama içeriği için istenen yol bulunamazsa özel içeriği belirtmek sağlar.

İçinde *App.razor* dosyası içinde özel içerik kümesi `<NotFoundContent>` öğesinin `Router` bileşeni:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

İçeriği `<NotFoundContent>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.

## <a name="route-parameters"></a>Yol parametreleri

Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

İsteğe bağlı parametreler, ASP.NET Core 3.0 Önizleme Blazor uygulamalar için desteklenmez. İki `@page` yönergeleri, önceki örnekte uygulanır. İlk Gezinti parametresi olmadan bileşenine izin verir. İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.

## <a name="route-constraints"></a>Rota kısıtlamaları

Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.

Aşağıdaki örnekte, rotaya `Users` bileşen yalnızca eşleşip eşleşmediğini:

* Bir `Id` yol kesimi istek URL'si hakkındaki varsa.
* `Id` Segmenttir tamsayı (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Aşağıdaki tabloda gösterilen rota kısıtlamalarını kullanılabilir. Sabit kültür ile eşleşen rota kısıtlamaları için daha fazla bilgi için tablonun altındaki bir uyarı görürsünüz.

| Kısıtlama | Örnek           | Örnek eşleşmeleri                                                                  | Değişmez değer<br>kültür<br>eşleştirme |
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
> Rota kısıtlamalarını URL'yi doğrulayın ve CLR türüne dönüştürülür (gibi `int` veya `DateTime`) her zaman sabit kültürü kullanır. Bu kısıtlamalar, URL yerelleştirilemeyen olduğunu varsayın.

## <a name="navlink-component"></a>NavLink bileşeni

Kullanım bir `NavLink` HTML Köprü öğeleri yerine bileşen (`<a>`) gezinme bağlantıları oluşturulurken. A `NavLink` bileşeni davranacağını gibi bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen. `active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.

Aşağıdaki `NavMenu` bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) nasıl kullanılacağını gösteren bir gezinti çubuğu `NavLink` bileşenler:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

İki `NavLinkMatch` atayabileceğiniz seçenekleri `Match` özniteliği `<NavLink>` öğesi:

* `NavLinkMatch.All` &ndash; `NavLink` Tüm geçerli URL eşleştiğinde etkindir.
* `NavLinkMatch.Prefix` (*varsayılan*) &ndash; `NavLink` geçerli URL herhangi bir önek eşleştiğinde etkindir.

Yukarıdaki örnekte, giriş `NavLink` `href=""` giriş URL ile eşleşen ve yalnızca alan `active` CSS sınıfı uygulamanın varsayılan temel yol URL'si (örneğin, `https://localhost:5001/`). İkinci `NavLink` alır `active` kullanıcı herhangi bir URL ile ziyaret ettiğinde sınıfı bir `MyComponent` önek (örneğin, `https://localhost:5001/MyComponent` ve `https://localhost:5001/MyComponent/AnotherSegment`).

## <a name="uri-and-navigation-state-helpers"></a>URI ve gezinti durumu Yardımcıları

Kullanım `Microsoft.AspNetCore.Components.IUriHelper` gezintisi ve bir URI'leri ile çalışmak için C# kod. `IUriHelper` Olay ve aşağıdaki tabloda gösterilen yöntemler sağlar.

| Üye | Açıklama |
| ------ | ----------- |
| `GetAbsoluteUri` | Geçerli bir mutlak URI alır. |
| `GetBaseUri` | (Eğik ile) bir mutlak URI oluşturmak için göreli URI yolları başına temel URI'sini alır. Genellikle, `GetBaseUri` karşılık gelen `href` belgenin özniteliği `<base>` öğesinde *wwwroot/index.html* (Blazor istemci-tarafı) veya *Pages/_Host.cshtml* () Blazor sunucu tarafı). |
| `NavigateTo` | Belirtilen URI'ye gider. Varsa `forceLoad` olduğu `true`:<ul><li>İstemci tarafı yönlendirmesi atlanır.</li><li>URI genellikle istemci tarafı yönlendirici tarafından işlenen olup olmadığını tarayıcı sunucusundan yeni sayfa yükleme zorlanır.</li></ul> |
| `OnLocationChanged` | Gezinti konumu değiştirildiğinde ateşlenir olay. |
| `ToAbsoluteUri` | Bir göreli URİ'yi mutlak bir URI dönüştürür. |
| `ToBaseRelativePath` | Belirtilen temel URI (örneğin, bir URI daha önce döndürülen tarafından `GetBaseUri`), temel URI'si ön ek göreli bir URI mutlak URİ'ye dönüştürür. |

Uygulamanın aşağıdaki bileşen gider `Counter` düğme seçildiğinde bileşeni:

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="@NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
