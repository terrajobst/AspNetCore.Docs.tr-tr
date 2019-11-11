---
title: ASP.NET Core içindeki uygulama bölümleri
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905760"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma

::: moniker range=">= aspnetcore-3.0"

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

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

Derleme içindeki görünümleri dahil etmek için:

* Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>ekleyin:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

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

### <a name="generic-controller-feature"></a>Genel denetleyici özelliği

ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar. Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`). Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Türler `EntityTypes.Types`tanımlanmıştır:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Özellik sağlayıcısı `Startup`eklendi:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* . Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıfı:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Örneğin, `https://localhost:5001/Sprocket` bir URL 'SI istemek aşağıdaki Yanıt ile sonuçlanır:

```text
Hello from a generic Sprocket controller.
```

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

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

Derleme içindeki görünümleri dahil etmek için:

* Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>ekleyin:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

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

### <a name="generic-controller-feature"></a>Genel denetleyici özelliği

ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar. Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`). Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Türler `EntityTypes.Types`tanımlanmıştır:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Özellik sağlayıcısı `Startup`eklendi:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* . Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıfı:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Örneğin, `https://localhost:5001/Sprocket` bir URL 'SI istemek aşağıdaki Yanıt ile sonuçlanır:

```text
Hello from a generic Sprocket controller.
```

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

::: moniker-end
