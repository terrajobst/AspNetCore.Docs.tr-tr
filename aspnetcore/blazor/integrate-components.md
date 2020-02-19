---
title: ASP.NET Core Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin
author: guardrex
description: Blazor uygulamalarında bileşenler ve DOM öğeleri için veri bağlama senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453298"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>ASP.NET Core Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin

, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından

Razor bileşenleri, Razor Pages ve MVC uygulamalarıyla tümleştirilebilir. Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden alınabilir.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Uygulamayı sayfalar ve görünümlerde bileşenleri kullanmak üzere hazırlama

Mevcut bir Razor Pages veya MVC uygulaması, Razor bileşenlerini sayfalarla ve görünümleriyle tümleştirebilir:

1. Uygulamanın düzen dosyasında ( *_Layout. cshtml*):

   * Aşağıdaki `<base>` etiketini `<head>` öğesine ekleyin:

     ```html
     <base href="~/" />
     ```

     Yukarıdaki örnekteki `href` değeri ( *uygulama temel yolu*), UYGULAMANıN kök URL yolunda (`/`) bulunduğunu varsayar. Uygulama bir alt uygulama ise, <xref:host-and-deploy/blazor/index#app-base-path> makalesinin *uygulama temel yolu* bölümündeki yönergeleri izleyin.

     *_Layout. cshtml* dosyası, bir MVC uygulamasında bir Razor Pages uygulamasının veya *görünümlerinin/paylaşılan* klasörünün *Sayfalar/paylaşılan* klasöründe bulunur.

   * Kapanış `</body>` etiketinden hemen önce *blazor. Server. js* betiği için bir `<script>` etiketi ekleyin:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Framework, *blazor. Server. js* betiğini uygulamaya ekler. Betiği uygulamaya el ile eklemeniz gerekmez.

1. Aşağıdaki içerikle projenin kök klasörüne bir *_Imports. Razor* dosyası ekleyin (son ad alanını, `MyAppNamespace`, uygulamanın ad alanına değiştirin):

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. `Startup.ConfigureServices`, Blazor sunucu hizmetini kaydedin:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. `Startup.Configure`, Blazor hub uç noktasını `app.UseEndpoints`öğesine ekleyin:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Bileşenleri herhangi bir sayfa veya görünümle tümleştirin. Daha fazla bilgi için, [bir sayfadan veya görünümden bileşenleri işleme](#render-components-from-a-page-or-view) bölümüne bakın.

## <a name="use-routable-components-in-a-razor-pages-app"></a>Razor Pages uygulamasında yönlendirilebilir bileşenleri kullanma

*Bu bölüm, Kullanıcı isteklerinden doğrudan yönlendirilebilir bileşenleri eklemeye aittir.*

Razor Pages uygulamalarda yönlendirilebilir Razor bileşenlerini desteklemek için:

1. [Sayfaları sayfalar ve görünümler 'de kullanmak için uygulamayı hazırlama](#prepare-the-app-to-use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.

1. Aşağıdaki içeriğe sahip proje köküne bir *app. Razor* dosyası ekleyin:

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

   Ad alanları hakkında daha fazla bilgi için [bileşen ad alanları](#component-namespaces) bölümüne bakın.

## <a name="use-routable-components-in-an-mvc-app"></a>MVC uygulamasında yönlendirilebilir bileşenleri kullanma

*Bu bölüm, Kullanıcı isteklerinden doğrudan yönlendirilebilir bileşenleri eklemeye aittir.*

MVC uygulamalarında yönlendirilebilir Razor bileşenlerini desteklemek için:

1. [Sayfaları sayfalar ve görünümler 'de kullanmak için uygulamayı hazırlama](#prepare-the-app-to-use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.

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

   Ad alanları hakkında daha fazla bilgi için [bileşen ad alanları](#component-namespaces) bölümüne bakın.

## <a name="component-namespaces"></a>Bileşen ad alanları

Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü/görünümü veya *_ViewImports. cshtml* dosyasını temsil eden ad alanını ekleyin. Aşağıdaki örnekte:

* `MyAppNamespace` uygulamanın ad alanıyla değiştirin.
* *Bileşenler adlı bir* klasör bileşenleri tutmak için kullanılmazsa `Components`, bileşenlerin bulunduğu klasöre geçin.

```cshtml
@using MyAppNamespace.Components
```

*_ViewImports. cshtml* dosyası, bir Razor Pages uygulamasının *Sayfalar* klasöründe veya bir MVC uygulamasının *views* klasöründe bulunur.

Daha fazla bilgi için bkz. <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Bir sayfadan veya görünümden bileşenleri işleme

*Bu bölüm, bileşenlerin Kullanıcı isteklerinden doğrudan yönlendirilemeyen sayfalara veya görünümlere bileşen eklenmesine aittir.*

Bir sayfadan veya görünümden bir bileşeni işlemek için `Component` etiketi yardımcısını kullanın:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Parametre türünün JSON serileştirilebilir olması gerekir, bu, genellikle türün bir varsayılan oluşturucuya ve ayarlanabilir özelliklere sahip olması anlamına gelir. Örneğin, `IncrementAmount` için bir değer belirtebilirsiniz çünkü `IncrementAmount` türü, JSON seri hale getirici tarafından desteklenen bir temel tür `int`.

`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:

* , Sayfaya ön gönderilir.
* , Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.

| `RenderMode`        | Açıklama |
| ------------------- | ----------- |
| `ServerPrerendered` | Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| `Server`            | Blazor sunucusu uygulaması için bir işaret oluşturur. Bileşen çıkışı dahil değildir. Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır. |
| `Static`            | Bileşeni statik HTML olarak işler. |

Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir. Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz. Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.

Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.

Bileşenlerin nasıl işlendiği, bileşen durumu ve `Component` etiketi Yardımcısı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
