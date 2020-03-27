---
title: ASP.NET Core Blazor şablonları
author: guardrex
description: ASP.NET Core Blazor uygulama şablonları ve Blazor proje yapısı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 71a9d9eee8637dda0b3cecac82ff96a0c3bfedb5
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320974"
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

* *App. razor* &ndash; <xref:Microsoft.AspNetCore.Components.Routing.Router> bileşenini kullanarak istemci tarafı yönlendirmeyi ayarlayan uygulamanın kök bileşenidir. `Router` bileşeni tarayıcı gezintisini karşılar ve istenen adresle eşleşen sayfayı işler.

* *Sayfalar* klasörü &ndash;, Blazor sunucu uygulamasının Blazor uygulamasını ve kök Razor sayfasını oluşturan yönlendirilebilir bileşenleri/sayfaları ( *. Razor*) içerir. Her sayfanın yolu [`@page`](xref:mvc/views/razor#page) yönergesi kullanılarak belirtilir. Şablon şunları içerir:
  * *_Host. cshtml* (Blazor sunucusu), Razor sayfası olarak uygulanan uygulamanın kök sayfasına &ndash;:
    * Uygulamanın herhangi bir sayfası başlangıçta istendiğinde, Bu sayfa işlenir ve yanıtta döndürülür.
    * Tarayıcı ve sunucu arasındaki gerçek zamanlı SignalR bağlantısını ayarlayan `_framework/blazor.server.js` JavaScript dosyası yüklenir.
    * Ana bilgisayar sayfası, kök `App` bileşeni 'nin (*app. Razor*) nerede işleneceğini belirtir.
  * `Counter` (*Counter. Razor*) &ndash; sayaç sayfasını uygular.
  * uygulamada işlenmeyen bir özel durum oluştuğunda `Error` (yalnızca*hata. Razor*, Blazor sunucu uygulaması) &ndash;.
  * `FetchData` (*fetchdata. Razor*) &ndash; verileri getir sayfasını uygular.
  * `Index` (*Index. Razor*) &ndash; giriş sayfasını uygular.

* *Paylaşılan* klasör &ndash;, uygulama tarafından kullanılan diğer Kullanıcı Arabirimi bileşenlerini ( *. Razor*) içerir:
  * `MainLayout` (*mainlayout. Razor*) uygulamanın düzen bileşeni &ndash;.
  * `NavMenu` (*Navmenu. Razor*) &ndash; kenar çubuğu gezintisini uygular. Diğer Razor bileşenlerine yönelik gezinti bağlantılarını işleyen [navlink bileşenini](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>) içerir. `NavLink` bileşeni, bileşeni yüklendiği zaman otomatik olarak seçili durumu gösterir ve bu, kullanıcının hangi bileşenin görüntülenmekte olduğunu anlamasına yardımcı olur.

* *_Imports. razor* &ndash;, uygulamanın bileşenlerine ( *. Razor*) dahil edilecek, ad alanları için [`@using`](xref:mvc/views/razor#using) yönergeleri gibi ortak Razor yönergelerini içerir.

* *Veri* klasörü (Blazor sunucusu) &ndash;, uygulamanın `FetchData` bileşene örnek Hava durumu verileri sağlayan `WeatherForecastService` `WeatherForecast` sınıfını ve uygulamasını içerir.

* *Wwwroot* , uygulamanın ortak statik varlıklarını Içeren uygulamanın [Web kök](xref:fundamentals/index#web-root) klasörünü &ndash;.

* *appSettings. JSON* (Blazor Server) uygulama için yapılandırma ayarlarını &ndash;.
