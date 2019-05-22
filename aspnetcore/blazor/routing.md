---
title: Blazor yönlendirme
author: guardrex
description: Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: blazor/routing
ms.openlocfilehash: 8402089dd818d519eeecfdd3c85e309bffd4d20d
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969862"
---
# <a name="blazor-routing"></a>Blazor yönlendirme

Tarafından [Luke Latham](https://github.com/guardrex)

Uygulamalar ve NavLink bileşenle ilgili istekleri yönlendirmeyi öğrenin.

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core uç noktası yönlendirme tümleştirmesi

Sunucu tarafı Blazor bütünleştirilmiştir [ASP.NET Core uç noktası yönlendirme](xref:fundamentals/routing). ASP.NET Core uygulaması ile etkileşimli bileşenleri için gelen bağlantıları kabul edecek şekilde yapılandırılmış `MapBlazorHub` içinde `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Rota şablonlarının

`<Router>` Bileşen yönlendirme sağlar ve erişilebilir her bileşeni için bir rota şablonu sağlanır. `<Router>` Bileşeni görünür *App.razor* dosyası:

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

`<Router>` İstenen yol olduğunda işlemek için bir geri dönüş bileşen ayarı destekler çözülmüş değildir. Bu katılımı ayarlayarak senaryoyu `FallbackComponent` geri dönüş bileşen sınıfı türü parametresi.

Aşağıdaki örnek, bir bileşen içinde tanımlanan ayarlar *Pages/MyFallbackRazorComponent.razor* geri dönüş bileşeni için bir `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Yollar düzgün bir şekilde oluşturmak için uygulamayı içermelidir bir `<base>` içindeki kendi *wwwroot/index.html* (Blazor istemci-tarafı) dosya veya *sayfaları /\_Host.cshtml* dosyasıyla (Blazor sunucu-tarafı) Belirtilen uygulama temel yolu `href` özniteliği (`<base href="/">`). Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="route-parameters"></a>Yol parametreleri

Yönlendirici, aynı adı (büyük küçük harfe duyarlı) karşılık gelen bileşen parametrelerle doldurmak için rota parametreleri kullanır:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

İsteğe bağlı parametreler, ASP.NET Core 3.0 Önizleme Blazor uygulamalar için desteklenmez. İki `@page` yönergeleri, önceki örnekte uygulanır. İlk Gezinti parametresi olmadan bileşenine izin verir. İkinci `@page` yönergesi gereken `{text}` rota parametresi ve değeri atar `Text` özelliği.

## <a name="route-constraints"></a>Rota kısıtlamaları

Türü bir bileşeni için bir yol kesimi üzerinde eşleşen bir rota kısıtlaması zorlar.

Aşağıdaki örnekte, kullanıcılar bileşen yolu yalnızca, eşleşen:

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

Bir NavLink bileşeni yerine HTML kullanan `<a>` gezinme bağlantıları oluşturulurken öğeleri. NavLink bileşeni gibi davranan bir `<a>` öğesi, onu değiştirir dışında bir `active` CSS sınıfı bağlı kendi `href` geçerli URL ile eşleşen. `active` Sınıfı, bir kullanıcının hangi sayfa etkin sayfa görüntülenen gezinti bağlantıları arasında olduğunu anlamak yardımcı olur.

Aşağıdaki NavMenu bileşeni oluşturur bir [önyükleme](https://getbootstrap.com/docs/) gezinti çubuğunda, NavLink bileşenleri gösterilmektedir:

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

İki `NavLinkMatch` seçenekleri:

* `NavLinkMatch.All` &ndash; Tüm geçerli URL eşleştiğinde NavLink etkin olması gerektiğini belirtir.
* `NavLinkMatch.Prefix` &ndash; Geçerli URL herhangi bir önek eşleştiğinde NavLink etkin olması gerektiğini belirtir.

Yukarıdaki örnekte, giriş NavLink (`href=""`) tüm URL'lerle eşleşir ve her zaman alan `active` CSS sınıfı. İkinci NavLink yalnızca alan `active` sınıfı kullanıcı Blazor rota bileşen ziyaret ettiğinde (`href="BlazorRoute"`).
