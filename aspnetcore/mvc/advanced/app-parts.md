---
title: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 11/11/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a102511478c40ae64aada919fee7072c3027ddcd
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959001"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts"></a>Uygulama parçalarıyla denetleyiciler, görünümler, Razor Pages ve daha fazlasını paylaşma

::: moniker range=">= aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır. Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> bir uygulama bölümüdür. `AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.

[Özellik sağlayıcıları](#fp) , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur. Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır. Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz. Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz. Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.

ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.

## <a name="load-aspnet-core-features"></a>ASP.NET Core özellikleri yükle

ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> ve <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıflarını kullanın. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler. `ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup.cs?name=snippet)]

Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup2.cs?name=snippet)]

Önceki iki kod örneği bir derlemeden `SharedController` yükler. `SharedController`, uygulamanın projesinde değil. Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) örneği indirmesi.

### <a name="include-views"></a>Görünümleri dahil et

Derlemeye görünümler eklemek için [Razor sınıf kitaplığı](xref:razor-pages/ui-class) kullanın.

### <a name="prevent-loading-resources"></a>Kaynakları yüklemeyi engelle

Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir. Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın. `ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir. Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın. Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın. Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.

`ApplicationPartManager` şunlar için parçalar içerir:

* Uygulamanın derlemesi ve bağımlı derlemeleri.
* `Microsoft.AspNetCore.Mvc.ApplicationParts.CompiledRazorAssemblyPart`
* `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

<a name="fp"></a>

## <a name="feature-providers"></a>Özellik sağlayıcıları

Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar. Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:

* <xref:Microsoft.AspNetCore.Mvc.Controllers.ControllerFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.MetadataReferenceFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.ViewsFeatureProvider>
* `internal class` [RazorCompiledItemFeatureProvider](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)

Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır. Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir. `ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir. Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.

### <a name="display-available-features"></a>Kullanılabilir özellikleri görüntüle

Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a>Uygulama bölümlerinde bulma

HTTP 404 hataları, uygulama parçalarıyla geliştirme sırasında sık karşılaşılan bir durumdur. Bu hatalar genellikle uygulamaların bölümlerinin nasıl keşfedildiği konusunda önemli bir gereksinimin eksik olmasından kaynaklanır. Uygulamanız bir HTTP 404 hatası döndürürse, aşağıdaki gereksinimlerin karşılandığından emin olun:

* `applicationName` ayarının, bulma için kullanılan kök derlemeye ayarlanması gerekir. Bulma için kullanılan kök derleme normalde giriş noktası derlemesidir.
* Kök derlemenin, bulma için kullanılan bölümlere bir başvuruya sahip olması gerekir. Başvuru doğrudan veya geçişli olabilir.
* Kök derlemenin Web SDK 'sına başvurması gerekiyor. Çerçevede, öznitelikleri bulma için kullanılan kök derlemeye damgalayan mantık vardır.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır. Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür. `AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.

*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur. Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır. Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz. Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz. Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.

ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.

## <a name="load-aspnet-core-features"></a>ASP.NET Core özellikleri yükle

ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için `ApplicationPart` ve `AssemblyPart` sınıflarını kullanın. <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler. `ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Önceki iki kod örneği bir derlemeden `SharedController` yükler. `SharedController`, uygulamanın projesinde değil. Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.

### <a name="include-views"></a>Görünümleri dahil et

Derlemeye görünümler eklemek için [Razor sınıf kitaplığı](xref:razor-pages/ui-class) kullanın.

### <a name="prevent-loading-resources"></a>Kaynakları yüklemeyi engelle

Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir. Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın. `ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir. Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın. Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın. Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.

Aşağıdaki kod uygulamadan `MyDependentLibrary` kaldırmak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kullanır: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` şunlar için parçalar içerir:

* Uygulamanın derlemesi ve bağımlı derlemeleri.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Uygulama özelliği sağlayıcıları

Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar. Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:

* [Denetleyiciler](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Etiket Yardımcıları](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Bileşenleri görüntüle](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır. Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir. `ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir. Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.

### <a name="display-available-features"></a>Kullanılabilir özellikleri görüntüle

Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a>Uygulama bölümlerinde bulma

HTTP 404 hataları, uygulama parçalarıyla geliştirme sırasında sık karşılaşılan bir durumdur. Bu hatalar genellikle uygulamaların bölümlerinin nasıl keşfedildiği konusunda önemli bir gereksinimin eksik olmasından kaynaklanır. Uygulamanız bir HTTP 404 hatası döndürürse, aşağıdaki gereksinimlerin karşılandığından emin olun:

* `applicationName` ayarının, bulma için kullanılan kök derlemeye ayarlanması gerekir. Bulma için kullanılan kök derleme normalde giriş noktası derlemesidir.
* Kök derlemenin, bulma için kullanılan bölümlere bir başvuruya sahip olması gerekir. Başvuru doğrudan veya geçişli olabilir.
* Kök derlemenin Web SDK 'sına başvurması gerekiyor.
  * ASP.NET Core Framework 'te, bulma için kullanılan kök derlemeye öznitelikleri damgalayan özel yapı mantığı vardır.

::: moniker-end
