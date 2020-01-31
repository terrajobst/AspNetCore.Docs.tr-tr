---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor WebAssembly ve Blazor sunucusu barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 145f385fd6c5d04510a4ac15a41b879591ab5caa
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885520"
---
# <a name="aspnet-core-blazor-hosting-models"></a>Blazor barındırma modellerini ASP.NET Core

[Daniel Roth](https://github.com/danroth27) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET Runtime (*Blazor webassembly*) veya ASP.NET Core (*Blazor Server*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir. Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.

Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz. <xref:blazor/get-started>.

## <a name="blazor-webassembly"></a>Blazor WebAssembly

Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır. Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir. Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür. UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur. Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.

![Blazor WebAssembly: Blazor uygulaması tarayıcının içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/blazor-webassembly.png)

İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.

**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır. ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar. Blazor WebAssembly uygulaması, Web API çağrılarını veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.

Şablonlar aşağıdakileri işleyecek `blazor.webassembly.js` betiği içerir:

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

ASP.NET Core uygulama, şu ekleme için uygulamanın `Startup` sınıfına başvurur:

* Sunucu tarafı hizmetler.
* İstek işleme işlem hattının uygulaması.

`blazor.server.js` betiği&dagger; istemci bağlantısını kurar. Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.

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

&dagger;`blazor.server.js` betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.

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

Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur. Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır. Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır. Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere bir Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz. <xref:security/blazor/server>.

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin

#### <a name="use-components-in-pages-and-views"></a>Sayfalar ve görünümlerde bileşenleri kullanma

Mevcut bir Razor Pages veya MVC uygulaması, Razor bileşenlerini sayfalarla ve görünümleriyle tümleştirebilir:

1. Uygulamanın düzen dosyasında ( *_Layout. cshtml*):

   * Aşağıdaki `<base>` etiketini `<head>` öğesine ekleyin:

     ```html
     <base href="~/" />
     ```

     Yukarıdaki örnekteki `href` değeri ( *uygulama temel yolu*), UYGULAMANıN kök URL yolunda (`/`) bulunduğunu varsayar. Uygulama bir alt uygulama ise, <xref:host-and-deploy/blazor/index#app-base-path> makalesinin *uygulama temel yolu* bölümündeki yönergeleri izleyin.

     *_Layout. cshtml* dosyası, bir MVC uygulamasında bir Razor Pages uygulamasının veya *görünümlerinin/paylaşılan* klasörünün *Sayfalar/paylaşılan* klasöründe bulunur.

   * Kapanış `</body>` etiketinin içindeki *blazor. Server. js* betiği için bir `<script>` etiketi ekleyin:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Framework, *blazor. Server. js* betiğini uygulamaya ekler. Betiği uygulamaya el ile eklemeniz gerekmez.

1. Aşağıdaki içerikle projenin kök klasörüne bir *_Imports. Razor* dosyası ekleyin (son ad alanını, `MyAppNamespace`, uygulamanın ad alanına değiştirin):

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. `Startup.ConfigureServices`, Blazor Server hizmetini ekleyin:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. `Startup.Configure`, Blazor hub uç noktasını `app.UseEndpoints`öğesine ekleyin:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Bileşenleri herhangi bir sayfa veya görünümle tümleştirin. Daha fazla bilgi için, <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> makalesinin *bileşenleri Razor Pages ve MVC uygulamalarına tümleştirme* bölümüne bakın.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Razor Pages uygulamasında yönlendirilebilir bileşenleri kullanma

Razor Pages uygulamalarda yönlendirilebilir Razor bileşenlerini desteklemek için:

1. [Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.

1. Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. *Sayfalar* klasörüne aşağıdaki içeriğe sahip bir *_Host. cshtml* dosyası ekleyin:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.

1. `Startup.Configure`' de uç nokta yapılandırmasına *_Host. cshtml* sayfası için düşük öncelikli bir yol ekleyin:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Uygulamaya yönlendirilebilir bileşenler ekleyin. Örneğin:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *sayfaları/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin. Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>MVC uygulamasında yönlendirilebilir bileşenleri kullanma

MVC uygulamalarında yönlendirilebilir Razor bileşenlerini desteklemek için:

1. [Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.

1. Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Aşağıdaki içeriğe sahip *Görünümler/giriş* klasörüne bir *_Host. cshtml* dosyası ekleyin:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.

1. Ana denetleyiciye bir eylem ekleyin:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. `Startup.Configure`' deki uç nokta yapılandırmasına *_Host. cshtml* görünümünü döndüren denetleyici eylemi için düşük öncelikli bir yol ekleyin:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Bir *Sayfalar* klasörü oluşturun ve uygulamaya yönlendirilebilir bileşenler ekleyin. Örneğin:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *görüntüleme/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin. Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="circuits"></a>Uygulanıp

Bir Blazor sunucu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)'nin üzerine kurulmuştur. Her istemci, *devre*adlı bir veya daha fazla SignalR bağlantısı üzerinden sunucusuyla iletişim kurar. Bir devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazor. Bir Blazor istemcisi, SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.

Bir Blazor sunucu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya iframe) bir SignalR bağlantısı kullanır. Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir. Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez. Bir Blazor sunucu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.

Blazor bir tarayıcı sekmesini kapatmayı veya bir dış URL 'ye gidilerek *düzgün* bir şekilde sonlandırılmasını kabul eder. Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır. Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir. Blazor sunucusu, istemcinin yeniden bağlanmasına izin vermek üzere yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar. Daha fazla bilgi için [aynı sunucuya yeniden bağlanma](#reconnection-to-the-same-server) bölümüne bakın.

### <a name="ui-latency"></a>UI gecikmesi

UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir. Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur. Bir Blazor sunucu uygulamasında her bir eylem sunucuya gönderilir, işlenir ve bir UI farkı geri gönderilir. Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.

Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur. Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.

Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir. Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği Daha fazla bilgi için bkz. <xref:security/blazor/server>.

Blazor sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir. Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz. <xref:host-and-deploy/blazor/server#measure-network-latency>. SignalR ve Blazor hakkında daha fazla bilgi için bkz.

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Sunucuyla bağlantı

Blazor Server uygulamaları, sunucusuna etkin bir SignalR bağlantısı gerektirir. Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır. İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.

#### <a name="reconnection-to-the-same-server"></a>Aynı sunucuya yeniden bağlanma

Sunucu üzerinde kullanıcı arabirimi durumunu ayarlayan ilk istemci isteğine yanıt olarak önceden bir Blazor Server uygulaması ön ekler. İstemci bir SignalR bağlantısı oluşturmayı denediğinde, istemci aynı sunucuya yeniden bağlanmalıdır. Birden fazla arka uç sunucusu kullanan Blazor sunucu uygulamaları, SignalR bağlantıları için *yapışkan oturumlar* uygulamalıdır.

Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz. Hizmet, Blazor sunucu uygulamasını çok sayıda eşzamanlı SignalR bağlantısına ölçeklendirmeye olanak tanır. Azure SignalR hizmeti için, hizmetin `ServerStickyMode` seçeneği veya yapılandırma değeri `Required`olarak ayarlanarak yapışkan oturumlar etkinleştirilir. Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/server#signalr-configuration>.

IIS kullanırken, yapışkan oturumlar uygulama Isteği yönlendirme ile etkinleştirilir. Daha fazla bilgi için bkz. [uygulama Isteği yönlendirme kullanarak HTTP yük dengelemesi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="reflect-the-connection-state-in-the-ui"></a>Kullanıcı arabirimindeki bağlantı durumunu yansıtır

İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir. Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.

Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.

| CSS sınıfı                       | &hellip; gösterir |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | Kayıp bir bağlantı. İstemci yeniden bağlanmaya çalışıyor. Kalıcı olarak göster. |
| `components-reconnect-hide`     | Etkin bir bağlantı sunucuya yeniden oluşturulur. Kalıcı olarak gizleyin. |
| `components-reconnect-failed`   | Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu. Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın. |
| `components-reconnect-rejected` | Yeniden bağlantı reddedildi. Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu. Uygulamayı yeniden yüklemek için `location.reload()`çağırın. Bu bağlantı durumu şu durumlarda oluşabilir:<ul><li>Sunucu tarafında devre dışı bir kilitlenme oluşur.</li><li>Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil. Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</li><li>Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a>Prerendering sonrasında durum bilgisi olan yeniden bağlanma

Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar. Bu, *_Host. cshtml* Razor sayfasında ayarlanır:

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:

* , Sayfaya ön gönderilir.
* , Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.

| `RenderMode`        | Açıklama |
| ------------------- | ----------- |
| `ServerPrerendered` | Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| `Server`            | Blazor sunucusu uygulaması için bir işaret oluşturur. Bileşen çıkışı dahil değildir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| `Static`            | Bileşeni statik HTML olarak işler. |

Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.

`RenderMode` `ServerPrerendered`, bileşen başlangıçta sayfanın bir parçası olarak statik olarak işlenir. Tarayıcı sunucuya geri bir bağlantı kurduğunda, bileşen *yeniden*işlenir ve bileşen artık etkileşimli olur. Bileşeni başlatmak için [Onbaşlatılmış {Async}](xref:blazor/lifecycle#component-initialization-methods) yaşam döngüsü yöntemi varsa, yöntem *iki kez*yürütülür:

* Bileşen statik olarak önceden kullanılırken.
* Sunucu bağlantısı kurulduktan sonra.

Bu, bileşen son işlendiğinde Kullanıcı arabiriminde görünen verilerde fark edilebilir bir değişikliğe neden olabilir.

Blazor sunucu uygulamasında çift işleme senaryosunu önlemek için:

* Prerendering sırasında durumu önbelleğe almak için kullanılabilecek bir tanımlayıcı geçirin ve uygulamayı yeniden başlattıktan sonra durumu alma.
* Bileşen durumunu kaydetmek için prerendering sırasında tanımlayıcıyı kullanın.
* Önbelleğe alınan durumu almak için prerendering öğesinden sonra tanımlayıcıyı kullanın.

Aşağıdaki kod, Çift işlemeyi engelleyen şablon tabanlı Blazor sunucu uygulamasındaki güncelleştirilmiş bir `WeatherForecastService` gösterir:

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme

Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.

Sayfa veya görünüm şunları işler:

* Bileşen sayfa veya görünümle birlikte kullanılır.
* Prerendering için kullanılan ilk bileşen durumu kayboldu.
* SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.

Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme

Aşağıdaki Razor sayfasında, `Counter` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.

### <a name="detect-when-the-app-is-prerendering"></a>Uygulamanın ne zaman prerendering olduğunu Algıla

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Blazor Server uygulamaları için SignalR istemcisini yapılandırma

Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir. Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.

*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:

* `blazor.server.js` betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.
* `Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/get-started>
* <xref:signalr/introduction>
