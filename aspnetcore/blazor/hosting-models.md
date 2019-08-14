---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor istemci tarafı ve sunucu tarafı barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 64393e826cb17550085f468f5916fca55973908f
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993384"
---
# <a name="aspnet-core-blazor-hosting-models"></a>Blazor barındırma modellerini ASP.NET Core

[Daniel Roth](https://github.com/danroth27) tarafından

Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı (*Blazor istemci tarafı*) veya ASP.NET Core (*Blazor sunucu tarafı*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir. Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.

Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz <xref:blazor/get-started>.

## <a name="client-side"></a>İstemci tarafı

Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır. Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir. Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür. UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur. Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.

![Blazor istemci tarafı: Blazor uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/client-side.png)

İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.

**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır. ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar. Blazor istemci tarafı uygulaması, Web API çağrıları veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.

Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:

* .NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.
* Uygulamayı çalıştırmak için çalışma zamanının başlatılması.

İstemci tarafı barındırma modeli çeşitli avantajlar sunar:

* .NET sunucu tarafı bağımlılığı yoktur. Uygulama, istemciye indirildikten sonra tamamen çalışır.
* İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.
* İş sunucudan istemciye boşaltılır.
* Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir. Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).

İstemci tarafı barındırma için aşağı taraf vardır:

* Uygulama tarayıcının özelliklerine kısıtlıdır.
* Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.
* İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.
* .NET çalışma zamanı ve araç desteği daha az olgun. Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.

## <a name="server-side"></a>Sunucu tarafı

Sunucu tarafı barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/server-side.png)

Sunucu tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)) kullanın. ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.

ASP.NET Core uygulama, eklenecek uygulamanın `Startup` sınıfına başvurur:

* Sunucu tarafı hizmetler.
* İstek işleme işlem hattının uygulaması.

*Blazor. Server. js* betiği&dagger; , istemci bağlantısını oluşturur. Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.

Sunucu tarafı barındırma modeli çeşitli avantajlar sunar:

* İndirme boyutu bir istemci tarafı uygulamadan önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.
* Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.
* Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.
* Ölçülü istemciler desteklenir. Örneğin, sunucu tarafı uygulamalar, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.
* Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.

Sunucu tarafı barındırma için aşağı taraf vardır:

* Daha yüksek gecikme süresi genellikle vardır. Her Kullanıcı etkileşimi bir ağ atmasını içerir.
* Çevrimdışı destek yoktur. İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır. Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.
* Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir. Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).

&dagger;*Blazor. Server. js* betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.

### <a name="reconnection-to-the-same-server"></a>Aynı sunucuya yeniden bağlanma

Blazor sunucu tarafı uygulamalar sunucuya etkin bir SignalR bağlantısı gerektirir. Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır. İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.
 
İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir. Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır. Kullanıcı arabirimini özelleştirmek için, `components-reconnect-modal` *_host. cshtml* Razor sayfasında `id` olarak bir öğesi tanımlayın. İstemci bu öğeyi bağlantı durumuna göre aşağıdaki CSS sınıflarından biriyle güncelleştirir:
 
* `components-reconnect-show`&ndash; Bağlantının kaybolduğunu ve istemcinin yeniden bağlanmaya çalışıyor olduğunu göstermek için Kullanıcı arabirimini görüntüleyin.
* `components-reconnect-hide`&ndash; İstemcinin etkin bir bağlantısı vardır ve Kullanıcı arabirimini gizleyin.
* `components-reconnect-failed`&ndash; Yeniden bağlantı kurulamadı. Yeniden bağlanmayı yeniden denemek için çağrısı `window.Blazor.reconnect()`yapın.

### <a name="stateful-reconnection-after-prerendering"></a>Prerendering sonrasında durum bilgisi olan yeniden bağlanma
 
Blazor sunucu tarafında bulunan uygulamalar, sunucu bağlantısı yapılmadan önce sunucu üzerindeki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar. Bu, *_Host. cshtml* Razor sayfasında ayarlanır:
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
İstemci, uygulamayı PreRender 'da kullanılan aynı durum ile sunucuya yeniden bağlanır. Uygulamanın durumu hala bellekte ise, SignalR bağlantısı kurulduktan sonra bileşen durumu tekrar verilmez.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme
 
Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir. Sayfa veya görünüm ne zaman işler, bileşen onunla birlikte gelir. Daha sonra uygulama, durum hala bellekte olduğu sürece istemci bağlantısı kurulduktan sonra bileşen durumuna yeniden bağlanır.
 
Örneğin, aşağıdaki Razor sayfası bir form kullanılarak belirtilen `Counter` başlangıç sayısıyla bir bileşen oluşturur:
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a>Uygulamanın ne zaman prerendering olduğunu Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>Blazor sunucu tarafı uygulamaları için SignalR istemcisini yapılandırma
 
Bazen, Blazor sunucu tarafı uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir. Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.
 
*/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:

* `autostart="false"` *Blazor. Server. js* betiği için `<script>` etikete bir öznitelik ekleyin.
* SignalR oluşturucuyu belirten bir yapılandırma nesnesi çağırın `Blazor.start` ve geçirin.
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/get-started>
* <xref:signalr/introduction>
