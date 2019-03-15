---
title: Razor bileşenleri yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/routing
ms.openlocfilehash: 39039c306a0ac0d9838e3c98815a6b1aade8863b
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978364"
---
# <a name="razor-components-routing"></a>Razor bileşenleri yönlendirme

Tarafından [Luke Latham](https://github.com/guardrex)

Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core uç noktası yönlendirme tümleştirmesi

Razor bileşenleri tümleştirilir [ASP.NET Core yönlendirme](xref:fundamentals/routing). ASP.NET Core uygulaması ile etkileşimli Razor bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapComponentHub<TComponent>` içinde `Startup.Configure`. `MapComponentHub` belirten kök bileşeni `App` Seçici eşleşen bir DOM öğesi içinde işleneceğini `app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>Rota şablonlarının

`<Router>` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır. `<Router>` Bileşeni görünür *Components/App.razor* dosyası:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Olduğunda bir *.razor* veya *.cshtml* ile dosya bir `@page` yönergesi derlendiğinde, oluşturulan sınıfın belirli bir <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> belirten rota şablonu. Çalışma zamanında bileşen sınıfları ile yönlendirici arar bir `RouteAttribute` ve hangi bileşen istenen URL ile eşleşen bir rota şablonuna sahip işler.

Bir bileşenin birden çok yol şablonu uygulanabilir. Aşağıdaki bileşen isteklerine yanıt veren `/BlazorRoute` ve `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute&highlight=1-2)]

`<Router>` İstenen yol, işleme için bir geri dönüş bileşen ayarı destekler çözülmüş değildir. Bu katılımı ayarlayarak senaryoyu `FallbackComponent` geri dönüş bileşen sınıfı türü parametresi.

Aşağıdaki örnek, bir bileşen içinde tanımlanan ayarlar *Pages/MyFallbackRazorComponent.cshtml* geri dönüş bileşeni için bir `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* belirtilen uygulama temel yolu dosyasıyla `href` özniteliği (`<base href="/" />`). Daha fazla bilgi için bkz. <xref:host-and-deploy/razor-components/index#app-base-path>.

## <a name="route-parameters"></a>Yol parametreleri

Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

İsteğe bağlı parametreler henüz desteklenmeyen böylece iki `@page` yönergeleri, yukarıdaki örnekte uygulanır. İlk Gezinti parametresi olmadan bileşenine izin verir. İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.

## <a name="route-constraints"></a>Rota kısıtlamaları

Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.

Aşağıdaki örnekte, kullanıcılar bileşen yolu yalnızca, eşleşen:

* Bir `Id` yol kesimi istek URL'si hakkındaki varsa.
* `Id` Segmenttir tamsayı (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

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

Bir NavLink bileşeni yerine HTML kullanan `<a>` gezinme bağlantıları oluşturulurken öğeleri. NavLink bileşeni gibi davranan bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen. `active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.

Aşağıdaki NavMenu bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğunda, NavLink bileşenleri gösterilmektedir:

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

İki `NavLinkMatch` seçenekleri:

* `NavLinkMatch.All` &ndash; Tüm geçerli URL eşleştiğinde NavLink etkin olması gerektiğini belirtir.
* `NavLinkMatch.Prefix` &ndash; Geçerli URL herhangi bir önek eşleştiğinde NavLink etkin olması gerektiğini belirtir.

Yukarıdaki örnekte, giriş NavLink (`href=""`) tüm URL'lerle eşleşir ve her zaman alan `active` CSS sınıfı. İkinci NavLink yalnızca alan `active` sınıfı kullanıcı Blazor rota bileşen ziyaret ettiğinde (`href="BlazorRoute"`).
