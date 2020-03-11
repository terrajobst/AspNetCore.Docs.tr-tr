---
title: ASP.NET Core Blazor şablonları
author: guardrex
description: ASP.NET Core Blazor uygulama şablonları ve Blazor proje yapısı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: acfa4b8a42cbd310c6fc6dc973573578e94ef999
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664243"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>ASP.NET Core Blazor şablonları

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor Framework, Blazor barındırma modellerinin her biri için uygulama geliştirmeye yönelik şablonlar sağlar:

* Blazor WebAssembly (`blazorwasm`)
* Blazor sunucusu (`blazorserver`)

Blazorbarındırma modelleri hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.

Şablondan Blazor uygulama oluşturmaya yönelik adım adım yönergeler için, bkz. <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>Blazor proje yapısı

Aşağıdaki dosya ve klasörler, Blazor şablonundan oluşturulan Blazor bir uygulama yapar:

* *Program.cs* , uygulamanın giriş noktasını &ndash;:

  * ASP.NET Core [ana bilgisayar](xref:fundamentals/host/generic-host) (Blazor sunucu)
  * WebAssembly Host (Blazor WebAssembly) &ndash; bu dosyadaki kod Blazor WebAssembly şablonundan oluşturulan uygulamalar için benzersizdir (`blazorwasm`).
    * Uygulamanın kök bileşeni olan `App` bileşeni, `Add` metoduna `app` DOM öğesi olarak belirtilir.
    * Hizmetler, ana bilgisayar Oluşturucu 'daki `ConfigureServices` yöntemiyle yapılandırılabilir (örneğin, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).
    * Yapılandırma, ana bilgisayar Oluşturucu (`builder.Configuration`) aracılığıyla sağlanabilir.

* *Startup.cs* (Blazor Server) &ndash; uygulamanın başlangıç mantığını içerir. `Startup` sınıfı iki yöntemi tanımlar:

  * `ConfigureServices` &ndash;, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) hizmetlerini yapılandırır. Sunucu uygulamalarında Blazor, hizmetler <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>çağırarak eklenir ve `WeatherForecastService` örnek `FetchData` bileşeni tarafından kullanılmak üzere hizmet kapsayıcısına eklenir.
  * `Configure` &ndash;, uygulamanın istek işleme ardışık düzenini yapılandırır:
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>, tarayıcıya gerçek zamanlı bağlantı için bir uç nokta ayarlamak üzere çağırılır. Bağlantı, uygulamalara gerçek zamanlı Web işlevselliği eklemek için bir çerçeveden [SignalR](xref:signalr/introduction)oluşturulur.
    * [Mapfallbacktopage ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) , uygulamanın kök sayfasını (*Pages/_Host. cshtml*) ayarlamak ve gezinmeyi etkinleştirmek için çağırılır.

* *Wwwroot/index.html* (Blazor WebAssembly) bir HTML sayfası olarak uygulanan uygulamanın kök sayfasını &ndash;:
  * Uygulamanın herhangi bir sayfası başlangıçta istendiğinde, Bu sayfa işlenir ve yanıtta döndürülür.
  * Bu sayfa, kök `App` bileşeninin nerede işleneceğini belirtir. `App` bileşeni (*app. Razor*) `Startup.Configure`içindeki `AddComponent` metoduna `app` DOM öğesi olarak belirtilir.
  * `_framework/blazor.webassembly.js` JavaScript dosyası yüklenir ve şunları yapın:
    * .NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirir.
    * Uygulamayı çalıştırmak için çalışma zamanını başlatır.

* *Pages/_Host. cshtml* (Blazor Server), Razor sayfası olarak uygulanan uygulamanın kök sayfasına &ndash;:
  * Uygulamanın herhangi bir sayfası başlangıçta istendiğinde, Bu sayfa işlenir ve yanıtta döndürülür.
  * Tarayıcı ve sunucu arasındaki gerçek zamanlı SignalR bağlantısını ayarlayan `_framework/blazor.server.js` JavaScript dosyası yüklenir.
  * Ana bilgisayar sayfası, kök `App` bileşeni 'nin (*app. Razor*) nerede işleneceğini belirtir.

* *App. razor* &ndash; <xref:Microsoft.AspNetCore.Components.Routing.Router> bileşenini kullanarak istemci tarafı yönlendirmeyi ayarlayan uygulamanın kök bileşenidir. `Router` bileşeni tarayıcı gezintisini karşılar ve istenen adresle eşleşen sayfayı işler.

* *Sayfalar* klasörü &ndash; Blazor uygulamayı oluşturan yönlendirilebilir bileşenleri/sayfaları ( *. Razor*) içerir. Her sayfanın yolu [`@page`](xref:mvc/views/razor#page) yönergesi kullanılarak belirtilir. Şablon aşağıdaki bileşenleri içerir:
  * `Index` (*Index. Razor*) &ndash; giriş sayfasını uygular.
  * `Counter` (*Counter. Razor*) &ndash; sayaç sayfasını uygular.
  * uygulamada işlenmeyen bir özel durum oluştuğunda `Error` (yalnızca*hata. Razor*, Blazor sunucu uygulaması) &ndash;.
  * `FetchData` (*fetchdata. Razor*) &ndash; verileri getir sayfasını uygular.

* *Paylaşılan* klasör &ndash;, uygulama tarafından kullanılan diğer Kullanıcı Arabirimi bileşenlerini ( *. Razor*) içerir:
  * `MainLayout` (*mainlayout. Razor*) uygulamanın düzen bileşeni &ndash;.
  * `NavMenu` (*Navmenu. Razor*) &ndash; kenar çubuğu gezintisini uygular. Diğer Razor bileşenlerine yönelik gezinti bağlantılarını işleyen [navlink bileşenini](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>) içerir. `NavLink` bileşeni, bileşeni yüklendiği zaman otomatik olarak seçili durumu gösterir ve bu, kullanıcının hangi bileşenin görüntülenmekte olduğunu anlamasına yardımcı olur.

* *_Imports. razor* &ndash;, uygulamanın bileşenlerine ( *. Razor*) dahil edilecek, ad alanları için [`@using`](xref:mvc/views/razor#using) yönergeleri gibi ortak Razor yönergelerini içerir.

* *Veri* klasörü (Blazor sunucusu) &ndash;, uygulamanın `FetchData` bileşene örnek Hava durumu verileri sağlayan `WeatherForecastService` `WeatherForecast` sınıfını ve uygulamasını içerir.

* *Wwwroot* , uygulamanın ortak statik varlıklarını Içeren uygulamanın [Web kök](xref:fundamentals/index#web-root) klasörünü &ndash;.

* *appSettings. JSON* (Blazor Server) uygulama için yapılandırma ayarlarını &ndash;.
