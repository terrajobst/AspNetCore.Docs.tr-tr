---
title: ASP.NET Core Blazor barındırma modelleri
author: guardrex
description: İstemci tarafı ve sunucu tarafı modelleri barındırma Blazor anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 80f5e3260ce991ef67fa2a0dd36f8be1f70b6271
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813376"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core Blazor barındırma modelleri

Tarafından [Daniel Roth](https://github.com/danroth27)

Blazor istemci-tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan tarayıcıda bir [WebAssembly](https://webassembly.org/)-.NET çalışma zamanı tabanlı (*Blazor istemci-tarafı*) veya sunucu tarafı ASP.NET Core (*Blazor sunucu tarafı* ). Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı*.

Bu makalede açıklanan barındırma modellerine yönelik bir proje oluşturmak için bkz: <xref:blazor/get-started>.

## <a name="client-side"></a>İstemci tarafı

Asıl barındırma için Blazor WebAssembly tarayıcıda çalışan istemci-tarafı modelidir. Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir. Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür. Kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde oluşur. Statik dosya olarak bir web sunucusu veya hizmeti statik içeriği istemcilere hizmet uygulamasının varlıklarını dağıtılır.

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:

* **(İstemci-tarafı) Blazor** ([dotnet yeni blazor](/dotnet/core/tools/dotnet-new)) &ndash; statik dosyalar bir dizi dağıtılabilir.
* **Blazor (ASP.NET Core barındırılan)** ([dotnet yeni blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; bir ASP.NET Core sunucusu tarafından barındırılan. ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet. Web API çağrıları kullanarak ağ üzerinden Blazor istemci tarafı uygulama sunucusu ile etkileşim kurabilir veya [SignalR](xref:signalr/introduction).

Şablonları içerir *blazor.webassembly.js* işleyen betik:

* .NET çalışma zamanı, uygulama ve uygulamanın bağımlılıklarını karşıdan yükleniyor.
* Uygulamayı çalıştırmak için çalışma zamanı başlatma.

İstemci tarafı barındırma modeli, çeşitli avantajlar sunar:

* Hiçbir .NET sunucu tarafı bağımlılık yoktur. Uygulama için istemciyi indirdikten sonra tam olarak çalışır.
* İstemci kaynakları ve özellikleri tam olarak yararlanabilirsiniz.
* İş, sunucudan istemciye boşaltılır.
* Uygulamayı barındırmak için bir ASP.NET Core web sunucusu gerekli değildir. Sunucusuz dağıtım senaryolarında (örneğin, uygulamayı bir CDN'den sunulması) mümkündür.

İstemci tarafı barındırma downsides vardır:

* Uygulama, tarayıcının yeteneklerini sınırlıdır.
* Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gereklidir.
* İndirme boyutu ve uygulamaların yüklenmesi daha uzun sürecektir.
* .NET çalışma zamanı ve araç desteği daha az olgun. Sınırlamalar, mevcut [.NET Standard](/dotnet/standard/net-standard) desteği ve hata ayıklama.

## <a name="server-side"></a>Sunucu tarafı

Sunucu tarafı barındırma modeli ile uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

![Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/server-side.png)

Sunucu tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core kullanan **Blazor (sunucu tarafı)** şablonu ([dotnet yeni blazorserverside](/dotnet/core/tools/dotnet-new)). ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındıran ve istemcilerin eriştikleri SignalR uç noktası oluşturur.

ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:

* Sunucu tarafı hizmetler.
* Ardışık Düzen işleme isteği için uygulama.

*Blazor.server.js* betik&dagger; istemci bağlantı kurar. Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.

Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar:

* Bir istemci-tarafı uygulaması önemli ölçüde daha küçük indirme boyutu ve uygulamayı çok daha hızlı yükler.
* Uygulama sunucusu özellikleri, herhangi bir .NET Core uyumlu API kullanımı dahil olmak üzere tam yararlanır.
* Sunucu üzerinde .NET core araçları, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle, uygulamayı çalıştırmak için kullanılır.
* İnce istemciler desteklenir. Örneğin, sunucu tarafı uygulamalar ile WebAssembly desteklemeyen tarayıcılar ve kaynak kısıtlı cihazlarda çalışır.
* Uygulamanın .NET /C# kod tabanına uygulama bileşeni kod dahil olmak üzere, istemcilere hizmet değil.

Sunucu tarafı barındırma downsides vardır:

* Daha yüksek gecikme süresi genellikle var. Her bir kullanıcı etkileşimi bir ağ atlama içerir.
* Çevrimdışı desteği yoktur. İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik, çok sayıda kullanıcı içeren uygulamalar için zordur. Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.
* Bir ASP.NET Core sunucusu uygulama hizmet vermek için gereklidir. Sunucusuz dağıtım senaryolarında (örneğin, uygulamayı bir CDN'den sunulması) mümkün değildir.

&dagger;*Blazor.server.js* betik sunulan ASP.NET Core paylaşılan Framework katıştırılmış bir kaynaktan.

### <a name="reconnection-to-the-same-server"></a>Aynı sunucuya yeniden bağlanma

Sunucu tarafı uygulamalar Blazor sunucunun etkin bir SignalR bağlantısı gerektirir. Bağlantı kaybedilirse uygulamanın sunucuya yeniden dener. Bellekte hala istemcinin durum olduğu sürece, istemci oturum durumunu kaybetmeden sürdürür.
 
İstemci bağlantısı kesildi algıladığında, istemci yeniden bağlanmayı dener ancak bir varsayılan kullanıcı arabirimini kullanıcıya görüntülenir. Yeniden bağlanma başarısız olursa, kullanıcı yeniden denemek için seçeneği sağlanır. Kullanıcı arabirimini özelleştirmek için bir öğe ile tanımlama `components-reconnect-modal` olarak kendi `id`. İstemci bu öğe, bağlantı durumuna bağlı aşağıdaki CSS sınıflarının biri ile güncelleştirir:
 
* `components-reconnect-show` &ndash; Bağlantı kesildi ve istemci yeniden bağlanmayı deniyor göstermek için uygulamanın UI göstermesi.
* `components-reconnect-hide` &ndash; İstemci UI Gizle etkin bir bağlantı vardır.
* `components-reconnect-failed` &ndash; Yeniden bağlanma başarısız oldu. Yeniden bağlanmayı yeniden denemek için çağrı `window.Blazor.reconnect()`.

### <a name="stateful-reconnection-after-prerendering"></a>Prerendering sonra durum bilgisi olan yeniden bağlanma
 
Blazor sunucu tarafı uygulamalar varsayılan olarak sunucuya istemci bağlantı kurulmadan önce sunucuda UI prerender şekilde ayarlanmıştır. Bu, ayarlanır *_Host.cshtml* Razor sayfası:
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
İstemci uygulama prerender için kullanılan aynı duruma sunucusuna bağlanır. Uygulamanın durumu bellekte hala varsa, SignalR bağlantı kurulduktan sonra bileşen durumu rerendered değil.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Durum bilgisi olan etkileşimli bileşenleri Razor sayfaları ve görünümler oluşturma
 
Durum bilgisi olan etkileşimli bileşenleri bir Razor sayfası ya da Görünüm eklenebilir. Sayfa veya Görünüm oluşturulduğunda, bileşeni ile prerendered. Durumu bellekte hala olduğu sürece, istemci bağlantısı kurulduktan sonra uygulama için bileşen durumu daha sonra yeniden bağlanır.
 
Örneğin, aşağıdaki Razor sayfası oluşturur bir `Counter` bileşeni ile form kullanarak belirtilen bir başlangıç sayısı:
 
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

### <a name="detect-when-the-app-is-prerendering"></a>Uygulama prerendering Algıla
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>SignalR istemcisi Blazor sunucu tarafı uygulamalar için yapılandırma
 
Bazen, Blazor sunucu tarafı uygulamalar tarafından kullanılan SignalR istemci yapılandırma gerekebilir. Örneğin, bir bağlantı sorunu tanılamak için SignalR istemci günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.
 
SignalR istemcisinde yapılandırmak zorunda *Pages/_Host.cshtml* dosyası:

* Ekleme bir `autostart="false"` özniteliğini `<script>` etiketinde *blazor.server.js* betiği.
* Çağrı `Blazor.start` ve SignalR Oluşturucu belirten bir yapılandırma nesnesi içinde geçirin.
 
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
