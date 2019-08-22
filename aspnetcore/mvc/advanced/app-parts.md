---
title: ASP.NET Core içindeki uygulama bölümleri
author: ardalis
description: Bir derlemeden Özellik yüklemeyi veya bir derlemeden bir derlemeyi önlemek için bir uygulamanın kaynakları üzerinde soyutlamalar olan uygulama parçalarını nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4900ccf5589500db076f8cecd9da198c6a7ceea4
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886459"
---
<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

# <a name="application-parts-in-aspnet-core"></a>ASP.NET Core içindeki uygulama bölümleri

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde, denetleyiciler, görüntüleme bileşenleri veya etiket YARDıMCıLARı gibi mvc özelliklerinin keşfedildiği bir soyutlamadır. Bir uygulama parçasının bir örneği, derleme başvurusunu kapsülleyen ve türleri ve derleme başvurularını sunan bir AssemblyPart değildir. *Özellik sağlayıcıları* ASP.NET Core MVC uygulamasının özelliklerini doldurmak için uygulama bölümleriyle birlikte çalışır. Uygulama bölümleri için ana kullanım örneği, uygulamanızı bir derlemeden MVC özelliklerini bulacak (veya yüklemeden kaçınmak üzere) yapılandırmanıza izin vermaktır.

## <a name="introducing-application-parts"></a>Uygulama bölümlerine giriş

MVC uygulamaları, [uygulama parçalarından](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)özelliklerini yükler. Özellikle, [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder. Denetleyiciler, görünüm bileşenleri, etiket yardımcıları ve Razor derleme kaynakları gibi MVC özelliklerini bulup yüklemek için bu sınıfları kullanabilirsiniz. [Applicationpartmanager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) , MVC uygulamasının kullanabileceği uygulama bölümlerinin ve özellik sağlayıcılarının izlenmesinden sorumludur. MVC 'yi yapılandırırken `ApplicationPartManager` ' de `Startup` ile etkileşim kurabilirsiniz:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

Varsayılan olarak, MVC bağımlılık ağacını arar ve denetleyicileri bulur (diğer derlemelerde bile). Rastgele bir derlemeyi yüklemek için (örneğin, derleme zamanında başvurulmayan bir eklentiden), bir uygulama bölümü kullanabilirsiniz.

Belirli bir derlemede veya konumda bulunan denetleyiciler için arama *yapmaktan kaçınmak* için uygulama parçalarını kullanabilirsiniz. Uygulamasının `ApplicationParts` koleksiyonunu`ApplicationPartManager`değiştirerek hangi parçaların (veya derlemelerin) kullanılabilir olduğunu kontrol edebilirsiniz. `ApplicationParts` Koleksiyondaki girişlerin sırası önemli değildir. Kapsayıcısını, `ApplicationPartManager` kapsayıcıda hizmetleri yapılandırmak için kullanmadan önce tam olarak yapılandırmak önemlidir. Örneğin, öğesini çağırmadan `ApplicationPartManager` `AddControllersAsServices`önce tam olarak yapılandırmanız gerekir. Bunu yapmazsanız, uygulama bölümlerdeki denetleyicilerin bu yöntem çağrısından sonra eklenmeyeceği anlamına gelir (hizmet olarak kaydedilmez) ve bu, uygulamanızın hatalı davranışına neden olabilir.

Kullanılmasını istemediğiniz denetleyicileri içeren bir derlemeniz varsa, ' den `ApplicationPartManager`kaldırın:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Projenizin derleme ve bağımlı derlemelerinin `ApplicationPartManager` yanı sıra, varsayılan olarak `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` parçalarını içerir.

## <a name="application-feature-providers"></a>Uygulama özelliği sağlayıcıları

Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar. Aşağıdaki MVC özellikleri için yerleşik özellik sağlayıcıları vardır:

* [Denetleyiciler](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Meta veri başvurusu](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Etiket Yardımcıları](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Bileşenleri görüntüle](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Özellik sağlayıcıları öğesinden `IApplicationFeatureProvider<T>`devralınır, burada `T` özelliğin türüdür. Yukarıda listelenen herhangi bir MVC Özellik türü için kendi özellik sağlayıcılarını uygulayabilirsiniz. Daha sonraki sağlayıcılar önceki sağlayıcılar tarafından alınan `ApplicationPartManager.FeatureProviders` eylemlere yanıt verebilmesi için koleksiyondaki özellik sağlayıcılarının sırası önemli olabilir.

### <a name="sample-generic-controller-feature"></a>Örnekli Genel denetleyici özelliği

Varsayılan olarak, ASP.NET Core MVC genel denetleyicileri yoksayar (örneğin, `SomeController<T>`). Bu örnek, varsayılan sağlayıcıdan sonra çalışan bir denetleyici özellik sağlayıcısı kullanır ve belirtilen tür listesi için genel denetleyici örnekleri ekler (içinde `EntityTypes.Types`tanımlanmıştır):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Varlık türleri:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Özellik sağlayıcısı içine `Startup`eklenir:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Varsayılan olarak, yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *genericcontroller ' 1 [pencere öğesi]* biçiminde olacaktır. Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirmek için kullanılır:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıf:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Sonuç, eşleşen bir yol istendiğinde:

![Örnek uygulama okumalarından örnek çıktı, ' bir genel Sprocket denetleyicisinden Merhaba. '](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Örnekli Kullanılabilir özellikleri görüntüle

`ApplicationPartManager` [Bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve ilgili özelliklerin örneklerini doldurmak için kullanarak uygulamanız için kullanılabilen doldurulmuş özellikler arasında yineleme yapabilirsiniz:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Örnek çıktı:

![Örnek uygulamadaki örnek çıkış](app-parts/_static/available-features.png)
