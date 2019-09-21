---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor WebAssembly ve Blazor Server barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: blazor/hosting-models
ms.openlocfilehash: 47c546a086588919e4458d6aeeb39453cbc754e0
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168144"
---
# <a name="aspnet-core-blazor-hosting-models"></a>Blazor barındırma modellerini ASP.NET Core

[Daniel Roth](https://github.com/danroth27) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET Runtime (*Blazor webassembly*) veya ASP.NET Core (*Blazor Server*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir. Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.

Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz <xref:blazor/get-started>.

## <a name="blazor-webassembly"></a>Blazor WebAssembly

Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır. Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir. Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür. UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur. Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.

![Blazor WebAssembly: Blazor uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/blazor-webassembly.png)

İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.

**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır. ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar. Blazor WebAssembly uygulaması, Web API çağrılarını veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.

Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:

* .NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.
* Uygulamayı çalıştırmak için çalışma zamanının başlatılması.

Blazor WebAssembly barındırma modeli çeşitli avantajlar sunar:

* .NET sunucu tarafı bağımlılığı yoktur. Uygulama, istemciye indirildikten sonra tamamen çalışır.
* İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.
* İş sunucudan istemciye boşaltılır.
* Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir. Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).

Blazor WebAssembly barındırması için aşağı taraf vardır:

* Uygulama tarayıcının özelliklerine kısıtlıdır.
* Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.
* İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.
* .NET çalışma zamanı ve araç desteği daha az olgun. Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.

## <a name="blazor-server"></a>Blazor Server

Blazor sunucusu barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür. Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/blazor-server.png)

Blazor Server barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, ASP.NET Core **Blazor Server uygulama** şablonunu kullanın ([DotNet yeni blazorserver](/dotnet/core/tools/dotnet-new)). ASP.NET Core uygulaması, Blazor sunucu uygulamasını barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.

ASP.NET Core uygulama, eklenecek uygulamanın `Startup` sınıfına başvurur:

* Sunucu tarafı hizmetler.
* İstek işleme işlem hattının uygulaması.

*Blazor. Server. js* betiği&dagger; , istemci bağlantısını oluşturur. Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.

Blazor sunucusu barındırma modeli çeşitli avantajlar sunar:

* İndirme boyutu bir Blazor WebAssembly uygulamasından önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.
* Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.
* Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.
* Ölçülü istemciler desteklenir. Örneğin, Blazor Server Apps, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.
* Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.

Blazor sunucusu barındırma için aşağı taraf vardır:

* Daha yüksek gecikme süresi genellikle vardır. Her Kullanıcı etkileşimi bir ağ atmasını içerir.
* Çevrimdışı destek yoktur. İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.
* Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır. Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.
* Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir. Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).

&dagger;*Blazor. Server. js* betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.

### <a name="comparison-to-server-rendered-ui"></a>Sunucu tarafından işlenmiş Kullanıcı arabirimine karşılaştırma

Blazor Server uygulamalarını anlamanın bir yolu, Razor görünümlerini veya Razor Pages kullanarak ASP.NET Core uygulamalarda Kullanıcı arabirimi oluşturma için geleneksel modellerden nasıl farklılık gösterir. Her iki model de, HTML içeriğini anlatmak için Razor dilini kullanır, ancak biçimlendirmenin nasıl işlendiği konusunda önemli ölçüde farklılık gösterir.

Bir Razor sayfası veya görünüm işlendiğinde, her Razor kodu satırı metin biçiminde HTML yayar. Oluşturulduktan sonra sunucu, üretilen herhangi bir durum da dahil olmak üzere sayfayı veya görünüm örneğini ortadan kaldırır. Sayfa için başka bir istek gerçekleştiğinde, örneğin sunucu doğrulaması başarısız olduğunda ve doğrulama özeti görüntülendiğinde:

* Sayfanın tamamı HTML metnine yeniden eklenir.
* Sayfa istemciye gönderilir.

Blazor uygulaması, *bileşen*olarak adlandırılan UI 'ın yeniden kullanılabilir öğelerinden oluşur. Bir bileşen kod C# , biçimlendirme ve diğer bileşenleri içerir. Bir bileşen işlendiğinde, Blazor bir HTML veya XML Belge Nesne Modeli (DOM) gibi dahil edilen bileşenlerin bir grafiğini üretir. Bu grafik, özelliklerde ve alanlarında tutulan bileşen durumunu içerir. Blazor, biçimlendirmenin ikili gösterimini üretmek için bileşen grafiğini değerlendirir. İkili biçimi şu şekilde olabilir:

* HTML metnine açıldı (prerendering sırasında).
* Düzenli işleme sırasında biçimlendirmeyi verimli bir şekilde güncelleştirmek için kullanılır.

Blazor içinde bir kullanıcı arabirimi güncelleştirmesi şu şekilde tetiklenir:

* Düğme seçme gibi kullanıcı etkileşimi.
* Zamanlayıcı gibi uygulama Tetikleyicileri.

Grafik yeniden tanımlanır ve bir UI *farkı* (fark) hesaplanır. Bu fark, istemcideki Kullanıcı arabirimini güncelleştirmek için gereken en küçük DOM düzenlemelerinin kümesidir. Fark istemciye bir ikili biçimde gönderilir ve tarayıcı tarafından uygulanır.

Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur. Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır. Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır. Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere bir Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz <xref:security/blazor/server>.

### <a name="circuits"></a>Uygulanıp

Bir Blazor sunucu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)'nin üzerine kurulmuştur. Her istemci, *devre*adlı bir veya daha fazla SignalR bağlantısı üzerinden sunucusuyla iletişim kurar. Bir devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazor. Bir Blazor istemcisi, SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.

Bir Blazor sunucu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya iframe) bir SignalR bağlantısı kullanır. Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir. Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez. Bir Blazor sunucu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.

Blazor bir tarayıcı sekmesini kapatmayı veya bir dış URL 'ye gidilerek *düzgün* bir şekilde sonlandırılmasını kabul eder. Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır. Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir. Blazor sunucusu, istemcinin yeniden bağlanmasına izin vermek üzere yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar. Daha fazla bilgi için [aynı sunucuya yeniden bağlanma](#reconnection-to-the-same-server) bölümüne bakın.

### <a name="ui-latency"></a>UI gecikmesi

UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir. Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur. Bir Blazor sunucu uygulamasında her bir eylem sunucuya gönderilir, işlenir ve bir UI farkı geri gönderilir. Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.

Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur. Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.

Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir. Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği Daha fazla bilgi için bkz. <xref:security/blazor/server>.

Blazor sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir. Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz <xref:host-and-deploy/blazor/server#measure-network-latency>. SignalR ve Blazor hakkında daha fazla bilgi için bkz.

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="reconnection-to-the-same-server"></a>Aynı sunucuya yeniden bağlanma

Blazor Server uygulamaları, sunucusuna etkin bir SignalR bağlantısı gerektirir. Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır. İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.

İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir. Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır. Kullanıcı arabirimini özelleştirmek için, `components-reconnect-modal` *_host. cshtml* Razor sayfasında `id` olarak bir öğesi tanımlayın. İstemci bu öğeyi bağlantı durumuna göre aşağıdaki CSS sınıflarından biriyle güncelleştirir:

* `components-reconnect-show`&ndash; Bağlantının kaybolduğunu ve istemcinin yeniden bağlanmaya çalışıyor olduğunu göstermek için Kullanıcı arabirimini görüntüleyin.
* `components-reconnect-hide`&ndash; İstemcinin etkin bir bağlantısı vardır ve Kullanıcı arabirimini gizleyin.
* `components-reconnect-failed`&ndash; Yeniden bağlantı kurulamadı. Yeniden bağlanmayı yeniden denemek için çağrısı `window.Blazor.reconnect()`yapın.

### <a name="stateful-reconnection-after-prerendering"></a>Prerendering sonrasında durum bilgisi olan yeniden bağlanma

Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar. Bu, *_Host. cshtml* Razor sayfasında ayarlanır:

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`bileşenin şunları yapıp kullanmadığını yapılandırır:

* , Sayfaya ön gönderilir.
* , Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.

| `RenderMode`        | Açıklama |
| ------------------- | ----------- |
| `ServerPrerendered` | Bileşeni statik HTML olarak işler ve bir Blazor Server uygulaması için işaret içerir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır. Parametreler desteklenmiyor. |
| `Server`            | Bir Blazor sunucu uygulaması için işaretleyici işler. Bileşen çıkışı dahil değildir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır. Parametreler desteklenmiyor. |
| `Static`            | Bileşeni statik HTML olarak işler. Parametreler destekleniyor. |

Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.

İstemci, uygulamayı PreRender 'da kullanılan aynı durum ile sunucuya yeniden bağlanır. Uygulamanın durumu hala bellekte ise, SignalR bağlantısı kurulduktan sonra bileşen durumu tekrar verilmez.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme

Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.

Sayfa veya görünüm şunları işler:

* Bileşen sayfa veya görünümle birlikte kullanılır.
* Prerendering için kullanılan ilk bileşen durumu kayboldu.
* SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.

Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme

Aşağıdaki Razor sayfasında, `MyComponent` bileşen bir form kullanılarak belirtilen bir başlangıç değeriyle statik olarak işlenir:

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Statik `MyComponent` olarak işlendiğinden bileşen etkileşimli olamaz.

### <a name="detect-when-the-app-is-prerendering"></a>Uygulamanın ne zaman prerendering olduğunu Algıla

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a>Blazor Server uygulamaları için SignalR istemcisini yapılandırma

Bazen, Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir. Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.

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
