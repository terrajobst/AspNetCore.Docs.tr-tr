---
title: ASP.NET Core içindeki uygulama bölümleri
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207354"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır. Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür. `AssemblyPart`derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.

*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur. Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır. Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz. Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz. Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.

ASP.NET Core uygulamalar, içindeki <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler. Sınıfı <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> , bir derleme tarafından desteklenen bir uygulama parçasını temsil eder.

## <a name="load-aspnet-core-features"></a>ASP.NET Core özellikleri yükle

ASP.NET Core özelliklerini ( `AssemblyPart` denetleyiciler, görünüm bileşenleri vs.) bulup yüklemek için vesınıflarınıkullanın.`ApplicationPart` Uygulama parçalarını ve özellik sağlayıcılarını izler.<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> `ApplicationPartManager`Şu şekilde yapılandırılır `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Aşağıdaki kod, kullanarak `ApplicationPartManager` `AssemblyPart`yapılandırma için alternatif bir yaklaşım sağlar:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Önceki iki kod örneği bir derlemeden yüklenir `SharedController` . `SharedController` Uygulamanın projesinde değil. Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.

### <a name="include-views"></a>Görünümleri dahil et

Derleme içindeki görünümleri dahil etmek için:

* Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Şunu öğesine ekleyin: <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Kaynakları yüklemeyi engelle

Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir. Kullanılabilir kaynakları gizlemek veya saklamak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın. `ApplicationParts` Koleksiyondaki girişlerin sırası önemli değildir. Kapsayıcısını, `ApplicationPartManager` kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce yapılandırın. Örneğin, öğesini çağırmadan `ApplicationPartManager` `AddControllersAsServices`önce öğesini yapılandırın. Kaynağı kaldırmak `Remove` için koleksiyondaçağrıyapın.`ApplicationParts`

Aşağıdaki kod uygulamadan kaldırmak <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> `MyDependentLibrary` için kullanır:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

, `ApplicationPartManager` Şunlar için parçalar içerir:

* Uygulamanın derlemesi ve bağımlı derlemeleri.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Uygulama özelliği sağlayıcıları

Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar. Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:

* [Denetleyiciler](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Etiket Yardımcıları](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Bileşenleri görüntüle](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Özellik sağlayıcıları öğesinden <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır, burada `T` özelliğin türüdür. Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir. İçindeki `ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma süresi davranışından etkileyebilir. Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.

### <a name="generic-controller-feature"></a>Genel denetleyici özelliği

ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar. Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`). Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Türler içinde `EntityTypes.Types`tanımlanmıştır:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Özellik sağlayıcısı içine `Startup`eklenir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* . Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıf:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Örneğin, aşağıdaki yanıtta `https://localhost:5001/Sprocket` sonuç URL 'si isteniyor:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>Kullanılabilir özellikleri görüntüle

Bir uygulama için kullanılabilen özellikler, bir `ApplicationPartManager` [bağımlılık ekleme](../../fundamentals/dependency-injection.md)isteği isteyerek numaralandırılabilir:

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
